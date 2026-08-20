---
schema_version: "1.0.0"
document_id: "7bd569f7ee04ae3b78fb240a6e0e1fd478b33f2437b1f5f46f1b66ee67309b13"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/how-vercel-uses-react-email-for-next-js-conf"
published_at: "2024-09-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:4ed88e5470cd2578f5ab5a86f019a5636e88de4353ee9e33391fc04f1cf70fea"
---

# How Vercel uses React Email for Next.js Conf

Vercel runs two conferences every year:[Vercel Ship](https://vercel.com/ship) and[Next.js Conf](https://nextjs.org/conf) .


This year, Vercel started using[React Email](https://react.email/) for Vercel Ship and continued for Next.js Conf. We met with[John Phamous](https://x.com/JohnPhamous) , the Lead Design Engineer at Vercel, to learn how the Vercel team used React Email to streamline email template development.


## What is React Email?


React Email is an open source framework that enables developers to create responsive and elegant email templates using React components.


[react.email](https://react.email/)


It leverages the familiar JSX syntax to streamline the design and development of consistent email layouts. It also ensures compatibility across different email clients, making it easier to maintain uniform styles.


## How Vercel uses React Email


Before React Email, the team used email templates with HTML strings that had to be manually updated for each conference and email type.


New team members working with John on the conference had to be onboarded to learn how the email template worked and how to edit it.


A simple change like changing a URL became a hassle with the previous template. Implementing the great design and branding Vercel is known for became a challenge.


> "We could seamlessly onboard new teammates on the email templates using React development workflows they were already familiar with. A developer could take the same mental models of working on the frontend of Vercel to their email design."
>
>
> John Phamous
>
>
> Lead Design Engineer


Starting with Vercel Ship this year, John and his team used React Email to manage their email templates. The emails were designed in Figma, and the team implemented them seamlessly utilizing the React framework. With the Next.js Conf coming up in October, John used the same reliable process.


How the final email looks


### Q: What made using React Email seamless compared to other options?


**A:** Using React Email helped avoid single ownership of conference emails. Instead of reading three pages of internal documentation to make a single change, we could seamlessly onboard new teammates on the email templates using React development workflows they were already familiar with. A developer could take the same mental models of working on the frontend of Vercel to their email design.


I especially liked how the team could use components to reuse common patterns like headers and footers across their emails. Previously, we would have to copy/paste each common pattern into each HTML string for each email.


The team was able to set ticket types, such as` virtual` or` in person` , within the email template.


```text
interface     RegistrationConfirmationEmailTemplateProps     {      type  :     'virtual'     |     'in-person'  ;     }
```


Then, they could format the email differently based on which ticket type they selected, all using the same email template logic.


```text
import     {        Section  ,        Text  ,        Row  ,     }     from     '@react-email/components'  ;
const     Content  :     Record  <        RegistrationConfirmationEmailTemplateProps  [  'type'  ]  ,        React  .  ReactNode     >     =     {      virtual  :     (          <  Section     className  =  "  p-[24px]  "  >            <  Row     className  =  "  w-full text-left  "  >              <  Text  >  Thanks for registering for Next.js Conf 2024.  </  P  >              <  Text  >                <  strong  >  Here's what to do next:  </  strong  >              </  Text  >              <  div     className  =  "  text-left  "  >                <  div  >                  <  CalendarLinks                    ical  =  "  https://www.addevent.com/event/id22506000+apple  "                    google  =  "  https://www.addevent.com/event/id22506000+google  "                    outlook  =  "  https://www.addevent.com/event/id22506000+outlook  "                  />                </  div  >              </  div  >            </  Row  >          </  Section  >      ),   };
```


The resulting email was then customized to the recipient.


## Try for yourself


If you're already familiar with React Email, you might want to see what was released in the new major version.


[React Email 3.0 An entire collection of pre-built components with a much faster development experience. resend.com/blog/react-email-3](https://resend.com/blog/react-email-3)


If you haven't used React Email yet, take a look at the[documentation](https://react.email/docs/introduction) .
