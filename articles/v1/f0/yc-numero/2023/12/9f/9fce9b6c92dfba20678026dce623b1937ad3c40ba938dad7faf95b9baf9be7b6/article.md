---
schema_version: "1.0.0"
document_id: "9fce9b6c92dfba20678026dce623b1937ad3c40ba938dad7faf95b9baf9be7b6"
company_key: "yc-numero"
company: "Numero"
source_id: "yc-numero-rss-2718f20b56fd"
canonical_url: "https://www.numero.ai/blog/turning-slack-into-your-central-fundraising-hub/"
published_at: "2023-12-12T05:00:00+00:00"
first_seen_at: "2026-07-25T16:42:35.962761+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:844f6f34ac11d0ee0a9f9b86ca7d4896247d18ba51f1e2b7b8d9864e23a3e278"
---

# Turning Slack into your Central Fundraising Hub

*This post was originally published by Kalani Tissot on*[his blog](https://www.tissotsolutions.com/post/turning-slack-into-your-central-fundraising-hub) *and has been reposted here with his permission.*


Our team started using Slack just a month ago, and we’ve fallen in love with it.


Even when we were just a two-person consulting firm, using Slack significantly helped us communicate and collaborate more effectively. Previously, we had just been talking via text, email, and a daily Zoom meeting — but through Slack, we’ve been had able to coordinate more and tone down the number of internal emails we send to each other on a daily basis.


Furthermore, Slack is more far than just a glorified professional chat room. We’ve turned it into a central dashboard and hub for our firm, and that’s made a world of difference.


Here are some of the integrations & tools we’ve put together to make Slack a core part of our firm’s tech infrastructure:


## **Live Donation Feed:**


(Names have been redacted for donor privacy)


As a political consulting firm with five clients, we currently manage numerous accounts on donation processors such as ActBlue,[Numero](https://numero.ai/) , and DemocracyEngine. To keep up with each client’s donations, we’d have to refresh their ActBlue account or pull data separately from each of their donation processors.


Instead, we can bring all of their credit card donations and some of their check contributions into one central feed on Slack.


This is set up through webhooks,[email parsing](https://mailparser.io/) ,[Zapier](https://zapier.com/) , and[Pipedream](https://pipedream.com/) .


Let’s break down how we bring each donation processor into this central feed:


**ActBlue** — The biggest donation processor on our list! To bring contributions into Slack, we set up an ActBlue webhook on each account that is connected to a Pipedream workflow. Typically we use Zapier as our main automation tool, however, Zapier gives us a limited number of “tasks” each month, and we prefer to use those for bigger automation needs. Instead, we use Pipedream, which gives us 10,000 invocations (what they call tasks or triggers) each month for free.


**Numero** — For[Numero,](https://numero.ai/) you have to use email parsing to bring data into Slack and to other apps in your stack. Our firm uses[Mailparser.io](http://mailparser.io/) , and it has been super helpful. New contribution alerts are emailed to our mail parser, which then sends the data to Zapier. Zapier then takes that data and sends it to our donation feed Slack channel.


**Here’s a Zap we have set up to do this:**[https://zapier.com/shared/6c520af78dc0babcfe88e95340d7de6a7ea9d6a7](https://zapier.com/shared/6c520af78dc0babcfe88e95340d7de6a7ea9d6a7)


You’ll note that this Zap also sends the Numero contribution data to the client’s Airtable database, adds the contributor to the Client’s[Action Network email](https://numero.ai/blog/using-action-network-for-fundraising-emails/) list, and then sends a Slack message to our channel.


**Democracy Engine** — For contributions on this platform, we also use[Mailparser.io](http://mailparser.io/) and Zapier to send in those donations to our Slack Channel. The client that currently receives money from this platform also has their Finance Plan on google sheets. This zap includes a step to update a row on their Finance Plan with the new contribution data to keep their Finance Plan up to date.


**Live Checks** — If you have staff members or a PAC/Fundraising Consultant that picks up checks for your client, you can have them include the mail parser on their check emails. This requires them to send check emails with the same template each time (Typically a quick table with date, check, check amount, and source code). If the template is the same, the mail parser can push that data into Zapier and add it to your Slack Channel, Finance Plan, and more.


## **Donor Birthday Feed:**


We have a dedicated Slack channel at our firm to notify staff of when donors have their birthday. We track that information for all of our clients to help them keep in touch with their supporters and send them a special note.


This is set up via Airtable (where we track Birthdays) and Zapier. Here’s the Zap:[https://zapier.com/shared/34284f6b3fb39cb20feb2ccd956697475335a5dd](https://zapier.com/shared/34284f6b3fb39cb20feb2ccd956697475335a5dd)


When Birthdays come into a view on Airtable, they are then sent to our Slack Channel.


As the author and master networker Keith Ferrazzi states in[Never Eat Alone](https://www.amazon.com/Never-Eat-Alone-Expanded-Updated/dp/0385346654) , “Don’t kid yourself — EVERYONE CARES ABOUT HIS OR HER BIRTHDAY.”


This is one of the few ways our clients can engage and keep in touch with their supporters without asking them for another contribution.


## **Client Emails:**


All five of our clients have a campaign email address that is separate from their personal or official email accounts. As their fundraiser, we keep a watchful eye on any emails from donors that come into these accounts to notify our clients or get a quick response back to said donor about an upcoming event or scheduled call.


**This is set up by a quick three-step Zap:**[https://zapier.com/shared/2dea60dec943bf67ae5645925a55afb26340e3fd](https://zapier.com/shared/2dea60dec943bf67ae5645925a55afb26340e3fd)


We use the Filter step to filter out any email lists or regular email alerts that we wouldn’t want to throw into the Slack channel.


## **Client Twitter:**


Staying up to date on everything our clients are doing is TOUGH. We work with great people, and their schedules keep them out and about all the time. To stay abreast of the latest news about their endeavors, we have a Slack Channel where all of their Tweets are automatically sent to.


We also make sure the icon is fitting for each client. Treasurer Conine gets a bank emoji, Attorney General Ford gets a police car emoji, and Cisco Aguilar, who is our candidate for Secretary of State, gets a letter in the mail emoji (mail-in voting)!


**This is set up by a two-step Zap that we have for each client:**[https://zapier.com/shared/ba1e0d20f316d55a96d7843cbe35eb99af725827](https://zapier.com/shared/ba1e0d20f316d55a96d7843cbe35eb99af725827)


## **Client News via RSS:**


Lastly, we also have a live RSS feed that brings in relevant news articles for our clients.


[Google Alerts](https://www.google.com/alerts) is a great tool that will automatically send you notifications when news articles matching your selected keywords are published. However, getting 5 emails a day from Google Alerts (one for each client) would be overwhelming!’


Instead, we automatically bring all of their articles into this Slack Channel. You don’t even need to use Zapier, Pipedream, or an automation tool to do this. Slack has an RSS Feed tool in their app directory:[https://slack.com/help/articles/218688467-Add-RSS-feeds-to-Slack](https://slack.com/help/articles/218688467-Add-RSS-feeds-to-Slack)


Each of these individual channels helps bring important data and information right into our firm’s Slack. Our whole team is kept up to date on critical developments, and it improves the efficiency by which we absorb information & alerts about our day-to-day work.


It is important to mute all of these channels — otherwise, you might get a few dozen alerts each day that will distract your focus.


Do you use Slack for your political campaign or consulting firm? Let me know! I’d love to hear about any integrations, custom channels, apps, or ways you’ve used Slack. You can shoot me an email directly at[\[email protected\] .](https://www.numero.ai/cdn-cgi/l/email-protection#81cae0ede0efe8c1d5e8f2f2eef5d2eeedf4f5e8eeeff2afe2eeec)
