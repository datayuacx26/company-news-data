---
schema_version: "1.0.0"
document_id: "5421578e46da6b4bafd218d23353867787058e043da4344d27c275c1fac9a023"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/once-a-maintainer-andre-luis-cardoso-jr"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:72b43b0ae9393c3fcdbcc38b936e91a8e2d5532a360aadf06493fa65f9d6567b"
---

# Once a Maintainer: André Luis Cardoso Jr.

Welcome to Once a Maintainer, where we interview open source maintainers and tell their story.


This week we’re talking to[André Luis Cardoso Jr.](https://github.com/andrehjr) , maintainer of the runtime developer console and IRB alternative[Pry](https://github.com/pry/pry) . André Luis is a Principal Software Engineer at RD Station, and he spoke with us from Brazil.


**How did you get into software development?**


I think when I was 14 or so I got my first PC, and I discovered that there were things like programming languages, operating systems, and I just felt like that was what I wanted to do. I remember reading tutorials and books on programming, C, C++, Javascript, even though I didn’t know English very well. So it was hard. Most of the stuff I didn’t understand but I knew it was what I wanted to do for the rest of my life. So I decided to do computer science when I got to university.


**How did you find the transition from self study and figuring things out on your own to a more formal computer science program?**


I thought it was pretty boring. When I joined university, I already knew a lot from the books and tutorials. But they also gave me a foundation that I didn’t have in algorithms, structures, simple stuff that I hadn’t read much about. Then in my second year I started a job as a software engineer for the university. It was in Java, writing software for digital learning for students at the university, e-learning essentially.


**What was your first exposure to the Rails ecosystem?**


When I was at university, I started reading about other languages. I knew Java because that was what they were teaching, then I moved to Python, Ruby, C, Lisp, and Haskell. I had a lot of free time, so I got every book or material I could get my hands on. I really wanted to understand how each of the languages worked. Also back in 2007-2008, the Rails community in Brazil was getting a lot of traction. I remember Rails Summit Latin America was a huge, huge event in Brazil. That conference was a life changer for me. I saw people from Github, from popular open source libraries. I remember David Chelimsky from RSpec speaking at that conference and my decision at that point was, I needed to find a Rails job. I thought here in my city, most of the jobs are Java. So I sent emails to a lot of places and eventually got from a startup in the US, and that was my first Rails job.


**Do you remember your first open source contribution?**


Yeah, I remember making a pull request for the[Thor](https://github.com/rails/thor) library. When you run your Rails server or any command line on Rails, you are actually using Thor. I remember I made a pull request for a simple refactor. It was just renaming a few methods, and the person that reviewed my pull request was[José Valim](https://github.com/josevalim) , who created Elixir but was very active in Rails at the time. So I was really happy when he merged my request.


**After that first PR, how did you start to think about contributing to open source in terms of your time?**


I think mainly for me, I like to help. I like to be useful, and give back to the community whenever I can. So when I see an opportunity to give back, I jump in. But as a software engineer, I believe that you must understand how things are working under the hood, not just be a user of some dependency. You need to understand how it works. And I like to read code. So for me it’s like exercise. When I make a contribution or when I read the code from a dependency that we use, I’m learning and that improves me as a developer and as a person.


At my current job, we have a huge Rails application and we’ve been upgrading Rails for about 10 years. We had a lot of dependencies that broke and we had to make fixes or forks and stuff like that. And that improved me a lot as a software engineer. Whenever I can I try to make pull requests. Right now I see that there are a lot of projects that are not well maintained, but we need more people helping. Most people just make an issue and say “fix this for me.”


**We’ve heard about the “graying of open source” and the fact that the average maintainer has been at it for more than 10 years. Why do you think that is, and how do we get a new generation of contributors?**


At least where I work right now, I try to teach the younger engineers that you need to understand how things work under the hood. And it's not about just learning how the dependency works, but to be a senior engineer, you need to understand more like how the database works. How does Redis work, which is an awesome C codebase to read. And I believe we need to teach the younger generation to contribute more.


For[Pry](https://github.com/pry/pry) , which is what I'm working on right now, the original creator is not working on it anymore. He's not even working in Ruby at the moment. So he moved on to other stuff that he likes and he passed down the torch to me. I was making a few pull requests and he said, “hey, do you want commit rights?” And that was it. He gave me access and let me do my thing. I think that comes with a lot of responsibility. There aren’t many people that I can ask for help. So we need to ask the younger generation and show them how to help.


There's a lot of simple stuff that people can do, documentation fixes or typos or whatever that people can make some pull requests and fixes for. The example I gave you earlier, my first pull request, it was just renaming some methods and I was really nervous. People should know that even making tiny fixes, that means a lot.


**How did you overcome those nerves you felt with that first pull request? We hear a lot from people that they want to contribute, but they are worried they’ll make a mistake and waste the maintainer’s time or get criticized harshly.**


At first, I think what made me comfortable was following the project before learning the code, getting a sense of the structure of the code, reading pull requests for newer commits, how the maintainer was reviewing them, reviewing the docs, and that gave me a sense of what was happening. And second, I believe that you should contribute to something that you use every day. No one is better than you at saying what is right or wrong if you’re using that tool daily.


With Pry, I use it every day whenever I’m debugging or jumping around code fixing things. And I noticed the project was not really active. There were a lot of open issues and pull requests, because the maintainer wasn’t really working on it anymore. And I wanted to make it still work with newer Ruby versions. I know Pry is acting a little weird with Ruby 3.3 for example, so we’re working on that.


**What are some other projects that you've taken inspiration from or you're looking at right now that you think are interesting or pushing Ruby forward?**


The main project that I'm looking at right now is[Prism](https://github.com/ruby/prism) , the new parser that's helping me to remove some code from Pry as well. I think that it's really nice. It's from[Kevin Newton](https://github.com/kddnewton) .


I'm also following[Reline](https://github.com/ruby/reline) closely. Reline is implementation of Readline in pure Ruby. Readline is the base of Pry and IRB. They are making a lot of improvements, like the new IRB features with multi-line editing and auto-completion with some drop downs that made me think I can bring some of that to Pry as well.


‍
