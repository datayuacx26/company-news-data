---
schema_version: "1.0.0"
document_id: "e058a5499684e4433c0f8a630e903ae29f9b28c41638001503a422c1f2e83421"
company_key: "yc-numero"
company: "Numero"
source_id: "yc-numero-rss-2718f20b56fd"
canonical_url: "https://www.numero.ai/blog/how-to-connect-actblue-numero-to-google-sheets/"
published_at: "2021-07-01T04:00:00+00:00"
first_seen_at: "2026-07-25T16:42:35.962761+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:3907f0bc898a94964669d6a8a2198b39f1b02f18e6d297e1f1a59ee1236a28c7"
---

# How to Connect ActBlue & Numero to Google Sheets

*This post was originally published by Kalani Tissot on*[his blog](https://www.tissotsolutions.com/post/how-to-connect-actblue-numero-to-google-sheets) *and has been reposted here with his permission.*


I’ve scoured the web, and there is no up-to-date guide on connecting popular donation processors like ActBlue or[Numero](https://numero.ai/) to a Google Sheet. **Let’s change that.**


Here’s how to integrate these three applications using Zapier and automate part of your fundraising workflow. (You can sign up for a[free Zapier trial here](https://zapier.com/page/welcome-start/) )


## **Connecting ActBlue to a Google Sheet:**


**Zapier Share link:**[https://zapier.com/shared/42a1cbd50f751c599432665874367f785793b2ad](https://zapier.com/shared/42a1cbd50f751c599432665874367f785793b2ad)


**Zapier:**


1. Create a new Zap.
2. For your trigger, use “Webhooks by Zapier.”
3. For the trigger event, select “Catch Hook.”
4. Copy the custom Webhook URL and hit continue (You can ignore the silent mode and “pick off a child key”).


**ActBlue WebHook:**


1. Log in to your ActBlue Account.
2. Go to Webhook Integrations in the Tools section.
3. Click on the green “Request a New Webhook” box.
4. For type, select “ActBlue Default.”
5. Paste your Zapier Webhook link into the endpoint URL.
6. Create a credentials log in (this won’t matter for this).
7. Submit your request and wait up to 24 hours for ActBlue support to approve the webhook.


**Google Sheets:**


1. Add the following columns to your respective worksheet: First Name, Last Name, Contribution Page, Amount, Date, Address, City, State, ZIP, Email, Phone Number, Occupation, and Employer.


**Zapier:**


1. Test the Webhook trigger, and a recent donation should come up.
2. For step two, select Google Sheets and connect your Google account.
3. Select your Google drive and the spreadsheet that you just created.
4. Map each webhook value to its respective column.
5. Test your action (Zapier should send your most recent donation to the Google Sheet).


**That’s it! Moving forward** , **this Zap will automatically send all of your ActBlue donations to the Google Sheet of your choice.**


## **Automatically adding Numero donations to a Google Sheet:**


**Zapier Share Link:**[https://zapier.com/shared/ebc13d21d3934cad26edf8e126e18d3033a4fa22](https://zapier.com/shared/ebc13d21d3934cad26edf8e126e18d3033a4fa22)


**Numero doesn’t have any webhooks available for donations, so we have to collect donation data via another means — email parsing.** Parsing is a type of software that allows you to collect & capture data from received emails.


Go to[parser.zapier.com](http://parser.zapier.com/) and log in via your Zapier account. Then, create a new email address to receive Numero contribution alerts.


In Numero, go to “Manage Team” and click on “Add a new Team Member.” Add in your new Zapier Email Parsing address as a new team member. Then, your new Zapier email parsing account will receive an invitation inviting them to join Numero. On an incognito window browser, log into your email parsing account and click the link to join. Then in Numero, under your user settings, set up notifications for contributions of $1 or more.


**Zapier:**


1. Create a new Zap.
2. For your trigger, use “Email Parsing by Zapier” and select the trigger event as “New Email.”
3. Connect your email parsing account to Zapier and run a test. The initial email inviting you to join Numero should pop up.


**Email Parser by Zapier:**


1. Now, wait until a Numero contribution is made. If you wish to speed this up, make a $3 Numero contribution to send an email notification to your parser account.
2. Once an email notification for a contribution has been received, we now have to create a new Template for the email parser to extract data.


**Here’s the template that I use for my Zap.** I’ve blacked out specific donor data for privacy.


The curly brackets, {data}, represent which data strings will be extracted from your email alert. **Here is what my output looks like:**


**Zapier:**


1. Now that your email parser is set up head back to Zapier and test your email parser trigger. Your most recent contribution on Numero should come up with a parse outlet line containing your extracted data.


**Google Sheets:**


1. Create a new Google spreadsheet (or worksheet in your Finance Plan) and add in the following columns: Date & Time, Amount, Name, Phone Number, Email, Address, Employer & Occupation, and Donation Form.


**Zapier:**


1. For your first action, select Google Sheets and choose “Create Spreadsheet Row in Google Sheets.”
2. Connect your account and select the respective Google spreadsheet & worksheet.


Here’s what your action step should look like.


**Zapier:**


1. Test your action on Zapier, and this Zap should add a new spreadsheet row to your Google Sheet!


**Now moving forward** , **every time you receive a Numero contribution, your respective Google Sheet will automatically be updated with new donation data.**


**Additional Integrations:**


Use Slack? Add a step to either of these Zaps and send Slack notifications when contributions are processed.


Want to send an email from your candidate’s Gmail account thanking each donor? Easy, add in Gmail as a third action step on your Zap and add parse output or webhook data to customize each email.


Do you use Action Network for your mass emails? Set up a third action and automatically add each new Numero donor to your email list.


If you use Zapier for any other integrations, send me a note at[\[email protected\] .](https://www.numero.ai/cdn-cgi/l/email-protection#763d171a17181f36221f0505190225191a03021f1918055815191b) I’d love to hear about how others are using tech tools like this.
