---
schema_version: "1.0.0"
document_id: "3452063196909531d5de1e740e90a8b40a9ce32d6fe624a64afaa6ca0770da38"
company_key: "yc-subimage"
company: "SubImage"
source_id: "yc-subimage-news-import-96b948f674dd"
canonical_url: "https://www.subimage.io/blog/react2shell/"
published_at: "2025-12-08T00:00:00+00:00"
first_seen_at: "2026-07-26T02:26:23.667341+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:c7d564c6d09989875106e13ea8a139ed5b7c960872d0aa5bdb2f4a2fd877b38e"
---

# How we used SubImage to fix React2Shell on our own infrastructure

This past Wednesday I had the chance to use our own product to fix the[React2Shell vulnerability](https://news.ycombinator.com/item?id=46136026) on SubImage's infrastructure (P.S. If you're concerned about this vuln, this guidance[from Vercel](https://vercel.com/blog/resources-for-protecting-against-react2shell) is a good resource).


I love learning from incidents like this because they quickly expose whether what we're building is useful or Yet Another Single Pane of Glass™️ that helps no one. Within minutes we knew we had the vulnerable package, which workloads were affected, whether it was exploitable, and we shipped a fix anyway.


This post is me bragging a little about our product and our deployment stack :), but it's also a walkthrough of what worked, what we want to improve, and some broader reflections on modern vulnerability management.


## Seeing the vulnerability


We saw the news of the vuln on Wednesday and conveniently enough, it showed up as a critical result on the SubImage deployment that we use for ourselves.


I was able to drill down and find out that React2Shell was affecting us on our backend, frontend, and scanner services on AWS ECS:


Our frontend does run next.js so it makes sense that the vulnerable version would be present there, so it was a bit surprising that it was also present on the backend and scanner.


This is an opportunity for improvement:


1. Is this data 100% accurate?
2. If yes, why are we including next.js on an image intended for backend or scanner use? (We might have been running tests for finding vulns on container base images, but more on that in a different post)
3. Is the vuln actually exploitable in all of the places we've highlighted?


- After all, it is possible that the package is present on the image, but we never run next.js.
- And then even if the image does run next.js, the specific React2Shell vuln requires the` use server` directive, so if our code doesn't use that, then we're not affected.


As a side note, we also run a Kubernetes cluster for testing (not for prod; that'd be severe overengineering at our stage :x)


## Applying the fix


Regardless of exploitability, the fix was straightforward, so we just got it done. We prepared the fix:


merged it to main:


and then deployed it to staging to make sure it didn't break anything in prod:


As another side note, this deploy setup is heavily inspired by our time at Lyft and[their world class dev tooling](https://eng.lyft.com/continuous-deployment-at-lyft-9b457314771a) .


SubDeploy goes ahead and triggers a change in ECS:


We verified that everything worked in staging, and then rolled out the change to all of our tenant environments:


Our vuln scan ran an hour later, and confirmed that we had resolved the issue🎉. End to end, it took longer for our container image to build than for me to understand what was going on and get a fix shipped.


I'm proud that we were able to build something that helped our own security, especially because having the problem yourself is the best source of product discovery for a startup.


## Reflections


Let's zoom out for a moment. The React2Shell vuln allows an attacker to craft a payload that grants them arbitrary remote code execution on a machine running a vulnerable version of next.js *if* React Server Actions are enabled. Going through the vuln mgmt triage process...


## Did we have assets exposed to the internet that were affected by the React2Shell vuln?


Yes.


### Was SubImage able to answer this question for us today?


Yes, this was the initial alert.


## Did the vuln matter for us?


### Specifically, did we have a machine running an affected version of next.js, that was using React Server Actions, and was open to the internet?


No, we do not use React Server Actions on any of our frontend components.


### Was SubImage able to answer this question for us today?


Theoretically yes. SubImage (via[Cartography](https://cartography.dev/) ) integrates with SAST tools like[Semgrep](https://semgrep.dev/) , and it's possible to enrich SubImage's vuln data with that context.


## If we *had* been using React Server Actions...


At this point we're done with our investigation because we didn't even use the affected feature, but for the sake of argument let's pretend that we did. Here are some questions we should ask next. I'll group them into **protect** , **detect** , and **respond** categories.


### \[Protect\] If the frontend was exploited, what is the impact? Does the attacker gain access to sensitive data?


If React2Shell led to RCE on our Next.js server, the attacker would inherit the frontend server’s own permissions. The frontend already talks to the backend using authenticated APIs, so an attacker with server-level access could call those same APIs and retrieve or modify any data the frontend is authorized to access.


### \[Protect\] What does the attack path and blast radius look like?


SubImage maps out what IAM roles are used by the frontend and backend, so we are able to see the full potential blast radius.


### \[Detect\] How can we detect if the vulnerability is present on our systems? How can we detect if the relevant features involved with the vulnerability are used in our code?


With SubImage we know what libraries are present on our systems through both container image scans and GitHub dependency manifests. Detecting if the relevant features are enabled and reachable is trickier, but Cartography integrates with SAST tools like Semgrep to help answer this question.


### \[Detect\] How can we detect actual exploitation on frontend or backend?


A runtime sensor would help here, though depending on the application this may be too invasive. It's also possible to configure cloud provider specific alerts for this kind of thing.


### \[Respond\] How quickly can we patch our systems?


In our case, it was as simple as cutting a PR and then using SubDeploy to ship the fix to all tenants. ezpz.


## Closing thoughts


Modern vulnerability management needs more than answering "is CVE-123 present?" It requires context, like:


- where does the affected software run
- what privileges does it have
- which users can reach it
- is the vulnerable code path exercised in production
- how can we patch quickly and safely (especially without breaking anything)


In this specific case, our fix action was very simple (cut PR + deploy) so it was better to eliminate the potential attack path as quickly as possible, even if we may not be affected.


More broadly, both fast patching *and* fast understanding are a must. SubImage helped us answer most of those questions within minutes, and I'm excited to sharpen the whole experience over time.
