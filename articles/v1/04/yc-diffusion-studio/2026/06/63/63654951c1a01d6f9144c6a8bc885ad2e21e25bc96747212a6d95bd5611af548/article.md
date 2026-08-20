---
schema_version: "1.0.0"
document_id: "63654951c1a01d6f9144c6a8bc885ad2e21e25bc96747212a6d95bd5611af548"
company_key: "yc-diffusion-studio"
company: "Diffusion Studio"
source_id: "yc-diffusion-studio-news-import-a88cd81ba245"
canonical_url: "https://diffusion.studio/changelog/text-to-lottie-skill-update-1/"
published_at: "2026-06-27T00:00:00+00:00"
first_seen_at: "2026-07-21T16:21:29.490873+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:2a09433894267547d3930f12eb1b3a63551f01d987efb5baf88471ff08955933"
---

# Text to Lottie skill update

A big upgrade to the text-to-lottie skill. Better design taste, better motion, fewer random design decisions, and fewer refinements. Simple prompts now produce polished, production ready Lottie animations with Claude Code, Codex, or Cursor.


```text
npx   skills   add   diffusionstudio/lottie
```


[GitHub - diffusionstudio/lottie: Generate production-ready Lottie animations with Claude Code or Codex github.com](https://github.com/diffusionstudio/lottie)


## Better design taste


The skill now produces less generic output. Typography is cleaner, spacing is tighter, and layouts feel more intentional. When no brand is provided, the skill defaults to a premium contrast palette instead of generic gradients and random color choices. The result is output that looks designed, not generated.


## Chapterized animations


Animations can now span multiple sequences with proper transitions between chapters. Each chapter has its own timing, composition, and motion, and the seams between them are handled automatically. This makes it possible to build longer, more structured animations from a single prompt.


## Stronger motion


Motion taste is one of the biggest improvements. Better staggering, better keyframe offsets, better timing, and better coordination between multiple animated properties and objects. The motion feels choreographed, not like everything moves at once. Easing anchors give the skill a vocabulary for describing motion behavior, so results are more consistent across different types of animations.


0:00 / 0:00


## More examples


Simple prompts now produce better results across different use cases. Copy text from a post, ask the skill to animate it, and get a polished Lottie with proper layout and motion.


0:00 / 0:00


## Output comparison


Same one shot prompt. The updated skill produces significantly different results. These two examples show how much the upgrade changed output quality.


Before


After


## What else is new


- Skill routing and parallel agent orchestration for faster generation
- Stronger design QA catches layout and spacing issues before output
- Improved icon animations
- Behavior based easing anchors for more consistent timing
