---
schema_version: "1.0.0"
document_id: "c32bcebb408869ccb999ffe176106ce8d601b12f45cb5ca5578c58e550f213bc"
company_key: "yc-streak"
company: "Streak"
source_id: "yc-streak-news-import-44963d82b4d4"
canonical_url: "https://www.streak.com/post/august-25-fixes-improvements"
published_at: "2025-08-06T00:00:00+00:00"
first_seen_at: "2026-07-26T02:24:36.945483+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:0ebb75ab0c4e7365ae2f1adeb26c56850c760870162750cadfa8c76221679993"
---

# Product fixes and polish: Mail merge, tracking, comments, and more

We’ve been working on bug fixes and small improvements across Streak, many of which came from customer feedback and reports (thank you!).


From making mail merge more reliable to restoring key tracking features and squashing some UI bugs, here’s what’s new and improved.


## 📬 Mail merge updates


### Mail merge remembers your unsubscribe link setting


Streak now remembers whether you included an unsubscribe link in your last[mail merge](https://www.streak.com/features#mail-merge) , so you don’t have to toggle it on each time you send a campaign.


### Clearer error messaging for plain text drafts


If your mail merge draft is in plain text, you’ll now see a clear error message explaining what needs to be fixed before sending.


### Mail merge stats no longer cause the page to shake


Viewing[mail merge stats](https://www.streak.com/post/mail-merge-reports) used to make the page jump—data would shift, but merge names stayed still. This visual bug is fixed for a smoother experience.


### Drip sequences now correctly pause on reply


In rare cases,[follow-up emails](https://www.streak.com/features#automated-follow-up) were sent after a recipient replied. We identified and fixed the issue so Streak reliably pauses sequences when replies come in.


## ✉️ Email and link tracking


### Link tracking now logs clicks correctly


Streak’s[tracked links](https://www.streak.com/features#email-tracking) weren’t registering clicks for a short period of time. We became aware of this issue quickly and it has since been resolved—tracking clicks on links in your sent emails now works as expected.


### Full view history returns to email tracking previews


The icon at the top of tracked emails wasn’t showing the full view history, even though it appeared in the sidebar. This preview now displays tracking data again.


### Read notifications now send reliably


[Read notification](https://www.streak.com/features#notifications) emails weren’t always sent due to issues with tracked links. They now send correctly the first time someone opens your message.


## 👥 Contact and organization updates


### Custom field changes now show in contact timelines


Updates to[custom contact fields](https://www.streak.com/post/track-contacts-in-gmail) are now visible in the[contact timeline](https://www.streak.com/features#contact-timeline) , so you can see when and how contact data changes over time.


### Contact importer matches by email address


Previously, if two contacts had the same name, imports might update the wrong one. We now prioritize email addresses when matching contacts to prevent overwrites.


### Custom tab titles for contact and org lists


You’ll now see helpful tab titles for your[contact and organization lists](https://www.streak.com/features#contact-tracking) , making navigation easier—especially when managing multiple tabs.


## ⚙️ Automation and formula fixes


### Call logs and meeting notes from automations now update magic columns


Automated logs now update the correct[magic columns](https://www.streak.com/features#magic-columns) and appear in the newsfeed. This works retroactively for existing logs too—no backfilling needed.


### ‘Date Created’ column available in automations


You can now use the ‘Date Created’ magic column in your automation actions.


### Formula columns now update instantly in the sidebar


[Formula columns](https://www.streak.com/features#formula-columns) in the sidebar didn’t update right away when related fields changed. They now refresh immediately, just like they do in the pipeline view.


## 💬 Comments and collaboration


### Long comments stay expanded after posting


Previously, long[comments](https://www.streak.com/features#comments) collapsed seconds after posting, making them seem like they disappeared. They now stay open so you can review or edit them right away.


### @mentions now save correctly, even at the end of comments


When an @mention was the last part of a comment, it sometimes failed to save. That’s now fixed—@ mentions work no matter where you place them.


## 🧭 Navigation and general improvements


### Autocomplete menus now respond to typing immediately


Autocomplete menus used to require a mouse hover before responding. Now you can simply start typing to assign boxes, choose data sources in reports, or select information in Streak Home.


### Saved view export issues resolved


Some users couldn’t export[saved views](https://www.streak.com/features#pipeline-filters) or find export files in Drive—especially when[exports](https://www.streak.com/features#data-export) were initiated by non-admins. This has been fixed, and exports now work as expected.
