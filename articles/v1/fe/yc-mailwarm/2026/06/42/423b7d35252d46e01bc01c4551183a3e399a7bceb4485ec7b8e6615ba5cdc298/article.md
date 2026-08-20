---
schema_version: "1.0.0"
document_id: "423b7d35252d46e01bc01c4551183a3e399a7bceb4485ec7b8e6615ba5cdc298"
company_key: "yc-mailwarm"
company: "Mailwarm"
source_id: "yc-mailwarm-news-import-5814b5ef4890"
canonical_url: "https://www.mailwarm.com/blog/how-to-avoid-landing-in-spam-full-guide"
published_at: "2026-06-18T17:07:52.608+00:00"
first_seen_at: "2026-07-22T03:09:39.845230+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:eab46a06054c7812e8368b6f3bd0ec5c387087a873fcc65ad5d6591f1d20f63d"
---

# How to avoid landing in spam (2026 full guide).

You may have read tons of articles related to **spam filters** , **domain reputation** ... You may have followed every step to write proper email content… But, the internet doesn’t care and your emails still get stuck in this **junk folder** . You’ll find here a complete guide to help you reach the precious **Inbox folder** and finally get your emails read!


Before going deep into the solutions and talking about obscure acronyms such as ISP, ESP, DKIM and so on, let’s face the REAL problem:


### You land in spam, because you’re spamming.


Boom.


Well, you might tell me «Gonzague, I didn’t come here to suffer, okay? ». Sure, we’re done, that was the hardest part ! That being said, this will reset your brain to start off a fresh new basis.


> Learn the spam rules, bypass them.


**Spam folder** probably already prevented you from frauds, scams or phishing. I guess you may easily detect these emails by yourself, but your parents or children probably won’t! This **junk folder,** supported by **spam filters,** is a key element to maintain a well-balanced email world. In 2019, more than 56% of the global email traffic was spam, hopefully spam filters do exist, right?


****


In order not to land in spam, you just need to be part of the 44% others. To achieve that, there’s only ONE SINGLE rule: ****


## A. Make your email account act normal.


Acting normal means you should make your emails look like it’s handled by yourself and not by a mass mailing machine (aka **ESP** (Email Sending Provider): Mailchimp for instance).


Keep in mind that the spam filters are algorithms, machines, robots and technical systems. There’s no feeling nor emotion involved that might come from an “Email-Spam-Officer”. If your **emails go to spam** , it’s only by your fault: you’re not playing the email game right. And that’s good news, because it means you can observe spam filters behavior, understand and bypass them.


These spam filters are algorithms that check the well-known **reputation score** of your IP address (the identity of your email account). If it’s too low, welcome to the spam folder!


## How do spam filters control your reputation score?


First, let’s have a look at your emails’ journey. Ready? Go!


Each time you send an email, it leaves your account from your **ISP** (Internet Service Provider) like Gmail, through your **SMTP** (Simple Mail Transfer Protocol). The email goes to the receiver’s ISP through its IMAP (Internet Message Access Protocol). Spam filters are located in the ISPs.


To decide whether your email gets blocked or your IP and domain gets blacklisted, ISPs refer to 2 types of control authorities:


- **Blacklists** : which basically are databases that store IP/Domain spammers. There are a lot of them and their criteria are specific. Just keep in mind that they mostly refer to your **email sending activity** : sending frequency, sending volume, bounce rate, spam reports **…;**


- **Spam filters** : which monitor the content of your emails. Those are software used by ISPs that check the email subject line, content, picture and links. If it’s potential spam (according to their point of view), they notify the ISP about it through a spam score. Hold on, this is not your final reputation score!


Now that your ISP knows everything about your IP, domain and email content, it will play the judge role and will claim the sentence:


## Your **reputation score** .


Or at least your instant reputation score. Why ?


Because ISPs run a high-speed justice. Your email might go to jail (Spam folder) and get back to freedom within the same day! That’s good news for you.


✨


If you are currently reading this article in a bad mood as your emails are SPAM, it is the moment you see a spark of hope, right?


And you got it well :


> You won’t keep landing in spam for the rest of your (IP) life, what an “IP” end…!


In less than 24 hours, your emails may find the inbox track again!


Actually, your reputation score is given by ISPs. The score is based on blacklist sources and spam filter results, which involve a bunch of variables that track each email you send. This means that your reputation score is fast-changing.


On the other hand, avoiding the junk folder is never a permanent win. In fact, it’s an everyday battle.


