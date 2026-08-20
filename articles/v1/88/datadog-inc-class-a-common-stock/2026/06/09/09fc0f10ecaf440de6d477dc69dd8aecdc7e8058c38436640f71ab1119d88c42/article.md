---
schema_version: "1.0.0"
document_id: "09fc0f10ecaf440de6d477dc69dd8aecdc7e8058c38436640f71ab1119d88c42"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/how-to-migrate-feature-flags/"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:621e1bfcc86d53f7543d5f27b76c2f8b1dfa805cb04be0ad1e5e07116a4a2662"
---

# How to migrate feature flags without breaking production

Anthony Rindone


Senior Product Manager


Ryan Lucht


Senior Technical Advocate


Natasha Silva


Technical Content Writer


Feature flag migrations have a reputation problem. Ask anybody who’s been through one before and you’ll hear the stories, usually from someone still a little frustrated about a bad cutover, with a postmortem or two to show for it.


The reputation is mostly undeserved. While the risks are real, they’re well understood and easily controlled. Getting a migration right doesn’t require a big coordinated effort. It requires knowing what can go wrong and designing around it from the start.


## Why feature flag migrations stall


In a large organization, a legacy SDK can span thousands of call sites across dozens of microservices. Replacing something like` oldClient.variation` with` newClient.evaluate` is repetitive work, but with agentic developer tooling, it can be done in days rather than weeks.


The harder problems are about risk, not effort. Production safety is the real concern and it comes down to three specific technical challenges:


-


