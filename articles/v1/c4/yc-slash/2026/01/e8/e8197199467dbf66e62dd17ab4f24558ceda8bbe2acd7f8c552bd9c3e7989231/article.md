---
schema_version: "1.0.0"
document_id: "e8197199467dbf66e62dd17ab4f24558ceda8bbe2acd7f8c552bd9c3e7989231"
company_key: "yc-slash"
company: "Slash"
source_id: "yc-slash-news-import-9ec40849fc98"
canonical_url: "https://www.slash.com/engineering/blog/intern-blog"
published_at: "2026-01-06T23:00:43.091+00:00"
first_seen_at: "2026-07-24T01:09:43.930358+00:00"
fetched_at: "2026-07-28T22:24:09.396466+00:00"
content_hash: "sha256:d8f7db0969973bc187b94ef39647de6843726fc35a64b6a4e5aef9ada5e141fa"
---

# My Internship with Slash

Hi there! I’m Joseph, a Waterloo CS student, and I recently wrapped up a Software Engineering internship at Slash from September–December 2025. To put this blog shortly – I had a blast and think you should join. Thanks for reading.


Okay, for real this time. I’ve previously interned at a variety of companies, from tiny music-related seed startups to Big Tech companies, but Slash offered a unique blend of intensity and ownership that I hadn't found elsewhere. The team was smart and tightknit, and I learned in-depth about a lot of complex topics from maintaining Kubernetes to DB performance. More importantly, there were some really interesting challenges that I got to solve, which is why I decided to write a blog about my experiences!


To give a quick summary of what Slash does, they started off as a financial platform for people like entrepreneurs and sneaker resellers, but are now a general banking fintech company that offers corporate cards, international transfers, crypto accounts, and more. Now, I didn’t know much of this when I first came across Slash. After all, I’m not a sneakerhead, nor was I deep into fintech, so I spent a lot of time thinking about whether this opportunity was worth it or not. However, as you can tell by this blog post, my choice to join ended up being one of the best decisions I could have made for my early career.


The main reasons why I ultimately chose Slash over the other companies was because:


- **It was a series B company** – I’ve never worked at a startup of this size, and wanted to experience that environment.
- **They were at 150M in annualized revenue, which is incredibly high for a series B company** – they’re not just surviving or hanging in there; they were actively scaling. As an added bonus, that meant they likely had good DX and engineering rigor that many small startups lack.
- **They were actively growing** – they 6x-ed their revenue in one year and recently raised their series B. This company was in its golden age of scale, and I wanted to work at a company experiencing that kind of momentum.
- **It’s a young, high-energy team** – From my interviews I could tell people here were chill and enjoyable to talk with. I was excited to build with peers that I could relate to.
- **It’s a small and lean company** – did I mention that this 150M ARR was done by a team of ~12 engineers? That’s 12.5M ARR *per* engineer, which is wild. Clearly, the people at Slash are top talent, and I’d have both high ownership and great mentorship.
- The compensation and small-company perks are top-tier for internships… 🤫


**TLDR** , I thought the environment, the talented folk, and the startup energy would provide an incredible place to grow. I figured I’d get the chance to own a project end-to-end, from setting up infrastructure to planning around scalability to thinking about system design and more, and so I took the chance!


## Project: Notifications V2


Here’s one special thing I immediately learned about Slash – they assign important projects to interns and trust that they can drive it through. In my second week, I was tasked with revamping our card transaction decline notifications. Doesn’t seem that hard, right? However, the existing notifications system had a few issues:


- Tight coupling with our internal event system, with business logic baked into the core of the service
- Poor email metrics and worse email deliverability due to the provider that we used
- Difficult developer experience, which led engineers to bypass the system entirely and directly call provider APIs


And so came the insidious task – to build better decline emails, we needed more than a patch. We needed a fully redesigned notifications system, and I became the DRI for design, development, rollout, and scaling for the entire project. That was super intimidating… but also an incredible opportunity to architect something that would eventually handle millions of notifications. I was excited to hop in.


From studying the previous notification system and discussing with engineers, I unearthed a few tenets that this new system should adhere to:


1. **An event-agnostic design** – the service should be able to work anywhere, whether in one-time scripts or event handlers
2. **A provider-agnostic design** – the opportunity to easily remove and introduce new providers with minimal effort would be priceless.
3. **A developer-first design** – no more overriding from frustration. The development experience should be as intuitive and easy as possible.
4. **Observable and scalable** - the notifications should have full lifecycle tracking, and we should eventually be able to support 3 million emails monthly!


