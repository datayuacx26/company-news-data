---
schema_version: "1.0.0"
document_id: "57e0ca51d8c55a91034e0e193396e1fce6eca6cf31df141053acbaedfd8b24d4"
company_key: "yc-zeplin"
company: "Zeplin"
source_id: "yc-zeplin-news-import-336c2f98fc66"
canonical_url: "https://blog.zeplin.io/collaboration/product-team-collaboration-in-Zeplin/"
published_at: "2024-08-01T15:28:30.139+00:00"
first_seen_at: "2026-07-24T09:13:12.817491+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:c62274461fafca659f8368b50baa00f48dfa0032ab3c169cc881ba4aac304f68"
---

# Product team collaboration in Zeplin: integrating with Jira, DevOps, Slack, and Storybook

Product development is a messy process. It's a chaotic dance of constraints and compromises to manage user expectations, budget limitations, and deadlines. I've seen brilliant ideas wither because timing wasn't right for the team, and watched scrappy prototypes blossom into launch headliners when people clicked.


Despite significant recent advances in tooling, many teams are still struggling to bridge the gap between a designer's vision and a developer's reality. At Zeplin, we call this transition "design delivery" — and it's something we think about everyday (we've even[codified a set of principles for it](https://zeplin.io/principles-of-design-delivery/) ).


Here's what we've learned:


It's not about forcing everyone into one mega-platform. It's about creating conversations between the tools we already use and love. In fact, at Zeplin we believe that every functional team should use tooling that empowers them to be productive and effective in their current working environment.


We've built[integrations](https://zeplin.io/integrations/) at Zeplin around this exact idea of a “connected workflow” for the entire product team. With Zeplin:


- All contributors have an equal space to access the information they need, anytime
- Designers and devs can enjoy their specialized tools for what they’re meant for
- Conversations and decisions don’t get lost in silos
- There’s less confusion, assumptions, and “tool training” to slow your team down


Here’s an overview of Zeplin’s most popular integrations that keep product teams aligned: Jira, Azure DevOps, Slack, Teams, and Storybook.


## Zeplin and Jira


The Zeplin and Jira integration is one of our most powerful ones and it’s able to do everything you’d hope for when it comes to automatically syncing information between Jira issues and the design screens that relate to them. Here’s how it works:


### When you’re in Zeplin


You can attach screens, sections, or projects in Zeplin to a Jira issue. And once you do, you can access the Jira issue directly from within Zeplin, benefiting you with:


**Real-time updates**


In Zeplin, see Jira issues that have been linked to a design and view any live changes made to its status, assignee, priority, due date details without having to log into Jira. You can also click the link to open directly in Jira.


**The ability to filter designs by Jira ticket**


You can then also filter screens or sections in Zeplin based on the Jira issues they’re attached to, allowing you to find the specific design you need to work on right away.


### When you’re in Jira


When you select “Attach resource from Zeplin” in Jira, you’ll be presented with your most recent screens, sections, or projects from Zeplin. This lets you quickly attach the screen you just exported to Zeplin.


Clicking an attached screen in Jira will display it in full resolution. And if you need to head to Zeplin for details, there are also handy links to open them in Zeplin’s web or desktop apps.


🔗[Install Zeplin for Jira from the Atlassian Marketplace](https://marketplace.atlassian.com/apps/1221529/zeplin-for-jira?hosting=cloud&tab=overview)


## Zeplin and Azure DevOps


The Zeplin and Azure DevOps (or DevOps) integration is another powerful alternative for dev teams who track issues in a Microsoft toolstack instead. With this integration, you can link Zeplin screens or sections to a work item to see a preview of the design right in DevOps. And when the design is updated in Zeplin, it’ll automatically refresh in DevOps, too.


This helps you stay focused and not waste time copy/pasting screenshots or searching for the right files.


[🔗 Install the plugin from Azure’s marketplace](https://marketplace.visualstudio.com/items?itemName=zeplin.zeplin-integration)


## Zeplin and Slack


The Zeplin and Slack integration is a must-see (and must-have!) if your team communicates in Slack.


When you enable the Slack integration in any project, Zeplin will automatically send updates about project activity to your channel(s) of choice — such as newly added screens, new commit messages, new comments or annotations, styleguide changes, etc. You can customize the types of updates you want to receive.


This is a great way for your developers, PMs, and anyone else interested in your designs’ progress — to be automatically notified of any changes.


The Slack update itself will include the person who made the update, the name of the resource that’s been updated, a preview, the commit message (if included), and direct links to open it directly from Slack.


[🔗](https://marketplace.visualstudio.com/items?itemName=zeplin.zeplin-integration)[Learn more about using the Zeplin & Slack integration](https://zeplin.io/integrations/slack/)


## Zeplin and Microsoft Teams


We know not all teams use Slack. That’s why Zeplin’s Microsoft Teams integration offers the same benefits of automated and customizable updates on all project activity.


It’ll help cut down those meetings where the team discusses news that could have been an email – or in this case, an async, auto notification. You can imagine how much time your team will save from no longer having to field those constant ad hoc questions asking things like “hey, did you update thes design?” or “what was recently changed?”.


[🔗](https://marketplace.visualstudio.com/items?itemName=zeplin.zeplin-integration)[Learn more about using the Zeplin & Microsoft Teams integration](https://zeplin.io/integrations/ms-teams/)


> *With Zeplin, we’re in a much better spot with how transparent we are with the designs, the functional specifications, and how easily…we can make them come to life. We’ve got our designs in Zeplin connected to Microsoft Teams, so whenever a new design is posted or there are open comments to address, me and my engineering team will get notifications there, too.*


## Communicate Approval activity with Slack & Teams


A big part of product team alignment is managing stakeholder approvals, and Zeplin has a dedicated feature for that that goes really well with the Slack and Teams integrations. With[Approvals in Zeplin](https://support.zeplin.io/en/articles/8179349-getting-started-with-approvals) , you can capture and track stakeholder approvals right from the designs themselves.


How it works: right from the design screen, you can tag the team member(s) you need approval from, make a change request, and track its status.


And if you have the Slack or Teams integration set up, you can enable automatic notifications for anyone on the team who wants to stay in the loop on Approval activity.


[🔗](https://marketplace.visualstudio.com/items?itemName=zeplin.zeplin-integration)[Learn more about Approvals in Zeplin](https://support.zeplin.io/en/collections/5550850-screen-approvals)


> *“One of the things I really like about Zeplin is Approvals. We’ve tried out different tools for approvals and none of them have worked out because they’re just too complicated. Whereas in Zeplin, the designs are right there and you can ask: Is it approved? And if not approved, give me a comment. It’s super easy to use.*


## Zeplin and Storybook


Do you use Storybook to build and test UI components? Well then, you’ll love the Zeplin and Storybook integration.


With the Zeplin and Storybook integration, your developers can directly add design components from Zeplin into Storybook. By doing that, you open up the ability to compare the visual accuracy of design components in Zeplin with their code counterparts in Storybook, all in real time and in one place.


You’ll also be able to leverage Zeplin’s add-on for Storybook, where you can compare two components side-by-side inside Storybook: the live code shown in the left window and finalized design from Zeplin in the right window.


[🔗](https://marketplace.visualstudio.com/items?itemName=zeplin.zeplin-integration)[Learn more about using the Zeplin & Storybook integration](https://zeplin.io/integrations/storybook/)


If you’re ready to try it, head over to[GitHub](https://github.com/zeplin/storybook-zeplin) where you’ll find instructions to help you get started. Don’t forget to also check out the[Zeplin addon for Storybook](https://storybook.js.org/addons/storybook-zeplin)


## **Use Zeplin's tool integrations to enhance product team collaboration**


No two product development teams are the same. Rather than making cross-team collaboration harder than it has to be, you can use Zeplin to surface design information, share updates, and unite everyone’s work with these popular integrations,[plus more we didn’t cover](https://zeplin.io/integrations/) .


Product teams at Amazon, Little Caesars, The Washington Post — as well as thousands of top freelancers and agencies —[use Zeplin as the unified point of reference](https://zeplin.io/customers/) for design delivery and throughout product development.


If you’re curious how Zeplin might work for your team,[try for free anytime](https://app.zeplin.io/register) or see it in action at an[upcoming product deep dive](https://zeplin.io/events/deep-dive/) .
