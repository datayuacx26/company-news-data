---
schema_version: "1.0.0"
document_id: "91e30b379b286fd5ee7735f8d79f0e86f969319bc11ba1323c23ea52fcfada76"
company_key: "yc-maya-labs"
company: "Maya Labs"
source_id: "yc-maya-labs-rss-b1d58f5c6b85"
canonical_url: "https://blog.mayalabs.io/general-machine-intelligence/"
published_at: "2020-12-12T16:30:00+00:00"
first_seen_at: "2026-07-24T10:53:11.666154+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:1de8060099ca886bc158ea6d285f3c5e8d67bdfcc42601381dd12bf31b9a1097"
---

# A new approach to building general machine intelligence.

The promise of general machine intelligence lies in the answer to the question - what would happen if you shrunk the time taken to automate anything down from the *days* ****** it takes a human to program a machine, to the *seconds* it takes a machine to program itself? This sort of change in the rate of automation has happened just twice in history - with the steam engine and the industrial revolution post the 1800s, and centuries before that with the domestication of animals and the agricultural revolution. It's an interesting exercise to have a look at the exponential growth and prosperity such shifts brought to human civilisation.


The importance of such shifts is hard to overstate. For most of history, success involved stealing value from others, not creating it. Only in the last couple of centuries have machines (and domesticated animals) helped us automate our work and produce the surplus of goods and services that has made it possible to cooperate & succeed through trade rather than through wars & plunder. Most of what we take for granted today is a result of this - the new age of scientific discovery, art & literature, democracy, multilateral world trade, modern medicine, urban society and a drastic reduction in poverty, war and famine. As long as the cake keeps growing, people don't fight each other to death over a tiny slice of it.


The size of the world economy 'cake' doubled approximately every 200,000 years during the hunter-gatherer age, every 900 years during the agricultural age, and doubles every 6 years today in the current industrial age. A way out of 21st century global stagnation seems unlikely with the same 20th century mixture of farming and industrial growth modes. Globalisation of existing technology seems to have reached it's limits, and the geopolitical fighting over tinier slices of cake worldwide seems to have heightened as a result, only accelerated by the 2020 pandemic. The only way out is to build new technology that could trigger another exponential shift on the lines of the Agricultural or Industrial revolution. Even if it took decades to get there and the odds don't look great, perhaps now is the time to start trying. Nothing like a pandemic-induced recession tide to lift all the boats in the automation sea.


Nick Bostrom puts it well in his excellent book on the topic, *Superintelligence* :


> "If another such a transition to a different growth mode were to occur, and it were of the similar magnitude to the previous two, it would result in a new growth regime in which the world economy would double in size every two weeks."


Such a growth rate rightly seems ridiculous today, but it would be equally preposterous to suggest to an observer living in the agricultural era that the world economy would double several times within a lifespan. Yet that is the extraordinary condition we now take to be ordinary.


Machines today can compute at 2 Ghz (seven orders of magnitude faster than the current biological limit of 200Hz), communicate & collaborate instantly, extend indefinitely without limits, store memories without forgetting, and replicate without the compulsory nine month incubation. It is hard to imagine a scenario where the world economy doubles every few weeks that does not involve the creation of artificial minds that are faster and more efficient than the current biological kind.


Since machines don't directly compete with us for scarce resources (like food, water, sleep, land, money & social validation), artificial minds could enable this while coexisting with the biological ones. That said, it's a safe bet to build this in a way that doesn't concentrate it in the hands of just a few people who could use this to corner all resources. Non-profits are a bad way to do this. We need to figure out a way to distribute access to it in the same way Henry Ford democratised the power of gasoline engine by putting it in a car that anyone could afford.


### Current Approaches Don't Cut It


Current approaches don’t seem to have a way to get there yet. A good way to make sense of this is via another handy automobile analogy - the Waymo vs Tesla approach to making self-driving cars. The Waymo approach is to spend billions of dollars and many years making a prototype car in secret that can drive itself perfectly only on a few streets in Phoenix. The Tesla approach is to make a car that is useful from day one with partial self-driving, put it in the hands of users, and use the miles of sensor data collected from millions of cars to improve Tesla Autopilot for all users over time.


There seem to be a lot of Waymos in the global race to build general machine intelligence which regularly churn out top-class research, but we think only a Tesla-like approach will work sustainably long-term, and it doesn't exist yet. This approach should yank this quaint intellectual academic idea out of the sanitised AI labs and put it in the hands of the user, while framing it in terms of a business problem - so making the machine better at doing more kinds of things over time becomes the only way to serve its customers better.


The 1969 Moon landing and subsequent decline in space exploration shows that it is possible to build hard things without it being a business problem, but impossible to sustain them over the long-term. SpaceX shows that if you make cheaper access to space a business problem, then you can invent reusable rockets that could one day get you to Mars.


After a year and a half of tinkering on this problem, we think we have a notion of what a Tesla approach to general machine intelligence would look like. Here's our three step plan :


