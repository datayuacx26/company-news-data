---
schema_version: "1.0.0"
document_id: "6281cf7e693f30137662f55427ab31f3bb958b9931d65e91e252c6f437fe3c00"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-b3032587319b"
canonical_url: "https://tryterra.co/blog/buy-or-build-should-you-build-integrations-in-house-or-use-terras-api-18030ccf4560"
published_at: "2022-11-01T00:00:00+00:00"
first_seen_at: "2026-07-22T16:07:05.122696+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:4315f5c46cb82a21dfd32c6dfe10d31cf2489c32c46863482f5811b607b399f1"
---

# Buy or build: Should you build integrations in house, or use Terra’s API | Terra

[< Blogs](https://tryterra.co/blog)[Kyriakos Eleftheriou](https://tryterra.co/blog/author/kyriakos-eleftheriou)


November 1, 2022


# Buy or build: Should you build integrations in house, or use Terra's API


We noticed that over time, most customers that try to build integrations in-house contact us to use Terra since they realize that integrations are much more difficult than they look and take much more time.


Furthermore, I wanted to write an article to explain the process of what's necessary, and how to make an informed decision about whether to build this in-house or use Terra to connect with all of them at once through our[fitness API](https://tryterra.co/products/api) /[wearable API](https://tryterra.co/) .


The process of creating an integration involves the following steps:


- Establishing a partnership
- Doing the integration
- Standardizing data
- Maintainance and updates
- Security and Privacy


**Forming a partnership**


Most people don't think about this - but it is one of the most critical steps. Assume you want to create an integration with a company like[Garmin](https://tryterra.co/integrations/garmin) or[WHOOP](https://blog.tryterra.co/whoop-is-now-available-through-terra-api-7394df24d60a) . You would need to contact the company and speak with multiple teams of theirs until you establish access to their technology - assuming it's available. In such cases, for many wearable companies, it actually takes a lot of time to be approved. In many cases, it might take almost a year for this to be completed. (yes, we've been there)


**Doing the integration**


Each wearable company uses a different technology. For example, a company like Garmin uses Webhooks, and a company like Fitbit uses HTTP and REST APIs. In addition, there are mobile developers with SDKs and so on.


Connecting with all these technologies will require good expertise in DevOps, and you would need to have the people internally for these tasks.


To connect with mobile apps, you would need a mobile developer for Webhooks, a backend developer, and so on. You would also need to establish your backend to support all of these, and hence you would need an expert in infrastructure.


**Standardizing data**


After you connect to all the integrations, you will realize that most companies use their language and have their own formats.


For example,[Peloton API](https://tryterra.co/integrations/peloton) might have ‘calories', whereas[Zwift API](https://tryterra.co/integrations/zwift) might have ‘kcal'. Moreover, you will see that everyone is using different files and formats. For example, Suunto API might use the FIT format protocol, and another wearable uses JSON.


Strong expertise in data engineering is necessary here.


And let's not forget. Unfortunately, most great hardware wearable companies are not the best at software, so you will have to go through many issues understanding inadequate documentation and so on.


**Maintenance and updates**


APIs and connections change constantly, and usually, there's a change every week. For example, WHOOP is a wearable company that moves fast due to how innovative they are. Hence, the data the device measures changes continuously, adding more and more. Thus you would need to keep up.


Likewise, Oura is adding more and more data to their devices. In fact, last week they added the ability to measure your activity from the ring. As you can imagine, this leads to some very important data points, and updating your integration with them would be very important.


In addition, many times an API or integration you already made will not respond, have issues, etc. So you would need to be on top of it all the time. (Terra has a handy dashboard that shows you the health of each integration.)


**Security and privacy**


Speaking to all these wearable APIs / fitness APIs requires constant testing and validation if things work. You should ensure that the data you receive is always secure and that your compliances are in place, such as GDPR, HIPAA, etc.


**To sum this up**


The way I would think about it is whether I want to have an in-house DevOps team who does this internally all the time, or if I want to focus on my core business and use[Terra's API](https://tryterra.co/products/api) to do that for me.


### Related Articles


[December 5, 2024 5 Lessons for Standing Out at HLTH 5 lessons from team Terra API for making a lasting impact at HLTH: from engaging senses to building real touch points, here’s what we learned from the HLTH event. Vanessa Neeff](https://tryterra.co/blog/5-lessons-HLTH-success)[November 21, 2024 Strava Pulls the Plug on their API: What This Means for Developers Strava discontinued their API service, changing the ecosystem of third-party apps that have relied on their platform. How can developers react to this? Terra API](https://tryterra.co/blog/strava-discontinues-api)[November 19, 2024 Alternatives to the latest changes in the Strava API Strava just introduced big changes to their API program. These changes will basically kill off a lot of apps. Use Terra API instead to avoid this Kyriakos Eleftheriou](https://tryterra.co/blog/Strava-API-Alternative-for-wearables)


### More Topics


All Blogs


Team Spotlight


Startup Spotlight


How To


Blog


Podcast


Product Updates


Wearables


[See All >](https://tryterra.co/blog?category=all)


The complete guide: How the new Google Health API works


Google Health API replaces the Fitbit Web API. This is the field guide with code, schemas, and a migration playbook to help you understand where Google Health is heading.


Vanessa Neeff


May 18, 2026


September 2025 updates


July: Terra Research launches, Lab Reports land in the dashboard with PDF/Image → JSON, and Samsung Health moves to the new Data SDK for a tighter Android integration. 🚀


Alex Venetidis


October 1, 2025


August 2025 updates


🎉 July Highlights: InBody Goes Global, Faster APIs, and Rock-Solid Data 💪📊


Alex Venetidis


September 1, 2025


July 2025 updates


July = rock-solid Terra: WHOOP V2, Garmin & Fitbit bug fixes, faster SDKs, plus bulk blood-report uploads with smarter reference ranges. Reliability + data power-ups! 💪🩸


Alex Venetidis


August 2, 2025


June 2025 Updates


June brings Terra MCPs for AI-driven setup, Fern-powered Python/JS SDKs with strong typing, and official Expo plugin support—build faster with less friction. 🚀🧰📱


Alex Venetidis


July 1, 2025