And from this, the handler-based design of Notifications V2 was born. The system is centered around notification handlers, which expose common handler functions, such as sending notifications or processing webhooks, for specific external providers of channels (e.g. Resend for emails, Twilio for SMS).


At the core of the system is the` NotificationIntent` , a generic transient object used to carry all the data for a notification such as provider, channel, and payload arguments. Each handler defines the shape of its own intents, and these objects are strongly typed and thus support static-time typechecks and introspection. This means that developers don’t need to know all the intricacies of each provider – they can take advantage of the IDE’s auto-complete and trust that typecheck will carry them through.


This example briefly illustrates how to use the service to send a variety of email notifications for an outgoing payment. All these create functions create notification intents, and they are strongly typed to suit the provider (in this case, Resend)


```text
const   sendOutgoingPaymentNotification   =   async   (context:   {
transactionId:   string  ;
amount:   number  ;
recipientEmail:   string  ;
alertEmail:   string  ;
}) => {
const   {   transactionId,   amount,   recipientEmail,   alertEmail   }   =   context  ;
const   {   originalSenderId   }   =   getOriginalSenderForTransaction  (  transactionId  );


await   notificationsV2Service.sendNotifications  ([
// We can create a one-off notification easily...
ResendNotification.create({
email: {
recipient: alertEmail,
content: {
subject:   "INTERNAL ALERT: An employee just sent an outgoing payment"  ,
body:   `  An   employee just sent an outgoing payment of   $$  {amount}.`  ,
},
},
}),
// Or use a React Email template for pretty emails..
ResendNotification.create({
email: {
recipient: recipientEmail,
content: YouJustReceivedPaymentTemplate.withProps({ amount }),
},
}),
]);


//   Or   send   to   specific   users,   respecting   their   preferences   +   registered   emails
await   notificationsV2Service.sendNotificationsToUsers  ({
users:   [originalSenderId],
preference:   NotificationPreferences.PaymentConfirmation,
createIntents:   (user) =  >   [
ResendNotification.create(  {
email:   {
recipient:   user.registeredEmail,
content:   PaymentConfirmationTemplate.withProps  ({ amount }),
},
})  ,
],
});
};
```


To ensure that business logic, like checking user preferences, was implemented in a low-coupled way, once a developer calls a send function, the intents pass through a` NotificationMiddleware` pipeline first. Notification middlewares are higher-order functions that have a preprocess step to allow for shared DB lookups, and a process function that lets us allow, transform, or drop notification intents. With this design, we can now define different send functions that use one underlying system to define and share validation logic. For example:


- We can define a basic “send notification” function that has intents flow through a` RecipientBanned` middleware, which drops intents if the recipient information (email, phone number) is on a custom banlist
- We can define a more complex “send to user” function that has intents flow through the same` RecipientBanned` middleware as well as a` RespectUserPreferences` middleware, which drops intents if the user has opted out of notifications on that channel


As an added benefit, it’s easy for future developers to add, remove, or modify new sending functions without having to trace a maze of functions.


An example of some flows that we decided to use. Notice how sendNotificationToUsers and sendNotification both use the heuristics middleware, but the user-sending function runs more user-specific validators.


After the middleware step, the service creates` Notification` lifecycle objects before starting a Temporal workflow. Temporal is great because it gives us built-in retries for workflow failures, durable state, and orchestration guarantees. These are all important to ensure deliverability of the notifications. The workflow queues the notifications, throttles sends to respect provider rate limits and so we don’t spam users, and then passes batches to provider handlers. The handlers then take responsibility for sending the notification to the provider and the service updates the lifecycle accordingly.


Once whatever external provider sends us a webhook response, the handlers convert them into a standardized data type and emit typed actions. Actions give us a structured and type-safe way to respond to events when receiving a notification, while keeping the service as unopinionated as possible. For example,


- We have an action for updating recipients – This way, handlers can update recipients as "permanently banned” or not depending on what the provider and channel determines is “banned”.
- We have an action for retrying a notification – for example, if an email temporarily bounces, we want to exponentially retry the email. If the handler determines that the email provider’s response was “bounced”, it can trigger this action.


