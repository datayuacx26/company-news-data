---
schema_version: "1.0.0"
document_id: "2ff8ca128e11a4f54ad15057bebc2ebff95505804d21e09e71b9041b1d0ce671"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/once-a-maintainer-nate-berkopec"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:6847f5d800177bf41c61dbf0f65bf87df71077233099c6e2d45829f2a4ffdecb"
---

# Once a Maintainer: Nate Berkopec

Welcome to Once a Maintainer, where we interview open source maintainers and tell their story.


This week we’re talking to[Nate Berkopec](https://github.com/nateberkopec) , maintainer of the Ruby web server[Puma](https://github.com/puma/puma) and expert on Rails performance. Nate lives in Tokyo where he runs[Speedshop](https://www.speedshop.co/) , a Rails performance consultancy.


**How did you become a software developer?**


I was going to school in New York, and I kind of knew I wanted to be involved in tech startups. I was just interested in the whole scene, but I didn’t really know how to get involved. So I was just going to meetups and stuff like that in college. And I had a professor basically advise me that if I wanted to be in the tech world, the easiest way to do that was to become a programmer. This was in 2011, which in retrospect was probably the easiest time in the last 20 years to be a junior programmer and get a job. So I did[Michael Hartl’s Rails tutorial](https://www.railstutorial.org/) . I self-studied, and then worked for a friend’s startup, and then got a job as basically the apprentice programmer at another startup, and it sort of snowballed from there. My first and last love was Ruby.


**Did you have a sense at the time, say 2011-2013, that Rails was the right choice? What was the feeling that led you in that direction?**


I think Rails was clearly already the framework of choice for the makers of the world, the people who were just out to build stuff and get stuff done. I think there were also two specific things: the first was a[blog post](https://innonate.com/hope) by a guy named Nate Westheimer. I don’t remember the name of it but he basically described the concept of going into the sweat lodge for a week and learning to code. He was also non-technical and learned to program with the Rails tutorial. I just remember that blog post really speaking to me.


The other thing that was in the water was Why’s[Poignant Guide to Ruby](https://poignant.guide/) , and I didn’t get it. I couldn’t follow what was going on. It didn’t teach me Ruby. But I remember thinking like, if these are the kind of people that write Ruby, then I want to be in that community. Those two little pieces of culture were really important for getting me into Ruby.


**Were you going to meetups in NY around this time as well?**


I remember meeting[Spencer](https://www.spencerfry.com/) (Fry), he’s been kind of a pillar in the Rails community in NY for more than 10 years now and he’s built two businesses with Rails. And Graham Lawler’s Ultralight Startups, that was a big meetup back in the day.


**What was your first contribution to open source?**


My first ever open source contribution was to Rails, and it was just in that spirit of “I'm out here getting stuff done, and this thing is broken - it's in my way. So let's fix it.” It was this weird thing with emails. I didn't know this at the time, but most of us are aware there's a plain text and an html part to an e-mail. The thing is, the order of those parts can matter. So in Gmail, it will display the first part that it can recognize and read. Which I guess is not true of all email clients. So I was looking at emails from our site in Gmail, and they were coming out in plain text, and then in Outlook and they were coming out in html. And I was like, what’s going on here?


It turned out that when you did a format block in ActionMailer, so you would write format.html, txt that was also setting the order of these mime parts in the e-mail, which kind of makes sense if you think about how Rails probably implements this. But to me it was so unexpected. I was like first of all, I didn't know this even mattered, right? Who would have any idea this would be important? So my[pull request](https://github.com/rails/rails/pull/8044/commits/9cf33b55f39779b98604e1652affc2c64873dd9b) to Rails was to basically always put the html part first. And that was my first ever open source contribution.


**What was the feedback from the Core team? Were they like, no one’s ever brought this up before, or obviously yes, we should do this?**


Pretty much, and I think that’s the case for a lot of things in Rails specifically, which is that you come into the project for the first time thinking that everyone’s thought about all of this very deeply. And you, in the trenches, come across a weird corner case that no one’s really thought about before. And so there was a bit of back and forth but generally they were like yeah, merge it in. I think the bar for a new feature or something like that is very different than the bar for changing the behavior of some obscure part of ActionMailer.


**How did you end up getting involved in Puma?**


So for the next three years or so after that first contribution I was working freelance, and I started writing about performance online and eventually wrote my book,[The Complete Guide to Rails Performance](https://www.railsspeed.com/) . And that kind of catapulted me into being the Rails Performance Guy online. I was going to RubyConf and RailsConf a lot, speaking. At the time,[Evan Phoenix](https://github.com/evanphx) was at Ruby Central and running those conferences. So I got to know Evan and[Richard Schneeman](https://github.com/schneems) from Heroku. And at one of those conferences, sometime in 2016, Evan just sort of grabbed Richard and me in the hallway and said, “Do you guys want to maintain Puma?” And we were like, “yeah, sure.” So we did.


**What inspired you to write the book on performance to begin with? Or why was that your focus?**


So this was now on the down slope of peak Rails, right? This is 2015, and the start of peak JavaScript, we'll call it that. And I was frustrated because I was seeing a lot of people start to think, oh, Rails doesn't scale. Shopify was not huge yet. GitHub wasn't massive scale yet. And so we didn't have a Rails community Top 20 website in the world kind of success story. We really just had LinkedIn rewriting to Node, and we had Twitter rewriting to Scala. So two of the big Ruby unicorns were rewrites.


I was really frustrated by this argument because it didn’t make sense to me. And mostly I was like well, I really like Ruby. I want to keep writing it. So I started to learn more about performance and writing about it and trying to change the perception. And the blog posts I was writing about it were getting tons of views. So I was like ok, I guess I’ll write a book.


**Did you get the sense that people were kind of waiting for someone to step into that role? Because many Rails community members have said things exactly like you're saying, they love Ruby so much, they love the projects they’ve worked on, and the Rails doesn't scale stuff maybe bothered them but they didn't quite know what to do about it. So do you feel like when you stepped into that role there was a rush of support around you?**


Yeah, yeah, yeah. Performance is important, but it's not the core value proposition of any business. You have to do something fast. You can't *just* be fast. And so the community was mature enough at this point that there were a lot of apps running and a lot of people that were starting to hit growth pains for the first time. They're thinking like, oh man, we're going to have to end up having to rewrite this thing in a different language. That sounds awful. So I wasn't the only one feeling the anxiety for sure.


**Taking it back to Puma specifically, how do you guys run it as a project? Like how do you manage all of the contributions from a people perspective?**


I think Puma's actually not that high volume of a project because it does one thing and it's not that complicated of a thing, being a web server. We don't have a mission like Rails, to continually expand what we're doing, right. Puma does a thing and we want to do it really well. So that helps keep the complexity of it down in general.


How Puma works is there are three core maintainers now, myself,[Greg](https://github.com/MSP-Greg) , and[Patrik](https://github.com/dentarg) . And basically each of us has our own little thing that we like to do and we just run around doing that thing. So Greg really likes to improve our CI, and Patrik is mostly running around responding to issues. He’s very responsive and more of the hey, can you reproduce this, do we need to move this to a discussion, etc. And then I just sort of fill in gaps. I tend to manage the pull request backlog. So merging or hey, this needs to change before we can merge. The other part of it is that I see it as sort of my mission with Puma to grow and find new contributors. I think the most effective thing I can do to help the health of the project is to just find more people rather than to you know, get in there and cowboy code myself because I don’t have that much time.


**Does anything stand out to you - could be a feature request, or an issue - as the craziest or most unexpected thing you’ve seen as a Puma maintainer?**


I think one thing that was unexpected was this feature that we call reforking, that was contributed by a startup founder ([Will Jordan](https://github.com/wjordan) ) who wanted it for his own app. Basically the idea is that it changes when Puma spawns new processes and which processes it spawns from. Because to spawn a process in Linux, you’re always copying another process. Instead of booting from the master process, this feature boots from what we would traditionally call a child process, the process actually responding to requests. And the idea was that it would reduce memory usage because you wouldn’t need a master process anymore that was just sitting there not really doing anything. That was such an interesting idea, I’ve never thought of that before. Then[Jean](https://github.com/byroot) took that idea and wrote an entire other web server that was basically Unicorn plus this refork feature and he called it Pitchfork. So that was cool because it was clearly such an interesting idea that it inspired other people to do their own stuff with it. I think that was the coolest feature we ever got out of the blue.


**I guess that brings up an interesting thing about Puma as a project, which is that it's an open source project where there are other direct competing or replacement projects that you could use instead. Like in the Ruby ecosystem you've got Puma, Unicorn, other web servers that all kind of do the same thing. How do you think about working on a project of this type, in terms of what could be seen as competition?**


Yeah, I don’t see it as competition I think. I think my job is to basically be a steward of this codebase that already exists, and to make it as good as it can be. And I don’t really care if it gets more or less usage as a result. That’s why I contribute to Puma. I don’t really like the framing of competition in open source, because we’re all doing this for free. What are we competing for? But it is true that it’s a pluggable part of the ecosystem, a web server, and I think it’s cool to look at what other people are doing and to look at those ideas.


The best time is when another web server does something and we go “yeah, we would never do that.” I think the person who is pushing the most new ideas in web servers right now is[Samuel Williams](https://github.com/ioquatix) and[Falcon](https://github.com/socketry/falcon) , and Falcon does two things that Puma will never do - one is fibers. We just aren’t interested in doing fiber-based concurrency, and Falcon does it really well. So we don’t have to do it. The other is HTTP/2. Falcon is HTTP/2 native, and I don’t really see the use case for it for Puma. I don’t think it’s on the feature tracker. The only reason HTTP/2 would be useful in Puma would be slightly better development performance. So it hasn’t been high on the list. It’s cool that we can have multiple projects and make different things a priority. Like the whole reason why Jean wrote Pitchfork was because he didn’t want a threaded web server. Shopify has said we’re not doing threads - we’re doing process based concurrency, and that’s it. So we’re not interested in Puma, we’re going to do our own thing with this Pitchfork server. I think that’s great.


‍
