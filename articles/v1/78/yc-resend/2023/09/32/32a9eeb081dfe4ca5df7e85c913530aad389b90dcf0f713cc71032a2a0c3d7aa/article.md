---
schema_version: "1.0.0"
document_id: "32a9eeb081dfe4ca5df7e85c913530aad389b90dcf0f713cc71032a2a0c3d7aa"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/how-to-send-emails-using-bun"
published_at: "2023-09-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:bafb2bbb12d7a29b03d91b1a516c79735e4f42b047091ab763f4821071ef7095"
---

# How to send emails using Bun

## What is Bun again?


[Bun](https://bun.sh/) is an all-in-one runtime for JavaScript. It comes with a bundler, test runner, and package manager.


The best thing about Bun is its speed. It's way faster than Node.js and Deno, which makes it pretty compelling for edge computing.


Since lots of emails are sent via background jobs running in edge functions, we decided to explore how to send emails using Bun.


## Starting the HTTP server


To get started, we will create an` index.tsx` file and include a simple HTTP server that returns a "Hello World" message.


```text
const   server   =     Bun  .  serve  (  {      port  :     3000  ,        fetch  (  request  )     {          return     new     Response  (  'Hello World!'  )  ;        }  ,     }  )  ;
console  .  log  (  `  Listening on localhost:  ${  server  .  port  }  `  )  ;
```


Run the local server by executing` bun index.tsx` on the terminal. Once you navigate to` http://localhost:3000` , you will see the message.


Now that we have this foundation, we can add the actual email functionality.


## Adding Resend for email sending


First, we import the` resend` package and create a new client that will authenticate using a[Resend API key](https://resend.com/api-keys) .


Then, we call the` send` method and return the response object. In this example, we're sending an email using the` html` property.


```text
import     {     Resend     }     from     'resend'  ;
const   resend   =     new     Resend  (  process  .  env  .  RESEND_API_KEY  )  ;
const   server   =     Bun  .  serve  (  {      port  :     3000  ,        async     fetch  (  )     {          const   data   =     await   resend  .  emails  .  send  (  {          from  :     'Acme <onboarding@resend.dev>'  ,          to  :     [  'delivered@resend.dev'  ]  ,          subject  :     'Hello World'  ,          html  :     '<p>It works!</p>'  ,          }  )  ;
return     new     Response  (  JSON  .  stringify  (  data  )  )  ;        }  ,     }  )  ;
console  .  log  (  `  Listening on http://localhost:  ${  server  .  port  }   ...  `  )  ;
```


## Integrating React Email templates


Being able to send an email using HTML is nice, but being able to use an engine like[React Email](https://react.email/) is even better.


To make things interesting, let's create a new` waitlist-email.tsx` file that will render a beautiful waitlist email.


```text
import     {        Body  ,        Container  ,        Head  ,        Heading  ,        Html  ,        Preview  ,        Text  ,     }     from     '@react-email/components'  ;     import     *     as     React     from     'react'  ;
interface     WaitlistEmailProps     {      name  :     string  ;     }
export     const     WaitlistEmail     =     (  {   name   }  :     WaitlistEmailProps  )     =>     (        <  Html  >          <  Head     />          <  Preview  >  Thank you for joining our waitlist and for your patience  </  Preview  >          <  Body     style  =  {  main  }  >            <  Container     style  =  {  container  }  >              <  Heading     style  =  {  h1  }  >  Coming Soon.  </  Heading  >              <  Text     style  =  {  text  }  >              Thank you   {  name  }   for joining our waitlist and for your patience. We             will send you a note when we have something new to share.             </  Text  >            </  Container  >          </  Body  >        </  Html  >     )  ;
const   main   =     {      backgroundColor  :     '#000000'  ,      margin  :     '0 auto'  ,     }  ;
const   container   =     {      margin  :     'auto'  ,      padding  :     '96px 20px 64px'  ,     }  ;
const   h1   =     {      color  :     '#ffffff'  ,      fontSize  :     '24px'  ,      fontWeight  :     '600'  ,      lineHeight  :     '40px'  ,      margin  :     '0 0 20px'  ,     }  ;
const   text   =     {      color  :     '#aaaaaa'  ,      fontSize  :     '14px'  ,      lineHeight  :     '24px'  ,      margin  :     '0 0 40px'  ,     }  ;
```


Now that we have this template, we can import it into our` index.tsx` file and use it to send an email.


```text
import     {     Resend     }     from     'resend'  ;     import     {     WaitlistEmail     }     from     './waitlist-email'  ;
const   server   =     Bun  .  serve  (  {      port  :     3000  ,        async     fetch  (  )     {          const   data   =     await   resend  .  emails  .  send  (  {          from  :     'Acme <onboarding@resend.dev>'  ,          to  :     [  'delivered@resend.dev'  ]  ,          subject  :     'Hello from Bun + Resend + React Email 🫓💌'  ,          react  :     WaitlistEmail  (  {   name  :     'Vitor'     }  )  ,          }  )  ;
```


By running` bun index.tsx` , you should see the email being sent using React.


## Learn more


We're excited to see what you build with Bun and Resend.


If you want to see the full code, you can check out the[GitHub repository](https://github.com/resend/resend-bun-example) .
