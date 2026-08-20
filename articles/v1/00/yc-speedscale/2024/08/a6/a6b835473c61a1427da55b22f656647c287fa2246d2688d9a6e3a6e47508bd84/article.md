---
schema_version: "1.0.0"
document_id: "a6b835473c61a1427da55b22f656647c287fa2246d2688d9a6e3a6e47508bd84"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/join-ken-on-smc-journal-scaling-kubernetes-microservices-and-ephemeral-environments/"
published_at: "2024-08-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:4083feb35dedfff67060e203d54a9f18f9c72d5338a414031ba9287fc7e4aa06"
---

# Join Ken on SMC Journal - Scaling Kubernetes, Microservices, and Ephemeral Environments

## TL;DR:


Check out Ken Ahrens and Scott Moore as they discuss some blockers of developer productivity when building in Kubernetes, and how removing environment and data challenges can reduce toil and frustration! You can catch the full podcast on Scott’s page here: \[caption id=“attachment_16610” align=“aligncenter” width=“800”\] SMC Journal Ephemeral Development Environments\[/caption\]


Scott Moore: \[00:00:00\] Hey everybody out there in internet meme land. It’s time to hide your kids and hide your wife because it’s time for the SMC Journal podcast. Some of you will get that joke. Others will not. I’m Scott Moore, your host. Thank you so much for being with me today. This is the podcast where we talk about software engineering in the enterprise IT space.


We talk about testing, tuning, performance, observability, security, AI, and more. Today is a special episode about Kubernetes. Specifically, how do you scale it properly? And what’s the necessary approach to do that? We’re spending a lot of money on these environments for it, and it’s just getting out of hand.


