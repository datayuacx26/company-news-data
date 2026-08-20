---
schema_version: "1.0.0"
document_id: "694480147c6724ec5534a14b3bb0269267a4e625580a3ff0eb4e7ea39e302dc8"
company_key: "yc-chatwoot"
company: "Chatwoot"
source_id: "yc-chatwoot-news-import-2810aa3d6c74"
canonical_url: "https://www.chatwoot.com/blog/its-a-bot-story"
published_at: "2020-04-19T00:00:00+00:00"
first_seen_at: "2026-07-21T13:15:56.789660+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:7fab79d0a8760b2508d5ead0413093522b137b4220ce35d6182d0e2edf25b29b"
---

# It's a bot story

*Building conversational bots into Chatwoot.*


Since the initial development days of Chatwoot, we have constantly asked ourselves how the platform should handle chatbots. Right from building a complete end to end solution for chatbots to just leaving it entirely open to the developers, with Chatwoot being the API platform alone, we have thought through numerous ideas over this.


Amid this, after open sourcing the project, we also saw the same question being increasingly posted to us by the community as well. To enter a space of clarity here, over the easter weekend, we decided to discuss the idea of chatbots in our product all-hands meeting. Here's what came out of it -


## Our vision


We have always believed in chatbots being crucial in customer support - our faith only reinforced by a lot of companies releasing metrics about how bots reduced more than 30% of their customer queries without the need for an agent intervention. Back in 2017 however, in the early days of Chatwoot, most of the bots available were very dumb. The state of the art implementations then relied on state machines and on an NLP platform like wit.ai. Nobody had it really figured out on how to go about chatbots. While the bots were wonderful for the 30% of time when it worked, when it didn’t, they proved to be nothing less than a nightmare for customers.


Our thoughts on the solution we wanted to build back then was a customer support platform where bots and agents could work together seamlessly. Essentially, instead of bots vs agents, both units co-existing peacefully and working in tandem to solve a query. Consequently, in the year 2017, since we didn’t have enough engineering capacity, we went ahead and built the agent platform, where most of our expertise lay. The idea was to bring in the bot platform later, which happened to not kickstart at that time. We believed back then that the world was still years away from having that one super AI that could solve it all and looking back at it now, the vision still seems to hold true.


## Bots in Chatwoot


Compared to 2016, chatbots have evolved tremendously from being conditionally scripted to ones that can retain context and sentiments across a whole conversation. We also have multiple closed source and open source tools with the likes of Dialogflow, Microsoft Bot Framework, Rasa, etc. available at our disposition to build them. Although the super smart customer support bot still seems to be a far away dream, most customer support platforms in the market like Intercom, Freshchat, and Drift, to name a few, offer some form of chatbot implementation in their suite. Along the same lines, in the pursuit of building a complete customer support solution, our team as well is working towards having similar bot capabilities built into Chatwoot.


Staying true to our product philosophy, our aim, as always, is to give our users utmost freedom in terms of their choice of options when it comes to bots. As opposed to trying to launch this as a mammoth feature, we will be addressing this in smaller iterations over our next few releases and will be listening to all your feedback. Besides that, we are also eager to have vendors developing their own solutions on top of Chatwoot. Ideally, we would love Chatwoot to remain as the best option for the next chatbot startup, over which they are able to build a solution without having to worry about developing a chat interface or conversation management capabilities.


With that, as our first iteration over chatbots, Chatwoot 1.3.0 was released earlier this month constituting,` Agent Bot APIs` .


## Agent Bot APIs?


> Agent Bot APIs will let you easily integrate any Bot platform into Chatwoot.


Agent Bots, in essence, are virtual agents that you could add to your Chatwoot inbox along with your support agents. In any agent bot connected inbox, the bot will first receive the message from the customer before it appears to the agents in their inbox. The bot decides whether it has the confidence to continue the conversation and tries to resolve it. In case it can’t, it then routes the conversation back to an agent and continues to provide assistance to agents while they try to resolve the conversation.


The` 1.3.0` release supplies necessary APIs for the above functionality to work, over which developers can have their solutions. This is particularly focused at vendors and enthusiasts who don’t mind getting their hands dirty. We also hope to get the agent bots platform turn into a marketplace where developers can build custom bot implementations quickly. In our upcoming releases, we look to continue our work over Agent Bots with further integrations and UI options.


## Implementing an Agent Bot in Chatwoot with Rasa


Among all the solutions we have explored for building conversational AI into Chatwoot, the one that stands at the top of our list is Rasa. We have been closely following them since their inception, also since their story is extremely relatable to us. Furthermore, being a completely open source solution, they would be our top recommendation, if you are to ever build a chatbot of your own.


Soon after the release of Agent Bot APIs, our team tried to hack together an integration with Rasa platform. Here's a screencast to run you through the process-


You can explore the demo project by following this[link](https://github.com/Chatwoot/rasa-agent-bot-demo?ref=www-internal-blog.chatwoot.com) . We hope to see the exciting ideas you will come up by building on top of the Chatwoot[Agent Bot APIs](https://www.chatwoot.com/developers/api/?ref=www-internal-blog.chatwoot.com#tag/AgentBot) . As always, do keep sharing your feedback and suggestions as we continue building our chatbot story 🤖.
