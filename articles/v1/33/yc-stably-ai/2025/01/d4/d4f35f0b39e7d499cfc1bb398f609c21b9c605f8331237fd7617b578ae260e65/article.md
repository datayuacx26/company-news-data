---
schema_version: "1.0.0"
document_id: "d4f35f0b39e7d499cfc1bb398f609c21b9c605f8331237fd7617b578ae260e65"
company_key: "yc-stably-ai"
company: "Stably AI"
source_id: "yc-stably-ai-news-import-4d9fcc9f146f"
canonical_url: "https://www.stably.ai/blog/mastering-stripes-test-mode"
published_at: "2025-01-10T00:00:00+00:00"
first_seen_at: "2026-07-24T02:44:25.851186+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:37fa51d8809a3c15dab76a3d62cfa8a8ab9b4fe934ae3929a1770867d6fc79df"
---

# Mastering Stripe’s Test Mode

Hey developers! Let's talk about one of the most crucial aspects of integrating Stripe: test mode. Whether you're building a new payment integration or debugging an existing one, understanding test mode is essential for shipping with confidence.


## What's Test Mode, Anyway?


Think of test mode as your payment integration's sandbox - a safe space where you can experiment without the fear of accidentally charging real cards or moving actual money. It's a complete simulation environment that mirrors Stripe's live environment but with zero financial risk.


## Test Mode vs Live Mode: The Deep Dive


When working with Stripe's API, every request happens in either test or live mode. Here's what you need to know about each:


### Test Mode


- Use this while building and testing your integration
- Card networks and payment providers don't process actual payments
- Perfect for simulating objects like accounts, payments, customers, charges, refunds, transfers, and subscriptions
- Can only use test credit cards and accounts
- Identity verification checks are simulated
- Connected account objects won't return sensitive fields
- API keys start with "sk_test_" and "pk_test_"


### Live Mode


- This is where real money moves
- Card networks and payment providers actively process payments
- Works with real credit cards and customer accounts
- Handles actual payment authorizations, charges, and captures
- More complex flows for disputes and certain payment methods
- API keys start with "sk_live_" and "pk_live_"


```text
// Test mode API key
const stripe = require('stripe')('sk_test_...');


// Live mode API key
const stripe = require('stripe')('sk_live_...');


```


Important note: The test mode toggle in Stripe's Dashboard doesn't affect your integration code - it's your API keys that determine whether you're in test or live mode.


## Testing Common Scenarios


The real power of test mode comes from simulating different payment scenarios. Here's how to test some common cases:


```text
// Test card numbers for different scenarios
const testCards = {
success: '4242424242424242',
declined: '4000000000000002',
insufficientFunds: '4000000000000341'
};


// Immediate capture
const charge = await stripe.paymentIntents.create({
amount: 2000,
currency: 'usd',
payment_method: 'pm_card_visa',
confirm: true
});


// Authorization with delayed capture
const authIntent = await stripe.paymentIntents.create({
amount: 2000,
currency: 'usd',
capture_method: 'manual',
payment_method: 'pm_card_visa',
confirm: true
});


```


A more in depth list of test cases can be found[here](https://docs.stripe.com/test-mode)


## Pro Tips


1. **Email Testing:** By default, Stripe doesn't send emails in test mode. You'll need to manually trigger them for testing customer communications.
2. **Cleaning Up:** You can easily delete all test data from your Dashboard under Developers > Overview. Just remember - it can't be undone!
3. **Webhook Testing:** Test your webhook handlers thoroughly. Failed payouts and successful transfers are especially important to verify.


## Moving to Production


When you're ready to go live:


1. Toggle off test mode
2. Replace your test API keys with live ones
3. Make sure you've tested all critical payment flows
4. Verify your webhook handling
5. Double-check your error handling


Remember: test card numbers won't work in live mode, so don't accidentally leave them in your production code! Testing thoroughly in test mode can save you from many headaches down the line. Happy coding! 🚀
