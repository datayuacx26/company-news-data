---
schema_version: "1.0.0"
document_id: "11e62b93f6efdeaafbdd883b0eeacf5d56aa5115baccaee40f09788ae19615bc"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/kyb-ai-api-enterprise-access/"
published_at: "2026-08-04T14:51:05.879+00:00"
first_seen_at: "2026-08-04T18:58:29.118711+00:00"
fetched_at: "2026-08-04T20:28:10.240209+00:00"
content_hash: "sha256:cd723280014255d501f0ab1f4fe624c97343eea55ee2fe481446cd92e8ef0ee1"
---

# Business Verification for AI API Access: Who Actually Controls This Account?

[Back to blog](https://didit.me/blog/) Blog · August 4, 2026


# Business Verification for AI API Access: Who Actually Controls This Account?


Anthropic named strengthened verification for educational and startup accounts among its distillation mitigations. At organisation tier the question is not whether a person is real — it is whether a company is, and who runs it.


By Didit


·


August 4, 2026 ·


Updated Aug 4, 2026


Among the mitigations Anthropic listed in its February 2026 report on distillation attacks, one is easy to skim past: **"strengthened verification for educational and startup accounts."**


It is worth stopping on, because it identifies a specific soft spot. Educational, research and startup programmes are where the generous terms live — elevated quotas, discounted or granted credits, early access to capabilities. They are also where the verification bar is traditionally lowest, because the whole point of those programmes is to be welcoming, and because the applicant is usually an institution rather than a consumer.


For that tier, "is this a real person" is the wrong question. The right one is: **is this a real organisation, who controls it, and is it what it claims to be?**


## **Key takeaways**


- Organisation-tier access needs **entity** verification, not just individual verification. A verified person can front a shell.
- Business Verification returns **registry data, ultimate beneficial owners, officers, and entity-level screening** — from **$2.00** per company.
- **Closed-loop KYB** : each beneficial owner can be sent through a linked identity verification from the same session. Verifying the company and the humans behind it is one flow, not two systems.
- The signal that matters most is often **ownership opacity** — an entity whose beneficial owners cannot be resolved is a different risk from one whose owners are named and screened.
- **From $2.00** per business, $0.20 per document, $0.20 for entity screening. Public pricing, no minimums.
- Verification is not a rejection mechanism. Most applicants pass; the value is in what the ones that do not have in common.


## **Why entity verification is a distinct control**


An individual identity check tells you a specific human is real and present. That is genuinely useful, and for most access tiers it is enough.


It stops being enough the moment the access is granted to an *organisation* , because the person completing the check and the party actually receiving the access are no longer the same. A real, verifiable, entirely honest person can register a company they do not control, apply to a research programme on its behalf, and pass every individual check you run — while the entity behind the grant is a two-week-old shell whose beneficial owner is somewhere else entirely.


Three questions only entity verification answers:


**Does this company exist, in the registry, in the jurisdiction it claims?** Registry lookup confirms legal existence, status, incorporation date and registered address. A "research lab" incorporated eleven days ago is not disqualifying, but it is information you should have before granting a research quota.


**Who ultimately owns and controls it?** Ultimate beneficial owner extraction resolves the ownership chain — the natural persons behind the entity, not the holding company in front of them. This is the question that shells are built to obscure.


**Is the entity or anyone controlling it on a sanctions or watchlist?** Didit screens both levels: **Company AML** puts the entity against sanctions, adverse media and regulatory enforcement lists, and **Person AML** screens each identified key person — beneficial owner, shareholder, director, representative — individually, using the same two-score match-and-risk model as user verification.


## **What Business Verification returns**


Business Verification runs through the same unified` /v3/` API as every other Didit check — a session with a KYB workflow.


```text
curl -X POST 'https://verification.didit.me/v3/session/' \
-H 'x-api-key: YOUR_API_KEY' \
-H 'Content-Type: application/json' \
-d '{
"workflow_id": "YOUR_KYB_WORKFLOW_ID",
"vendor_data": "org_2291",
"callback": "https://yourplatform.example/kyb/complete"
}'
```


The workflow assembles the features you need:


Feature What it returns Price


**Registry lookup** Legal existence, status, incorporation data, registered address included from $2.00


**UBO extraction** The ultimate beneficial owners behind the ownership chain included


**Officer data** Directors and officers of record included


**Entity AML screening** The company against sanctions and watchlists $0.20


**Document collection & OCR** Incorporation documents, structure charts $0.20 per document


**Linked identity verification** Each beneficial owner verified as an individual billed at User Verification rates


Business entity statuses are` ACTIVE` ,` FLAGGED` and` BLOCKED` , and the webhooks are` business.status.updated` and` business.data.updated` — so the result lands in your system the same way every other Didit decision does.


## **Closed-loop: the company and its people in one flow**


The part that is genuinely different: from a single KYB session, each identified beneficial owner can be sent through their **own linked identity verification** .


That closes a loop most stacks leave open. The usual arrangement is a KYB vendor that returns a list of names for beneficial owners, and then — separately, in a different system, often manually — someone attempts to verify those people. In practice that second step is where the process quietly dies. Names get collected and never checked.


With linked verification, the ownership chain resolves to natural persons and those persons complete a real identity check with liveness, in the same session, with the results attached to the same business record.


For an AI platform granting research or enterprise access, that produces something specific and useful: not "an organisation called X was verified," but "organisation X exists, is controlled by these named natural persons, and each of them has completed a liveness-checked identity verification." That is an attributable counterparty.


And because a linked owner verification runs a liveness check like any other, those beneficial owners enter the same face index as everyone else — provided the sessions were run with` save_api_request=true` . Once they are in it, they participate in the same linking primitives: a 1:N search will show you that the beneficial owner of your newest research applicant is also the person behind eleven accounts you banned last month.


## **What to actually look at**


Verification produces data. Deciding requires knowing which parts matter.


**Ownership opacity.** The strongest signal is not a bad answer, it is no answer. An entity whose beneficial ownership cannot be resolved is a materially different proposition from one whose owners are named, screened and verified.


**Incorporation recency against the size of the ask.** A company incorporated last month requesting the largest research quota you offer is a combination worth a human's attention. Either factor alone is unremarkable.


**Jurisdictional mismatch.** The entity is registered in one jurisdiction, its beneficial owners resolve to another, and the access requests arrive from a third. Legitimate distributed companies exist and this is not proof of anything — but it is a reason to look, particularly where you have geographic access restrictions.


**Officer and owner overlap across applicants.** The same natural persons appearing behind several entities all applying for research access is precisely the coordination pattern that per-application review cannot see, and it is the entity-level analogue of a hydra network.


**Screening hits on the entity or its controllers.** Directly relevant where you have jurisdictional or sanctions-driven access restrictions.


None of which prevents anything on its own. **Verifying a company does not prevent model extraction and does not detect it** — a fully legitimate, fully verified entity can still violate your terms. What entity verification changes is that the counterparty is attributable and its controllers are named and screened, so an enforcement decision has somewhere to land. Model-level controls and traffic detection remain separate layers.


## **Where it belongs in the access architecture**


Business verification is the top tier of a risk-tiered design, and it should apply to the smallest population:


Tier Population share Check Cost


Free large majority none, or email $0 – $0.03


Paid self-serve moderate IP + device $0.03


High quota / credit small full identity bundle $0.33


**Organisation / research / enterprise** **smallest** **business verification** **from $2.00**


At $2.00 per company against an enterprise agreement, a research grant, or an elevated organisational quota, the cost is not a meaningful factor in the decision. Pricing is public, pay-per-success, and carries no minimum.


## **Use cases**


**AI research and educational programmes** verifying the institution and the named researchers before granting elevated access — the tier Anthropic specifically flagged.


**Enterprise API tiers** confirming the contracting entity exists and resolving who controls it before provisioning organisational quota.


**Cloud AI marketplaces and model hubs** verifying publishing organisations before listing.


**Reseller and aggregator programmes** where an intermediary requests bulk access and the beneficial-ownership question is the entire risk.


**Startup credit programmes** where entity age, ownership and officer overlap across applications reveal coordinated farming of the same programme.


## **Frequently asked questions**


**How long does business verification take?**


Registry data returns from the source registries; where document collection or linked owner verification is involved, completion depends on the applicant. The individual verifications inside the flow return in under two seconds each.


**What if beneficial owners cannot be resolved?**


That is itself the finding, and often the most important one. Opaque ownership is a legitimate reason to require documents or route to manual review rather than a reason to guess.


**Do beneficial owners have to complete identity verification?**


That is your policy. Linked verification is available from the same session — you decide whether it is required, optional, or reserved for higher-value grants.


**What does "from $2.00" cover?**


Registry lookup, UBO extraction and officer data. Entity AML screening is $0.20, document collection and OCR is $0.20 per document, and linked identity checks for beneficial owners are billed at standard User Verification rates.


**Is this only for enterprise tiers?**


It fits anywhere access is granted to an organisation rather than a person — enterprise, research, educational, reseller and startup-credit programmes all qualify.


**Does verifying a company prevent it from distilling our model?**


No. It makes the counterparty attributable, resolves who controls it, and screens the entity and its owners. Whether the access is then used appropriately is a question for your traffic layer and your model-level controls. Entity verification changes who you are dealing with from unknown to known — which is a real change, and not the same as prevention.


## **Ready to get started?**


Verify the organisation before you provision the quota.


- **Read the docs** —[Business Verification overview](https://docs.didit.me/business-verification/overview) and the[Sessions API](https://docs.didit.me/api-reference/overview) .
- **See the product** —[Business Verification](https://didit.me/products/business-verification) .
- **Check the pricing** —[from $2.00 per business](https://didit.me/pricing) , public, no minimums.
- **Start free** —[business.didit.me](https://business.didit.me/) . Build the workflow and run your first checks.


Keep reading


## Related articles


- [The Hydra Account Problem: Why Distillation Defense Starts With Identity Resolution](https://didit.me/blog/llm-distillation-defense-identity/)
- [Verified API Access for AI Model Providers: A Risk-Tiered Architecture](https://didit.me/blog/verified-api-access-ai-model-providers/)
- [Face Search 1:N: Finding Every Account One Person Controls](https://didit.me/blog/face-search-duplicate-account-detection/)
- [Biometric Step-Up for AI API Access: Binding Privilege to a Person](https://didit.me/blog/biometric-authentication-ai-api-access/)
- [Hydra Account Networks: How 20,000 Accounts Become One Actor](https://didit.me/blog/hydra-account-networks-ai-api-abuse/)
- [Blocklist Propagation: Making One Confirmed Abuse Case Kill the Whole Network](https://didit.me/blog/ai-api-abuse-blocklist-propagation/)