‍


### 1 ISP = 1 reputation score policy.


Each ISP sets its own rules. It means your IP might be blacklisted by one ISP and not from others, which also means that those algorithms are changing aaaand which also means that:


## The ultimate checklist to avoid the spam folder doesn’t exist.


Well, your reputation score is a changing value, not only depending on your actions but also on the ISPs your emails go through (Gmail to name one, if you’re a bit lost) and their own algorithms.


This reputation score is not only bound to your IP (which refers to your email address) but to your domain too. A bad reputation score has an impact on your whole domain reputation.


> Okay that’s enough theory, let’s talk about actions.


Before diving deep into how you should use your email account, you need to make sure it’s set right:


‍


# B. What should you do before sending any email?


You came here, to get results, right? Follow this, step by step.


## 1. Keep your main domain name safe:


Please don’t use your website domain name for email marketing purposes, unless you’re sure about it. Look at the newsletters you receive, you might notice that most companies use subdomains, other extensions (com, io, co…) or various domain names to prevent affecting their main domain from one single IP bad reputation score.


### A bad reputation score will get your domain blacklisted (you don’t want that).


If your **domain gets blacklisted** , you have to follow a time consuming and uncertain process to get back in the game. Well, you don’t want to face this, so keep your website/product domain name safe.


If you want to check whether your domain is blacklisted or not, you can have a look to[this great tool](https://mxtoolbox.com/blacklists.aspx) or[this one.](https://www.ultratools.com/tools/spamDBLookupResult)


> Anyone can send an email with your address as a “from”. That’s a fraud, and that’s called: spoofing.


## 2. Configure your domain name:


You should always **authenticate your email account** , which will send ISPs two positive signals: “I agree I’m the one who sent this email" and "the message isn’t altered”. Here are the three parameters you have to set:


### Set your SPF


**SPF** stands for Sender Policy Framework. It’s a small text (a record) that defines who’s (which IP) authorized to send an email from the DNS (domain name server, what comes after the @ of your email address). This allows the receiver to authenticate your email and certify you’re the **original sender** .


****


If you’re using Gsuite, you can follow[this tutorial](https://support.google.com/a/answer/33786?hl=en) :


If not, search in Google : “{your domain provider} SPF record” and follow a step-by-step tutorial.


Then verify it’s properly set with this[spf check tool](https://mxtoolbox.com/spf.aspx) .


****


> SPF authenticates the bound between **your IP and your domain** .


### Set your DKIM


**DKIM** stands for Domain Key Identification Mail. It is an **authentication standard** which guarantees that your message has not been altered and that it was sent from your domain. It basically associates your email to your domain name in a hidden signature located in the header of your email. The authentication is possible thanks to a DNS (domain name server) record that provides an encryption key.


If you’re using Gsuite, you can follow[this tutorial](https://support.google.com/a/answer/174126?hl=en) :


If not, search in Google : “ {your domain provider} DKIM record” and follow a step-by-step tutorial.


**Then verify it’s properly set with this**[dkim check tool](https://mxtoolbox.com/dkim.aspx) **.**


> DKIM authenticates the bound between **your email and your domain** .


And what if SPF and DKIM would collaborate...? Boom: DMARC.


### Set your DMARC


**DMARC** stands for Domain-based Message Authentication, Reporting and Conformance. It basically unifies SPF and DKIM and enrich them by providing :


• **Spam reports** : about emails that have been blocked and the reasons of this issue;


• **Spam policy** : explaining what to do with emails that didn’t pass authentication checks;


• I **dentity alignment check** : actually the email identity of the sender may change between: your FROM address, the DKIM signature and the “return-path address”. DMARC takes the FROM address field as a reference, since it’s the one that will be read by a human. It will take into account DKIM and SPF authentication test results, ONLY IF they run their authentication process based on the same email address.


If you’re using Gsuite, you can follow[this tutorial](https://support.google.com/a/answer/2466563?hl=en) :


If not, search in Google : “ {your domain provider} DMARC record” and follow a step-by-step tutorial.


Then verify whether it’s properly set with this[dmarc check tool](https://mxtoolbox.com/dmarc.aspx) .


Now you get an idea of what’s happening with your emails going through ISPs, spam filters, blacklists and how you should prepare your email account for the war…


Let’s see how to end up victorious and keep your **IP reputation** high.


# C. The HCA Framework.


To sum up, you’re doing spam in ISP eyes, because normally your account should act like a human. BUT sending hundreds of emails each day doesn’t seem to fill this requirement. And don’t forget that there’s no ultimate checklist as spam filters rules are customized, obscure and changing… Well, let’s analyze the situation.


In order to make your email account “act normal” and **avoid landing in spam** , I used to say to my clients that they needed to take into account three parameters; the HCA Framework:


• **History** of their IP/domain


• **Content** of their emails


• **Activity** of their account.


Every single tip you’ll read on this topic is related to one of these key elements: spam wording, warm-up process, daily sending volume, ESP daily limits, IP reputation, reputation score… That’s enough buzzwords for today, you got it and they all fit in these three sections.


If you have the right mindset and keep this HCA in mind, you’ll make the right decisions regarding your IP domain and will see your **reputation score rising** to the moon and beyond or at least getting better.


Okay, give me an H! Boom, History.


# 1. HISTORY: How does your domain and IP history affect your reputation score?


Why are you here, reading this article, working for this company…? Because of the sum of the actions you previously did in your life. Your email account works the same and its history explains its current reputation score.


Should you read the whole biography of your email account? Well, hopefully not. Just ask yourself: When was the last time I’ve used my email?


If it’s more than 1 month:


Your IP reputation got reset to its lowest rates. Thus, you should treat your email account like it’s a new one: veeeery carefully.


Your IP is new but your domain is active through other IPs:


This is the case when you join a company that already has a sending history. For instance, Jeff Bezos children benefit from the actions of their father, that’s the same for your email IP. You’ll benefit from the current domain (what stands after the @ in your email address) reputation aaand (be careful) your future actions will impact the **domain reputation** too. Family affair.


You frequently use your email account, but you notice that your emails are in SPAM:


**‍** You should start from zero and monitor your **deliverability rate.** Send 10, then 50, then 100/day. Once you notice your emails land in spam, lower the volume until 100% deliverability and keep it steady for a few days to sustain and reinforce your **sender reputation** . If you don’t go out from spam, you may be blacklisted and should go through a process to get your address out of it.


> Take your email account history as a referring point to start at.


If your email already sends dozens of daily emails, you slightly can increase the **sending volume** . It’s recommended not to exceed x1.5 per day: if you’ve sent 100 emails yesterday, you can increase up to 150 today.


Always track your deliverability rate: if you send emails to verified addresses, but still get 3% **bounce rate** (it’s way too high), you should decrease your daily sending volume.


Well, unfortunately you can’t do anything about your history as by nature it refers to the past. You just have to get this in mind and not follow pre-made **warm-up schedules** that would be inappropriate to your specific situation.


# **2. CO** NTENT: How to write the best email content to avoid landing in spam?


First of all, don’t think about **avoiding spam** or not while writing your emails. Just, write down your marketing email to your leads, as if you were talking to them personally.


I recommend you to imagine a single person (not a crowd) to whom you explain how you can provide help and deliver value. Just like I’m doing here, talking to you as you were sitting in front of me.


> Write your email to one person THEN send it to thousands.


Writing your email this way will avoid sounding like a pushy salesperson. This will naturally keep you **away from spam words** AND catch the interest of your leads.


## Your email content should **avoid** :


• **Any spam words** : You basically should avoid any promotional terms or expression : “amazing offer for free” won’t sound amazing for spam filters. This[Mailshake tool](https://mailshake.com/email-analyzer/) analyzes the content of your emails.


‍


• **Capital letters** : DON’T WRITE THIS FOR INSTANCE.


• **Excessive punctuation marks** : you should avoid this !!!!!


• **Attachments:** Especially .exe or zip files, as it’s a common practice to hack a computer. Common text, image or document files are ok, just keep in mind it’s a spam filter parameter.


• **High image/text ratio** : your email shouldn’t be covered by more than 40% images (and should provide at least 60% text content). This is not an absolute value as ISPs set their own rules regarding this ratio. But, keep in mind that you should have much more text than image coverage.


**** • **Text on images** : Your images shouldn’t get more than 20% of their surface covered by text. Spam filters don’t like it as this method is often used for fraud purposes. Facebook provides a[tool to check text on image](https://www.facebook.com/ads/tools/text_overlay) ratio for FB ads. You can use it for emails as well, because they require the same ratio.


Following these few rules will guarantee you to pass spam filters related to content. Providing good content will also raise your **reply rate,** which will have a good impact on your email activity…


Following these few rules will guarantee you to pass spam filters related to content. Providing good content to your[cold email outreach](https://wiza.co/blog/30-cold-email-tips-to-increase-responses-roi/) will also raise your **reply rate** , which will have a good impact on your email activity…


Email activity ?


It's the third pillar of the HCA Framework, let’s talk about it.


# 3. ACTIVITY: What’s the best sending activity for your email account?


Email activity gathers all the actions that are related to your email account. Here are the three **sending activity patterns** you should follow to keep your reputation score high:


## a. Warm-up your email account:


You always need to start your email sending activity with the so-called **email warm-up** phase. It consists in gently increasing your sending volume, with a consistent growth to reach your target daily frequency.


> Don’t be in a hurry.


You may have seen a lot of articles talking about sending thousands of daily emails. The rule is to always remember your history. Don’t feel ashamed about sending 20, 50, 100 emails a day, especially if you’re doing **email cold outreach** : cold leads are more eager to not open your emails and mark you as spam. Situations are totally different whether you’re cold-mailing, sending a newsletter or sending dozens of transactional emails (automated emails like email confirmation, billing receipts…).


If you’ve not used your email IP & domain for a while, and start sending 50 emails day 1, you probably will land in spam and get a bad IP reputation score. Whether it’s a new email account or a static one for more than 30 days, warm it up.


### How to warm-up your IP?


You should always gradually raise and decrease your sending. According to this golden rule: if your email address (domain or IP) is new, you should start with a few emails the first day, and increase your sending everyday a bit more. Each situation is unique and depends on how your recipients behave toward your emails (replies,deletes...). The rules are not strict but the more time you take to increase the sending, the better it is for your reputation.


During the warm-up phase, avoid getting any **spam complaints** . Your reputation, at this stage, is still very sensitive and any complaint will drastically impact your score.


> A warm-up is necessary but not enough.


Do athletes warm-up once, then tick the box “warm-up done”, next step: Olympic Games!? Well, of course they warm-up EVERY SINGLE DAY. They do it so to perform with warm muscles and so should your email account behave: warm, always.


Here’s how to achieve this.


## b. Linear activity:


You may have heard about warm-up here and there: how to do it, why, which schedule (which is different on every article…) etc... But, have you ever heard about “email cool-down”?


Well, me neither ahah.


Just talking about “email warm-up” pretends it’s a one shot miracle action that keeps you away from spam. I guess you guess: it doesn’t.


### The email cool-down effect.


If you cook a meal, then wait too long, it will cool down. Clap clap, Captain Obvious. Actually, your email account works exactly the same! As written previously, **ISP spam filters** are running fast and often. So, if your email account activity pauses, your **IP will cool down** and this will be taken into account for its next reputation score calculation.


> You should keep your email account active every day.


This results in a **linear email activity** . I noticed it with my own emails accounts in the past and it was one of the main purposes why we implemented our own[warm-up tool](https://www.mailwarm.com/) .


You probably prepare your marketing or **cold-outreach campaign** , send emails, track the metrics, analyze results, try out new subject lines, send again, monitor the evolution, brainstorm, find new leads etc... Sounds quite accurate?


Well, the problem is it doesn’t maintain a consistent IP activity, but instead generates sending peaks like:


• Day1: 100 emails


• Day 2: 0 emails


• Day 3: 0 emails


• Day 4: 200 emails


etc.


This is considered as a spam activity. ISPs don’t hit the like button.


### How to reach a linear activity?


You should keep your sending volume as flat as possible by keeping it steady on the long run. If you send 200 emails every two days, you should flatten this by sending 100 emails per day.


## c. Positive activity ratio:


Actually is a score combining both positive and negative actions the receiver might do with your email:


‍Positive actions:


- **open it**
- **read it**
- **reply to it**
- **mark it as important**


Negative actions:


- **unsubscribe**
- **delete it**
- **flag it as spam**


Spam filters adjust your reputation score according to these behaviors. Remember spam filters are algorithms, so they look at human reactions to draw their conclusions.


Some passive events may have an impact on your positive email activity:


- **Bounce** : this mainly occurs when the email address you want to reach doesn’t exist. It’s a bad indicator for your IP as it means you don’t know who you’re sending your emails to and you probably are a spammer.
- **Spam traps** : ISPs and spam filters providers create emails addresses and spread them all around the world (wide web). Those are meant to detect spammers. If they receive an email from your IP, you instantly get blacklisted. Ouch.


### How to reach a **positive email activity** ?


#### Build a qualified leads list:


Not here to do some “opt-in” bashing, BUT you always need to make sure you send emails to people that are interested in your content. This will prevent your emails from being deleted or **marked as spam** . Even a few spam complaints are too much, you really should aim at the 0 spam complaints goal as it heavily weighs on ISP **reputation score calculation** . So, source your leads properly (choose your own definition of it), segment your campaigns and custom your messages.


> Opt-in doesn’t mean spam invincible nor bounce free.


#### Verify EVERY SINGLE email address:


Even though your leads are opt-in. Have you ever put a random email address to get that ebook for free? Boom. You just add an opt-in email to the list, that will cause trouble to the sender, if it doesn’t get verified. Oooh that’s bad. Actually, sending an email to a non-verified address will raise your bounce rate and it’s a strong negative signal for spam filters. You may use an email verifier like[Clearout,](https://clearout.io/)[Hunter.io](https://hunter.io/email-verifier) or[AeroLeads.](https://aeroleads.com/blog/bulk-email-verifier/)


#### Write **** catchy **** subject **** lines **:**


So people will open your emails. You should follow the same rules as the Content topic. This[coschedule tool](https://coschedule.com/email-subject-line-tester) does that well.


#### Provide value to your leads:


So your emails will be read and hopefully marked as important/starred. That’s even more important for repetitive emailing such as newsletters. If it doesn’t bring any valuable content to your customers, they will end-up trigger a negative activity and lower your IP reputation.


You can bring info, statistics, a tip or your help for a specific need. Don’t talk about your product: just pitching your product/service is not bringing value. You must write about the value you’re bringing, which should describe the new situation the person will experience by using your product (whatever it is). **** For instance: doing a specific task 3x faster.


#### Add a call-to-action that involves a reply:


To do that, simply ask a yes/no question. **** The less engagement (reading time, thinking, decision and answer) it requires, the more you'll get replies. A reply is great from the **avoid spam box’s** perspective, but also from a business perspective as it's a lead-to-customer conversion opportunity.


‍ **‍**


#### BONUS : Turn negative actions into positive ones.


Follow-ups emails are more eager to generate an action from your leads, whether it be positive or negative as the feeling will intensify over the emails: whether it be interest or anger. If they’re bored, they won’t hesitate to **flag your emails as spam** or **hit unsubscribe** . But you can turn a negative action into a positive one with a little trick.


#### The “No-Thanks” trick


In your second or third follow-up email, expressly ask your lead to reply “No thanks” if not interested by what you’re asking/offering. Less people will unsubscribe or flag you, which is great, and more people will reply (that’s even greater).


On a business side, this also may trigger a discussion with your lead. If some explanations are given: “No thanks, I’m already using Y..”, and you could reply : “Oh but we’re quite different from Y because our solution is more this and this”. So Give it a try !


> Positive activity is a balance


Notice that we’re talking about a **sending activity ratio:** 10 spam complaints would have no impact on your reputation score against 500 replies. This means you should do your best, implement all the previous actions without going crazy about each one.


# D. Sum-up & how to automate this ?


Following all these tips will bring you back to inbox road .


I guarantee you that you won’t see your emails get flagged as spam anymore as you’ll play by spam filters rules. But as you can feel it, that’s not easy to do :


• Set up your email account


and


• Schedule a warm up


and


• Get a linear sending activity: okay it's getting less easy


and


• Get your emails opened: you don't have full control on this...


and


• Get replies: even less control on this one.


and


• Get your emails marked as important: does anyone really hit the star button ?!


and..


• Don't get spam complaints nor deletes and email unsubscribes: finger-crossed !!


and….


• Reach 0% bounce rate: let's roll the dice ?


> It seems like standing in front of Everest North Face, right ?


Well, at least that’s what my cofounder and I experienced.


That’s why we've built an automation[tool to warm-up](https://www.mailwarm.com/) , maintain a linear sending and reach a great positive activity ratio. The goal is to start focusing more on our valuable tasks. In fact, taking all this into consideration is time consuming. We offer you a service that you definitely should have a look at if spam is an issue for your business.


Thanks for reading, I hope you’ve learnt at least one thing that will help you in your business. If it’s the case, I’m glad about it. Don’t hesitate to share it with a coworker, customer or a friend.


Cheers and good luck with your email campaigns!


Gonzague
