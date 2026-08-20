---
schema_version: "1.0.0"
document_id: "e90b0477cab961e25c426c49a9b68ba5c29cb5b668a9511d31c054512ade49fc"
company_key: "yc-nextmv"
company: "Nextmv"
source_id: "yc-nextmv-news-import-d2226782a65f"
canonical_url: "https://www.nextmv.io/blog/in-conversation-project-success-and-failure-in-decision-science"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-22T06:03:40.021254+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:5d2aa9787b43f683a17ab1cdf9fa2849b267f1cee16f6219a916dbe14d470531"
---

# In conversation: Project success and failure in decision science

Nextmv co-founders, Carolyn Mooney and Ryan O’Neil, hosted a conversation with industry titans[Doug Gray](https://www.nextmv.io/author/doug-gray) , Director of Data Science at Walmart, and[Karla Hoffman](https://www.nextmv.io/author/karla-hoffman) , Professor Emerita at George Mason University and former INFORMS President, to recount success stories and lessons learned from project failure. Drawing from their experience, they also discuss the role of testing and prototyping and offer advice for the next generation of decision intelligence practitioners.


*The following is a companion interview to*[a longer, related conversation](https://www.nextmv.io/videos/lessons-lived-and-learned-project-success-and-failure-in-decision-science) *that has been edited for length and clarity. ‍* **Carolyn Mooney** : Doug, I was reading your book,[Why Data Science Projects Fail](https://www.routledge.com/Why-Data-Science-Projects-Fail-The-Harsh-Realities-of-Implementing-AI-and-Analytics-without-the-Hype/Gray-Shellshear/p/book/9781032660301) , and you wrote a little about what happens if you hand over a black box solution. Can you expand on that, or talk about one of those failure modes for decision and data science projects?


**Doug Gray** : It's a rare occasion that you get to fail on a project and then 20 years later take on the same project again and succeed, so I have my success and my failure together.


It was very early in my career, and I thought that you looked at a problem, you wrote down the math, you coded it, and you handed it to the customer. But that didn't work. The domain was at American Airlines and their Irregular Operations (IROPS) recovery. Everybody has been stranded due to weather or when the whole airline has gone kaput, for a variety of reasons. Airlines have to put those schedules back together in real-time like a puzzle: the aircraft, the passengers, the crew, the cargo, etc. You have to abide by a variety of rules such as, you can't land at Orange County after 10:00pm, or take off due to noise abatement. There's all these rules about how airlines and airports operate. We literally wrote down the math, and our customer had no interest in anything that we were doing. The data was scattered everywhere from mainframes to PCs to charts on the wall. It wasn't very conducive to building a model, testing it, or validating it because the users were convinced there was no way we were ever going to be able to get a computer to solve this very, very complex problem. And we failed. We worked on it for six months and we never got any traction, and we went back several times trying to get different stakeholders to engage and work with us. It taught me how important business domain expert involvement and engagement is—we call them champions, nothing ever gets done without a champion. The math, data, and technology, as important as that is, it's really quite secondary to that whole process of change management, where you're completely revolutionizing the way people do things. It's intimidating, it's scary. It was definitely an early career failure on my part.


**Carolyn:** Ryan, you and I saw the same thing at some of our prior work, especially when we were at Zoomer. I think one of the coolest things that we had in that experience was sitting next to operators.


**Ryan O'Neil** : Yes, we would get a lot of direct feedback, but there was nothing like being right next to the dispatcher when they were looking at routes and saying, "Oh, this is why I don't like this one." It was feedback that you’d never get through any other channel.


**Doug** : Absolutely. Now fast forward to 2008 at Southwest Airlines. They had started working with a gentleman named[Dr. Mark Gao Song](https://www.informs.org/News-Room/INFORMS-Releases/News-Releases/CONTINENTAL-AIRLINES-WINS-BEST-PRACTICES-AWARD-May-21-2002) , who was an Edelman prize winner when he was at Continental. He had solved the real-time crew scheduling problem and won the prize for that – a brilliant mathematician and OR guru. But Southwest’s first problem was they didn't have the data. So once again he wrote down a beautiful formulation, a multi-commodity network flow problem with a lot of side constraints, a very hard problem to solve. They had to get real-time data, then they had to get cached data where you could hit a button and download the current state of the airline. Those systems didn't even exist yet, they were all still being built.


When I showed up in 2014, they had already been working on this for six years. The key to success was putting four extraordinarily talented OR PhDs, led by Mark, in a room with two domain experts: One with 30 years of experience, a supervisor of dispatch that knew everything there was about dispatching aircraft and managing major airline disruptions, and a more junior team member that was very tech-savvy. Those folks sat in a room for a couple of years, so when I got there, they were about ready to put this into production. I said, "Well, have we tested it on live data? Have we tested all the data pipelines?" and none of that really had been done. I said, "I think we need to back up." Not only did we need to back up, it took another year of data refinement, model refinement, and model validation to actually conquer the change management curve.


We ran tests called “contests” to "beat the model." The humans were winning 50% of the time and the model was winning 50% of the time. So we just kept enhancing the model and climbing that S-curve until the model couldn't be beaten. Not only could it not be beaten, it was solving major airline disruptions to optimality in about 30 minutes. It used to take humans six years to do that. That system won[the AGIFORS'](https://agifors.org/) Best Operations Application award a few years ago. I thought it could probably qualify for an Edelman but we never got around to doing the paperwork on that. It had a significant impact on reducing the number of canceled flights, reducing the number of displaced passengers, and increasing on-time performance in winter storms by a factor of two or three. It was a significant improvement.


**Carolyn** : I think that's huge. And Karla, I’m guessing that your success story might have some similar themes to it?


**Karla Hoffman** : We learned from our failure that you really need processes in place that talk to the user and how that user makes decisions. We did end up having a bunch of PC packages where the parameters basically made it one package and users learned how to do that. The proof of success was that 10 years later I got this call saying, "We can't get rid of these computers and we need to get rid of these computers, but they have a math coprocessor chip, and we don't know where to find new computers with math coprocessor chips." To which I said, "All computers have math coprocessors, but isn't it time to relook at the problem and see how it can be improved?"


I also want to say that probably my biggest success was with the Federal Communication Commission (FCC) and their auction work. Like Doug, this was a six-month gig where we were supposed to give them the software and get out. We created an optimization team but it should have been called a data science team. We found the problems that the FCC were having and tried to give them decision tools to solve them. And 20 years later, there was this auction that brought in $20 billion, but the work involved in that was similar to what Doug described. You had to have everything tested to an extraordinary extent so that there was no possibility during this auction that something would go wrong, like a bidder bidding incorrectly; all of the little details and the change management associated with getting systems like that in place as well as talking to economists, lawyers, and a variety of telecommunication people. You had to speak their language, not my language. I think that's my biggest lesson learned.


**Carolyn** : It's always interesting in our space, especially with decision models that are impacting real-world operations. You do end up having a variety of different teams and stakeholders in the process. There are millions if not billions of dollars of impact. How do you get people engaged in that process, and how do you start to break down some of those silos? Do you talk about value first, or do you put something in front of people?


**Karla** : First, you find out what the issues are for every person in the room that you're talking to. I don't think you walk in with a prototype and say, "Look how great I am." Instead say, "Here's a possible way of looking at that problem. Am I wrong? How do we make tools to help you with your problem?"


**Doug** : Building on that, I can't do a session like this without channeling our friend[Dr. Gene Woolsey](https://weare.mines.edu/s/840/19/interior.aspx?sid=840&gid=1&pgid=7339#:~:text=Woolsey%2C%20who%20died%20in%202015,always%20putting%20his%20students%20first.) , who did a lot of consulting work in practice. Two things that stuck with me when he came to speak to our group at American Airlines was one, if you try to tell other people how to do their job better with math and computers, without understanding everything about what they do today, you're a fraud. That's a strong word, but that really hit me and it stuck with me. And two, a manager would rather live with a problem that they can't solve than implement a solution that they can't understand. In the example that I gave earlier, I failed on both of those fronts. On the back end when we succeeded, we went to great lengths to understand exactly how an airline recovers from irregular operations, the data that gets collected, the decisions that have to get made, how to model those decisions, and more importantly, how people are going to use the system. The human factor of all this is the absolute most important part. The math is necessary, the technology is necessary, but neither are sufficient to get the job done. If you're missing or shorting the human factor, the change management factor, or the engagement factor, if you’re not learning exactly how this works in the real world and understanding the business problem and underlying process, everything else is just a waste of time and you're headed down a rocky road to failure.


**Carolyn** : Ryan, what is your favorite phrase about getting models live?


**Ryan** : "If it's not in production, it has no value."


**Carolyn:** Exactly. Addressing the blockers to production, like getting things in front of users quickly, sounds like it was a key piece for both of your projects. Giving people something to react to as you're learning their process and coming in with humility, "I started to model this. Can you give me some feedback about the structure or about the input/output?" is always a nice way to start the process.


Over the course of your careers, where these projects are successful, what happens within those orgs? Do they start taking on more data science, decision science, and AI projects? Do those folks level up within the organization and tackle new issues? How have you seen that transform businesses?


**Karla** : In my experience, yes, they do. Being in this consulting space is great fun because you are always learning and people are always surprising you with their depth of understanding and the complexity of the problems they have to handle. And if you’re really curious, which is probably one of the most important characteristics needed in this field, you get to learn an awful lot. Then people naturally begin to trust you because you're not just hyping technology.


**Doug** : It's better to be lucky than smart. I was very lucky that I came out of grad school in '87 and was hired by American Airlines OR working for[Tom Cook](https://www.informs.org/Explore/History-of-O.R.-Excellence/Biographical-Profiles/Cook-Thomas-M) . There were 40 people in that group, and I was the 40th person. They were just coming off of the success of revenue management which[Barry Smith](https://www.informs.org/Recognizing-Excellence/Award-Recipients/Barry-Smith) and Tom invented from scratch for the airline business. The CEO at that time said, "If we can have this kind of impact, I wonder what we could do in all the other departments at American Airlines." He then took a pro-rata share of each department's budget and gave it to Tom: maintenance and engineering, food and beverage, flight scheduling, crew scheduling – every single department got a taste of what OR could do for them. Fast forward five years, there is American Airlines Decision Technologies with 500 people. We grew over an order of magnitude and then merged with Sabre, and the rest is history.


The same thing happened at Southwest. I inherited a group of 11 OR people that grew to 22. We had success in crew scheduling, then other people would hear about it and next thing you know the fuel department is showing up saying, "We need help with the fuel supply chain." Then the liquor department said, "Can we use that same tool?" And we did. We used exactly the same supply chain management tool that we used for jet fuel to forecast and optimize liquor inventory. So if you do it right, as Karla said, you can build that trust and reputation.


It happened at Walmart too. I started with four people and I had 20 by the time I got moved somewhere else. We started in one small area and just kept growing our little patch as other people came to us with other problems they wanted us to solve. That's how we delivered two billion dollars in value over the six years since I've been at Walmart. There's definitely growth potential if you get some early successes because everybody wants a piece of that pie.


**Ryan** : Groups that are in on-demand last mile seem to do it well. They keep getting new problems to solve. They start with routing and then scheduling, and then they have incentives and workforce management and so on and so forth.


**Doug** : All those things you just said either my boss or my old team are now doing in last-mile delivery at Walmart, particularly, how to incentivize drivers.


**Carolyn** : It seems like the key there is talking about the value very tangibly. Both of you have mentioned dollar values. Often, I hear practitioners talking about the objective function being better, or the improvement in the time to optimality or things like that, which are important. But how do you all see the difference between that type of language and talking with business users? Sometimes we have a gap there. How can we have more impact at a larger number of organizations, and what does that look like in terms of building that value proposition?


**Karla** : I have two different perspectives on this. One says that if you're talking to the CFO, you ought to be talking in the language of finance and economics. And one of my real successes was with a telecommunications company. The question was where should they be putting all of this new technology, so we did a natural economic model. There was a surprise though because they gave us a budget, and we stuck to the budget, but then we increased the budget just a little to see what would happen. We saw a very large rate of return and said, "Why aren't you borrowing money and doing more?" If you're talking to the CFO, that works really well. Of course it was also explained that the stock market had never seen this telecommunications company borrow money, so they couldn't borrow as much as they might have wanted to economically. The other perspective is that in a lot of analytics and OR projects, it's not even the ROI that you should necessarily be worrying about. It's about making their decisions better: easier to implement, easier to understand, less risk. If you walk into projects with that as your argument, it's probably a lot easier than talking about the bottom line. The bottom line will take care of itself. It depends on who you're working for.


**Doug** : I wrote an article that ended up in *The Art of Data Science* book called, "My MBA Made Me a Better Data Scientist." I went back to school in my late 40s and got an executive MBA and learned all about finance, marketing, and strategy. That was extremely helpful. The most important thing about that whole concept was, "Where do we start doing OR, data science, or AI?" and I said, "With the annual report and the financial statements." People usually react with confusion when they hear that, but an annual report tells you what business you're in and what the company's strategy is. If you read close enough, all the challenges that the company is facing are there: high inventory, low sales, slower delivery times, etc. Then the income statement will tell you exactly where your cost issues are.


If you think about an airline with $20 billion in revenue, 45% of that gets eaten up with crew and fuel costs right off the top. Our CIO at Southwest Airlines asked me, "How do you know that our crew schedule optimizer is really saving us money?" So I asked my guru in crew scheduling to run an experiment. “Let's take your crew schedule optimizer and turn the dial all the way down to zero in terms of its optimization capability so that it will just generate a feasible crew schedule." A greedy solution, basically. "The schedule will work. But then, turn the optimization dial up to 11, that's the max, and see what happens." The difference in the cost of the two schedules was $100 million a year. That answered the question. Those are terms that the crew department can understand because they have to crew the schedule at minimum cost and still be operationally feasible. The CFO is happy, the CEO is happy, and frankly the CIO got it and said, "Now I really understand the quantitative business impact and economic value that these things create." That exercise gets everybody on the same page. There are lots of other metrics, but there's nothing like good old-fashioned cost, revenue, and profit.


**Carolyn** : Karla, you mentioned being able to explain the decisions. Explainability and auditability ties into risk profile and ultimately the bottom line as well.


We’ve talked about a lot of the business aspects of decision science and making it successful, so let’s talk about the IT aspects. In today's industries, they're looking at cloud infrastructure and how to manage those pieces. How have you seen interactions with IT departments and software teams be impactful for getting solutions live and successful?


**Karla** : The faster a team can take the work away from an IT department that has a lot of other things that they have to worry about, the better. You also need to worry about the long term. Who is going to be responsible for the IT? Whether it's getting software licenses, ensuring that the costs in the cloud are reasonable, or making sure that the data continues to be linked properly, these things are hard. What my students would consider the fun part of an operations research project is less how you get the change to take place and more how you maintain and ensure that as the company is moving forward. The team responsible for maintaining the project needs to be aware of what is happening throughout the corporation.


**Doug** : I’ve lived through many cycles of compute: everything from mainframes to minicomputers to PCs and now the cloud. The increase in compute power that’s happened over my 40-year career is amazing. The testament to that is combining AutoML tools where you can give all your Ys and Xs to your own homegrown forecasting as a service platform. My team built that at Walmart and I asked them, "How long does it take to build a good predictive time series model or XGBoost ?" and they said, "an afternoon, once we have the data." It might take three months to get the data, but once you have the data, the modeling part is really easy. Like Karla said, it's the fun part. Even the coding has been reduced to a minimum that you can come up with the very best model or ensemble of models in an afternoon.


MIT produced a study that validated the number that we put in our book, *Why Data Science Projects Fail* . Moving from a Jupyter notebook or the cloud instance of a model to production is two orders of magnitude more time-consuming and more expensive than it is to build a model in the cloud. That factors in everything that Karla said: building in data pipelines, building in automatic refit, or detecting data drift or model drift and automatically refitting the models, which we do all the time now. We have a division of labor where there are data scientists and there are ML engineers that do a lot of that pipeline fitting. Then there are MLOps people that build monitoring tools and are constantly managing, monitoring, and maintaining because that model has to be up and running 24/7. There is both a huge IT component and a huge data engineering component to doing this successfully. There has to be a three-part team: the scientist, the data people, and the IT people that do the software, the systems, the cloud instances, etc. It’s about getting those three teams to work closely together to deliver a production solution. It's not inexpensive and it's not fast, unfortunately.


**Carolyn** : You had one of the pieces around silos in your book, with the IT team supporting an initial MVP for a model with the data science team. They might have to allocate resources to make you successful as a data science team. In reality, if you just had a sandbox or a pre-prod environment for your modeling that you could control, to Karla's point, it unlocks a lot of capability for decision and data science teams.


**Doug** : I'm a big fan of MVP and moving very fast with iterative, interactive test and development cycles. That gets you to the point where once the model's working then you need to put it into production. There are some times when it's a one-time decision and you may very well build a model, get the answer, and then you're done. More often than not though, if you're doing forecasting to figure out how many tractor trailers we need at Walmart every week, that's going to go on indefinitely. That has to be a stable ongoing system. We build everything as microservices, like contract programming: you give me inputs, I give you outputs. They're two prongs on our microservice that plug in to the whole ecosystem of many, many other systems at Walmart. That engineering team then has to take our model and plug it into their GUI, database, or system and that can result in waiting a bit until their backlog catches up.


**Carolyn** : With any of these models that live in perpetuity, once you get the model out there it's like, "Congratulations, now you can just spend all of your time researching why it did what it did and maintaining it." Maintenance is something that we don't talk about enough either in the transition from academia to industry too. Once you've created the artifact, your team is now responsible for that decision. That piece of code is now your responsibility for maintaining and observing what it's doing over time. All of that infrastructure needs to be in place for you to do that job successfully and get all the outcomes we just talked about.


‍
*Check out*[the techtalk recording for the full interview](https://www.nextmv.io/videos/lessons-lived-and-learned-project-success-and-failure-in-decision-science) *of lessons lived and learned in operations research and decision science.*