### 01. Create a powerful robotic assistant.


Build a virtual assistant that a human can rewire in a few minutes or hours to do any task - from quickly navigating websites and scheduling video calls to automating data entry and controlling robotic arms.


### 02. Distribute it to everyone on the planet.


Bring the power of this virtual assistant to every individual and business on this planet, to help them quickly build automation workflows to automate their tasks and improve their output by an order of magnitude.


### 03. Make it more general-purpose over time.


Till the point it can recursively self-improve, use the different types of automation workflows created in the process to manually train the virtual assistant to rewire itself in seconds to do more and more kinds of complex tasks for its users.


‌ ‌


## Introducing Maya v0.0.5


Maya is a virtual assistant that is the first realization of our three-step plan.


The same ~500k words in the English dictionary can be arranged into billions of sentences with different meanings. Similarly, the custom Maya language consists of a few of blocks that can be wired together to do anything - from navigating websites to controlling industrial IoT devices.


Today, you can drag-and-drop & manually build new Maya skill workflows in minutes, but we're working on making it partially or fully generate its own skills. Here's a (cherry-picked) sneak peak. Hint : for now, this uses GPT3-like transformers under the hood.‌ ‌


Transformers might not be the silver bullet to get there yet. A more suitable architecture in the future would handle world state and long-term memory better.


How long does it take to automate any complex, useful task you want a computer to do? Making complex python scripts takes hours/days, a drag-and-drop tool can reduce that to minutes, a general-purpose machine intelligence, when built, will reduce this time-to-automate to seconds. The business problem hence is - how do you keep reducing the time it takes for the user to automate a wider variety of complex tasks? This could use GPT-3-like transformers today, but the methods to achieve this could gradually evolve in the next 10 years.‌‌‌‌The market will reward whatever takes the shortest time to automate the widest range of tasks. Perhaps a thousand scrappy companies competing globally on this metric will drive the creation + widespread adoption of general machine intelligence.


The key insight here is that you can optimise modern machine learning to do one thing well, so it can play superhuman Starcraft or Chess, but for generality, we need to go further – optimise generality itself.


Executing on our three-step plan could result in a powerful virtual assistant that would exhibit increasingly more generality over time by enabling a virtuous cycle to get there. More types of tasks automated using the assistant could lead to more types of workflows for the assistant to learn from and generalise with which, resulting in more kinds of people finding the assistant useful, feeding into more kinds of tasks being automated - and so on.


In the beginning, Maya will let businesses and individuals make custom automations to speed up repetitive back-office tasks like data entry, scheduling calls, navigating websites or extracting information from PDFs. As it learns to do more and more kinds of tasks, it could move into adjacent roles - an online receptionist, omni-channel customer support assistant, or a product unit tester. Soon, it could be trained to do more cognitive tasks like listening to audio, looking at video, and operating a robotic arm to manipulate objects. Down the line, abilities for it to generate natural language conversations could be introduced - so it could be an always-on virtual employee/assistant you could talk with to get things done on Zoom on Slack.


It's possible to manually build Maya skills that do all these today, but it is truly exciting to think of what would happen if we could improve Maya overtime to reprogram itself by generating these skill flows to do these tasks on its own. The ultimate solution of the time-to-automate problem is when Maya can reach a point of recursive self-improvement i.e. become adept at the very task of improving itself.


## **Deterministic Optimism**


All versions of the future in popular imagination today are full of doom and gloom. The robots seem to inevitably take over the world. The astronauts seem to always get stuck in space. The social networks end up tricking all of us lemmings into jumping off the cliff. If you took any of the current sci-fi out there seriously, you'd think it better to quit all technology, retire to a muddy tropical island and spend the rest of your life rubbing flint stones. This fear psychosis seems to spare no one. Even billionaire futurists, unable to feel oppressed by mere humans, see a bit too much in "artificial intelligence" - as if to satisfy that innate human need to feel oppressed by a superior force.


Perhaps there is a different way to think about general machine intelligence - in terms of creating new things. For instance, it is possible today to build a billion dollar company with a few hundred people armed with laptops. What would happen if a single person with an idea could do it in their basement with a fleet of Maya assistants? How many different one-person billion dollar companies would be created every month? As the result, how fast would the world economy double - up from the current six-yearly rate? Every few months? Weeks? When the economy grows this rapidly,[how much](https://kardashev.fandom.com/wiki/Type_II?ref=blog.mayalabs.io) of the sun's energy would we capture? Which planets would we expand to? There is a gilded age of ideas on the other side, of a kind we've never seen before. This is an inspiring & exciting version of the future to wake up to.


There's still a long way to go, but we think we have one possible way to get there.


*We're a tiny, well-funded team of engineers looking for fellow problem-solvers who resonate with our mission. If this sounds exciting, have a look at our open[roles](https://www.mayahq.com/join?ref=blog.mayalabs.io) and drop us a mail at founders\[at\]mayalabs\[dot\]io.*
