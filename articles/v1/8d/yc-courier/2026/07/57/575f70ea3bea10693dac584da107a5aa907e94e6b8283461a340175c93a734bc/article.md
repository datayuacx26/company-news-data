---
schema_version: "1.0.0"
document_id: "575f70ea3bea10693dac584da107a5aa907e94e6b8283461a340175c93a734bc"
company_key: "yc-courier"
company: "Courier"
source_id: "yc-courier-news-import-df9818472bef"
canonical_url: "https://www.courier.com/blog/email-open-tracking-consent"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-25T00:50:30.955928+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:274b6f48058d21424188155ccd61641a1e902cc4365db2bb2a19294c3a2a5e10"
---

# Email open tracking and consent: the new rules in Europe

For years, the pixel that tells you an email was opened sat in a regulatory grey zone. Cookie banners governed the trackers on your website, while the invisible tracker inside your email went mostly unquestioned. That's changing: regulators in Europe have started treating the email open pixel like a cookie, as something that accesses the recipient's device and therefore needs consent.


**The short version:**


- **What changed:** France (CNIL) and Italy (Garante) now treat the email open-tracking pixel like a cookie. Using it for marketing, profiling, or performance measurement needs the recipient's consent.
- **Who's affected:** Anyone emailing people in France or Italy, including US companies. The rule follows the recipient's location, not your headquarters. If your whole audience is in the US, it doesn't reach you.
- **What to do:** Keep sending as usual, and fire the tracking pixel only for recipients who've consented. Consent controls tracking, not delivery.


The rest of this post explains each of those points, then shows how to set it up if you send with Courier.


## What changed, and why the email pixel now needs consent


The change starts from a single reclassification. A tracking pixel is a tiny, invisible image embedded in an email. When the recipient opens the message, the image loads from your server, and that load is what tells you the email was opened. Click tracking works on the same principle, routing your links through a redirect that records the click.


