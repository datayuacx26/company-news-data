---
schema_version: "1.0.0"
document_id: "cb54088a32f7405815814586a1e12fef8d09fa635c7c76ac9c2b9926a0284b6d"
company_key: "yc-recurse-center"
company: "Recurse Center"
source_id: "yc-recurse-center-rss-cb01fb48046e"
canonical_url: "https://www.recurse.com/blog/187-day-in-the-life-sam"
published_at: "2023-03-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:55.368514+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:62f6f99061f69a2296782544ab3c8d1e2c741d8ff6f89d786a5190313333dc13"
---

# RC Down Under: A day in the life of Sam Uong

How do you participate in RC when you’re in Australia? It’s not easy, and different Recursers have taken different approaches to it. Sam Uong lives in Melbourne and started his batch at RC in January. We spoke about halfway through his batch about what he’s working on, and how he’s shaped his days to collaborate with other Recursers as much as possible.


*(as told to Sydney by Sam)*


**2:45 am:** Before I joined RC, I committed to being awake during core hours, which start at 11am New York time. That’s 3am here in Melbourne, so I have my alarm set for 2:45 every day. I quickly get ready – wash up, brush my teeth, make a coffee, and try to be at my desk at 3.


The first week was pretty rough — it’s like flying to another city and having jet lag, except I’m at home. My approach to travel is to immediately switch to the local timezone and stick to it, so that’s what I did here, but it took somewhere about two weeks to fully adjust. Another Australian Recurser told me he had the same experience.


**3:00 am:** It’s still a bit too early to head right into coding, but there are a lot of messages on Zulip to catch up with. I read through everything, and I’ll ping whoever I’ve been matched up with for coffee chats and for pairing. That’s the first couple of messages I’ll send to get organized for the day.


It’s really useful to share what you’re doing with people. I try to make everyone aware of what I’m working on during daily check ins, pairing sessions with other Recursers, and through presentations and other meetings. When I have presented to others (which admittedly I don’t do often enough) I get people come to me afterwards to learn more about what I’m doing, and this also gives them some extra context for pair programming.


**3:30 am:** For the next hour I work on writing an assembler, which is a program that translates a list of human-readable text instructions into binary machine language that can be executed on a CPU. I just started working on this yesterday, and it’s one part of a compiler toolchain that I’m writing for “nand2tetris”, which is a project to build a functioning computer (that can run a game like Tetris) from the ground up (from NAND logic gates).


**4:30 am:** I have a coffee chat at 4:30. This is something that is organized automatically by the chat bot, which randomly pairs me up with other Recursers. I’ve been doing coffee chats every day since my first week, and it’s a great way to meet people and get a sense of what they’re working on and what their interests are. Today I met another person in my batch, and we talked about the game engine that he’s writing in C++.


**5:30 am:** Time for breakfast. Today I’m having soaked oats with blueberries and a mix of nuts and seeds – and another coffee.


**6:00 am:** Today is one of the more meeting-heavy days of the week, and the next meeting is with a group of other Recursers who are learning Rust. This is the programming language that I’m using for my main projects – both the assembler that I mentioned earlier, as well as a language server that I’m building for a language called Octave (the Language Server Protocol is what allows code editors like VSCode or Vim to do things like jump between functions or perform auto-completion).


This is a fairly unstructured session, and people come to show what they’ve built using Rust, share what they’ve learned, or ask questions. Today we took a look at the various string types in Rust, benchmarked the performance of each of them, and also did some experiments to measure how much overhead would be incurred with a copy-on-write string. I also had a question around some advice about efficiency in the Rust documentation, and after some benchmarking and investigation we found out that this was due to a bug in the documentation! (A pull request to fix this was sent later.)


**7:00 am:** For the next hour, I pair with another Recurser on a Linux kernel module. We’d spent a bit of time the other day figuring out the kernel’s build system, and during this session we’re writing some code to inspect the kernel’s page table entries. One of the things I really enjoy about RC is being able to scratch the surface of the software we use every day, and see how things are implemented underneath. I learned a lot about how page table lookups happen, and how virtual memory addresses are mapped to physical memory.


**8:00 am:** The last meeting of the day on Friday is the weekly presentations, where people come to demo what they’ve been working on. I had just done a presentation for my language server project a few days ago, so today I’m just going to see what others are working on.


**9:00 am:** 9am in Melbourne is 5pm in New York, and this is when people in US Eastern Time start to drop off. So this is usually around the time that my day shifts away from group collaboration towards doing my own thing. Today I’m working on my Octave language server for the rest of the morning.


Before I started my batch, I tried to pick a single project to focus on: this language server. But one thing that surprised me after I joined was the breadth of things to work on and learn! I’ve ended up spending time on nand2tetris, machine learning (mostly learning about Stable Diffusion and other generative AI), frontend web development, and a bit of kernel hacking. I try to be careful with how much time I dedicate to each of these areas, so there are a number of things that I’ve decided to put away and pick up again some time after I finish up my batch.


The other thing that really surprised me is the number of alums who stay engaged with the RC community after their batch is complete. In my first week, I was paired with people who did RC a year ago, and it’s incredible to have that community be an ongoing thing.


**11:30 am:** My wife and I go out for brunch at a local cafe, before heading off to the beach. It’s summer in the southern hemisphere at the moment and it’s going to be a warm day – about 30C/90F, so perfect beach weather. The beaches in Melbourne are inside Port Phillip Bay so it’s very calm, not huge waves. The ocean beaches are about an hour’s drive away, but the local beaches are a nice place to relax and go for a swim.


**7:00 pm:** It’s about 8 hours before I have to wake up again, so it’s time for bed! The sun won’t set for more than another hour, but luckily I’ve got really good curtains in my bedroom to block out the light.


The time difference has obviously been difficult, and it’s taken some effort to manage my time and stay focused. Another challenge that I’ve had is that I haven’t really been coding day-to-day for most of the last 5 or 6 years. I studied computer science at uni and worked professionally as a software engineer for a number of years, but like a lot of people I moved into more of a Tech Lead role and then into a management role. So I’m a little rusty and there are things that I haven’t kept up with, such as advances in distributed systems and machine learning, as well as languages like Rust that have recently gained in popularity.


RC has been a great way to get back up-to-speed on a lot of that. I’ve taken three months off of work to attend, so this gives me the time to focus that I didn’t have while I was busy with my day job. And it’s given me access to a lot of very talented people who have a lot of knowledge in these areas, and are motivated to keep developing as programmers, which I think is incredibly valuable.
