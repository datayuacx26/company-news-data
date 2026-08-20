---
schema_version: "1.0.0"
document_id: "45f76310d387750830faf87b9ec815257143205660f8c70184c7406ac5a9c017"
company_key: "yc-magic-loops"
company: "Magic Loops"
source_id: "yc-magic-loops-rss-5d6a51a0bf61"
canonical_url: "https://blog.magicloops.dev/posts/enhanced-validation/"
published_at: "2024-01-25T17:00:00+00:00"
first_seen_at: "2026-07-24T10:22:08.394696+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:226f46d2e42111bb53aebffb5c75b980e09e0b09c1c3e3bb479b9031d07e6dbe"
---

# Improving validation for the Loop Creator

## **tl;dr**


> Validation just got easier 🎉 — we’ve made the Loop Creator more intuitive and error-tolerant


## **Before**


In many cases, the Loop Creator was… a pain to work with.


It would attempt to validate and rewrite a Block (up to 3 times), even if the Block’s output was already correct or irreparable.


## **Now**


### Instant Feedback


To streamline the Loop building process, we’ve upgraded the Validate functionality to provide immediate feedback on each block:


- **Success** : “Block looks good.”
- **Fail** : “Block output appears incorrect!”


Here’s what success looks like:


And here’s what a potential failure looks like:


### **Overruling False Validation**


When a Block fails validation, you can now exercise more authority:


- If you’re confident the Block’s output is fine, override the decision and proceed with: **Correct** *(Block is actually good, continue!)*
- If the output is **Incorrect** , you now have the power to revise the description directly, giving you finer control over the automatic fixes to your Loop.


Here’s what revising a Block’s description looks like:


## Other changes


This update includes a few other fixes as well:


#### **Streamlined JSON Mode**


Function calling is out; enter pure JSON mode. This change means the Loop Creator (LC) can focus on one block at a time, enhancing stability and predictability.


> We should probably dedicate a whole post to why we moved away from Function Calling to JSON mode… another time!


#### **Revamped Validate Prompt**


We’ve rewritten the validate prompt to be more transparent and actionable. This means clearer communication, reducing development time and friction.


#### **Launching “Request Variable” in LC**


We’ve had this functionality feature-flagged for awhile, but are excited to share it with y’all!


The Loop Creator can now “Request Variables” as part of it’s validation step, helping alleviate situations where an email, API key, or other required variable is needed for the Block to work.


Here’s what it looks like:


#### **Improved Error Handling**


We’ve tidied up loose ends in error handling within the Loop Creator, aiming to make the Loop creation as smooth as sailing.


> **As smooth as sailing** — thanks ChatGPT…


## **Wrap-Up**


These enhancements are a big step forward in giving you the autonomy and tools to build more complex, error-free Loops faster than ever. We’re committed to improving the Loop Creator experience, and we welcomeyour feedback on these updates.


> Start exploring these new features now and take your Loop generation to the next level!


[Create a Loop with new validation controls today](https://magicloops.dev/)
