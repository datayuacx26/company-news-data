---
schema_version: "1.0.0"
document_id: "ed407841df1cdf568464820eb77fae1a93cbc58ff1d39f81424e81c41c482c1a"
company_key: "yc-loops"
company: "Loops"
source_id: "yc-loops-news-import-d3d77458967f"
canonical_url: "https://loops.so/changelog/guardian"
published_at: "2025-09-17T00:00:00+00:00"
first_seen_at: "2026-07-25T13:13:35.252230+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:05effd7449dabf22bb5096a70fab6c47c368d4836c22035ee2bc7ee35cf2d27d"
---

# Introducing Guardian

### **Release: Guardian**


[Guardian](https://loops.so/docs/creating-emails/guardian) is a lightweight, proactive filter set that lives in the email editor and Loop Builder actively alerting you and preventing mistakes. You will see it at the top of any campaign, loop or transactional email as a light checkmark when everything looks good but when an issue is detected it will make itself known with a colored notification dot.


Helpful when you need it, out of your way when you don’t. Current checks include:


-


Misplaced variables


-


Unsaved changes (save state)


-


Missing button links


-


Missing fallbacks for dynamic content


Have ideas for future guardrails? Just email[\[email protected\]](https://loops.so/cdn-cgi/l/email-protection#3d5e554f544e7d5152524d4e134e52) and tell us what to add next.


### **Updates**


**Supabase OAuth Support**


Head to the[Supabase Settings](https://app.loops.so/settings?page=supabase) inside Loops and use one-click OAuth to connect Supabase for SMTP setup.


**Start from any email in a Loop**


You can now start a new email in a Loop from any email in the current or another Loop. We also support starting from a Campaign or Transactional email (both published and draft states).


Finally, we added a simple search filter to make it easier to find the exact email you’re looking for.


**UI redesign**


You may have noticed, the app got a bit of polish! This should make it a bit easier to parse large groups containing many emails.


Some of the improvements we’ve made:


-


Saving custom sorting by Updated, Sent and other top-level columns.


-


Better drag-and-drop between groups as cards now shrink to a more manageable size and show a drop indicator.


-


Block manual regrouping on system sorted emails. For example, if you sort ascending and attempt to drag, we’ll help you revert to your custom sort in one click.


-


Better contrast on group headers. This improves accessibility and scanning on wide monitors.


Most importantly, these changes introduced a new` /groupId` page. You can click the number next to a group, or the arrow on the far right, to open the group in a dedicated page.


This makes bookmarking dedicated groups possible. For example, you can now create a dedicated bookmark for product updates without having to drill through the app every time.


Additionally, the Loop Builder has had a light coat of paint to improve dark mode and smooth out some rough edges.


### **Hiring update**


We’re excited to welcome **Max** to the team! Max was previously at Meta working on the Lexical framework, the text editor we use here at Loops.


Max joining will help us extend and lay the foundation in the editor for what’s coming next.


### **Everything else**


**Better search**


You can now paste in a` transactionalId` ,` campaignId` or` loopId` in the` CMD+K` (the search bar) in the app to filter and navigate to the given email.


**Mailing list support for incoming Webhooks**


Automatically add contacts piped in via incoming Webhooks (Stripe, Clerk, Supabase) to the mailing lists you’ve set up in Loops.


**Standardized API errors**


Every error now includes` status` and` message` to make it easier to process errors and integrate with your apps.


**Hit**` **N**` **to create a new email**


Press` N` on your keyboard anywhere in the app to start creating a new email!


**Paste in Markdown improvements**


We tweaked our formatter to account for soft breaks and a few other inconsistencies we noticed in markdown coming from third-party apps.


**Format text in buttons**


You can now bold text in buttons along with other text decorations.