ProcessWebhook returns the same body structure, allowing specific webhook logic to be handled by the actual provider itself.


```text
const   statusMap:   Record  <  string,   strin  g  >   =   {
bounced:   "failed",
//   more   statuses   here...
};


const   ResendHandler   =   createHandler  ({
//   This   is   a   simple   example   of   something   we   could   do
processWebhook:   (webhookBody:   ResendWebhook  ) =  >   {
const   notificationId   =   extractIdFromWebhook  (  webhookBody  );


const   newInternalStatus   =   statusMap[webhookBody.state]  ;


const   actions   =   newInternalStatus   ===   "failed"
?   [{   action:   "retryNotification",   notificationId   }]
:   [];


return   {
status:   newInternalStatus,
actions,
};
},
});
```


Whew! That’s most of the flow. Along the way, I ran into several other interesting engineering challenges, like


- Supporting exponential retries – We schedule future retry jobs inside Temporal depending on the number of retries already done
- Good traceability – For example, we store trace ids, as well as notifications and their retries, in a` NotificationSet` entity
- Batching – Integrating both individual and batching send allows us to avoid exceeding provider rate limits and gives developers control over how they want notifications sent.


I’ll let any prospective new engineers discover the details in the code, once they join 😉


## Project: Facelift


Project Facelift was our team’s end-to-end redesign of the Slash web experience (see the eng blog[here by Albert Tian, the father of facelift](https://www.slash.com/engineering/blog/facelift) ). Now I won’t go too in-depth on what Facelift was (again, see the wonderful eng blog), but at its core it was introducing a new component library + Tailwind to our entire massive frontend. Now, to prepare for the launch, the engineering team decided to have a little fun and run a focused hackweek! This wasn’t your typical hackweek at the office though – we got into a couple cars, drove three hours to a cabin at Bass Lake beside Yosemite, and spent the week building, refactoring, and genuinely having fun with one another.


During this week, I shifted entirely from backend work to help facelift the transactions and dispute flows. This was mostly frontend work, which was a refreshing change of pace from my previous work, and also meant I got to work across much of our frontend design infrastructure, such as:


- **Core primitives** : for example, how we generate our side panels, modals, and popups
- **Reusable composable components** : for example,` SelectableSearch`
- **Table manager** : our custom component to build tables with easy sorting, filtering, pagination, and search
- **Robust subform validation** : using Arktype, Tanstack forms, and our own model generation system


Facelifting was less about complex product logic and more about witnessing how fast our team can move when focused and aligned on one collective project. Watching the entire site transform over the week, alongside building and bonding with my colleagues in said cabin, was definitely a highlight of my internship. Facelift work didn’t end that week though – for the next couple of weeks we made sure the launch went smoothly by working with support to crush nits around the product!


## Project: Templates


At the end of every work term, my university likes to send out a little form containing a simple question – “How applicable was your coursework to your job?”. Well, during my time at Slash, I actually got to apply some concepts I learned in class directly! You see, we wanted our transactional emails to match our new Facelift design, so it made sense for our notifications system to support custom templates. To ensure we used the same colours and spacing as our frontend, we ended up building these templates in code using React Email, a templating email library allowing us to use JSX for these templates. This was perfect for us; since our team is intimately familiar with React JSX, we could ensure brand consistency by using the same Tailwind CSS v4 design system across both web and email.


However, there was one catch – one of the key perks of TailwindCSS 4 is its ability to use custom CSS files instead of the traditional JS config object, which our team leveraged. However, React Email’s Tailwind component actually *required* the JS config object.


This meant I got to build a transpiler!


```text
@import   "tailwindcss"  ;


@import   "../custom-styles.css"  ;


@custom-variant   dark (&:where(.dark, .dark *):not(:where(.light, .light *)));


@theme   {
--color-  *  : initial;


--spacing-base-scale-0: 0px;
--spacing-base-scale-1: 1px;
--spacing-base-scale-2: 2px;
--spacing-base-scale-4: 4px;
--spacing-base-scale-6: 6px;
--spacing-base-scale-8: 8px;
--spacing-base-scale-12: 12px;
--spacing-base-scale-14: 14px;
--spacing-base-scale-16: 16px;
--spacing-base-scale-20: 20px;
--spacing-base-scale-24: 24px;
--spacing-base-scale-28: 28px;
--spacing-base-scale-32: 32px;
--spacing-base-scale-36: 36px;
--spacing-base-scale-40: 40px;
--spacing-base-scale-44: 44px;
--spacing-base-scale-48: 48px;
--spacing-base-scale-52: 52px;
--spacing-base-scale-56: 56px;
--spacing-base-scale-60: 60px;
--spacing-base-scale-64: 64px;
--spacing-base-scale-68: 68px;
--spacing-base-scale-72: 72px;
--spacing-base-scale-76: 76px;
--spacing-base-scale-80: 80px;
--spacing-base-scale-84: 84px;
--spacing-base-scale-88: 88px;
--spacing-base-scale-92: 92px;
--spacing-base-scale-96: 96px;
--spacing-base-scale-100: 100px;
--spacing-base-scale-104: 104px;
```


```text
// WARNING: This file is generated by a script. Do not edit it manually.
// Generated from: ./lib/tailwind.build.css
// To regenerate: yarn build:tailwind-config


import   type   { GenericAny }   from   '@slashfi/slash-base'  ;


const   plugin   =   require  (  'tailwindcss/plugin'  );


export   default   {
theme: {
extend: {
backgroundColor: {
"surface-subtle"  :   "#f9f8f7"  ,
"surface-neutral"  :   "#ffffff"  ,
"neutral-subtle-default"  :   "#fcfcfb"  ,
"neutral-subtle-hover"  :   "#f9f8f7"  ,
"neutral-subtle-active"  :   "#f9f8f7"  ,
"neutral-subtle-disabled"  :   "#fcfcfb"  ,
"neutral-normal-default"  :   "#f9f8f7"  ,
"neutral-prominent-default"  :   "#f4f3f1"  ,
"neutral-muted-default"  :   "#e9e7e3"  ,
"neutral-bold-default"  :   "#958f82"  ,
"neutral-bold-hover"  :   "#6d685f"  ,
"neutral-bold-active"  :   "#6d685f"  ,
"neutral-bold-disabled"  :   "#bdb7aa"  ,
"neutral-muted-hover"  :   "#e0ddd6"  ,
"neutral-muted-active"  :   "#d9d6ce"  ,
"neutral-muted-disabled"  :   "#efeeeb"  ,
"neutral-prominent-hover"  :   "#efeeeb"  ,
"neutral-prominent-active"  :   "#efeeeb"  ,
"neutral-prominent-disabled"  :   "#fcfcfb"  ,
"neutral-normal-hover"  :   "#f4f3f1"  ,
"neutral-normal-active"  :   "#f4f3f1"  ,
"neutral-normal-disabled"  :   "#fcfcfb"  ,
"neutral-bolder-default"  :   "#6d685f"  ,
"neutral-boldest-default"  :   "#32312c"  ,
"neutral-boldest-hover"  :   "#32312c"  ,
"neutral-boldest-active"  :   "#32312c"  ,
"neutral-boldest-disabled"  :   "#bdb7aa"  ,
"neutral-bolder-hover"  :   "#32312c"  ,
"neutral-bolder-active"  :   "#32312c"  ,
"neutral-bolder-disabled"  :   "#bdb7aa"  ,
"interactive-neutral-subtle-default"  :   "#4b3a1c"  ,
"interactive-neutral-normal-default"  :   "#4b3a1c"  ,
"interactive-neutral-prominent-default"  :   "#4b3a1c"  ,
"interactive-neutral-prominent-hover"  :   "#4b3a1c"  ,
"interactive-neutral-prominent-active"  :   "#4b3a1c"  ,
"interactive-neutral-prominent-disabled"  :   "#4b3a1c"  ,
"interactive-neutral-normal-hover"  :   "#4b3a1c"  ,
"interactive-neutral-normal-active"  :   "#4b3a1c"  ,
"interactive-neutral-normal-disabled"  :   "#4b3a1c"  ,
"interactive-neutral-subtle-hover"  :   "#4b3a1c"  ,
"interactive-neutral-subtle-active"  :   "#4b3a1c"  ,
```


To more precisely describe it, during the build step, I added a script to build an Abstract Syntax Tree (AST) from the design system CSS files. This AST provided a structured representation of the CSS rules, and allowed me to easily parse the file to:


- **Filter out unsupported CSS classes** : for example, hover styles and pointer utilities don’t work in most email clients.
- **Convert to a compatible colour system** : We used OKLCH to define our colours, but email clients can’t render that, so these are converted to HEX.
- **Build the TS configuration object** : This gets compiled into JS, and then can be easily imported anywhere in the codebase.


This transpilation step was a fast way to implement a single source of truth for our design system. If a colour was tweaked, it would be consistent throughout all our mediums – web or email – since it would be immediately transpiled once rebuilt. Additionally, other clients, like our mobile application, could use this transpiled object as needed. With the generated configuration object, I was able to build a set of common React components for our emails that mirrored our web components, like` Typography` and` Button` components, with nearly identical props. All this helped improve developer experience, making templating less of a chore!


As a final note, I also made notification templates leverage our internal registry framework, a custom data structure that we built (see[this blog post](https://www.slash.com/engineering/blog/scaling-1m-lines-of-typescript-registries) ). As a quick summary, registries gives us a type-safe way to define templates, serialize and deserialize them from the database, and keep business logic defined together. For example, the` card-authorization.notification-template.tsx` lives directly beside the card authorization event handler code, which is great for quickly seeing what events have notifications! It was great to be able to use registries in this project – it really illustrated the team’s commitment to write good, reusable code.


### What did I take away?


I came in thinking this would be a unique experience, and I’m happy I decided to join – Slash taught me more about ownership, platform design, and engineering velocity than any role I’ve had before. Some key takeaways I got was:


- **Trust is invaluable** – Slash has no daily standups. Slash has no weekly sprints or backlog grooming. We have one meeting a week, and that’s just to demo things we’ve built to the entire team. This works because the team overall has strong communication and impressive ownership skills. That’s not to say there is no mentorship – I have weekly one-to-ones to unblock myself – but instead, Slash trusts you to do your best work and cultivates an environment where you can lock in as much as possible.
- **The team is incredible** – In my other co-ops, my colleagues were professional, but here, my coworkers were *my people* . It’s clear that Slash has a great culture with fun, supportive, and easy-to-work with people – there wasn’t a single person who I didn’t enjoy working with or who I was intimidated by, and they treat you just like a full-timer. Plus, you can go shoot hoops with them on the weekends 😃
- **As an intern, you will grow exponentially here compared to anywhere else** – At any other company, the intern would not have been given the entire core notification system as a project. The difficult, growth-inducing problems would have either been solved or are being solved by some senior engineer. What makes Slash unique is the combination of lean teams, real business-critical problems, and a culture that trusts even the interns with important work.


Here are some other fun memories from this internship:


- We ate around a collective 20lbs of steak in one week… thanks hack week!
- I crushed my fellow intern in a pool competition and then immediately got humbled by a full timer.
- I finally managed to bench one plate!
- I am a key contributor to “office-wide gluttony” (and potentially related, I found out I love Shake Shack).


These 4 months flew by faster than I expected, and I’m grateful for the experience I’ve had at Slash. I’ll be graduating soon, and regardless of what life has in store for me a month, year, or decade after, I know that interning at Slash has given me the knowhow and confidence to succeed no matter where I go.


If you’re interested in chatting more,[feel free to reach out to me on LinkedIn!](https://www.linkedin.com/in/wang-joseph) Happy to answer any questions you might have.


## Read more from us


### [One Engineer, Three Months, Zero Compromises](https://www.slash.com/engineering/blog/new-mobile-app)


≈ 17 minutes read


### [Building Card Transaction Processing at Slash](https://www.slash.com/engineering/blog/card-transaction-processing)


≈ 12 minutes read


### [Building Banking-Grade Stablecoin Rails at Slash](https://www.slash.com/engineering/blog/banking-grade-stablecoin-rails)


≈ 19 minutes read


### [The Facelift](https://www.slash.com/engineering/blog/facelift)


≈ 15 minutes read


### [Winning & Culture](https://www.slash.com/engineering/blog/winning-and-culture)


≈ 4 minutes read


### [Scaling 1M lines of TypeScript: Registries](https://www.slash.com/engineering/blog/scaling-1m-lines-of-typescript-registries)


≈ 5 minutes read


### [Health Checks](https://www.slash.com/engineering/blog/health-checks)


≈ 7 minutes read


### [Type challenge: Datadog Logs Preview](https://www.slash.com/engineering/puzzles/datadog-preview-type-challenge)


≈ 2 minutes read
