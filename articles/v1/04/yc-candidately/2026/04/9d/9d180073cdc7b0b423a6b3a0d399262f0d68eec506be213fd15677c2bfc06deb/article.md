---
schema_version: "1.0.0"
document_id: "9d180073cdc7b0b423a6b3a0d399262f0d68eec506be213fd15677c2bfc06deb"
company_key: "yc-candidately"
company: "Candidately"
source_id: "yc-candidately-rss-05fc0b5de6e9"
canonical_url: "https://www.candidately.com/blog/what-recruiters-taught-us-about-design"
published_at: "2026-04-08T06:18:32+00:00"
first_seen_at: "2026-07-20T23:20:14.594006+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:9d8dec21243fe948fb51d9b4f2b7f7abd1910cc1dc30635ae0770b2eeee6fefb"
---

# What Recruiters Taught Us About Design

## TL;DR


After years of building software for staffing agencies, the biggest design lesson is this: recruiters don't need more features. They need fewer clicks, faster output, and tools that don't fight their workflow. Only 43% of recruiters rate their tech stack as 'good.' The bar is underground, and clearing it requires understanding how recruiting actually works.


I used to think I knew what good software looked like.


Clean layouts. Consistent spacing. The right type scale. I could spot a misaligned button from across the room. I had strong opinions about border radius.


Then we started building for recruiters.


And recruiters taught me that most of what I thought mattered didn't matter at all. Not because design doesn't count in recruiting. It counts more than in almost any B2B space I've worked in. But the things that actually matter are completely different from what a designer assumes sitting in a nice office with two monitors and a quiet Slack channel.


Here's what I got wrong, and what agency recruiters set straight.


## The Tuesday Morning Reality Check


Before we wrote a single line of code, we spent time watching recruiters work. Not interviewing them. Watching. There's a difference.


Here's what a typical Tuesday morning looks like for an agency[recruiter](https://www.candidately.com/glossary/recruiter)


.


Tab What's happening


Bullhorn Searching for a .NET developer a client asked about 20 minutes ago


LinkedIn Recruiter A saved search from yesterday that still needs review


Gmail 14 unread, two from candidates, one from a client marked "urgent"


Google Sheets Pipeline tracker the team shares because Bullhorn's reporting doesn't cut it


Microsoft Word A half-formatted resume that was interrupted by the last phone call


Phone (face-up on desk) Next candidate callback in four minutes


That's six contexts, and the phone hasn't even rung yet.


