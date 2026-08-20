---
schema_version: "1.0.0"
document_id: "a3d419e7d3a4aa5b6f90eb76ce11a7f93eaf51b70456cfbb762920c432eb5c21"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/changelog-07-2026/"
published_at: "2026-07-13T13:00:00+00:00"
first_seen_at: "2026-07-22T15:22:16.880049+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:547c8d6ed49fc5c9c21818097226d49eaf16a1eda005d9b22e7e0b369baeb9af"
---

# Svix Changelog July 2026

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


Summer months tend to be slower, but not this summer. This summer we have a whopping list of updates, and many more in the pipeline!


Here are the updates for July:


## Webhooks AutoConfig


We launched what we call[Webhooks AutoConfig](https://www.svix.com/blog/webhooks-autoconfig/) , which we believe makes it much easier to consume webhooks.


Webhooks AutoConfig is a declarative way to configure webhooks. Instead of configuring webhooks subscriptions in the UI, it's done in code, and automatically stays up to date as the receiver code updates. You can read more about it in the[announcement post](https://www.svix.com/blog/webhooks-autoconfig/) .


## AI agent skills


AI agents already do a great job integrating with Svix just by reading the documentation. However, the Svix AI skills include more context for the agents helping it make decision about best integration strategies for a specific project, making for an even faster integration.


We also created a Standard Webhooks and Svix compatible webhook receiving skill that can be used by your customers. It follows webhook ingestion best-practices and makes consuming webhooks a breeze.


Try it out at[the Svix AI repo](https://github.com/svix/ai) on Github.


## Consumers can stop webhook delivery


Svix now has a better way for webhook consumers to control their subscription behaviors. Webhooks consumers can now optionally include the` webhook-delivery` header when responding to webhooks and set it either to` disable` which will disable the endpoint in its entirely (in addition to respecting` 410 Gone` dictated by standard webhooks), or` abort-message` which will cancel retries for that specific message.


This is useful for a variety of use-cases, for example` abort-message` is useful when an endpoint already consumed an event even though it returned an error previously (so it knows for sure that no retries are necessary), and` disable` is useful for a tool like Zapier that may ask deliveries to stop if it knows that the related workflow has been cancelled.


## Dashboard analytics improvements


We've added more to the dashboard's usage screen, including counts of applications with active endpoints or subscriptions, counts of enabled and disabled endpoints, and the ability to filter by event type to find the most active applications; as well as the ability to choose custom date ranges rather than the previous preset ones.


We also added event type stats on the application pages, showing a break down of activity by event type in addition to the overall success and failure counts.


## UI for additional advanced endpoints


Svix supports a variety of[Advanced Endpoint Types](https://docs.svix.com/advanced-endpoints) to let you easily send your webhooks to where your customers would like to receive them.


We have no added the missing UI for a variety of additional sink types. These sink types have been around for a couple of years, with this change finally adding them to the UI. This makes it easy to consume webhooks directly in tools like AWS SQS, ClickHouse, Redshift, BigQuery and more.


## OpenTelemetry configuration improvements


We've cleaned up the OpenTelemetry header configuration UI in the dashboard, making it easier to configure OpenTelemetry streaming.


## Improved self-serve SSO setup


The SSO setup process has been greatly improved, letting Enterprise customers setup SSO in a more self-serve manner while maintaining the same level of security offered by the less automatic workflow from before.


## Passkeys for email and password login


Accounts using email and password authentication can now enroll passkeys, giving you a faster and more phishing-resistant way to sign in.


## Endpoint secret rotation with expiry


When rotating an endpoint's signing secret, you can now pass an expiry for the old secret. Leaving it unset keeps the default overlap window, and passing` 0` expires the old secret immediately.


## Editing API token scopes


You can now edit the scopes of an existing API token directly in the dashboard, as well as in Terraform, instead of having to delete and recreate the token.


## Support for the "canceled" message status in the SDKs


Following up on last month's change to display transformation-canceled attempts as canceled, the SDKs now understand the` canceled` message status returned by the API, so you can distinguish canceled deliveries from successful ones in your own code.


## Transformations UI improvements


The transformations editor now shows the "Test" tab side by side with your code, so you can iterate without switching back and forth. We've also given the overall transformations screen a cleaner layout based on customer feedback.


## Transformation variables


Transformations now support a` variables` field for storing key-value pairs, so you no longer have to hardcode values inside the transformation code. You can set these variables from a dedicated UI in the app portal and reference them from your transformation.


## Copy prompt for LLM in connectors


The "Generate with AI" button in connectors has been replaced with a "Copy prompt for LLM" button. Instead of calling out to a hosted model, we now hand you the full prompt, including your event type schemas and connector context, so you can generate a transformation with the LLM of your choice.


## Tailscale as an ingest provider


You can now use Tailscale as a Svix Ingest provider, making it easier to receiver Tailscale webhooks. We have been using Tailscale with Svix Ingest, and it has been great. Glad to be releasing this one.


## Copying event types between environments


You can now copy an event type, or several at once, from one environment to another with a single button. This makes promoting event types from staging to production much easier.


## Better MFA error handling


We've improved the error messages and error handling in the MFA flow so it's clearer what went wrong and how to recover when signing in with multi-factor authentication.


## Closing words


As always, please let us know if you have any feedback on the product, or anything else we can add or build to make your lives easier. A lot of the Svix product came from customer feedback, and we are sure` ${next_big_thing}` will come from feedback as well. No feedback is too small!


You can[contact us](https://www.svix.com/contact/) directly or join the discussion on[our community Slack](https://www.svix.com/slack/) .


---


For more content like this, make sure to follow us on[X / Twitter](https://x.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
