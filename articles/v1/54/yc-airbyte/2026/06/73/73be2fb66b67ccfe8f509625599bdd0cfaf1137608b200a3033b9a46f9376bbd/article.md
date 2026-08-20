---
schema_version: "1.0.0"
document_id: "73be2fb66b67ccfe8f509625599bdd0cfaf1137608b200a3033b9a46f9376bbd"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/automating-breakfast-menu-with-airbyte-agents"
published_at: "2026-06-15T05:34:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:2c808e7e97b533f436b69a96e5e907c7555bb106e5ca5bb1c3cb2ead2df31044"
---

# Automating Our Office Breakfast Menu with Airbyte Agents

I manage the workplace experience at Airbyte's San Francisco office. My background is in recruiting, personnel, and HR, and somehow over the years that evolved into me running every part of the office that makes people actually want to show up: the food program, happy hours, wellness sessions, building logistics, birthday celebrations, you name it.


If it involves keeping this office running and keeping these people fed, that's me.


I am not an engineer. I want to be real clear about that.


For a while now, I'd been wanting to get our weekly breakfast menu into Slack automatically. All the information was already sitting in my vendor confirmation emails. I tried to make it happen with Claude a while back, but it just couldn't connect my email to Slack in a way that actually worked. So I shelved the idea.


## **A breakfast conversation about breakfast**


Last Tuesday, I was chatting with one of our engineers, Aldo, over breakfast in the kitchen. I mentioned the idea, half joking, like "wouldn't it be nice if my vendor emails just showed up in Slack every week."


He didn't laugh. He said, "Airbyte Agents could do that."


Now, I hear the engineers around here talk about agents and connectors and the Context Store all day long. That's not my world, but Aldo made it sound straightforward enough that I figured it was worth trying.


## **What I actually built**


I went to app.airbyte.ai and used two connectors: Gmail and Slack. The Gmail connector is still in early access, so I got a little sneak peek before the rest of the world.


Here's what it does. The agent reads my Gmail, finds the order confirmations from my food vendors, pulls out what I ordered and for which day, creates a Slack channel (it made #chat-breakfast all by itself), and posts the full menu broken down by day. Each day gets its own post with the vendor name, every item listed out, and cute little emoji next to each one.


The whole thing took a few commands to set up. I told it what I wanted in plain English. "Scrub my email for food orders from my vendors and post the breakfast items to Slack by day." A couple of back-and-forth messages to help it understand how my vendor emails are structured, and that was it.


I did not write a single line of code. Not one.


## **The results were fantastic**


When I saw the first posts show up in #chat-breakfast, formatted all nice with the vendor name in bold and little bacon and egg emoji next to each item, I was so excited!


The team went from asking me five times a morning to just checking the channel. Tuesday is Black Bear Diner thick-cut french toast. Wednesday is Panera. Thursday is Specialty's croissants and breakfast sandwiches. Friday is Schlok's bagels and lox. It's all right there, every week, without me lifting a finger.


## **Why this matters (from a non-technical person)**


I have been looking for an AI win in the kitchen. Something that saves me real time on a real task that I actually do every week. Airbyte Agents gave me that.


And the thing that gets me is how low the barrier was. I didn't need to learn Python. I didn't need to understand APIs. I didn't need an engineer to build it for me, though I did need one to point me in the right direction over scrambled eggs. I just described what I wanted, and the product handled the rest.


If non-technical team members can build automations easily with the product, it says a lot.


I’m already thinking about what else I can build with Airbyte Agents to improve the office experience!
