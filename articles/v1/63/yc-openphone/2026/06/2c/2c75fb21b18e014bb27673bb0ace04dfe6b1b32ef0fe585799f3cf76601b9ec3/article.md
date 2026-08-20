---
schema_version: "1.0.0"
document_id: "2c75fb21b18e014bb27673bb0ace04dfe6b1b32ef0fe585799f3cf76601b9ec3"
company_key: "yc-openphone"
company: "Quo (fka OpenPhone)"
source_id: "yc-openphone-news-import-b4955105a2fb"
canonical_url: "https://www.quo.com/blog/quo-claude-call-insights/"
published_at: "2026-06-04T21:55:44+00:00"
first_seen_at: "2026-07-22T10:45:40.935626+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:b1d39aec0b94bb6cdb4d73f83a7e227c5625fa4155ab94d22248dbef0c873535"
---

# The 4 levels of building Claude workflows for your team

If you run a small business, you’re sitting on a goldmine of insights from your customer conversations. Every intake call, support interaction, or pricing negotiation is an opportunity to better understand your customers.


But who has time to listen to 50 calls a day? And as you scale, how do you stay on top of your workload without adding headcount to your team?


With Quo’s Claude connector, you can uncover customer insights from your conversations in minutes. Plus, you can turn these insights into action just as quickly.


In this article, we’ll cover the four levels of building Claude workflows for your team. We’ll use real examples from the Quo team and from Hannon De Palma, a law firm that uses Quo to manage their client communications. By the end of the article, you’ll be armed with step-by-step workflows that you can immediately implement in your business operations.


