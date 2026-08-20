---
schema_version: "1.0.0"
document_id: "7b3e9c29da6d128913aae013982363043d5f2c5d34c007449ad97cc2b3118805"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/drive-thru-voice-agent-order-testing-template"
published_at: "2026-06-20T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:a919a47e7a5c69196e95dddbf6b30a889e5aa5d76b0eb4ac94c24890272b5059"
---

# Drive-Thru Voice Agent Order Testing Template

**Drive-thru voice agent order testing** proves that a restaurant voice agent can take a messy spoken order and leave the store with the same order in the cart, POS, and readback.


If the agent only answers store hours or loyalty-program FAQs, this is too much. A few conversation tests are fine.


If it takes drive-thru, phone, kiosk, or catering orders, transcript-only testing is where teams get burned. The call can sound normal while the cart contains the wrong modifier, the combo lost its drink, the POS accepted an unavailable item, or the allergy note never made it to the kitchen.


That failure has a name I would put on the wall of every restaurant voice AI test plan: **cart drift** . The caller and agent seem aligned, but the durable order state is no longer the order the caller asked for.


> **TL;DR:** Build drive-thru voice agent tests as a menu-cart-POS matrix:
>
>
> - **Menu state:** store, menu version, item IDs, modifier groups, prices, availability, and schedule.
> - **Caller order:** the exact spoken phrase, including substitutions, allergies, corrections, and background noise.
> - **Cart snapshots:** expected cart state after every add, remove, change, upsell, and readback.
> - **POS mode:** mock, sandbox, test store, or narrow live-scoped path.
> - **Evidence:** transcript, tool trace, item and modifier IDs, price check, final POS state, and cleanup result.
>
>
> A passing transcript is useful. It is not proof of an accurate order. The cart and final order state have to agree.


> **Methodology Note:** This template is based on Hamming's analysis of production voice agent calls involving order capture, menu state, tool calls, POS writes, background noise, and QSR-style workflow failures across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> Use it as a launch-safety template for drive-thru, phone ordering, kiosk, and catering voice agents that touch real menu or POS state.


**Last Updated:** June 2026


**Related Guides:**


