---
schema_version: "1.0.0"
document_id: "03750c1647c1cea2f1e7512af060bf1d809d10802a29f5fcf01874f652f3585a"
company_key: "yc-vibrant-labs"
company: "Vibrant Labs"
source_id: "yc-vibrant-labs-news-import-1f38412d73f2"
canonical_url: "https://vibrantlabs.com/research/ragas/evaluating-the-evaluators"
published_at: "2025-08-18T00:00:00+00:00"
first_seen_at: "2026-07-27T06:02:47.548224+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:b465d01c4824c67f0017a5c8d218be7b55ee9cf42c30de2f47dccaa376d0c9e4"
---

# Evaluating the Evaluators

If you’re building AI products today, chances are you use LLMs as judges. They grade correctness, faithfulness, or goal success so your team can ship faster without relying on human annotators for every iteration. But here’s the catch: are these LLM-as-judges actually reliable, and can we make them better?


We ran a study to find out. We built a benchmark dataset closer to real-world outputs than academic sets like TriviaQA, and then tested in-context learning strategies that many teams already use. The results were not what we expected: bigger reasoning-focused models improved by as much as **10 points in F1** , smaller distilled models got worse when “optimised,” and Anthropic’s models turned out to be the most stable.


This blog breaks down how we built the benchmark, what we tested, and what these results mean for anyone relying on LLMs as evaluators.


## How We Designed the Benchmark


We began by narrowing the scope to a single but widely used evaluation metric: **answer correctness** . The task is simple to state: given a model response and expert-written grading notes, the judge must decide whether the response sufficiently covers all the required points.


From this definition, we designed the dataset schema:


-


**Question** : a domain-specific query


-


**Grading notes** : expert-written checklist of points that a good answer must contain


-


**Response** : an AI-generated answer


-


**Verdict** : a human-written pass/fail judgment


To build the dataset, we adopted a synthetic data generation pipeline and then applied human review for verification. This ensured both scale and quality. The final dataset contains about 200 samples, each resembling the longer and more nuanced responses found in real-world systems rather than the short trivia-style answers in academic benchmarks.


```text
question   =  'What are the key methods for determining the pre-money valuation of a tech startup before a Series A investment round, and how do they differ?'
grading   notes   =  "DCF method: *future cash flows, requires projections; Comp. analysis: similar co. multiples; VC method: rev x multiple - post-$; *Founder's share matter*; strategic buyers pay more."
response   =  'Determining the pre-money valuation of a tech startup before a Series A investment round is a critical step, as it significantly influences the negotiation process and the ultimate percentage of ownership acquired by new and existing shareholders. Here’s an overview of key valuation methods and their differences:\n\n1. **Discounted Cash Flow (DCF) Method:**\n   - **Overview:** This method focuses on forecasting the startup\'s future cash flows and discounting them back to their present value using a discount rate. This is particularly relevant for startups with predictable cash flows and clear growth projections.\n   - **Future Cash Flows!:** Expect to develop detailed financial projections for at least 3-5 years ahead. These projections should include expected revenues, operating costs, taxes, and capital expenditures. It’s crucial to justify the growth assumptions with market data and industry benchmarks.\n   - **Complexity:** The DCF model can be complex as assumptions about growth rates, discount rates, and terminal values can significantly impact the valuation.\n   - **Action Tip:** Engage with financial analysts or use advanced financial modeling tools to ensure that your cash flow projections and discount rates are robust and as accurate as possible.\n\n2. **Comparable Company Analysis (Comp. Analysis):**\n   - **Overview:** Also known as "market multiples" approach, this method involves comparing the startup to similar companies in the same industry and region that are publicly traded or were recently valued/IPOed.\n   - **Similar Companies Multiples:** Key multiples may include Price-to-Earnings (P/E), Enterprise Value-to-EBITDA (EV/EBITDA), or Enterprise Value-to-Revenue (EV/Revenue). For tech startups, the EV/Revenue multiple is often more appropriate due to typically lower earnings at early stages.\n   - **Action Tip:** Identify a set of 5-10 comparable companies, considering factors like size, growth rate, and market focus, to derive an average multiple to apply to your startup’s metrics.\n\n3. **Venture Capital Method:**\n   - **Overview:** This involves estimating the startup’s exit value, dividing it by the expected ROI for VCs, and working backward to derive the current valuation.\n   - **Revenue x Multiple - Post-Money:** Calculate the expected future revenue, apply an appropriate multiple (based on industry standards), and subtract the desired ROI from the post-money valuation forecasts.\n   - **Founder\'s Share Matter:*:** The dilution of the founder\'s share will depend on the pre- and post-money valuations. Founders need to balance between receiving significant investment and not losing substantial ownership/control.\n   - **Action Tip:** Use this method to understand the expectations and goals of VCs, and assess how your financial and growth plans align with their required rates of return.\n\n4. **Strategic Buyers\' Perspective:**\n   - **Strategic Buyers Pay More:** Companies that see strategic value in your startup might pay higher valuations compared to financial buyers. This occurs because strategic buyers can extract synergies, access new markets, or eliminate competition.\n   - **Action Tip:** If engaging with potential strategic acquirers, highlight your company’s unique assets or market positioning that align with their strategic goals.\n\nIn conclusion, each valuation method offers a unique lens through which the startup’s value can be perceived. Founders should use multiple methods to triangulate a more defensible and comprehensive valuation number. Professional valuation consultation and alignment of investor expectations can ensure a smooth and fair valuation process, avoiding significant founder dilution and maintaining strategic control.'
verdict   =  'pass'
reason   =  'response contains all points mentioned in the grading notes'
```


