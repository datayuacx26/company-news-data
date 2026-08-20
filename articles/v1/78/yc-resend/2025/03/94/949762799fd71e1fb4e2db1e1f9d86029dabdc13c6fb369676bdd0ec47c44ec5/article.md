---
schema_version: "1.0.0"
document_id: "949762799fd71e1fb4e2db1e1f9d86029dabdc13c6fb369676bdd0ec47c44ec5"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/react-email-4"
published_at: "2025-03-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:332da956a4aa397614371e5d56d4f05ae9333d86c1ec4196bb5910656788f178"
---

# React Email 4.0

We're excited to announce[React Email 4.0](https://react.email/) , featuring:


- **Linter**
- **Spam Score**
- **Compatibility Checker**
- **Responsive Preview**
- **New Components**


Update today and check theupgrade instructions below.


```text
npm i react-email@latest


```


React Email now has **422,541 weekly downloads** on[npm](https://www.npmjs.com/package/react-email) , that is a **56% increase** since the last major release 5 months ago.


We also have **15,509 stars** on[GitHub](https://github.com/resend/react-email) and would like to thank all the **154 contributors** who made this possible.


## Linter


The new Linter tool will analyze every link in your email to see if it's valid or not.


- If the links are valid and returning a` 200` response status
- If the links are secure and using` HTTPS` instead of` HTTP`


It will also try to load every image in your email and recommend tips to improve it.


- If the images are optimized and no bigger than` 1 MB`
- If the images are accessible and contain` alt` text


## Spam Score


The Spam Score will look at your email content and use a robust scoring framework to determine if the email is likely to be spam.


It's powered by[SpamAssassin](https://spamassassin.apache.org/) , the #1 open source anti-spam platform.


Here are some of the checks it performs:


- If the email has common spam words related to pharmaceuticals, scams, or lottery.
- If the email has an unusually high number of URLs in the email body.


## Compatibility Checker


The new Compatibility Checker will see how well the HTML/CSS is supported across popular mail clients like Outlook, Apple Mail, and more.


It's powered by[Can I Email](https://www.caniemail.com/) , an open source repository of features and their compatibility for each email client.


There are hundreds of checks available, but here are some of the most common ones:


- If the HTML contains` <svg>` which is not supported by Gmail
- If the CSS contains` display: flex` which is not supported by Outlook


## Responsive Preview


React Email now comes with a Responsive Preview, so you can see how your email will look on different device sizes.


There are presets for mobile and desktop, and you can also customize the width/height of the preview.


## New Components


We've added 8 new components that you can copy and paste into your emails.


They include Bento Grids, Pricing Tables, Feature Lists, Ratings, and more.


- [List (2 components)](https://react.email/components/list)
- [Pricing (2 components)](https://react.email/components/pricing)
- [Marketing (1 component)](https://react.email/components/marketing)
- [Feedback (3 components)](https://react.email/components/feedback)


## Upgrade instructions


Update your` react-email` package to` 4.0.x` .


```text
npm i react  -  email@latest
```


You can also check if you have any missing dependencies for your emails to be bundled.


## Conclusion


If you want to know all the details about this release, check the[React Email Changelog](https://react.email/docs/changelog) .


If you have any problems upgrading, feel free to share on[GitHub](https://github.com/resend/react-email) or[X](https://x.com/resend) .
