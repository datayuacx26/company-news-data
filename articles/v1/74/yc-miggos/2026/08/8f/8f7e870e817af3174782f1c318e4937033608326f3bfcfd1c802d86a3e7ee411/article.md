---
schema_version: "1.0.0"
document_id: "8f7e870e817af3174782f1c318e4937033608326f3bfcfd1c802d86a3e7ee411"
company_key: "yc-miggos"
company: "Miggos"
source_id: "yc-miggos-news-import-47b875555782"
canonical_url: "https://www.miggo.io/post/the-backup-that-backfired-how-one-whitelisted-objref-hands-any-veeam-domain-user-system-cve-2026-44963"
published_at: "2026-08-19T14:16:30.356+00:00"
first_seen_at: "2026-08-19T17:35:06.350420+00:00"
fetched_at: "2026-08-19T17:35:07.126483+00:00"
content_hash: "sha256:ae5f63f18e4f3b223eb0d578b03b96b2c4dc103e1f53760bea982239d384e9c9"
---

# The Backup that Backfired: How One Whitelisted ObjRef Hands Any Veeam Domain User SYSTEM (CVE-2026-44963)

###### TL;DR


##### ‍
***An authorization check in Veeam Backup & Replication became the exploit. CVE-2026-44963, and one domain password.***


## Background


A single domain credential, the kind that shows up in every breach dump, was enough to execute code as NT AUTHORITY\\SYSTEM on a Veeam Backup & Replication server: the one machine that holds the encryption keys and stored credentials for every backup you own. No admin rights. No Veeam role. No user interaction.


A Veeam Backup & Replication server is the machine that makes that first step necessary, because it is the one box that can undo the whole operation, and it also happens to hold the encryption keys for every backup you own, the stored credentials for every hypervisor it protects, and fairly often a service account with domain-admin reach. So it is worth asking who is allowed to talk to it, and on a v12 server, for one particular service, the answer turns out to be everybody.


The bug lives in the **Veeam Threat Hunter Service** , Veeam's bundled malware scanner, installed on every Backup & Replication server, which listens on TCP port **6175** , speaks .NET Remoting (a protocol Microsoft deprecated years ago), and deserializes whatever you send it into a live object graph. The twist, which we'll build up to, is that the server's *own authorization check* is what pulls the trigger. This is **CVE-2026-44963** , and what's surprising about it is how familiar it feels. It's the third deserialization RCE in this product's recent history, and the mechanism is a near-perfect rhyme with the previous two.


This writeup walks the chain: how we found it, why a hand-curated whitelist of 4,200 types still let the single most dangerous .NET Remoting primitive through, the transport-layer trick that neutralizes the last line of defense, and — because a CVE is only half the story — how you'd detect this at runtime whether or not the patch is applied.


## **Credit, up front.**


We found this vulnerability independently and reported it to Veeam — and the Veeam Backup security team let us know it was the same issue **Sina Kheirkhah (@SinSinology) of watchTowr** had already reported, a little ahead of us. Full credit to Sina and watchTowr for getting there first; we think highly of that team, and the finding is theirs to headline. The fix ships in **12.3.2.4854** (KB4869). We're publishing our own account because good research converges, and because the path we took — the Threat Hunter service, port 6175, and the transport-sink bypass detailed below — hadn't been made public.


## Quick Details


- **What:** Remote code execution as` SYSTEM` on Veeam Backup & Replication v12.x, reachable by any authenticated domain user (and, in a non-default Guest configuration, effectively unauthenticated).
- **Where:** The Threat Hunter Service (` VeeamThreatHunterSvc` ), a .NET Remoting endpoint on TCP 6175.
- **Why:** Three compounding weaknesses — (1) the Threat Hunter grants access to every member of` BUILTIN\\Users` , i.e. every domain user; (2)` System.Runtime.Remoting.ObjRef` — the one .NET Remoting type that opens a callback channel — is on the deserialization whitelist while every known gadget is blocked; (3) that whitelist guards inbound requests only, so the gadget rides home on the attacker-controlled reply leg. The trigger is subtle: the server's own authorization check reads` msg.MethodBase` off the deserialized message, and that read is a remote call to the attacker.
- **Impact:** Full compromise of the backup fabric — keys, credentials, and the ability to destroy every backup copy before deploying ransomware.
- **Fixed in:** 12.3.2.4854. **Not present in v13.0+** (Threat Hunter rewritten in .NET 8, .NET Remoting removed, BinaryFormatter disabled).


## Affected Versions


**Version** **Status**


**v12.0 – 12.3.2.4465** Vulnerable — Threat Hunter uses .NET Remoting + BinaryFormatter


**12.3.2.4854 (KB4869)** Fixed


**v13.0.1.2067+** Not affected — service rewritten in .NET 8, no .NET Remoting, BinaryFormatter disabled


**Version:**
v12.0 – 12.3.2.4465


**Status:**
Vulnerable — Threat Hunter uses .NET Remoting + BinaryFormatter


**Version:**
12.3.2.4854 (KB4869)


**Status:**
Fixed


