---
schema_version: "1.0.0"
document_id: "e53b483de9e4a6c94d2a73fcfa3d844c2e28c6988b26f0aeeea90458009297f3"
company_key: "yc-streak"
company: "Streak"
source_id: "yc-streak-news-import-44963d82b4d4"
canonical_url: "https://www.streak.com/post/february-29th-bug-fixes-and-improvements"
published_at: "2024-03-01T00:00:00+00:00"
first_seen_at: "2026-07-26T02:24:36.945483+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:e55ce12328e133b6cb29d8d18412a47641bdad8932b768e30e6b54e6b4680c96"
---

# February 29th: Bug fixes and improvements

Our engineers tackled a number of improvements and fixes over the last couple weeks.


You’ll find improvements across Streak, including everything from viewing Streak University videos in Gmail to making better automations and AI-generated formulas to importing and exporting your data.


As always, we appreciate your feedback - it helps us continue improving Streak and making a better CRM experience for our customers.


## Importing and exporting your data


### Creating a new column during import resulted in with blank column names


Our[importer tool](https://www.streak.com/post/import-crm-data) allows you to import leads and opportunities into your pipelines from a spreadsheet without leaving Gmail.


It also allows you to create new pipeline columns during the import process. There was a bug that caused your new columns to be created without column names, which caused confusion and other problems down the line.


We fixed the issue, so you can now create and name new columns using the importer tool.


### Exporting pipelines with special characters could cause errors


Pipelines with special characters in their names, like “Marketing + Operations,” caused errors when[exporting your pipeline data to Google Sheets](https://www.streak.com/post/export-to-google-sheets) . We’ve fixed this issue so you can continue to include special characters in your pipelines.


### Import error for Excel files stored in Google Drive


Our[native Streak importer tool](https://www.streak.com/post/import-crm-data) allows you to import multiple file types into your Streak pipelines.


However, people trying to import Excel files stored in Google Drive were hitting errors when doing so. This has been fixed, so you can access the files in your Google Drive to add contacts and leads to your pipelines.


## Native integrations and automation


### New: incoming message trigger can ignore replies


Setting up a[Gmail integration and automation](https://www.streak.com/post/native-crm-automations-and-integrations-in-streak) can help your team create tickets for new support or sales inquiries, orders, and more.


We recently added a filter that allows you to ignore replies for incoming messages. This filter means your automation will only run if the incoming message is the first email in a thread, not all the back-and-forths that follow.


### Add thread to box action failing


[Native integrations and automation](https://www.streak.com/post/native-crm-automations-and-integrations-in-streak) allow you to connect Streak with other apps (like Typeform, Calendly, or Google Forms) and create automations inside Streak.


A common automation is to add an email thread to a box when you receive a new email that meets certain criteria. The “Add thread to box” action was failing in some scenarios, but we identified the issue and fixed the bug.


## Streak University video lessons


### Streak University video mole issues


We recently revamped the Streak Home sidebar, which you can find on the right side of your Gmail window by clicking the Streak icon. The revamp included adding our popular[Streak University videos](https://www.streak.com/university/welcome-to-streak) , which are bite-sized lessons and step-by-step instructions to help you get started.


After selecting a video in the sidebar, the video pop up mole was missing the maximize icon that allows you to watch the video in a bigger window. If you outsmarted the system and clicked the missing icon anyways to maximize the window, the toolbar would disappear altogether after exiting the video player.


This has been fixed.


## AI-generated pipeline formulas


### New: Easier to adjust AI formula writer prompts based on results


Our AI formula writer helps you add logic and calculations to your pipelines by creating formulas out of your natural language prompts.


Now when you hit “enter” on your prompt, we’ll keep the text of the prompt visible so you can go back and make quick changes to fine-tune your formula.


### AI formula writer “Couldn’t generate formula from text” error


[Streak’s AI features](https://www.streak.com/ai) include a formula writer that helps you add logic and calculations to your pipelines with natural language prompts. That means you can type something like “multiply the deal size by .15 to calculate commission” and the formula generator will spit out the code for you. It’ll even understand more complicated logic, so feel free to put it to the test.


We recently fixed a bug that caused the formula writer to spit out a “Couldn’t generate formula from text” error. You can continue to describe the formulas and calculations you’d like to add to your pipelines.


## Miscellaneous bug fixes


### Link missing from the mail merge compose window


The option to add contacts from a pipeline was missing from the[mail merge](https://www.streak.com/mail-merge-gmail) compose window.


This has been fixed, so you’ll once again see the “choosing from pipeline” link when you compose a new mail merge.


### Stage labels are cut off in gmail search results


You can use the Gmail search bar to find boxes, contacts, organizations, and relevant emails in Streak.


The pipeline label, which shows “Negotiating” above, was slightly cut off along the bottom edge. This ruined its perfectly rounded corners, and the perfectionists among us took notice. Order has been restored.


### Date column not updating for certain entries


Date columns are[a type of custom column](https://www.streak.com/university/pipelines-deepdive-columns) that allow you to track important events associated with boxes in your pipelines.


From the box view, you can enter dates into date columns by typing in the date and hitting the tab key. Some date columns wouldn’t accept dates in certain formats, resulting in an empty column after hitting enter.


### Email drafts added to multiple boxes only added to first box


When composing an email and[adding it to multiple boxes](https://www.streak.com/post/new-add-an-email-to-multiple-pipelines) before it’s sent, it was erroneously only added to the first box you selected upon sending.


We fixed this issue so you can be sure your emails will be added to each box you select.


## Miscellaneous improvements


### New: Identify when contacts have sent messages in your threads


Contact and organization pages show you a timeline of your team’s interactions with people and companies in your pipelines.


We’re making it easier to see who’s contributed or responded to threads in your pipelines by bolding the name of anybody who’s sent an email in that thread.


This has been fixed, so you can quickly update date columns as you’re working in boxes. (You can also try our[AI-powered data autofill](https://www.streak.com/ai) , which will grab information from your emails to fill in columns for you.)


### Assign and add new team members in your pipelines


You can now add new members to your Streak team when you assign them to a task or box in your Streak pipelines.