How do we deal with this? Well, in order to answer that question, we need to talk to somebody who is used to solving that problem. And that’s why today I’m going to be talking to the co founder of[Speedscale](https://speedscale.com/company/) . We’re going to be talking about problems that people are experiencing In scaling Kubernetes in a microservices and containerized environment.


So let’s \[00:01:00\] talk to Ken. Ken, welcome back to the SMC Journal podcast. It’s great to see you again. Yeah, great to see you too, Scott. I think Speedscale’s been on the show maybe three times? This is either the third time or the fourth time, uh, so your regular guest, uh, but for those are watching this show for the first time and they don’t know who you are and who Speedscale is, why don’t you talk a little bit about that?


[Ken Ahrens](https://speedscale.com/company/) : Yeah, awesome. Thanks, Scott, for having us again. Uh, Ken Ahrens, I’m the, one of the co founders and CEO of Speedscale. We are a Kubernetes development tool for developers who are building microservice. Apps and trying to figure out, Hey, I’m changing this code. Is this about to blow up before I push it into production?


We help them figure that out by creating the production conditions, uh, in their staging environments and on their own local development machine. And we do it by capturing the real traffic from an application and letting them replay it. Change the scenarios around and things like that. \[00:02:00\] So, uh, definitely we don’t want to, uh, last thing you want when you’re putting some code in and pushing it to prod and go to lunches is to get that alert that something’s broken.


So we want to prevent that as much as possible.


Scott Moore: Those people who have been doing performance testing and load testing in the past, they’re used to creating automated scripts, replaying them through some kind of a mechanism. And then, and watching this. This is totally different. This is, you’re taking like actual traffic and converting it in some ways so that you can replay it as if it’s real trafficking in, and you deal with all of those things, like making sure that the dynamic data that’s being passed is always different so that we’re not caching databases.


Like you’ve solved those problems, right?


Ken Ahrens: Yeah, absolutely. So, uh, our approach is, is different. People are very familiar with this space. I can sit down and write a script. I can write a load test and things like that. We want to help developers do that faster instead of having to write scripts or write code.


We produce the traffic. It looks like a spreadsheet or \[00:03:00\] a logs, a list of all the different API calls. And obviously you can’t just take it from one environment and replay it in another. Uh, hopefully you do some kind of authentication and those authentication tokens expire. We use basically ETL type of data transformations where you can say transform this data and play it in the new environment, instead of writing a script, you write a transformation rule, but you don’t have to write it over and over, you just do it once.


Then you can grab traffic. I’ve got customers who dip in, uh, into production every day and they go grab little slices of traffic and they’re always available to the developers and they can say, okay, let me grab that five minutes from this morning replayed on my machine and ooh, that would be bad if I released it.


No one has time to sit down and write scripts for, for a couple of weeks before they do a release. And so we’re trying to automate that whole process out.


Scott Moore: I think it’s a really cool approach. It’s different. You gotta have a little bit of a mindset change there, but it’s very cool. And one of the areas where you guys are really strong \[00:04:00\] and where you’re emphasizing your software is in the Kubernetes space, right?


You’re helping to figure out how to make Kubernetes perform, how to make it scale. And that is something that I think traditional load testing vendors have struggled with. It, Kubernetes has just taken over the cloud, like it’s the operating system of the cloud. And it was so fast. It was like, well, Yeah.


We can still do some of the stuff we used to do with traditional load testing tools and we’ll create scripts and we’ll, we’ll just kind of hit the website the way we normally did or the application getting into the intricacies of seeing how Kubernetes works, all the components to it and how to make that scale specifically and monitoring it, by the way, and using things like Prometheus.


And it’s been a change. It’s been a big, a slow change, in my opinion. Tell me about how you, uh, are, are able to just hit Kubernetes. And see how it scales.


Ken Ahrens: Kubernetes enables the teams to build microservice architectures. And the, there’s a bunch of advantages that you get in a \[00:05:00\] traditional kind of application, you would have a monolith and testing a monolith is, is a very straightforward.


You can do an end to end test and get a good idea of how this scales and say, okay, I need 20 servers, a hundred servers, whatever it is. Well, Kubernetes lets you break the monolith into pieces, and instead of scaling the whole thing, you can scale each piece so you end up with the ingress where all the traffic comes into your environment.


You want to scale that up because everything is. It’s the front door. Everything’s coming in, but you’ll have some service that gets a lower amount of traffic and you don’t have to give it as many resources and you get a couple of advantages here. You can update just one component without updating the others.


And so the way that you release it becomes a lot different and it’s very streamlined. So I can make one small code change and then do a quick validation and push it to production. And I get this big efficiency for my ability to, uh, to have a time to market \[00:06:00\] advantage. That’s why companies pick technologies like Kubernetes because I can say make a small change.


I can get it into production. Well, the idea of sitting down and running an end to end test starts to fall apart because I didn’t change the end to end system. I’ve just updated one micro service here. I’ve just added one small algorithm. And so what we allowed teams to do is to decouple the, uh, these problems and say, I just want to test a service in isolation.


Well, uh, it becomes difficult. Let’s take an example of a payment API. And I just have that one payment microservice. I don’t need to test the rest of the system. Well, payment API probably call something like a Stripe or a PayPal or a credit card processor. And now in my non prod environment, I need that set up.


I might not have it. So you need to mock that out. A lot of people don’t want to take the time to mock out these endpoints because it’s labor intensive. So we built an automation That just like how we can automate the inbound calls that look like load tests, we can automate the outbound outgoing calls out of the app and one little container.


It’s \[00:07:00\] run automatically and it’s boom. I’ve got a virtual stripe. I’ve got a virtual PayPal. It’s sitting in my cluster. Now I can load the thing up. We actually found that was the huge enabling technology. Once I can mock out these dependencies, I can take my end to end system and break it into smaller parts.


It might be as small as one service, it might be a group we’ve called subsystem testing, a group of two or ten services that work together, and I can load them up, uh, as a group. And that lets teams have a lot more flexibility in figuring out how to discover, is my component or my sets of components scaling without having to do a giant end to end test, yeah.


Scott Moore: I know that you have a history, a long history of being involved with service virtualization and mocking. Today, you feel like mocking is even more important now than it was ever before, right?


Ken Ahrens: You’re totally right. So it used to be there was one key back end system or one key third party provider, uh, that kind of holds up your environment.


Well, \[00:08:00\] once you move to microservice architecture, Every service has dependencies on other things, and you end up with what some people have coined the microservices monolith, where you actually can’t run the app anyway without running every single service. And so you do need a way to decouple things, and like I said earlier, it might not be a single service, but a development team might own four or five services that work together, and what they really want is an ability to run their own stuff.


Without running all the other team services and, of course, dealing with third parties. So the importance of mocking out back ends is more important than ever. And so we’ve actually seen the popularity of[mocking tools](https://speedscale.com/blog/api-mocking-tools/) like[wiremock](https://speedscale.com/blog/wiremock-alternatives/) s, probably one of the most popular open source tools that’s out there.


And, uh, of course, Speedscale for us. It’s a key capability that we think, um, is ground, you know, as, uh, is critical for, uh, teams who are trying to test their code. This is something that, when people think about, you know, Kubernetes, uh, testing it, it’s not just \[00:09:00\] for simulating production, it’s something that you can actually do in development, in pre production environments, right?


The whole idea is about flexibility. We’ve seen some themes around things like ephemeral environments,[developer environment](https://speedscale.com/blog/modern-development-environments/) s,[preview environments](https://speedscale.com/blog/preview-environments/) . Um, what what people are reaching for is they’re trying to figure out. I need to get my new code up and running, but. Again, because of this microservices problem, I can’t just run copies of everything.


I’ve got a couple customers who have 200, 300 microservices. And if you set up a preview environment, you’re not going to let every developer and every MR, uh, create 200 microservices. So, uh, we still want to get a view of the new code and how it’s working. So you need to shrink the environment down. And so a lot of our time actually for these kinds of customers is helping them as part of their CI.


Mock out just the things that are right behind their micro service. I’m working with a hotel company right now. They call it bubble environments, and that’s \[00:10:00\] part of their cloud bees automation that build a new container. They put it in a Kubernetes cluster and immediately install Speedscale mocks right behind it.


And then they run their existing test automation against it.


Scott Moore: Here’s kind of a left field thing. I want to go back to what we’re talking about, performance testing and load testing Kubernetes itself. So these developers are probably really just concerned with their code, their features, their APIs they’re developing, uh, those microservices and the code that’s running inside of them.


But what about the orchestration pieces of Kubernetes that makes this container orchestration work? When you need to tune and be able to scale each of those pieces. Pieces that make Kubernetes work. How do you approach that?


Ken Ahrens: This is actually a very, a very interesting topic because if you remember back in the Java days, uh, you would spend a lot of time running a test and then setting your Java flags, how much memory to give it, uh, the kind of garbage collection.


So Kubernetes has a version of these things, uh, They’re simpler. They don’t, \[00:11:00\] there’s not as many flags and things like that. But how many replicas should I run? Uh, how much memory and CPU, um, and storage should I give to each replica? And, uh, Kubernetes gives us an advantage. It’s very easy to change these settings and do another run.


I recently worked with a customer that they ran a big load test using Speedscale. They saw one of their really key services, which was about like location store location data. It spun up 100 pods. They, that was actually their limit. They didn’t want to get more than a hundred pots. And he looked at it and said, Oh, if we gave each pod a little bit more memory, maybe we won’t need as much.


He gave just a little bit more memory and he could do the same work with 10 pots. So you’ve got to go in and make these settings. And again, it was an end to end test, but as they went in and looked through their monitoring data, they said, this is sticking out and not working right. So there is a pod auto scaler that’s already there.


It will decide when it’s time to make a new. Pod, but you as a \[00:12:00\] user have to define how much memory and CPU does this workload need, and you need good load testing to figure that out. And so these two kind of go together. You have the load testing world going with kind of that SRE DevOps world to figure out how do I size the environments, but the advantages you can really fine tune.


Each service can have its own set of, uh, how it should scale.


Scott Moore: It sounds to me like a lot of companies, they want to jump into Kubernetes and they don’t realize they need to be doing this and then they end up over provisioning all of this stuff and then they pay a whole lot of money in the cloud because they’ve done, they haven’t tuned it and it would save them so much money if they just started off.


By tuning and making it is the most efficient that it could be.


Ken Ahrens: Oh, yeah, it’s very easy to kind of get out ahead of it. Even at Speedscale. Early on, we had a cluster for our production environment. I’m not kidding. Scott is 10 times the size of our cluster today, and we actually have a lot more customers now.


Because early on, you turn it on, you put a couple workloads in and \[00:13:00\] then all of a sudden you have a lot of nodes. There are some innovations that I have seen that help you with your node sizing. We’re using one called Carpenter with a K. It came out of AWS. It’s for, uh, AWS. Eks clusters specifically it will help figure out what’s normal for your workload and get the correct size of nodes so It usually will put a really big node in there and and fit as many pods on as possible And then when your workloads move around it will shrink things back as well.


So Those tools help you with your nodes Which is kind of like how many servers you’re running, but you, your applications are running in the pods. You that’s on you. You got to figure out how to size them. A lot of people have been focused on how to tune these in production. You can use your production monitoring data.


You know, I used to work at New Relic and I’ve, and I’ve used a lot of these tools, New Relic, Datadog, AppDynamics, Dynatrace, they give you a lot of great data to understand how well are \[00:14:00\] you utilizing your infrastructure and people use that to tune production. Then you go into non prod, and none of it is tuned.


I recently found a study that said 45 percent of the cloud bill is going to non prod. I’m working with a customer right now. They do blue green deployments. They have about 350 microservices. Okay, so just one cluster has got 700 microservices running in it. I know because, uh, we have a GUI that lists the services and it timed out pulling them all, the early version of our stuff.


We had to make it work for such a long list. I said, why are you running so many? Well, we have blue, green and production, and this is how we have non prod set up. And then I learned to have two of these clusters. Because they have the current version of the software and the next version of the software.


Well, Scott, what do you think the average CPU utilization is on these environments? I’d say very minimal. It’s approaching zero, right? Yeah. So we’ve got four copies of the environment and then they’ve got a dedicated environment for performance. So \[00:15:00\] this is where service mocking can help a lot because you don’t have to make four, five, six, seven copies of the, uh, end to end infrastructure for non prod.


You can have one good staging environment where you put everything in, but actually work with the developers so that they have what’s what they need on their own machine for a while. I was a big fan of the desktop based Kubernetes. If you look on our blog on Speedscale, I’ve evaluated. I use mini cube is the one from CNCF, but there are others.


MicroK8S from canonical is pretty well known, and there’s kind. There’s many of these different tools. What I found was. It’s too much cognitive load for a developer to say you’re going to be an expert in Java in CICD in building testing tools, and you’re going to know Kubernetes, the cloud infrastructure and all this stuff.


So at Speedscale, what we’ve done the past couple months. Will cut out the whole kubernetes part so you can actually mock out all of your dependencies This code \[00:16:00\] depends on the code from your team scott. It depends on a third party api It depends on a database and with one command line tool on your machine It can mock all those things out and talk to your existing code your java your golang your node.


js, whatever And that’s been a huge surprise We chop away at that 45 percent of the cloud bill and the developers are actually happier because they don’t have to, uh, you know, they don’t have to run minikube and try to understand all the kubectl commands and all that stuff. So, uh, this has been kind of eye opening for us how innovative it is.


Take a look and follow our kind of YouTube as we release some of these capabilities. Our CTO, Matt just did a recording, five minute recording on how you run all these on your own machine locally. And we released it this week. So this is a huge area of focus for us right now.


Scott Moore: Well, I don’t know if you can hear that rumble, but I think there’s some footsteps that are just running towards the screen.


How can I. like Ken has the answer to my problem and getting a handle on all these microservices and the \[00:17:00\] money. So how can people find out more about this and get these solutions from you?


Ken Ahrens: So obviously we’re online Speedscale. com. You can come and read our blogs and take a look at our site, but, uh, stay tuned here to see some of our updates as we talk about.


Developers were working on a VSCode extension, for example, and say, Hey, I got my code, but I want to, um, you know, how about all my dependencies? We’re working on integrations with some of the really popular tools that people use to solve these problems like test containers and things like that. And for all the Kubernetes fans who will be at Kubecon this year in Salt Lake City in November, and we’ve got a booth yet again, and that will be a chance to come and connect with us.


Mhm. Uh, for folks who are in the Atlanta area, we also run the Atlanta Kubernetes Meetup, and we’ve hosted it the last several months at our office in Midtown. So, um, we’re always trying to figure out ways to get out there and connect with the, you know, with folks in person and not just online.


Scott Moore: Yeah, \[00:18:00\] that’s great.


And next time I’m in Atlanta, I’m definitely going to look you up because we got to do some barbecue.


Ken Ahrens: I was thinking about that. So I don’t know if you know Fat Matt’s Barbecue. So you got to check out Fat Matt’s. It’s, uh, just outside of downtown Atlanta and I will definitely take you there next time you’re in town.


Scott Moore: I haven’t been there, but I’m, I’m now it’s on my list now to do. It’s a bucket list item now. So thanks for being on the show, Ken. We appreciate it. And, and welcome back anytime. I want to keep up with what the latest is with Speedscale.


Ken Ahrens: Awesome. Thank you so much for having me, Scott.


Scott Moore: One of the things I was surprised that came out of this interview was the fact that mocking and service virtualization is still a big issue, or even bigger issue.


I thought we had that problem fixed 10 years ago. Apparently not, and apparently more people need to understand how that works and why it’s the best approach to dealing with this type of development. Did you get anything outta this? I’d like to know. You can contact me in various social media platforms.


If you’ll scan that QR code, you can \[00:19:00\] find out all the places where you can reach me. You can also reach me by email atheyScott@smcjournal.com , and I would love to hear from you about this, and I would encourage you to please like this video and subscribe to my YouTube channel. It really helps the YouTube algorithm understand what my channel’s about and that it’s actually worth.


So, thank you again for joining me, and we’ll see you on the next SMC Journal podcast. It’s got more saying thanks. Bye bye.