**Version:**
v13.0.1.2067+


**Status:**
Not affected — service rewritten in .NET 8, no .NET Remoting, BinaryFormatter disabled


- **CVE:** CVE-2026-44963 (CWE-502, Deserialization of Untrusted Data)
- **CVSS v4:** 9.4 Critical —` AV:N/AC:L/AT:N/PR:L/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H` (per Veeam KB4869)
- **Prerequisite:** any authenticated domain user (membership in` BUILTIN\\Users` , which includes all domain users by default)
- **Tested on:** v12.3.2.4465 and v13.0.1.2067, Azure marketplace images, Windows Server 2022 Datacenter (` Standard_D4s_v3` )


We didn't take the "not affected in v13" claim on faith. We deployed **v13.0.1.2067** and confirmed the Threat Hunter now runs on net8.0 as an ASP.NET Core service, port 6175 is closed, and` EnableUnsafeBinaryFormatterSerialization` is` false` in the runtime config. The bug class is genuinely gone in v13 — which helps the organizations that have already made that jump, and not the far larger number still running v12 in production.


# Deep Dive


## **Two locked doors, and a room nobody guarded**


We started where you would expect, on the network, and got nowhere twice.


Port 9401 runs WCF with` clientCredentialType="None"` , which is exactly as inviting as it sounds. No client authentication on the channel at all, and the contract behind it is a method called` Invoke(scope, method, parameters)` . We connected with no credentials, called it, and got back` Access denied` from a JWT check deeper in the stack, where the token has to be signed by the server's own certificate and bound to a live restore session, which is not something an outsider produces. The transport waved us through and the application layer stopped us at the second door.


Port 9419, the REST API, exposes exactly three anonymous endpoints. XXE against the parsers was blocked, the usual JSON deserialization tricks were blocked, and there was nothing else on that port worth the trouble.


Two failures in, we stopped looking for a door with no lock and changed the question from what is unauthenticated to what is over-trusted, which is a different and usually more productive thing to ask: which service takes a low-privilege user more seriously than it should?


There was a whole one.


## **The service that trusts everyone**


Veeam ships a proper role model. Six roles, Backup Administrator down to Backup Viewer, each with a GUID and a permission set. The Threat Hunter service, the bundled malware scanner listening on TCP 6175, uses none of it. It carries its own access checker instead, and the entire gate, the whole of it, is one line:


```text
if   (  new   WindowsPrincipal(windowsIdentity).IsInRole(WindowsBuiltInRole.User))
return     true  ;


```


` BUILTIN\\Users` contains` Authenticated Users` by default on every domain-joined machine. So the question that gate actually asks is not whether you are a Veeam operator, it is whether you have a password. Veeam's own advisory says the same thing in fewer words: authenticated domain users on domain-joined backup servers.


Behind the gate sits .NET Remoting, a protocol Microsoft deprecated years ago, feeding BinaryFormatter, a serializer Microsoft flags as dangerous in its own tooling. Veeam knows this. They bolted two locks onto it.


## **The one type on the list**


The first lock is a hand-written allowlist: 4,200 type names in a` whitelist.txt` embedded in the shipping DLL. That is real work, and it mostly works. Rather than trust the decompiler, we loaded the shipping DLL on a live server and asked its own whitelist object about every gadget in the public catalogue, one at a time.` DataSet` , blocked.` ObjectDataProvider` , blocked.` Process` , blocked.` SortedSet` , blocked.` WindowsIdentity` , blocked twice over, once by the list and once by a hardcoded deny set that contains exactly one type.


Exactly one thing came back clean.


```text
ALLOWED: System.Runtime.Remoting.ObjRef
```


An ObjRef is the serialized form of "a reference to an object somewhere else" (the plumbing that makes a remote object look local). Deserialize one and .NET builds you a live proxy aimed at whatever address the bytes happen to name. Hand a server your ObjRef and you have handed it a phone number, and from that point the server is your client.


The second lock is .NET's own and it exists to refuse precisely this:` TypeFilterLevel.Low` is the safe setting Microsoft tells you to use, its refuse-list runs to four types, ObjRef is on it by name, and Veeam duly sets it one line below the allowlist.


Microsoft's list of types too dangerous to deserialize has four entries on it. ObjRef is one of them.


Veeam's list of types safe to deserialize has 4,200 entries. ObjRef is one of those too.


Two hand-written lists, one type, opposite verdicts. The allow list wins.


Beating the second lock does not even take a trick, only a careful reading of where .NET chose to look. The filter runs only when the parser believes it is reading a method call, a state it tracks in a flag called` IsRemoting` , which means that sending a method call fires the check and sending a bare serialized object does not, because nothing announces itself, the flag stays false, and the check never runs at all. The designers assumed an ObjRef could only ever turn up inside a method message, so a method message is the only place they look for one. (Our own client fought us harder than the server did. A Remoting proxy always wraps calls in a method record, that being the entire point of a proxy, so we reflected past it and wrote our bytes straight into the transport sink underneath.)


**Please hold, I need to ask the attacker**


