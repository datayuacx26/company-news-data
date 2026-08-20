---
schema_version: "1.0.0"
document_id: "79c8d6996df21eb2115016927bd5a69b0331bb88e9375b4b24d8f9d297a00677"
company_key: "yc-pibit-ai"
company: "Pibit.ai"
source_id: "yc-pibit-ai-news-import-b491cbbe82b1"
canonical_url: "https://pibit.ai/blog/open-knowledge-format-underwriting-submissions"
published_at: "2026-06-28T00:00:00+00:00"
first_seen_at: "2026-07-22T08:59:11.961361+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:583b0dd9c30220a991ec5cd7f22e9a52e94f24c0f4ca33a29b30db3030b16577"
---

# Open Knowledge Format: a new lens on underwriting submissions

On June 12, Google Cloud published a specification called the Open Knowledge Format, and most of insurance scrolled past it. It reads like a release for data platform teams, the sort of announcement that never reaches an underwriting floor. I read it twice, and both times I was thinking about submissions. I have watched enough standards arrive with noise and leave without a trace to hold a fresh one loosely. What kept my attention was not the spec itself but the way it describes a problem I have lived for years, and I think it is a useful lens for anyone who handles submissions, whether or not the format goes anywhere.


## Inside Google's Open Knowledge Format


**OKF v0.1** represents knowledge as a directory of markdown files with YAML frontmatter, with a small set of agreed-upon conventions that let wikis written by different producers be consumed by different agents without translation.


The[Open Knowledge Format](https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing) is an open way to write down what an organization knows so that people and software read it the same way. Instead of scattering that knowledge across shared drives and a few senior people's heads, you put each concept in its own plain file with a small block of structured metadata, then link the files that relate. Those links turn a pile of separate files into a graph that software moves through, rather than a folder it re-reads from the top each time.


Google calls this a format rather than a platform. There is no runtime to install and no database to buy. The spec is a few weeks old, it sits at version 0.1, and nobody has built anything load bearing on it yet. None of that settles whether the idea underneath it is right, which is the part worth weighing.


A submission is scattered knowledge too, arriving as a stack of documents instead of a wiki, and the problem underneath is the one Google describes.


## Inside a submission today


Picture what lands when a broker sends in a risk: an ACORD application, a statement of values, several years of loss runs, a thread of broker emails, and a couple of supplemental forms behind all of it. These files sit in one folder and share nothing. Each time an underwriter opens the account, they start reading from the first page again. Extraction tools made that reading faster over the last decade, and it helped, but they left the shape of what an underwriter is handed untouched. You still get fields lifted out of flat files, and someone still has to fit those fields back into one coherent picture on each pass, for each account.


