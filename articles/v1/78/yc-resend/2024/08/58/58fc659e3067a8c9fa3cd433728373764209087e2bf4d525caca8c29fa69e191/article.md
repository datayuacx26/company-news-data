---
schema_version: "1.0.0"
document_id: "58fc659e3067a8c9fa3cd433728373764209087e2bf4d525caca8c29fa69e191"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/react-email-3"
published_at: "2024-08-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:6ccad91ad3542381955e8a511655fbb8fdadbed4873cb8a11d1ccfb841f14538"
---

# React Email 3.0

We're excited to announce[React Email 3.0](https://react.email/) , featuring:


- **Brand new component library**
- **11x performance improvement**
- **Support for React 19 RC**
- **Deprecation of renderAsync**
- **Supabase Email Hook**


Update today and check theupgrade instructions below.


```text
npm i react-email@latest @react-email/components@latest


```


React Email now has **270,627 weekly downloads** on[npm](https://www.npmjs.com/package/react-email) , that is a **136% increase** since the last major release 7 months ago.


We also have **13,503 stars** on[GitHub](https://github.com/resend/react-email) and would like to thank all the **132 contributors** who made this possible.


## Brand new component library


As developers, we are big fans of projects like[Tailwind UI](https://tailwindui.com/) and[shadcn/ui](https://ui.shadcn.com/) . These projects make it dead simple to create elegant UIs by simply copying and pasting components.


We want to provide that same experience with React Email, so we made **54 components** for you to create beautiful emails.


Here's how to get started:


### 1. Navigate to Components


When you go to[react.email/components](https://react.email/components) , you will see all the available categories, including e-commerce, marketing, and more.


### 2. Find a component you like


Once you find a component you like, you can see the preview on desktop and mobile.


### 3. Copy and paste the code


When you inspect the code, you will see two options: **Inline CSS or Tailwind CSS** .


Pick your preferred option, and copy the code directly into your project.


We can't wait to see what you'll build with these components.


Shoutout to[@luxonauta](https://x.com/luxonauta) for the amazing work on the new website pages, and[@leandrodragani](https://x.com/leandrodragani) , who's the creator of[React Email Templates](https://reactemailtemplate.com/) , and inspired this project.


## 11x performance improvement


Great developer experiences mean fast startup times. That's true for any framework, library, or tool.


That's why we decided to focus on improving the performance of React Email 3.0 even more.


We ran a performance benchmark for the old version of React Email (` 2.1.6` ) against the new one (` 3.0.0` ), and the results were massive.


p99 for React Email 2.1.6 vs 3.0.0


The tests evaluated the startup time, plus the time to render the first email preview, and were executed on a Linux machine with 3.7 GHz 12-core AMD Ryzen 9 5900x CPU, 16 GB 3600 MHz DDR4 memory (see[benchmark details](https://github.com/resend/react-email/tree/canary/benchmarks/preview-server) ).


Here's the breakdown of the performance improvements:


**P99 (99th percentile)**


- Old version: **11331ms**
- New version: **975ms**


We are committed to providing a fast and efficient local development experience, and we hope these improvements will help you build better emails faster.


## Support for React 19 RC


Even though React 19 is still a Release Candidate, we have made some changes to ensure compatibility in the future.


With this, you won't have to worry about overriding things when trying React 19 with React Email, and you will be able to make it work just as it would with older versions.


To make that happen, we had to implement a few internal changes so that the` render` function works 100% with React 19.


## Deprecation of renderAsync


The future of React includes async rendering - there is no avoiding it. Suspense and Server Components will be the norm, and we want to ensure React Email is ready for that.


The new version of React will deprecate` renderToStaticMarkup` , which would break our old` render` function.


That is why we decided to deprecate our old` renderAsync` in favor of a new` render` function, which will be async by default.


If you are using` renderAsync` now, you need to replace it with the new` render` . If you are using the old` render` , you have to treat the new Promise that it returns.


This is going to pave the way for future headache-free upgrades for all users.


## Supabase Email Hook


Supabase users have been asking for a better way to customize their authentication emails.


Before, you had to create a React Email template, then export the HTML result, and copy it to the Supabase UI. Needless to say, this manual process was hard to maintain.


Now, Supabase released a new feature called[Email Hooks](https://supabase.com/docs/guides/auth/auth-hooks/send-email-hook?queryGroups=language&language=http) that allows you to use a custom email provider to send emails directly from edge functions.


This enables you to use React Email templates directly in Supabase:


- [Read the docs](https://supabase.com/docs/guides/functions/examples/auth-send-email-hook-react-email-resend)
- [Find the code](https://github.com/supabase/supabase/tree/master/examples/edge-functions/supabase/functions/auth-hook-react-email-resend)


Thor from Supabase recorded a video showing how it works:


## Upgrade instructions


1. Update your` react-email` package to` 3.0.0` .


```text
npm i react  -  email@latest
```


1. Update your` @react-email/components` package to` 0.0.23` .


```text
npm i @react  -  email  /  components@latest
```


You can also check if you have any missing dependencies for your emails to be bundled.


## Conclusion


If you want to know all the details about this release, check the[React Email Changelog](https://react.email/docs/changelog) .


If you have any problems upgrading, feel free to share on[GitHub](https://github.com/resend/react-email) or[X](https://x.com/resend) .
