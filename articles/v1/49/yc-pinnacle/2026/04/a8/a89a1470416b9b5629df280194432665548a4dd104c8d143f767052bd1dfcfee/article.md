---
schema_version: "1.0.0"
document_id: "a89a1470416b9b5629df280194432665548a4dd104c8d143f767052bd1dfcfee"
company_key: "yc-pinnacle"
company: "Pinnacle"
source_id: "yc-pinnacle-news-import-eb12d43fa2a4"
canonical_url: "https://pinnacle.sh/blog/sms-pre-send-filter"
published_at: "2026-04-29T00:00:00+00:00"
first_seen_at: "2026-07-24T09:16:03.254405+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:d3f4212f55d04857087cc1bbc3e37d989c466fb18384f52170ac86333f94929e"
---

# Building an SMS Pre-Send Filter: The Architecture That Actually Works

## The Filter Most Teams Build First (and Regret)


Most first attempts at a pre-send check look the same: grab a list of bad words from somewhere, compile it into a regex, block on match. Within a few weeks the support queue fills up — half the tickets are "why did my legitimate appointment reminder get blocked," and the other half are "why did my marketing blast still get filtered by T-Mobile." Both are the same problem.


A regex of bad words is the wrong architecture. Carriers don't filter that way, and your filter shouldn't either. What follows is the architecture Pinnacle uses: three-tier verdicts, family-level lexicons, Unicode normalization, and a small set of behavioral controls that compose with the content check.