In the[Accenture and The Institutes P&C underwriting survey](https://insuranceblog.accenture.com/why-underwriters-dont-underwrite-much) , commercial underwriters spend close to 40% of their time on tasks outside core underwriting, with redundant data entry and disconnected systems as the top causes. Accenture has described the work as a paper-first process, with the data underwriters need trapped in PDFs and spreadsheets attached to broker emails. Most carriers still run their submission intake exactly this way.


‍


## The submission as a connected object


‍


‍


Borrow the format's shape for a moment and the submission changes. The named insured connects to each of its locations. Each location carries its own loss history and its own exposures. The losses point back to the conditions that produced them. The total insured value on the application ties to the figure on the statement of values and to the number a broker quoted in an email, so when those numbers disagree, you catch it instead of missing a gap buried three documents deep. You stop rebuilding those connections by hand on each review, because they live in the structure.


I call that a structured risk object: the whole account held as one connected thing, with the pages already fitted together.


I have sat with an underwriter who found, twenty minutes into a review, a total insured value on the second page of an application that did not match the SOV, when a connected file would have shown the mismatch in the first thirty seconds. The information was all there, but nothing connected it, so the underwriter became the connection.


Underwriting judgment happens in how the pages relate to one another, and a folder leaves that out. An underwriter needs to know whether the loss history lines up with the exposure being priced, and whether the insured values still agree across the application and the SOV. A connected structure puts those questions up front, instead of leaving the underwriter to reassemble the account from a folder each time.


## The format decision inside a carrier


‍


For a CIO or a head of underwriting technology, the word to notice in Google's post is format. A format is an agreement about shape, and it does not force you to adopt one vendor's runtime to honor it. Inside a carrier, that difference shows up as cost. Most underwriting data strategies still let each application read the submission on its own terms, so a new tool builds its own version of the account, and the versions disagree.


Represent the submission as one connected object and your rating engine, your policy administration system, and your AI agents read the same account the same way. You stop paying the tax of reconciling one tool's version of the risk against the next. That makes the account shared infrastructure, and the systems you build on it stop re-solving the same problem.


## The format's limits


The Open Knowledge Format stops short of the part underwriting needs most. Version 0.1 standardizes the shape of knowledge and almost none of its meaning. It settles how files are arranged and how they link, and leaves what those links mean to whoever builds on top. For a general knowledge base, that is a reasonable place to begin. For a regulated underwriting decision, structure is half of what you need, and the other half is provenance.


## What this looks like built for underwriting


None of this idea is new to us at Pibit.AI, and we did not wait for a spec to chase it. We have spent years building this shape for underwriting, and the format simply puts a public name on the general version of it. The CURE™ platform holds each submission as a connected risk object, with every field validated before it reaches the underwriter, so the account they open is already assembled the way they read risk. Clearance runs against that object the moment it lands, and appetite rules score it as coded logic instead of a checklist in someone's memory. The same structured account then routes to the right carrier without a second round of data entry. Kinetic runs this in production today, matching a single structured submission against several carrier partners' coded appetites and sending each risk to the market that wants it.


Across the carriers and MGAs running it, underwriters spend their hours weighing risk instead of assembling it, and written premium per underwriter rises about 32%. That is the point of the concept rather than a feature list, the account arrives ready to be judged, so judgment is where the time goes.


## What changes when everything speaks to each other


This is the direction the connected version points, and I want to be careful about how far I push it. When the submission is one connected object, the same intel reaches clearance, the rating engine, and the underwriter at the same moment, and the underwriter decides on the full picture rather than the slice each system managed to read. The underwriter still owns the call, and the difference is that the call rests on an account that already agrees with itself. That is the theory, and parts of it are still theory.


The market is already paying for some of that difference. In[WTW's 2026 analytics survey](https://www.wtwco.com/en-us/news/2026/03/insurers-using-advanced-analytics-and-ai-report-strong-returns-on-investment-and-premium-growth) of 59 North American P&C insurers, the carriers furthest along with advanced analytics and AI posted combined ratios 6 percentage points lower and premium growth 3 points higher than slower adopters over 2022 to 2024. The same survey names what holds most carriers back: 42% point to data quality and integration as the barrier, and only 16% use AI to support underwriting today. So the edge is measurable and the adoption is thin at the same time, which is roughly where I would expect a real shift to sit this early.


One early example runs in production at[Hiscox](https://www.hiscoxgroup.com/news/press-releases/2024/12-08-24) , built on Google's Gemini, which reads a submission, prices the in-scope risk, and hands the underwriter a quote in minutes, with the underwriter keeping the final decision. Industry write-ups put the change at three days down to three minutes on that line. It is worth noting how narrow that start is, a single specialty line and renewals first, which is the sensible way to test something rather than proof that a whole book runs this way. The point that holds is the shape, a connected account the machine reasons over, with the expert still deciding.


## The missing layer: provenance


The spec points at the harder problem on its own. The Open Knowledge Format includes optional log files that record how a piece of knowledge changed over time, and some readers call that audit ready by default. For most organizations, that history is useful on its own.


For an underwriting decision that a reinsurer or a market conduct examiner will pull apart, a directory level change log does not come close. You need to prove where a single value came from, down to the source document and the spot on the page, which model read it, how confident that read was, what checks it passed, and who signed off, recorded once so any later change leaves a mark. That is field level provenance, and a defensible decision rests on it.


Structure lets a system reason across a whole account at once. On its own, structure proves nothing, which is where provenance comes in. I will make the full case for field level provenance in[the next post](https://pibit.ai/blog/field-level-provenance-ai-underwriting-audit-standard) , and show how it turns a connected account into a decision you can defend in an exam.


For years, teams treated the model as the hard part of AI in underwriting, and in my experience the model was the easy part. The hard part is handing the model an account that is already structured and connected and worth reasoning over, and then proving each value inside it. Whether the Open Knowledge Format becomes a standard or fades like plenty of specs before it, the way of thinking is worth borrowing now. I would not bet the year on the format, and I would spend real time on the shape it points to.
