---
schema_version: "1.0.0"
document_id: "4486d968582da65991d17170152e9ea48912556412b941fe15d9b6ab8a3bcdab"
company_key: "sps-commerce-inc-common-stock"
company: "SPS Commerce Inc."
source_id: "sps-commerce-inc-common-stock-rss-e53e7c01950b"
canonical_url: "https://tech.spscommerce.com/2021/11/01/peace-of-mind.html"
published_at: "2021-11-03T19:56:15+00:00"
first_seen_at: "2026-07-20T23:19:01.810458+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:a9b296661e7e9da98a789bdffd6fb265abce6b6fa178e1959374d8e1167201ee"
---

# Peace of Mind & Two SPS Engineers Were the Big Winners at Hack The Gap:

Two of our Software Engineers from our Minneapolis office,[Ana Knickerbocker](https://www.linkedin.com/in/anaknickerbocker/)[(find her on Twitter too!)](https://twitter.com/akknickerbocker) &[Jenna Truong](https://www.linkedin.com/in/jenna-truong-26442320/) took part in a hackathon in October through Hack the Gap. We asked them to share a little about the hackathon and what they did. They were on a team of 4 individuals and their project ended up winning, aligning nicely with the SPS Value of *Win Today, Win Tomorrow.* 🎉🏆


**About Hack the Gap:**


“At Hack the Gap our focus is increasing equity in technology by highlighting diverse voices and perspectives. This includes women, non-binary, black, indigenous, and people of color. We do this through hackathons, programming, and investing in technology built by and for women, non-binary and BIPoC peoples.” ([https://www.hackthegap.com/](https://www.hackthegap.com/) )


**About Hack the Gap Hackathon:**


Hack the Gap hosts hackathons for women and non-binary individuals. These are usually different from a typical hackathon as they might occur in the middle of the week or over a longer period of time to better accommodate the schedules of mothers, women, and non-binary individuals that they cater to.


**Our Project:**


Our project was built to address a gap in available task messaging apps. In our demo we highlighted several key points:


- Are you highly skilled in the art of forgetfulness? I know I am! We’re here to offer you some Peace of Mind.
- Peace of Mind is a task management app that won’t let you forget. You can get an indefinite number of alerts via text messages, emails, and phone calls so you can’t ignore the task at hand.
- The next iteration will offer more relentless notifications. For example, “text me every 1 minute until the task is completed” or “text me every 5 minutes for the first hour minutes, then call me every minute until the task is completed.”


We chose this project because time management has gotten tricky since the start of the pandemic. People are experiencing difficulty perceiving the passage of time, and often feeling “brain fog.” Many tasks are too important to be delayed or forgotten, like taking medications, caring for others, or caring for yourself. We believe this application will help some stop worrying about forgetting, and bring some peace of mind.


The project was initially generated using Nx. We used a Node/Express/TypeScript backend with a React/TypeScript frontend. On the frontend we incorporated components from Bootstrap, Twillio & Ant Design. One of our teammates was a designer who also created some beautiful logos! The scheduling of alerts was accomplished by calculating the next alert DateTime, saving that DateTime to the database, and running a query against the database every minute using a cron job. The alerts were sent out via Twilio/SendGrid APIs.


**What we learned:**


On the frontend, in addition to building a UI from scratch for the first time, we learned about incorporating new design systems. Ana created 2 databases (Postgres > RethinkDB > Postgres) and deployed both of them! We also learned that no matter how good the last demo recording was, you’re likely to just keep re-recording up until the deadline. *(Two more SPS Values: Thirst For Growth and Know More To Be More.)*


Since we wanted our tool to function as an app instead of just a webpage, in the future we hope to learn how to make this a better cross-platform tool. For example, we would like to include recurring tasks, symptom and mood tracking, and a caregiver mode where you can set alerts for others. Additionally, the API will be made public for easy integration with applications like Trello, Google Calendar, Siri, and Alexa.


**What we enjoyed:**


The highlight of the hackathon: our team won! *(Again, demonstrating those SPS Values: Succeed Together & Win Today, Win Tomorrow.)* We also enjoyed the opportunity to work with new teammates located across the country (and world) in Michigan and Spain! A beginner developer on our team had never seen an application built from scratch before! It was fun showing her how everything worked, helping her contribute, and answering all her excellent questions. It was also refreshing to work on a side project where we could be as creative and as innovative as we wanted.


[SPS Technology](https://tech.spscommerce.com/)[@spstech](https://micro.blog/spstech)