- [Hamming for QSR voice AI testing](https://hamming.ai/industry/qsr) - QSR-specific testing surface for menu orders, customizations, load, and drive-thru conditions
- [Lilac Labs customer spotlight](https://hamming.ai/blog/lilac-labs-customer-spotlight) - public example of automated drive-thru order accuracy testing
- [Background Noise Voice Agent Testing](https://hamming.ai/resources/background-noise-voice-agent-testing-kpis) - test speaker distortion, engines, wind, passengers, and store noise
- [Voice Agent Sandbox Testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) - run side-effect tests without production writes
- [Customer-Specific Workflow Rules Template](https://hamming.ai/resources/voice-agent-customer-workflow-rules-testing-template) - handle store, franchise, region, or account-specific rules
- [Voice Agent Workflow Testing Runbook](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - broader workflow and tool-call coverage
- [Voice Agent Tests as Code](https://hamming.ai/resources/voice-agent-tests-as-code-template) - keep menu fixtures and expected cart state reviewable in Git
- [Voice Agent Load Testing Guide](https://hamming.ai/resources/voice-agent-load-testing-guide) - test lunch rush, dinner rush, and promo spikes
- [Structured Output Validation Checklist](https://hamming.ai/resources/voice-agent-structured-output-validation-checklist) - validate extracted items, modifiers, and order fields
- [Failed Production Call Regression Runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook) - turn missed orders into repeatable tests


## What Drive-Thru Order Testing Should Prove


Drive-thru order testing checks whether the voice agent can turn the way people actually order food into the exact durable order the restaurant expects.


Layer What the test proves Failure example


Menu understanding The agent maps speech to canonical menu items, modifier groups, sizes, prices, and availability "No cheese" is captured as a note instead of a modifier removal


Cart mutation The cart changes correctly after each turn Caller says "make that a large" and both the medium and large remain in the cart


Readback The agent confirms the order in language the customer can correct Agent reads back the wrong drink or skips the allergy note


POS write The final order sent downstream matches the confirmed cart POS receives a stale item ID from a previous menu version


Operational safety Tests avoid real orders unless the release owner explicitly allows a scoped live check CI creates a kitchen order or payment authorization


> **Drive-thru order test:** a voice agent test that loads a menu snapshot, runs a spoken ordering scenario, verifies cart state after every mutation, checks final POS or order state, and saves cleanup evidence.


The hard part is not "Can the agent understand fries?" It is whether it keeps the same order intact after the caller changes their mind, a passenger adds a drink, the lunch menu is missing an item, and the speaker sounds like it was installed in 2008.


## Build the Menu-Cart-POS Matrix


Start with a matrix. Do not start by dialing the agent 200 times.


Field What to capture Sample


Scenario ID Stable identifier for the test` qsr_combo_modifier_017


`


Store fixture Store, franchise, daypart, menu version, tax region` store_fixture_midwest_03


` , breakfast disabled


Caller phrase What the customer says "Can I get a cheeseburger combo, no pickles, Coke Zero, and make the fries large?"


Menu expectation Canonical item IDs, modifier IDs, availability, price burger item, no-pickle modifier, combo drink slot, large fry upcharge


Cart checkpoints Expected cart after each add, remove, or change 1 combo, 0 pickles, Coke Zero, large fries


Forbidden state What must not happen duplicate burger, default drink, medium fries, stale price


POS mode Mock, sandbox, test store, or live scoped POS sandbox with fixture order tag


Evidence What proves success transcript, tool trace, cart snapshots, POS response, cleanup query


Gate Blocking, scheduled, pre-release, or manual blocking for allergy, payment, and POS writes


Keep this matrix near your[tests-as-code definitions](https://hamming.ai/resources/voice-agent-tests-as-code-template) . A teammate should be able to review the expected order without replaying the call.


### Copyable Matrix Starter


```text
suite  :     drive_thru_order_accuracy    owner  :     voice  -  platform    menu_version  :     qsr_menu_2026_06_20_lunch      scenarios  :        -     id  :     qsr_combo_modifier_017          store_fixture  :     store_fixture_midwest_03          dependency_mode  :            menu  :     snapshot            cart  :     sandbox            pos  :     test_store          caller_goal  :     "  Order a cheeseburger combo with no pickles, Coke Zero, and large fries  "          expected_cart_checkpoints  :            -     turn  :     "  add cheeseburger combo  "              item_ids  :     [  "  combo_cheeseburger  "  ]              modifier_ids  :     [  ]              subtotal_cents  :     849            -     turn  :     "  no pickles  "              item_ids  :     [  "  combo_cheeseburger  "  ]              modifier_ids  :     [  "  remove_pickles  "  ]            -     turn  :     "  Coke Zero and large fries  "              item_ids  :     [  "  combo_cheeseburger  "  ]              modifier_ids  :     [  "  drink_coke_zero  "  ,     "  fry_large  "  ]              subtotal_cents  :     999          forbidden_states  :            -     duplicate_combo            -     default_drink            -     medium_fry_after_large_upgrade            -     kitchen_note_instead_of_modifier_id          final_guardrails  :            readback_contains  :              -     "  cheeseburger combo  "              -     "  no pickles  "              -     "  Coke Zero  "              -     "  large fries  "            pos_order_count_for_run  :     1            cleanup_status  :     verified
```


That is more structured than a transcript score. It is also what lets you catch the order bug before a customer receives the wrong bag.


## Test Menu Understanding Before Cart Mutation


Menu understanding has to be tested before cart mutation because the wrong canonical item poisons every downstream check.


Public restaurant ordering APIs expose why this gets tricky.[Google Cloud's Food Ordering AI Agent menu integration docs](https://docs.cloud.google.com/food-ai/menu-integration) separate menu structure, modifiers, schedules, prices, and availability before ordering works.[Toast's modifier docs](https://doc.toasttab.com/doc/devguide/apiOrdersOrderModifierOptionsAndPreModifiers.html) call out behavior around default modifiers, pre-modifiers, special requests, and quantity mismatches.[Lightspeed's online ordering API docs](https://api-docs.upserve.com/olo/) similarly distinguish menu management from order creation and confirmation.


Your tests should do the same.


Spoken request Menu-state guardrail Why it matters


"No pickles" Uses the remove-pickles modifier or explicit default-modifier removal Kitchen notes may not remove the ingredient


"Extra sauce" Adds allowed modifier quantity within menu constraints Free-text notes may bypass price or kitchen routing


"Make it a large" Changes the size or combo slot, not a second item Duplicate items hide behind a correct readback


"I'll take the breakfast sandwich" at 2 PM Marks item unavailable and offers valid alternatives Menu schedules change the correct answer


"I'm allergic to sesame" Adds the safe allergy workflow or escalation path Allergy handling should not be treated as a casual note


> **Menu snapshot rule:** every order test should name the menu version it loaded. If the menu can change without changing the test expectation, the test is not reproducible.


This is where QSR tests differ from generic tool-call tests. A tool call named` add_item


` is not enough. The test has to prove the item and modifier identifiers match the restaurant's active menu model.


## Test Cart State Across Corrections


Most order bugs happen after the first item. That is why a single happy-path burger order is such weak evidence.


The caller changes their mind. A passenger interrupts. The agent suggests an upsell. The customer says "actually make that two." The agent needs to update the cart without keeping stale state.


Use turn-by-turn cart checkpoints.


Turn Caller says Expected cart state Common failure


1 "I want a spicy chicken sandwich combo" 1 spicy chicken combo, default size, default side pending Adds sandwich only, not combo


2 "Make the fries large" Same combo, large fries, upcharge applied Adds separate large fries


3 "No mayo" Same combo, remove-mayo modifier Stores note but leaves default mayo


4 "Actually make that grilled chicken" Replaces spicy chicken with grilled chicken, keeps compatible modifiers Leaves both sandwiches


5 "Add a kids meal too" Combo plus kids meal Replaces cart instead of appending


6 "Read it back" Spoken readback matches cart and price Reads back transcript memory, not cart state


The test should fail as soon as cart state diverges. Waiting until the final readback turns a small state bug into a scavenger hunt across transcript, tool trace, and POS payload.


### Evidence Envelope


```text
{        "  run_id  "  :     "  drive_thru_run_2026_06_20_0042  "  ,        "  store_fixture  "  :     "  store_fixture_midwest_03  "  ,        "  menu_version  "  :     "  qsr_menu_2026_06_20_lunch  "  ,        "  scenario_id  "  :     "  qsr_correction_024  "  ,        "  cart_snapshots  "  :     [          {            "  turn  "  :     1  ,            "  expected_item_ids  "  :     [  "  combo_spicy_chicken  "  ]  ,            "  actual_item_ids  "  :     [  "  combo_spicy_chicken  "  ]  ,            "  expected_modifier_ids  "  :     [  ]  ,            "  actual_modifier_ids  "  :     [  ]          }  ,          {            "  turn  "  :     4  ,            "  expected_item_ids  "  :     [  "  combo_grilled_chicken  "  ]  ,            "  actual_item_ids  "  :     [  "  combo_grilled_chicken  "  ]  ,            "  forbidden_item_ids_observed  "  :     [  ]          }        ]  ,        "  final_order  "  :     {          "  pos_mode  "  :     "  test_store  "  ,          "  pos_order_count_for_run  "  :     1  ,          "  price_match  "  :     true  ,          "  readback_match  "  :     true  ,          "  cleanup_status  "  :     "  verified  "        }    }
```


The vendor, test runner, or reviewer does not need private customer data. It needs enough structure to know whether the order state stayed honest.


## Prove POS And Kitchen State Without Touching Production


Use the same dependency-mode thinking from[voice agent sandbox testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) , but make the order boundary explicit.


Dependency mode Use it for Good signal Release risk


Mocked menu and cart Fast CI, negative cases, prompt changes, schema checks Agent selects the right menu IDs and cart operations Can miss POS auth, menu drift, and provider validation


Menu snapshot plus POS sandbox Integration checks with fixture stores and fake orders Final order validates against a realistic menu and POS contract Requires fixture hygiene and cleanup


POS test store Pre-release order injection and kitchen-routing checks The real order path accepts the payload Can pollute test dashboards if run IDs and cleanup are weak


Live scoped path Production-only routing, store availability, or provider limits Real path still works under release-owner controls Can create real orders if allowlists are wrong


For most teams, CI should never create a real kitchen order or payment authorization. Use mocks for speed, sandbox and test-store paths for confidence, and live scoped checks only with explicit release-owner approval.


When the voice agent does write downstream, assert the durable state:


- exactly one order for the run ID
- expected item IDs, modifier IDs, prices, tax, store, and fulfillment type
- no duplicate cart lines from retries
- no unavailable items accepted
- no production customer record touched
- cleanup verified after pass and fail


The[failed production call regression runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook) is the right place to promote any escaped order bug into this matrix.


## Cover The Edge Cases That Actually Break QSR Agents


Do not try to generate every menu combination on day one. Start with the cases that change the order, safety risk, or operations.


Edge case Test example Blocking?


Allergy or dietary restriction "I'm allergic to sesame. What can I get?" Yes


Modifier removal "No onions, no mayo" Yes


Default modifier behavior Item usually includes cheese; caller says no cheese Yes


Combo substitution Swap drink, side, size, or protein Yes


Unavailable item Breakfast item during lunch menu Yes


Special request "Sauce on the side" Usually


Upsell accepted Agent suggests fries; caller accepts Usually


Upsell rejected Agent suggests dessert; caller says no Usually


Quantity correction "Actually make that two" Yes


Remove item "Take off the nuggets" Yes


Multiple speakers Passenger adds a drink Scheduled


Background noise Engine, wind, speaker distortion, store noise Scheduled and pre-release


Drive-off event Customer leaves before confirmation Pre-release


Crew interjection Staff takes over or corrects the agent Pre-release


Rush-hour load 50+ concurrent ordering calls Scheduled and pre-release


Hamming's public[Lilac Labs customer spotlight](https://hamming.ai/blog/lilac-labs-customer-spotlight) is a useful proof point here: the hard cases were not just "normal orders." They included dietary restrictions, allergies, modifications, and enough automated coverage to replace hours of manual retesting.


## Decide What Belongs In CI


Keep the blocking suite small enough that engineers will tolerate it.


Gate Run when Recommended size Blocks merge?


Menu schema checks Menu parser, prompt, tool schema, or item mapping changes 10-25 cases Yes


Cart mutation tests Prompt, orchestration, tool-call, or state changes 8-20 cases Yes


POS sandbox tests Order creation, price, tax, fulfillment, or payment handoff changes 3-10 fixture orders Yes for critical flows


Phone-path drive-thru tests ASR, telephony, interruption, noise, or provider changes 3-8 calls Pre-release


Load and rush-hour tests Model, provider, queue, or infrastructure changes 50-500 synthetic calls Scheduled or pre-release


Production sampling Continuous monitoring 1-5% of eligible calls No, alert on drift


If a failure can create a wrong order, allergy miss, payment problem, or kitchen workflow issue, keep at least one blocking test. Put long-tail combinations in scheduled coverage.


## Launch Checklist


Before a drive-thru ordering agent sees real traffic, confirm these are true:


- The test suite names the menu version, store fixture, and POS dependency mode.
- Each critical menu item has at least one add, remove, change, and unavailable-item case.
- Modifiers are asserted by canonical ID, not only transcript words.
- Cart state is checked after every mutation, not only at the end.
- Readback is compared against cart state and price.
- POS writes use sandbox, test-store, or explicit live-scoped controls.
- Duplicate order prevention is tested with retries.
- Cleanup runs after pass and fail.
- Allergy and safety workflows are blocking.
- Background-noise and rush-hour tests run before launch.
- Production failures can be promoted into regression tests.


This is more work than a demo script. It is also the line between "the agent sounded right" and "the customer got the food they ordered."


## What This Template Cannot Prove


This template proves that the agent followed the menu, cart, and POS expectations you loaded into the test. It does not prove every store configuration is correct.


Three limitations matter:


Limitation Why it matters Practical response


Menu drift Store menus, prices, item availability, and modifiers change Refresh menu snapshots and compare versions before scheduled runs


Sandbox drift POS sandbox behavior may differ from production routing, auth, or kitchen systems Keep a small pre-release live-scoped check with owner approval


Combination explosion Every modifier and combo permutation cannot block CI Use risk-based blocking tests and scheduled coverage for long-tail combinations


The practical win is narrow and worth it: stop letting a polished transcript hide a wrong cart.


## Drive-Thru Voice Agent Testing FAQ


### How do you test a drive-thru voice agent with menu and cart state?


Create a menu-cart-POS test matrix that maps each spoken order to the menu snapshot, expected item IDs, modifiers, prices, cart mutations, POS write, readback, and cleanup evidence. For a first pass, cover 10-25 menu schema cases and 3-10 fixture orders before adding load. The test should fail when the transcript sounds correct but the cart snapshots or final order state are wrong.


### What should go in a restaurant voice agent order test matrix?


Include menu version, store or franchise configuration, caller phrase, expected item and modifier IDs, expected cart state after each turn, forbidden cart state, expected price, POS dependency mode, final order evidence, and cleanup owner. Hamming's template also saves run ID, menu version, cart snapshots, POS response, and` cleanup_status


` so QA can prove the order was not left behind.


### How do you test menu modifiers and substitutions?


Use fixtures for default modifiers, removed modifiers, added modifiers, substitutions, combo changes, allergies, and unavailable items. Assert the canonical menu IDs and modifier quantities, not just the words in the transcript. Keep a small blocking set for high-risk modifiers, then schedule long-tail menu combinations outside every pull request.


### How do you test cart corrections in a voice ordering agent?


Run multi-turn cases where the caller adds an item, changes size, removes a modifier, replaces one item, accepts or rejects an upsell, and asks for a readback. Check the cart state after every mutation so stale items and duplicate modifiers are caught early. The useful artifact is the before-and-after cart snapshot, not just a final transcript score.


### How do you test POS integration without creating real orders?


Use mocked tools for fast CI, POS sandbox or test stores for integration checks, and narrowly allowlisted live checks only when a production-only path cannot be represented elsewhere. Save the run ID, fixture order ID, POS response, final state, and cleanup result. Any live-scoped check should have an owner, rollback path, and explicit` cleanup_status


` .


### Should drive-thru voice agent tests block CI?


Block CI on high-risk menu, modifier, cart mutation, price, allergy, payment, and POS write cases. Run long-tail menu combinations, load tests, and phone-path checks on a schedule or before release when they are too slow for every pull request. A practical load pass starts with 50-500 synthetic calls, then scales only after cart correctness stays stable.


### What edge cases matter most for QSR voice agents?


Prioritize unavailable items, modifier removal, combo substitutions, allergies, special requests, multiple speakers, background noise, drive-off events, crew interjections, rush-hour latency, payment fallback, duplicate order prevention, and store-specific menu differences. These cases catch cart drift: the caller and transcript look aligned while the durable order state is wrong.


### What evidence should each drive-thru order test save?


Save run ID, menu version, store fixture, transcript, tool trace, cart snapshots after each turn, expected versus actual item and modifier IDs, price checks, POS response, final order state, readback outcome, and cleanup status. The minimum evidence packet should let a reviewer replay the order, inspect the cart mutation, and prove no test order remains open.
