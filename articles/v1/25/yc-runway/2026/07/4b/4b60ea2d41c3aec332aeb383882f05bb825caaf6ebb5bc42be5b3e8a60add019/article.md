---
schema_version: "1.0.0"
document_id: "4b60ea2d41c3aec332aeb383882f05bb825caaf6ebb5bc42be5b3e8a60add019"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/ai-will-never-build-your-app-and-your-releases-will-never-be-fully-automated"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:11b3b06c7496d92bf76659fabf859e2924dbd88262614e17fb2a973f7d124f81"
---

# AI will never build your app and your releases will never be fully automated

AI and its dumber older cousin, automation, are everywhere. Waymo has self-driving cars taxiing people around San Francisco, and the factories where Jaguar produces the I-Pace cars Waymo uses are filled with giant robot arms and conveyances that automate much of the car assembly process. ChatGPT can write a short story about vampires as told by Dolly Parton, and Nvidia uses Electronic Design Automation to assist in making the GPUs that provide it with the processing power to do so. Apple Intelligence will soon generate more and uglier images for you on-demand than any artist could ever hope to draw, and with a bit of know-how, a Swift dev can use the App Store Connect API to automate the uploads of these images to their App Store listing.


This is nothing new. We’ve been trying to make the car drive itself, the computer to think, the plane to fly on its own for as long as these things have existed. Did you know, for example, that airplanes have had automation since 1912?


Yes, that’s right, the first autopilot — which used gyroscopic and attitude indicators to hydraulically operate the rudder to maintain heading and altitude — was invented only nine years after the Wright brothers achieved the first-ever powered flight. Airplane automation has become much more advanced in the 112 years since then, to the point that pilots only manually fly the plane during the early stages of takeoff and the late stages of landing, and are otherwise planning flight routes and managing all of the automation. Still, from the first moment humans could fly, we’ve been trying to push much of the actual work off to computers.


Despite this long history of automation, one essential part of flying still involves much less automation: Air traffic control.


Why?


Well, in a twist that is probably not all that shocking considering you’re reading this on the blog for a mobile release management platform, it’s for many of the same reasons why AI will never build your app and your release process will never be fully automated away.


## Automation is incapable of deviating from standard procedure


You can quite obviously automate many of the steps in your release process. Lots of little pieces of manual work can be wiped away with a bunch of custom fastlane scripts that can take on work that would otherwise involve you or someone on your team repeatedly typing a bunch of commands into your terminal window every week or two. This is useful because it provides a fancy copy and paste that cuts down on busywork.


But you can’t just set those scripts loose and walk away. What if QA discovers several problems during regression testing (and they will)? The fixes need to be implemented, new release candidates need to be made, and more testing needs to be done. Automation can’t take on any of that.


What if someone is two hours late getting their work in for the release and the PM is adamant that the work is included and cherry-picked in? Someone has to make a judgment call on whether this is reasonable and worth the effort, or whether doing so will reinforce the idea that it’s ok to be late with your work.


What if one of the scripts simply fails and sits there broken for five hours until someone finally is like, “Uh, what’s going on with the release? Oh no, a script is broken… who can fix this?”


Maybe these automations aren’t 100% reliable, but the computer is getting smarter thanks to the work of OpenAI and others, so soon enough you’ll be able to trust an AI to create screenshots for your app and write your marketing copy so you’ll at least be able to stop worrying about that, right?


I could be biased since I’m writing this blog post and don’t want the computer to replace me and destroy my career, but no AI in our lifetimes will be smart enough to do this work without quality control. AI has no context to know which five screens represent the most essential features of your app, so you’ll at least need to ensure it’s pointed in the right direction. Trusting an AI to write your copy is an excellent way to sound like everyone else who trusts an AI to write their copy. Imagine a below-average marketer writing your copy for you. Do you really want that?


Air traffic controllers face similar types of problems, even in a perfect world where every plane takes off on time and the weather is always ideal. Pilots could maintain very specific, exacting, and unchangeable airspeeds (perhaps possible, but only on a different planet that doesn’t have wind) and a robot system could probably maintain that air traffic. But even this would immediately fall apart ten minutes into the robot’s first day.


I once had a flight take off an hour late because the flight crew accidentally slept late and the small airport I was at in Tennessee had no available cabs to pick them up. They had to wait for the single hotel airport van to cycle around and get them. I’d like to see an AI overcome the power of sleepiness.


Repeat with late flights throughout the day caused not only by sleepy people but by storms, mechanical problems, fog, cats on the runway, and late delivery of crates of ginger ale and you soon have a system that would spiral out of control without humans there to manually adapt to everything happening. It’s the same thing with all of your releases: late arriving updates, bugs that need fixing, scripts that fail — all of this stuff adds up to quickly knock your perfect plans out of the sky.


## Automation can’t brainstorm and come up with better plans. Neither can AI.


If you spend enough time on Twitter (I’m sorry, X the Everything App, a company I’m super excited to one day very soon trust for all my banking needs), or maybe if you spend too much time on there, you’ll run into a huge number of people who place an enormous amount of faith in the power of AI, imbuing it with fantastical powers that make no sense if you think about them for more than ten seconds.