The research matches what we observed. Recruiters spend 80% of their time on administrative tasks and 20% on actual recruiting (Recruit[CRM](https://www.candidately.com/glossary/crm-recruitment)


). One study found that 17% of recruiters switch apps or tabs more than 100 times per day (Maayu). Not exaggerating. One hundred times.


I watched a recruiter find a great candidate in their database, get genuinely excited, and then spend the next 25 minutes copying and pasting that person's resume into a branded Word template. By the time the formatted version was ready to send, the energy was gone and two other tasks had piled up.


Their software was supposed to make this faster. Instead, it added more steps, more forms, more mandatory fields that nobody reads after they're filled in.


First lesson: if your tool adds time to someone's day, it's not a tool. It's a tax.


## The Resume Is the Product


This one took me longer to understand than it should have.


When I design a screen, I think about the interface. The layout, the interactions, the flow. A recruiter thinks about something completely different: the thing that leaves the building. The resume. The shortlist. The candidate profile that lands in a client's inbox.


Because that document IS the agency's product.


Not the relationship. Not the[sourcing](https://www.candidately.com/glossary/sourcing)


. Not the clever search string. The thing the client actually sees and judges. If it looks like a copy-paste job from Word with broken formatting and three different fonts, the agency looks sloppy. Even if the candidate is perfect.


> "I can find great people. That's not the hard part. The hard part is making them look great on paper in five minutes instead of forty-five."


What Number


Average time to manually format one resume 45 minutes


Loaded hourly cost of a recruiter ~$30


Cost per formatted resume $22.50


10-person team, 5 submittals each per week $1,125/week


Annual formatting cost for that team ~$58,500


That's almost $60,000 a year spent on what is essentially a copy-paste exercise.


When I first saw those numbers, my designer brain said: "We need a better template system." The recruiter's answer was simpler and smarter: "I don't want a template. I want the finished document. Just give me the output."


That sentence changed how I think about product design. Don't improve the workflow. Remove the step.


We ended up building a tool that takes any resume, in any format, and produces a branded, client-ready PDF in about 60 seconds. No Word. No copy-paste. No broken tables. The recruiter never touches the formatting because there's nothing to touch.


The lesson wasn't about resumes. It was about realizing that the most important thing you design might not be a screen at all. It might be the output.


## Nobody Reads the Manual


Here's a question I now ask about every feature we build: "Would a recruiter use this on their first day without anyone showing them how?"


If the answer is no, we redesign it.


Sounds obvious. It isn't. Most recruiting software takes months to learn. Reviews of major[ATS](https://www.candidately.com/glossary/applicant-tracking-system)


platforms describe "the sheer complexity" and how "it takes new consultants a long time to become masters." One Capterra reviewer called their implementation "a nightmare" and said "training was horrible and the trainer clearly was not trained herself."


Now consider this: the staffing industry has roughly 50% annual turnover among recruiters. Half of your user base is gone within a year. The other half is training the replacements. If your software needs a certification course, you've built a product that fights its own market.


Karri Saarinen, the designer behind Linear, said it well: "You shouldn't need a handbook to start using your software."


Compare two ways of searching a candidate database.


Boolean Natural language


What you type (title:"Senior Software Engineer" OR title:"Staff Engineer") AND (skills:"React") AND (location:"Chicago") NOT (title:"Junior") "Senior React developer in Chicago"


Time to build the query 15-30 minutes 5 seconds


Precision rate 23% (AIHR study) Significantly higher (understands intent)


Training required Weeks of practice None


What happens when you misspell something Zero results Still works


Twenty years of[Boolean search](https://www.candidately.com/glossary/boolean-search)


in recruiting, and the precision rate is 23%. Three quarters of the results are wrong.


[Natural language search](https://www.candidately.com/glossary/natural-language-search)


isn't a feature. It's a design correction for a broken interface pattern that the entire industry accepted as normal because "that's how it's always worked."


The lesson: if your users need training, the design needs work.


## Design for the Phone Call


Recruiting is an interrupted workflow. Always.


A recruiter starts reviewing a candidate profile. Their phone rings. A client wants an update on a different role. The recruiter switches context, pulls up that pipeline, gives the update, hangs up, and now they're staring at a screen with no memory of what they were doing 90 seconds ago.


Sound familiar? If you've ever lost your place in a spreadsheet after a meeting, multiply that by 40 interruptions per day. That's recruiting.


Nielsen Norman Group's research on interrupted workflows describes this problem precisely. Users return to a task and can't remember what they were doing, which steps they completed, or why they started in the first place. Their recommendation: supplement working memory with external memory. Let the system remember where you were.


Most recruiting tools don't do this. They're built around forms and workflows that assume linear, uninterrupted use. Fill out step one, proceed to step two, complete step three. Nice in theory. Completely disconnected from a job where the phone is part of the desk.


Smashing Magazine published a piece on designing for stress that introduced a concept I think about constantly: cognitive tunneling. When people are under pressure, their peripheral vision narrows, reading comprehension drops, and patience collapses. They can't parse a complex screen. They need the one thing they're looking for, and they need it immediately.


Recruiters live in this state. They're juggling 15 to 25 active requisitions simultaneously (Recruiterflow). They're switching between candidates, clients, and pipeline stages while a candidate waits on the other line.


So I stopped designing screens that try to show everything at once. Every view now answers one question: "What should I do right now?" Not a dashboard of charts. Not a wall of metrics. A list of actions, ordered by urgency.


The lesson: design for the three-second glance between phone calls, not the 30-minute deep work session that never happens.


## The Person Who Buys It Never Uses It


This one is the root cause. If I could explain every bad ATS interface with a single sentence, it would be this: the person who chose this software does not use it.


HR directors and procurement teams buy recruiting platforms. They evaluate based on feature checklists, compliance capabilities,[integration](https://www.candidately.com/glossary/integration)


counts, and vendor presentations. They ask: "Does it do X? Can it report on Y? Is it[SOC 2](https://www.candidately.com/glossary/soc-2)


compliant?"


Recruiters, the people who sit in front of the thing eight hours a day, have no say. They show up on Monday and there's a new system on their screen.


The result? ATS platforms optimized for the buyer's spreadsheet, not the recruiter's workflow. Every feature box is checked. The feature might be buried under three menus and require four clicks to reach, but it exists, so the checkbox gets ticked.


Only 43% of HR professionals rate their[talent acquisition](https://www.candidately.com/glossary/talent-acquisition)


tech stack as "good" or "excellent," according to HR.com's 2025-26 survey. That number dropped from 45% the year before. It's going backward.


Gareth Dunlop, who runs a UX consultancy called Fathom, put it perfectly: "Software replacement cycles are managed using a calendar rather than a stopwatch. If the enterprise finds itself with poor software, it can be years before it finds out about it and years before it can do anything about it."


This is why "works on top of your existing system" became our core design principle. Recruiters can't change their ATS. Their boss picked it. Their IT team configured it. Their workflows are built around it. But they can add a layer that makes the experience better without asking anyone's permission.


The lesson: when the buyer and the user are different people, build for the person in the chair.


## Your Data Is a Mess. That's Normal.


Designers love clean data. Perfectly filled-out forms. Consistent naming conventions. Complete records with every field populated.


Recruiting databases have none of that. And pretending otherwise is how you build software that fails on day one.


What you expect What you actually get


"Senior Software Developer" "sr. dev", "SDE II", "Senior Software Developer", "Snr Dev"


"JavaScript" "JS", "Javascript", "java script", "ECMAScript"


"New York, NY" "NYC", "New York", "Manhattan", "NY", "nueva york"


Current employment status A role they left three years ago


Complete skills list Whatever they felt like typing in 2019


One clean record per candidate Three duplicates with different email addresses


30% of B2B contact records go stale within 12 months (SignalHire). In recruiting, it's worse, because candidates change jobs, skills, and locations constantly. One study found that 70% of company data in recruiting databases becomes stale every year (Recruiterflow).


When we first built our search engine, we tested it on clean sample data. Worked great. Then we connected it to a real agency's database. The results were garbage.


Not because the search was broken. Because we'd designed for data that doesn't exist in the wild.


So we rebuilt everything around the mess. Semantic search instead of[keyword matching](https://www.candidately.com/glossary/keyword-matching)


. Because keywords only work when everyone spells things the same way. Nobody does. The system needs to understand that "React.js" and "ReactJS" and "React" are the same framework. That "Dallas" and "DFW" and "North Texas" describe the same area.


The lesson: don't design for how data should look. Design for how data actually looks.


## When Good Is Remarkable


Here's the most counterintuitive thing I've learned building for this industry.


In recruiting software, the bar is underground.


Most tools look like they were built in 2003. Because they were. One Bullhorn reviewer described the interface as "looking like it was designed in 2003." The Daily Hire called the pricing "steep" while noting it remains the industry standard anyway. People pay premium prices for software they know is bad, because switching costs are enormous and the alternatives aren't much better.


Karri Saarinen, who built Linear after designing at Airbnb, said something that stuck with me: "We started with quality. People actually noticed, because it's a rare approach."


That's exactly what happened with us. When agencies started sharing branded candidate portals instead of PDF email attachments, the content was the same. The candidates were the same. But the presentation changed, and clients responded differently. Faster feedback. More engagement. Agencies told us clients started saying things like "this looks really professional."


Not because we did anything groundbreaking. We just treated the client-facing experience like it mattered. Consistent spacing. Readable typography. A loading state that doesn't make you wonder if the page crashed.


In consumer software, that's baseline. In recruiting tech, it's genuinely remarkable. And that gap is an opportunity for anyone building in this space.


One agency started showing their candidate portal during new client pitches. Not as a feature demo. As proof that they're a modern, detail-oriented firm. A back-office tool became a sales asset because it looked like someone cared.


The lesson: when the industry standard is bad, good is all you need.


## What I'd Tell a Designer Starting in Recruiting


If you're a designer who just landed at a staffing tech company, or if you're a recruiter wondering why your tools feel like they were built by people who've never seen a recruiting desk, here's what I wish someone had told me.


**Watch before you wireframe.**


Sit next to a recruiter for a full day. Not a demo. Not an interview. Watch their screen. Count the tab switches. Time the resume formatting. Listen to the phone calls that interrupt everything. Your best design decisions will come from the chaos you observe, not the workflows you imagine.


**Design the output, not just the interface.**


The resume that gets emailed. The portal the client sees. The profile the candidate views. These are the real product. Your interface is just where they get assembled.


**Assume the data is broken.**


Whatever validation rules you want to add, whatever "required fields" you want to enforce, the database your users inherited has years of messy records that nobody will ever clean up. Your search needs to work despite that. Your displays need to handle missing fields gracefully. Your system needs to make sense of nonsense.


**Speed beats beauty.**


A recruiter will choose an ugly tool that's fast over a beautiful tool that's slow. Every time. If your page takes three seconds to load, they've already switched to the spreadsheet they swore they'd stop using.


**The person who bought this software is not the person sitting in front of it.**


Design for the person in the chair, not the person who signed the contract.


And maybe the most important one: this industry deserves better tools. Not fancier tools. Not tools with more features. Tools that respect how recruiting actually works, instead of how it looks on a product roadmap.


That's what recruiters taught me about design.


## Key Takeaways


- ✓


Recruiters spend 80% of their time on admin and 20% on actual recruiting. Their tools should flip that ratio, not reinforce it.
- ✓


The resume a recruiter sends to a client IS the product. If the output looks bad, the agency looks bad. Design doesn't stop at the interface.
- ✓


Boolean search has a 23% precision rate in practice. Replacing it with natural language isn't a feature — it's a design correction.
- ✓


Recruiting is an interrupted workflow. A recruiter mid-phone-call can't navigate a menu tree. Design for the three-second glance.
- ✓


The person who buys the software is never the person who uses it eight hours a day. That disconnect is the root of every bad ATS.


### Customer Stories


[NES Fircroft How NES Fircroft Modernized Candidate Submissions Across 45 Offices with Candidately NES Fircroft replaced manual email-based candidate submissions with Candidately across 45 offices and 100+ countries. The result: a streamlined, transparent recruitment process that both consultants and clients adopted within weeks.](https://www.candidately.com/customers/casestudies/nes-fircroft)[BenchmarkIT How BenchmarkIT Gained Visibility into Client Resume Reviews BenchmarkIT, a boutique IT consulting firm in Stamford CT, had no way to know if clients even opened their candidate resumes. Candidately gave them real-time tracking and faster feedback loops.](https://www.candidately.com/customers/casestudies/benchmarkit)


## Frequently asked questions


Why is most recruiting software so hard to use? +


−


Because the people who buy it and the people who use it are never the same. HR directors evaluate on feature checklists and compliance. Recruiters evaluate on speed and whether the tool gets out of their way. Those two lenses produce very different products. Add in long replacement cycles and the fact that recruiting software evolved from compliance systems, not productivity tools, and you get interfaces optimized for auditors, not the humans doing the actual recruiting.


What makes a good ATS interface? +


−


Speed, simplicity, and output quality. Every action should take under two seconds. The interface should be simple by default and powerful when needed, not everything on screen at once. And the documents recruiters share with clients should look professional without manual formatting. If a recruiter needs a certification course to use basic features, the design has failed.


How does software design affect recruiter productivity? +


−


Bad design costs staffing agencies roughly $77,000 per year in wasted recruiter time on data entry alone. Recruiters lose one full workday per week to administrative tasks their tools should handle. The 80/20 inversion, where 80% of time goes to admin and 20% to recruiting, is largely a design problem. Good software doesn't add features. It removes steps.


What should staffing agencies look for in recruiting software UX? +


−


Tools that don't require months of training, work with your existing systems rather than replacing them, and produce client-ready output without manual effort. Pay attention to how the tool handles messy data, because your database is messy. And look at the output, not just the interface.


Why do most ATS platforms look the same? +


−


They were all built around the same assumption: that recruiting is a data management problem. It's not. Recruiting is a speed and relationship problem that happens to generate data. When you design around data storage, you get forms and grids. When you design around what recruiters actually do, you get something very different.


## Ready to place candidates faster?


AI-powered candidate search, resume formatting, and client presentation - all on top of your existing ATS.


[Book a demo](https://www.candidately.com/book-a-demo)[See all features](https://www.candidately.com/features)
