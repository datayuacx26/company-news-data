---
schema_version: "1.0.0"
document_id: "d5009a3ea54eb284d9226e8e36167dbd428366efd98a2317b5236fdee90f4081"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-news-import-ffde4d661374"
canonical_url: "https://www.usewaypoint.com/blog/aws-denied-your-production-access-to-amazon-ses"
published_at: "2024-08-20T00:00:00+00:00"
first_seen_at: "2026-07-22T19:31:17.685303+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:6396646d801c5421defe868d99656d882651dd37fddb5f347a90d57c8c8d23fc"
---

# AWS denied your production access to Amazon SES?

### Overview


Amazon SES is cheapest way to programmatically send reliable emails at scale. While there are[some tradeoffs to consider](https://www.usewaypoint.com/blog/what-s-the-cheapest-way-to-send-transactional-emails) (particularly if you and your team is less technical), it’s hard to beat the price and flexibility.


This is why developers are attracted to SES. However, the unfortunate side effect of ‘reliable emails + inexpensive at scale’ is that this combination also attracts spammers.


AWS knows this and will default to denying access if there is any doubt. This is a fantastic thing since nobody loves receiving unsolicited emails – however, this also makes it difficult for well intentioned developers to get approved for[production access](https://docs.aws.amazon.com/ses/latest/dg/request-production-access.html) to send live emails. This is especially a challenge for developers working at early-stage companies that lack reputation.


More often than not, teams will receive the following response from AWS SES support:


*”We reviewed your request and determined that your use of Amazon SES could have a negative impact on our service. We are denying this request to prevent other Amazon SES customers from experiencing interruptions in service. For security purposes, we are unable to provide specific details."*


If you've received something similar, note that you’re not alone:


-


[“SES Email Sending Request Refused” via Reddit](https://www.reddit.com/r/aws/comments/1cjd1mv/ses_email_sending_request_refused/)


-


[“Tell: AWS Denied Production Access to SES” via Hacker News](https://news.ycombinator.com/item?id=34867603)


-


[“SES request for production denied. Help needed!” via Reddit](https://www.reddit.com/r/aws/comments/14eev1n/ses_request_for_production_denied_help_needed/)


-


["SES Production Mode Request Denied - how to solve?” via Reddit](https://www.reddit.com/r/aws/comments/m7ovsw/ses_production_mode_request_denied_how_to_solve/)


### Tips on getting out of the SES sandbox


1.


**Don’t be a spammer.**


-


If you intend to send cold emails or unsolicited emails, don’t use SES.


-


Even if you initially get access, it will quickly be shut down by SES.


2.


**Don’t look like a spammer.**


-


Before sending a request for approval, have at least a landing page with information about your company and app that you plan to send emails on. Ideally with a privacy policy linked in your footer as well.


-


In the request for approval, explain who will be receiving emails, how often, and the type of emails that will be sent. If sending to a mailing list, explain where existing contacts will come from, the content of the newsletter, and how unsubscribes are managed. The more details, the better.


3.


**Be proactive about monitoring for bad activity.**


-


Before sending a request for approval consider connecting the following within AWS:


-


[Setup SNS](https://docs.aws.amazon.com/ses/latest/dg/configure-sns-notifications.html) with[SES event data](https://docs.aws.amazon.com/ses/latest/dg/working-with-event-data.html) to monitor bounces, complaints, and deliveries.


-


[Setup the Reputations metrics dashboard](https://docs.aws.amazon.com/ses/latest/dg/reputation-dashboard-dg.html) in the SES dashboard to monitor at the aggregate level.


-


[Create reputation monitoring alarms using Amazon CloudWatch](https://docs.aws.amazon.com/ses/latest/dg/reputationdashboard-cloudwatch-alarm.html) .


-


In the request for approval, explain what’s been setup to proactively monitor for abnormal activity as well as your process within your team for resolving any issues that might come up.


### An alternate path to consider


While SES is a fantastic option, if you are working within a team that is less technical, you may consider an alternative approach that reduces developer bottlenecks. With Waypoint, teams not only have a reliable email API, it also comes with a tightly integrated visual template builder and frustration-free logs.


*Example of building templates on Waypoint (top) vs SES (bottom).*


*Example of end-to-end email observability on Waypoint (top) vs SES (bottom).*


Learn more on how[Waypoint compares to Amazon SES](https://www.usewaypoint.com/compare/amazon-ses-alternative) .
