---
schema_version: "1.0.0"
document_id: "9764aa5bd280d3c54f427c0bcc1676344b28e7910ee4a8ac822a25ea91a9f8bb"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/react-email-5"
published_at: "2025-11-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:d3c5e28a3a18984c4cb7dfae3d2b61898d0bb658e5928431d1f02ebe8d885e1f"
---

# React Email 5.0

We're excited to announce[React Email 5.0](https://react.email/) , featuring:


- **Dark Mode Switcher**
- **Tailwind 4**
- **Resend Integration**
- **8 New Components**


Update today and check theupgrade instructions below.


```text
npm i react-email@latest @react-email/components@latest


```


React Email now has **920,325 weekly downloads** on[npm](https://www.npmjs.com/package/react-email) , that is a **117% increase** since the last major release 7 months ago.


We also have **17,041 stars** on[GitHub](https://github.com/resend/react-email) and would like to thank all the **[182 open source contributors](https://github.com/resend/react-email/graphs/contributors)** who made this possible.


## Dark Mode Switcher


Theming in email has always been a challenge, since each client renders emails differently. The new theming system makes this much easier. We've tested dark mode across the most popular email clients to ensure compatibility.


## Tailwind 4


React Email now supports Tailwind 4. This allows for simpler code, and provides the opportunity for performance improvements.


Because React Email checks the compatibility of your CSS, you can safely develop with confidence that your emails will render correctly.


## Resend Integration


[Resend Templates](https://resend.com/docs/dashboard/templates/introduction) are a new way to collaborate on emails with your team, even if they're not technical.


With this new integration, you can upload a React Email template to Resend and your entire team can collaborate in real-time in the visual editor.


Here's how to[upload Templates from React Email to Resend](https://react.email/docs/integrations/resend#set-up-templates-with-resend) .


After running` npx react-email@latest resend setup` and pasting an API key, the React Email CLI can upload Templates to Resend.


## 8 New Components


React Email includes components that you can copy and paste to build beautiful emails. It's one of the easiest ways to get started. React Email 5 adds several new components to the list to inspire your emails.


- [Avatars (4 components)](https://react.email/components/avatars)


- [Stats (2 components)](https://react.email/components/stats)


- [Testimonials (2 components)](https://react.email/components/testimonials)


You can find more in the[Components Gallery](https://react.email/components) .


## Upgrade instructions


Here's how to update from React Email 4.0 to 5.0.


A) Update your React Email packages:


```text
npm i react-email@latest @react-email/components@latest


```


B) Replace all` renderAsync` uses with` render` method.


Make sure you update` @react-email/components` alongside` react-email` . The compatibility checker now only supports Tailwind 4, so you need to update both in sync.


## Conclusion


In addition to the features we've released today, we've also upgraded React Email to support React 19.2 and Next.js 16.


If you want to know all the details about this release, check the[React Email Changelog](https://react.email/docs/changelog) .
