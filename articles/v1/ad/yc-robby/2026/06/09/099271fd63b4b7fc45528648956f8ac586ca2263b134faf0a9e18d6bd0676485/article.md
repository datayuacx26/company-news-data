---
schema_version: "1.0.0"
document_id: "099271fd63b4b7fc45528648956f8ac586ca2263b134faf0a9e18d6bd0676485"
company_key: "yc-robby"
company: "Robby"
source_id: "yc-robby-news-import-47ee1caa7dbe"
canonical_url: "https://www.tryrobby.com/blog/southern-air-ai-custom-software-paul-interview"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-23T23:13:24.345752+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:15ed81305f1784b7c06904ddd4391665287eb9fed3ef5a8957b18aa475cf2a8a"
---

# Single Source of Truth: How Southern Air's Team Built a 20-Year Software Stack — Then Supercharged It with Claude

This is the eighth installment of our Home Services Leaders in AI series, where we sit down with operators actually using AI and automation to run their companies — no theory, no hype. (Catch up on the[first interview with Sarah at Golden Rule Cleaning](https://www.tryrobby.com/blog/golden-rule-cleaning-ai-automation-sarah-interview) , the[second with Dave at Glass Therapy Clarksville](https://www.tryrobby.com/blog/glass-therapy-clarksville-ai-window-cleaning-dave-interview) , the[third with Adam at Blanton & Sons](https://www.tryrobby.com/blog/blanton-and-sons-ai-automation-adam-grubb-interview) , the[fourth with Bekka at Advanced Air Conditioning + Heating](https://www.tryrobby.com/blog/advanced-air-ai-hvac-bekka-interview) , the[fifth with Kara at Air Repair Pros](https://www.tryrobby.com/blog/air-repair-pros-ai-hvac-kara-interview) , the[sixth with Tim at IRBIS](https://www.tryrobby.com/blog/irbis-hvac-ai-vendor-vetting-tim-interview) , and the[seventh with Matt at Harley's Heating & Air](https://www.tryrobby.com/blog/harleys-heating-air-ai-hvac-matt-interview) .)


[Southern Air](https://www.southern-air.com/) is a mechanical contracting company founded in 1946 that covers three states with roughly a thousand employees. About half the business is construction — building the HVAC, plumbing, and piping systems inside hospitals, offices, and industrial plants — and the other half is truck-based service: maintaining, modifying, and repairing every building they've built or someone else has. They rank near the top 50 mechanical contractors in the country. Paul Denham is the CEO, but he's quick to frame everything as a team effort. He came in 37 years ago as a mechanical engineer and worked his way through project management, engineering, and estimating. The company runs on a strong servant-leader culture — Paul provides ideas and direction, and gives full credit to the team that does the work.


## One Record, One Update, No Exceptions


Southern Air's custom software layer traces back to one rule the team set 20 years ago and never broke: if a piece of data exists somewhere, you reference it — you never recreate it. "If you create two copies of my last name and somebody goes to update one, they don't know they've got to update the other, and you end up with a corrupt database." That principle — a single source of truth, enforced from day one — is what drove the team to build a custom interface layer on top of their enterprise platforms.


Southern Air does use enterprise software — Viewpoint for ERP and Acoro for HR and benefits. But the team's secret is the strong API integrations they've built to customize the interface most employees actually use day to day. It started with an Access database for employee management, something simple that put data at everyone's fingertips. When Access hit its ceiling, they migrated to SQL Server. Every new module — time cards, job tracking, tool inventory, customer records, service tickets — looked back at the existing data instead of duplicating it. An employee number in the time-card module referenced the employee database. A job assignment referenced the job database. "This theory just kept on growing, and then as we found other applications to automate, we would build out these modules that then looked back at the existing data."


Today the system can answer questions like: where was this employee for the last three weeks? It returns every job and every service ticket they've worked on. Thousands of customers, thousands of buildings, hundreds of assets per building — all co-related, all queryable, all flowing from that one founding rule. The architecture took the team 20 years to build. Then Claude showed up and changed how fast it could grow.


> If the data exists somewhere, you reference it, you don't recreate it. If you create two copies of my last name and somebody goes to update one, they don't know they've got to update the other, and you end up with a corrupt database.
>
>
> — Paul Denham, Southern Air


## Nuclear Reactors to Air Conditioning


Paul's background is unusual for a mechanical contractor CEO. His undergrad is in mechanical engineering. His first job out of school was at Babcock and Wilcox, building nuclear reactors for Navy ships. Five years of technically fascinating work buried under layers of defense-contracting bureaucracy. "You add the nuclear industry and you add defense contracting together — bureaucracy on steroids. While I liked the technical aspect of it, I just couldn't handle the BS side of it."


He came to Southern Air on a whim. Knew the thermodynamics, knew nothing about air conditioning as a trade. Thirty-seven years later, he's CEO of a thousand-person company, purchasing a $15 million annual health benefits program and managing the 401k plan. "If you told me when I was in engineering school that I would be doing this, I'd have told you you were nuts. But here I am."


He still finds his way back to the engineering side. Right now his team is figuring out how to prefabricate a six-story assembly of pipes and ducts for a large hospital — build it in one piece in their factory, lower it through a hole in the roof, and take all the intricate, dangerous work off the floor. "We create something from nothing. It's like a giant jigsaw puzzle. We go out and buy all the pieces from somewhere, ship them to a job site, and orchestrate how they have to go together in the right sequence. And then it's all got to work, too."


## Why They Stopped Buying Software


Most operators in this series adopt tools — ServiceTitan, HouseCall Pro, GoHighLevel, Hatch, Rilla.[Tim at IRBIS](https://www.tryrobby.com/blog/irbis-hvac-ai-vendor-vetting-tim-interview) runs one of the deepest third-party stacks we've profiled. Southern Air's team went a different direction. They use enterprise platforms like Viewpoint and Acoro for the heavy lifting — ERP, HR, benefits — but they build the custom interface layer their employees actually touch every day. That layer is where the single-source-of-truth rule lives.


"We would buy software off the shelf, and it began to force us to modify how we did business to the way the software was set up." Enterprise systems like Oracle and Microsoft Dynamics offered configurability, but that configurability became a trap. "You can get the best project managers in the world to help you implement that software. But then you find out, 10 steps ago, we made this decision of how we were going to set up certain general ledger accounts or labor accounts, and it's boxed us into a corner 10 steps down the road." You either go back to stage one or live with the constraint.


"What we kept on finding was it was harder and harder to get our people to adapt to the technology we were trying to implement." So the team flipped the question: instead of asking how to adapt the business to fit a vendor's interface, they asked — we know how to run our business, what happens if we just build the interface ourselves? The single-source-of-truth architecture was their answer. Twenty years of building API-connected modules that mirror existing workflows, reference existing data, and make change management dramatically easier because employees never have to learn a workflow that doesn't match how they already think.


## From 60% OCR to 99% AI on Equipment Nameplates


Asset management is the backbone of Southern Air's service business. They work on thousands of buildings, each with potentially hundreds of pieces of equipment. For every unit — AC-1, RTU-2, whatever the tag — the service tech needs to capture the model number, the serial number, and the unit tag. That data unlocks everything downstream: five-year spend tracking per asset, replacement recommendations, wiring diagrams, repair instructions.


The problem: manufacturer nameplates are a mess. "Some of these model and serial numbers are crazy long, alphanumeric. None of it — it means something, but it doesn't necessarily make sense to a common individual." And the physical labels vary wildly. Standard fonts for some lines, metal stamps for others, rotated text, different fonts from one line to the next. Standard OCR was about 60% accurate. "You might as well just not use it."


📊


From 60% to 98–99% Accuracy on Equipment Nameplates


Southern Air replaced standard OCR with an AI interpretation model for reading manufacturer nameplates. Where OCR failed on mixed fonts, metal stamps, and rotated text, the AI reads model numbers, serial numbers, and unit tags at 98–99% accuracy — and the tech just has to take a photo.


Now a service tech photographs the nameplate. The system reads it, identifies the equipment, and automatically pulls the wiring diagram and repair instructions. "All of a sudden, all a service tech has to do is do that. It registers it. Now we know in real time what piece of equipment he's working on." The company can tell a customer exactly how much they've spent on a specific asset over the past five years and recommend whether it's time to replace or budget for replacement. "We're providing more value to our customers. We're providing more accuracy to ourselves."


## Claude as the Development Accelerator


The nameplate reader is one module. The bigger story is what Claude did to the team's entire development pipeline. Before Claude, maintenance on the SQL Server codebase was eating productivity. Keeping modules up to date with current versions of the base software consumed so much time that new development stalled. "Well, as we've started to implement Claude, it has just accelerated the pace at which we can develop software. We've got all of our modules up to date now with the latest version."


The team uploaded their entire codebase into a proprietary Claude account. The model emulates the patterns and conventions they already use, but the prompts are written in plain English — the same English the operations team speaks. That's what closed the language gap that had plagued the company for years. The operations team would write wish lists. IT would go off for three or four months to build. "They sent their work product to them, and the operations guys are like, 'it's not what I was talking about.'" Then the personality conflicts would start. "And then now they're pissed at each other. Instead, they're on the same team."


Claude fixed it by making the conversation happen in plain English instead of tech speak. "Now my IT people are speaking in the same language that my operations team is, and we're finding they're hitting the mark way faster and way more accurately." Paul's verdict: "I'm solving way fewer personality conflicts than I ever had to." Different shape than[the backend-only rule at Harley's Heating & Air](https://www.tryrobby.com/blog/harleys-heating-air-ai-hvac-matt-interview) , where Matt keeps AI invisible to his crew. Southern Air's team interacts with AI directly — but the interaction is in plain language, not code. Same net result: the humans don't have to change how they think.


> Now my IT people are speaking in the same language that my operations team is, and we're finding they're hitting the mark way faster and way more accurately because they're talking the same language instead of tech speak versus operations speak.
>
>
> — Paul Denham, Southern Air


## DOS to Claude in One Career


The digital transformation at Southern Air spans Paul's entire 37-year tenure. The day he walked in, the company had one computer on a rolling cart with a dot matrix printer and a cathode ray tube screen. Estimating was manual takeoff on big sheets with tick marks — counting fittings by hand, multiplying by material and labor rates. There was an IBM AS400 mainframe for accounting, green screen, command prompt. They still had a mimeograph machine.


Paul was an early believer. He bought one of the very first portable computers — a seven-inch CRT that tilted up, keyboard that clicked onto the front, basically an IBM PC with a carrying handle — and lugged it onto airplanes between job sites in Connecticut and Virginia. "Thousands and thousands of dollars. Very little memory. It used floppy disks. Sometimes you were changing out floppy disks because you had some files here."


Today every person in the office has a laptop. Field staff work off iPads. Drawings are digital. They build digital twins of buildings in BIM software, run clash detection through Navisworks, and collaborate digitally with every subcontractor on a project. The speed went from printing physical drawings, taking them to a reproduction company, and driving copies to job sites six hours away — to instantaneous. "We've gone almost entirely paperless. We're not even printing drawings now for our field staff."


And yet: "We still covet a handful of laptops that run DOS." They need them to interface with certain legacy control systems that predate Windows. Their service techs span the full range — pre-Windows serial-bus equipment to BACnet over IP. "Our service techs are as much a network engineer as they are an air conditioning technician."


> Our service techs are as much a network engineer as they are an air conditioning technician, because very often we may be interfacing with equipment through NFC, Bluetooth, special cables with some type of serial bus, and a lot of the communication is switching over to BACnet over IP.
>
>
> — Paul Denham, Southern Air


That range — DOS to Claude, mimeograph to iPad — is what makes Southern Air's AI story different from the other operators in this series. Most are small shops that started digital-native or switched to a SaaS platform in the last few years. Southern Air's team has been digitizing for nearly four decades, building each layer on the one before it. Claude didn't replace their system. It supercharged the system they'd already spent 20 years building.


## The Southern Air Framework


1. Build the interface your people actually use. Enterprise platforms handle ERP and HR, but the daily workflow layer should mirror how your team already works — connected through strong APIs, not forced into a vendor's UI.
2. Maintain a single source of truth. Every module references existing data rather than recreating it. One record, one update. The moment you have two copies of a last name, your database is headed for corruption.
3. Use AI to close the language gap between IT and operations. Upload your codebase, write prompts in plain English, and let the model translate between the people who talk in records and fields and the people who talk in workflows and nameplates.
4. Automate how you already work before you try to change how you work. The software that sticks is the software that mirrors existing processes, not the one that forces new ones.
5. Invest in asset data. Knowing every piece of equipment in every building — model, serial, tag, five-year spend — turns a service call into a consultative relationship.
6. Accept that the digital arc is long. Southern Air's system took a team 20 years to build. Claude supercharged the latest chapter, but the foundation was decades of disciplined data architecture.


## The Bottom Line


Paul Denham came to Southern Air 37 years ago as a mechanical engineer and helped shape the direction, but the team built it. They didn't replace their enterprise platforms — they layered a custom interface on top of Viewpoint and Acoro through strong APIs, all organized around a single founding rule: if the data exists somewhere, you reference it, you never recreate it. That discipline, and the team that enforced it for 20 years, is why the system works at a thousand-employee scale.


The throughline is the single source of truth. It drove the team to build their own interface layer. It shaped every module they added over 20 years. And when Claude arrived, it didn't replace the architecture — it supercharged it, closing the language gap between IT and operations so the software finally gets built right the first time. In a series full of operators picking tools off the shelf, Southern Air is the team that said: we'll build it ourselves, thanks.


## Ready to Automate Your Home Services Business?


Whether you build your own systems or buy from a vendor, the goal is the same: software that runs the way your business actually works, not the other way around. Robby helps HVAC, plumbing, and electrical companies automate the sales and back-office work that drags on revenue — AI-powered proposals, automated documentation, load calculations, follow-up, and clean integrations with ServiceTitan, HouseCall Pro, and the rest of the stack. We fit your workflow, not the reverse.


Book a free demo and we'll walk through your specific workflow and show you where the biggest wins are.


ℹ️


Home Services Leaders in AI


This article is part of our video series interviewing home-services business owners and operators who are leading the charge with AI and automation. Follow along on our blog and LinkedIn for new interviews each month. Got a story to share or know someone we should talk to? We'd love to hear from you.