**Check logic parity between systems** . Feature flags[aren’t just on/off switches](https://martinfowler.com/articles/feature-toggles.html) . They encode complex evaluation logic, such as percentage rollouts, user segments, prerequisites, and dynamic payloads. Two systems rarely interpret the same rule identically.


-


**Configuration synchronization** . During the migration window, product managers and engineers might keep modifying flags in the legacy system to ship features, run tests, and manage incidents. If rules are imported on Day 1 but the cutover happens two months later, the new platform is already out of sync and any validation done against the original state is now stale. Shortening the migration window limits exposure. Freezing configuration immediately before cutover eliminates it.


-


**Confirm cutover safety** . Feature flags frequently sit in critical paths. A flag gating a payment flow or an infrastructure failover isn’t just affecting one user when it misfires. The entire service is exposed. Teams need confirmation that the new system produces identical evaluations before committing to a cutover.


Each of these challenges also has a cross-functional dimension: product managers need to know when to stop creating flags in the legacy system, and engineering teams need to communicate cutover timelines clearly so that no one is caught off guard. A migration freeze on a shared calendar is a small coordination habit that can prevent a large class of problems.


Logic parity, configuration synchronization, and cutover safety don’t need to be solved all at once. The right approach is to make the migration incremental by design and to accept something that sounds counterintuitive at first.


## Is it really okay to run two feature flag systems at once?


Running two feature flag systems simultaneously is safe and already common in organizations that use separate systems for platform and product use cases. It can be mildly annoying in practice, but it’s not dangerous. The benefit to doing so is that it can allow migrations to take place naturally over a few weeks, instead of requiring a high-coordination high-risk sudden cutover.


The nightmare scenario that engineers might imagine when considering having two parallel feature flag systems is a critical incident in production where teams cannot find the right flag to disable. With[a reasonable on-call setup](https://www.datadoghq.com/blog/on-call-paging/#establish-on-call-roles-and-responsibilities) , the engineer being paged owns that feature and knows where the flag lives.


Once you accept that two systems in parallel are workable, the migration path becomes less daunting.


## How to structure a feature flag migration


Start with an audit. Before writing any code, categorize your existing flags. Most fall into three categories:


-


**Zombie flags** have reached 100% rollout for months and should already be removed from the codebase. Start by cleaning these up. Don’t migrate technical debt.


-


**Short-lived flags** govern experiments and temporary rollouts. They run for a few weeks or months before being cleaned up.


-


**Long-lived flags** don’t have a clear expiry date. They are typically associated with kill switches, infrastructure configuration, or permanent feature gates. These require deliberate migration planning.


From this point, the strategy has three phases:


### Redirect all new flags to the new system


Establish one rule immediately: all new flags must be created in the new system. Don’t migrate anything yet. Just stop adding to the old system. Redirecting new flags costs almost nothing and starts moving active flag logic toward the new platform.


### Phase out short-lived flags


As short-lived flags complete their rollouts and get cleaned up, they disappear from the legacy system on their own. To accelerate this, enforce a deprecation policy: remove flags within a sprint or two of their reaching 100% rollout. In larger organizations, a hard 6–8 week rule is easier to enforce. With this in place, most of the flag inventory will migrate itself within a quarter.


### Plan for the cutover


By this point there’s a much smaller list, mainly consisting of the long-lived flags that need careful handling. For each one, ask whether *this is still needed.* More than expected will turn out to be retirable. For those that remain, the focus is verifying correctness before cutting over.


## Verify feature flag logic parity before cutover


Run both systems against real traffic and confirm they produce identical evaluations before committing to a cutover. There are two approaches, depending on the stack and how much set up makes sense.


### Use a wrapper function


A simple wrapper function covers most migration scenarios. Evaluate both systems for every flag check, log any discrepancies, and return the legacy system’s answer as authoritative:


```text
1   def     evaluate  (  flag_name  ,   context  ):    2          legacy_value = legacy_client.evaluate(flag_name, context)    3          new_value = new_client.evaluate(flag_name, context)    4
5         if   legacy_value != new_value:    6              metrics.increment(    7                 "feature_flag.migration.mismatch"  ,    8                 tags  =[  f  "flag:  {  flag_name  }  "  ]    9              )    10
11         return   legacy_value
```


Run the new system’s evaluation asynchronously to avoid adding synchronous latency. Keep mismatch logging asynchronous and non-blocking.


### Set up shadow mode with OpenFeature


[OpenFeature](https://openfeature.dev/docs/reference/intro) is a[Cloud Native Computing Foundation](https://www.cncf.io/) (CNCF) incubating project that provides a vendor-agnostic API for feature flagging. The specification offers the same shadow-mode pattern as a first-class abstraction.


One caveat:[OpenFeature’s multi-provider support](https://openfeature.dev/docs/reference/sdks/sdk-compatibility/) covers only Node, Web, and Kotlin SDKs. If your stack sits outside those runtimes, or you’re migrating from an in-house system without an OpenFeature provider, the wrapper approach above is the more practical choice.


With[OpenFeature’s ComparisonStrategy](https://openfeature.dev/blog/openfeature-multi-provider-release/#comparison-strategy) , teams can run shadow mode evaluation at the SDK level:


```text
1   import   {   OpenFeature   }   from     '@openfeature/server-sdk'  ;    2   import   {   MultiProviderPlugin  ,   ComparisonStrategy   }   from     '@openfeature/multi-provider'  ;    3
4   OpenFeature  .  setProvider  (    5       new     MultiProviderPlugin  (    6          [  legacyProvider  ,   newProvider  ],    7         new     ComparisonStrategy  ({    8           onMismatch  :   (  flagKey  ,   legacyValue  ,   newValue  )   =>   {    9             metrics  .  increment  (  'feature_flag.mismatch'  , {   flag:     flagKey   });    10            }    11          })    12        )    13   );
```


The` ComparisonStrategy` evaluates both providers on every flag check, returns the legacy value to the application, and fires a mismatch handler when results diverge, with zero user impact during the validation window.


Once parity is confirmed for a given flag or service,[FirstMatchStrategy](https://openfeature.dev/blog/openfeature-multi-provider-release/#first-match-strategy) enables the actual cutover. The SDK evaluates the new provider first. If the flag isn’t found there, it falls back to the legacy system:


```text
1   OpenFeature  .  setProvider  (    2       new     MultiProviderPlugin  (    3          [  newProvider  ,   legacyProvider  ],    4         new     FirstMatchStrategy  ()    5        )    6   );
```


Using` FirstMatchStrategy` allows teams to migrate flags individually, on their own schedule, without touching application code again.


Both approaches produce the same result: a stream of mismatch events tagged by flag name and service. But that signal is only useful if someone is watching it and acting on what they see. Route mismatch metrics to a dashboard where teams can track parity trends and decide when to cut over.


## Set up a feature flag migration dashboard


Route the mismatch metrics into a dashboard broken down by flag name and service, and track at least three things:


-


total evaluations versus mismatches


-


mismatch rate trend over time


-


first-seen and last-seen timestamps per discrepancy


The mismatch rate trend line matters most. It should be declining toward zero and holding there, not fluctuating. A flag that’s been zero for several days across real traffic volume is ready to cut over.


With[Datadog Feature Flags](https://www.datadoghq.com/product/feature-flags/) , engineers can send wrapper metrics directly into Datadog. Teams can correlate their feature flags with metrics, logs, and traces in a single view. If a mismatch is causing silent breakage in production, they can see it in context without switching tools.


Before cutting over any specific flag or service, briefly freeze its configuration in the legacy system. The state validated in shadow mode should match exactly what exists at cutover time. A freeze of 24–48 hours is sufficient for high-traffic flags to accumulate statistical confidence. For lower-traffic flags, extend the window accordingly.


## Migrate safely with Datadog Feature Flags


While feature flag migrations can carry real risk, structuring the process in discrete, verifiable steps makes each one addressable. Flag evaluation discrepancies appear in Datadog alongside the metrics, logs, and traces. With the migration complete, flag deployments are observable in Datadog within the same context as your services and infrastructure. Read our documentation on[setting up Datadog Feature Flags](https://docs.datadoghq.com/getting_started/feature_flags/) to configure tracking in your environment.


To start monitoring your feature flag migrations,sign up for a free 14-day trial .


-
-
-
