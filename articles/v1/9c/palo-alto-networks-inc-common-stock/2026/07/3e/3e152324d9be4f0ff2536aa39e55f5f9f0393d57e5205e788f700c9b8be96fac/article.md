---
schema_version: "1.0.0"
document_id: "3e152324d9be4f0ff2536aa39e55f5f9f0393d57e5205e788f700c9b8be96fac"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/webauthn-added-to-browser-based-rdp/"
published_at: "2026-07-02T22:00:39+00:00"
first_seen_at: "2026-07-22T17:15:05.756127+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:fbf2dbd39538a83aff83746a9444257f64d6925a586d8e5c46f43448482a7e96"
---

# How We Added WebAuthn to a Browser-Based RDP Client

## The Pitch


**TL;DR:** We built the first RDP client outside of Windows to support WebAuthn redirection, beating Microsoft's own macOS, iOS and Linux clients to it (since then FreeRDP has added support too, which we’re happy to see). No browser API could do what we needed, the protocol spec was missing entire commands, and we discovered that Microsoft's Windows implementation routes through internal, undocumented code paths we had to reverse-engineer. This is the story of what we found.


[Prisma Browser](https://docs.paloaltonetworks.com/prisma-access-browser) offers support for more than the standard secure web browser. We often build support for protocols that go beyond web applications. We develop tailored solutions for Prisma Browser to enable its users to solve complicated security issues. One useful example is Remote Desktop Protocol (RDP).


IT operations frequently require SSH, while remote users need to access legacy and on-premises applications using protocols such as RDP. In the past, these were only accessible via[thick clients](https://www.eposaudio.com/en/us/about-epos/insights/articles/thin-client-or-thick-client-whats-the-difference) , which required opening risky network tunnels, or through remote session hops that translate traffic into browser-native protocols like HTML5. This translation often impacts performance and breaks core functionality.


To meet this need, we have developed native clients directly inside the browser. Doing this has also surfaced a demand for even more flexibility and features within these native clients. This post covers one of our recent announcements that not only fills a critical gap but makes our native clients superior to the common thick applications used today.


It all started when someone asked: "Can we support security keys? If a user is on a website inside the remote session and it asks for their YubiKey, can we just redirect that to their local machine?"


Sure. Microsoft has a protocol for that: \[MS-RDPEWA\], the WebAuthn Virtual Channel Extension. There's a spec. How hard can it be?


Two weeks, one IDA Pro license and several existential crises later, we had it working.


## Obligatory AI Section


Two years ago, the reverse-engineering part of this work would have taken days, easily. Today, it took a few hours with AI to write a working IDA model context protocol (MCP) bridge, then a few more hours using that bridge to ask the binary the right questions and find the pieces we needed.


AI did not reverse-engineer the feature for us. The hard parts were still knowing what to ask, validating every answer, setting breakpoints, comparing traces and proving the protocol behavior end to end. But the workflow changed dramatically: Instead of clicking through disassembly for days, we could build a tool, connect it to IDA and iterate in minutes.


## Reading the Spec


The \[MS-RDPEWA\] specification describes a dynamic virtual channel (DVC) called Microsoft::Windows::RDP.Webauthn


. The server sends[CBOR](https://datatracker.ietf.org/doc/html/rfc7049) -encoded WebAuthn requests, the client talks to a local authenticator and sends back the response. Four commands:


**Command** **ID** **Purpose**


WEB_AUTHN


5


MakeCredential


or


GetAssertion


IUVPAA


6


"Got a platform authenticator?"


CANCEL


7


Cancel current operation


API_VERSION


8


Version negotiation


Clean. Straightforward. Surely the hard part is just wiring it up. We'll get back to that.


Note: When we did this work the spec stopped here. The \[MS-RDPEWA\] 3.0 revision (March 2026) has since documented two more commands, GetCredentials


(9) and GetAuthenticatorList


(12), gated to Windows 11 24H2+ and Server 2025+ via KB5065789.


## "Just call navigator.credentials"


Our initial plan: receive the WebAuthn request from the server, call navigator.credentials.create()


in the extension, send back the response.


Here's the problem. When a user visits okta\[.\]com


inside the remote session and triggers WebAuthn, the server intercepts the ceremony. It computes clientDataHash = SHA-256(clientDataJSON)


, where clientDataJSON


contains the page's origin, the challenge and the ceremony type. Then it sends that 32-byte hash over the RDP channel.


On the client side, navigator.credentials.create()


insists on constructing its own clientDataJSON


, with its own origin: chrome-extension://...


, not hxxps://okta\[.\]com


. It hashes that, hands it to the authenticator, and the authenticator signs over it.


When the assertion gets back to Okta's server: hash mismatch. Signature verification fails. SHA-256 is a one-way function. There's no going around this.


We briefly considered reconstructing the clientDataJSON


ourselves from the known challenge and origin. Three reasons that doesn't work:


- Browsers don't produce byte-identical JSON for the same inputs (field ordering, encoding).
- Native apps using WebAuthn SDKs add more variability.
- Older Windows servers don't even send the ingredients. They send only the 32-byte hash. No origin. No challenge in cleartext. Just the hash.


## Building a Custom Browser API


No existing browser API can accept a pre-computed clientDataHash


and pass it directly to an authenticator. Not navigator.credentials


. Not chrome.webAuthenticationProxy


(designed for the opposite direction). Not remoteDesktopClientOverride


(requires the original JSON). Not WebHID (USB-only, no Touch ID, no phone-as-authenticator).


Since we built this, the W3C WebAuthn working group has started standardizing exactly this case (the remoteClientDataJSON


extension, editor's draft section 10.1.6). It is not yet in any shipping browser, so the custom API below is still required, but the direction is encouraging.


So we built one: a custom extension API with makeCredential()


and getAssertion()


methods identical to navigator.credentials in every way except one. The caller supplies the clientDataHash


directly; it goes straight to the authenticator.


The upside of being a web-based RDP client inside Chromium: We get Chromium's entire FIDO2 stack for free. Authenticator discovery across USB, BLE, NFC and platform authenticators.[Cloud-assisted Bluetooth low energy](https://www.goodsignin.com/blog/what-does-cable-have-to-do-with-passkeys) (caBLE)/Hybrid transport for phone-as-authenticator. Touch ID and Windows Hello integration. The credential selector UI. Our custom API is a thin wrapper reusing all of this machinery.


**Why not just use libfido2, the way FreeRDP does?** Because FreeRDP is a native client and we are not. libfido2 reaches authenticators over USB or NFC by opening raw HID devices directly, which a native process can do but code inside a browser cannot.


From WebAssembly there is no raw HID access. The only in-browser transport is WebHID, which is USB-only (no Touch ID, no Windows Hello, no phone-as-authenticator). Getting true device access would mean shipping a separate native helper outside the browser, defeating the point of an in-browser client. And even then, libfido2 alone would not give us platform authenticators or phone-as-authenticator over caBLE/hybrid.


By wrapping Chromium's own FIDO2 stack we get all of those for free. That is also why, on Windows, our path runs into Chromium's WebAuthn machinery and its clientDataJSON


requirement, while a native libfido2 client does not.


This worked. YubiKey blinks. User touches it. Registration succeeds. We celebrated for approximately 90 seconds before the next problem emerged.


## "It Works on This Windows Version But Not That One"


Our WebAuthn redirection worked beautifully against some Windows servers and completely failed against others. Same client code, same authenticator, same relying party.


Here's why. On Windows, every browser (including Chromium) calls WebAuthNAuthenticatorMakeCredential


from webauthn.dll


. This public API **unconditionally requires** the full clientDataJSON


:


1


2


3


if


(


!


pbClientDataJSON


|


|


!


cbClientDataJSON


|


|


!


pwszHashAlgId


)


return


NTE_INVALID_PARAMETER


;


// 0x80090027


Our approach of passing a raw hash directly works on macOS and Linux (where we control the authenticator stack) but hits a wall on Windows.


The deeper issue: **Not all Windows servers send the same data.** Newer Windows servers (API version 9, currently only Windows 11 25H2+) additionally transmit clientDataJSON


, remoteWebOrigin


and the full W3C credential options over the wire. Microsoft published the[v9 struct changes to webauthn.h](https://chromium.googlesource.com/external/github.com/microsoft/webauthn/+/0ef86c2fe31e5b0608cc4fce4820935bd644cabb%5E%21/) in September 2025. If both sides are v9, you get the new fields and everything is straightforward.


But older Windows servers (Windows 10, Server 2019/2022, Windows 11 through 24H2) send only the 32-byte hash. This is the vast majority of enterprise environments.


So how does Microsoft's own mstsc.exe


handle older servers? It uses the same webauthn.dll


. But how it actually processes a hash-only request is undocumented.


Time to break out IDA Pro.


## Reverse Engineering mstsc.exe


Before diving into disassembly, we had one paranoid question to answer first: what if the server is just... cheating? Like, what if it quietly detects that it's talking to mstsc.exe


and slips it a clientDataJSON


in some side channel, and the whole thing only works because Microsoft wrote both ends and decided third parties were on their own? We had to know before spending days in a disassembler.


So we hooked mstscax.dll


with Frida and intercepted the WebAuthn DVC channel in both directions across two complete FIDO2 ceremonies. Result: no clientDataJSON


on the wire. The server sends a 32-byte clientDataHash


. The client sends back a CTAP2 response. No JSON anywhere. So mstsc.exe


is dealing with the exact same problem we are. But it works. Sorry I doubted you, Microsoft.


**mstsc.exe never calls the public WebAuthn API.**


webauthn.dll


has a “dual personality”:


1. **Public API** ( WebAuthNAuthenticatorMakeCredential


, etc.): documented, stable, used by browsers and apps, requires clientDataJSON


.
2. **The DVC plugin path** : an exported function VirtualChannelGetInstance()


creates a WebAuthNDVCPlugin


(implementing IWTSPlugin


). This plugin handles everything through private, unexported functions that are perfectly happy with just clientDataHash


and no JSON.


Microsoft does acknowledge this dual role. The[IWTSPlugin MSDN page](https://learn.microsoft.com/en-us/windows/win32/api/tsvirtualchannels/nn-tsvirtualchannels-iwtsplugin) notes: *"The IWTSPlugin interface is implemented by %System32%\\webauthn.dll to enable the Remote Desktop WebAuthn redirection functionality."* The page also points you to VirtualChannelGetInstance


(which has its own reference page) to obtain the interface. What it omits is what the plugin does with a hash-only request.


VirtualChannelGetInstance


takes a GUID parameter, and Microsoft documents the prototype on its reference page (noting it is not shipped in a header, so you declare it yourself). The GUID is IID_IWTSPlugin


, which has shipped in tsvirtualchannels.h


in the Windows SDK for years. It is a genuine third-party integration point; what Microsoft does not document is how the plugin handles a hash-only request, which is the behavior we reverse-engineered to replicate in our own Windows code path.


The call chain we reverse-engineered:


1


2


3


4


5


6


7


8


9


10


11


mstscax


.


dll


loads


webauthn


.


dll


from


System32


- >


calls


VirtualChannelGetInstance


(


IID_IWTSPlugin


{


a1230201


- .


.


.


}


)


\[


documented


;


in


tsvirtualchannels


.


h


\]


- >


gets


IWTSPlugin*


(


WebAuthNDVCPlugin


)


- >


plugin


registers


for


"WebAuthN_Channel"


- >


incoming


CBOR


- >


CtapCborDecodeRpcRequest


(


)


\[


PRIVATE


,


NOT


EXPORTED


\]


- >


I_ProcessRemoteRpcRequestOnClient


(


)


\[


PRIVATE


,


NOT


EXPORTED


\]


The private CtapCborDecodeRpcRequest


treats clientDataJSON


as optional. If it's in the CBOR, use it. If not, pass through the hash. The public API is strict; the private plugin path isn't.


## What We Ended Up With


- A **DVC plugin** for the WebAuthN_Channel


, implemented in C as part of our existing WebAssembly (Wasm) client
- A **custom Chromium extension API** that accepts pre-computed clientDataHash


values, supporting USB keys, Touch ID, Windows Hello and phone-as-authenticator via caBLE/Hybrid
- A **TypeScript protocol layer** handling CBOR encoding/decoding for MS-RDPEWA, including parts that were undocumented when we built it
- A **detailed reverse-engineering report** on webauthn.dll


's dual nature


Works on both newer Windows servers (with clientDataJSON


) and older ones (hash only). Supports registration and authentication. Handles the commands that were undocumented at the time. When we shipped it, no other non-Windows RDP client did this. FreeRDP has since added support (version 3.25.0, April 2026), which is great news.


## Since We Wrote This…


- Microsoft updated the MS-RDPEWA spec (version 3.0, March 2026) to document commands that were previously missing
- The W3C is standardizing the browser side via the remoteClientDataJSON extension
- FreeRDP shipped a non-Windows implementation in version 3.25.0 (April 2026)


We have folded these in, in the text above. The reverse engineering was necessary when we did it, and the cross-platform challenge is unchanged.


## Takeaways


**The protocol is platform-agnostic; the implementation knowledge is not.** MS-RDPEWA defines a clean wire protocol. But implementing it correctly required reverse-engineering mstsc.exe


, because the spec was missing commands, field definitions and protocol extensions (Microsoft has since documented some of these in the MS-RDPEWA version 3.0 revision, March 2026).


**The Windows version split is the core architectural challenge.** Newer servers send clientDataJSON. Older servers send only the hash, requiring an approach that no standard browser API supports. Microsoft handles this through webauthn.dll


's DVC plugin; a documented entry point, but one that only works on Windows and whose hash-only behavior is undocumented. If you're not on Windows, you're on your own.


Microsoft’s documentation gaps aren’t edge cases. Niche details such as a missing command, undocumented protocol fields, the undocumented behavior behind the plugin or a spec example with the wrong command number are things that must be fixed for the implementation to work at all.


**When we shipped it, this was the first implementation of WebAuthn redirection outside of Windows.** Since then FreeRDP has added support too (version 3.25.0, April 2026), which is great to see. Microsoft's own macOS, iOS, Android and Linux RDP clients don't support it. We hope this post explains why, and what developers can do to leverage this knowledge to improve their own RDP clients.


*Native RDP and SSH clients in the browser allow you to implement security and data controls for all your sessions. To find out more about Enterprise Browsers' security capabilities, you can read our deep-dive on*[native RDP and SSH access](https://www.paloaltonetworks.com/blog/sase/seamless-and-secure-rdp-and-ssh-access-using-prisma-browser/) *or explore the*[five non-negotiables for choosing a secure enterprise browser](https://www.paloaltonetworks.com/blog/sase/the-five-non-negotiables-for-choosing-a-secure-enterprise-browser/) *.*
