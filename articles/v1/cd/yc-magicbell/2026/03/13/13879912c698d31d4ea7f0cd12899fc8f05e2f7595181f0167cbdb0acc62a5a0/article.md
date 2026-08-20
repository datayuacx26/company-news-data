---
schema_version: "1.0.0"
document_id: "13879912c698d31d4ea7f0cd12899fc8f05e2f7595181f0167cbdb0acc62a5a0"
company_key: "yc-magicbell"
company: "MagicBell"
source_id: "yc-magicbell-news-import-9a6b19b555ea"
canonical_url: "https://www.magicbell.com/blog/stripe-webhooks-guide"
published_at: "2026-03-25T00:00:00+00:00"
first_seen_at: "2026-07-22T03:05:45.790203+00:00"
fetched_at: "2026-07-28T22:17:51.657377+00:00"
content_hash: "sha256:e4c750e855a03979a4e90a7ebc9374ec3e66a88fccbc474a584640f7ae753cb6"
---

# Stripe Webhooks: Complete Guide with Event Examples

**Stripe webhooks are HTTP callbacks that send real-time updates about events in your Stripe account.** When a customer pays, disputes a charge, or ends a trial, Stripe sends an HTTP POST request to your endpoint with detailed event data. This guide covers setup, security, event handling, and testing for Stripe webhooks.


