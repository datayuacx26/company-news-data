---
schema_version: "1.0.0"
document_id: "75ab83222e160ec0143a048fd38afbccd7336109802dc35b52556d0916b661d3"
company_key: "yc-toku"
company: "Toku"
source_id: "yc-toku-news-import-57695167b900"
canonical_url: "https://www.toku.com/resources/how-toku-brings-stablecoin-payroll-to-rippling"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-26T02:46:59.685075+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:ab9770dfe54dac993a6cfc4589a1b9d7348ffa3b1b4e2791a4340a443dd7ea60"
---

# How Toku Brings Stablecoin Payroll to Rippling

You run payroll on Rippling. You want to pay part of your team in stablecoins. You do not have to replace your payroll system to do it. Toku adds a compliant stablecoin payout layer on top of Rippling through the Rippling API, and the[Rippling integration](https://docs.toku.com/integrations/rippling) keeps Rippling as your payroll system of record while Toku handles the digital-asset side.


**TL;DR**


- Toku adds compliant stablecoin payroll on top of Rippling through the Rippling API. Rippling stays your payroll system of record.
- Employees choose what share of their pay to receive in stablecoins. Each pay period, Toku applies a single deduction as a pay input and delivers the stablecoin portion on-chain.
- Gross pay, taxes, and your existing pay items do not change. You run payroll in Rippling exactly as you do today.
- The connection is a scoped, admin-approved OAuth install: read-only access to your roster and payroll, one write-back (the pay input), revocable anytime, with tokens encrypted at rest.
- Setup is onboarding-assisted, a short session with the Toku team, rather than a self-serve toggle.


Stablecoin payroll means paying part of an employee's net pay in dollar-pegged stablecoins such as USDC or USDT, each redeemable one-for-one for a US dollar, used as the settlement rail. Compensation stays denominated in fiat. The stablecoin is how the money moves, not the currency the salary is set in. That distinction is what lets stablecoin payroll sit on top of Rippling without changing anything about how Rippling calculates pay.


## Why run stablecoin payroll on top of Rippling?


Rippling is strong at what it does: running payroll, HR, and IT in one place. What it does not do is move money to a worker on stablecoin rails. For a team with people outside your home country, that gap is where cost and delay collect.


Cross-border payroll leaks money on every run. FX markup, wire fees, correspondent-bank hops, and the multi-day float where money sits in transit are rarely shown as a line item you approve. They are skimmed off the exchange rate instead. Stablecoin rails cut most of that: stablecoin-to-stablecoin payments carry no FX markup, and settlement is fast and runs outside banking hours. Everyrealm moved to compliant payouts across 20+ countries and reported 87% lower costs than traditional banking.


The reason to do this on top of Rippling, rather than in a separate tool, is control. When the stablecoin payout rides your existing payroll run, it inherits the approvals, the withholding, and the reconciliation you already trust. A parallel payout tool outside Rippling is a second system your controls never see. Toku is built to avoid exactly that.


## How the Rippling and Toku integration works


The integration runs on the Rippling API through an OAuth connection your admin approves at install time. Once connected, it works in four steps.


1. **Connect.** Authorize Toku's Rippling connection for your company with a standard OAuth install. There are no credentials to copy around.
2. **Sync.** Toku syncs your worker roster and payroll runs from Rippling automatically, on a read-only basis.
3. **Elect.** Employees choose what portion of their pay they want to receive in stablecoins.
4. **Deduct and deliver.** Each pay period, a single Toku deduction is reflected as a pay input on the payroll run, and Toku delivers the corresponding stablecoin payment on-chain.


Companies can fund payroll in fiat or stablecoins, and employees can receive stablecoins or their local currency. Toku handles the conversion in either direction. For an in-depth look see the[Rippling integration page](https://docs.toku.com/integrations/rippling) .


## What changes, and what stays the same?


Your payroll process does not change. The stablecoin layer sits on top of it.


Aspect With the Toku and Rippling integration


Payroll system of record Stays Rippling


How you run payroll Exactly as you do today, in Rippling


Worker and payroll data Syncs to Toku on a read-only basis


What Toku writes back One thing only: a single Toku deduction as a pay input


Gross pay, taxes, existing pay items Unchanged


Stablecoin delivery On-chain, handled by Toku


Reporting and reconciliation Provided by Toku


Access Scoped, admin-approved OAuth, revocable anytime


Stablecoin payments appear in Rippling as one dedicated line item on the worker's pay. Toku calculates each worker's amount every pay period based on their election and applies it through Rippling's payroll APIs, with no manual data entry.


## Who should turn this on?


Stablecoin payroll on Rippling is not an all-or-nothing switch. It is worth it for some parts of a team and adds little for others, so the useful question is which segment qualifies.


**International contractors are the strongest case.** Cross-border contractor payments carry the most FX and fee leakage, and they need no local entity, so the economics are clearest here.


**Employees in high-FX or weak-banking markets are a strong, opt-in case.** Where local banking is slow or expensive, or a local currency loses value between pay date and spending, dollar-denominated stablecoin pay is a real benefit. It should always be the worker's choice, not a default.


**Crypto-native teams are a natural fit.** Workers at these companies are often already asking to be paid in stablecoins, and Toku keeps token-grant administration in the same stack.


**Domestic employees already paid over local rails gain little.** Where ACH or SEPA is already fast and cheap, there is not much to save. The honest answer is to leave that group on the rail it already uses.


## How do you set it up?


Setup is a short, guided process rather than a self-serve toggle. You will need admin access to your Rippling account, admin access to your Toku account, and a short implementation session with the Toku team.


1. **Authorize the connection.** Your Rippling administrator installs and approves Toku's connection, reviewing the read scopes for workers and payroll plus the single write scope Toku uses for pay inputs.
2. **Confirm the Toku deduction.** Toku sets up the dedicated pay line item for participating workers as part of onboarding.
3. **Connect from Toku.** In the Toku dashboard, go to Settings, then Payroll Integrations, then Add New Integration, then Rippling, and complete the connection.
4. **Verify the sync.** Toku validates the connection, previews your worker roster, and begins syncing payroll runs.


The deduction is configured once during implementation, so there is no ongoing setup after go-live. Most companies start with a small pilot group, usually willing international contractors, and expand from there while keeping the traditional payment option in place for everyone else.


## What data does Toku access, and is it secure?


Worker and payroll data syncs from Rippling to Toku on a read-only basis. The single Toku pay input is the only information Toku writes back to Rippling. Access uses OAuth with admin-approved, per-company scopes, so your admin sees and approves exactly what the connection can read and write at install time.


You can revoke access at any time from Rippling, which stops the sync and the write-back. All tokens are encrypted at rest. Because Rippling stays the system of record, revoking the connection never puts your payroll data at risk: the source of truth was always Rippling.


## What does each pay period look like?


Once connected, the integration runs on autopilot. Toku syncs upcoming payroll runs and worker changes from Rippling, calculates each worker's stablecoin amount based on their election, and applies the Toku deduction to the run. You run payroll in Rippling exactly as you do today. Toku delivers the stablecoin payments on-chain and provides full reporting and reconciliation, so every payout ties back to the payroll run.


## Key benefits


**For your team.** Employees choose how they are paid: traditional deposit, stablecoins, or a split, and they can receive stablecoins or their local currency. Workers who want dollar-denominated pay get it without your finance team building anything new.


**For finance and operations.** The stablecoin payout rides the payroll run you already approve in Rippling, so there is no parallel payment process to manage. Reporting and reconciliation come back from Toku, and the connection needs no manual data entry once it is live.


**For compliance.** Gross pay, taxes, and existing pay items are unchanged, and withholding is calculated and applied the same as traditional pay, per employee jurisdiction. Rippling stays the system of record, the connection is read-only except for the single pay input, and access is scoped, admin-approved, and revocable.


## Why run stablecoin payroll with Toku?


Toku is a global payroll and Employer of Record provider built on stablecoin rails, combining payroll, EOR, contractor management, and token-grant administration in one stack across 100+ countries, and it now processes more than $1 billion in annual token payroll volume. Stablecoin payroll is included in every plan with no crypto add-on fee, so adding it to your Rippling stack does not mean a separate crypto product or a second payroll system. EOR starts at $599 per employee per month and contractor management at $19 per contractor per month.


Because the rails are native rather than bolted on, the controls above are the product, not a workaround. For the wider picture, see[what stablecoin payroll is](https://www.toku.com/resources/what-is-stablecoin-payroll) , and for other systems, the[guide to stablecoin payroll integrations with ADP, Workday, and Gusto](https://www.toku.com/resources/guide-to-stablecoin-payroll-integrations-adp-workday-gusto) .


## Frequently asked questions


### Do I have to leave Rippling to pay my team in stablecoins?


No. Rippling stays your payroll system of record. Toku connects through the Rippling API and adds a stablecoin payout layer on top, so you run payroll in Rippling exactly as you do today. The stablecoin portion is applied as a single deduction line item each pay period, so there is nothing new to run and nothing removed from your current setup.


### Does the integration change gross pay or taxes?


No. Stablecoin payments appear as one dedicated deduction line item applied as a pay input. Gross pay, taxes, and your existing pay items are unchanged. Toku handles the digital-asset side while Rippling handles payroll as before. The deduction is configured once during implementation, and from then on it is calculated and applied automatically each pay period.


### Is the Rippling integration self-serve, or does it need setup?


It is onboarding-assisted. Your admin approves a scoped OAuth install, and a short implementation session with the Toku team configures the deduction. It is not a self-serve toggle. There is no code to write on your side: your admin approves the OAuth scopes and Toku's team handles the configuration once, after which it runs each pay period.


### How long does setup take?


Setup is fast because there is no code to build. The work is an admin approving the OAuth connection, a short implementation session to configure the Toku deduction, and a verification of the sync. Many companies start with a small pilot group of willing international contractors, then expand once the first runs look right, keeping traditional payment available throughout.


### What data does Toku access in my Rippling account?


Worker and payroll data syncs to Toku on a read-only basis. The only thing Toku writes back is the single Toku deduction as a pay input. Access uses admin-approved OAuth scopes, is revocable at any time from Rippling, and all tokens are encrypted at rest. Rippling remains the system of record throughout.


### Which employees receive stablecoins?


Only the ones who elect to. Each employee chooses what portion of their pay to receive in stablecoins, and Toku calculates and delivers that portion on-chain each pay period. Employees can receive stablecoins or their local currency instead, so participation is always opt-in rather than a default applied to the whole team.


### Which stablecoins can workers be paid in?


Toku pays in dollar-pegged stablecoins such as USDC and USDT, each redeemable one-for-one for a US dollar. Companies can fund payroll in fiat or stablecoins, and workers can receive stablecoins or their local currency, with Toku handling the conversion either way, so neither the company nor the worker has to hold crypto to take part.


## Add stablecoin payroll to your Rippling stack


If your team runs on Rippling and you want to offer stablecoin payroll without switching systems, the quickest way to scope it is a short call.[Book a demo](https://www.toku.com/contact) to walk through the[Rippling integration](https://docs.toku.com/integrations/rippling) with the Toku team.


*Toku provides compliance infrastructure and is not a law firm. This content is for informational purposes only and does not constitute legal or tax advice. Consult your legal counsel for jurisdiction-specific guidance.*
