---
schema_version: "1.0.0"
document_id: "7835e685c9f7c2d30c44dc3b22db20f54bc1e962a911d2cf7d0f37e34b10d456"
company_key: "yc-openphone"
company: "Quo (fka OpenPhone)"
source_id: "yc-openphone-news-import-b4955105a2fb"
canonical_url: "https://www.quo.com/blog/claude-prompt-examples/"
published_at: "2026-05-11T09:55:20+00:00"
first_seen_at: "2026-07-22T10:45:40.935626+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:f21022a0bec42c74ac49f72cb86acc1ce499e837be5713c6e0b548f7ac2e7321"
---

# 23 Claude prompts to get more out of every conversation in Quo

Every week, you rack up dozens of customer conversations in Quo. Most of them never get reviewed. Quo’s Claude connector fixes that.[Connect it](https://support.quo.com/core-concepts/integrations/mcp) once, and you can ask Claude to spot patterns across your calls and texts, draft replies, coach your team, and clean up your contacts. If you can ask the question, Claude can work with it.


Here are 23 prompts you can use as a springboard to get started.


##


**Easily review insights**


Claude can go through weeks of your calls and texts in seconds. These prompts ask the right questions to get useful answers back.


### **1. Surface weekly trends across your team**


*Pull conversations from the last seven days on \[my business phone number\]. Identify the top three objections that came up and how our team handled them. Share the top three answers for how customers heard about us.*


Run this every Monday, and you’ll see where customers are finding you — and how they’re pushing back.


### **2. Find out why you’re losing deals**


*Fetch the calls tied to \[my business phone number\], along with the most recent text conversations from that same number from the last 30 days. From there, identify the main reason leads choose to not move forward with us.*


Claude can pull up to 100 calls and 100 texts per prompt, but you don’t need to max that out. It can pull more in multiple batches if you ask Claude to review calls and texts based on a specific timeframe. The point is to give Claude enough data to spot a pattern. For a solo operator, that can be 25 calls and texts. For a team, you likely need more.


### **3. Ask Claude what you’re missing**


*I run a residential cleaning company with four employees. We use Quo for all customer calls and texts. Based on my last 30 days of calls and texts, which patterns should I be paying attention to that I might be missing?*


This is a good prompt when you’re not sure where to start. Share the context, and let Claude tell you what deserves your attention.


### **4. Surface the top customer complaints**


*Pull my last 100 messages on \[my business phone number\]. What are customers complaining about the most? Group the issues by theme and tell me how many times each came up.*


Grouping complaints by theme shows what’s bothering customers the most. Is it the bill coming in higher than the quote or a tech who didn’t show up on time? Fix the problem, and you’ll save yourself dozens of follow-ups.


### **5. Spot call-handling inefficiencies**


*Pull my last 30 call transcripts on \[my business phone number\]. Flag any calls where the customer had to repeat themselves, the conversation went in circles, or the issue could have been resolved faster. What patterns do you see?*


Find the calls that went off track and what they have in common. It could be the process slowing things down, not the rep.


##


**Coaching sales calls**


You can’t sit in on every sales call. These prompts pull from your team’s transcripts to flag anything that’s worth a closer look.


### **6. Score a single sales call**


*Pull the transcript from my last call with the lead at \[customer’s phone number\]. Score the call on a 1–10 scale for: greeting and rapport, asking the right questions, handling pushback, and setting clear next steps. Give me one thing I did well and one thing I can improve on.*


Scoring on the same four criteria every time gives you something to compare against week over week.


### **7. Self-coaching**


*Pull my last call transcript on \[my business phone number\]. Did I clearly explain our pricing? Did the customer seem hesitant? Were there moments where I deflected instead of giving a direct answer? What should I say differently next time?*


Pricing is where many folks lose their nerve on the phone. This reviews how you handled it and tells you what to say differently.


### **8. Find missed upsell opportunities**


*Pull the last 20 call transcripts on \[my business phone number\]. Did any caller mention something they needed — a different service, an add-on, a second job — that we didn’t follow up on? List each one with what they said and what the opportunity was.*


A homeowner calling about a roof repair mentions their gutters are in bad shape. The conversation moves on without acknowledging it. That’s a missed opportunity you’d never catch without going back through the transcripts.


##


**Coaching customer service teams**


If your reps are handling customer issues across calls and texts, these prompts let you keep tabs on quality without sifting through every conversation.


### **9. QA review across recent conversations**


*Pull the last 20 conversations on \[my business phone number\]. For each one, rate whether the team member resolved the issue, kept a professional tone, and sent a follow-up message or scheduled a next step — yes, no, or partially. Then summarize any patterns across the batch.*


You’ll get a snapshot of what’s working across the team without having to listen to dozens of calls. Run it weekly, and you’ll get a handle on existing issues before they pile up.


### **10. Find repeat callers and resolution gaps**


*Pull my last 100 calls on \[my business phone number\]. How many people called more than once about the same issue, and what was the issue?*


If the same person is calling three times about the same thing, that could be a process problem, not a customer problem. This prompt spots those patterns so you can fix what’s causing them.


### **11. Write a coaching note from a specific call**


*Pull the transcript from my team’s last call with the customer at \[customer’s phone number\]. Tell me what happened, what the rep did well, and what they could do differently next time. Include a script they can practice with.*


The practice script gives your rep something actionable to rehearse before their next call, rather than just feedback.


##


**Follow up faster, without starting from scratch**


Following up takes time you don’t always have, and slow replies could cost you customers. These prompts turn calls and texts into replies, briefings, and reminders.


Keep in mind with the Quo Claude connector, you can ask Claude to send a text based on some of the prompts below. This action uses[pre-paid credits](https://support.quo.com/core-concepts/administration/billing/credits-and-usage?_gl=1*8dmm7p*_gcl_au*MTAxNTUzMzMxOS4xNzc1MDY1OTgz*_ga*NzQ4MzczMDAxLjE3NzUwNjU5ODM.*_ga_WX1XYTKSR5*czE3Nzc1Njc5MTUkbzIkZzAkdDE3Nzc1Njc5MTUkajYwJGwwJGgw) .


### **12. Get a daily or weekly briefing**


*Pull my calls and messages from \[my business phone number\] for the past week. Give me a summary that includes: how many calls, how many texts, how many people who asked for a quote vs. last week, and any conversations that still need follow-up.*


Run this prompt every Monday morning. It’ll save you the 20+ minutes you’d spend going through your inbox trying to remember where you left off. For incoming voice messages, texts, and missed calls, you can set up[auto-replies in Quo](https://support.quo.com/core-concepts/ai-automations/auto-replies) , so customers get a text back automatically, no prompt needed.


### **13. Catch leads who haven’t received a reply**


*Pull my last 20 messages on \[my business phone number\] and flag any new leads that haven’t received a reply yet.*


This prompt helps make sure a lead’s incoming text message doesn’t slip through the cracks on a busy day.


### **14. Turn a call transcript into a follow-up text**


*Pull the transcript from my last call on \[my business phone number\]. Draft a follow-up text that recaps what we discussed and confirms any next steps we agreed on.*


After a call, you don’t always have time to write a recap. This drafts it for you to review and send.


### **15. Batch follow-up on unanswered leads**


*Pull my recent messages from \[my business phone number\]. For anyone I texted in the last 24 hours who hasn’t replied, draft a short, friendly follow-up.*


This saves you from writing follow-up texts one by one when leads have gone quiet. If you’ve got a long list, ask Claude to keep going if it stops short. It can pick up where it left off.


### **16. Structure a qualified lead from a call**


*Pull the transcript from my last call on \[my business phone number\]. Pull out the caller’s name, the service they need, their timeline, their budget if they mentioned it, and whether they’re the decision-maker. Format it as a clean lead summary I can copy into my CRM.*


You won’t always have time to write a clean lead summary right after the call. This pulls the qualifying details into one place, so they don’t get lost between the call and your CRM.


### **17. Catch up on a contact before you call them back**


*Pull all my calls and texts with \[customer’s phone number\] on \[my business phone number\]. Give me a quick summary of what we’ve discussed, what they need, and what was last agreed upon.*


This is useful when someone calls back after a few weeks, and you need context fast. Instead of scrolling through the history mid-conversation, you get a one-paragraph catch-up before you dial.


### **18. Address a concern in a follow-up text**


*Pull the call transcript from \[customer name\] on \[my business phone number\]. They liked the plan but were concerned about the timeline. Draft a follow-up text that addresses their concerns and gives them a reason to move forward.*


When a concern comes up on a call, the follow-up is your chance to address it. This prompt drafts that text for you.


### **19. Follow up on a quote**


*Pull my recent messages with \[customer name\] on \[my business phone number\]. I sent them a fee estimate for their \[service\] two days ago and haven’t heard back. Based on our conversation, draft a brief follow-up that’s professional but approachable.*


If you’re dealing with longer sales cycles that need more touchpoints, change the prompt to pull the full conversation history and draft a follow-up that builds on everything you’ve discussed so far.


### **20. Move a verbal ‘yes’ to a booked appointment**


*Pull my conversation with \[customer\] on \[my business phone number\]. They said yes to the \[service, job, etc.\] on our last call, but we haven’t confirmed it in writing. Draft a text to lock in a date, time, and address for this week or next.*


This locks in a date in writing, so it doesn’t slip while you’re juggling a dozen other conversations.


### **21. Send a batch update to customers**


*Pull my text conversations from the last seven days on \[my business phone number\]. Find any where a customer and I agreed on a specific appointment date and time for this week. Draft a short reminder text for each.*


*A reminder text the day before an appointment helps cut down on no-shows. This drafts a week’s batch in one go.*


##


**Keeping contacts clean**


Your contact list is only useful if it’s up to date. These prompts pull updates from your recent conversations and flag contacts who’ve gone quiet.


### **22. Update contacts from recent calls**


*Pull my last 20 call transcripts on \[my business phone number\]. If any customer provided a new email address, phone number, or company name during the call, update their Quo contact information.*


A customer drops a new email address halfway through a call. This catches it before your next invoice bounces back.


### **23. Flag contacts with incomplete records**


*Pull my contacts in Quo. Which ones are missing an email address or a last name? Give me a list of the top 20, sorted by who I’ve spoken with over the last seven days.*


Contacts fill up fast: a lead calls in, you save the number, and the rest never gets added. This surfaces the gaps and sorts by who you’ve spoken to most recently.


##


**Get more out of using Claude with Quo**


Once you’ve got the hang of these prompts, use these tips to get the most out of Quo’s Claude connector:


- **Be specific with your prompts.** The more context you give Claude, the better the answer.
- **Try different Claude models.** Opus is strongest for analysis and coaching, while Sonnet drafts replies quickly.
- **Share your best prompts with your team.** If you already save[snippets](https://support.quo.com/core-concepts/messaging/snippets) in Quo for common replies, shared prompts work the same way.[Share your best prompts](https://support.claude.com/en/articles/10722177-sharing-prompts-in-the-claude-console) so the whole team can reuse them without having to start from scratch.


- **Ask Claude to improve your prompts.** If a prompt isn’t returning quite what you want, paste it into Claude and ask: “How can I make this more specific?” or “What am I missing?” For more on writing effective prompts, check out[Anthropic’s prompting guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) .
- **Automate the prompts you run every week.** If you’re pulling the weekly recap every Monday or running the same QA review every Friday, you can turn those into scheduled skills that run automatically.[Here’s how to set that up:](http://link-to-skills-automation-docs/)
