---
schema_version: "1.0.0"
document_id: "8dd86d8b259b7d2192d0801ff20b07dd75aa8d80ef3f83ec72e10806e21585d4"
company_key: "yc-zeplin"
company: "Zeplin"
source_id: "yc-zeplin-news-import-9b7fbebec6c2"
canonical_url: "https://blog.zeplin.io/design-delivery/UX-design-version-control/"
published_at: "2023-08-30T13:55:59.351+00:00"
first_seen_at: "2026-07-24T09:13:15.076143+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:383a0e9752ec2703c4f975747bf8d05207afcfafd6f07845332613099521d1b1"
---

# Conquering the design version control problem when creating UX

If your work involves iterative creation like UX design or software development, you know there could be anywhere from dozens to even hundreds of versions of a project before it’s “done”. Keeping all versions in order is a matter of version control, but version control looks very different depending on whose process you’re looking at.


First, let’s get it straight. What is version control?


Version control is the practice of recording and managing the changes you make to your files, so you can refer back to previous states and understand the differences between them. A solid version control process allows you to:


- Keep detailed track of changes to your work over time, for clarity and documentation
- Easily distinguish between new and outdated versions, to improve[designer-developer collaboration](https://blog.zeplin.io/designer-developer-collaboration-insights-and-statistics) and reduce confusion
- Record and trace back exactly who made which change, for accountability and to prevent and diagnose (potentially costly) mistakes


Another way to think about it: version control is the difference between having to work with a messy pile (that’s tidied up once in a while) vs. a neatly arranged system of files.


Anyone who creates, views, or gives input on versions can work faster and more confidently when there’s version control. So you’d think that any established team would have a standard process for it, right? Well in reality, that’s not the case.


There are two groups in particular who regularly create complex things together, but are worlds apart when it comes to version control practices — designers and developers. Developers have a standard version control system that’s widely-accepted across their industry, designers don’t.


### Git, a standard for version control


Like designers, developers are also in the business of iterative creation. But instead of iterating on a design file, developers manage source code. For developers, version control isn’t up to question. Systematically tracking and managing changes to source code is a core part of how to write great software, and virtually every developer today relies on the same system for it: Git.


Git is an open-source version control system that dates back all the way to the early 2000’s. It gives developers a controlled space and an intentional process for storing snapshots of source code, recording changes, and helping team members view and apply changes to code during development. You might be familiar with a related name, GitHub, a popular cloud-based software for dev collaboration and version control — GitHub is built on Git.


The key things to know about Git as a version control system are commits and branching, two concepts that make up the backbone of how Git works.


Commits are how developers intentionally save code snapshots, or versions, into the system. Branching keeps committed versions separate from code that’s in-progress, so developers have full control and visibility of all commits over the course of development. So rather than maintaining a “messy pile” of versions, Git works like an organized file system where developers view, manage, and control code changes across even the largest projects.


Git is the industry standard for version control for developers. What about designers? They work closely with developers and routinely produce an enormous number of versions, too, yet version control for product design is nowhere near the same industry standardization. Why is that?


## Version control for designers is a two-part problem


### **Problem 1: the product design workflow**


The modern product design process itself poses major challenges for UX version control.


For one, the path between initial design idea to final product isn’t as straight as it used to be. It’s normal for designers to be working on designs continuously and jumping between different steps in the[design process](https://blog.zeplin.io/the-design-process-and-5-reasons-why-it-matters) such as research, analysis, or prototyping. But this fluid approach goes against the idea of control and order — which is what you need with version control.


And second, the product design process has become increasingly more open and collaborative, with multiple designers and non-designers regularly getting their hands on designs while changes are still ongoing. This may be great for ensuring transparency and a shared vision — especially when designers are moving fast — but it doesn’t take much for things to get complicated.


Every team will optimize their product design workflows however works best for them, and the truth is that they’ll lean on tools to help them do that. Unfortunately, today’s design tools prioritize design iteration and live collaboration over systematic[design version management](https://blog.zeplin.io/design-version-management-zeplin) and control.


### **Problem 2: designers’ tools**


So there's the other big problem with version control in product design — the tools that are *supposed* to help you do it, don’t.


UX design tools are meant to be infinitely flexible and customizable. Order, structure, and predictability are not qualities you would expect from them, since that’s simply not what the iterative design process is all about. That’s fine, until you think about version control. Version control systems require order, structure, and predictability, or else they’re not reliable.


Take Figma, for example — the leading design tool among product designers today by a long shot. Figma supports version history, but not direct version control. Version history in Figma is more or less just a chronological list of all your autosaves of the entire canvas, that’s about it.


You might be thinking — “hey, I can name and save versions in Figma. Isn’t that Figma’s version control?”.


Right, there is the option to manually save and name a particular version of your Figma file by grouping a collection of saves, naming it, and giving it a description. But that doesn’t solve some big issues:


- Conflicting copies of the same file can (and often do) appear within the version history
- These versions *always* contain the entire file, even if there was only a tiny change on one screen
- Figma’s interface doesn’t make it easy to spot, distinguish, or quantify meaningful changes over the course of a long project


We also can’t forget about Figma branching, a feature available on higher tier Figma plans. It’s obvious that Figma branching is modeled after Git, but it doesn’t work as an effective design version control system like you’d hope. Let’s talk about why that is.


With Figma branching, designers can separate streams of design work from a main file into different branches where they can continue to iterate. Then, when they’re done with changes within a branch, they can merge them with the main file.


But if you look closely, this experience is very different — and less controlled — than Git:


**Figma branches can quickly get cluttered and inconsistent.** Branches are extensions of an entire file, not a single page or screen. So if you’re trying to use Figma branches for version control across designs, not only would you have to manage and maintain multiple branches for the same file, those branches may contain duplicate and conflicting information.


**Branches aren’t designed for tracking small updates.** Figma themselves actually[recommends against using branching](https://www.figma.com/best-practices/branching-in-figma/#making-minor-updates-to-the-source-of-truth) for “minor or time-sensitive updates, or … little polish changes. For example, copy edits, layer organization, updating colors to use shared styles.” But as you know, these are the exact types of design changes that developers need visibility and version control for during build.


For all these reasons, you’ll more likely find teams using Figma branching to manage large additions to the main file — like a new product feature — rather than for design version control.


## Product designers need version control to build great products


When designing and building products at scale, design version control is one of those things that gets really costly to overlook.


For example, imagine your version control process — or lack of one — led one of your team members to miss a small update in the copy or a few pixels of spacing. These might seem like minor correctable details, but that team member may have to work on a project they thought was already finished. The delay can cause a ripple effect of other missed deadlines, upset customers, and for some in regulatory industries, product flaws that cause irreparable damage.


There’s a positive way to look at it, too. The upsides to having a standard version control system for your product designs include stronger cross-team collaboration, faster production speed, and more consistent build quality. Here are a few ways that’s possible:


### **Reliable source of truth**


Designers should spend as much time as possible designing solutions to meet your users’ needs. Having a structured design version control process protects that time, while ensuring the rest of the product team has a consistent source of truth on design versions and changes.


### **Compliance and traceability**


If you don’t systematically track changes, you’re bound to miss them. And not only do these oversights tie up valuable time across your team, it also poses risk. A design version control system fixes and prevents mistakes that can be extremely costly for some organizations.


### **Product design at scale**


To grow an organization, you need clearly defined processes. A design version control system is one of those processes. A standard version control system prevents inconsistent version tracking across your design teams, so they can focus on their craft and designing beautiful products.


## Solve the UX version control problem with Zeplin


Product designers deserve a standardized system for version control that’s just as structured and meaningful as Git is for developers. This is one of the things we’ve been thinking about and working on for a while at Zeplin. In fact, we've built Zeplin around the[7 principles of design delivery](https://zeplin.io/principles-of-design-delivery/) , and the concept of standardized design version control falls within one of these guiding principles.


Today, Zeplin is ***the only Git-inspired version control system for designers*** , with purpose-built features that don’t exist in Figma and other UX design tools. We help 5+ million users across countless teams manage design versions and accelerate product delivery at scale, including BP, Chipotle, Amazon, PepsiCo, Oracle, and more.


One of the several unique version control capabilities you’ll find only in Zeplin is[Version Diff](https://blog.zeplin.io/announcing-version-diff/) . You can think of it as a way to precisely compare screen versions and instantly "spot the difference" — here's a quick overview:


If you're still craving more info about how Zeplin solves design version control, you can[contact us](https://zeplin.io/enterprise/contact-sales/) or give it a try yourself with[a free Zeplin project](https://app.zeplin.io/signup) .
