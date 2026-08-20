---
schema_version: "1.0.0"
document_id: "59e7e30df31c48bcdb208705ed5c359ba4dce35901a7c2752857be03b8d39436"
company_key: "yc-fly-io"
company: "Fly.io"
source_id: "yc-fly-io-news-import-54d37e81cc45"
canonical_url: "https://fly.io/blog/games-as-model-eval/"
published_at: "2025-08-11T00:00:00+00:00"
first_seen_at: "2026-07-21T20:38:55.214316+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:c26c0bbafd581a0312546cb305f6c134be0d8515ce2b235e371f31c7c877e1af"
---

# Games as Model Eval: 1-Click Deploy AI Town on Fly.io

Author Name Daniel Botha Image by


[Annie Ruygt](https://annieruygtillustration.com/)


Recently, I suggested that[The Future Isn’t Model Agnostic](https://fly.io/blog/the-future-isn-t-model-agnostic/) , that it’s better to pick one model that works for your project and build around it, rather than engineering for model flexibility. If you buy that, you also have to acknowledge how important comprehensive model evaluation becomes.


Benchmarks tell us almost nothing about how a model will actually behave in the wild, especially with long contexts, or when trusted to deliver the tone and feel that defines the UX we’re shooting for. Even the best evaluation pipelines usually end in subjective, side-by-side output comparisons. Not especially rigorous, and more importantly, boring af.


Can we gamify model evaluation? Oh yes. And not just because we get to have some fun for once. Google backed me up this week when it announced the[Kaggle Game Arena](https://blog.google/technology/ai/kaggle-game-arena/) . A public platform where we can watch AI models duke it out in a variety of classic games. Quoting Google; “Current AI benchmarks are struggling to keep pace with modern models… it can be hard to know if models trained on internet data are actually solving problems or just remembering answers they’ve already seen.”


When models boss reading comprehension tests, or ace math problems, we pay attention. But when they fail to navigate a simple conversation with a virtual character or completely botch a strategic decision in a game environment, we tell ourselves we’re not building a game anyway and develop strategic short-term memory loss. Just like I’ve told my mom a thousand times, games are great at testing brains, and it’s time we take this seriously when it comes to model evaluation.


## Why Games Don’t Lie


Games provide what benchmarks can’t, “a clear, unambiguous signal of success.” They give us observable behavior in dynamic environments, the kind that would be extremely difficult (and tedious) to simulate with prompt engineering alone.


Games force models to demonstrate the skills we actually care about; strategic reasoning, long-term planning, and dynamic adaptation in interactions with an opponent or a collaborator.


## Pixel Art Meets Effective Model Evaluation - AI Town on Fly.io


AI Town is a brilliant project by[a16z-infra](https://github.com/a16z-infra) , based on the the mind-bending paper,[Generative Agents: Interactive Simulacra of Human Behavior](https://arxiv.org/pdf/2304.03442) . It’s a beautifully rendered little town in which tiny people with AI brains and engineered personalities go about their lives, interacting with each other and their environment. Characters need to remember past conversations, maintain relationships, react dynamically to new situations, and stay in character while doing it all.


I challenge you to find a more entertaining way of evaluating conversational models.


I’ve[forked the project](https://github.com/fly-apps/ai-town_on_fly.io) to make it absurdly easy to spin up your own AI Town on Fly Machines. You’ve got a single deploy script that will set everything up for you and some built-in cost and performance optimizations, with our handy scale to zero functionality as standard (so you only pay for the time spent running it). This makes it easy to share with your team, your friends and your mom.


In it’s current state, the fork makes it as easy as possible to test any OpenAI-compatible service, any model on Together.ai and even custom embedding models. Simply set the relevant API key in your secrets.


Games like AI Town give us a window into how models actually think, adapt, and behave beyond the context of our prompts. You move past performance metrics and begin to understand a model’s personality, quirks, strengths, and weaknesses; all factors that ultimately shape your project’s UX.


Next post ↑[Trust Calibration for AI Software Builders](https://fly.io/blog/trust-calibration-for-ai-software-builders/) Previous post ↓[The Future Isn't Model Agnostic](https://fly.io/blog/the-future-isn-t-model-agnostic/)
