---
schema_version: "1.0.0"
document_id: "1c586ea754ae99d26eaba8d5525ce3db5545f3550295c8c15eeb69a90861d625"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/once-a-maintainer-armin-ronacher"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:745e05d9e4b1c30fec9a7a9b92d7d8f2ae69531ab8e69fa730938fa83f427ad2"
---

# Once a Maintainer: Armin Ronacher

Welcome to Once a Maintainer, where we interview open source maintainers and tell their story.


This week we’re talking to[Armin Ronacher](https://github.com/mitsuhiko) , creator of the[Flask](https://github.com/pallets/flask) framework and founder of the[Pocoo team](http://www.pocoo.org/) , a group of open source developers working on several widely used Python projects. Armin is a regular speaker at various developer conferences and currently works as a Principal Architect for Sentry.


**How did you get into open source? Do you remember your first contribution?**


Actually, it took me a lot longer to contribute than to write my own open source software. In 2004 when Ubuntu released Warty Warthog, there was quite a bit of excitement about that Linux distribution. There was a bit of momentum, and I joined a bunch of people and decided it would be a good time to create a community for the German speaking population in Germany, Austria and Switzerland. There we used PHP, VB and I think Dokuwiki. I was also learning Python at the time. It was essentially a community website for an open source project.


Through that I started building things, utilities that I could use to switch this thing over to Python. Basically the idea was to take the PHP code and replace it with Python code. And so I think in 2004-2005 I started my first open source libraries just out of personal interest, and because I noticed that it was kind of hard to do web development in Python compared to how easy it was for PHP. And so that's how I got into it gradually. But a lot of it was my own libraries. I think it took probably a year and a half, two years more until I contributed to other people's stuff.


**When you first released Flask, did you know right away that it was going to be popular? Did it hit right away or did it kind of take a while?**


So the pre-story there is that I wrote the libraries that Flask was based on quite a bit earlier. I couldn't tell you by how much, but the precursors of those libraries are probably like three years older than Flask was. And there was at the time quite a bit of sentiment that you had to do these single Python, no dependency libraries, monolithic, everything in one place kind of thing. And so I made an April Fool’s joke where I bundled all my libraries into a zip file and just made some marketing for it. And that was obviously a joke. But the[joke was surprisingly popular](https://lucumr.pocoo.org/2010/4/3/april-1st-post-mortem/) .


What I learned through that joke is that marketing actually matters quite a bit. With the libraries I did before, I didn't even try to do that. With this one I actually tried to make a proper website, proper documentation. And it felt somewhat clear right from the start that there was a certain need or an interest from people. Now, it was not as measurable as open source projects today with like Github stars and everything. But for me it was measurable by the activity that we had in the IRC channel. Compared to all the questions that came in before, which were once in a while, all of a sudden there was like a constant flurry of questions and answers and it felt like there was more activity behind it right from the start.


**How quickly did you bring in other people to start helping maintain it?**


Before Flask I sort of informally worked with a guy called[Georg Brandl](https://github.com/birkenfeld) and we started this[Pocoo](http://www.pocoo.org/) website where we basically collected a bunch of open source libraries that we created together. It was primarily there as a way of creating a community, which was entirely based on IRC. It wasn't really based on the idea of getting more people in, more on the idea that you're not in this alone. If there are questions, then there's another person you can ask. It wasn’t a very clear work arrangement or anything like that, but it was one where multiple people were always joining or leaving or or doing something. Some of those projects always had people technically capable of committing to it, but there was no real structure around it. And that actually never changed for Flask until eventually[David Lord](https://github.com/davidism) started contributing to it, and he's sort of the face of the project nowadays and he has a much more organized approach.


**I want to talk about a blog post you wrote a few years ago about PyPI naming your project a “critical” package - your post is called**[Congratulations: We Now Have Opinions on Your Open Source Contributions](https://lucumr.pocoo.org/2022/7/9/congratulations/) **. I'm curious now that it's been a few years, do you feel any differently than you did in the post, or how have your thoughts evolved on the package indices?**


It didn’t become much clearer. So that particular \[post\] was just like hey, we're setting a bar here that's not a bar for everybody, but a bar for let’s call it popular people. If you want to publish your critical package now this extra rule applies to you. Which is not a particularly impactful rule, it was just two factor authentication, but it was still a rule that didn't apply to everybody. And it basically meant the index all of a sudden sets something in place that wasn't there originally. And the problem is that the open source package kind of turns from someone's toy into this critical piece of infrastructure. And I think we have never really figured out what the relationship of an index to the open source contribution is.


I don't feel particularly strong about two factor authentication. But I do kind of feel that we have never really addressed the fundamentals of how this sort of stuff is supposed to work and the unclear relationship of the indices to the packages I think is one part of it. It's not the most impactful one. But yeah, it's kind of interesting.


**Do you feel like any other ecosystems are doing it in a better way, different way, worse way?**


I mean the extreme versions of this is do you even need a package index? You have Go which doesn't really have one because you can directly depend on GitHub, but then GitHub is sort of hard coded into the language. I'm not entirely sure if decentralization actually is the solution for this problem, but there are all these different approaches and some of them are maybe better than others in certain aspects.


The fundamental problem is we got very willing to depend on a whole bunch of stuff, on external dependencies, and we haven't really established well defined rules about what it means to depend on something. And so that has created a bit of a mess. And in an attempt to solve this mess, a bunch of people ran to the package indices like “you've got to figure this out now.” So for instance,[left-pad](https://qz.com/646467/how-one-programmer-broke-the-internet-by-deleting-a-tiny-piece-of-code) was an example where people went to the package index and said “Undo this”. **** And they were basically like well, the maintainer clearly went rogue. So we're going to override his decision to unpublish this package. And the question is like, is the index supposed to do that or not? The index can do whatever they want, right? Because they're just republishing under the license that the creator used. And so you can't really take it away.


So yeah, I think all the indices are more or less in the same position. People depend on packages, so the packages kind of have to stay there. But then that always turns political and most package indices don't have the capacity to deal with the politics too much. In theory, if you want to run a package index there should be contracts in place and rules and regulations and everything, but it’s mostly made up and they have quite a bit of flexibility in how to interpret their own rules. I don't think any index is doing it particularly better than others.[npm](https://www.npmjs.com/) probably has the most well defined rules today of how the index is supposed to work.


**Is the npm registry run by Microsoft now?**


I think it's technically run by GitHub, which is part of Microsoft, but I'm not entirely sure how this works. It doesn't have Microsoft branding or anything, so they are probably keeping it somewhat intentionally separate.


I think the core problem is just that there's a big difference in practice from publishing something on an index versus putting it on your own website. And it looks very much the same, but it really isn't. And that's kind of not disclosed. And it's not well understood what the differences actually are, so some people only learn like years later.


**Do you mean, for example, that once I publish to an index I've given license for that index to keep that code forever and make it available to people if they want to? Or what sorts of distinctions are you thinking of?**


One is that the index clearly has rights that it sort of gains - the terms of service of using the index apply and then you have to adhere to it. But in a very specific way, it also takes away some agency. I'm not saying anyone should ever be doing this, but there was a very well known Ruby programmer called[why the lucky stiff](https://en.wikipedia.org/wiki/Why_the_lucky_stiff) . And he decided one day that he’s just done. He's going to remove all his content on the Internet. And that sort of removal of your stuff has lasting implications that really only works if you're in full control over where you're hosting. It’s probably not a good idea, but this was a freedom he had, to take it away. If you're on a package index, you really don't have that freedom. And that is not so much because being on an index takes away a right, but because people literally depend on the package being installable on the index.


So here's an example, non-malicious. When we publish an update to Sentry CLI, which is one of the tools that we have, and there's a bug in it, within minutes there will be a bug report and tons of people will say “hey, this broke.” And it's not because they all suddenly upgraded at this very moment, but because the CI systems run all the time and they run on the latest version. So if something is not working, then you have the mass effect of everybody being broken all at once. In the time before public package indices, it was pretty common that your website wasn't perfectly available. The website might be down every once in a while. You might be like OK, I can't download this right now. So you would mirror it, you would vendor it, you would do a whole bunch of stuff to keep this code around for yourself because the original source wasn't all that reliable to begin with. With package indices, you don't have to do that because the index is there to serve it up for you. But it also means that now this act of unpublication is gone. So the way you interact with an index is just fundamentally different.


I made this joke that curl cannot be unpublished because everybody has their own version of curl. I remember when I worked on a game console, and we didn't even use the official curl version. We used a thing that looked like curl but it was actually written by someone else. It just emulated the curl interface.


**There are also package managers at the operating system level that Ubuntu runs or Debian runs. I get the feeling that they are governed more strictly than the language package managers are historically. Is there any difference?**


Well, I can tell you my version of this. The first time I published an open source library, I think it was a Python web library, and there was no PyPI. Or maybe there was PyPI, but nobody used it. So I went to SourceForge, and if you wanted to create a project on SourceForge, it took seven business days and you had to go through a manual review process. I'm pretty sure I had to sign a letter with pretty terrifying legal text to read through. It was a relatively daunting process compared to ‘npm publish’, which is what we do now. And I don't think that's a good thing. After I had the package there, eventually someone from Debian started putting it into their Debian archive. People don't do this anymore. But I remember for many years I had conversations with Debian people where they told me I cannot do such and such because the license prohibits it, or I have to do such and such because otherwise they can't do their job. For instance, I wasn't allowed to republish a tag on git because they would pin against the tag and if something changed, they got an alert.


I think that eventually it may go back to something like that. But we're definitely in a completely different world now. It's kind of scary in a way how we do software development now because we have a lot of dependencies, but we don't trust anyone. Everybody has complete rights to screw you over. In practice it doesn't happen often. People are good most of the time, but security-wise it's pretty terrifying compared to how we used to do this like ten years ago.


**And ten years ago there was a lot more emphasis on GPL licenses and having the expectation that if anybody used your code that they were going to contribute back to the community. That's changed a lot too. Do you have an opinion on what combination of index, registry, server systems, governance, open source licensing decisions or other kind of decisions make for the healthiest open source community?**


I don't know what the right answer to this is. I'm very skeptical of GPL in general. One thing is that the only way in which you can actually have a GPL enforced is if you're willing to go to court. And I think that has been demonstrated multiple times throughout the years that the most successful GPL projects have enforcement alongside them. And that’s never something that I would want to do. A license is only as good as the enforcement. If the license is BSD or Apache you have to do something that is really extreme for enforcement to ever come into play. Typically it's just like, people forgot to put the license file into some copyright screen. You're not going to go to court for that.


But that also means that if you want to do something beyond open source, like you actually want to make money, GPL kind of always had a way for you to make money. In the historic world that doesn't really work for a more permissive license like BSD or Apache. Then it becomes really complicated, because my belief on how you'd combine open source and and commercial things is that it is no longer open source. For example the license that we use at Sentry is the[FSL license](https://blog.sentry.io/introducing-the-functional-source-license-freedom-without-free-riding/) , where for two years it has an exclusivity period and then on a rolling two years it turns into Apache, or there's a second version that turns into MIT. That’s sort of my and David’s way of squaring this. But it’s also very loaded because it’s violating this belief for people of what open source should be. But if you don't actually have a way to earn money, then it kind of sucks. There’s just burnout and a whole bunch of other stuff that doesn't really work. So there has to be a way to commercialize something along the way without giving up on the benefits of open source entirely. I don't know what the perfect solution is, but that has been what I feel are some of the most reasonable approaches.


**Lastly, it’s been about a year since you released**[Rye](https://rye-up.com/) **. What led you to create it?**


Multiple things sort of led to it. The original version was never published. The reason I don't publish all the things that I sort of hack on in my free time is because I feel like I can no longer put something on GitHub without having some sort of responsibility for it in one way or another.


I wrote it because I had a need to use multiple Python versions and I didn't like compiling them. I don't actually know what finally made me change my mind on publishing it. My best guess is that multiple things aligned. For one, there were people in the Python community raising money for Python tooling in one form or another. And so I felt like, ah, people are going to go and build a package manager or really a project manager/package manager kind of thing. And I just felt like, hey, since I already have it, let me polish this up and show what it could be and maybe somebody wants to build it. There is an article from someone who wrote if you don't have the desire to actually build something, just publish the idea. And so there was a little bit of that, let me just show you the idea of what it could be.


‍
