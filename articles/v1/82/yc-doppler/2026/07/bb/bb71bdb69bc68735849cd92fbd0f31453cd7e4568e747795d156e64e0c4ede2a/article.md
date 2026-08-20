---
schema_version: "1.0.0"
document_id: "bb71bdb69bc68735849cd92fbd0f31453cd7e4568e747795d156e64e0c4ede2a"
company_key: "yc-doppler"
company: "Doppler"
source_id: "yc-doppler-news-import-86fd2de2d678"
canonical_url: "https://www.doppler.com/blog/compare-secrets-across-environments"
published_at: null
first_seen_at: "2026-07-25T01:53:43.668103+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:4eadcf56858739a8506fdb331eca51f229b2c74d77c08ccc3c895ace6797c8cb"
---

# Building a better way to compare secrets

If you have ever been in the situation where you need to see if your secrets are the same in staging as they are in production for several configs, but had to manually compare them side by side in separate tabs, you probably found yourself in a frustrating situation. Doppler’s compare feature was supposed to solve that. It was built for a specific use case, and over time, the gaps became impossible to ignore. The original design of Compare only scratched the surface of what developers actually needed.


It was time to give developers the compare they deserve. This is how and why we rebuilt Compare from the ground up.


## What we shipped


The redesigned compare feature addresses all of the original pain points and more.


### Compare unlimited configs at once, even across projects


The old view capped how many configs you could line up, which meant breaking a real audit into multiple passes. Now you can select dev, staging, prod, and every branch, across different projects, and see them all in one view.


### Group by value


Secrets that share the same value across configs are visually grouped, making it immediately obvious where configs are in sync and where they’ve drifted.


### Compare everything to a reference config


Pick one config as the reference point and see how everything else stacks up against it. Combined with grouping, this makes large audits much easier to read and diagnose.


### Updating secrets from Compare


You can now create a change request directly from Compare. Simply select the secrets you want to include in the change request, and a new form will open up with those secrets loaded and ready for updates. No more exiting Compare and hunting down each secret across multiple screens.


### Easier to find


Compare is now easier to find as it lives in the primary navigation. You can still get to Compare by hovering directly over a secret to start your search there or by clicking the overflow menu on a specific config.


## How to try it


Compare is available now in the[Doppler dashboard](https://dashboard.doppler.com/)


under the primary navigation.


First, select **Compare** from the primary navigation to open the view.


Next, choose the configs you want to compare. Pick as many as you need, across projects, and compare pulls them all into a single view.


Finally, tune the display settings to answer the question you came in with. **Group by value** clusters secrets that share the same value, so sync and drift are obvious at a glance. **Show diffs to a config** measures every other config against a reference you choose. And **show dismissed and excluded secrets** brings back anything you’ve filtered out when you need the full picture.


## The problem: A feature that worked, but didn’t work *well*


Compare has been in Doppler for a while, but we knew it needed improvements. I took a deeper look at the usage data and customer feedback we’ve collected and found that this feature needed attention and a better user experience. When deciphering why there was a lack in usage and what the pain points were for those who provided feedback, I found that there were four patterns that kept coming up.


### 1. Secret differences were hard to scan


Values were presented in a way that required you to actually read each row. There was no visual hierarchy that let your eyes go straight to what was different about each secret, leading to a slower analysis.


### 2. Limited comparison capacity


The old UI restricted the number of configs a user can compare simultaneously, forcing them to conduct multiple comparisons for larger audits and reviews. This created inefficiency, especially for users managing large amounts of configs. There was no way to get the full picture in a single view.


### 3. Comparing a secret vs. a config was a forced choice


The old compare flow asked you upfront: Are you comparing a secret or a config? That sounds reasonable until you realize most people don’t know what they need until they see it.


### 4. Inefficient secret updates post-comparison


After identifying the secrets that the user would like to update, they were required to exit the compare view and navigate through multiple screens to hunt down the secrets they needed to update. The absence of shortcuts and suggested paths to edit secrets added unnecessary steps, slowing down their workflows.


## Ideation: The good, the bad, and the scrapped


Good design comes from generating lots of options, even the ones that seem impossible at first, and getting multiple minds together to jam on those ideas. So, I facilitated a how-might-we workshop with our engineering team to brainstorm solutions to our users’ pain points with the current compare experience. Together, we identified a few ideas we believed would address most of those pain points.


The biggest challenge in designing our dream state (breaking down limitations and supporting new features like highlighting differences, showing exact matches, and comparing multiple configs) was organizing the information in a digestible, easily scannable way. A side-by-side diff view looked appealing, but it broke down at scale. Once you’re comparing four or five configs, a column view doesn’t work anymore.


The ideas that survived were the ones that put power and control in users’ hands. We wanted to support as many use cases as possible while still providing structure and giving our users the tools they needed to answer the questions they came to Compare for.


## Research: Gathering feedback from real users


Before committing to a defined direction, I sent a video walkthrough of the new flows to a handful of enterprise customers who we knew managed large, complex setups and had previously shared feedback about Compare. I wanted to ensure that this new design actually solved their pain points and that we were heading in the right direction.


One user called out, “After importing existing environments into Doppler, we’ve seen some major drift in missing values alone. This will be very helpful in triaging and investigating.”


## Building it: Design and engineering collaborating in real time


The primary driver that sped up our ideation cycles was the constant collaboration between design and engineering. Rather than presenting a finished spec, we worked iteratively. Mike (engineering) demoed early implementations, raised questions about edge cases and performance, and suggested improvements that would enhance the user experience and overall performance of compare.


This kind of collaboration has proven to produce better results than the traditional waterfall approach of passing designs over to engineering without giving all of the background context. Additionally, engineers (especially the ones at Doppler) have great UX instincts paired with knowing what is technically straightforward versus what would be a complex change.


## What’s next


Research always opens up more than it closes, and based on customer feedback, we will be exploring ways to solve a few more pain points, such as improving how we handle resolved versus raw references in the Compare view.