```text
verdict   =  'pass'
reason   =  'response contains all points mentioned in the grading notes'
```


Example sample in EvalsBench Dataset


With the dataset ready, we evaluated a range of in-context learning strategies that AI teams often experiment with in practice. These included


-


**Vanilla** : baseline LLM as judge with only simple prompt defined by the user.


-


**Fixed few-shot** : always show the same examples


-


**Random few-shot** : randomly chose examples from a list of expert annotated samples.


-


**Dynamic few-shot** : retrieve closest examples to current task from a list of expert annotated samples.


-


**Automated prompt optimisation** : a coding agent optimising prompt based on expert annotated samples.


Baseline LLM as judge performance compared to various in context learning approaches


**What We Found**


**Provider**


**Model**


**Vanilla**


**Fixed few shot**


**Random few shot**


**Dynamic Few shot**


**Automatic prompt optimisation**


**Anthropic**


claude-3.5-haiku


66.95


67.8


66.95


66.1


**74.42**


**Anthropic**


claude-4-opus


86.02


94.34


91.14


**95.18**


95.12


**Anthropic**


claude-4-sonnet


87.43


92.12


90.12


90.57


**93.57**


**Google**


gemini-2.5-flash


87.91


87.78


90.4


89.89


**98.16**


**Google**


gemini-2.5-flash-lite


**89.53**


80.77


76.83


79.49


56.64


**Google**


gemini-2.5-pro


86.19


92.4


94.05


92.94


**95.71**


**OpenAI**


gpt-4o


80.63


82.35


80.63


80.0


**92.86**


**OpenAI**


gpt-4o-mini


**84.49**


79.79


75.86


76.65


51.38


**OpenAI**


o3-mini


87.43


95.76


93.02


91.43


**95.81**


###### *F1 score for various models and in context learning strategies. Higher is better.*


When we put the models through these strategies, the results surprised us. There was no single strategy that worked across the board. Instead, the effectiveness of each approach depended heavily on the underlying model.


Here are the most important lessons:


-


**No silver bullet** . No in-context learning method consistently improved performance across all models. Teams should expect to tune their strategy depending on the model they use.


-


**Bigger “thinking” models benefited the most** . Claude Opus, Gemini Pro, and o3-mini saw gains of up to **+10 points in F1 score** when paired with the right optimisation strategy.


-


**Smaller, distilled models behaved unpredictably** . Models like GPT-4o-mini and Gemini Flash-Lite sometimes performed worse after optimisation. This shows that strategies designed for larger models do not transfer well to smaller ones.


-


**Anthropic models were the most stable** . Their performance improved in ways that aligned with intuition, making them easier to work with.


In short, the experiment showed that while in-context learning can make LLM-as-judges more effective, the “how” is very model dependent. The code and data is available[here](https://github.com/explodinggradients/EvalsBench) .


### Recommendations for Practitioners


After running this experiment, here are our recommendations for AI engineers and PMs working with LLM-as-judges:


1.


**Validate on your own data, not just benchmarks:** Academic datasets often don’t reflect real-world complexity. Always test LLM-as-judges on your production-like data to understand how they behave in practice.


1.


**Start with review and alignment:** LLM-as-judges rarely perform their best out of the box. Always run at least one iteration where human experts review and correct the judge’s outputs to align it with your domain.


1.


**Feed feedback back into the judge:** Don’t let review data sit idle. Incorporate it as few-shot examples or use it for prompt optimization so the judge continuously improves.


1.


**Favor larger models for critical evaluation:** Use reasoning-focused models for offline evaluation and CI/CD pipelines where accuracy matters most, while smaller models can be used for faster iteration.


1.


**Prioritize reasoning in verdicts:** Structure prompts so judges provide not just “pass/fail” labels but reasoning. This makes it easier for teams to debug, trust, and refine the evaluation process.


### Conclusion and Future work


For teams using LLM-as-judges today, the main takeaway is simple: **don’t assume your evaluation method is universally reliable** . The effectiveness of prompts and few-shot strategies depends heavily on the model you choose.


Looking ahead, we want to extend this work beyond correctness and into other key metrics like faithfulness and goal success. We also plan to scale the dataset to cover more domains and explore advanced optimisation methods like DSPy and PhaseEvo, which may help smaller models behave more like larger ones.


If you’re looking to implement evaluation in your organisation, we do ragas office hours to help community and enterprises to evaluate and improve AI systems. Schedule[here](https://cal.com/team/ragas/office-hours) or write to us at founders@explodinggradients.com
