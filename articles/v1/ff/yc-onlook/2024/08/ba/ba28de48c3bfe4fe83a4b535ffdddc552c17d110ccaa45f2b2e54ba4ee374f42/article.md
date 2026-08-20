---
schema_version: "1.0.0"
document_id: "ba28de48c3bfe4fe83a4b535ffdddc552c17d110ccaa45f2b2e54ba4ee374f42"
company_key: "yc-onlook"
company: "Onlook"
source_id: "yc-onlook-rss-a3426f90edb2"
canonical_url: "https://onlook.substack.com/p/overcoming-the-fear-of-shipping-code"
published_at: "2024-08-21T12:18:18+00:00"
first_seen_at: "2026-07-27T04:07:26.124598+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:22fdf2f339c85194ce49a0d73b0ec104bed6c0304022a41e1bd30610b83d72e5"
---

# Overcoming the fear of shipping code as a Designer

# Overcoming the fear of shipping code as a Designer


### Addressing the reasons you won't ship code + some tools to kickstart your frontend career


[Daniel Farrell](https://substack.com/@danielfarrell)


Aug 21, 2024


Making the leap into software engineering was always an intimidating idea — “shipping something to production” is a phrase most designers would cringe to hear. Yet despite this, I (designer) recently shipped more code than I ever have.


Was it hard? Not as hard as I thought it would be. But that’s what everyone says when it comes to shipping code: “Oh it’s so easy when you get into it!”


Rather than going on platitudes about how you just need to be more determined or something to ship code, I figured that perhaps I can clear the air around some of the “dangers” of trying to ship code. Maybe I can dispel some of the things that may prevent you (a very capable builder) from making your first code contribution to a project.


I’m writing this now while my own novice experience is still fresh. I know the newness will wear off as I become more comfortable contributing to Onlook, so I figured I better capture some thoughts now before my engineering ego takes off. In the meantime, if you’re technical, forgive me for being extremely naive about the basics here – some of this will be dumb obvious to you.


## What could I contribute to a codebase?


Coming from a design background, I often see plenty of things I’d like to improve in a UI. Even with a talented technical cofounder who cares a lot about design, small things sometimes don’t get implemented perfectly to spec (sorry, Kiet).


A recent example? A border radius was 12px when it should have been 16px. I’m not blaming anyone – that’s a tough thing to spot – and also a great starting point for a designer.


My first piece of advice is to get familiar with adjusting styles in a codebase. You probably already know some examples of things you’ve wanted to change in an app you may be designing for, so channel that


frustration observation into a reason to jump into the code. But before you do, let’s address some potential blockers.


## All the Reasons You Tell Yourself Not to Try to Code


You’re probably thinking some form of these thoughts, so let me address them:


-


**Coding is hard to learn!** — Yes. But AI tools have made it so much easier to figure out what is happening in a codebase. Also, much of it can be decoded if you study it long enough. You don’t need to know how to write it, you just need to figure out what code does.


-


**I tried before, and it was just too much to take on** — You definitely have to start somewhere, and for designers, I think the easiest place to see your tangible output is with visuals. We’ve all had our moments of complaining to engineers when a color or style was incorrect, so start with what you’re familiar with and can visually understand. If you try to do too much, you’re going to get overwhelmed.


-


**I don’t want to mess up my computer** — The terminal has this cyber-like vibe and is where engineers go to hack stuff, so there’s a lot of lore around the idea that the terminal can do some serious damage. If you stick to some of the basic commands, you’ll be far from destroying your computer. You can do the same damage with your mouse as you can with a keyboard.


-


**I can’t just start creating; there’s so much setup!** — With design, you instantly see what you’re creating. In order to contribute to a team, you could draw your idea on a napkin, and it could be sufficient to hand off to an engineer. But to actually contribute something useful in engineering, you need to put it in the context of implementation, and onboarding to that context takes a lot of time. I was shocked when my cofounder told me that it takes a few weeks for someone to get up to speed and ready to contribute to a codebase.


You should expect setup to take some time, and it will be a little frustrating, but if you have someone who is patient and willing to go through the process with you, you’ll be fine. For getting set up for style changes, you can be ready in less than a day.


## The Hardest Parts of Contributing to a Codebase


1.


Finding where in the codebase you need to make a change.


2.


Having the patience to troubleshoot when something isn’t quite working the way you’d like.


3.


Getting the version of the codebase you want to work on onto your machine…


4.


…and then giving your edited version to your colleagues.


For numbers 3 and 4, you’ll pick it up after a couple of reps. My cofounder (Kiet) says finding where to make changes in code will always be a challenge. Luckily,


[Onlook Studio](https://onlook.dev/?utm_source=substack&utm_medium=social&utm_campaign=Overcoming%20the%20Fear%20of%20Shipping%20Code) makes it easy to find where to make visual frontend changes in your codebase because it has a 1-to-1 tie from the canvas to your code.


For just getting started with shipping code as a designer, I think improving the styles of an app is a great place to begin. If your team already uses


[Tailwind CSS](https://tailwindcss.com/) , it’ll be very obvious how to change a style when you look at the codebase.


## The Four Tools You’ll Need to Start Shipping Code


There are so many ways of getting into code, it can be really confusing to know where to get started. I’m sure you’ve heard of many of these tools but let me give you the base ones you need.


### VSCode


This is your canvas for writing code—it gives you all the right windows for you to browse through files, create new ones, and run projects. It’s a text editor that lets you open and edit files easily. (


[Download Link](https://code.visualstudio.com/) )


While I haven’t tried it yet, there’s also


[Cursor](https://www.cursor.com/) , an AI-powered code editor. It seems like it combines some of the power of AI with the context of your codebase, but I’ll have to report back later after I’ve tried it.


### Terminal / Command Line


This is probably the most intimidating tool because it really does feel like you’re working with the raw “computer” itself and a misplaced keystroke could be the end of your machine.


In the same way that you navigate your computer with a mouse, think of the terminal as navigating with your keyboard. Yes, there are some destructive things you can do, but if you stick to the commands below (which are mostly just navigation commands), then you’ll be fine.


You can access the terminal from VSCode by floating your mouse at the bottom of the VSCode window and dragging up from the bottom.


The commands you’ll need to pick up are pretty simple—and are either for moving things around, starting programs, and stopping programs.


-


**\`cd\`** = Change your location into a specific directory. This is how you get around the computer and enter “spaces” in your file directory.


-


**\`cd ..\`** = Go up a directory from the one you’re in.


-


**\`ls\`** = List everything you see in the directory. This is especially helpful for orienting yourself in the abstract world of just text representations of a file system. This command especially made me appreciate what it’s like to use a Graphical User Interface to use a computer and not have to type everything out to get anything to work.


-


**\`npm install\`** = The command you’ll use to install things.


-


**\`npm run\`** = The command you’ll use to “run” a program.


-


**\`CTRL + C\`** = Cancel/close the program you’re running.


### GitHub Desktop


You’ll need a way to collaborate with your colleagues, and GitHub is the most popular for coding. You can use your command line to do GitHub stuff, but I prefer


[GitHub Desktop](https://desktop.github.com/download/) because it’s easy to just click on what you need. Also, it’s probably obvious, but if you don’t already have a GitHub account, you should definitely get one.


### AI Assistants


This right here is the secret sauce for getting the confidence to build things. I find


[Anthropic’s Claude](https://claude.ai/) to be really helpful for coding purely because it will write the code to the right side of your chat and explain some of it on the left side.


A screenshot of my code I was asking for help with from Claude


Unfortunately, Claude has a less generous free plan that will limit you after not many messages, telling you to come back in like 8 hours, so


[ChatGPT](https://chatgpt.com/) can be a good alternative, especially if you’re just doing styling improvements to an app.


---


Stepping into the world of coding as a designer can definitely feel overwhelming – it feels a little like you’re entering a battlefield with no idea where to go and nothing to defend yourself with, but it's not as daunting as it seems.


The key is to start small—focus on what you know (styling) and build from there. The tools I’ve mentioned are just the beginning, but they’ll give you the foundation you need to make your first contribution with confidence. The most important takeaway from my experience is that the barrier between design and code isn’t as high as we often make it out to be. With a bit of patience and curiosity, you can bridge that gap and start making tangible improvements to the products you care about.


And who knows? You might find that shipping code is just as rewarding as designing it.


Subscribe for free to go deeper into the world of design and development as we build[Onlook](https://onlook.dev/) .
