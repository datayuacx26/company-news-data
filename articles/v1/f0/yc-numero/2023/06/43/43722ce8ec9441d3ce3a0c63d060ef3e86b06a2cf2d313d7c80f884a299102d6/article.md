---
schema_version: "1.0.0"
document_id: "43722ce8ec9441d3ce3a0c63d060ef3e86b06a2cf2d313d7c80f884a299102d6"
company_key: "yc-numero"
company: "Numero"
source_id: "yc-numero-rss-2718f20b56fd"
canonical_url: "https://www.numero.ai/blog/using-action-network-for-fundraising-emails/"
published_at: "2023-06-29T04:00:00+00:00"
first_seen_at: "2026-07-25T16:42:35.962761+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:6b55a2e859e7f350c34fa8bade3fd513cd45896a697306356b783a929c52fe39"
---

# Using Action Network for Fundraising Emails

*This post was originally published by Kalani Tissot on*[his blog](https://www.tissotsolutions.com/post/using-action-network-for-fundraising-emails) *and has been reposted here with his permission.*


**When running for office, you need a powerful targeted mass email service.** At my firm, three of our clients are on Action Network for all of their email fundraising. It’s a phenomenal platform and a great tool for candidates up and down the ballot.


This article is a detailed overview and guide of how to use Action Network for your political campaign.


[Action Network](https://actionnetwork.org/) itself is an online platform owned by a 501c4 dedicated to building tech tools for the progressive moment. Their online technology is used for mass emails, text messages, events, and more. Currently, organizations such as the DNC, AFL-CIO, and Daily Kos use Action Network exclusively for their mass email programs.


Here’s what we dive into:


1. Why you should use Action Network
2. Creating your Action Network Account
3. Building a custom email wrapper
4. Creating an HTML snippet to center your email content
5. Integrating ActBlue with your Action Network account
6. Writing a welcome series & creating an automated ladder
7. Putting together your first email list
8. Best practices for email fundraising


## **Why use Action Network?**


Action Network has competitive email deliverability and customization for campaigns of all sizes. It’s also affordable for low-budget campaigns with paid partnerships starting at just $10 a month. There are also no set up fees or any contracts that you’ll have to sign.


Action Network has:


1. Fully customizable email templates & wrappers
2. Direct integration with ActBlue (New donors will automatically be subscribed to your list)
3. Advanced email targeting & deliverability.
4. Automated ladders that you can use for an email welcome and reactivation series.


First-time candidates with small and medium-sized campaigns won’t be better served by any other platform — including NGP. To use NGP’s email service, the minimum fee is $165/month for an email list of under 500 contacts. With an email list of that size, you’ll save $155 a month by using Action Network instead.


**When to use NGP instead:**


1. You are already using NGP and want a seamless CRM that combines all of your fundraising & email data.


2. You are sending out a high volume of emails, and NGP will save you money.


Here’s the math. Action Network charges $1/thousand emails for a movement partnership. For a 10,000 person email list, NGP charges $225/month. Each send to your whole list on Action Network would cost $10. If you were to send over 23 emails a month, then NGP would be more cost-efficient. At a much bigger scale, say 25,000 email subscribers, NGP will be cheaper for email programs with send rates greater than 12 per month.


## **Getting setup on Action Network:**


Before you set up your account, you’ll need to get your[website domain](https://numero.ai/blog/political-campaign-website-dos-and-donts/) and campaign email address in place.


If you don’t already have a website in place, consider using:[Wix](http://wix.com/) ,[Squarespace](https://www.squarespace.com/) , or[WordPress](https://wordpress.com/) for an easy website design tool that will allow you to build a custom website on the cheap.


For campaigns of all sizes, I can’t recommend using[Google Workplace](https://workspace.google.com/pricing.html) enough. Google charges you anywhere from $6-12/person/month for a professional email address, inbox, and more. The candidate and each campaign staff member will need a professional email address for their day-to-day communications.


For mass emails, it’s a best practice to create a separate info email account that all emails and website messages are sent to. This looks like[\[email protected\]](https://www.numero.ai/cdn-cgi/l/email-protection#fdb4939b92bd8492888f9e9c908d9c949a93d39e9290) say[\[email protected\]](https://www.numero.ai/cdn-cgi/l/email-protection#aac3c4ccc5eae1cbc6cbc4c39ee4cfdccbcecb84c9c5c7) .


Make sure to also set up your own SPF, DKIM, and DMARC records for your email and domain. These records will improve your email deliverability and help your emails hit inboxes, not spam folders. More info on how to do that here:[https://medium.com/@jakebarn/why-your-gsuite-emails-are-going-to-spam-b231caf93355](https://medium.com/@jakebarn/why-your-gsuite-emails-are-going-to-spam-b231caf93355)


Now that you are set up with a custom domain and professional email, you can sign up for your Action Network account here:[https://actionnetwork.org/users/sign_up](https://actionnetwork.org/users/sign_up)


Once your account is set up, you can sign up for their paid movement partnership here:[https://actionnetwork.chargebee.com/hosted_pages/plans/movement](https://nyl.as/t1/184/elwq4obpa5z35kr4ytzmbvioa/1/9fc58c2b44887b73eb322355cd1ed766003bbf89f3df533a5c83e403494c3a7d)


After that, you need to email[\[email protected\]](https://www.numero.ai/cdn-cgi/l/email-protection#6d070204032d0c0e190402030308191a021f0643021f0a) , and they will get your account up and running.


## **Building a custom wrapper for your emails:**


The next step is to build out your email wrapper. This is the logo and footer that go around your email content and include your disclaimer info, an ask to donate, your website link and an additional unsubscribe link.


Here’s where to set this up:


Here’s an example of what an email wrapper looks like:


Email wrappers are built using HTML code and have two parts — a header and a footer. But don’t worry, you don’t need to know how to write HTML to build a beautiful template. You can use an editor that translates text and design into HTML code for you. **Here’s the editor I use:**[https://html5-editor.net/](https://html5-editor.net/)


For headers, I recommend using Action Network’s auto-generate wrapper tool, which will take your unique campaign logo, host it online for you, and place it on your email header. The HTML code will have your logo automatically centered on the email, which is perfect.


For footers, below is HTML code that you can copy into the HTML editor and customize for your unique campaign. (Make sure to edit the last line; otherwise, your page will link to AG Ford’s website!)


Footer Code:


This is a great spot for a quick bio about your candidate and what they are fighting for. Make sure to include a line at the bottom such as”you can support their campaign here” and link it to your ActBlue page.


Contributions or gifts to the YOUR CAMPAIGN are not tax-deductible.


Paid for by YOUR CAMPAIGN


Address Line 1


City, State, ZIP United States


[www.YourWebsite.com](http://www.fordfornevada.com/)


## **Setting up your HTML Snippet:**


Now that you’ve set up a wrapper with both a footer and header, the next step is to create a “Snippet” that will center your email’s content in the middle of the page. Snippets are saved HTML lines that can be added to emails with just one click.


Here’s where to go:


Once there, please copy and paste the below HTML code and then save it. This code will be used to center the body of your email while keeping the text aligned to the left.


Once this is saved, you’ll be able to add this to each email by clicking on the Snippets section of the email editor:


Make sure to start each email by selecting your custom wrapper and inserting this snippet. You’ll then input the content of your email in the table that this snippet creates. Then your content will be perfectly centered in one column with your header at the top and your footer at the bottom.


## **Integrating ActBlue with Action Network**


One of the best features of Action Network is its direct integration with ActBlue. By integrating both platforms, Action Network will automatically subscribe new ActBlue donors to your email list and will provide you live data of donations made per each email.


**More info on setting that up here:**[https://help.actionnetwork.org/hc/en-us/articles/210384283-Integrating-with-ActBlue](https://help.actionnetwork.org/hc/en-us/articles/210384283-Integrating-with-ActBlue)


## **Writing a Welcome Series & Creating an Action Network Ladder**


A welcome series typically consists of two emails that each new subscriber will receive upon being added to your list. The first email should introduce your candidate to the new email subscriber, and the second email should include a stronger ask for a donation.


**Here’s where you go to create your first ladder:**


A two-part welcome series will have the following steps on its ladder:


**It’s important to create a tag just for activists added to your welcome series.** That way, you can exclude them from receiving other email content while they are on your welcome ladder.


For your first email, focus on your candidate’s background and biography. If they are a challenger, talk about why they are running for office. If they are an incumbent, focus more on their accomplishments and their goals for another term in office. I typically end the first email with a soft ask for a donation. For the State Treasurer, his first email ends with, “Consider[chipping in a donation of any size](https://secure.actblue.com/donate/zc-grassroots?refcode=welcomeseries) and help power our campaign forward.”


The second welcome series email should be more direct. Include a strong ask with a question mark and a sense of urgency. Appeal to ideological reasons for why someone should support your candidate.


More information on ladders can be found here:[https://help.actionnetwork.org/hc/en-us/articles/115002611186-Creating-or-editing-a-ladder](https://help.actionnetwork.org/hc/en-us/articles/115002611186-Creating-or-editing-a-ladder)


**Also, consider setting up a re-activation series ladder.** This will automatically send emails to users who haven’t opened or engaged with your content within a set period of days (say 120 days). Once those emails have been sent, if a subscriber still hasn’t engaged with your content, the ladder will remove them from your list. This is important to maintain high open rates and keep your list active.


More information on setting up a reactivation series here:[https://help.actionnetwork.org/hc/en-us/articles/115002595303-Example-ladder-A-re-activation-series](https://help.actionnetwork.org/hc/en-us/articles/115002595303-Example-ladder-A-re-activation-series)


## **Putting together your email list:**


If you are a first-time candidate, your list is being built from scratch. Here are a few steps to build your campaign email list.


**1. Pull all of your personal email contacts and add them to Action Network**


**Gmail:**[https://support.google.com/contacts/answer/7199294?co=GENIE.Platform%3DDesktop&hl=en](https://support.google.com/contacts/answer/7199294?co=GENIE.Platform%3DDesktop&hl=en)


**Outlook:**[https://support.microsoft.com/en-us/office/export-contacts-from-outlook-10f09abd-643c-4495-bb80-543714eca73f](https://support.microsoft.com/en-us/office/export-contacts-from-outlook-10f09abd-643c-4495-bb80-543714eca73f)


Make sure to delete any .gov or government email addresses from your list. Government employees are not allowed to participate in political campaigns at work.


**2. New donors**


If you set up the integration above, new donations made on ActBlue will automatically subscribe the donor to your email list. If you are setting this up later in the campaign, make sure to backfill ActBlue donations when setting up the integration. The team at ActBlue can do this for you.


If you have donors that give via check, make sure to ask them what their email address is so you can add them to your list and stay in touch.


**3. Website signups**


Your campaign website should absolutely have a signup form. If you use WordPress, Wix, or Squarespace, you can set up an automation[via Zapier](https://zapier.com/) that will automatically subscribe these folks to your email list.


**4. Clipboard signups and business cards**


Make sure to bring a clipboard everywhere you go. Every event, canvass launch, or fundraiser should involve guests signing in on paper so you can collect their phone number and email for future contact.


Add all of these folks to your email and call time lists.


**5. Advertise your email list on social media**


Using Action Network’s page builder, you can create customizable petitions and forms that you can share on social media. New signups using these tools will automatically be subscribed to your email list.


**6. List swaps & email acquisition**


If you are a first-time candidate, it’s unlikely you’ll need to do this. List swaps are when campaigns exchange a set number of email addresses with each other (typically what we call 90-day openers or subscribers that have opened an email in the last 90 days). Email acquisition is when you purchase emails to add to your list. Both of these methods require experienced staff and are best reserved for larger campaigns.


## **Email Best Practices:**


Here are a few best practices to consider when creating and sending fundraising emails out to your list.


Quality over quantity:


It’s a common misconception that having as many emails as possible on your list is a good thing. In fact, you want to avoid this. Instead, what you want is an email list of engaged subscribers who want to open your content.


Subscribers who don’t want to be on your list will lower your open rates and can report your emails as spam which can tank your email deliverability.


Asking for donations:


**When asking for donations, make sure to appeal to why donors on your email list should support you or your candidate.** These subscribers often came to your list for two reasons: they are a part of your (or your candidate’s) personal network and want you to succeed, or they share ideological values with your candidate and support the mission they are working on. Your email content should appeal to both of these types of subscribers.


**You’ll also want to have a strong sense of urgency.** Every email should have a reason or argument for why donors should give right then at that moment. This can include a fundraising deadline, a new opponent, an ideological group like the NRA endorsing your opponent, and more. **If there is no urgent reason that immediately comes to mind, create one.**


Make sure not to treat your email list like an ATM. Put time and effort into writing quality emails that appeal to your subscribers and include a thoughtful (and urgent) ask at the end.


Ace that Subject line:


**35% of email recipients open emails based on the subject line alone.**


Keep your subject lines short, descriptive, and occasionally personalized (You can add in first names as a merge field via clips). It’s also good to avoid using lots of punctuation or emojis (no more than one is ideal).


Here are a few subject lines that were written by professionals for multi-million dollar email programs:


re: your partnership →


I’m asking:


A humble request:


HUGE opportunity re: the Senate


re: Georgia →


first major test for Democrats in 2021 (fast approaching)


tough fight ahead


A couple of big updates


*(More email statistics here:*[https://blog.hubspot.com/sales/subject-line-stats-open-rates-slideshare#sm.0016i5xrc1ckjdi6wc31mj851d5fd](https://blog.hubspot.com/sales/subject-line-stats-open-rates-slideshare#sm.0016i5xrc1ckjdi6wc31mj851d5fd) *)*


Optimize for Mobile:


The majority of all emails are now opened on a mobile device first. When sending a sample of an email draft, make sure that your sample looks great on both your desktop and cell phone.


Maintain High Open Rates:


**Aim for an open rate of 20% or more on all of your emails.** Open rates below this figure put your emails at risk of being placed in spam folders or not being delivered to your subscribers. Email service providers (ESP) like Gmail want to protect their users’ inboxes and can, at a moment’s notice, mark your outbound email as spam.


More tips on deliverability here:[https://help.actionnetwork.org/hc/en-us/articles/360030901752-Deliverability-Guide](https://help.actionnetwork.org/hc/en-us/articles/360030901752-Deliverability-Guide)


I hope this helps you build an incredible email program on Action Network.


If you have any other questions or feedback about this blog post, feel free to contact me directly at[\[email protected\]](https://www.numero.ai/cdn-cgi/l/email-protection#da91bbb6bbb4b39a8eb3a9a9b5ae89b5b6afaeb3b5b4a9f4b9b5b7) .