While APIs let you pull data from Stripe,[webhooks push data to a webhook endpoint](https://www.magicbell.com/blog/what-is-a-webhook-and-how-does-it-work) when events happen. A payment goes through, a subscription renews, or an invoice fails, and Stripe tells your app right away.


This guide covers everything you need to set up Stripe webhooks well: creating endpoints, handling key payment and subscription events, adding security, and testing your setup.


## How Stripe Webhooks Work


Stripe webhooks follow the standard[webhook pattern](https://www.magicbell.com/blog/webhooks-tutorial) . When an event happens in your Stripe account, Stripe sends an HTTPS POST request to your webhook endpoint. The request body contains event data in JSON format.


### The Webhook Flow


1. **Event occurs** : A customer finishes a payment, a subscription renews, or an invoice fails.
2. **Stripe creates event object** : Stripe builds an Event object with details about what happened.
3. **HTTP POST sent** : Stripe sends a POST request to your webhook URL with the event data.
4. **Your application responds** : Your endpoint processes the event and returns a 2xx status code.
5. **Retry on failure** : If your endpoint does not respond with 2xx, Stripe retries the webhook.


### Event Object Structure


Every Stripe webhook contains an Event object with this structure:


```text
{
"id": "evt_1234567890",
"object": "event",
"api_version": "2024-11-20",
"created": 1732800000,
"type": "payment_intent.succeeded",
"data": {
"object": {
"id": "pi_1234567890",
"amount": 2000,
"currency": "usd",
"status": "succeeded",
"customer": "cus_1234567890"
}
}
}


```


The` type` field shows which event happened (e.g.,` payment_intent.succeeded` ). The` data.object` holds the full resource that triggered the event.


## Setting Up Stripe Webhooks


Set up webhook endpoints through the Stripe Dashboard or API to start getting event updates.


### Dashboard Configuration


1. Navigate to **Developers > Webhooks** in your Stripe Dashboard.
2. Click **Add endpoint** .
3. Enter your webhook URL (must be HTTPS in production).
4. Select events to listen for (or select "all events" during development).
5. Save and note your webhook signing secret.


### Endpoint Requirements


Your webhook endpoint must:


- Accept POST requests.
- Parse JSON request body.
- Verify webhook signatures.
- Return 2xx status code within 5 seconds.
- Process events idempotently (handle duplicates).


### Basic Endpoint Setup


```text
// Express.js example
app.post('/webhooks/stripe', express.raw({type: 'application/json'}), (req, res) => {
const sig = req.headers['stripe-signature'];
const endpointSecret = process.env.STRIPE_WEBHOOK_SECRET;


let event;


try {
// Verify webhook signature
event = stripe.webhooks.constructEvent(req.body, sig, endpointSecret);
} catch (err) {
console.log(`Webhook signature verification failed: ${err.message}`);
return res.status(400).send(`Webhook Error: ${err.message}`);
}


// Handle the event
switch (event.type) {
case 'payment_intent.succeeded':
const paymentIntent = event.data.object;
handlePaymentSucceeded(paymentIntent);
break;
case 'customer.subscription.created':
const subscription = event.data.object;
handleSubscriptionCreated(subscription);
break;
default:
console.log(`Unhandled event type: ${event.type}`);
}


// Return success response
res.json({received: true});
});


```


## Payment Events


Payment webhooks tell you about charge and payment intent lifecycle changes. These events matter most for filling orders and handling payment failures.


### Successful Payment Events


**` payment_intent.succeeded`** : Sent when a payment goes through.


```text
{
"type": "payment_intent.succeeded",
"data": {
"object": {
"id": "pi_abc123",
"amount": 5000,
"currency": "usd",
"status": "succeeded",
"customer": "cus_xyz789",
"payment_method": "pm_card123"
}
}
}


```


Use this event to:


- Fill orders and deliver digital goods.
- Send payment confirmation emails.
- Update user account status.
- Record successful transactions in your database.


**` charge.succeeded`** : Sent when a charge is created. This is similar to` payment_intent.succeeded` but works at the Charge level.


### Failed Payment Events


**` payment_intent.payment_failed`** : Sent when a payment attempt fails.


```text
{
"type": "payment_intent.payment_failed",
"data": {
"object": {
"id": "pi_abc123",
"amount": 5000,
"status": "requires_payment_method",
"last_payment_error": {
"code": "card_declined",
"message": "Your card was declined."
}
}
}
}


```


Handle failed payments by:


- Telling customers about the failure.
- Sharing retry steps or a link to update their payment method.
- Logging failure reasons for analytics.
- Sending high-value failures to your support team.


### Refund Events


**` charge.refunded`** : Sent when a charge is refunded, either fully or partially.


```text
{
"type": "charge.refunded",
"data": {
"object": {
"id": "ch_abc123",
"amount": 5000,
"amount_refunded": 5000,
"refunded": true
}
}
}


```


## Subscription Events


Subscription webhooks track the full lifecycle of recurring billing. They cover everything from trial starts to cancellations. For SaaS businesses, these events are key for access control and customer updates.


### Subscription Creation


**` customer.subscription.created`** : Sent when a customer subscribes to a plan.


```text
{
"type": "customer.subscription.created",
"data": {
"object": {
"id": "sub_abc123",
"customer": "cus_xyz789",
"status": "active",
"current_period_end": 1735689600,
"items": {
"data": [{
"price": {
"id": "price_abc123",
"product": "prod_xyz789"
}
}]
}
}
}
}


```


Actions to take:


- Grant access to subscription features.
- Send a welcome email with account details.
- Set up usage tracking or limits.
- Record the subscription start date.


### Trial Events


**` customer.subscription.trial_will_end`** : Sent 3 days before a trial ends.


```text
{
"type": "customer.subscription.trial_will_end",
"data": {
"object": {
"id": "sub_abc123",
"status": "trialing",
"trial_end": 1735603200
}
}
}


```


This event lets you:


- Remind users to add a payment method.
- Show product value to boost conversions.
- Offer limited-time discounts.
- Prevent churn by addressing concerns early.


You can[automate subscription update notifications](https://www.magicbell.com/workflows/stripe/customer-subscription-updated) so no customer slips through the cracks at this key conversion moment.


### Subscription Updates


**` customer.subscription.updated`** : Sent when subscription details change (plan upgrade/downgrade, payment method update, etc.).


Handle this event to:


- Adjust feature access based on the new plan.
- Calculate prorated charges or credits.
- Update billing info in your database.
- Notify the customer of changes.


### Subscription Cancellation


**` customer.subscription.deleted`** : Sent when a subscription is canceled and ends.


```text
{
"type": "customer.subscription.deleted",
"data": {
"object": {
"id": "sub_abc123",
"status": "canceled",
"canceled_at": 1732800000
}
}
}


```


Key actions:


- Revoke access to subscription features.
- Send a cancellation confirmation.
- Trigger win-back campaigns.
- Archive user data per your retention policy.


## Invoice Events


Invoice webhooks handle billing cycles, payment collection, and dunning. These events matter for keeping subscription revenue healthy.


### Invoice Payment Failed


**` invoice.payment_failed`** : Sent when automatic payment collection fails for an invoice.


```text
{
"type": "invoice.payment_failed",
"data": {
"object": {
"id": "in_abc123",
"customer": "cus_xyz789",
"amount_due": 2000,
"attempt_count": 1,
"next_payment_attempt": 1732886400
}
}
}


```


This event needs quick action:


- Tell the customer about the failed payment.
- Share a link to update their payment method.
- Set up an escalation flow (retry, then suspend, then cancel).
- Track failed payment metrics.


[Automate invoice failure handling](https://www.magicbell.com/workflows/stripe/invoice-payment-failed) to notify customers right away and escalate to your billing team after repeated failures.


### Invoice Payment Succeeded


**` invoice.payment_succeeded`** : Sent when invoice payment succeeds.


Use this to:


- Send a payment receipt to the customer.
- Extend the subscription period.
- Reset payment failure counters.
- Update accounting records.


### Invoice Upcoming


**` invoice.upcoming`** : Sent 1 day before an invoice is created for the next billing period.


```text
{
"type": "invoice.upcoming",
"data": {
"object": {
"customer": "cus_xyz789",
"amount_due": 2000,
"next_payment_attempt": 1733097600
}
}
}


```


This is a chance to communicate early:


- Remind customers of the upcoming charge.
- Give them time to update their payment method.
- Preview the invoice amount for transparency.
- Reduce involuntary churn.


## Webhook Security Best Practices


Stripe webhooks carry sensitive customer and payment data. Add security measures to block unauthorized access and protect data integrity.


### Verify Webhook Signatures


Every Stripe webhook includes a` Stripe-Signature` header with an[HMAC signature](https://www.magicbell.com/tools/hmac-generator) . Always check this signature before processing events.


```text
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);
const endpointSecret = process.env.STRIPE_WEBHOOK_SECRET;


// Verify signature
try {
event = stripe.webhooks.constructEvent(
req.body,
req.headers['stripe-signature'],
endpointSecret
);
} catch (err) {
console.log(`Signature verification failed: ${err.message}`);
return res.status(400).send(`Webhook Error: ${err.message}`);
}


```


The` constructEvent` method checks that the webhook came from Stripe and was not changed in transit. Never skip signature checks in production.


### Handle Duplicate Events


Stripe may send the same webhook more than once due to network issues or retries. Design your webhook handler to process events idempotently.


```text
async function handlePaymentSucceeded(paymentIntent) {
// Check if already processed
const existing = await db.payments.findOne({
stripe_payment_intent_id: paymentIntent.id
});


if (existing) {
console.log(`Payment ${paymentIntent.id} already processed`);
return;
}


// Process payment and mark as complete
await db.payments.create({
stripe_payment_intent_id: paymentIntent.id,
amount: paymentIntent.amount,
status: 'completed',
processed_at: new Date()
});
}


```


Use the event ID (` event.id` ) or resource ID (` payment_intent.id` ) to track what you have already processed. This prevents duplicate actions.


### Use HTTPS Endpoints


Stripe requires HTTPS for production webhook endpoints. This encrypts webhook data in transit and prevents eavesdropping or tampering.


For local development, use the Stripe CLI to forward webhooks to your local HTTP endpoint safely.


### Respond Quickly


Return a 2xx status code within 5 seconds. Do this before running any slow operations:


```text
app.post('/webhooks/stripe', async (req, res) => {
// Verify signature
const event = verifyWebhook(req);


// Immediately return success
res.json({received: true});


// Process event asynchronously
processEventAsync(event).catch(err => {
console.error(`Error processing event ${event.id}:`, err);
});
});


```


If your endpoint times out, Stripe retries the webhook. This can cause duplicate processing.


## Testing Stripe Webhooks


Test webhooks well before going to production. Stripe offers testing tools and a local development workflow to help. You can also[test your Stripe webhook integration](https://www.magicbell.com/tools/webhook-tester) with a free webhook tester to inspect payloads before writing any handler code.


### Stripe CLI for Local Testing


The Stripe CLI forwards webhook events from Stripe to your local development server:


```text
# Install Stripe CLI
brew install stripe/stripe-cli/stripe


# Login to your Stripe account
stripe login


# Forward webhooks to local endpoint
stripe listen --forward-to localhost:3000/webhooks/stripe


```


The CLI outputs a webhook signing secret for local testing. Use this secret in your local environment variables.


### Trigger Test Events


Send test webhook events straight from the CLI:


```text
# Trigger successful payment
stripe trigger payment_intent.succeeded


# Trigger subscription creation
stripe trigger customer.subscription.created


# Trigger invoice payment failure
stripe trigger invoice.payment_failed


```


### Test Event IDs


Stripe provides test event IDs that trigger webhooks in test mode:


```text
// Create test PaymentIntent that will trigger webhook
const paymentIntent = await stripe.paymentIntents.create({
amount: 2000,
currency: 'usd',
payment_method: 'pm_card_visa',
confirm: true
});


```


Use test cards like` 4242 4242 4242 4242` for successful payments. Use` 4000 0000 0000 0002` for declined cards. These generate different webhook scenarios.


### Monitor Webhook Deliveries


Check webhook delivery status in the Stripe Dashboard under **Developers > Webhooks** . This shows:


- Recent webhook attempts (successful and failed).
- Response status codes from your endpoint.
- Response time.
- Retry attempts.


You can manually retry failed webhooks from the dashboard for debugging.


## Handling 200+ Stripe Events


Stripe creates over 200 different webhook event types. You do not need to handle every one. But knowing the event categories helps you build solid setups.


### Event Naming Convention


Stripe webhook events follow the pattern` resource.action` :


- ` payment_intent.succeeded` - PaymentIntent succeeded.
- ` customer.subscription.deleted` - Subscription deleted.
- ` invoice.payment_failed` - Invoice payment failed.


### Common Event Categories


**Checkout Events** :` checkout.session.completed` ,` checkout.session.expired`
**Customer Events** :` customer.created` ,` customer.deleted` ,` customer.source.expiring`
**Dispute Events** :` charge.dispute.created` ,` charge.dispute.closed`
**Fraud Events** :` radar.early_fraud_warning.created` ,` radar.early_fraud_warning.updated`
**Payout Events** :` payout.paid` ,` payout.failed`


### Event Filtering


Pick which events your endpoint gets in the Stripe Dashboard. This cuts down on extra webhook traffic and processing work.


During development, enable all events so you can see the full Stripe event flow. For production, subscribe only to events your app handles. This saves resources.


## Automating Stripe Webhooks with Workflows


Custom webhook endpoints give you full control, but they take time to build and maintain. For common Stripe notification flows, automation tools can remove the need for custom code.


### No-Code Workflow Automation


[MagicBell's Stripe integration](https://www.magicbell.com/workflows/stripe) handles webhook events and triggers notification workflows on its own. You do not need to write webhook handlers. Just connect your Stripe account and set up notification templates.


For example, to notify customers about trial endings:


```text
{
"key": "integration.stripe.customer.subscription.trial_will_end",
"steps": [
{
"command": "broadcast",
"notification": {
"title": "Your trial ends in 3 days",
"content": "Add a payment method to continue your subscription",
"recipients": ["{{ customer.email }}"],
"channels": ["email", "in_app"]
}
}
]
}


```


The workflow fires when Stripe sends the` customer.subscription.trial_will_end` webhook. It delivers notifications through multiple channels without custom code.


### Combining Custom Code and Automation


Use both approaches where they fit best:


**Custom webhooks** for core business logic:


- Order fulfillment.
- Inventory management.
- Internal system updates.
- Complex custom workflows.


**Workflow automation** for customer notifications:


- Payment confirmations.
- Subscription status changes.
- Trial reminders.
- Invoice notifications.


This hybrid approach saves development time while keeping your options open for complex needs.


## Production Deployment Checklist


Before deploying Stripe webhooks to production, check these items:


### Security


- Webhook signature verification added.
- HTTPS endpoint with valid SSL certificate.
- Environment variables for webhook secret (not hardcoded).
- IP allowlist set up if needed.
- Rate limiting on webhook endpoint.


### Reliability


- Idempotent event processing (handles duplicates).
- Async processing for long-running operations.
- Error handling and logging.
- Database transactions for key updates.
- Monitoring and alerts for failed webhooks.


### Testing


- All key event handlers tested with Stripe CLI.
- Edge cases covered (duplicate events, bad data).
- Load testing for expected webhook volume.
- Rollback plan if webhook processing fails.


### Monitoring


- Webhook delivery tracking in Stripe Dashboard.
- Application logs for webhook processing.
- Alerts for webhook failures or timeouts.
- Metrics tracking (processing time, error rates).


## Troubleshooting Common Issues


### Webhook Not Receiving Events


**Check endpoint URL** : Make sure the URL in Stripe Dashboard matches your server endpoint exactly.


**Confirm HTTPS** : Production webhooks need HTTPS. For local development, use Stripe CLI forwarding.


**Review event subscriptions** : Make sure your endpoint is set to receive the events you are testing.


**Check firewall rules** : Make sure your server accepts requests from Stripe's IP ranges.


### Signature Verification Failing


**Raw body required** : Parse the webhook body as raw text, not JSON, before verification:


```text
// Correct - raw body
app.post('/webhook', express.raw({type: 'application/json'}), handler);


// Wrong - parsed JSON body
app.post('/webhook', express.json(), handler);


```


**Correct signing secret** : Use the webhook signing secret from the specific endpoint in the Stripe Dashboard. Do not use your API key.


**Header case sensitivity** : Use the exact header name` stripe-signature` , not` Stripe-Signature` .


### Webhooks Timing Out


**Return 2xx right away** : Do not wait for database writes or external API calls before responding:


```text
// Correct
res.json({received: true});
processEventAsync(event);


// Wrong - will timeout
await processEvent(event);
res.json({received: true});


```


**Speed up processing** : Move heavy logic to background jobs. Stripe retries timeouts, which causes duplicate processing.


### Duplicate Events


**Add idempotency** : Track processed event IDs to stop duplicate actions:


```text
const processedEvents = new Set();


if (processedEvents.has(event.id)) {
return res.json({received: true});
}


await handleEvent(event);
processedEvents.add(event.id);


```


**Use database constraints** : Unique constraints on Stripe resource IDs prevent duplicate database records.


## Conclusion


Stripe webhooks let you build real-time, event-driven payment systems. Set up secure webhook endpoints, handle key payment and subscription events, and follow production best practices. This gives you reliable payment infrastructure that grows with your business.


Key takeaways:


- **Always verify webhook signatures** to make sure events come from Stripe.
- **Process events idempotently** to handle Stripe's retry system safely.
- **Focus on payment and subscription events** for core SaaS features.
- **Test well** using Stripe CLI before production deployment.
- **Monitor webhook deliveries** to catch and fix issues fast.


For common notification flows like payment confirmations and subscription updates, try[workflow automation](https://www.magicbell.com/workflows/stripe) to save development time while keeping reliable customer updates.


Whether you build custom webhook handlers, use automation tools, or combine both, Stripe webhooks give you the base for responsive, event-driven payment experiences.