The server now holds a live proxy pointed at us, and nothing has left the building yet, because Remoting connects lazily on first use and something has to touch the proxy before it dials.


The thing that touches it is the authorization check.


To work out what permission an incoming call requires,` EnsureAccessIsAllowed` reads` msg.MethodBase` off the message, collects the access attributes declared on that method, and hands the result to the access checker along with the caller's identity, which is a perfectly reasonable design as long as the message is a local object. For an ordinary in-process message that read costs nothing. For a proxy conjured out of an attacker's ObjRef, that read is a network call.


So the server has to phone the attacker to find out whether the attacker is allowed to phone the server.


Authorization is not bypassed here. It is the trigger.


*Figure 1. The full exchange. The first outbound arrow is the authorization check, not the dispatcher.*


## **The reply leg, where nobody is watching**


From there the server keeps talking, walking the message members one by one to assemble the invocation it believes it received, and every single read is another callback to us.` Uri` .` MethodName` .` TypeName` .` MethodBase` again. We answer each one with something dull and valid to keep the conversation alive.


Then it asks for` Properties` , and we hand back a` SortedSet<string>` carrying a TypeConfuseDelegate gadget straight out of ysoserial.net (no custom crafting, no novel primitive, the same payload that has been in the public catalogue for years).


The reason that works is one boolean: Veeam's allowlist is gated on` _serializingResponse` , so it inspects inbound requests and nothing else. The same` SortedSet` the whitelist rejected outright on the way in arrives on the reply leg, where nobody is checking, rebuilds its internal tree, calls its comparer to order two elements, and the comparer is` Process.Start` .


The command runs as NT AUTHORITY\\SYSTEM rather than as our unprivileged user, and that is not an accident of the gadget. The service runs as LocalSystem, and the channel takes an Identification-level token, which lets it identify a caller but never impersonate one. Our identity gets used exactly once, to pass the access check, and then never again.


## **POC**


*Figure 2. demo_lowpriv, a Users-only local account, reaching NT AUTHORITY\\SYSTEM. The callback order is the mechanism in sequence.*


` demo_lowpriv` is a local account made for the demo, a member of` Users` and nothing else, with no Veeam role and no admin rights. It connects, sends a 570-byte ObjRef, and the server obligingly calls back six times. The first callback is the authorization check.


```text
PS C:\> type C:\PWNED_DEMO.txt
nt authority\system


```


Entry price: one password. Every domain account in your directory holds one.


## What To Do


**Patch to 12.3.2.4854** (KB4869), or move to v13, where the Threat Hunter was rewritten on .NET 8 with Remoting gone and BinaryFormatter switched off. The bug class is not present there.


If you cannot patch this week, two controls actually stop this rather than merely reporting it. **Deny egress from the backup server** to anything it doesn't need, because the chain cannot complete unless the server can reach the attacker. And **stop VeeamThreatHunterSvc** , which removes the listener on 6175 outright. The second one costs you signature-based malware scanning, so it is a trade rather than a free win (worth making for a week, harder to justify for a quarter). Firewall 6175 inbound while you are in there, along with 9392, 9393 and 10001-10006, which are built on the same sink.


For detection, two signals survive a competent attacker, because any exploit of this class has to do both of them: a Veeam Remoting service opening an outbound connection to an address that arrived inside a request, and ObjRef resolving on an inbound deserialization path at all. A third, a backup service spawning` cmd.exe` , is free to deploy and trivial to dodge with a gadget that writes a file instead. Deploy it anyway.


## Conclusion


This is the third deserialization RCE in this product, and every fix was a list. CVE-2024-40711 was patched and a blocklist went in. CVE-2025-23120 walked past the blocklist with a couple of` DataSet` subclasses, so a whitelist replaced it. CVE-2026-44963 walked past the whitelist using a single entry nobody caught.


Lists encode what their author could imagine on one particular afternoon. Attackers converge on whatever got left out. A signature written for any one of these three would have missed the next, because the type name changes every time and the behaviour never does.


The full technical account is in the[companion document](https://www.miggo.io/miggo-engineering-insights/system-on-the-backup-server) : the decompiled sink, both bypasses in detail, the live callback trace, two further findings, and the PoC.


```text
<script src=  "https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"  ></script>
<  script     src  =  "https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/Flip.min.js"  >  </  script  >


<  script  >
document  .addEventListener(  "DOMContentLoaded"  ,   (  event  ) =>   {
gsap.registerPlugin(Flip);
const   state = Flip.getState(  ""  );
const   element =   document  .querySelector(  ""  );
element.classList.toggle(  ""  );
Flip.from(state, {
duration  :   0  ,
ease  :   "none"  ,
absolute  :   true  ,
});
});
</  script  >
```


```text


<  script  >
document  .addEventListener(  "DOMContentLoaded"  ,   (  event  ) =>   {
gsap.registerPlugin(Flip);
const   state = Flip.getState(  ""  );
const   element =   document  .querySelector(  ""  );
element.classList.toggle(  ""  );
Flip.from(state, {
duration  :   0  ,
ease  :   "none"  ,
absolute  :   true  ,
});
});
</  script  >
```
