---
schema_version: "1.0.0"
document_id: "73cec368b79142efa42607fa805c42b303ca40a5faafaf8b1ba1fa2a0c2f039e"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2018-06-08-ny-hackathon/"
published_at: "2018-06-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:2e82cbc3958fed9b14c6e3b515fce865403a5e2c429ca557aa30dd8fe4f71497"
---

# Interview with the Winners of trivago's New York Hackathon

On the 11th of November 2017 trivago hosted a Hackathon at NASDAQ’s MarketSite in New York. The winners were awarded with a trip to Europe to visit trivago’s offices as well as conference tickets for[The Next Web, Amsterdam](https://thenextweb.com/event/tnw-conference/) . We invited the winners to our office where we got the chance to interview them about their Hackathon experience.


## Winners:


[Colvin Zhu](https://www.linkedin.com/in/colvin-zhu-30484769/) is an Application Developer at[JPMorgan Chase & Co.](https://www.jpmorganchase.com/) He is working on a data injection platform. He mostly uses Scala, Hadoop, Kafka, and Redis.


[Josh Devasagayaraj](https://www.linkedin.com/in/joshdev/) is a Product Backend Engineer at[Squarespace](https://www.squarespace.com/) . He is working in the Analytics Product team. His main programming language is Java. He writes Hadoop jobs and backend services to crunch and pivot data to power the customer-facing analytics panels.


[Sankalpa Khadka](https://www.linkedin.com/in/sankhadka/) is a Data Engineer at[JPMorgan Chase & Co.](https://www.jpmorganchase.com/) He works in Asset Management where he builds ingestion pipelines for various internal and third party data sources and integrates them to help marketing and sales teams make data driven decisions. He mainly works with Python, Java, Hadoop HDFS, Spark and Scala.


[Alex Reibman](https://www.linkedin.com/in/alex-reibman-67951589/) is a Data Scientist and Software Developer at[Axio](https://axio.com/) . He feels at home using Python, Flask, ReactJS, and various machine learning and data visualization libraries.


[Moritz Knorr](https://linkedin.com/in/moritzknorr) works as a “Data Strategist” at[AXA](https://www.axa.com/) , he develops and realizes AXAs strategy regarding topics like big data infrastructure, analytics, innovation and cloud. He still likes to program (Python, Bash) as a hobby. *Moritz was not able to participate in the interview.*


## Q: How did you hear about the Hackathon? What was your motivation to sign up for it?


**A:** (Alex starts) Colvin and I are good friends from College. I was visiting NYC when I saw the Hackathon online for the following weekend. The idea was to meet up for the weekend and hack together for 24 hours. Then, Colvin asked his teammate Sankalpa if he was interested in joining. Sankalpa thought it would also be exciting for his roommate Josh. We all came together and proposed our idea. Later on, we met[Moritz](https://www.facebook.com/lifeattrivago/videos/1980952281920365/) at the hackathon, and he joined our team for the competition. Moritz was in NYC because he solved[trivago’s coding challenge](https://tech.trivago.com/2017/09/25/guerilla-advertisement-for-creative-thinkers/) and won a trip to NYC.


## Q: What was your strategy to work together? What was the idea of the project?


**A:** We all focused on our unique strengths, so we were each able to execute what we were best at. Three of us got together and started working on the prototype. Moritz and Alex were focusing on the idea and getting the presentation ready. We mostly worked in parallel.


What we built was inspired by a true story, actually. Alex was travelling in Japan for a few weeks. He needed to book a hotel, but he was in a very remote region with no internet connection. He could only use his phone’s SMS service, so he texted his friend Tim back in the USA and asked him to book a hotel. From this idea, we thought that it would be substantially valuable to have an SMS chatbot that you can query and book hotels without an internet connection. We created this product, and we named our project “Roaming Tim” after Alex’s friend Tim who booked the hotel.


## Q: It is fascinating that you built something new on top of the already existing legacy service with your creativity. How did you do that? What technologies did you use?


**A:** Before we started, we first wanted to talk with the judges to learn more about how trivago works and how it makes money. It was important to understand trivago’s business model before we designed our project, and we learned that trivago earns revenue based on click-outs. Unfortunately, trivago had no open API that we could use. We had to find our way to get the search results or any data; therefore, we used[Selenium](https://www.seleniumhq.org/) , a web browser automation framework to automatically query hotels and scrape data.


The SMS portion was powered by[Twilio](https://www.twilio.com/sms) . Twilio was just connecting to this worker robot, who would search for a hotel – just like there was a real person on the other side. This fits trivago’s cost per click model, so the clicks were actually happening in the background, but the result was sent back to the user as an SMS. We built this chat-based SMS API where you could go and search for hotels by sending SMS.


The API was built on top of Java and used the[Spring framework](https://spring.io/) . After querying a destination, the bot would return the first five hotels to the user in the SMS. To keep things simple, we did not use any natural language processing. Our bot had options like “press number 2, if you would like to search a hotel” or “press number 3 to select”etc. It is way easier to use the tool if the options are given to the users instead of waiting for the users to formulate a full-text query. It is simple for both sides.


## Q: Was it surprising that you won? What did you feel when you heard that you are the winner?


**A:** (laughter) Oh, it was indeed surprising. They started announcing the winners from third to first place. When we were not announced until the end, we thought, “oh no it is certainly not us” :)


## Q: Can you describe some other projects and what you liked about them?


**A:** We liked Tinder for hotels, which was second place. Instead of browsing through pages and pages of results, every hotel will be presented as a “card”. It was an interesting concept. Whenever you swipe left, it would optimize the next suggestion. Every interaction refines the results. Eventually, the app could score the hotels based on these user-interactions, so even selecting a hotel that is not going to be booked in the end is valuable information.


Another funny one was a speech-based hotel search based on Amazon’s Alexa. During the demo, the voice agent was misbehaving and kept interrupting the speaker, so they had to unplug it.


## Q: Would you change anything if you could do the hackathon project again?


**A:** We were using Selenium to scrape results from the trivago website and we had problems with it until just before the presentation. So if we had a proper API for this, it would have been a much more robust solution.


We also overcomplicated things in the beginning and learned that we could achieve the same results with a much simpler approach. In the beginning, we had a full conversational bot. However, talking to it was hard, so we replaced the complex text query handling with simple keywords and number-based options. That worked much better. On one side, you want to show something exceptional, but under time pressure, you can’t include everything.


The good thing was that we trusted and supported each other every step of the way, so these issues didn’t really matter in the end. One of our strengths was that even though there was some uncertainty in the plan, we were flexible and could adapt. We made sure that no single component would fail during the presentation, and we had backup plans.


## Q: For many people, participating in their first hackathon can be intimidating. What would you tell those people?


**A:** A hackathon might seem scary on the surface, but it’s a great opportunity to take a chance on yourself. Everybody is trying to support you and encourages you to try your best. If you fail, it’s a soft failure - you don’t lose much. On the upside, you can gain a lot of wisdom just by participating. So even if you can’t finish your project, you come away with a stronger sense of self and a sense of how to craft a minimum viable product in a very short amount of time.


## Q: What is the difference between a Hackathon project and a production-ready product in your opinion?


**A:** Real products should follow a clearly defined development plan. They are commonly more thought-out and built with less time pressure. A hackathon project is much more experimental. You quickly move from problem to problem and you don’t have much time to reflect on your design decisions. Also, during a hackathon, you don’t have to worry about security, privacy, etc. In a real product, you have to seriously consider these things. A hackathon is built around compromises and is optimized for the demo. For example, we had to remove features to be able to present something. The edge-cases and the failure-cases are much more important for a real product. You also have to do a lot of user-testing to be sure you built something that people actually want at the end of the day.


## Q: What’s the ratio between art and engineering during a hackathon?


**A:** In the beginning, we were brainstorming about different ideas, so we prioritized creativity over productivity. Later, the engineering part took priority as we implemented features and had to compromise on ideas to get the project done.


## Q: Why do you love programming?


**A:** It feels very fulfilling when you can spend time doing a craft, such as writing, painting, or coding, that you are passionate about. Programming is great because you can easily meet people around the world that also share this passion. As an engineer, you get a problem and constraints and time to think it through. It’s very enjoyable to write well-thought-out solutions. Programming is about writing elegant solutions to the challenges in front of you, and you feel a sense of accomplishment when everything falls into place.


## Q: As a prize for winning the Hackathon, trivago paid for a trip from NYC to our offices in Amsterdam and Düsseldorf. How do you like the journey so far?


**A:** Because of the hackathon, we grew from a ragtag team of coders into a group of great friends. Based on our initial success, we’ve also since competed in and won a few more hackathons. It’s amazing to be able to travel and get to know each other even better, and it has planted the seeds for a very bright future for us.


## Q: In addition to the trip, you also won tickets for a conference organized by[TNW (The Next Web)](https://thenextweb.com/) . What are your expectations for that?


**A:** As a matter of fact, we also won the TNW hackathon in New York last year. We already went to their New York conference, and we liked it a lot. The upcoming conference is going to be even bigger with even more participants, so we are looking forward to meeting more like-minded people there.


## Summary


Needless to say, trivago is very happy with the outcome of the Hackathon. The ideas that the teams brought to the table and the quality of the prototypes that they created in such a short amount of time is simply astonishing. We strongly believe in an open environment that encourages knowledge-sharing, so we’re looking forward to our next programming events.
