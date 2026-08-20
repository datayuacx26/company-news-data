---
schema_version: "1.0.0"
document_id: "6eb0ce8084dab574a1a94882e15dbde8d5ab494a58f8a3e6f781f4c98f19a9eb"
company_key: "yc-fortuna-health"
company: "Fortuna Health"
source_id: "yc-fortuna-health-news-import-89ab9c75f976"
canonical_url: "https://www.fortunahealth.com/resources/the-best-person-to-build-the-tool-is-the-person-who-needs-it"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T01:59:38.382935+00:00"
fetched_at: "2026-08-12T01:59:41.026285+00:00"
content_hash: "sha256:60dcaed9b56d1776e19d851c4b9de6999099335ddb9489ccb0de93f58e357326"
---

# The Best Person to Build the Tool is the Person Who Needs It

Nikita, our CEO, used to start her morning with a coffee and open dozens of tabs of Medicaid news. She followed several blogs and figures on LinkedIn and Twitter who shared policy analysis. She even had Google Alerts firing to make sure she didn't miss a single breaking story on state budgets. It's a useful habit for someone who is obsessive about Medicaid and making mental models of how federal and local legislation relate to Fortuna.


It was also a huge manual commitment for someone who doesn't have an endless fount of time.


Now Nikita has a bot, running every weekday at 9 AM, that pulls from 142 sources and shares the top news in Slack. These sources include many state agency feeds and local newsrooms like North Carolina Health News and The Texas Tribune that previously someone would take hours to cull through. Nikita built - vibe coded - her replacement from scratch.


It would never have crossed Nikita's mind to ask someone on the engineering team to help her build a bot. A relatively low-priority ask does not earn dedicated engineering resources over shipping product.


Now, with vibe coding, we don't need to make those tradeoffs. The person who is closest to the problem is now the person who can build the fix.


Our COO, Cydney Kim, did a similar thing with contract management. She wanted one place to track every legal agreement Fortuna signed. She looked at external tools like Ironclad. They would work, but would miss certain specifics that were important to Fortuna, like extracting data obligations in each contract. These specifics are relevant to how we build, operate, and serve our customers. She built her own tool in an afternoon.


Neither Cydney nor Nikita has a technical background (though Cydney likes to remind us that she took one Java class in college).


The power of vibe coding is democratized velocity, giving every operator the ability to solve their own problems. But Fortuna is also a healthcare technology company with enterprise customers. We cannot take a "move fast and break things" mentality; we are not one-shotting a side project and trying to make it go viral on Twitter/X.


Fortuna is prioritizing and promoting vibe coding internally, but we're doing it in a measured way. Here are our principles on vibe coding and how non-technical team members are building with AI.


### Defining and scoping vibecoding


Andrej Karpathy's original sense of “vibe coding” meant building without much concern for how the code works. That's an accurate description of what happens here. Nobody on the operations or growth team is reviewing architecture or reading diffs line by line. They describe what they want and they ship.


This works because we're vibe coding to rapidly build non-core internal infrastructure: think prototyping/MVPs, internal tools, reporting dashboards. We narrow the boundary to work where we are largely the audience, or where being wrong is cheap and shows up fast.


Our product isn't on that list. Anything a consumer or a client touches gets built by engineers, with the architecture and security scrutiny that implies. Engineers are right to be skeptical of AI-generated code going in front of users, which is exactly why we keep this program away from that. What's left over is still a lot. A great deal of what a company needs doesn't require product or security judgment to build, and empowering the rest of the team to build those things is the whole point of this approach.


### Making a vibe coding proposal


Someone who wants to vibe code something writes up a project proposal. This includes a short paragraph and answering a list of questions on what the tool does, who uses it, what data it touches, and why its existence makes sense.


Ben, our CTO, reads it and either approves it or initiates a conversation with the submitter of the proposal. Approval means we get the person set up with Claude Code, access to our contained repo, and access to the internal deploy environment (which is configured to deploy to our walled-garden environment); then they go build. If it’s not approved, the proposal may need to be amended, or Ben may make the choice to delegate the build to our engineering team.


The write-up takes ten minutes and does two jobs. It kills bad ideas before anyone spends a weekend on them, and it forces the author to confront the question people skip: *what data does this touch?*


