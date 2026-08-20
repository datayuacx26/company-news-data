---
schema_version: "1.0.0"
document_id: "f5b3bdc3c9549bd8731a4dace5ff8bc3379be9b0fe150e97ca7921af822624f5"
company_key: "tripadvisor-inc-common-stock"
company: "TripAdvisor Inc."
source_id: "tripadvisor-inc-common-stock-rss-6295d6870799"
canonical_url: "https://medium.com/tripadvisor/taming-a-million-tickets-how-a-small-team-used-ai-to-modernize-an-overgrown-jira-instance-at-scale-ba92a34a77c2"
published_at: "2026-06-08T15:45:02+00:00"
first_seen_at: "2026-07-20T23:18:15.449539+00:00"
fetched_at: "2026-08-20T00:33:41.838119+00:00"
content_hash: "sha256:8941b147ffe9894dcfdd45c2537b9a400b0a534e50dd67c98de09157ee0bb250"
---

# Taming a Million Tickets: How a Small Team Used AI to Modernize an Overgrown Jira Instance at Scale

Image credit: Sid Iyer


*A tight deadline. A mountain of data. A tricky migration issue that tripped us up. Here’s how AI helped us pull it off.*


*By*[Sid Iyer](https://www.linkedin.com/in/siddharthpiyer) *, Senior Manager and*[Morley Tooke](https://www.linkedin.com/in/morley-tooke/) *, Information Architect*


### The Hard Nut to Crack


Over time, large, fast-moving organizations can often get bogged down by legacy processes, untended workflows, and technical debt. The tools your teams use, the workflows they follow, the data they generate…when not updated or maintained, can cause problems that compound over time. Solid process governance combined with proper tools should become institutional muscle memory. More often than not however, they become institutional debt.


At Tripadvisor, our Jira instance had arguably become the latter. Over a decade of organic growth had left us with 245 projects, well over a million tickets, and a configuration landscape that was, to put it charitably, eclectic. Every team had made reasonable decisions, but often, those were taken in isolated verticals. In aggregate, those decisions had produced something that was nearly impossible to measure, difficult to standardize, and expensive to change. Forget comparing apples to oranges; In our case, we had to compare apples to oranges to grapes to bananas to…you get the picture.


We had tried to standardize in the past, however nothing had stuck. The problem wasn’t effort or intent — it was scale. How do you get a clear picture of how an organization actually uses a tool across a decade of data without an army of analysts and months of runway? And how would you go about harmonizing fields on a ticket or states in a workflow, across an organization with 100+ projects and engineering teams spread across the globe?


The answer, it turned out, was to stop thinking like it was 2019.


When leadership asked us to move fast (and they meant it) our first instinct was to start from scratch. Blank slate, clean config, fresh start. We quickly talked ourselves out of it. Historical context in Jira is organizational memory. In the age of LLMs where context is a goldmine, writing that off felt like the wrong call. And let’s not even talk about the mutiny that would ensue if every team at Tripadvisor had to rebuild all of their processes, automations and workflows from the ground up…again.


So, the better question was: *given the scale, how could we use AI to help us migrate all of our teams to a standard process and configuration? And furthermore, could we be thorough, careful, compassionate, and quick when we did it?*


We decided to find out. And the first thing we needed was data.


### Intelligence Gathering at Scale


We started by asking questions we’d never had the bandwidth to ask before.


What projects were actually seeing activity? How were teams actually using Jira? Which fields were being populated consistently, and which fields were never used? How much variation existed across projects in terms of issue types, workflows, and field configurations? And critically, if we proposed removing certain fields, who would actually feel it? The sheer scale of the data made it less than ideal for us to visualize it via the out of the box reporting tools provided by Jira. We had to turn to AI.


*A basic histogram of our legacy field schema. Fields proposed for removal in red.*


We used AI to write a series of scripts that did what would have taken a team weeks to do manually. The novel approach we employed was to actually state the end goal to the LLM, and workshop a solution with it while clearly defining the boundary conditions (e.g. “ *the number of columns and rows will not fit in a spreadsheet so we have to selectively pull parts of the dataset for analysis”).* The resulting set of scripts formed the backbone of our operations for the coming weeks.


We pulled schema data across all Tripadvisor projects via the Jira API, including issue types, field configurations, workflow states, and a sample of 500 recent tickets per project to understand how fields were actually being used in practice. The output was a set of per-project schema reports, each detailing field usage and emptiness percentages.


Then we made it visual.


The resulting charts of project activity over time, field usage histograms, issue type counts per project, sample values per issue type and project and field metadata allowed us to have the clearest picture of Jira usage at Tripadvisor for the first time. We were quickly able to establish a cutline for what fields and projects we needed to take with us, and what could be cut away and left behind. The challenge now was, how do we measure the impact of rolling out these changes to the organization and bringing everyone on board?


*A project’s field schema visualized. Fields in orange are proposed for removal. Fields in red are proposed for removal but contain data and must be verified by each team. This spreadsheet was presented to each team as part of the migration survey.*


Here’s where it got interesting. We weren’t just building a report for ourselves now. We needed every team in the organization to be able to look at their own project and immediately understand what was changing and what wasn’t. So we took it further.


We went back to the LLM and used it to write scripts that helped us visualize the impact of the proposed field cuts, on every project. Underused fields that were always empty and slated for removal were flagged in orange. No impact, safe to cut. Fields that had some data, but were found to be extraneous and were slated for removal were flagged in red. Someone would feel this, and we needed to know about it before they did. Fields that were flagged in red, but that actually contained substantial data, were good candidates for custom fields in the new schema.


Each project got its own schema sheet, a snapshot of their configuration, their field usage, their emptiness percentages, all color-coded. A Google Apps Script then pulled every one of those CSVs from Google Drive and loaded them into a single index spreadsheet. One row per project. One link per schema. Click a project key, see exactly what’s changing for that team.


What we ended up with wasn’t just a report. It was an org-wide impact map, the kind of artifact that would previously require a dedicated team and weeks of work. We had it in hours. And when it came time to have conversations with individual teams about the migration, we weren’t asking them to trust our judgment. We were showing them their own data.


That changed the conversation entirely.


When we presented teams with the field data during the POC phase and later during the compulsory migration survey, people were genuinely surprised at the insights the data showed them. How they were actually *not* using a field they fought tooth and nail to introduce 5 years ago. How their teams were tracking *the same* data across multiple fields. How their workflows had slowly become a jumbled mess over time, and how nobody noticed because the changes were always incremental. We saw genuine excitement for the upcoming migration.


The philosophy throughout was simple: measure twice, cut once. Time spent in analysis is time saved in rollback. Once we were confident the impact was understood and, by our estimates, wouldn’t hamper business operations, we were ready to design the new Jira.


### Designing the New Jira


With our field schema analysis complete and our list of active and deactivated projects in hand, we were able to move forward with our barebones, simplified Jira approach.


Our goal was twofold:


- Drive consistency across the organization by prescribing a common set of ticket types, and a minimal set of fields, screens, and workflows. This would enable teams to generate better analytics data, while also creating the potential for more automation and better AI enablement.
- Create a simple, repeatable development process that teams could use to log and track their work. Some of our previous workflows were complex and had far too much overhead for what little benefit they provided.


We quickly settled on a minimum, standard set of work items that centered around the *initiative, epic, story, task, bug and incident* . Eliminating unnecessary custom work item types and providing guidance on when to use each standard work item type would help to ensure that teams logged work correctly.


We then designed a very simple, frictionless workflow that contained the states that we wanted to measure and the states that we felt were absolutely necessary for the majority of our development teams. We included a minimal level of transitional logic in order to limit some of the headaches of moving tickets from state to state. This would become our base workflow and one that we hoped would apply to as many teams as possible.


We knew that there would always be some workflow and field schema customization involved to account for teams with unique use cases. To handle this, we created a consistently named, tiered set of Jira configuration schemes that we could apply to teams with special requirements. If a team relied on a specific set of fields or specific workflow state — no problem, we’d simply create a new configuration tier based on the common scheme.


Not only did this kickstart our much-needed Jira configuration scheme cleanup, but it also gave us levers for deploying org-wide changes to the overall process. Changes could now be made in only a handful of verifiable places (rather than in some deep dark place in our Jira admin panel).


We were also mandated to incorporate two custom fields that would help improve our metrics and reporting, specifically when it came to velocity and resource allocation:


- To better determine velocity, we harmonized the usage of the Story Point field on all tickets across all of Engineering.
- To track spend, or rather to determine what our developers were working on from a cost perspective, we mandated that all tickets must belong to an epic, and that each epic must be associated with a cost center.


Our challenge was to provide enough leeway to our development teams without being overly prescriptive, and without adding too much complexity and overhead. To mitigate this, we created an AI-based story point estimator that assigns a story point value to a ticket in cases where a developer has neglected to do so. When a ticket with an empty story point field transitions to *In Progress* , the AI agent reads the ticket description and compares it to a pre-populated table of historical ticket data taken from projects across the organization. The agent then estimates a story point value for the new ticket based on the story point values in the historical set of tickets. It then updates the Story Point field, marks the ticket with a label and adds a comment providing justification for the estimate. It also provides instructions for the ticket assignee to verify the estimate. So far, it’s been very effective, the teams are onboard, and it’s helped to keep our velocity accurate and our burndown rate in line with expectations.


*A comment added to a Jira ticket by the story point estimator AI agent.*


To make sure that our initial decisions reflected the greater good and to catch as many edge cases as possible, we kicked off an initial POC with a small subset of our Engineering teams. We enlisted teams that had diverse requirements and we worked directly with them from initial project migration to day-to-day support. The feedback was overwhelmingly positive, which gave us a ton of confidence to continue with the rest of the migration.


Our analysis looked good, our users were happy, our workflow decisions had gone over well, and nothing had broken; now it was time to convince the rest of the organization.


### Preparing the Rest of the Organization


Communication is difficult, especially at scale.


We knew that we had a complex migration process ahead of us with some pretty severe risks. It would require plenty of bi-directional communication between our small migration team and all of our project stakeholders.


After formally announcing the migration timeline, the first task was identifying and communicating the list of inactive Jira projects to be archived along with the complete list of active projects set for migration.


We sent a carefully worded Slack message and email to all members of the Engineering organization that clearly communicated both the deadline and the list of projects along with their migration statuses. We repeated the same message to Jira project administrators and *power users* with extra emphasis on their responsibility as project leaders. Consistently-worded emails and Slack messages sent to the same, well-known announcement channel, with followups on a calculated basis: this would become a common pattern.


The next step was to approach teams with outlier workflows and unique field configuration schemes to figure out how best to accommodate them. We had performed a careful Jira configuration analysis early on to identify projects with complex, custom workflows. Our goal was not only to listen to their requirements, but also to sell them on the benefits of our new simplified approach. We called it our *project roadshow* , and we did our best impression of a flashy, high-end boat or timeshare salesperson in the hope of convincing each team of a better tomorrow.


It all went surprisingly well.


After a few nervous icebreakers, a pre-canned sales pitch or two, and a few thoughtful followup questions, the majority of our outlier teams were happy to move over and saw no real issues with the new approach. This was a big relief and a major validation of the design work that we had put in earlier.


For the few teams who were unsure or that voiced objections to our approach, we engaged in meaningful dialog and sometimes well-meaning negotiation. We wanted to provide each team the chance to justify carrying-forward their existing approach and any customization that would come along with it. Thankfully, we spared them our good cop/bad cop routine, and no harsh words were spoken during this phase of the project. Our approach was not iron-fisted conformity, but rather simple understanding and patience.


While this was going on, we sent a survey to teams that we deemed low and medium risk, where *risk* was ** determined almost entirely by the complexity or commonality of their current workflow configurations. We asked them to answer questions about specific aspects of their project and we provided them with the link to the field schema visualizer spreadsheet that we described earlier. Each survey responder was able to find their project’s schema in the spreadsheet and scroll through their fields to determine whether or not it was critical to their process. In a matter of minutes, they had the information they needed to determine the risk associated with migrating their project.


This was a game changer.


The last step was education. We needed to bring our users up to speed on the new workflow changes. This is when we decided to get creative. Rather than send another email or Slack message, we engaged with our talented Learning and Development team to create an engaging, *plumbing* -themed learning module based on a well-known, multi-billion dollar space opera franchise and pop-culture juggernaut. After months of sorting out our Jira plumbing issues, we felt that this theme was kind of… *fitting (that’s plumber humor)* . Our designer, Greg Wlasiuk, quickly created something that was both engaging and clever, but also lightweight and simple enough that it would not be a burden for our already busy staff.


Feedback was very positive.


*The learning module that was assigned to everybody in the Engineering organization, with moon in the background.*


To further emphasize the importance of estimating story points, we even distributed a graphic novel featuring a highly litigious, makeup and costume-adorned rock band from the 1970s and 80s. The comic, which was created entirely using AI, featured the leader of the band giving a spirited tutorial to the other members about how to properly estimate tasks using story points.


They’re silly, but they’re engaging and they get people talking.


Overall, we had a simple philosophy when it came to communication. We didn’t mandate Kanban or scrum, but instead reinforced the need to use story points. We did our best to always update the organization on the current state of the migration and on what was to come.


It can be a challenge to get a developer to read an email or a Slack message. Sometimes you need to bug them repeatedly with clearly worded communications, and other times it helps to be creative.


In the end, nobody was blindsided by the deadlines and almost all teams were overwhelmingly excited about the changes we were proposing.


On to the migration!


### The Bulk Move Problem — and the Debugging Story


We thought we’d have clear skies and smooth sailing from here on out. We didn’t.


Once the actual migration began, we hit a wall almost immediately. Jira’s admin UI has a hard cap of 1000 tickets for bulk operations. That sounds reasonable until you’re staring at projects with 40,000+ legacy tickets that need to be moved to new issue types. For those projects, we needed a different approach for anything above that threshold. And because issue type migration is a prerequisite for workflow migration, projects that couldn’t complete issue type moves were now blocked from workflow updates too.


What we had initially estimated as a few weeks of migration work suddenly looked like a few months. That’s the kind of recalculation that kills momentum on a project like this.


After a few helpful consultations, our Jira support rep had pointed us toward their bulk move API. This was a way around their known issue sticking point, but using it at this kind of scale still required tooling we didn’t yet have.


We went back to AI.


While on a group call, we described the problem to the LLM, defined our constraints, and within the span of a single meeting had the basis for a script that could call the bulk move endpoints in controlled batches. Four parameters: *batch size* , *page size* , *polling delay* , and *batch delay* . Simple on the surface, but configurable enough to handle projects of wildly different sizes without hammering the API or timing out. We had a path forward.


*The bulk move script (console output) that we used to migrate well over 100,000 legacy tickets.*


Then we hit a second wall. A smaller one, but stranger.


44 tickets in one of our projects kept failing, no matter what we tried. Every other ticket in that project migrated cleanly. These 44 refused. Jira’s error responses weren’t telling us much, and we didn’t have time to wait on a support thread or comb through forums.


So we did what had worked before. We went back to AI, described the problem, and asked it to write a script that would pull key fields from those 44 tickets, including summary, description, and metadata, and dump them to a file. The goal wasn’t automation. It was just to get a good look at the data.


We again wrote that script live on a call. It took about ten minutes.


And when we looked at the output, it was immediately obvious. Those tickets had massive descriptions. Walls of text, far beyond anything else in the dataset. We fed that observation back to the LLM and asked it to help us reason through what character limits we might be hitting. We landed on a range of 18,000 to 30,000 characters as the likely threshold.


Atlassian support was incredibly helpful throughout this process and helped us verify exactly what our own AI-assisted debugging was already telling us. The character limit was real, and now we had confirmation.


We had diagnosed the root cause in minutes rather than days. That’s the whole story in miniature, right there.


Finally, the ship was moving again.


### The Migration


Over three and a half days, we worked through the remaining projects systematically. Five steps per project, in order: field configuration, legacy work type migration, work type configuration, workflow, kanban board column updates to reflect new statuses, and finally, updated screen configurations to reflect the new field configurations.


Rinse and repeat.


We had expected friction. We got surprisingly little. Teams that we thought might push back on the new schema largely embraced it. The tiered configuration system ended up simpler than we’d anticipated because most teams were willing to migrate to our standard configuration without needing custom adjustments. That’s a testament to the analysis work upfront. When you show people their own data and walk them through exactly what’s changing and why, the resistance tends to dissolve.


The smoke test came almost immediately. Reporting dashboards that had previously shown incomplete or inconsistent data started returning meaningful signals within days of the migration completing. Story points populating. Epics being tagged. The infrastructure was working.


The real long term win though is what IT now has in their hands. The tiered configuration system means that any future SDLC change, any new guideline, any update to how the org tracks work, can be rolled out across all managed projects by pulling a single lever. No more project by project manual work. No more configuration drift. The new Jira is leaner, more consistent, and for the first time, actually scalable.


### The End Result


The numbers tell part of the story. 84 projects migrated. 22 left untouched due to critical business operations. 139 set to read-only, earmarked for archival by end of year. 245 projects accounted for, every one of them deliberately categorized.


But the more important outcome is what we’ve been able to achieve during this months-long exercise in simplification and streamlining. Clean, consistent data across every active engineering project at Tripadvisor. Story points on tickets. Every ticket belongs to an epic. The foundational signals that make DevEx metrics actually meaningful.


We didn’t just clean up Jira, we built the data infrastructure that makes it possible to measure and improve how we work.


Furthermore, no meaningful disruptions occurred and teams were able to step into the new workflow without missing a beat. We designed a simple and efficient Jira configuration, and we did our best to communicate timelines and educate our users on the upcoming process changes. Simple changes, transparent messaging, and the willingness to let them choose their own development approaches without being heavy-handed.


The org isn’t done. There’s more to build on top of this. But, the foundation is solid enough to build on.


Here’s the takeaway we’d leave anyone with who’s staring down a similar problem: You don’t need a team of analysts or a year-long program to effect a large-scale migration like this. You simply need the right questions and an AI that can help you get to the answers fast. The project that had stumped the org for years was completed in a quarter, not because AI did the work, but because AI made it possible to do the large-scale analysis that proved its feasibility.


We could finally ask the questions that were effectively off limits before, not because they weren’t worth asking, but because the cost of getting to the answers was prohibitive. That’s the shift.


AI didn’t replace the thinking. It removed the tax on asking.


*Big shoutouts to*[Hector Sanchez](https://www.linkedin.com/in/hector-luis-sanchez-6b224295/) *and*[Olivia Malcolmson](https://www.linkedin.com/in/oliviamalcolmson/) *for helping us make sense of our Jira configurations.*


---


[Taming a Million Tickets: How a Small Team Used AI to Modernize an Overgrown Jira Instance at Scale](https://medium.com/tripadvisor/taming-a-million-tickets-how-a-small-team-used-ai-to-modernize-an-overgrown-jira-instance-at-scale-ba92a34a77c2) was originally published in[Tripadvisor Tech](https://medium.com/tripadvisor) on Medium, where people are continuing the conversation by highlighting and responding to this story.
