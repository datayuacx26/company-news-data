---
schema_version: "1.0.0"
document_id: "a9b9b35dfa0d68d5593220f71ba731c80a4a1a11b4021aaea2764a0097cf7462"
company_key: "yc-warp"
company: "Warp"
source_id: "yc-warp-news-import-3eac4f975b78"
canonical_url: "https://www.warp.co/blog/rise-of-design-engineering"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T06:20:56.453666+00:00"
fetched_at: "2026-08-06T06:20:57.553965+00:00"
content_hash: "sha256:072683652c64eebaaad9d60f51094518e1873f959573c8a224c100ce649c7873"
---

# The Rise of Design Engineering

**TL;DR:** A design engineer is someone who works in both code and design, treating them as one job. The point of the role is to fill the gap where quality usually dies, both in code and design.


This is an edited version of a talk I gave at MIT Startup Week on the rise of design engineering, why the role exists, how to develop the "taste" it depends on, and where I think all of this is going.


## Where a design engineer fits


Most teams still split the work: people who care how things look, people who care how things work. Between them is a gap, things one side knows that the other doesn't, and neither can fully explain to the other. That gap is where quality dies.


It takes real effort for a designer to hand something to an engineer who doesn't understand type systems. It takes the same effort for an engineer to walk a designer through a new auth flow and say the design has to work like this. A design engineer lives in that gap: fluent in both, refusing to treat them as separate. You look at a Figma file and see the code underneath. You sit with a backend engineer who just shipped a messy API and turn it into something that feels right on screen.


## The day-to-day of a design engineer


