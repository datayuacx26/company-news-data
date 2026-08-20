---
schema_version: "1.0.0"
document_id: "6f6ffffb10db872dc1f4f0c494c94b2fbbcbdf484e63488bb757d4bbdf9d5035"
company_key: "yc-autumn"
company: "Autumn"
source_id: "yc-autumn-news-import-b2979559dee3"
canonical_url: "https://useautumn.com/blog/component-fail"
published_at: "2025-06-30T00:00:00+00:00"
first_seen_at: "2026-07-21T08:43:21.090032+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:42a9bf33bd1b68a81ffbd7a992c3318c5ad0a00cdcd415c31c0b799d35c4c7e8"
---

# React vs shadcn, and trying to be too clever

You know those moments where you have an idea so brilliant you feel like Steve Jobs? Launching our component library felt like that. Novel, high risk, and a radically different approach to what was on the market. If it worked out, it'd be incredible.


Unfortunately, it didn't. We ended up with a weird mess and had to spend the last week rewriting everything.


*For context: we're building Autumn, pricing and billing software. Part of our offering is frontend components for things like a pricing table, paywalls etc. You can read more about them*[here](https://docs.useautumn.com/quickstart/shadcn) *.*


### **V1 as a React library**


We first attempted this as a standard npm React library:


```text
import PricingPage from "autumn-js"


```


It's biggest issue was customisability**.** There are two ways to make npm components customisable: through props, or a no-code interface in our app.


1.


With props there were too many class variables. For example with the pricing page, each pricing card is composed of many headers, descriptions, buttons and more. And each card itself can have variants (recommended, discounts, annual etc).


2.


A low code interface is a bad experience for devs, who want full customisability and have 10x experience designing with tailwind/css. Even more so with the era of cursor. This is subjective but I’ve never had a good experience with one.


*Sidenote:* If you ever try to make your npm component customisable via tailwind, all the best. That was one of the most frustrating days of our lives.


Also, If you ever forgot what jsx styles look like, here’s a snippet of what we had to write…


We made one measly post on linkedin about it, decided it was unusable and shelved it.


### **V2 as shadcn/ui components**


A few weeks later we started looking into it again with shadcn.


```text
npx shadcn@latest add https://ui.useautumn.com/classic/pricing-table.json


```


This immediately felt like a more customizable and fluid DX, but had its own challenges:


1.


Because shadcn components install as a user file, we cannot fully control it via our SDK. For example, if we wanted to trigger a modal/popup to confirm a plan upgrade, the user needs to explicitly pass it into a function. This does take away slightly from that “magical DX”.


```text
//upgrading to Pro tier
<  Button
onClick  ={async   ()   =>
await   attach  ({
productId:   "pro"  ,
dialog: ProductChangeDialog,
})
}
/>;
```


2.


Since users own and control the component files, deciding what data abstraction level to return to the frontend is hard. For example, our upgrade dialog shows different text and styling depending on the scenarios:


- One time purchase vs subscription
- Upgrades vs downgrades vs cancellations vs renewals
- Does the upgrade require an input from users (eg, quantity of credits to purchase)


With a React library, everything would be "completely processed", with no customizability. Initially this is the same approach we took with the shadcn registry, but users quickly told us they wanted to customize the text too. We switched to the "in-between" approach.


Our API will return a scenario (eg, upgrade, downgrade, add-on, free-trial etc), and our shadcn components install with a library of cases that users can edit to control the messaging.


```text
switch   (scenario) {
case   "scheduled"  :
return   {
title: <  p  >Scheduled product already exists</  p  >,
message: <  p  >You will downgrade on   {  scheduled_date  }  </  p  >,
};


case   "active"  :
return   {
title: <  p  >Product already active</  p  >,
message: <  p  >You are already subscribed to this product.</  p  >,
};


case   "new"  :
if   (recurring) {
return   {
title: <  p  >Subscribe to   {  product_name  }  </  p  >,
message  : (
<  p  >
By clicking confirm, you will be subscribed to   {  product_name  }   and
your card will be charged immediately.
</  p  >
),
};
}
//..... etc
```


However, the shadcn library turned out as a huge mess for several reasons:


**Mistake 1**


We decided to launch our components as a shadcn package instead of plain React. Our components approach was inspired by Clerk, but I always thought they looked out of place and wanted users to own them. Shadcn had launched registry functionality so people could share and download components, and[Supabase](https://supabase.com/ui) launched one too, so it seemed promising.


Unfortunately only half our users use shadcn, but the others wanted components and couldn't use them. More importantly, we were iterating quickly on the underlying API that controls the component content. Since the components weren't synced to our SDK, every update we made broke integrations. We should have kept it simple with React to start, then expanded to shadcn once stable.


**Mistake 2**


We launched a separate open source pricing component library called[pricecn](https://useautumn.com/blog/pricecn.com) . We thought it would go viral like[React Email](https://react.email/) by Resend. We designed it so pricecn users could easily migrate to using Autumn components later later, and so made the Autumn component library have the pricecn library as a dependency.


Pricecn didn't take off. People liked generating pricing cards from JSON, but it's hard enough to promote one project, let alone two. Having Autumn components depend on pricecn ones was just a confusing experience for users, as each component came with several files in different folders.


**Mistake 3**


We launched with 3 different styles: classic, clean and dev. I thought this would grow adoption since people could pick what suits them.


Again, people liked the concept but it made maintenance hell. Every issue meant jumping into 3 component files, so we kept putting off fixes until it became unusable.


**What we have now**


1.


We launched a React library as the core experience, but allowing users to download the same files as shadcn components to customize it


2.


We only kept the "classic" style and simplified it.


3.


We cut the dependency on pricecn.


We kept the shadcn components since people liked owning them when they worked, and now our API is a little more stable, it's working better.


Maybe we're still trying too hard to be clever but here's the cool part: instead of maintaining two sets, our shadcn library automatically replicates from our React ones so they always stay synced. Here's the script that syncs them whenever we` npm run dev` :


[https://github.com/useautumn/autumn-js/blob/main/package/scripts/sync-registry.ts](https://github.com/useautumn/autumn-js/blob/main/package/scripts/sync-registry.ts)


To be honest we have no idea how that script works but it does. Thanks Claude Code!
