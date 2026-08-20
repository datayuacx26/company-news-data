---
schema_version: "1.0.0"
document_id: "c2b89c176060f578131e5f5f19d9d9a359baba061b15fc42eadd572535e83303"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/tailwind-with-react-email"
published_at: "2023-03-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:da2ac38a87c45fc76b26c28412dc422729c04d125aea15a98026cbd726efc8ef"
---

# Send emails using React and Tailwind CSS

Libraries like[React](https://reactjs.org/) and[Tailwind CSS](https://tailwindcss.com/) have made it easier to build modern websites. However, when it comes to emails, we were still stuck with old-school HTML and CSS.


Today we're excited to announce that you can now use[Tailwind CSS](https://tailwindcss.com/) with[React Email](https://react.email/) to build beautiful emails without having to write any CSS.


> "Resend not only streamlines our emails to accommodate our expanding customer base, but their team also offered valuable hands-on support during the transition from our old API. Their product is visually stunning and seamlessly integrates with React Email."
>
>
> Thiago Costa
>
>
> Co-founder of Fey and Narative


## What is React Email again?


[React Email](https://react.email/) is an open source set of unstyled components maintained by our team to help you build beautiful emails without having to deal with` <table>` layouts and maintaining archaic HTML code.


If you're new to React Email, I highly recommend checking out the[getting started guide](https://react.email/docs) or watching this amazing video from[Chris Pennington](https://twitter.com/cpenned) :


## How to use Tailwind CSS with React Email


We recommend using[create-email](https://www.npmjs.com/package/create-email) , which is the easiest way to get started:


```text
npx create-email@latest


```


This will create a new folder called "react-email-starter" with a few email templates.


Now, import the component and wrap it around your email body:


```text
import     {   Button  ,   Tailwind   }     from     '@react-email/components'  ;
const     Email     =     (  )     =>     {        return     (          <  Tailwind  >            <  Button           className  =  "bg-brand px-3 py-2 font-medium leading-4 text-white"            href  =  "https://example.com"            >            Click me           <  /  Button  >          <  /  Tailwind  >        )  ;     }  ;
```


You can customize the default theme with the available properties in[Tailwind CSS docs](https://tailwindcss.com/docs/theme) .


> "As of our last deployment all of our emails are using Resend. We are loving the development experience of React Email - not having to leave my dev environment to develop new emails is a game-changer."
>
>
> Adam Rankin
>
>
> Founding Engineer at Warp


## Live example


If you want to see a live example, check out the[Vercel template using Tailwind](https://demo.react.email/preview/vercel-invite-user) .


Vercel template using Tailwind


## Thanks


The[Tailwind CSS integration](https://react.email/docs/components/tailwind) wouldn't be possible without the help of our incredible open source community. A special thanks to[Vinicius de Moraes](https://github.com/vinicoder) , who led the initial implementation and fixed several issues along the way.
