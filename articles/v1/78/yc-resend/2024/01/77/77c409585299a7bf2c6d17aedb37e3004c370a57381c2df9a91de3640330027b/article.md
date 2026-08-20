---
schema_version: "1.0.0"
document_id: "77c409585299a7bf2c6d17aedb37e3004c370a57381c2df9a91de3640330027b"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/react-email-2"
published_at: "2024-01-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:26:19.240569+00:00"
content_hash: "sha256:2235e3ee47c4deff00d1faeb986598d8282a0f4c769ad647ca86a4477b4c01f0"
---

# React Email 2.0

We're excited to introduce React Email 2.0, featuring:


- **Re-imagined preview experience**
- **Massive performance improvements**
- **New components**
- **Deployment to Vercel**
- **Better support for monorepos**


Update today and check theupgrade instructions below.


```text
npm i react  -  email@latest
```


React Email has now **114,583 weekly downloads** on npm, and **10,924 stars** on GitHub. We would like to thank all the **106 contributors** that made this possible.


## Re-imagined preview experience


The preview server is the core of the React Email experience.


It's what allows you to see how your templates look in the browser without having to send tons of real emails. It's what makes the feedback cycle much faster, and the developer experience much better. That's why we decided to re-imagine it from the ground up.


**New mobile preview**


You can now see how your emails will look like on a small screen without having to resize the browser window or open the DevTools.


**Revamped file tree**


The file tree now supports subdirectories, so you can organize your emails however you want. You can also hide the file tree to get more space for the email preview.


**Better user feedback**


Powered by[Emil's Sonner](https://sonner.emilkowal.ski/) , the new preview server will show you a message after you send a test email.


**Improved error handling**


The new preview server will show you a message when something goes wrong, so you don't have to guess what happened.


## Massive performance improvements


One thing we believe is really impactful for developer experience is how fast you can install and get things up and running.


We tested the old version of React Email (` 1.10.1` ) against the new one (` 2.0.0` ), and the results were staggering.


While testing the local server start up times with a fresh installation, the old version took **~40s** for all the tests we ran. The new version, took, at most, only **~7s** .


Once the initial setup is done, you should see much faster boot up times around **~1s** .


~40s before and now ~7s


The tests were run on a Linux machine with a 3.6 GHz 6-core AMD Ryzen 5 4500 CPU, 16 GB 3600 MHz DDR4 memory, and a download speed of about 114 mbit/s.


These tests were made considering the first experience the user would have by deleting both the` .react-email` and` .next` folders.


Less time waiting for downloads and installs, means more time doing the actual work.


## New components


One thing we've always hated is seeing code displayed as an image instead of actual text. It's not only annoying, but it's also not accessible. You can't copy the code, and you can't search for it.


When it comes to email, we've seen this happen a lot. That's why we decided to add new components to React Email to make it easier to display code in your emails.


**Code Block**


With the new[code-block](https://react.email/docs/components/code-block) component, you can render code with a selected theme and syntax highlight powered by Prism.js.


```text
import     {   CodeBlock  ,   dracula   }     from     '@react-email/components'  ;
const     Email     =     (  )     =>     {        const   code   =     `  {    max-width: 80vh;    width: 100%;   }  `  ;
return     <  CodeBlock code  =  {  code  }   lineNumbers theme  =  {  dracula  }   language  =  "css"     /  >  ;     }  ;
```


Code Block Example


**Inline Code**


And with the new[code-inline](https://react.email/docs/components/code-inline) component, you can display a predictable inline code HTML element that works on all email clients.


```text
import     {   CodeInline   }     from     '@react-email/components'  ;
const     Email     =     (  )     =>     {        return     (          <  p  >          Use   <  CodeInline  >  transform  <  /  CodeInline  >   and  {  ' '  }            <  CodeInline  >  opacity  <  /  CodeInline  >     in     CSS     for   60fps animations  .          <  /  p  >        )  ;     }  ;
```


Code Inline Example


## Deployment to Vercel and others


Something that we really needed ourselves was being able to deploy the React Email preview directly to Vercel, and now we have been able to do so.


This new version makes it so much more easier to deploy, not only to Vercel but any cloud provider, when compared to the previous version.


Here are the steps to deploy your React Email app to Vercel:


1. Add "build" script to` ./package.json`


```text
{        "scripts"  :     {          "build"  :     "email build"        }     }
```


1.


Change "Framework Preset" on Vercel's project settings to Next.js


2.


Change "Output Directory" to` .react-email/.next`


In the end, your settings should look like this:


Vercel configuration for React Email


This will enable deployments to both production and preview URLs.


## Better support for monorepos


React Email is now compatible with turborepo, npm, bun, pnpm, and yarn workspaces.


No more downloads, no more package manager installation hell.


To fix this, we now run esbuild on the emails. This allows for bundling which will bring along your installed dependencies, making the behavior for rendering much more predictable. It also means that if you are using code from other packages on your monorepo, it will bring it along into the preview without any hastle.


turbo / npm / bun / pnpm / yarn


Before we would have the preview client on our repo, and then we would download it based on a tag for its version. Now, once you install the CLI globally it will come along with the preview client out-of-the-box.


## Upgrade instructions


1. Update your` react-email` package to` 2.0.0` .


```text
npm i react  -  email@latest
```


1. Update your` @react-email/components` package to` 0.0.14` .


```text
npm i @react  -  email  /  components@latest
```


You can also check if you have any missing dependencies for your emails to be bundled.


## Conclusion


If you want to know all details about this release, check the[React Email Changelog](https://react.email/docs/changelog) .


If you have any problems upgrading, feel free to share on[GitHub](https://github.com/resend/react-email) or[X](https://x.com/resend) .
