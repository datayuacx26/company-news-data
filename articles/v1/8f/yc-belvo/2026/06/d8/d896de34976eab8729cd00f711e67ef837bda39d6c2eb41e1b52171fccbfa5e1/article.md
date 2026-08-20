---
schema_version: "1.0.0"
document_id: "d896de34976eab8729cd00f711e67ef837bda39d6c2eb41e1b52171fccbfa5e1"
company_key: "yc-belvo"
company: "Belvo"
source_id: "yc-belvo-rss-3d04ef8adabc"
canonical_url: "https://belvo.com/blog/smart-collections-how-works/"
published_at: "2026-06-10T13:37:45+00:00"
first_seen_at: "2026-07-20T23:23:37.443894+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:27a3ff914923ea8dcd7741c63536200fc3adc1c277e8412662ed352f63298ecd"
---

# Smart Collections: collect when the money comes in

Meet Smart Collections, Belvo's new product built for loan repayment. The solution collects automatically at the exact moment the customer has balance to pay, with no action required from the borrower.


Most lenders run collections on a fixed schedule: the installment is due on the 5th, so a reminder goes out on the 5th, another follows on the 15th, one more on the 30th, and if nothing comes back, the case goes to a collections agency. The cadence is set in advance and doesn't respond much to what happens in between, which is a problem, because the one thing that actually matters (whether your customer has money in their account today) is precisely what that schedule can't see.


That's exactly the gap Smart Collections closes. It's not a reminder, a chatbot, or a better-calibrated collections sequence. The product answers one simple question: when, exactly, should the charge happen? The answer is: when there's money in the account. The rest of this post explains how we got there, how the product works in practice, and where it fits in your collections cycle.


## The problem isn't lack of intent, it's timing


Every traditional channel has one thing in common: it needs your customer to do something, whether that's scanning a code, opening an app, tapping a link, or picking up a call. That friction is the number one reason collections fail, because most of the time, people genuinely want to pay. What's missing isn't intention, it's the combination of having the right balance at the right moment, the right platform to execute, and spare attention on an already full day.


There's a second problem hidden in there too: the collections model is binary, attempting to collect the full installment or nothing. If your customer owes R$500 and has R$180 in their account, the traditional flow walks away empty-handed and queues another attempt for next month, even though there was real money on the table.


## Collect when there's balance, not on a fixed date


Two pieces of infrastructure moved at roughly the same time, and together they changed what's possible.


The first is that **Open Finance connectivity** makes your customer's account observable in real time. Not in batch, not once a day, but continuously, so you know when a paycheck lands, when a Pix comes in from somewhere else, and when the balance crosses a threshold that matters to you. The second is **Intelligent Pix** , which gives you a way to actually move the money based on that signal, executing a recurring debit between accounts under the same CPF, within the limits your customer agreed to upfront, without them needing to confirm anything in the moment.


When you combine solid balance monitoring with a payment rail that executes on its own, the action shifts from the borrower to the infrastructure. The lender stops chasing, the engine waits patiently in the account, and the charge happens the same second the conditions for it are met. That's the whole idea of Smart Collections in one line: **observation plus action, at the moment of payment.**


It's worth being specific about what that moment is, because the product was designed for a well-defined window. Smart Collections was built for normal-cycle collections: the routine installment, the card bill, the purchase in installments, where your customer isn't yet delinquent and still wants to pay, but needs the right rail to fire at the moment the money is actually there.


## How Smart Collections works


We kept the integration deliberately simple, so most of the intelligence lives inside the engine and very little sits on the client side. End to end, it's four steps.


**1. You define the collections policy.** You send Belvo a collections instruction for each debt or installment, identifying the borrower and describing, through a policy, what to collect, when, and for how long to keep trying. The policy supports time windows, different target amounts across days, and as many rules as you need to reflect your credit policy.


**2. Belvo monitors balance in real time.** Through Open Finance, our engine tracks the customer's connected account continuously, learning the borrower's deposit patterns to do this efficiently. Your customer doesn't need to do anything.


**3. A webhook fires when the policy is met.** When the balance hits the conditions of an active rule, Belvo notifies your system in real time, along with other relevant events in the collections cycle.


**4. The Pix is initiated.**[Intelligent Pix](https://belvo.com/blog/intelligent-pix-how-automatic-transfers-work/) executes the debit within the limits your customer authorized upfront. The money moves, the installment is paid, and the borrower wasn't asked to do anything.


## Partial collections to optimize cash flow


This is the part most teams underestimate the first time they look at Smart Collections, and it's worth spending a moment on it, because it changes the math of recovery in a way that isn't obvious just from the four-step flow.


The traditional model was built around full installments. If the installment is R$100 and the borrower has R$30 in their account, the system either does nothing, or attempts the full R$100 debit and fails, leaving the R$30 on the table because the model either didn't see it, or wasn't authorized to accept less than the full amount. Multiply that across a portfolio over several months, and you're looking at a meaningful slice of recoverable money that simply never gets recovered.


Smart Collections treats partial collection as a legitimate, intentional form of recovery. If your policy allows it and your customer has R$30 of a R$100 debt, the engine collects the R$30 and keeps monitoring the rest, so the next time R$70 comes in, it can collect the remainder and close the debt in two moves, instead of letting it age. Across the portfolio, that means installments that would have aged into delinquency end up resolved while still inside the normal collections window, where the unit economics still work.


## Compliance e UX, por design


The mechanics described here only work because a few things are true at the same time on the regulatory and UX side, and they're worth saying clearly, because they're usually the first place risk and compliance teams want to land.


The product runs on the Open Finance consent framework, which gives your customer full control over what they authorized and for how long, with the ability to revoke at any time. All of that is handled by Belvo as part of the product, not as a layer you need to build. The money itself only moves between accounts under the same CPF and respects the limits your customer agreed to upfront: nothing leaves the borrower's own sphere, and no third party enters the middle.


On the borrower's side, the entire experience is a single onboarding step at the point of credit origination, with a redirect to their bank, and nothing after that. The product is most visible when it isn't visible at all, because the installment just gets paid.


## What this means for your roadmap


If you run a credit fintech in Brazil, whether in consumer credit, SME, BNPL, or revolving credit, and you already spend on boletos, WhatsApp, calls, and collections agencies, Smart Collections lets you stop asking *how do we send a better reminder?* and start asking a more useful question instead: **how much of our overdue portfolio is just a timing problem, one that can be solved the minute the paycheck lands?** In our experience, it's more than most teams expect upfront.


A few things worth knowing as you think through the fit:


**You can go live in weeks, not quarters** , because Smart Collections plugs into your existing credit flow without requiring you to rewrite anything in your codebase.


**The product is modular by design.** If you already have your own payment rails, you can use just the balance monitoring layer in your existing infrastructure, or use Smart Collections in full with Intelligent Pix built in.


**Built on seven years of Open Finance and over 19 million consents.** That volume became infrastructure you don't need to build: we've already learned how each bank behaves, what to do when a connection drops, and how to keep data quality stable day to day. The platform is ISO 27001 certified and compliant with BCB and LGPD.


See the[Smart Collections page](https://belvo.com/smart-collections) or talk to our team about your collections stack.