For the broader picture of what carriers actually filter and why, see[SMS Carrier Filtering: What Actually Blocks Your Messages](https://www.pinnacle.sh/blog/sms-restricted-keywords) . This one is the implementation.


## Why Token Lists Don't Hold Up


Token-only filters miss context.[Davidson et al.](https://arxiv.org/abs/1703.04009) and most of the hate-speech-detection literature converge on the same finding: lexical matching has poor precision because the same string can be a slur in one message and a quoted reference in another. The regex doesn't know which one it's looking at.


They're also trivial to evade. Inserted spaces, character swaps, homoglyphs, leetspeak, synonym substitution, hybrid attacks — the attacker only needs to read your list once. Carriers know this and don't bet their networks on lexical match alone.


The third problem is policy. T-Mobile prohibits filter-evasion assistance, snowshoeing, URL cycling, redirects, and number cycling. AT&T warns against misspelling and unusual capitalization to evade. The major US messaging policies forbid intentional misspelling. A filter that quietly "helps" senders dance around the rules is itself a Severity 0 risk.


The architecture that holds up is high-recall family matching plus behavioral and consent controls. Not a giant raw token list.


## Block, Flag, Allow


Three verdicts, applied in order. Every message lands in exactly one.


- **Block** — explicit, sale-intent, or Severity 0 content. Refuse to send. Examples: explicit adult content, hate-speech families, firearm sales, controlled-substance promotion, cannabinoid retail, and most vapor-product promotion.
- **Flag** — age-restricted or borderline content. Hold for human review, verify route plus age-gate, then send. Examples: alcohol promotions, tobacco-product promotion, and obscene-language patterns in promotional copy.
- **Allow** — neutral, branded, transactional content with valid consent. Send.


Splitting "no" into block (auto-reject) and flag (queue for human review) is what recovers the precision a binary filter loses. Most false positives in filtering pipelines come from treating ambiguous content the same way as Severity 0 content.


## Decision Logic


The actual content check is two branches.


TypeScript


```text
function   preSendCheck  (  message  :   Message  ,   sender  :   Sender  )  :   Verdict   {
// Pre-flight (consent, registration, route, URL, evasion)
if   (  !  hasValidConsent  (  message  .  recipient  ,   sender  .  brand  ))   return   block  (  "  consent  "  );
if   (  !  sender  .  tcr  .  active  )   return   block  (  "  registration  "  );
if   (  !  routeAllowsCategory  (  sender  .  route  ,   sender  .  category  ))   return   block  (  "  route  "  );
if   (  containsRiskyUrl  (  message  .  body  ))   return   block  (  "  url  "  );
if   (  containsEvasionPattern  (  message  .  body  ))   return   block  (  "  evasion  "  );


// Family match (after Unicode normalization)
const   families   =   matchFamilies  (  normalize  (  message  .  body  ));
if   (  families  .  some  (  f   =>   f  .  tier   ===   "  blocked  "  ))   return   block  (  "  content  "  ,   families  );
if   (  families  .  some  (  f   =>   f  .  tier   ===   "  restricted  "  ))   return   flag  (  families  );


return   allow  ();
}
```


Everything else — family lexicons, URL allow-list, consent ledger, TCR mirror, route-category matrix — is infrastructure that the two branches sit on top of.


## Family-Level Lexicons, Not Token Regex


The` matchFamilies` function takes a normalized message and returns a list of matched lexeme families. Each family has a tier (blocked, restricted, likely-blocked) and a set of representative tokens.


⚠️


#### Family lexicon shape (hidden by default)


This block uses SHAFT and drug-category identifiers as keys. Hidden by default to keep this page out of generic content-policy heuristics — reveal to copy the shape into your own implementation.


Two ideas matter about this shape. First, families carry the tier; tokens don't. A token belongs to a family, and the family has a tier. That decoupling lets you update token coverage (add a new euphemism, remove a false positive) without touching policy, and adjust policy per route without rewriting your matchers.


Second, routes can override family tiers. Cannabinoid content on T-Mobile US is tier` blocked` . The same family on certain Canadian operators with proper registration might be tier` restricted` (flag for review). One family, two routes, two verdicts. The override lives in your route matrix, not in the family definition itself.


The 15 families with the strongest public consensus across CTIA, T-Mobile, AT&T, and the major US messaging-policy frameworks are listed in the reference below. It's wrapped behind a reveal because the row labels include several restricted-content category names.


⚠️


#### Prioritized 15-family reference (hidden by default)


This table lists restricted-content category labels — sex, hate, firearms, controlled-substance, tobacco, alcohol, and obscene-language families — with tier and recommended action. Hidden by default to keep this page out of generic content-policy heuristics. Reveal to use it for template auditing.


For the copyable token-level reference per family, see the hidden list in[the broader filtering guide](https://www.pinnacle.sh/blog/sms-restricted-keywords) .


## Normalization Comes First


Run normalization **before** family matching. Otherwise leetspeak, homoglyphs, and inserted spaces all bypass your matchers.


TypeScript


```text
function   normalize  (  body  :   string  )  :   string   {
return   body
.  toLowerCase  ()
.  normalize  (  "  NFKC  "  )                     // canonical Unicode form
.  replace  (  /[  ​-‍­  ]/  g  ,   ""  )   // zero-width chars, soft hyphens
.  replace  (  CONFUSABLES  ,   c   =>   CONFUSABLES_MAP  [  c  ])   // Cyrillic а → Latin a
.  replace  (  /(  \w  )[  \s.  \-_  ]  +  (?=  \w  )/  g  ,   "  $1  "  )   // collapse spacing inside words
.  replace  (  /(  .  )  \1  {2,}  /  g  ,   "  $1  "  );            // squash repeated-letter masking
}
```


Two rules about how to use these signals:


1. **Normalization signals only escalate.** If a message matches a family AND triggers a normalization signal, the verdict goes up (allow → flag, flag → block). Never use them to suggest acceptable rewrites — that trains senders to encode against your filter, which T-Mobile and the major US messaging policies both prohibit.
2. **Leetspeak detection runs separately from family match.** A` \[a-z\]*\\d\[a-z\]*` pattern near a family root token is enough to trigger evasion-tier review. Don't try to write` m0ney` ,` c@sh` ,` v1agra` into your family token lists; let the normalizer collapse them and the leetspeak detector flag them.


## The Pre-Flight Checks


Most filtering decisions actually happen here, before the content match runs at all.


**Consent** . The recipient opted in to messages from the specific brand. Not transferred from an affiliate. Not bought from a list. CTIA and the major US messaging policies all enforce this independently of content.


**Registration** . The campaign is registered with TCR (10DLC) or the toll-free verification system. The use case description matches the live traffic. Pinnacle's[registration guide](https://www.pinnacle.sh/blog/sms-compliance-10dlc-toll-free-rcs-registration-guide) covers the details.


**Route + category** . Cannabinoid content on a US T-Mobile-routed campaign is blocked regardless of state legality. Alcohol on an unregistered campaign is blocked regardless of age-gating. Maintain a route × category matrix and default-deny anything not explicitly approved.


**URL hygiene** . Reject messages with public shorteners (bit.ly, tinyurl, t.co, ow.ly), chained redirects, raw IP URLs, non-HTTPS URLs, or domains under 30 days old. Replace them with a branded short domain.


**Evasion patterns** . Misspelling-to-evade, snowshoeing (rotating numbers across a burst), URL cycling, number cycling. T-Mobile and the major US messaging policies all treat these as filter-evasion.


TypeScript


```text
function   containsEvasionPattern  (  body  :   string  )  :   boolean   {
return   (
LEETSPEAK_NEAR_FAMILY_ROOT  .  test  (  body  )   ||
HOMOGLYPH_DETECTOR  .  matches  (  body  )   ||
SPACING_INJECTION  .  test  (  body  )   ||
ALLCAPS_RATIO  (  body  )   >   0.5
);
}
```


## The Sender Control Stack


Family matching is one of nine controls. They compose — leaving one out tends to mean the others can't actually protect you.


Control What to implement Why it matters


Family-tier policy Three tiers (block / flag / allow); SHAFT plus controlled-substance categories in block; alcohol, tobacco, and obscene-language patterns in flag Aligns to public carrier categories rather than ad hoc token lists


Text normalization Lowercase, NFKC, confusable mapping, whitespace collapsing — before family matching Detects evasion without publishing bypass strings


Subject-specific consent Direct, brand-specific opt-in tied to the actual message subject; no transferring or reselling consent Consent failures are a major blocking vector independent of content


Brand and opt-out hygiene Brand name in recurring messages; standard STOP/HELP handling Reduces spam classification risk; required by US A2P guidance


DOB-based age-gating For alcohol or eligible tobacco: real DOB verification before opt-in or restricted-content exposure Public sources reject simple yes/no age confirmations


Route + geography matrix Per-sender-type and per-destination route matrix; default-deny for conditional families Different routes have different SHAFT tolerances; Verizon, T-Mobile, and toll-free differ


Alignment review Sample messages, live messages, website, privacy policy, and TCR registration must all describe the same use case Provider review classifies the entire submission, not just the SMS body


URL hygiene Branded short domain only; no public shorteners; no chained redirects; HTTPS; domain over 30 days Public shorteners are heuristically blocked on 10DLC


Human review queue All flagged messages reviewed by a human before send Family matching is high-recall, not perfectly precise — review protects against false positives


## Compliant Templates Pass First Time


The single best template pattern is **boring on purpose** . Brand name first, neutral payload second, support and opt-out last. Five patterns that pass review on the first submission:


text


```text
Acme Dental: Reminder—your appointment is tomorrow at 2:30 PM.
Reply C to confirm or call 555-0100 to reschedule.
Reply STOP to opt out.
```


text


```text
Acme Store: Your order 48291 has shipped.
Track at acme.example/track/48291.
Reply STOP to opt out of shipping alerts.
```


text


```text
Acme Utilities: Payment of $84.20 received on Apr 29.
Receipt: acme.example/r/1038.
Reply STOP to opt out of billing alerts.
```


text


```text
Acme Internet: Service in your area has been restored as of 4:18 PM.
If you still need help, call 555-0102.
Reply STOP to opt out of outage alerts.
```


text


```text
Acme Bank: Your security code is 482913.
It expires in 10 minutes. Do not share this code with anyone.
```


All five examples share the same shape: an identified sender, one purpose per message, a branded short domain instead of a public shortener, and clean STOP/HELP language. CTIA and T-Mobile both prefer that over stylized copy.


## How Pinnacle Implements This


Everything above runs at the Pinnacle API layer before a message touches the carrier:


- The` /messages` endpoint matches templates against the family-tier policy at send time. Block-tier matches return a` 400` with the matched family. Flag-tier matches return` 202 Accepted` and hold the message in a review queue.
- Unicode normalization, confusables mapping, whitespace collapsing, and zero-width-char stripping all run before family matching. Your code doesn't need to think about evasion.
- Per-carrier delivery analytics in the dashboard separate "accepted by carrier" from "delivered to handset," with breakdowns by carrier. Silent T-Mobile filtering surfaces in minutes.
- Every account ships with a custom branded short domain. No` bit.ly` traffic.
- Anti-evasion lint rejects messages with misspelling-to-evade or homoglyph substitution at the API to protect your sender reputation across the entire 10DLC ecosystem.
- For regulated verticals (healthcare, financial services, and age-restricted SHAFT categories), our compliance team maintains template patterns that pass carrier review on the first submission.


Building this from scratch is roughly a quarter of engineering plus a year of feedback from real carrier rejections. We've spent that time already.


## Caveats


This architecture is most reliable for US and Canada A2P, where the deepest public materials live. A few honest gaps: no reviewed source publishes a complete carrier-owned token inventory, so the family lexicons are necessarily approximations of what carriers actually enforce. Verizon's posture is "comply with CTIA" rather than detailed proprietary content rules; AT&T's docs are AUP-oriented. EU and UK regimes are less specific in their public docs, so international filter intelligence comes from operator-by-operator empirical patterns instead of a published spec.


The takeaway: govern the program around intent, linked content, and workflow controls, not just dictionaries.


## Book a Call


If you're building this filter yourself, we'd rather you ship on top of ours than reinvent it. We've worked with teams across SMS-first startups, healthcare and fintech programs, and regulated retail.[Book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+building+an+SMS+pre-send+filter) . We'll go through your pipeline, look at how our pre-send filter handles your templates, and put you on a configuration that delivers.


## Key Takeaways


- A binary "match the bad word" filter is the wrong architecture. Use three tiers: block (auto-reject), flag (human review), allow (send).
- Family-level lexicons beat token-level regexes. Update token coverage without changing policy; adjust policy per route without rewriting matchers.
- Normalize before you match. Leetspeak, homoglyphs, zero-width chars, and inserted spaces all bypass family matchers if you skip Unicode NFKC + confusable mapping + whitespace collapsing.
- Five pre-flight checks (consent, registration, route, URL, evasion) catch most filter-able messages before content matching even runs.
- Pinnacle's API ships the entire pipeline so most senders don't need to build their own.


## FAQ


**1. Should the filter ever auto-rewrite a message?** No. Return flagged messages to the sender for review and rewrite. Auto-rewriting trains senders to encode against your filter, which T-Mobile and the major US messaging policies both prohibit.


**2. How often should I update the family lexicons?** Token coverage inside each family: quarterly is the right cadence. The family taxonomy itself shifts much more slowly — usually only when CTIA or a carrier publishes new policy.


**3. How do I handle non-English content?** Same family-level architecture, but family matching needs language-specific lexicon coverage. Layer language-detection upstream of family matching, and maintain family lexicons per language. Hatebase covers many hate-term families across languages and is useful for recall (not as carrier ground truth).


**4. What about RCS — does it need the same filter?** RCS messages from a registered, branded sender are far less aggressively filtered, because the sender is verified end-to-end. Same SHAFT category rules, much lower false-positive rate. See our[RCS for finance](https://www.pinnacle.sh/blog/rcs-for-finance) ,[healthcare](https://www.pinnacle.sh/blog/rcs-for-healthcare) , and[e-commerce](https://www.pinnacle.sh/blog/rcs-for-ecommerce) guides.


**5. Can I get Pinnacle's pre-send filter without using the full API?** Currently the filter is part of the` /messages` API. For high-volume senders we sometimes work out custom integrations —[talk to us](https://www.pinnacle.sh/contact?message=I%27m%20interested%20in%20Pinnacle%27s%20pre-send%20SMS%20filter%20and%20compliance%20pipeline.) about your setup.
