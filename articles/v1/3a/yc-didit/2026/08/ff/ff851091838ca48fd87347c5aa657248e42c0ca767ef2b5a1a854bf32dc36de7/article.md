---
schema_version: "1.0.0"
document_id: "ff851091838ca48fd87347c5aa657248e42c0ca767ef2b5a1a854bf32dc36de7"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/hydra-account-networks-ai-api-abuse/"
published_at: "2026-08-04T14:49:32.399+00:00"
first_seen_at: "2026-08-04T18:58:29.118711+00:00"
fetched_at: "2026-08-04T20:28:10.240209+00:00"
content_hash: "sha256:ed674ef05dabb2ea61d75df4e819ed380a635fac62b3ad5cf5b5b223b8a7bef3"
---

# Hydra Account Networks: How 20,000 Accounts Become One Actor

[Back to blog](https://didit.me/blog/) Blog · August 4, 2026


# Hydra Account Networks: How 20,000 Accounts Become One Actor


Cut off one account and two more appear. Hydra networks beat per-account review by design. Here is how cross-account linking — face, device, IP, email, phone — collapses thousands of accounts into a single resolvable actor.


By Didit


·


August 4, 2026 ·


Updated Aug 4, 2026


The most useful number in Anthropic's February 2026 report on distillation attacks is not the 16 million exchanges or the roughly 24,000 fraudulent accounts. It is this: "a single proxy network managed more than 20,000 fraudulent accounts simultaneously."


Simultaneously. Twenty thousand accounts, alive at the same time, under one operator.


That is a hydra network, and it is a specific adversary design — not sloppiness, not opportunism, but an architecture built explicitly to survive the defense it expects to meet. Understanding why it works is the prerequisite to breaking it, because the failure it exploits is not a missing rule or a badly tuned threshold. It is a category error in what the defense is measuring.


## **Key takeaways**


- A hydra network **spreads one campaign across thousands of accounts** so that no individual account exceeds any per-account threshold.
- Lowering thresholds does not help. The attacker simply **adds accounts** — the cheapest input in the system.
- The signals that expose these networks are **relational** : shared devices, shared networks, shared payment methods, shared timing, shared biometrics. A per-account verdict cannot produce them.
- **Face Search 1:N** links two accounts to one human being. **Device and IP analysis** links accounts to shared infrastructure. Together they collapse the account graph into an actor graph.
- The most valuable single code in this context is` DEVICE_RECOVERED_HIGH_CONFIDENCE` — a device that has been seen before, returning after a reset or reinstall. That is regeneration, caught at the door.
- Linking is not accusation. Duplicate signals are **informational by default** — you decide the policy.


## **What a hydra network actually is**


Strip away the specifics and a hydra network has four properties.


**Horizontal distribution.** The workload is divided so that each account's behavior sits comfortably inside normal. Anthropic described patterns that "suggested 'load balancing'" — which is exactly the right word. This is an infrastructure problem being solved with infrastructure thinking.


**Cheap regeneration.** New accounts can be created faster than old ones are removed. Every removal is a rounding error against the supply.


**Shared substrate.** Under the surface, the accounts run on a finite pool of real resources — devices, IP ranges, phone numbers, payment instruments, and in many cases a small number of actual human beings completing whatever checks exist.


**Behavioral homogeneity.** Because one operator drives them all, the accounts converge. Anthropic observed "identical patterns, shared payment methods, and coordinated timing," and prompt variations arriving "tens of thousands of times across hundreds of coordinated accounts."


The third property is the vulnerability. Distribution is cheap at the account layer and expensive at the physical layer. You can mint 20,000 email addresses for nothing. You cannot mint 20,000 human faces, 20,000 unmodified handsets, or 20,000 residential IP addresses in unrelated ranges without cost that scales.


**Every hydra network is narrower at the bottom than it is at the top.** The whole game is measuring at the bottom.


## **Why thresholds are the wrong instrument**


Consider a platform that flags any account exceeding 50,000 requests in a month with a concentrated query distribution. Reasonable rule. Against a single abusive account it works.


Against an operator with 20,000 accounts, it means each account can make 2,500 requests without ever being looked at — 50 million requests, entirely under the radar. Drop the threshold to 5,000 and the operator moves to 250 requests per account and adds accounts if needed. Every reduction in the threshold costs you false positives on real developers and costs the attacker almost nothing.


This is a losing exchange, and it loses for a structural reason: **the threshold is denominated in the unit the attacker controls.**


The attacker chooses how many accounts to use. They do not choose how many faces they have, how many physical devices they own, or how many independent network paths they can reach. Move the measurement to a unit the attacker does not control and the economics invert.


## **The linking primitives**


Four signal families do the collapsing. Each answers a different version of "have I seen this before?"


### **Biometric — is this the same person?**


**Face Search 1:N** searches a face against every approved verification your application has already performed. It is free with Didit identity verification and returns in under two seconds.


```text
curl -X POST 'https://verification.didit.me/v3/face-search/' \
-H 'x-api-key: YOUR_API_KEY' \
-F 'user_image=@./selfie.jpg' \
-F 'search_type=most_similar' \
-F 'save_api_request=true' \
-F 'vendor_data=account-8842'
```


The response wraps a singular` face_search` object —` { request_id, face_search: { ... } }` — carrying` total_matches` and a` matches` array, where each match includes` session_id` ,` similarity_percentage` ,` vendor_data` ,` verification_date` and` is_blocklisted` . If the same operator verified 40 accounts with the same face, one call surfaces all 40 — with your own` vendor_data` on each, so you can map them straight back to account IDs.


This runs automatically during liveness inside a verification session, so in the common case you get the signal without making a separate call at all.


### **Device — is this the same machine?**


Device and IP analysis costs **$0.03** and is included in the $0.33 verification bundle. The codes that matter for account multiplication:


Warning What it tells you


` DUPLICATED_DEVICE_FINGERPRINT` The same device is behind multiple verifications


` DEVICE_RECOVERED_HIGH_CONFIDENCE` A device seen before, returning after a reset or reinstall


` DUPLICATED_IP_ADDRESS` The same address is behind multiple verifications


` AUTOMATION_FRAMEWORK_DETECTED` The client is scripted rather than human-driven


` DEVICE_EMULATOR_DETECTED` An emulator, not a real handset


` DEVICE_ROOTED_OR_JAILBROKEN` A compromised operating system


` DEVICE_RUNTIME_HOOKING_DETECTED` Runtime instrumentation on the client


` DEVICE_APP_TAMPERED` A modified application binary


` PRIVATE_NETWORK_DETECTED` A private or anonymising network path


` COUNTRY_FROM_DOCUMENT_DOES_NOT_MATCH_COUNTRY_FROM_IP` Document and network geography disagree


` DEVICE_RECOVERED_HIGH_CONFIDENCE` deserves particular attention. Didit distinguishes a **duplicated device** from a **recovered device** — a device that reappears after being wiped, reset, or having the app reinstalled. Wiping the device is the standard move for an operator regenerating accounts after a ban. That code is the hydra's regeneration step, made visible.


### **Contact — is this the same reachable identity?**


Email and phone verification ($0.03 for email; phone via SMS, WhatsApp, Telegram, RCS or voice) test whether a contact point is real and reachable rather than merely well-formed. Farmed accounts lean heavily on disposable addresses and recycled numbers. Phone values normalise to E.164, so the same number cannot hide behind formatting differences.


### **Document — is this the same credential?**


Identity verification emits` DUPLICATED_DOCUMENT` when the same document is submitted again, and` POSSIBLE_DUPLICATED_USER` when a submission resolves to a person already in your verified set. Both catch accounts sharing a credential even when the face or the device differs — and` POSSIBLE_DUPLICATED_USER` is skipped when the document number is on your document allowlist, so known-good credentials do not generate noise.


## **From account graph to actor graph**


Individually these are just warnings. Together they collapse a graph.


Suppose your traffic layer flags account` acct_7781` — concentrated queries against one capability, unusual timing. Per-account review gives you one verdict on one account.


Instead, take the verification session behind that account and pivot:


1. **Face** — search the session's face across your verified users. Twelve accounts share it.
2. **Device** — the session carries` DUPLICATED_DEVICE_FINGERPRINT` . Nine more accounts share the device, four of which are not in the face set because a different person completed those checks.
3. **Network** —` DUPLICATED_IP_ADDRESS` across a CIDR range pulls in another cluster.
4. **Contact** — three of the newly surfaced accounts share a phone number in E.164 form.


One flagged account has become a cluster of thirty-plus, discovered from a single alert, using signals that were already collected at onboarding. And you did not need to inspect a single prompt to get there. The traffic layer told you *something is wrong here* ; identity resolution told you *how far it goes* .


Which is also the limit of what this does. **Identity resolution does not prevent model extraction, and it does not detect it.** It never sees your prompts. What it does is turn one alert into the full set of accounts behind it, and make the next account off the same face, device or network expensive to create. Model-level output controls and semantic traffic detection remain separate, necessary layers — and they stay yours.


## **The rule that keeps this honest**


**A link is not a verdict.**


Didit is deliberate about this. In Face Search,` status` is` "Declined"` only when a blocklist match is found. A pure duplicate match returns` "Approved"` with` DUPLICATED_FACE` in the warnings — informational. The dedupe policy is yours, not ours.


That default is correct, because duplicates have innocent explanations. A developer with a personal account and a company account. A shared office network producing` DUPLICATED_IP_ADDRESS` for a dozen unrelated engineers. A family device. A university lab where twenty students verify from the same room.


The right way to use links is as **evidence that raises or lowers a decision you were already making** , not as an automatic ban. Two accounts sharing a face is weak on its own. Twelve accounts sharing a face, a device, a network range and a query signature is not weak at all. Warning actions are configurable per code — a blocklist hit can force a decline while a recovered-device signal routes to review — so the escalation ladder is yours to set.


## **Use cases**


**AI API platforms** correlating a traffic-layer alert to the full set of accounts an operator controls, instead of banning one and waiting for the next.


**Trial and credit abuse** — free-tier farming uses identical mechanics. The same primitives that surface a distillation cluster surface a promo-abuse cluster.


**Marketplaces and gig platforms** detecting sellers or couriers re-registering after removal.


**iGaming** enforcing single-account rules and self-exclusion, where the same person returning under a new identity is the core compliance failure.


## **Frequently asked questions**


**Does this require storing biometric data?**


Face Search runs against the face index your application builds through prior verifications — sessions with` save_api_request=true` , or Passive Liveness with` save_api_request=true` . You control what enters the index and you control retention, in line with your own privacy notice and legal basis. If you do not enrol faces, 1:N search has nothing to search.


**What if the operator uses different people for each account?**


Then the biometric layer thins out and the device, network and contact layers carry the weight — which is exactly why linking uses several independent families rather than one. Paying real humans to complete checks is also the single most expensive way to farm accounts, which is the point: it moves the attacker onto costs that scale.


**How fast is a 1:N search?**


Sub-two-second response. It also runs automatically during liveness inside a verification session, so in most flows the linking signal arrives with the verification result.


**Won't this generate false positives on shared networks?**


` DUPLICATED_IP_ADDRESS` alone is a weak signal and should be treated that way — offices, universities and mobile carrier NAT all produce it legitimately. Weight it low, require corroboration from an independent family, and configure the action per code rather than declining on any single warning.


**Can I search a face that never went through a Didit verification?**


Yes. Face Search accepts any` user_image` (jpg, jpeg, png, tiff or webp, up to 5 MB — PDFs are not accepted) and searches it against your index. If no face is detected the call returns HTTP 400.


## **Ready to get started?**


Cross-account linking is available on every Didit account — no separate product, no minimum.


- **Read the docs** —[Face Search 1:N](https://docs.didit.me/core-technology/face-search/overview) and the[IP & Device Analysis warning catalogue](https://docs.didit.me/core-technology/ip-analysis/warnings-ip-analysis) .
- **See the product** —[User Verification](https://didit.me/products/user-verification) .
- **Check the pricing** —[publicly listed](https://didit.me/pricing) : Face Search 1:N free, IP and device analysis $0.03, full verification bundle $0.33.
- **Start free** —[business.didit.me](https://business.didit.me/) , 500 KYC verifications a month at no cost.


Keep reading


## Related articles


- [The Hydra Account Problem: Why Distillation Defense Starts With Identity Resolution](https://didit.me/blog/llm-distillation-defense-identity/)
- [Business Verification for AI API Access: Who Actually Controls This Account?](https://didit.me/blog/kyb-ai-api-enterprise-access/)
- [Verified API Access for AI Model Providers: A Risk-Tiered Architecture](https://didit.me/blog/verified-api-access-ai-model-providers/)
- [Face Search 1:N: Finding Every Account One Person Controls](https://didit.me/blog/face-search-duplicate-account-detection/)
- [Biometric Step-Up for AI API Access: Binding Privilege to a Person](https://didit.me/blog/biometric-authentication-ai-api-access/)
- [Blocklist Propagation: Making One Confirmed Abuse Case Kill the Whole Network](https://didit.me/blog/ai-api-abuse-blocklist-propagation/)
