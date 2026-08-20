---
schema_version: "1.0.0"
document_id: "1fe7ec7606abd10d62abb042b89a1e5fed9d0bf0e71a6d9448169eaec8a9f91e"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/once-a-maintainer-santiago-pastorino"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:fd5162b0bc38c27a499f6dae01cd19499c67dad74c022b30fed2b865cfd02b4a"
---

# Once a Maintainer: Santiago Pastorino

Welcome to Once a Maintainer, where we interview open source maintainers and tell their story.


This week we’re talking to[Santiago Pastorino](https://github.com/spastorino) , contributor to the Rust compiler team and alumni of the Rails Core team. Santiago is also a cofounder of a software development consultancy called[Wyeworks](https://www.wyeworks.com/) , and he spoke to us from Uruguay.


**How did you first get into programming?**


I live in Montevideo, Uruguay. And I would say that like most of the people back in the day who started when I did, we got into programming through formal study. I went to university to do computer science. It was less of a hobby like it may be for many people now. I studied structural programming, algorithms, object oriented programming, and things like that.


**And what was your first job out of school?**


I started working in some local companies as a consultant doing mostly Java. But my friend and I, we wanted to see if we could do things a little bit differently. And we were getting bored of Java. So we started our own consultancy. And these were the days that DHH and Ruby on Rails was really starting to have its rise with the demo building the blog in 15 minutes and all that. My friend was already doing some Ruby, I was more into Python. But we had some people that wanted to do some prototypes in Rails. And we started to follow people on Twitter who were advocating for things like agile, scrum, TDD. And we found that really interesting.


This was actually a huge shift in the way that we worked. We used to meet with clients, have maybe a document, spend months working on something and then find out afterward that everything was wrong. But since we started our own agency, we could try out these new ways of doing things that we found interesting. That was also the beginning of my contributions to open source.


**Can you talk a little bit about the progression from your first contribution to Rails to becoming a Core team member? Do you remember what your first contribution was?**


I don’t remember exactly, but there was a client that needed a certain feature. And I ended up building something crazy that required me to go into how ActiveRecord worked. So that gave me a taste for the framework. Then I saw one of the Rails Core members say that they were going to have a weekend hackathon sometime soon. And I thought, why not? They included some gamification, you earned points or something. So I ended up sending 3 or 4 simple pull requests, maybe fixing a warning or adding a test case, something simple.


A lot of people think that when you start working in open source you must work on some super complex project, and the first thing you do is a total rewrite, and it’s not true. For me personally I have a really local understanding of what I’m doing and I don’t need to know the whole thing. These are really complex codebases, and it’s really hard to know what’s going on everywhere. I know my little piece of it.


**When did you start getting more into Rust?**


So when I was doing Rails around 2014-2015, I started to notice that people were talking about Rust. I started reading more about it, following people that talked about it, and went to this Rust conference in the US without even doing anything in it beforehand.


In my personal work experience, we were also starting to shift again the way that things were done, and I started to become less interested in the web development side of things. This is without judgment, but there were a lot of changes, mainly on the JavaScript side of things, where it seemed like every day there was something new and that was sort of unsustainable to my brain. It was like today we use A, but tomorrow B, then we use C, and not really for any reason. In my opinion it was changing for the sake of changing. Which is fine, but it was not for me.


So I started to get into more lower level programming. Rust kind of aims to cover the space of system programming, but with added safety warranties. So that was an interesting thing to me. I started looking into more, their solution for safety, and it was very principled and elegant. And so I started attending more events, and eventually I decided that it was a really complex language to just be working on on the side while I was still doing web development, only an hour a week or something. So I decided OK, let’s get a project. And the project ended up being the Rust compiler.


**Ruby has this mission statement or is organized around the principle of developer happiness. What would you say is the motto for Rust?**


Yes. It’s about safety, and about performance. It’s also about having stability in the language. You upgrade your software to the latest Rust version and it’s not going to break. Something that I’ve seen that is kind of new to me is that when you as a developer go to propose a change to the Rust compiler, you can request what’s called a[Crater](https://github.com/rust-lang/crater) run. And what that does is basically we fetch all the libraries that exist, all the Rust libraries in the ecosystem. And we build them and run them and test them against the new version of the compiler. So it’s a way to check with every release that we’re not breaking existing codebases. Of course we don’t have private codebases to check. But at the very least we consistently check that we are not regressing.


**I read the blog post that your team put out a few weeks ago about the reorganization of the compiler team. How do you, as a group of people, think about the roadmap for the project? How do you move the project forward?**


In open source I think these are really interesting questions - what is the roadmap, what is the vision. Because you know, this is not a company where people are getting paid to do some specific thing. There are people that are paid to work on Rust, but by different entities and each entity has different interests, right? So it’s kind of hard to come together and say this is the roadmap, and these volunteers must adhere to the roadmap. In general, different contributors tend to contribute based on their interests.


But there are teams inside Rust that have different responsibilities. The lang team for example, in order to make important changes to the language you need to follow their RFC process where you write a formal document, and there is a vote. There is a community team, a library team, an infrastructure team, and then there is the Rust Foundation, which is kind of the legal representation of the project. Each team runs in their own way.


**It sounds like it’s primarily bottoms up in that it’s driven by the contributors and their interests, but there are teams responsible for different functional areas that have their own processes.**


Lately it’s been sort of half year based. We introduced a concept of project goals for the first half of next year, and we decided together between the volunteers and the team members the kinds of things we want to focus on. And then each goal needs an owner who is responsible for it. They are the main force behind it, and then it needs a little plan, and the people to implement it. If other teams are affected they need to say yes, we agree. So it’s more bottoms up, as you say, versus how a company would do it.


**What would you say is your personal focus for the next six months on the compiler team?**


I have been working on a couple of things. First, we have a project goal that is about building a proper async programming story. And that involves a lot of different things. It wasn’t long ago that async functions inside traits were implemented in the compiler. Before that you needed to use a crate for it, which was basically a[proc macro](https://doc.rust-lang.org/reference/procedural-macros.html) that generated code, right? It wasn't able to generate the most performant code because you needed to box the stuff and that required heap allocation and things like that. So recently the concept of async functions in traits was introduced. We still don’t support dynamic dispatch for those. So at some point we are going to add support for that. We are building a new crate that will allow dynamic dispatch for asynchronous functions inside traits.


There is another project goal about ergonomic ref counting. If you work in particular with a lot of RC or Arc data types and threads and things like that, you need to resort to a lot of clones to get a new reference count. There's a person showing a use case where they need to clone like 20 variables or things like that. So we’re looking at ways to make this simpler and more user friendly.


‍


‍