I'll use myself as an example. At Warp, I'm mostly focused on the last mile of development, the same instinct that drove[our recent rebuild of the Warp UI from scratch](https://www.warp.co/blog/warp-2.0) . I work closely with another design engineer, Arnie. Someone hands me a design, and I fill in the gaps: where can I make this more beautiful, where can I push it one step further. I add animations, add transitions, critique what's already been built, and try to leave something better than I found it.


Arnie sits closer to the systems side, building out component systems and making sure the codebase itself is as considered as the product it produces. That consistency matters enormously at scale, in startups and in big companies alike. You need a codebase someone can walk into and build almost anything on top of. That's the yin and yang of design engineering. There's a wide spectrum of us. We're not all the same; we have different strengths, but what we all care about, at the end of the day, is how the interface feels. Making sure it's as high quality as possible. Leaving no gaps, no room for error. We want everything at 110%.


## How this differs from adjacent roles


A front-end engineer, handed a static design, will build exactly what they're shown. Most Figma files aren't spec'd out with animation detail, so a front-end engineer builds what's there and moves on. They're not necessarily wrong to do that, but they lack the opinionated push to make something better than the file they were handed. We both care about shipping the final product. A design engineer just refuses to let the static design be the ceiling.


A full stack engineer's skills are spread wide across the stack, which means they often lack the depth a design engineer brings to the surface layer, the tiny interactions that most people never consciously notice but always feel. A full stack engineer trying to ship a product will understandably gloss over those details. We don't.


And for a designer, the work usually ends in Figma. You can't browse a website inside Figma. What a design engineer brings is the ability to design the whole experience and also know exactly how to turn it into code, skipping the handoff stage entirely. That's the real distinction.


## Why we need design engineers now


It's never been easier to ship something that works. That's exactly why this role exists now.


### Slop.


AI can generate a website in seconds: purple gradients, generic layouts, the whole thing. People feel when something lacks a human hand, even if they can't say why. Bad scroll-driven animation. Off layouts. Color that doesn't sit right. Copy that feels wrong. Spacing that's *almost* fine. A design engineer's job is making sure that never ships on our watch. We want products that feel human. That's the difference between something people want to use and something they merely have to.


### Taste.


Taste gets thrown around online until it means nothing. I think it's simpler than that. Rick Rubin is in the room for some of the biggest records ever made, and he's not a trained musician. Artists bring him a track and ask, "Is this good or bad?” People trust his ear. As a design engineer, you have to be willing to be that person. Take a design, push back, say let's rework this, let's try something else. That opinionated taste is what turns something good into something special.


### Speed.


You also have to adapt fast. A few weeks ago I built a small sound lab with the Web Audio API, with zero background in sound design. Historically that would've meant two weeks of research just to understand what a synthesizer is. With AI, I talked through the problem and had something working in hours. The speed wasn't the point. Once the model gave me something functional but ugly, I could put my own taste on it: different color pops, subtle morphing details, the layer that makes something feel considered instead of generated. Speed without taste is still slop. Speed with taste is the whole job.


## Can taste be learned?


Yes. Here's how I built mine.


### Study.


I started with no design background. I spent time on X, on Are.na, anywhere I could find good work, inspecting what made the good stuff good and the bad stuff bad, then copying it one-for-one in my own projects. Over time that built a sensitivity. I could look at a layout and see the type was misaligned or the spacing was off.


Recently I was looking at Linear's nav bar and caught something most people wouldn't: during a page transition, the inner content was bleeding slightly outside its container because it wasn't wrapped in a` content-visibility` container. That's not a knock on the Linear team, they ship some of the most considered software out there. Almost nobody would consciously register it. Those small things are exactly what years of studying teach you to see. Once you see them, you know how to fix them.


### Take notes.


Coherency is everything. If one part of an interface uses a different timing or easing curve than another, people feel it even if they can't name it. That's the "something's off" feeling. Training yourself to notice and name it - this transition uses a different curve, this type scale doesn't match - is what lets you fix it instead of just sensing it.


### Build.


There's a book called *Steal Like an Artist* , and that's basically the practice. I've copied more interfaces than I can count, not to publish the copies, but to understand why a decision was made so I could apply the same thinking later. That's how you build the mental archive: when to reach for a spring versus an easing curve, when tighter letter-spacing is doing real work. None of this happens by reading about it. You learn by doing. If you're not doing, you're not learning.


## Making The Jump


If you're a designer, you already have the advantage most engineers don't: the eye. What you need is enough technical fluency to put your own designs into something real. Go build in Claude or Cursor. It doesn't need to be a full app. Start small. Over time you'll start visualizing code the way you already visualize a Figma layout: here's the header, here's the markup, here's the component structure.


If you're an engineer, you already have the harder half. Most of the technical problems you'll hit have been solved by someone else on the team. What you need is the eye. Study your favorite products relentlessly. Ask why they made the choices they made, then try that thinking on what you build.


> Warp is[currently hiring design engineers](https://www.warp.co/careers) , if you want to see what this looks like on a real product team.


## The Future


I think we're heading into another renaissance. A lot is getting generated from a single prompt right now, and people can tell the difference between something generated and something cared for. Design engineers sit in that middle: we can move at the speed AI allows and still put in the human layer that makes a product worth coming back to.


It's a weird, good moment. You can build the thing you've always wanted to build, fast, and still build it with style, with quality, with care.


[Watch the full talk here.](https://youtu.be/uEMEs499fUs)


## **FAQ**


### **What is a design engineer?**


A design engineer works fluently in both design and code, and treats them as one discipline. They can look at a design and see what the shipped code should look like, and they can take a rough technical system and turn it into something people actually enjoy using.


### **What does a design engineer do day to day?**


It depends on the person and the team. Some focus on the last mile: taking a handed-off design and adding the animation, transition, and polish that make an interface memorable. Others lean into design systems and component architecture, so the codebase is as considered as the product. The main through-line is the enforcement of quality.


### **How is a design engineer different from a front-end engineer or product designer?**


A front-end engineer usually builds what a static design file specifies. A product designer's work often ends once the file is handed off. A design engineer does both: designs the experience and ships the code, without a handoff in between, and pushes back when something can be better instead of only building it as spec’d.


### **Can taste in design actually be learned?**


Yes. Same as any other skill: study great and bad work until you can name what separates them, note the small inconsistencies, then build things yourself and apply what you've noticed. It starts rough. It gets better with repetition and honest critique.


### **How is AI changing the design engineer role?**


AI compresses the time from idea to working prototype. Things that used to take weeks of research can take hours. That raises the bar. The differentiator isn't whether something can be built anymore. It's whether someone's taste got applied on top of what the model generated. That's the layer design engineers add.