It’s common to hear people say they’re excited for the day to come when they can hand their ideas off to an AI and it will:


- Write their book or movie script
- Generate an entire film that’s ready for release
- Make a cool video game
- Build their website
- Create their app


While it’s fun to imagine that any person will soon be able to wield the amount of electricity it takes to power Austin, Texas for a year to see their no doubt incredible idea for a video game about cool zombies be brought to life by a computer that can think (and maybe even feel)... this isn’t going to happen.


There are a lot of reasons for this. One of the big ones is that no (good) idea or plan can be conceived, fleshed out, and completed by a single person without feedback, brainstorming, and discussion, especially for a project as complex as a movie or a mobile app. Your idea/plan will not be as good if you don’t talk to other people about it. Automations do not facilitate this brainstorming and discussion, and relying on them to eliminate busy work doesn’t make either of these things any less necessary.


Even if you limit an AI model’s work to something at least reasonably possible in the near future, like coding a view based on a plain language description of what it’s supposed to look like, there is simply no way on earth the AI is going to get the view exactly right even if you’re the Shakespeare of writing prompts. You’ll need to review whatever it generates to make numerous tweaks and fixes and be on alert for any bugs it may have written (though, to be fair, writing bugs makes a robot more of a real engineer than if it were perfect). And your team will still need to think of everything you want it to build. You won’t just be able to ask it to build you a cool view; you’ll need to be very specific to get anything valuable out of it.


This problem with AI and automation extends beyond building an app; it also impacts the release process. Most major mobile apps have a lot of stakeholders invested in their success. Their contributions and feedback have to be facilitated and the better job you do facilitating, the better your results will be. Do all stakeholders have an easy way to keep up with your release process? Are they fully aware of all the steps to get the app out the door? Are their concerns and needs regularly heard, and do they help drive improvements to the process? Or are they kept in the dark unless they ask about it or pay close attention to a Slack channel where you discuss it?


You (or someone or a series of someones) likely put hours into each of your releases. Are you sure you’re doing things the best way possible? The way you find out if you are is by talking, planning, and strategizing. Automation can’t take care of any of this for you.


Air traffic control works very closely with pilots to make flight plans that take into consideration weather at their origin, at their destination, and along the way, as well as all the other planes that’ll be in the air heading to the same destination. They work with those pilots to adapt to those plans as conditions change, using both their own experience and the pilots’ experience to chart the best course of action based on the situation at hand. Then they adapt even more when and if those plans clash with another plane arriving at the same destination.


No automated system or AI can make these plans for ATC or your release process. Or for someone’s movie.


## AI and Automation can’t effectively troubleshoot all of your problems (and may cause some new ones)


If you ask an AI model to give you an answer on how to write a particularly tricky piece of code, the quality of the answer it provides will be highly dependent on whether or not someone has at some point provided a good answer to that question on Stack Overflow and that there aren’t also any equally popular bad answers competing with that good one.


The same thing is true if there’s a bug in your code. AI can find some surface-level bugs — and you will need to verify if what it has found is even the cause of a problem at all — but maybe not the trickier issues where you’re, for example, tracing a problem in your mobile app to an API call it’s making to a backend service where the root of the issue can be found. Some engineers (like you) or a group of engineers will be working to figure that out.


Going down a couple of levels of technical complexity, if you rely on automation — especially if you’ve created a lot of bespoke scripts and processes — you will also have to deal with problems *caused* by the automation. Automation will break down and be buggy sometimes, and you must fix it. The time an automation saves is partially undercut by the time you must spend keeping it running.


When something goes wrong with a flight, air traffic control has to figure out how to deal with that problem. Automation can’t be relied upon to discern the cause and in many cases *is* the cause.


A not uncommon issue (relatively speaking, considering how safe it is to fly) is a plane’s landing gear getting stuck in the down position after takeoff. This may not seem like a big deal since the down position is exactly where you want the gear to be for landing, but there are several issues:


- Aerodynamics are wrecked and the plane burns fuel at nearly 2x the normal rate, so you can’t continue on to your destination.
- You can’t just turn around and land as your plane is too heavy with a full tank, so you’ll need to get rid of fuel by flying around to burn it or dumping it.
- The pilots have no way to see the gear, so they’re relying on the autopilot/sensors to tell them it’s stuck down, but that could mean anything: a problem with the sensor, the gear being stuck only partially down so that a safe landing is impossible, only one gear stuck down, etc.


To plan their landing they need to strategize with air traffic control and then fly low over the control tower so that the people there can look at the gear to see if it’s stuck down and fully extended. If a robot on the plane can’t tell you with 100% certainty that the landing gear is safely extended, then a robot on the ground won’t be able to tell either. Humans are necessary.


The point of this post is not to write off automation (or even way overhyped AI). Automation is beneficial in both aerospace and mobile engineering. The more you can automate away the busywork of software development and mobile releases, the more time you’ll have to put into building and improving your app.


But there is no panacea coming. Automation and AI are tools that humans wield, not magic spells that will take any idea we have and create it for us. We will always need to do the hard work of strategizing, planning, adapting, and building complex apps and systems with other people. It’s important to have tooling and processes to support better collaboration alongside any automation you may create or buy.