Regulators now argue that loading that pixel means reading from and writing to the recipient's device, which is the same legal trigger that governs cookies. Under that reading, firing the pixel for marketing, profiling, or campaign-performance measurement needs the recipient's prior consent, the same standard a cookie banner exists to collect. Two European data protection authorities formalized this in 2026:[France's CNIL](https://www.cnil.fr/fr/recommandation-pixel-suivi-courriels) and[Italy's Garante](https://www.garanteprivacy.it/home/docweb/-/docweb-display/docweb/10241943) . They differ in the details and in legal force, but both land on the same core rule: get consent before you track.


The consent requirement itself comes from Europe's[ePrivacy Directive](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32002L0058) (the same legal family as cookie consent), while GDPR sets the standard that consent has to meet: freely given, specific, informed, and unambiguous, with profiling held to an even higher bar. This isn't a one-country quirk anymore, and the underlying logic (a pixel is a tracker, and trackers need consent) is broadly consistent with privacy principles that apply across the EU. More authorities may follow, so treat this as a pattern to design for, not a single deadline to scramble against.


## The key idea: sending and tracking are separate


One distinction matters most: these rules govern **tracking** , not **sending** .


You can still deliver the email to someone who hasn't consented to tracking. The receipt, the password reset, the shipping update, the newsletter they opted into: all of that still goes out. What you can't do is fire the tracking pixel for that send, so you won't learn whether they opened it or clicked. Compliance means withholding the pixel, not the message.


The CNIL is explicit that these are two separate questions. You might have a perfectly valid basis to send someone an email and still need separate consent before you drop a tracking pixel into it. So the mental model to hold onto is: **send always, track only with consent.** Everything practical flows from that.


## Who needs consent to track email opens?


Scope is where people over-correct, so be precise about it. Both measures come from national implementations of the EU ePrivacy Directive, which means they follow the recipient's location, not where your company is based.


- Recipients in **France** are covered by the CNIL's guidance.
- Recipients in **Italy** are covered by the Garante's provision.
- Recipients **anywhere else** , including the US and the rest of the EU, aren't covered by these two specific rules today.


If your entire audience is in the US, you can keep tracking exactly as you do now. The case that catches people out is the cross-border one: a US-based company that emails recipients in France or Italy is in scope for those recipients, because the obligation travels with the person, not the sender. Whether a regulator would realistically pursue a company with no European presence is messier, but the safe reading is that the rule applies wherever the recipient is.


There's a practical wrinkle that shapes every compliance approach: an email address doesn't tell you where someone actually lives. Trying to apply tracking rules only to the people you *think* are in France or Italy is unreliable and hard to maintain. The cleaner move is usually an audience-wide decision, which we'll come back to.


## What compliance requires


The specifics differ between France and Italy (one is a binding provision, the other a best-practice recommendation, and they carry different transition deadlines). For the full breakdown, including the exact dates, the narrow exemptions, and a step-by-step remediation checklist, see the[email tracking consent section of our transactional email guide](https://www.courier.com/guides/the-developers-guide-to-transactional-email/chapter-4-operating-transactional-email-at-scale#how-to-handle-email-open-tracking-consent) . At the principle level, though, both point at the same handful of requirements:


- **Collect consent when you collect the email address.** The strongest approach asks for tracking consent at signup, on the form where you gather the address. Asking inside a later welcome email tends to fail, because the pixel in that email fires before the recipient has had a chance to answer.
- **Keep tracking consent distinct from send consent.** Consent to receive your email and consent to be tracked are different permissions. Bundling them is allowed in some cases only when it's presented neutrally, but treating them as separate signals is cleaner and safer.
- **Make withdrawal as easy as granting.** Recipients need a low-friction way to turn tracking off, typically a link in the email footer, without re-entering their address and without having to unsubscribe from your emails entirely.
- **Record each person's choice.** Store consent as a per-recipient value you can act on at send time, and keep a record of it as proof.
- **Pick a default and apply it consistently.** You can track until someone objects (opt-out), track only people who've affirmatively consented (opt-in), or disable open tracking altogether. Opt-in is the strictest and the one that satisfies a consent-first requirement.


The exemptions are narrower than people hope. Consent generally isn't required when the pixel is used strictly for things like delivering the service the recipient asked for, aggregate anonymized statistics, or security and authentication. The moment tracking feeds marketing, profiling, or performance, you're back in consent territory. The guide covers the exemptions in full.


## How consent affects your open and click rates


This one surprises people, so set expectations early. When you stop tracking part of your audience, your open and click rates are calculated over fewer tracked sends. That's expected, not a malfunction. The honest way to read engagement afterward is to compare tracked sends against tracked sends, rather than expecting the same absolute open rate you saw when you tracked everyone. Open rate was already a shaky metric thanks to things like Apple Mail Privacy Protection pre-loading pixels; consent-based tracking is one more reason to lean on click-through and task completion for the numbers that matter.


## How to handle this in Courier


If you send with Courier, here's how the "send always, track only with consent" model maps onto the product. The whole approach comes down to controlling one lever, the tracking pixel, based on a consent signal you store per recipient.


### The tracking controls you have


Courier lets you control email tracking at three levels:


- **Globally** , under Settings > General, where you can turn open tracking and click-through tracking on or off for the entire workspace. See the[workspace settings overview](https://www.courier.com/docs/platform/workspaces/workspaces-overview) .
- **Per link** , in the template designer, where you can turn off tracking for an individual hyperlink or action block.
- **Per send** , with a tracking override in your send request. This is the one that makes consent workable, because it turns tracking into a per-recipient decision.


The per-send override looks like this. Set` open` to` false` and Courier delivers the email without the pixel:


```text
{         "message"  :     {           "to"  :     {     "user_id"  :     "8675309"     }  ,           "template"  :     "PASSWORD_RESET"  ,           "channels"  :     {             "email"  :     {               "override"  :     {                 "tracking"  :     {     "open"  :     false     }               }             }           }         }      }
```


The[email override reference](https://www.courier.com/docs/external-integrations/email/intro-to-email) documents the full set of fields.


### Store consent as a user attribute


Courier[user profiles](https://www.courier.com/docs/platform/users/users) hold custom attributes, so record each person's tracking choice there. Treat it as three states: consented, objected, or unset.


```text
{         "profile"  :     {           "email"  :     "renee@example.com"  ,           "email_open_tracking_consent"  :     "granted"         }      }
```


Set it wherever you capture user data: at signup, through the API when someone updates their preferences, or via a bulk import when you backfill your existing audience. A missing value means "no consent recorded," which you should treat as "don't track" if you're taking the opt-in approach.


### Decide whether to track at send time


If you send through the API, read the attribute and set the override accordingly. Everyone gets the email; only consented recipients get the pixel:


```text
const     {     CourierClient     }     =     require  (  "@trycourier/courier"  )  ;      const   courier   =     CourierClient  (  {   authorizationToken  :   process  .  env  .  COURIER_AUTH_TOKEN     }  )  ;
async     function     sendReceipt  (  user  ,   receipt  )     {         const   hasConsent   =   user  .  email_open_tracking_consent     ===     "granted"  ;
await   courier  .  send  (  {         message  :     {           to  :     {   user_id  :   user  .  id     }  ,           template  :     "ORDER_RECEIPT"  ,           data  :     {   receipt   }  ,           channels  :     {             email  :     {               override  :     {                   // Track only when the recipient has consented.                 tracking  :     {   open  :   hasConsent   }  ,                 }  ,               }  ,             }  ,           }  ,         }  )  ;      }
```


If your sends run through[Courier Journeys](https://www.courier.com/docs/platform/journeys/nodes/branch) instead of direct API calls, use a Branch node on the consent attribute: route consented recipients down a path that sends with tracking, and everyone else down a path that sends without it. Both paths deliver the same email, so nobody loses their message.


### Capture and withdraw consent through the preference center


Courier's[preference management](https://www.courier.com/docs/platform/preferences/preferences-overview) gives you a hosted, no-login page where recipients control their choices, and you can[add a preference link to any email footer](https://www.courier.com/docs/tutorials/preferences/how-to-set-up-hosted-preference-center) . Use that as the place people grant and withdraw tracking consent, and write their choice back to the` email_open_tracking_consent` attribute so your next send respects it. That covers the "easy withdrawal" requirement without building your own page.


Keep one thing straight: a notification preference controls whether Courier *sends* a given notification, while tracking consent controls whether Courier *tracks the open* on an email it's already sending. Keep them as separate signals so opting out of tracking never accidentally stops someone's password resets.


### What Courier doesn't do yet


Being straight about the gap: Courier gives you the mechanisms to control tracking, but it doesn't yet have a built-in consent mode that reads a reserved attribute and enforces it automatically across every send. You assemble the behavior from the primitives above, and the consent check is logic you write and maintain, so it won't apply itself to a new send path unless you wire it in. If a native tracking-consent mode would help your team, tell us. That's the kind of feature that gets built when customers ask for it.


## Frequently asked questions


### Do you need consent to track email opens?


In France and Italy, yes, as of 2026. Both countries' data protection authorities treat the open-tracking pixel like a cookie, so firing it for marketing, profiling, or performance measurement requires the recipient's prior consent. Elsewhere, including the US and the rest of the EU, these specific rules don't apply yet. Consent affects tracking, not delivery: you can still send the email without the pixel.


### Does email open-tracking consent apply to US companies?


It can. The rules follow the recipient's location, not the sender's headquarters, so a US-based company that emails recipients in France or Italy is in scope for those recipients. A US company that only emails people in the US isn't affected by these two rules.


### Can you still send emails without tracking consent?


Yes. These rules govern the tracking pixel, not the message. You can deliver receipts, password resets, shipping updates, and opted-in newsletters to someone who hasn't consented to tracking. You send the email without the open pixel, so you won't know whether they opened it.


### Is email tracking consent the same as an unsubscribe or marketing opt-in?


No. Consent to receive an email and consent to be tracked are separate permissions. You might have a valid basis to send someone an email and still need distinct consent before adding a tracking pixel. Withdrawing tracking consent should turn off tracking without unsubscribing the recipient from your emails.


### Does GDPR require consent for email tracking pixels?


The consent requirement comes primarily from Europe's ePrivacy rules, the same framework behind cookie consent, which France and Italy implement in national law. GDPR sets the standard that consent must meet (freely given, specific, informed, and unambiguous) and governs any profiling built on the tracking data. In practice the two work together: ePrivacy says you need consent for the pixel, and GDPR defines what valid consent looks like.


## Wrapping up


The regulatory shift is real, but the response is straightforward once you hold onto the core idea: sending and tracking are separate, and only tracking needs consent. You keep delivering email to everyone; you fire the pixel only for the people who've said yes.


To put it in place, wherever you send:


- Understand your scope. This is France and Italy today, based on where your recipients are, not where you're based.
- Capture tracking consent at collection, and give recipients an easy way to withdraw it.
- Store each person's choice and act on it at send time, defaulting to untracked.
- In Courier specifically, store consent as a user attribute and set the per-send` tracking.open` override (or branch in a journey), using the preference center for capture and withdrawal.


For the full regulatory detail, the France and Italy deadlines, and the exemptions, read the[email tracking consent section of the transactional email guide](https://www.courier.com/guides/the-developers-guide-to-transactional-email/chapter-4-operating-transactional-email-at-scale#how-to-handle-email-open-tracking-consent) . If you're setting this up in Courier and hit a wall,[reach out](https://www.courier.com/docs/welcome) .


**Primary sources:**


- [CNIL, "Recommandation relative aux pixels de suivi dans les courriels"](https://www.cnil.fr/fr/recommandation-pixel-suivi-courriels) (France;[English PDF](https://www.cnil.fr/sites/default/files/2026-05/recommandation_tracking_pixels_emails.pdf) )
- [Garante per la protezione dei dati personali, Provvedimento n. 284 del 17 aprile 2026](https://www.garanteprivacy.it/home/docweb/-/docweb-display/docweb/10241943) (Italy)
- [ePrivacy Directive 2002/58/EC](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32002L0058) (the EU legal basis both authorities rely on)


*This post is informational and isn't legal advice. You're responsible for determining which laws apply to your audience and how to configure your sending to meet them.*
