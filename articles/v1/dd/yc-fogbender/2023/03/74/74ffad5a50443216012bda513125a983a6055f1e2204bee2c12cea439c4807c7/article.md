---
schema_version: "1.0.0"
document_id: "74ffad5a50443216012bda513125a983a6055f1e2204bee2c12cea439c4807c7"
company_key: "yc-fogbender"
company: "Fogbender"
source_id: "yc-fogbender-news-import-46cf0bf99c36"
canonical_url: "https://fogbender.com/blog/what-are-customer-triage-rooms"
published_at: "2023-03-05T00:00:00+00:00"
first_seen_at: "2026-07-21T20:41:04.212025+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:b78b0e0b1a937fe39b9809f898b3e2dee38742e0d8aeeeaf11c56d19f09ce06a"
---

# What are customer Triage rooms?

Every customer1 in Fogbender has a built-in room called Triage2. This is room is special, because it’s always there and cannot be closed or hidden. A Triage room is an entry point for all conversations between a particular customer’s users and the vendor’s customer-facing team.


The most important thing about the Triage room is that it is visible to all users associated with a customer. If a user posts a message in Triage, all other users associated with the same customer will see the message and will have the opportunity to respond or follow the conversation.


If you’re familiar with Slack Connect, a Triage room is very similar to a channel shared between two Slack workspaces in a vendor-customer relationship.


A conversation in Triage can result in one of two outcomes: either the question gets resolved on the spot, or the vendor’s team decides that further action is necessary.


Fogbender comes with several features that address the “further action” part, covering a range of use cases from trivial to complex.


## # Replying to an older message


Users often ask multiple questions at once, as shown in the following example.


To reduce ambiguity when answering, the agent uses the “reply” feature to respond to each message independently.


This results in what can be thought of as “extremely lightweight threads”, where the relationship between question and response is preserved, but no additional action is necessary to “expand” the conversation.


The conversation above is an example of a support exchange being resolved “in place”, without the need to open any tickets. This differs from traditional customer support workflows, where users typically initiate the creation of a ticket by submitting a form or sending an email. By using the Triage approach, several benefits can be gained.


First, it’s quite possible that Clint has a colleague who knows the answers to Clint’s questions—this colleague would have been notified of Clint’s questions via email, offering an opportunity to respond.


Second, the ability (though not a requirement) to have a real-time exchange would likely result in a faster, more accurate answer, while facilitating a personal connection—something that can be quite valuable in B2B vendor-customer relationships.


## # Creating a new room for the topic


In some cases, it makes sense to reserve a designated space for a discussion. For example, sometimes a customer support question may be too complex to be answered “in place”, requiring further analysis. Or, in case there are two discussions happening in Triage at the same time, it might make sense to continue one of them in its own context.


Below is an example of a conversation that resulted in the decision to create a dedicated space to continue discussing the topic. The agent can select the first and the last message in the conversation, thus determining its boundary.


From there, the agent brings up the “File issue” modal, names the conversation (or asks AI to help), and creates a new room.


This results a bi-directional “forward link” that can be used to quickly navigate to the new room, as well as to jump from the new room back to Triage.


From the customer’s side, there is now a new item in the sidebar, called “Share query via URL feature”. This is helpful, because any other user on Clint’s team can immediately tell which topics are currently active between Floating Dirigibles and Easel DB.


If you’re familiar with Slack threads or Microsoft Teams conversations, you can think of this sub-room as a “named thread” or a “named conversation”—with one other key distinction: it has the ability to create its own sub-rooms.


## # Creating a new room for the topic and linking it to a ticket in an external issue tracker


Efficient communication between the customer-facing team and the product team is a key goal for most organizations. However, a recurring difficulty lies in translating customer feedback into specific, actionable tasks that can be effectively tracked and addressed by developers, product managers, and quality engineers.


Fogbender provides a set of features that can be used to associate customer-facing rooms with tickets in a developer-facing issue tracker, such as Jira, GitHub, GitLab, or Linear. We will expand on of this capability in another post; meanwhile, let’s associate the room above with a new ticket in GitHub:


---


1 A “customer” in Fogbender is the second “B” of “B2B”: an organization, company, or account—never an individual.


2 “Triage” is a term commonly used in medical contexts to refer to the process of determining the priority of patients’ treatments based on the severity of their condition. It involves quickly assessing the nature and severity of injuries or illnesses to determine which patients need immediate attention and which can wait. The term is also used more broadly to refer to any process of prioritizing tasks or issues based on their level of urgency or importance.


---


### # Further reading:


To learn about all the different ways users and customers (teams of users) can be imported into your Fogbender workspace, take a look at the article called[End-user management](https://fogbender.com/blog/fogbender-user-management) .
