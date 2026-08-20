---
schema_version: "1.0.0"
document_id: "07c25a09763e05b7ee71203c91db0c98c013ef2891cc11a9e1853e644d4c94c2"
company_key: "yc-belvo"
company: "Belvo"
source_id: "yc-belvo-rss-3d04ef8adabc"
canonical_url: "https://belvo.com/blog/how-automatic-transfers-work/"
published_at: "2026-06-10T13:30:47+00:00"
first_seen_at: "2026-07-20T23:23:37.443894+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:53483c08407e114b2563dd375de76002b309590df6e255a8b6ef6705f54a8a5a"
---

# Smart Transfers: how automatic transfers work

Learn what Intelligent Pix is, how single-consent works via Open Finance, and which use cases it unlocks for recurring payments.


What if you could receive payment via Pix without the payer having to open an app, copy a code, and confirm every single charge?


That's exactly what Intelligent Pix does. The product runs automatic transfers between accounts with the same owner via Open Finance: the user authorizes once, and from that point on, transfers happen for the exact amount at the right time, with or without any action from the customer.


In this post, we explain what Intelligent Pix is, how single-consent works, and which use cases it unlocks for businesses that collect recurring payments.


## **What is Intelligent Pix?**


Intelligent Pix is a Pix feature introduced by the Banco Central through the Open Finance rails. Also known as Smart Transfers, at its core, it's a system that lets accounts belonging to the same individual or business move funds between different financial institutions automatically and on a schedule.


The difference from a standard Pix transfer is automation. Traditionally, moving money between your own accounts is a manual process: open the app, enter the amount, confirm. With Intelligent Pix, that flow becomes a rule. The user gives consent through Open Finance and sets the parameters. From there, transfers execute automatically, with no friction at each transaction.


For businesses that collect recurring payments, that means getting paid by automatic transfer, not by reminder.


## **How it works, from authorization to settlement**


The full flow has four steps:


1. **Single consent.** The user authorizes transfers once, directly in their bank's app, within Open Finance rules.
2. **Amount and trigger defined.** You set a recurring schedule or trigger the transfer based on balance data using Smart Collections.
3. **Automatic execution.** The transfer happens between same-owner accounts, with or without any action from the user at that moment.
4. **Settlement via Pix.** The exact amount lands in the destination account in seconds, with real-time confirmation.


Once the customer gives consent, there's no limit on the number of transfers, subject to the Banco Central's Pix limits and any limits the user sets with their bank.


## **Use cases: what Intelligent Pix unlocks**


Single consent opens the door to billing models that previously relied on cards, boletos, or the payer's memory.


**Digital wallets.** Top up the user's wallet the moment there's a need: on a schedule or triggered when the balance runs low. More balance in the wallet means more transactions on your platform.


**Credit installments.** Debit each installment directly from the borrower's main account, on the due date and for the exact amount. Think of a R$318.50 installment due on the 10th of every month: instead of issuing a boleto and hoping for payment, the amount transfers automatically. Paired with Smart Collections, the debit happens when there's balance in the account, and if the balance is partial, Smart Collections executes a charge for the available amount and picks up the remainder in a subsequent attempt.


**Investments.** Automate recurring contributions from the customer's bank account to their investment account, on the day and amount they choose. Contributions that don't depend on a reminder translate to more assets under management.


## **Intelligent Pix vs. boleto, card, and manual Pix**


Every traditional recurring billing method has a known failure point:


Manual Pix depends on the payer taking action for each charge. Forgetfulness becomes delinquency.


Boleto carries a per-issuance cost, slow settlement, and low conversion for recurring charges.


Credit card charges MDR on every transaction, expires, and fails without warning.


With Intelligent Pix, the transfer executes automatically after a single consent. The exact amount is debited at the right moment, settlement happens in seconds, and there's no card MDR or per-boleto cost.


## **Intelligent Pix and Pix Automático: what's the difference?**


Both work with single consent and instant settlement, but they serve different needs, and the distinction matters for businesses that collect recurring payments.


Pix Automático was designed for payments between different account holders: the customer authorizes a company to debit their account on a recurring basis. It's the right model for subscriptions, monthly plans, and utility bills. The limitation, though, is structural: in case of insufficient balance, Pix Automático allows up to three retry attempts over seven calendar days, always for the same amount. If the account still has no balance after that window, the charge fails.


Intelligent Pix operates on a different logic. Transfers happen between accounts with the same owner, and Smart Collections decides the right moment to debit: when funds are available. There's no retry limit. The charge keeps being attempted until the amount is in the account, within the parameters the user set at consent. And if the balance is partial, Smart Collections executes a charge for the available amount and collects the remainder later, something Pix Automático doesn't do.


Pix Automático Intelligent Pix


**Account ownership** Different account holders Same account holder


**Primary use case** Subscriptions, monthly plans, utility bills Credit installments, digital wallets, investments


**Retries** Up to 3, within 7 calendar days Unlimited, balance-driven


**Partial collections** No Yes, via Smart Collections


**Trigger** Due date Date or available balance


**Integration** Via payer's bank Via licensed ITP (Belvo)


---


**Infrastructure regulated by the Banco Central**


Like Biometric Pix, Intelligent Pix is only possible through a licensed Payment Transaction Initiator (ITP). Belvo is an ITP licensed by the Banco Central do Brasil, and Intelligent Pix runs on the same infrastructure already processing Open Finance payments for payment companies, digital wallets, and credit fintechs.


That means user consent is registered and auditable under Open Finance rules, and you initiate transfers and access balance data in the same integration, through a single API.


Learn more about our infrastructure solution for companies with an ITP license here.


## **And for the end user?**


The automation also solves problems for payers:


**Bills paid on time, without the effort.** Installments, investments and contributions happen on their own, on the right date.


**No friction.** One consent replaces the back-and-forth between different banking apps.


**Full control.** The user sets amounts, dates, and triggers, and can revoke consent whenever they want.


**Money instantly.** Settlement via Pix is instant, with real-time confirmation.


## **Collect with Intelligent Pix**


Talk to a specialist to find out how much of your recurring revenue can be settled automatically via Pix, or explore the documentation to see how to integrate scheduled and recurring transfers via API.
