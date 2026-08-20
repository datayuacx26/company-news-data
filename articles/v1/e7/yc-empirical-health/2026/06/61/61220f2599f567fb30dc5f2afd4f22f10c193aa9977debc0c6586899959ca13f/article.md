---
schema_version: "1.0.0"
document_id: "61220f2599f567fb30dc5f2afd4f22f10c193aa9977debc0c6586899959ca13f"
company_key: "yc-empirical-health"
company: "Empirical Health"
source_id: "yc-empirical-health-news-import-485d82c6bcdb"
canonical_url: "https://www.empirical.health/blog/llm-scaling-laws-hold-for-sensor-data/"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-21T18:04:02.816963+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:fbb48ce93b7c30465542fe261760e23f9ab070ff014bac6d89b373059f16680f"
---

# LLM-style scaling laws hold for sensor data

# LLM-style scaling laws hold for sensor data


[Brandon Ballinger](https://www.empirical.health/blog/author/bballinger) ·


Jun 30, 2026


Much of the magic of LLMs comes from the fact loss scales predictably with model size, dataset size, the amount of compute used for training. It’s easy to take scaling laws for granted, but they only published in 2020 and their structure underlies both the economics of AI (if not for scaling laws, frontier labs couldn’t invest nine figures in a training run) and AI’s emergent capabilities (the Phillip Anderson quote, “more is different”, comes to mind).


Do similar scaling laws apply to non-language foundation models, such as[wearable foundation models](https://www.empirical.health/blog/wearable-foundation-models) ? It turns out they do. Is the form exactly the same? It is not, which leads to some interesting questions.


First, let me describe an example of a non-LLM scaling law.


## A non-LLM scaling law


Google’s[Scaling Wearable Foundation Models](https://arxiv.org/abs/2410.13638) was, to my knowledge, the first paper to establish a scaling law for physioloigical sensor data from wearables: *Scaling performance of a wearable foundation model as a function of data size & model size. Source:[Scaling wearable foundation models](https://arxiv.org/abs/2410.13638) .*


Validation loss LL


L


scaled as:


L=aCb+cL = aC^{b} + c


L


=


a


C


b


+


c


where CC


C


is compute, bb


b


is the power-law exponent, and cc


c


is an irreducible floor (more on that later). Across multiple orders of magnitude, loss falls along a nearly straight line on the log-log plot before bending toward the floor cc


c


(the same shape holds when you vary data hours or parameters instead of compute). LSM tested four model sizes (2M, 7M, 110M, and 328M parameters) against data from a few thousand hours up to 40 million. Bigger models and more data both helped on every generative task they measured: random imputation, temporal interpolation, sensor imputation, and forecasting. The payoff on downstream, post-trained tasks was good too. Fine-tuned LSM improved interpolation and forecasting by 16-23% over baselines and lifted activity recognition by 29%.


## Non-LLM scaling laws are similar, but not identical to LLM scaling laws


LLM scaling laws were first established in[Kaplan et al. (2020)](https://arxiv.org/abs/2001.08361) , and then refined in the[2022 Chincilla paper](https://arxiv.org/abs/2203.15556) . In the Chinchilla scaling laws, for a fixed compute budget, you should scale parameters and tokens together, about 20 tokens per parameter. Chinchilla was a 70B model trained on 1.4 trillion tokens, and it beat models several times its size that had been starved of data.


The Chinchilla LLM scaling laws are expressed as:


L(N,P)=L∞+a⋅N−b+c⋅P−dL(N, P) = L_\\infty + a \\cdot N^{-b} + c \\cdot P^{-d}


L


(


N


,


P


)


=


L


∞


​


+


a


⋅


N


−


b


+


c


⋅


P


−


d


Here, L(N,P)L(N, P)


L


(


N


,


P


)


is the validation loss; L∞L_\\infty


L


∞


​


represents the irreducible loss floor; and aa


a


, bb


b


, cc


c


, and dd


d


are fitted constants (exponents and multipliers). One widely cited finding is that, under a fixed compute budget, optimal results are achieved by scaling data and model size together: specifically, the compute-optimal regime is where the number of training tokens NN


N


is proportional to the number of parameters PP


P


(in practice, about 20 tokens per parameter).


One major difference is that LSM’s gains flattened out around 10 million hours of data and roughly 100 million parameters. LLMs have shown no such ceiling at consumer scale. Chinchilla used 1.4 trillion tokens and frontier models have gone well past that, with no flattening yet. (Both scaling laws have an irreducible error term, so this isn’t a difference in functional form but rather an empirical result.)


That’s a potentially interesting opportunity for startups. We trained a JEPA-style wearable foundation model,[JETS](https://www.empirical.health/blog/wearable-foundation-model-jets) , on the same order of magnitude of data as Google and Apple with a four-person team. So whereas starting another LLM foundation model company requires billions of dollars of investment, non-LLM domains might actually be open for smaller startups.


## Some open questions I have


While the power laws rhyme, many of the underlying details are pretty different:


Dimension LLM scaling Wearable sensor scaling


Unit of data Tokens (discrete vocabulary) Hours of continuous, multi-channel signal


Pretraining objective Next-token prediction Masked reconstruction (80% of patches hidden, MSE loss)


Loss Cross-entropy / perplexity Mean squared error on held-out patches


Saturation None yet at trillions of tokens Flattens near 10⁷ hours and ~10⁸ parameters


Compute-optimal recipe ~20 tokens per parameter (Chinchilla) Scale data and model together; total hours dominate


Data supply Finite; the public text pool is being exhausted Renewable; billions of devices generate hours continuously


Economics Oligopoly with $1B+ entry cost Capital light?


This leads to several interesting questions:


- **Data wall.** LLMs are running into a data wall, where the stock of high-quality public text is close to spent and synthetic data is an uneasy substitute. As Ilya Sutskevar put it in his talk on the end of pretraining, “we have but one internet.” Physiological data has the opposite problem. Every watch on every wrist generates roughly 8,760 hours of new signal a year, passively, forever. The binding constraints for sensor models are labeled outcomes, compute, and the messiness of real-world data. If we can find architectures that reduce c to 0, there’s actually a very high ceiling on these laws.
- **Market structure.** T Rowe Price put it, “the economic rationale for AI capital expenditure ultimately rests on scaling laws.” Marginal compute must lead to marginal performance, which is why frontier labs are essentially an oligopoly with an entry price of $1B+. If the scaling laws are different, does this mean the compeitive dynamics are different?
- **Same or different latent space?** We’ve seen interesting implications from aligning vision models into the LLM’s latent space (e.g., CLIP). Will there ultimately be one latent space for all models? Or will we see physiological and other models have their own latent spaces that make meaningful distinctions that are too subtle to fit in language?


## Get your free 30-day heart health guide


Evidence-based steps to optimize your heart health.
