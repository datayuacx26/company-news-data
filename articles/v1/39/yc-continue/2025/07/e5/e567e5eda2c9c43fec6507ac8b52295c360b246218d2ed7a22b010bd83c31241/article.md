---
schema_version: "1.0.0"
document_id: "e567e5eda2c9c43fec6507ac8b52295c360b246218d2ed7a22b010bd83c31241"
company_key: "yc-continue"
company: "Continue"
source_id: "yc-continue-rss-8ad3fe8c275c"
canonical_url: "https://blog.continue.dev/intervention-rates-are-the-new-build-times"
published_at: "2025-07-21T16:30:53+00:00"
first_seen_at: "2026-07-24T23:23:23.996128+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:c8c50e3c74842419c639f3004d2597362b1baaad9fced147a6cdc55601af2dce"
---

# Intervention Rates Are the New Build Times

Remember when CI/CD teams obsessed over build times? That single metric captured everything: code quality, infrastructure efficiency, process bottlenecks. Teams that measured and improved build times systematically ended up transforming how they shipped software.


My career started with fixing a soul crushing build time.We were working on an embedded distributed system based on a[StrongARM](https://en.wikipedia.org/wiki/StrongARM) chip, with millions of lines of code. Building natively on the embedded platform was painful—a full build took a literal day. Cross compilers weren't readily available in the package repositories back then.


One day I kicked off a build, realized hours later I'd made a minor mistake, and just snapped. I went out and bought a GCC book, learned to build a cross-compiling toolchain for our project, and transformed our build time from a day to 10 minutes. Later I added[distcc](https://www.distcc.org/) and[ccache](https://ccache.dev/) , making it even faster. The systematic approach to improving our build infrastructure was life changing for the team.


We're at a similar moment with AI-assisted development. And in a[recent thread](https://x.com/chipro/status/1945527700808184115)[Chip Huyen](https://huyenchip.com/) identified the metric that matters: intervention rates.


Her data tells a compelling story. After analyzing 1,746 commands with Claude Code (Anthropic's command-line AI tool), she found herself interrupting or correcting the AI 24.5% of the time. But instead of just accepting this as "how AI works," she's treating it like those early CI teams treated build times—as a key metric to measure, analyze, and systematically improve.


## The Data Tells the Story


Just as build times became the North Star metric for CI/CD, intervention rates capture the effectiveness of your entire AI-assisted workflow. When you measure how often you need to interrupt your AI assistant, you're measuring prompt quality, code organization, task breakdown, and tool selection all at once.


Chip's analysis of 1,746 commands revealed clear patterns. 35% of errors were "Content Not Found"—the AI couldn't locate the files or functions it needed.


This wasn't just an interesting observation—it was actionable intelligence. She restructured her codebase to improve AI discoverability. Result: average steps per task dropped from 8 to 7, a 12.5% efficiency gain from addressing one systematic bottleneck.


Her intervention rate varies between 15% and 60% depending on the task:


That variance reveals something important: AI effectiveness depends heavily on context and workflow design. Without measurement, these patterns remain invisible. But with systematic tracking, you can identify what works and replicate it.


This is exactly how early CI teams approached build optimization—measure, identify bottlenecks, fix systematically, repeat. Most developers are still in the "hoping it works" phase with AI tools, but some folks like Chip are starting to apply this same methodical approach to their AI workflows.


## Getting Started with Your Own Measurement


You don't need sophisticated analytics to begin. Start with the fundamentals:


**Identify your baseline** : Track when you correct or redirect AI tools. What's your intervention rate across different task types?


**Find your biggest bottleneck** : Chip's "Content Not Found" errors pointed to code organization issues. What causes most AI failures in your workflow?


**Make systematic improvements** : Look for changes that address root causes rather than symptoms.


**Measure the impact** : Did your intervention rate improve? Are tasks completing in fewer steps?


The goal isn't perfect automation immediately—it's building the measurement habits that enable continuous improvement.


## Try It Yourself


Want to explore your own AI development patterns? Chip built Sniffly, an open-source tool for analyzing Claude Code logs. If you're using other AI coding tools, Continue offers broader development data analysis capabilities.


Install Continue and start tracking your intervention rates. You might be surprised by what the data reveals about your AI-assisted workflow patterns and bottlenecks.


## The Real Opportunity


The most valuable insight from Chip's work isn't the specific **24.5%** number or even her particular improvements. It's the mindset shift: treating AI tools as measurable infrastructure rather than unpredictable magic.


As AI becomes central to how we develop software, this systematic approach will separate teams that build lasting advantages from those that remain stuck hoping their tools work better tomorrow.


The teams that start measuring intervention rates now will likely have advantages as AI tools become more central to development workflows. The question isn't whether systematic measurement will become more common—it's whether you'll start building these habits early or play catch-up later.


---


## References


This analysis is based on Chip Huyen's detailed breakdown of her AI development workflow using Sniffly, her open-source tool for analyzing Claude Code logs:


- [Full Twitter thread with original data and analysis](https://x.com/chipro/status/1945527700808184115)
- [Sniffly tool on GitHub](https://github.com/chiphuyen/sniffly)


All data visualizations and insights are credited to Chip Huyen's original analysis.
