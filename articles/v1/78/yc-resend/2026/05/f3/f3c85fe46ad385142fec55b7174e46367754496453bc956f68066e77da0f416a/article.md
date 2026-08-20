---
schema_version: "1.0.0"
document_id: "f3c85fe46ad385142fec55b7174e46367754496453bc956f68066e77da0f416a"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/6-tips-for-accessible-emails"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:bf1a50db71b59c18848bc3e83f65d986f9a09ca3a16dac167a53ebedbd4ecd75"
---

# 6 Tips for Accessible Emails

The[Email Markup Consortium](https://emailmarkup.org/) just released its[2026 email accessibility report](https://emailmarkup.org/en/reports/accessibility/2026/) , analyzing **376,348 emails** .


Of all emails analyzed, **only eight passed every check** (just 0.002%!). Nearly all failed emails contained issues rated Serious or Critical, and yet most of the fixes are small, mechanical changes.


In this post, we'll cover **what the report found** and **what you can do about it** .


## Why build accessible emails?


Accessibility is often easy for developers to ignore, but it's crucial for every recipient.


- **Assistive technology.** Screen readers, braille displays, low-vision zoom, and right-to-left languages. For them, structure is not decoration; it is the only way to navigate.
- **Every reader.** Accessibility matters when reading on a phone in bright sun, when your inbox forces dark mode, or when a client translates text into another language.
- **Agents and assistants.** Modern email clients and operating systems summarize, extract content, and act for you. Accessible email is machine-readable email.


Building accessible emails is best for everyone.


## What did the report find?


The report's most common issues are also its most fixable.


Here are the ones worth your attention, with the fix in plain terms.


### 1. Include language and direction attributes


Tell clients what language you are speaking and the direction the text flows. The` lang` and` dir` attributes were missing on the body of roughly **96%** of emails. Without them, assistive tech guesses pronunciation and translation tools stumble.


Your email HTML should include` lang` and` dir` attributes on the` <html>` element and on the` <body>` 's direct children.


- ` lang` : the language of the email, written in[BCP 47 language tag](https://developer.mozilla.org/en-US/docs/Glossary/BCP_47_language_tag)
- ` dir` : the direction of the email content


- ` ltr` : left-to-right
- ` rtl` : right-to-left
- ` auto` : as a last resort


```text
<  html lang  =  "en"   dir  =  "ltr"  >        <  head  >          <  title  >  Your weekly product updates  <  /  title  >        <  /  head  >        <  body  >          <  div lang  =  "en"   dir  =  "ltr"  >            <  !  --   email content   --  >          <  /  div  >        <  /  body  >     <  /  html  >
```


Several email clients strip the attributes from` <html>` , which is why duplicating them on the body's children is important.


### 2. Define layouts as presentational


Email layouts rely on` table` elements, but if you don't define them as` role="presentation"` , screen readers announce each row (for example, "table, row 1 of 6") and make your emails nearly unreadable.


```text
<  table role  =  "presentation"  >          <  tr  >              <  td  >  ...  <  /  td  >          <  /  tr  >     <  /  table  >
```


In their testing, the Email Markup Consortium found that **84%** of emails didn't correctly identify presentation-only tables.


### 3. Provide a clear semantic outline


Screen readers, agents, and clients scan content based on the headings in an email. As with the web, each email should have a clear, hierarchical structure using headings.


```text
<  h1  >  Main Heading  <  /  h1  >        <  h2  >  Subheading   1  <  /  h2  >          <  h3  >  Sub  -  subheading   1.1  <  /  h3  >          <  h3  >  Sub  -  subheading   1.2  <  /  h3  >        <  h2  >  Subheading   2  <  /  h2  >          <  h3  >  Sub  -  subheading   2.1  <  /  h3  >          <  h3  >  Sub  -  subheading   2.2  <  /  h3  >
```


The test found that **74%** of emails have no` <h1>` . Provide a clear structure to map your email's content and make it easy to scan and understand quickly.


Note that short emails, such as notifications or authentication emails, can be sent without headings since readers can easily discern their purpose and content.


### 4. Add descriptive link and alt text


Links should have descriptive text that tells the user what to expect when they click.


- **Poor:**` <a href="...">click here</a>`
- **Good:**` <a href="...">learn more about our new product</a>`


Images should have an` alt` attribute that describes the image visually.


- **Poor:**` <img src="..." alt="photo of a bike">`
- **Good:**` <img src="..." alt="A red bicycle leaning against a brick wall on a rainy street">`


Good alt text describes the image’s **purpose and important details** in context. Poor alt text is too vague, redundant (for example, "a photo of ..."), or obvious.


If your image is decorative only, it should use an empty` alt=""` so it is skipped cleanly.


### 5. Add a <title> tag


Many email clients and assistive technologies read the` <title>` tag first, and yet it was missing on **45%** of emails analyzed.


```text
<  title  >  Weekly product updates from Resend  <  /  title  >
```


When possible, the title should describe the specific email content, like a subject line.


### 6. Check for color contrast


Over half of emails fail the 4.5:1 contrast ratio. Use[Contrast Checker](https://webaim.org/resources/contrastchecker/) to verify your colors meet this standard.


Some popular inboxes render every message in dark mode, and they choose dark mode colors based on your initial colors. Ensuring at least a 4.5:1 contrast ratio increases the likelihood that email-client-enforced dark mode colors are still accessible.


## Upgrading React Email


Many developers use React Email as the basis for their email templates. We just shipped **better accessibility defaults** in response to this report.


Update the package to get the latest version and improve your email accessibility.


```text
npm     install   react-email@latest
```


Our updates included:


- Language and direction are now set on` <Body>` by default
- ` <Img>` defaults to an empty` alt`
- ` <Markdown>` tables render` role="presentation"` by default
- ` <Preview>` now also emits a` <title>`


Along with this, we've improved the[React Email agent skill](https://github.com/resend/react-email/tree/canary/skills/react-email) to help your agents build more accessible emails.


Don't use React Email? Check out the[Email Best Practices](https://github.com/resend/resend-skills/tree/main/skills/email-best-practices) agent skill. We built it to help you write better emails, regardless of your email setup. This skill has also been updated with accessibility best practices.


## A checklist for your content


React Email handles the structural defaults, but many of these findings require your judgment. Here is a quick checklist to get you started:


- **Headings** : open with an` <h1>` , then nest in order.
- **Alt text** : describe meaningful images and link destinations; leave decorative ones empty.
- **Link text** : say where the link goes, never "click here."
- **Contrast** : meet 4.5:1, and preview in dark mode.
- **Language** : set` lang` and` dir` to match the content (React Email defaults to` lang="en" dir="ltr"` )


Small acts of consideration, repeated across every email you send, add up to a better experience for everyone.


- **Non-sighted readers** can take action on password reset emails
- **Agents** can parse receipts efficiently
- **Everyone** benefits from proper contrast, even if an email client enforces dark mode


Accessibility is a shared responsibility that makes every email better for every reader. How else can we help you analyze and improve your email accessibility?
