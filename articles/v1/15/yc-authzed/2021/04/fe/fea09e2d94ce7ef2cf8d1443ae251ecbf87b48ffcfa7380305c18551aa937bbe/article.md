---
schema_version: "1.0.0"
document_id: "fea09e2d94ce7ef2cf8d1443ae251ecbf87b48ffcfa7380305c18551aa937bbe"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/onboarding-with-an-m1"
published_at: "2021-04-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:c3b57f405664e41889f63093a025b56cc2d53635fa3a896b5a0115db32640cd1"
---

# Onboarding with an M1

**Update Jan 7, 2022** : Check out[the follow up to this post](https://authzed.com/blog/m1-to-the-max/) as we document how our team upgraded to M1 Max MacBook Pros!


This is my first week at Authzed as a software engineer and I've been provided with a new M1-based MacBook Pro as my primary development machine. Apple’s new M1 chip has been[receiving high praise](https://www.theverge.com/21569603/apple-macbook-air-m1-review-price-specs-features-arm-silicon) for being quiet, battery efficient, and performant, so naturally I was excited to use my new laptop. Turns out, I wasn’t the only one excited to start using M1 MacBook Pros...


Thus far, Authzed’s services have been built and deployed on x86-based machines. The Apple M1 is an ARM based system on a chip. If *someone* were to find a way to build and run our tools and services on an M1, then presumably any Authzed engineer (especially those who have older Intel-based laptops with loud fans) could do their work building software while enjoying all the nice features of Apple’s new laptop. My teammates graciously volunteered me to be that person 😅.


## Introducing our application stack


At Authzed, we primarily use Python and Go for our backends,Typescript and React for our frontends, and gRPC to communicate between services. We build containers and deploy to Kubernetes, using Docker Desktop on our development machines. We maintain[client libraries](https://github.com/authzed) in several other languages but this stack is our bread and butter (or rice and kimchi if you prefer).


## Installing tools and dependencies


My first step was to install basic tools: homebrew, git, and docker. Thankfully now that a few months have passed since the M1 launch, most popular tools have added native support. For the few tools that don’t have an ARM build, I’ve found no noticeable difference in performance when run through[Rosetta translation](https://developer.apple.com/documentation/apple-silicon/about-the-rosetta-translation-environment) . And for the more niche tools, such as my terminal of choice[Kitty](https://isapplesiliconready.com/app/kitty) , I found these compatibility tracking sites helpful:


- [https://doesitarm.com](https://doesitarm.com/)
- [https://isapplesiliconready.com](https://isapplesiliconready.com/)


Note about Docker for Mac: A[preview of M1 support](https://docs.docker.com/docker-for-mac/apple-m1/) was only recently released on Feb 21. All of my tests were run on preview version 3.1.0 but release candidate versions have been posted since the experiences documented here.


**Update:** As of April 15, the[GA release](https://docs.docker.com/docker-for-mac/apple-silicon/) for M1 support is now available in Docker for Mac 3.3.1 and our containers continue to build and run on the latest version!


Next up was installing Python and Go. My initial attempt to install Python 3.8.3 failed with the error:


1


2


```text
./Modules/posixmodule.c:9221:15: error: implicit declaration of function 'sendfile' is invalid in C99 [-Werror,-Wimplicit-function-declaration]
ret = sendfile(in, out, offset, &sbytes, &sf, flags);


```


1


2


```text
ret = sendfile(in, out, offset, &sbytes, &sf, flags);


```


Others had[encountered this](https://github.com/pyenv/pyenv/issues/1768) and some used a patch to get v3.8.3 building:


1


```text
$ pyenv install --patch 3.8.6 <<(curl -sSL https://raw.githubusercontent.com/Homebrew/formula-patches/113aa84/python/3.8.3.patch\?full_index\=1)


```


1


```text


```


I wanted to avoid depending on a one-off patch and found that Python 3.9 builds natively. A quick run of our tests using 3.9 were successful so we were able to upgrade to 3.9 safely. Go added native M1[support in 1.16](https://blog.golang.org/ports) so I simply installed the latest, 1.16.2, using homebrew:


1


```text
$ brew install go


```


1


```text
$ brew install go


```


All of our required tools had been installed successfully! So far so good.


## Building our services


We use Docker to build containers for our services. Fingers crossed, I ran` docker build .` in one of our service directories in hopes that it would just work...it did not. Without revealing too much about our internal projects, the build errors fell into 2 categories: missing build dependencies and incompatible binaries.


An example of a missing build dependency issue was attempting to install the grpcio-1.34.0 package. The` poetry install` step in our Dockerfiles was failing with the error:


1


```text
FileNotFoundError: [Errno 2] No such file or directory: 'c++'


```


1


```text
FileNotFoundError: [Errno 2] No such file or directory: 'c++'


```


This indicated that the docker image was missing a C++ compiler and/or headers. The simplest solution was to change from a slim base image to a more full image.


1


2


```text
-FROM python:3.8-slim
+FROM python:3.9


```


1


2


```text
-FROM python:3.8-slim
+FROM python:3.9


```


An example of incompatible binaries was attempting to install the psycopg2-binary package. The installation would fail with:


1


```text
Error: pg_config executable not found.


```


1


```text
Error: pg_config executable not found.


```


I didn’t dig into whether the executable was actually missing or not executable on my machine. Again, the simple solution was to update base images by specifying ARM-specific Docker images.


1


2


```text
-FROM node:14.11-alpine3.12
+FROM arm64v8/node:14.16-alpine3.12


```


1


2


```text
-FROM node:14.11-alpine3.12
+FROM arm64v8/node:14.16-alpine3.12


```


After making the above changes to all of our Dockerfiles, voila: our containers built successfully!


One thing to remember: the initial goal was to test whether we could build and run our services for development and testing. A future step will be to set up[multi-architecture builds](https://medium.com/nttlabs/buildx-multiarch-2c6c2df00ca2) to be able to share x86 builds as well.


## Deploying to a M1 MacBook Pro


✅ Tools installed
✅ Containers built
❓ Services deployed


Tools are installed, containers are built, but what good are they if I couldn’t run them? I’ll save you the suspense and show that our full application deploys and runs on Kubernetes 1.19.7 through Docker Desktop.


The challenges of running Kubernetes on Mac are probably best left for their own post but a few tips from my experience are to **turn off** the *“Use gRPC FUSE for file sharing”* option and to **remove** the default shared directories except for */tmp* .


## The result


I set out to find out if Authzed’s tools and services would build and run on a M1 MacBook Pro and after much Googling and some trial and error, I happily reported a simple, “IT WORKS!” back to my teammates.


The first 24 hours using my laptop were filled with video chats, building containers, and Kubernetes running for most of the day, yet it was still going strong with 28% battery remaining. I didn’t hear the fan turn on once and in fact, I don’t think the internal temperature went over 130F even in the middle of builds. (The temp is running at a cool 71F as I write this post).


Safe to say, I think my teammates will be shopping for new MacBooks in the near future.


I’m just getting started here at Authzed but if you’re also interested in these tools and want to help build our fast, flexible, and scalable permissions API, check out our[jobs](https://authzed.com/jobs) page and join us!


### Epilogue


One downside to the M1 MacBook Pros I discovered much too late is that it only supports one external monitor, although there are some workarounds using[docks and proprietary display drivers](https://plugable.com/blogs/news/how-to-connect-more-external-displays-to-apple-silicon-m1-macs) . Despite my idle second monitor, I’ve found the benefits of the M1 MacBook Pro heavily outweigh this one downside.


On this page


- Introducing our application stack
- Installing tools and dependencies
- Building our services
- Deploying to a M1 MacBook Pro
- The result
- Epilogue


## Related


[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Melissa Smolensky · Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)


[Engineering AuthZed is 5: What We Learned from Our First Authorization Infrastructure Event We celebrated our 5th birthday with talks from Canva, Turo, and Carnegie Mellon. Here's what we learned about the dual-write problem, scaling authorization in production, and why everyone keeps reimplementing the PostgreSQL wire protocol. Sep 2, 2025 · 9 min](https://authzed.com/blog/authzed-is-5-event-recap-authorization-infrastructure-insights)[Engineering AuthZed is 5: What We Learned from Our First Authorization Infrastructure Event We celebrated our 5th birthday with talks from Canva, Turo, and Carnegie Mellon. Here's what we learned about the dual-write problem, scaling authorization in production, and why everyone keeps reimplementing the PostgreSQL wire protocol. Corey Thomas · Sep 2, 2025 · 9 min](https://authzed.com/blog/authzed-is-5-event-recap-authorization-infrastructure-insights)


[Company Joining a Startup During a Pandemic My experience starting at Authzed during the pandemic. May 26, 2021 · 6 min](https://authzed.com/blog/starting-at-authzed)[Company Joining a Startup During a Pandemic My experience starting at Authzed during the pandemic. Sam Kim · May 26, 2021 · 6 min](https://authzed.com/blog/starting-at-authzed)
