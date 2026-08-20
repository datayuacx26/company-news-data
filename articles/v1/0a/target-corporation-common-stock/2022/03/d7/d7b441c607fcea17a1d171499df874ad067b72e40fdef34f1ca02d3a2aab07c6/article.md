---
schema_version: "1.0.0"
document_id: "d7b441c607fcea17a1d171499df874ad067b72e40fdef34f1ca02d3a2aab07c6"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/my-time-for-target"
published_at: "2022-03-17T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:aa9240dbbe76222e16de7c6ddc1081888f613aaabdf32deb964444c141e4a99e"
---

# myTime for Target: Built for Target by Target

Target has spent the last several years modernizing our technology stack and advancing our in-house development capabilities. We’ve grown to a global team of over 4,500 incredibly talented technologists and created a culture of testing, learning, and agile processes to great success. This journey has led to an uptick in fantastic new homegrown innovations that help make Target a joyful place to shop and work.


We’re here to highlight one internal innovation in particular – myTime for Target, a mobile scheduling app for our field team. myTime for Target was built in-house from the ground up and rolled out last year to enable a more connected and engaged field workforce. Team members can voluntarily use the app as another way to digitally view their schedules and timecards, indicate availability preferences, and request coverage for shifts as well as pick up new ones.


How We Started


myTime for Target started as many engineering discussions do – ideating about how we can deliver purposeful capabilities that improve an experience for our guests and/or team. In this case, the team asked itself: How can we make scheduling more efficient for field leaders and more flexible, convenient, and empowering for field team members?


It’s this type of experience-centric approach that guides our product engineering team here at Target. We spend time understanding the personas of who we are building for and what they value most. In this situation, we wanted a tool that would provide our frontline team members with more options of working when they want as well as more adaptability and control in managing their own schedules. We wanted this to be an impactful investment in technology that would translate into a meaningful investment in our team.


Developing the Technology


We needed a solution that would be easy to use and could scale to meet the needs of 350,000+ hourly team members across the country. We also needed a solution that emphasized responsiveness but could manage difficult data structures comprised of large volumes of schedule and timecard data.


Target’s technology product model really proved advantageous once we determined our objectives and requirements. There was no need to devise new technologies – we built the app and its features completely on our existing technology stack. It was a testament to our years of investment in developing a customized platform and microservices architecture.


We started defining minimum viable product parameters by engaging with distribution center partners and focusing on key improvements to their process. Schedule preferences and visibility were important to one portion of that team while the ability to manage and communicate about voluntary and mandatory schedule changes were critical to another portion. We built these functions into the app and began testing them with a single distribution center and a couple thousand team members.


During the process, our team pivoted to producing data from Kafka and consuming it into caches that serve the myTime for Target application. All of this operates at and for Target scale showing a minimum amount of delay between the underlying administrative system and the mobile application, while maintaining integrity of our scheduling and timekeeping data.


Testing and Learning


We slowly expanded the number of distribution centers we worked with and continued to improve the app’s functionality based on more input. Focus group feedback gave us valuable insight on how team members were using myTime, what additional features would be helpful to extend our use cases, and how we should adjust engineering priorities as we continued to expand the pilot. Early on, our engineers built a “request to cover” option where team members can post and swap shifts or pick up more hours simply, and with automatic approvals that account for data complexities. The capability has helped speed up the time it takes for some weekly scheduling and planning processes to just a few minutes.


With an average of over 70 interactions per second on the platform by team members, we have helpful user feedback arriving via multiple channels and our team continues to make additions, corrections, and improvements to the platform through weekly releases. Our goal remains the same – a solution that streamlines tasks and improves the scheduling experience.


We’re proud of this technology we’ve built and the positive impact that it’s having for our team members and leaders. And much more is possible. As we work to deliver additional scheduling capabilities and features, we will provide more opportunities for deeper engagement, improved work life balance, and a more empowered and connected workforce. The app and our engineering process is proving that both what we build and how we build are competitive differentiators for Target.
