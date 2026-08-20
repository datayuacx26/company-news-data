---
schema_version: "1.0.0"
document_id: "5dbfcea2058d26026c1cda1d09c7f7d9d670e5c2221bfedfd484fb25356818d3"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/stripe-projects-blaxel-integration"
published_at: "2026-06-10T05:38:56+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:49:15.049109+00:00"
content_hash: "sha256:4fc8089c35b5ea130ddc29800a7ed326e1ec4b29b62d530b0a2c489a968fca81"
---

# Let your agents provision Blaxel resources with Stripe

Agents are getting smarter every day. Now, they can read documents, prepare slides, perform advanced calculations, write and execute code...the list goes on. But what they couldn't easily do was register themselves autonomously as users for more sensitive tasks: making payments for services, spinning up infrastructure, deploying code to prod.


To eliminate this friction, Stripe recently launched[Stripe Projects](https://docs.stripe.com/projects) . Stripe Projects lets you or your agents provision multiple services, generate and store credentials, and manage usage and billing without hopping across multiple dashboards. It is designed for AI-assisted workflows, enabling easy set up of hosting, databases, authentication, AI, analytics, and more directly from the CLI.


Blaxel is now compatible with Stripe Projects, which means that your agents can now use your Stripe credentials and predefined payment methods to directly provision paid Blaxel accounts, as well as[sandboxes](https://docs.blaxel.ai/Sandboxes/Overview) and[drives](https://docs.blaxel.ai/Agent-drive/Overview) , all without human intervention after the initial setup.


## How it works


Stripe Projects leverages Stripe as the trusted infrastructure between AI agents and third-party providers like Blaxel. Because Stripe already knows who you are and has your payment method on file, Stripe Projects can authenticate with Blaxel on your behalf, create an account and workspace, and sign up for services - all without needing to request your payment details.


See how it works:[https://www.tella.tv/video/how-to-use-stripe-projects-x-blaxel-2v00](https://www.tella.tv/video/how-to-use-stripe-projects-x-blaxel-2v00)


## Getting started


### Prerequisites


- [Stripe CLI](https://docs.stripe.com/stripe-cli/install) installed
- A Stripe account with at least one saved payment method (if none is set, you will be prompted to add one during the plan selection stage)


A Blaxel account is not needed; Stripe Projects will create one automatically.


### Setup


Install the Stripe Projects plugin:


bash


` stripe plugin


install


projects


`


Create a new project (you will be prompted to log in to Stripe):


bash


` stripe projects init my-app


`


### Select a service plan


Blaxel is[fully usage-based via credits](https://blaxel.ai/pricing) , and implements a tiered quota system. Higher tiers are dependent on recent credit topups and give access to higher concurrency and retention limits. Your credits are consumed based on your usage of storage (Agent Drive, sandbox snapshots) and compute runtime (sandboxes).


So, before provisioning a sandbox or Agent Drive, you (or your agent) must activate a top-up plan for one of the tiers via Stripe Projects. Check the available plans:


bash


` stripe projects catalog blaxel


`


Select a plan:


bash


` stripe projects


add


blaxel/tier-1


`


Stripe will directly communicate with Blaxel's API, create an account, and activate the selected plan. Payment will be executed through your existing payment methods via a[Shared Payment Token (SPT)](https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens) - a Stripe feature to grant access to a user's payment method - without any additional input from you.


### Start provisioning


Once the plan is activated, you (or your agent) can start provisioning sandboxes and drives on Blaxel:


bash


` stripe projects


add


blaxel/sandbox


stripe projects


add


blaxel/drive


`


Once provisioned, use the Stripe CLI to create a local environment file (` .env` ) with the necessary credentials:


bash


` stripe projects


env


--pull


source


.env


`


Blaxel account credentials and workspace will have been automatically added to your local environment by Stripe Projects. You can now[install the Blaxel CLI](https://docs.blaxel.ai/cli-reference/introduction) and start using it directly:


bash


` brew


install


blaxel


export


BL_API_KEY


=


$BLAXEL_PLAN_API_KEY


bl login


$BLAXEL_PLAN_WORKSPACE


bl new sandbox`


## What's next


With a sandbox or Agent Drive provisioned, your agents can access isolated compute environments and persistent shared storage without any manual credential management. Learn more:


- [Sandboxes documentation](https://docs.blaxel.ai/Sandboxes/Overview)
- [Agent Drive documentation](https://docs.blaxel.ai/Agent-drive/Overview)
- [Stripe Projects documentation](https://docs.stripe.com/projects)


Questions?[Ask us on Discord](https://discord.gg/enAfyZFWHW) .
