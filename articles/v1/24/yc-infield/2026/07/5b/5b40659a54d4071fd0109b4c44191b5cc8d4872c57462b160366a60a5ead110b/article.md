---
schema_version: "1.0.0"
document_id: "5b40659a54d4071fd0109b4c44191b5cc8d4872c57462b160366a60a5ead110b"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/once-a-maintainer-rafael-franca"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:61208a8e33fe8a6f2a55adf51f61406bd3b05cd2c9ed777f7280afe2ae820059"
---

# Once a Maintainer: Rafael França

Welcome to Once a Maintainer, where we interview open source maintainers and tell their story.


This week we’re talking to[Rafael França](https://github.com/rafaelfranca) , member of the Rails Core team since 2012 and contributor with the most commits to the framework. Rafael is a Principal Engineer at Shopify where he leads the team responsible for the Ruby and Rails ecosystem within the company.


**How did you come to be on the Rails Core team?**


I guess I always wanted to be part of Rails Core, at least as soon as I knew what the Rails Core team actually was. But in reality, how it came to be is that one of the Rails Core team members was ****[José Valim](https://github.com/josevalim) - he was the first Brazilian person I knew that was working on open source projects as big as Rails.


Of course later I learned that there are more, but at the time he was the only person that looked like me that was on a team that important, and I wanted to work with him. One year later I was moving to São Paulo to work with José, and I learned a lot. I started maintaining a few open source projects like devise and simple_form. A year later I decided that I would try to help with Rails. I was basically mimicking his steps, doing what he was doing.


**This was pre-Shopify, or were you already there?**


This was five years pre-Shopify, so 2012. I was doing consulting, and working with José and others like[Carlos Antonio](https://github.com/carlosantoniodasilva) who is also Rails Core. That’s how I started, mostly mimicking José, doing what he was doing. Then I found my own things, the things I like in Rails, and to me that’s helping others and unblocking people, growing new contributors, etc.


Then I came to Shopify as the first person responsible for directing open source. At Shopify I had a lot of different responsibilities. The first one was to make sure that Rails works for Shopify, the company. But now it’s a lot of things - YJIT, parsers, LSPs, everything in the Ruby ecosystem we try to touch. I still kept my role at Rails Core. As a person, that’s my role. As an employee of Shopify my role is the entire Ruby ecosystem, not just Rails.


**So Rails 7.1 came out last October. How soon after that release does the planning start for the next version?**


That’s a good question. And I don’t know if anyone knows this, but we never have a plan, and it’s no problem. The first time that we actually exposed a plan was this year, I believe[David](https://dhh.dk/) (DHH) decided to expose a plan \[last\] December. That plan is there, and people actually thought that we started that plan in December. But the things that he created as public milestones have been there for years. Those are things where we say hey, we would love to build something like this, and we use Basecamp to track what those ideas are. It accumulates, and some day people say hey I’m going to work on this, and we do.


One of the reasons why we never made the plan public is that it adds the pressure that we need to finish, and in private we never have to finish the thing. We’ll say “I want this for Rails 7,” for example. But then life goes on, you change interests, and you do other things and forget about that thing, and you don’t feel bad because you never promised anyone that we would do it.


But this year, last October, during Rails World we had our first ever meeting of Rails Core. We’d never had a meeting. Our first meeting was last year. And one of the things we discussed in that meeting was maybe we should have a regular cadence for releases - we try to, but sometimes we miss. Our original plan was to release once a year, but if you pay attention, 7.1 took way more than one year. So one of the decisions we made was let’s try to follow a regular cadence. And the cadence is now going to be six months.


The next release is going to be next month - 7.2, not 8. And the second decision was maybe we should make our plan public. Just to let people know what direction we’re going in, in case they want to steer in the same direction or even change their own plans or if they’re building something similar maybe that can help us. And that’s why David decided to open the milestones. All those ideas were there for years - there are things I put there three or four years ago, and now it’s public for Rails 8.


For a big release like 8 or 7, we try to see how things actually evolve on the internet. The goal for Rails 8 is simplifying, have less moving pieces. So we are planning to remove things that we believe add confusion - like today the assets pipeline has Sprockets, Propshaft and jsbundling. We don’t need all of that. So we use a plan to decide what we want to achieve and then someone will go ok, I’m interested in this area and I’m going to build this. It’s very individual. But we agree as a team with the idea. So like[Xavier Noria](https://github.com/fxn) , years ago said he’d write the auto-loader, he wrote[Zeitwerk](https://github.com/fxn/zeitwerk) . But that was his personal plan, it was not a team plan.


So after the meeting last year, we are going to try to keep the milestones public as we are doing now with Rails 8. The interesting thing though is that most of the things we added to the Rails 8 milestones are coming on 7.2. Because we are not trying to plan specific releases, we are trying to plan big ideas. Those big ideas make sense as a big release, but it does not mean that we need to wait until October, we can release things as we go, and the things we are finishing now are going to be in 7.2, and the things that are not are going to be in 8.0.


**How do you distinguish between those two things? So if you have one over-arching roadmap, and individuals are kind of picking things off the roadmap that they are working on, how do you decide what goes into the major release versus let’s just put this in 7.2?**


There are two main decision points. One is, how big the change is. So when we talk about simplifying the assets pipeline as we are talking about, this needs to be a big change. If we had finished it this month, I would say 8.0 would be released in April, not in October, because for us that’s a big change in the direction of the framework. I think the same thing happened with 7.0 when David introduced Hotwire. People were like ok, maybe I shouldn’t be doing more in JavaScript-land like using React, using webpack, etc., and Rails introduced Turbo/Hotwire with its vision of simplifying the web. It could have been 6.2. Like if you look at the release history, it was always 4.1, 4.2, 5.0. 5.1, 5.2, 6.0. But 6.2 was never a thing. We decided we have this new thing, it’s very big. It’s not going to be a small change for people, it’s not incremental. It’s a large change in the vision of the framework.


8.0 for us is going to be the same. Like I said, the goal is to simplify things. The simplification is large enough that it requires a milestone, a celebration of something. That’s why we are going to say it’s 8.0.


**So it’s more about impact.**


Yes. David usually calls that, because he has the larger picture in his head. In the past we also used the major versions to drop Ruby support. But now we are trying to change that because otherwise we are going to be supporting versions of Ruby that Rails Core or Ruby Core has not supported for years. Like today, Rails 7.1 supports Ruby 2.7, that’s not supported at all by Ruby Core. So now we are trying to change this as well, because removing support for old Ruby versions was a reason to bump major versions. Now it’s not anymore.


**How do you balance items on the roadmap that are maybe from the Core team themselves versus from the community, versus from your work at Shopify? You have all these different stakeholders who use Rails, in some capacity. How do you balance across those different communities?**


So just to reiterate, it’s mostly individual driven. What I care about are some of the same things that David cares about or Xavier cares about. Like there were a lot of changes in authentication to make it easier to build your own authentication. Those were community driven. I said something about it on a podcast, but it was not me that built it. But I was championing it because I think the idea is great. Or Xavier, he cares about auto-loading. So if people have an idea about how to push this forward, he will be open to it.


There are other things where we prefer not to get input from the community - the assets pipeline is one of them. Because everyone has an opinion. And if you try to satisfy everyone you would end up with the same mess we have today. There are so many choices that you can pick and choose and none of them work as great as the default worked years ago. But sometimes in the community there are a lot of small ideas that together make a large vision, and I care about those. One of those is people are trying to extract things from ActiveRecord to ActiveModel. Individuals that don’t work together are doing this. And so I’m trying to look at the overarching story around that, like why are we doing this? What are the reasons someone might extract things right now or in the future? Rails right now is very mature. It’s harder to change. Like I could not just say “the way you do this is wrong, let’s do it completely differently.” And for community members, it’s hard to think about backwards compatibility and stability. They want to change faster. And that’s why you see them building things on the more experimental side, building the libraries from Rails but outside of the framework.


I wish we had a better way to start to integrate some of those ideas back in Rails. Some of them are happening. So encryption is one of them. For years we had libraries that do data encryption. Now it's baked into Rails. If we build it ourselves we can build a different developer experience, we can integrate with different modules, while with a library you have to deal with different integration points. I try not to make Shopify dictate the decisions we make at Rails Core. There are things the team works on that we treat the same as any other contributor - they have to open a PR, a member of Rails Core has to review, and then we merge.


**So besides which features make it in, there’s an element of how much breaking-ness can we introduce. As an outsider it seems like as the framework has gotten more mature it’s gotten more backwards compatible, more deprecation warnings, a lot of work has gone into making that smoother. How does the team think about helping existing applications stay up to date going forward?**


That’s a really good question. I remember when I was doing work similar to what you guys do - it was really hard. The worst time was from Rails 2.3 to 3.0, it was almost impossible to do. I don't know if there are any apps on 2.3 today, but if there are they are in real trouble. So I said no more, now I actually have power to make things happen. I don’t ever want to see that happen again. And one of the things I shouldn’t have said about us trying to change the pace of release from one year to every six months - I don’t want companies every six months to be having to upgrade Rails, removing deprecations, dealing with a lot of code. Like if it was only bumping the version, I would be OK with companies doing that every six months because it’s not a lot of work, but removing the deprecated code or fixing breaking changes - I don't want that every six months. So I'm trying to think about how we can increase the pace without increasing the burden on the community.


There are some ideas I have, like not removing deprecations every release, removing every year only. I don’t know if it’d be the first half or second half of the year, but once a year. So we’d have two versions of Rails with the same deprecations. Things like that. It would allow people to upgrade but they don’t need to do any major breaking changes or rewrite parts of code, etc.


**So you’re able to get features out throughout the year without waiting an entire annual cycle, but people can take those features without breaking their code.**


Yes, exactly. I care a lot about this problem space, and Shopify is the same, and we automated that kind of work. There is a tool at Shopify that helps more than 300 services be upgraded from one version of Rails to another. And the tool helps you open PRs, tell you what you need to do, etc. And we do this every year. We are very good at this. I don’t know that any company gets close. But even for Shopify, I’m not willing to say, hey we have tooling, I want you to upgrade every six months. I’m still on the fence. If we, even with automation, are not comfortable doing it every six months then I don’t want the community to feel like they need to do it every six months. It should be easy to upgrade Rails. It should not be hard. I’m not saying we don’t need expertise, but it should not be as hard as that 2.3 to 3.0 upgrade was. It took Github like three years to upgrade from 2.3 to 3.0.


**Zeitwerk is probably a recent introduction of a major breaking change that was still painful handling the upgrade, but there was tooling shipped as part of the release and deprecations that made it much more tractable and much safer to do than, for example, Strong Parameters in the past where it was much harder to know if you’d done it correctly.**


Yeah, so what I say is, you should need to do work. But the work needs to be directed, and paced and let’s say even official sometimes. Like before, you could call form_for and pass an object, now you call form_with. So we discuss. Should we deprecate form_for? Maybe, yes, it’s simpler for Rails to have one API. But I’m not sure if we should require people to change this code. There’s no reason to.


**So if Rails does go to a six month cadence on release, does that affect the maintenance commitment to older Rails versions? How are you thinking through that?**


Yeah, this is another thing I’m thinking about. One of the reasons we want that cadence is that it’s hard to predict how long your version is maintained. Because if we had a calendar, I would say ok yes - it’s maintained today, next year it’s not. But because we have no strict cadence, you don’t know. We could release tomorrow or in two years. So we want to help people plan. But that’s another problem, because every minor release we do, that means the old one is not supported anymore. Before, you would get at least one year, or more. So one of the ideas I had around this was maybe we should have a yearly maintenance policy, no matter what version you’re on. So 7.1, you have one year, 7.2, that should come next month, you have one year. That would mean we’d be maintaining at least two versions of Rails at once, but that’s our problem it’s not the problem of the community.


To me, the right window is one year for bug fixes, two years for security releases. I think that’s enough. That’s less than other ecosystems do, like if you go to Java, Oracle still supports Java that is 12 years old. I don’t want that, because I think that makes the community lag behind. So I want to balance pushing the community forward, bringing them with us, and still supporting them. I think a yearly window for bug fixes, and two year window for security fixes is enough.


**How much time do you spend thinking about the other frameworks and how they're doing things?**


I don't spend a lot of time looking at other frameworks. I spend a lot of time looking at other languages. Because my larger job at Shopify is to be responsible for Ruby. I have a very good friendship with José Valim still. He created the language called Elixir that has a framework called Phoenix. I chat with him all the time, we discuss different approaches. I know what Phoenix is doing. He knows what Rails does because he was also a Rails Core member. So that one I'm very close to. I pay attention to Hanami, that's Ruby. I read every single blog post they release. I follow those people around. I work a lot with Rust right now, I pay attention to what they get right, what I hate about the language.


JavaScript for me, not in terms of frameworks but in terms of tooling - how easy it is to work with JavaScript in your editor is one of the reasons why at Shopify I created the team to work on LSPs, code editors, parsers, type checkers **** because I believe that thing is good and we should bring it to Ruby and Rails as well.


Tooling especially is what Ruby is lacking right now. Matz three years ago, maybe two years ago, he made a keynote saying Ruby 4 is going to be about tooling. And that’s what Shopify is building Ruby 4 to be. We are working on a new parser because we believe the parser should be the same across all the tooling, so we could have better code mods. Better Rubocop - it’s great but it’s slow, at least for our codebase. Typing is a hard one because I don’t want to push typing to the community, but there is a lot of value in typing especially in large codebases. So the storytelling around typing should be better than it is today - not put typing everywhere, but there is a specific case where you need typing.


So my inspiration from other languages is mostly about the tooling. I want to see what tools they are building so we can build the same in Ruby or even better.


**OK, last question: was there ever anything on the Rails roadmap that was a real moonshot, something that was thought about but never done, that you wish you did?**


There is one, but I’m not sure if I should talk about it. One of the struggles I see a lot of applications have is what to do when your company grows. Like say when your team gets to be more than 20 people - how do you deal with that code base? So one of the things we did at Shopify, with 8,000 engineers, that I think should be part of Rails, but I’m not comfortable yet putting out into the public is to be able to build applications as if it were Lego sets. So you break your app into pieces. A lot of people have tried to do this, like Shopify released a library years ago called[Packwerk](https://github.com/Shopify/packwerk) that helps you to do it, but we never told you how we actually did it, using a feature in Rails that is already there.


The feature is there, but people don’t use it for that purpose. Rails[engines](https://guides.rubyonrails.org/engines.html) could be used to create small applications, and you could use them to build bigger ones. Usually people use engines to share code, but this is not for code sharing, it’s for code organization. The feature is there, people can use it already - that’s how we’re doing it at Shopify. But we could make it easier, because the way we make engines right now makes it hard - it requires you to have a gemfile, gemspec, it actually generates engines to be shared, and that’s not the goal. So the new feature is actually generating less code, not more code. But I’m afraid of sharing this yet because I don’t want people to think that as soon as you create an app you should have this. It’s a tool to solve a problem, but you need to have that problem first. If you use the tool too soon, you’ll be in trouble. That’s why I never shared it outside of Shopify talks, blog posts, etc. I don’t know yet how to document the problem space well enough to tell you “don’t do this yet if you don’t need to.”


**Fascinating. We’ve definitely seen engines gone wrong for modularization inside of smaller teams that they regret doing. But also the idea of how do you scale a Rails app, not just the code but as the engineering team itself grows how do you support it, has been a question the whole time.**


So to me, it’s a sharp knife. But it’s a sharp knife you can’t pull out. So that’s the problem I’m struggling with. It’s built, the code is there, but I haven’t released it. The documentation, to tell the story properly is the hard part. Because as soon as you add it to your app and you realize you don’t need it anymore, you’re stuck. To remove it is a lot of work. So that’s the hard part. It’s one of the ideas I wish I would release, because it would help Shopify a lot, and we’re probably going to have a lot of big companies in the same situation as us. But it’s hard to sell in the right way. Because people will do what they do with Google - oh Google does this, so we should do it. But you’re not Google.


**Right. But in the meantime, you've got, you know, people adopting dozens of micro services or something, and maybe they shouldn't have done that.**


Yes. So the other side of the coin is that this could be a response to microservices because it allows you to deploy the same app in different ways. You can say I have this monolith, composed of many lego pieces. But here I want to make a man, and here I want to make a car. You can use the same lego pieces to make two different things. Google actually wrote a paper last year about this. But we need to get the story right. I would say that’s what Rails is, in part. The code is the least of the thing. Rails is storytelling, it’s leadership, it’s showing that we can do it differently. The code is there in support of this.


‍


‍