### The three things engineering looks for


Ben approves most of what reaches him, and the proposals that stall are usually reshaped rather than killed. He's weighing three things, and a project that runs into one of them either changes or moves to engineering.


**1. Security and data privacy:** We operate in healthcare, a regulated space. Some of our data is protected health information, and the rules governing it are indifferent to how the code was written. If a proposed tool touches PHI or member records, it stops being a vibe coding project and becomes an engineering project.


We don't enforce that by asking nicely. Here's Nikita shipping the news digest:


Note the constraints: reaching production necessitates a pull request that clears CI, security scanning included, just like a change to our production systems. That structure matters more when the person shipping isn't reading the code.


**2. Engineering and product considerations:** Does this project duplicate something we already have or are about to build? Will it create a second source of truth for data that's already modeled elsewhere? Is it going to end up in front of a customer, at which point it becomes a product and has to be built like one?


**3. Cost/value tradeoff:** Building got cheap, but maintenance did not. A tool that saves one person twenty minutes a week and needs monthly attention from an engineer is a net loss, and the loss stays invisible until someone takes additional time to tally it up. Whoever builds a tool also owns the responsibility of keeping it running, which is the cost incurred by skipping the queue of the engineering roadmap.


Underneath all three sits one heuristic that resolves most edge cases: **the more the engineering team needs to be involved, the more sense it makes for them to own the build.** A project that needs several rounds of review plus a schema change plus a production credential was never going to save any time in the first place. It's a normal engineering project wearing a costume. Handing it back is the efficient outcome for everybody.


### What people have built


**Medicaid daily digest / news tracker:** Nikita's, above.


**Contracts tracker:** Cydney's, above. Every signed agreement in one place, using AI to detect and track operational obligations. An external vendor gets you a generic contract database, and the things we need to stay on top of aren't generic.


**Internal task tracking:** Our own version of Monday or Asana, shaped around how we actually work.


**CRM:** Anji, Chief of Staff and Head of Business Development, vibe coded our CRM with Notion AI. Anji connected meeting transcripts and various Operations workspaces to produce contacts, organizations, and deals as linked databases, with pipeline stages that match how our deals actually move. Growth and Operations now work out of the same CRM which has streamlined handoffs between sales, contracting, and implementation steps.


**Client demos:** Our goal is to delight customers while ensuring no PHI or consumer data from our actual product is surfaced. Neel, on our growth team, vibe codes these. He cares about making the customization go deeper than showing client colors and logos: demos also include rules that apply in that client’s state, the client’s respective terminology, and actual providers that serve that client. In essence, reflecting fidelity to what their product would look like even if everything comes from public sources to avoid PHI. This deep configuration has proven more successful than the polish of a generic demo.


He's incorporated vibe coding with the rest of his growth work too, from SEO and AEO analysis to paid ads and website modifications.


#### A closer look: Faxify


Healthcare and government agencies still use faxes! Why? It's an accepted channel for sensitive documents like medical notes and paystubs under HIPAA, while unencrypted email, for example, is not.


A raw scan makes a terrible fax. Pages come in tilted on gray backgrounds, and the file sizes are often too large for a fax line to accept/receive. Sam, on our operations team, worked with Matt, one of our engineers, to build the converter that fixes all of it: upload a PDF of any size, get back a de-skewed, high-contrast file compressed to the fax standard, and send through on the first attempt.


For some applications, Sam has to work through hundreds of printed pages, and a batch that used to take a slow manual pass now takes a single upload.


Faxify processes in memory and does not store any data. It works like a photocopier: you hand it a document and it hands back a fax-ready version. A database is for information software has to retrieve later; however, Faxify does not need to retrieve anything since the documents are only ever passing through. Sam and Matt landed on vibe coding for exactly that reason, since a tool handling sensitive paperwork usually goes to engineering and this one had no data for anyone to worry about.


### What we're finding


Most of the industry conversation about AI and engineering is about whether engineers get replaced. For a startup our size, in health tech, we're finding something different: everyone at Fortuna gets to do more with vibe coding. Engineering can prioritize the most important product work because non-technical people are empowered to build.


With the right vibe coding guardrails and principles, Fortuna is moving faster.


‍
