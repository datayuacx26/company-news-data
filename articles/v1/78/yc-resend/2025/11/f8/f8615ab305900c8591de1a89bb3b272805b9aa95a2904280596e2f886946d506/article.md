---
schema_version: "1.0.0"
document_id: "f8615ab305900c8591de1a89bb3b272805b9aa95a2904280596e2f886946d506"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/unsubscribe-topics"
published_at: "2025-11-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:c133a0396b4296f02086a80f83f72ebc7c31610e9bdd89b8e916a6174adaa431"
---

# Unsubscribe Topics

It may sound counterintuitive, but letting people unsubscribe from emails they don’t want from you is great for deliverability.


When you only send to engaged recipients, it’s good for everyone. People are happier to receive your emails, and mailbox providers evaluate you as a high-quality sender.


Today, we’re excited to introduce **Topics** , a new way to allow users to manage their email preferences.


## How it works


When you send emails to your Contacts, you can now assign a Topic to that email. For example, label your emails as "Monthly newsletter" or "Deals & savings".


When users manage their preferences, they’ll see which Topics they’re currently subscribed to and can adjust their settings to their liking.


Sometimes people in your audience want to receive monthly newsletters, but don't want to know about every sale you run. Giving people options means they can stay subscribed to what they want and unsubscribe from the rest.


The **Subscribed** status for a Contact is a global setting that enables or disables sending Broadcasts to a Contact.


- If a Contact’s **Subscribed** status is **false** , they will not receive emails from your account, even if they have opted in to a specific Topic.
- If a Contact's **Subscribed** status is **true** , they can receive Broadcast emails from your account.


## Creating a new Topic


You can create a new Topic by navigating to **Audience** > **Topics** :


You can also[create Topics using the API](https://resend.com/docs/api-reference/topics/create-topic) :


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  topics  .  create  (  {        name  :     'Store Events'  ,        defaultSubscription  :     'opt_out'  ,     }  )  ;


```


When creating a Topic, you can set the default subscription to **Opt-in** or **Opt-out** .


- **Opt-in** : Contacts will be subscribed to this Topic by default.
- **Opt-out** : Contacts will not be subscribed to this Topic by default.


For example, you can create a Topic for security updates and set the default subscription to **Opt-in** , ensuring all of your Contacts will receive these emails. On the other hand, you can create a Topic for marketing emails and set it to **Opt-out** , ensuring no one will receive these emails unless they have explicitly opted in to that Topic.


## Assigning Topics to Contacts


Each Contact can be assigned to multiple Topics. This can be done on the dashboard:


## Customizing your unsubscribe page


While Resend handles the unsubscribe flow for you, the experience should look and feel like your brand.


You can customize your[unsubscribe page](https://resend.com/settings/unsubscribe-page) by uploading a logo, adjusting the background and text colors, changing the wording, and more.


When someone unsubscribes, they'll see your custom branded page instead of a generic one.


Learn more about[customizing your unsubscribe page](https://resend.com/changelog/custom-unsubscribe) .


## The power of user preference


The best way to curate an engaged audience is to empower your users with more control over the ways they hear from you. It's a good way to meaningfully signal to your users that you care about their experience.


Unsubscribe Topics launches today. Add it to your app by[following the guide](https://resend.com/docs/api-reference/topics) .
