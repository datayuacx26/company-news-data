---
schema_version: "1.0.0"
document_id: "562c21a223c40905d5c71d4c3602db534fcd3304f7a338baa60bf4d061d5d200"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-d50bd684d529"
canonical_url: "https://zeropath.com/blog/openclaw-clawdbot-credential-theft-vulnerability"
published_at: "2026-02-02T00:00:00+00:00"
first_seen_at: "2026-07-24T06:50:52.708520+00:00"
fetched_at: "2026-07-28T22:22:47.419354+00:00"
content_hash: "sha256:b5dd14d4a21d7d93615c1cb22c6eb4256c9edf4d6954ee356e885f7fd82f81db"
---

# Malicious Websites Can Exploit Openclaw (aka Clawdbot) To Steal Credentials

# Malicious Websites Can Exploit Openclaw (aka Clawdbot) To Steal Credentials


# Overview


ZeroPath security researchers found a security flaw in the extremely popular Openclaw AI assistant formerly known as Clawdbot, which has earned 149k stars on its Github repo ([https://github.com/openclaw/openclaw](https://github.com/openclaw/openclaw) ) in just its first few months. The flaw allowed malicious websites to abuse its browser relay server and steal cookies from other tabs open in the same browser.


An attacker who can induce a user to visit a malicious site can hijack active sessions from unrelated websites, including from services like gmail or microsoft 365. These session credentials are especially valuable because they do not require two factor auth to use, which is why modern AiTM (attacker in the middle) phishing kits like evilnginx target them.


We’ve reached out to Openclaw via a Github Security Advisory ([https://github.com/openclaw/openclaw/security/advisories/GHSA-mr32-vwc2-5j6h](https://github.com/openclaw/openclaw/security/advisories/GHSA-mr32-vwc2-5j6h) ), and the issue has been patched as of commit[a1e89afcc19efd641c02b24d66d689f181ae2b5c](https://github.com/openclaw/openclaw/commit/a1e89afcc19efd641c02b24d66d689f181ae2b5c) .


# Openclaw (aka Clawdbot)


Openclaw is a bleeding edge, AI-powered personal assistant that has taken the world by storm over the last few weeks. Maturity-wise, it’s unapologetically closer to early stage proof of concept than a stodgy enterprise app, but if you’re willing to give the 0.1 version of an AI agent sweeping access to your computer and accept the sharp edges that comes with, it delivers experiences that are almost magical at times.


It’s exactly because the assistant is so powerful that security issues in it are significant – excited early adopters don’t necessarily take the disclaimers that it should only be used in environments where security is not a priority seriously, making it important to surface and fix some of the rough edges to protect its rapidly-growing community.


# History of Openclaw Security Concerns


Because Openclaw does not prioritize security, researchers have been able to uncover a number of significant issues, most notably Jamieson O’Reilly (@theonejvo) in this[excellent three part series](https://x.com/theonejvo/status/2016510190464675980) .


Jamieson found that many people configured Openclaw in a way that exposes the control panel to the world without authentication, allowing hackers to drive it. He also conducted multiple successful supply side attacks – adding a backdoored skill to Clawdhub, and devising an attack to steal Clawdhub users’ session credentials with a malicious svg.


At ZeroPath, we focused on analyzing the Openclaw code itself rather than the infrastructure around it or issues caused by its default configuration. Our findings are not as flashy as some of this prior research, but they’re important because they suggest that, given the security quality of the code of the Openclaw agent itself, even a properly-configured and locked down instance of the agent can be compromised in unexpected ways.


This is of course part of the deal you make when you install Openclaw – it’s exciting and useful, but does not aim to be secure (yet).


# Openclaw Browser Functionality


To understand the vulnerability we found, it’s necessary to understand how Openclaw interacts with the user’s browser.


The assistant gives the user a number of options to let it access web pages, including installing a Chrome extension. If the user installs this extension, Openclaw starts up a HTTP server listening on port 17892 by default. This server exposes two WebSocket endpoints:


- /extension – extension connects to this from within the user’s web browser
- /cdp – Openclaw connects to this


To orchestrate the browser, Openclaw sends commands using the[Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) to the websocket endpoint at /cdp. The relay server then forwards the commands to the extension listening on the /extension websocket, and it executes them within the user’s browser.


# The Bug


The endpoint we’re interested in on the browser relay server is the /cdp one, which accepts arbitrary CDP commands. This endpoint seems secure at first glance:


```text
const   wssCdp   =     new     WebSocketServer  (  {     noServer  :     true     }  )  ;


server  .  on  (  "upgrade"  ,     (  req  ,   socket  ,   head  )     =>     {
const   url   =     new     URL  (  req  .  url     ??     "/"  ,   info  .  baseUrl  )  ;
const   pathname   =   url  .  pathname  ;
const   remote   =   req  .  socket  .  remoteAddress  ;


if     (  !  isLoopbackAddress  (  remote  )  )     {
rejectUpgrade  (  socket  ,     403  ,     "Forbidden"  )  ;
return  ;
}


// ...
if     (  pathname   ===     "/cdp"  )     {
// ...
wssCdp  .  handleUpgrade  (  req  ,   socket  ,   head  ,     (  ws  )     =>     {
wssCdp  .  emit  (  "connection"  ,   ws  ,   req  )  ;
}  )  ;
return  ;
}


rejectUpgrade  (  socket  ,     404  ,     "Not Found"  )  ;
}  )  ;


```


When a caller initiates a websocket connection (via the UPGRADE verb), Openclaw first checks to verify that the network connection is coming from an IP address local to the computer it’s being run on… if that check fails, the connection is rejected. Even if a user configured the relay to be accessible on the public internet, attackers could not connect to it.


However, when javascript code served by a website running in the user’s browser creates a websocket, it uses the user’s own network interface to make the connection. This means malicious code running in a browser can evade the security restrictions and connect to the browser relay websocket, like this:


```text
const   ws   =     new     WebSocket  (  "ws://127.0.0.1:18792/cdp"  )  ;
```


Once connected, the malicious code can use the[Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) to orchestrate the browser – for example, to run javascript in another tab:


```text
ws  .  send  (  JSON  .  stringify  (  {
id  :     2  ,
method  :     "Runtime.evaluate"  ,     // evaluates javascript
sessionId  :     // session id obtained via previous command
params  :     {
expression  :     "document.cookie"  ,      // or any JS
returnByValue  :     true
}
}  )  )  ;


```


Among other things, this allows a savvy attacker to steal and exfiltrate session tokens from other tabs, or insert malicious code into these tabs.


The[Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) is fairly extensive, and it may be that further abuse is possible.


# The Fix


When javascript code initiates a websocket connection from within a browser, regardless of whether it executes in the context of an extension or a webpage, the browser sets the “Origin” header on the request to the URL of the webpage or extension.


This security measure cannot be bypassed.


A minor modification to the browser relay limits websocket access to only the expected extension:


```text
// ids of approved Openclaw extensions
const     ALLOWED_EXTENSION_IDS     =     new     Set  (  [
"abcdefghijklmnopqrstuvwxyz123456"  ,
]  )  ;


const   origin   =   req  .  headers  .  origin  ;


// websocket connections from extensions have an origin url that begins with chrome-extension://
const   match   =   origin  ?.  match  (  /  ^chrome-extension:\/\/([a-z]{32})$  /  )  ;


// Allow connections without origin (not initiated from within browser) and connections from the valid extension
if     (  origin   &&     (  !  match   ||     !  ALLOWED_EXTENSION_IDS  .  has  (  match  [  1  ]  )  )  )     {
rejectUpgrade  (  socket  ,     403  ,     "Forbidden: invalid origin"  )  ;
return  ;
}


```


This mitigation does not block attackers with code running on the user’s machine from abusing the browser relay server – since those attackers are outside of the context of the browser, they can connect to the local server using any Origin header they want (or none at all).


Possibly for this reason, Openclaw maintainers went even farther in their patch ([https://github.com/openclaw/openclaw/commit/a1e89afcc19efd641c02b24d66d689f181ae2b5c](https://github.com/openclaw/openclaw/commit/a1e89afcc19efd641c02b24d66d689f181ae2b5c) ), adding active authentication on the /cdp endpoint, making it more difficult even for malicious local code to abuse the /cdp endpoint.


# Full POC


We’ve provided a fully-working proof of concept for people who’d like to test if their setup is vulnerable, or explore further:


Code:[https://github.com/ZeroPathAI/clawdbot-stealer-poc](https://github.com/ZeroPathAI/clawdbot-stealer-poc)
Live version (will not work against Openclaw versions released after February 2nd 2026):[http://clawdbotstealer.s3-website-us-east-1.amazonaws.com/](http://clawdbotstealer.s3-website-us-east-1.amazonaws.com/)


# AI-Powered SAST


We discovered this issue quickly using our own ZeroPath’s AI-Powered SAST engine. We won’t bore you with a sales pitch here, but if you’d like to find out more,[this post](https://zeropath.com/blog/how-zeropath-won-over-curl-with-170-valid-bugs) about ZeroPath finding 170 valid, accepted issues in the curl project is a good starting point.
