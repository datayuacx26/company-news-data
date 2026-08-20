---
schema_version: "1.0.0"
document_id: "a76b754440da8d184bc870be6aae3a3b8b2e4b748ab1a3219cd6adfe17486d0b"
company_key: "yc-mocha"
company: "Mocha"
source_id: "yc-mocha-rss-d0ffed2c2227"
canonical_url: "https://getmocha.com/how-to-accept-payments-stripe"
published_at: "2026-01-16T00:00:00+00:00"
first_seen_at: "2026-07-24T11:28:42.148680+00:00"
fetched_at: "2026-08-20T02:22:16.718062+00:00"
content_hash: "sha256:f8bda132cb8554f9fd8153f935e9cba85f0aa3eb0091bf32bb3daef4e8dc0db5"
---

# How to Accept Payments in Your App with Stripe

import PromptBox from '@/components/PromptBox';


We're going to build a payment hub for users to create and register for workshops or events hosted either online or at a physical location. The workshops will accept payments from registered attendees, all handled by Stripe.


## Why Stripe?


Stripe is an easy-to-use integration that gets you up and running and accepting payments in no time. You can quickly set up a Stripe account that allows you to accept payments from all types of methods, including credit cards, debit cards, electronic wallets, and more.


To get started, head over to[stripe.com](https://stripe.com/) and sign up for an account.


## Start Your App on Mocha


The foundational prompt matters. When you first create your app on Mocha, make sure your genesis prompt is clear and precise. Use exact language to touch on all the important aspects of the build, and provide as much detail as possible about what you're trying to build.


Here's the prompt we'll use:


<PromptBox client:load prompt={\`Build a workshop booking and payment system for experts who want to sell online sessions.


As the workshop host, I want to:


- Create workshops with a title, description, date/time, duration, and price
- Set a maximum number of attendees
- See who has registered and paid


As an attendee, I want to:


- Browse available workshops
- Pay to register for a workshop
- Receive confirmation of my booking
- See my upcoming registered workshops


Use Stripe for payment processing. The checkout flow should be smooth — when someone clicks "Register", they pay through Stripe and get confirmed automatically.


Include proper webhook handling so registrations are only confirmed after successful payment. Show sold out status when workshops reach capacity.\`} />


Notice how this prompt includes:


- A clear description of the app's purpose and audience
- Specific features broken down by role
- Technical requirements (Stripe, webhook handling)
- Experience expectations (smooth checkout flow)


This level of detail helps Mocha understand exactly what you're building and sets a solid foundation for the rest of the development process.


Once you've submitted the initial prompt, you should see an input field asking you to add a few API keys from your Stripe account. We can retrieve them from the Stripe dashboard.


## Setting Up the Stripe API Key


During the build, you should see a prompt to add secrets for Stripe. You'll need:


- ` STRIPE_SECRET_KEY`
- ` STRIPE_WEBHOOK_SECRET`


If you need a walkthrough for adding secrets, click the walkthrough button and follow the instructions.


### How to Get Your` STRIPE_SECRET_KEY`


1. Visit[dashboard.stripe.com](https://dashboard.stripe.com/)
2. Click on the **Developers** menu in the bottom left corner
3. Click on the **API keys** option third from the bottom
4. Create a new "Secret key" by clicking the **Create secret key** button
5. Copy the **Secret key** and paste it into the` STRIPE_SECRET_KEY` field in your Mocha app. The secret key should begin with the letters` sk_live_` .


> **Important:** Make sure it's the secret key, not the publishable key. The publishable key starts with` pk_live_` while the secret key starts with` sk_live_` .


## Configuring Webhooks


This is the most involved piece of the setup. You'll be asked to provide a webhook secret, but you must first look for the webhook URL in your Mocha app. In order for Stripe to access your webhook URL, you'll need to publish your app to the public internet.


### Publish Your App First


Pro plans can publish their app to a custom subdomain. I've named mine` workshopstudio.mocha.app` . If you're on a free plan, your app will be published to a random` .mocha.app` subdomain. Pay attention to what your URL is, because we'll use it to set up our webhook in Stripe.


### Finding the Webhook URL


Now go back to the Mocha chat log and find the part that mentions the webhook endpoint. It will look something like this:


Notice how the Mocha AI agent mentions the` /api/webhooks/stripe` endpoint. This is the URL that Stripe will use to send webhooks to your app.


Now take the published URL of your app and add the` /api/webhooks/stripe` endpoint to it. For example, if your app is published to` https://workshopstudio.mocha.app` , the full URL will be` https://workshopstudio.mocha.app/api/webhooks/stripe` . This is the URL we'll use to set up our webhook in Stripe.


### Setting Up the Webhook in Stripe


In the Stripe Dashboard:


1. Go to **Developers → Webhooks**
2. Click **Add destination**
3. Select events: In my example, the Mocha AI agent wanted me to listen for` checkout.session.completed` events. You can listen for other events as well, but for this example, we'll use` checkout.session.completed` .


1. Set the endpoint URL to your Mocha app's webhook route (in my example, it's` https://workshopstudio.mocha.app/api/webhooks/stripe` )


1. Copy the **Signing secret** and paste it into the` STRIPE_WEBHOOK_SECRET` field in your Mocha app.


> **Note:** The webhook signing secret should begin with the letters` whsec_` .


## Testing Payments


Once you've set everything up correctly, payments should start appearing in your Stripe account from your Mocha app. Test this by using your live published Mocha app and sending payments to your Stripe account.


## Wrapping Up


In this guide, we walked through the process of setting up a Mocha app that accepts payments through Stripe. We covered:


- Crafting a strong initial prompt
- Setting up Stripe API keys
- Configuring webhooks for payment confirmation


Stripe does deduct a small fee from each payment. This amounts to roughly 2.9% + $0.30 per transaction, but this may vary depending on your country and the type of payment method used.


You can see the full source code for the app we built by cloning the[Workshop Payments Example App](https://workshopstudio.mocha.app/) .


---


**Ready to start accepting payments in your app?**


- [Start building for free with Mocha →](https://getmocha.com/)
- [Read the Mocha documentation →](https://docs.getmocha.com/)