*Note: These workflows were shared in a recent webinar featuring Hannon De Palma CEO Christopher Sands and Quo co-founder and COO Daryna Kulya.*[Sign up for our upcoming events](https://www.quo.com/webinars) *to stay informed.*


##


**Level 1: Get call insights with a single prompt**


At Level 1, you’re just getting started with Quo and Claude. Connect Quo to Claude, then ask Claude any question about your Quo data.


This is the foundation for more advanced levels. Once you can pull insights with a single prompt, you can layer on more sophisticated workflows.


### **How to connect Quo to Claude**


1. Open Claude and go to **Settings** .
2. Click **Customize** and **Connectors** , then **Connect your apps** .
3. Search for **Quo** .
4. Click **Connect** on the Quo connector.
5. **Sign in to your Quo workspace** when prompted.
6. **Authorize Claude** to access your Quo data.


*Note: You can choose permissions for each tool in your Claude connector for added customization. Select from* ***Always allow*** *,* ***Needs approval*** *, and* ***Blocked*** .


After you’ve connected Quo to your Claude account, you can get insights about your call and text data instantly. Understand where your team may be falling short, get coaching notes for individual reps, and build scoring rubrics for your team.


Wondering how effective Claude can be for your team? Hannon De Palma CEO Chris Sands asked Claude to analyze his star rep Karen’s conversations and find out what makes her so effective. Claude analyzed 400+ call transcripts across six months, then synthesized the patterns that led to conversions.


“410 transcripts analyzed in less than 15 minutes, created this beautiful report that we can hand to our other two salespeople and to future salespeople,” says Chris. “A massive data set that would have taken months.”


Here are a few prompts to get you started:


*“Are there any prospects who mentioned concerns and didn’t convert?”*


*“Are there examples of times when reps effectively overcame objections?”*


*“Analyze the transcripts from Karen’s liaison phone number to find out why she’s so effective.”*


For more prompts you can use as a starting point, check out our list of[23 Claude prompt examples](https://www.quo.com/blog/claude-prompt-examples/) .


##


Level 2: Build recurring reports with Claude Cowork


Single prompts are powerful, but you don’t want to type the same question every day. That’s where Claude Cowork comes in. It lets Claude automatically get the same information on a recurring basis without needing to prompt it each time.


For example, the Hannon De Palma team records every sales call across their three client liaison phone numbers. Then they have Claude produce a daily report of every sales conversation. The head of sales reads it each morning and can spot patterns across the three salespeople without having to check in with each one individually.


At Quo, Daryna has a daily call review report that she uses Cowork to run. She has a custom scoring rubric, developed with Claude, and she uses it to review sales calls with Quo customers. She also connected Quo’s Slack and Notion workspaces to Claude. Claude runs this scoring workflow every morning and sends a custom report to the Quo team in Slack and Notion.


### **How to build a recurring report in Claude Cowork**


1. Open Claude and switch to **Cowork** .
2. **Upload any supporting files** Claude will need to run the report. For a call review, this might be a scoring rubric or a list of definitions for what counts as a qualified call.
3. **Write your prompt.** Tell Claude what data to pull from Quo, what to do with it, and what format you want the report in.
4. **Review the first run.** If the output isn’t quite right, ask Claude to adjust the format, the level of detail, or how it groups information.
5. Once you’re happy with the report, ask Claude to set it up as a recurring task. Specify the cadence and where you want it delivered.


- Connect tools like Slack and Notion from the **Connectors** menu, similar to how you connected your Quo account.


6. Check the **Scheduled tasks** section in Cowork to confirm the task is live.


**💡Pro Tip: Download Daryna’s daily call review plugin**


Want to start scoring your own calls and sending a daily report to your team?


[Here’s the plugin](https://links.quo.com/e/c/eyJlbWFpbF9pZCI6ImRnVC00UVFEQU5Iajl3SFA0X2NCQVo0c1hkRWF3UV9hQUFfRGZBNjdjZz09IiwiaHJlZiI6Imh0dHBzOi8vY2RuLnF1by5jb20vcXVvLWRhaWx5LWNhbGwtcmV2aWV3LnBsdWdpbj91dG1fY2FtcGFpZ249Q2xhdWRlJTJGTUNQK1dlYmluYXIrRm9sbG93LXVwKyU1QkF0dGVuZGVkJTVEXHUwMDI2dXRtX2NvbnRlbnQ9Q2xhdWRlJTJGTUNQK1dlYmluYXIrRm9sbG93LXVwKyU1QkF0dGVuZGVkJTVEXHUwMDI2dXRtX21lZGl1bT1lbWFpbF9hY3Rpb25cdTAwMjZ1dG1fc291cmNlPWN1c3RvbWVyLmlvIiwiaW50ZXJuYWwiOiJmZWUxMDQ4NzAxZmY0YWQxZTNmNzAxIiwibGlua19pZCI6MTM1MDV9/66c21da2b636b0eb64d0d69d492fd3162eea95831acb16200828c5e00d6f5054) Daryna uses to score sales calls with Quo customers.


To install the plugin:


1. Go to


**Claude** and


**Cowork** .


2. Select


**Customize** .


3. In Personal plugins, click the


**+** button.


4. Select


**Create plugin** and


**Upload plugin.**
5. Drag and drop the plugin file.


##


Level 3: Generate custom proposals with Projects in Claude


For service businesses, sending a proposal, a quote, or an estimate is an important step toward closing deals. But proposals take time to write, and most small businesses don’t have a dedicated proposal person on staff.


Chris Sands faced a similar issue at Hannon De Palma. He turned to Claude to create proposals for his clients. Now, he’s able to send custom proposals to each client within an hour of speaking to his team over the phone.


To achieve this, Chris uses Claude’s Projects feature. Projects allow you to create self-contained workspaces with their own chat histories and knowledge bases. Within each project, you can upload documents, provide context, and have focused chats with Claude. The advantage you get with each project is its own memory; Claude can reference project files and past chats to provide effective responses.


### **How to build a proposal generator with Projects in Claude**


1. Open Claude and click **Projects** in the left navigation.
2. Click **Create a new project.** Name it something clear, like “Proposal Generator.”
3. Turn on **visibility** for your team so everyone can use the same project.
4. In the **Files** section on the right side of the project, upload two files:


- **Agent instructions.** A markdown file with your rules, such as how to synthesize the call data, what tone to use, and any guardrails. For Chris’s team, one of those guardrails is “do not give any legal advice.”
- **Proposal template.** A markdown or Word file showing how you want the final proposal to look. The more specific, the better. If you need help writing either file, ask Claude to draft them for you in a separate chat first, then upload them.


5. **Test the project.** In the project chat, run a prompt like, *“Pull the most recent call transcript from \[client name\] on our sales line and generate a proposal for them following the template.”*
6. **Refine the instructions and template** until the output is consistent.
7. **Share the project with your team.** From here, anyone can open the project, run a proposal for a new client, and update existing client proposals as new calls come in.


##


**Level 4: Automate follow-ups with your leads**


In the first three levels, we covered workflows that handled individual tasks with Quo. In the final level, we’ll show you how you can use Quo alongside your CRM and Slack.


Following up with your leads is a tedious manual process that can add hours to your team’s workload. Claude connects all your tools to make it as easy as writing a single prompt.


That’s how the Hannon De Palma team follows up with their leads. They store their leads in a massive Airtable database. While they used to manually follow up with leads in Quo, they now automatically send follow-ups with Claude. All they need is one prompt:


*“Look in Airtable, find all the leads with the status ‘Booked Consult,’ send them a Quo SMS asking them to fill out an intake form, then send me a Slack message with a list of the people you just texted.”*


Chris’s team also uses Quo call transcripts to update their Airtable records with the latest information after every call, so the CRM is always current.


“Without any programming, without an engineer, everybody can become a builder in your company,” says Chris. “As long as they can talk to Claude and figure out what they want to connect.”


### **How to chain your CRM and Slack to Quo through Claude**


1. **Connect Quo, Airtable, and Slack to Claude.**


1. Go to **Customize** and **Connect your apps** .
2. Select **Quo** , **Airtable** , and **Slack.**
3. Log in to your accounts in each of these platforms.


2. **Decide on a workflow.** Start with something small, like Chris’s booked consult example. Write the workflow as a single instruction that mentions every system involved.
3. **Test the prompt in Claude.** For example, *“In Airtable, find every lead with the status ‘booked consult’ from this week. Send each one a Quo SMS message with a link to our intake form. Then post a summary of who you texted to our #sales Slack channel.”*
4. Once the workflow is working reliably, **move it into Claude Cowork** as a recurring task. Schedule it to run daily or weekly, based on your team’s cadence.
5. **Layer in a second workflow** that updates your CRM from Quo. For example, *“After every call on our sales line, pull the transcript, extract the key client details, and update the corresponding Airtable record.”*


##


**Start building Claude workflows for your team today**


With Claude, small teams have access to enterprise-level intelligence at a fraction of the cost. Analyze your calls, send texts, run daily reports, and more. Accelerate your operations in minutes.


Hannon De Palma went from processing five to 10 new leads a day in one state to processing 30 to 50 new leads a day across three states. Workflows like these are part of how they got there.Find out the other strategies they use to scale their client relationships. Check out our[legal client relationships playbook](https://www.quo.com/blog/legal-client-relationships/) from Hannon De Palma CEO Chris Sands.
