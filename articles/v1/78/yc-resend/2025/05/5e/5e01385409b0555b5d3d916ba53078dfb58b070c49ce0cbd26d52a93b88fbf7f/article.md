---
schema_version: "1.0.0"
document_id: "5e01385409b0555b5d3d916ba53078dfb58b070c49ce0cbd26d52a93b88fbf7f"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/engineering-idempotency-keys"
published_at: "2025-05-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:b21397a16259b0f2cec477b81fc8e8f289758035f1884994bad751b5952d3589"
---

# Engineering Idempotency Keys

We just[released idempotency keys](https://resend.com/changelog/idempotency-keys) for our email API.


While adding idempotency to applications is a common pattern, the process includes a lot of details. We learned valuable lessons we'd like to share about the process.


## What is idempotency?


An idempotent operation is an action you can perform more than once, with the same input, and it always produces the same outcome and avoids repeating side effects.


Once an operation is idempotent, you can safely retry it without risking causing its effects more than once (e.g., sending an email again, using up quota twice, etc.).


## How to send an idempotent email using Resend


We added support for idempotency keys on the` POST /emails` endpoint.


By adding an` Idempotency-Key` header, or using the equivalent field on our SDKs, you can tell Resend that this specific email should only be sent once, even if we get more than one request from you about it.


```text
await   resend  .  emails  .  send  (        {          from  :     'Acme <onboarding@resend.dev>'  ,          to  :     [  'delivered@resend.dev'  ]  ,          subject  :     'hello world'  ,          html  :     '<p>it works!</p>'  ,        }  ,        {          idempotencyKey  :     '9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d'  ,        }  ,     )  ;


```


We mark the request as idempotent by checking the` Idempotency-Key` header and the request payload.


- **Key** : the same key (256 characters max)
- **Timing** : sent within 24 hours
- **Payload** : the same payload in the request body


Sending the same payload when you retry a request with an idempotency key is important. We check the request payload to avoid unexpected side effects, such as suppressing unrelated emails. For instance, if you accidentally set the same string for all your idempotency keys, on different emails, and we didn't check that, only your first email would be delivered. By checking the payload, we can detect this is a mistake and let you know in the API response.


## How idempotency benefits our customers


Idempotency is a key ingredient for building reliable systems.


Here are a few key benefits for our customers:


- Dramatically simplifies retries for failed requests
- Facilitates writing resilient, self-healing applications
- Prevents duplicate emails for end users
- Saves resources for both Resend and our customers


## How to format your idempotency key


The exact format of your idempotency keys is up to you, but here are some recommendations.


Idempotency keys can be formatted to include an event type (e.g.,` order-sent` ) and an entity ID (e.g.,` 12345678` ).


Here are some common scenarios:


**Order tracking** : combine an order ID with an event type


```text
`  order-created/12345678  `     `  order-shipped/12345678  `     `  order-delivered/12345678  `
```


**Single-events** : for events that happen only once per user, combine the user ID with an event type


```text
`  user-sign-up:8ac1f001-b745-4b45-aab1-68ddd6c9da49  `
```


**Daily notifications** : consider including the date in the key


```text
`  2025-05-07#user928472#daily_notification  `
```


Note that for us there is no special meaning to the separator characters or event names. As long as the keys are 1–256 characters, Resend will hash and store them. The specific format you use is up to you.


## Patterns to avoid


Importantly, for idempotency to work, the key must identify exactly one email you want to send. Avoid the following patterns:


**Hard-coded keys only**


```text
// Bad -- only the first user will get the welcome email,     // other emails will fail due to idempotency key reuse     const   idempotencyKey   =     'welcome-email'
// Good     const   idempotencyKey   =     `  welcome-email#  ${  user  .  id  }  `
```


**Random UUIDs for the same email**


```text
// Bad -- this is the same as not having idempotency keys     // at all, as if you run this function again it will     // generate a new key and the email will be sent again.     const   idempotencyKey   =     randomUUID  (  )
```


## Our implementation details


When we receive an email to send with a valid idempotency key, we first check against existing emails with the same key. The process leads either to an error, the email being sent for the first time, or the cached response being returned.


Here is a full diagram of how we've implemented idempotency keys:


## Testing our implementation


To thoroughly test our implementation, we pursued three main strategies.


**1. Automated tests**


We wrote automated tests to validate the idempotency key implementation.


**2. Internal usage**


We added idempotency key to our own usage of Resend's API (e.g., team invitations and quota threshold notifications) and then introduced deliberate retries in some of these flows to validate that emails were not sent more than once.


**3. User testing**


We reached out to users who previously requested this feature to try it out, validate and give us feedback.


Once confident in our internal and user testing, we rolled out the feature to all users. Because using the feature requires sending a specific header, it doesn't impact existing workflows.


## Other considerations and lessons


During the implementation, we made several decisions about our implementation.


### SDKs rollout


Since many of our users use our SDKs, we needed to **model idempotency in natural ways** for each of the platforms we support.


### Implementation level


We decided to **implement it on an API request level** , instead of the data model object. This allows us to in the future generalise the implementation to other endpoints more easily, and also support endpoints that don't relate to a single entity, like the batch email endpoint.


### Request normalization


We decided to **normalize and hash the request body** and store it in the idempotency cache with its response. This allows us to check for accidental idempotency key reuse without storing large amounts of data unnecessarily.


## Resources we found useful


We referred to community resources and other tech companies implementations of idempotency and to requests from our users who were interested in this feature. Some useful resources include:


- [Stripe's documentation on idempotency](https://docs.stripe.com/api/idempotent_requests)
- [Make Your API Idempotent, Avoid Ruining Clients Lives](https://apisyouwonthate.com/blog/idemptoency-keys/)


## Conclusion


Support for idempotency keys was a highly requested feature, and we're happy to bring it to our users. We anticipate many useful use cases for it, and we're sure you will find even more.


We're excited to include this important feature in our API and hope this post helps you implement it in your own applications. If you have any questions, reach out to us and we'll be happy to help.
