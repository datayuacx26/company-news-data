---
schema_version: "1.0.0"
document_id: "5010e3502d4d1802e0ff8a44dc09828ef227db9ecd4fab353b331c8eda74f312"
company_key: "yc-exa"
company: "Exa"
source_id: "yc-exa-news-import-e15579f9a79b"
canonical_url: "https://exa.ai/blog/rl-search-outcomes"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-25T04:05:34.244049+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:efd7114d567f8f9379fe78058db1427332d48c1f4c5a0a9c466895fe98d7bf5c"
---

# How Search Quality Shapes RL Outcomes

## # Introduction


Search agents have demonstrated dramatic improvements in performance when trained with reinforcement learning (RL). However, existing work often uses Google as the backend. An understudied question is how training on different web search tools impacts RL outcomes.


We compare Exa with a Google baselinea a We use SERP as a proxy for Google results, as it is widely used for post-training language models today.1 ,2 ,3


and find that agents trained with Exa as the search backend during RL reach higher performance with less training compute.


Prior research in training search agents with reinforcement learning has often kept the retrieval method fixed.


1. Early research taught models to retrieve over offline corpora4 ,5 ,6 , learning to reason over multi-hop questions.
2. Later work added live web search to the action space7 ,8 , but still fixed the retriever, usually to a SERP-based backend.


Issuing queries across the live web during training can be prohibitively expensive and unpredictable9 , so ablations of the retrieval method for RL usually remain limited to offline indexes10 ,11 . Because Exa has its own search engine and web index, we can study this interaction at scale.


## # Experimental Design


### # Model and Action Space


To isolate the impact of live search during RL, we train two models, one with Exa and one with SERP, and hold all other variables constant.


We train Qwen3-4B-Instruct-250712 with LoRA adapters using[Tinker](https://thinkingmachines.ai/tinker/) . Prior work has found that LoRA can match full fine-tuning performance, even with small ranks13 .


Both agents use the same system prompt, adapted from Search-R14 ,14 , to avoid prompt-induced behavioral differences. Each search tool call returns 5 live web results, each truncated to a snippet of maximum 2,000 characters. The agent is not aware of the search variant it has access to. This tool configuration was chosen with model context limits in mind, as we initially observed long multi-hop trajectories quickly saturating the context window. Though other harness optimizations are possible, here we focus primarily on the comparison of search providers.


Question


“At the end of what year did the tribes from the place where the Rossen culture is located invade the Roman Empire?”


Expected Answer


406


Exa Agent Rollout


From the search results:
The crossing of the Rhine by a mixed group of barbarians (including Vandals, Alans, and Suebi) is traditionally considered to have occurred on 31 December 406


, initiating a wave of destruction in northern Gaul.


Correct Answer


406


Query


Response


I need to find out at the end of what year the tribes from the place where the Rossen culture is located invaded the Roman Empire. Step 1: Identify the location of the Rossen culture. Step 2: Determine which tribes are from that location. Step 3: Find out when those tribes invaded the Roman Empire, specifically the end year of that invasion. First, I need to identify the location of the Rossen culture.


Tool call


{"query": "Rossen culture location"}


Results


1.


[https://en.wikipedia.org/wiki/R%C3%B6ssen_culture](https://en.wikipedia.org/wiki/R%C3%B6ssen_culture)


2.


[https://en.wikipedia.org/wiki/Rossen_culture](https://en.wikipedia.org/wiki/Rossen_culture)


3.


[https://dbpedia.org/page/R%C3%B6ssen_culture](https://dbpedia.org/page/R%C3%B6ssen_culture)


4.


[https://www.historyatlas.com/groups/rossen-culture/](https://www.historyatlas.com/groups/rossen-culture/)


5.


[https://www.archaeologs.com/w/rossen-culture/en](https://www.archaeologs.com/w/rossen-culture/en)


Response


I need to identify the location of the Rossen culture to determine which tribes are from that location and when those tribes invaded the Roman Empire. From the search results: - The Rossen culture (also spelled Rössen culture) is a Central European culture of the middle Neolithic (4,600–4,300 BC). - It is named after the necropolis of Rössen (part of Leuna, in the Saalekreis district, Saxony-Anhalt). - The Rössen culture has been identified in 11 of the 16 states of Germany (it is only absent from the Northern part of the North German Plain), but also in the southeast Low Countries, northeast France, northern Switzerland and a small part of Austria. - The geographical range is North-Central Europe corresponding roughly to modern-day Germany, Austria and the Low Countries. So the Rossen culture is located in Central Europe, primarily in Germany, Austria, and the Low Countries. Now I need to find out which tribes from this location invaded the Roman Empire and at what year, specifically the end year of that invasion. Let me search for information about tribes from Central Europe (specifically Germany, Austria, or Low Countries) that invaded the Roman Empire and the year of that invasion.


Tool call


{"query": "tribes from Central Europe Germany Austria Low Countries invaded Roman Empire year"}


Results


1.


[https://en.wikipedia.org/wiki/Chronology_of_warfare_between_the_Romans_and_Germanic_peoples](https://en.wikipedia.org/wiki/Chronology_of_warfare_between_the_Romans_and_Germanic_peoples)


2.


[https://en.wikipedia.org/wiki/Germanic_tribes](https://en.wikipedia.org/wiki/Germanic_tribes)


3.


[https://en.wikipedia.org/wiki/Quadi](https://en.wikipedia.org/wiki/Quadi)


4.


[http://www.britannica.com/topic/Marcomanni](http://www.britannica.com/topic/Marcomanni)


5.


[https://en.wikipedia.org/wiki/Crossing_of_the_Rhine](https://en.wikipedia.org/wiki/Crossing_of_the_Rhine)


Response


I need to determine the end year of the invasion by tribes from the Rossen culture location (Central Europe, primarily Germany, Austria, and the Low Countries) into the Roman Empire. From the search results: - The Rossen culture is a Central European Neolithic culture (4,600–4,300 BC), so it is ancient and not directly related to invasions of the Roman Empire. - The Germanic peoples (tribes in Northern Europe during Classical antiquity and Early Middle Ages) are mentioned as having conflicts with the Romans. - The Marcomanni, a Germanic tribe, invaded Roman territory around 167 AD, penetrating into Italy. - The crossing of the Rhine by a mixed group of barbarians (including Vandals, Alans, and Suebi) is traditionally considered to have occurred on 31 December 406, initiating a wave of destruction in northern Gaul. The Rossen culture is from the Neolithic period (4,600–4,300 BC), which is very ancient and predates the Roman Empire. Therefore, tribes from the Rossen culture cannot have invaded the Roman Empire. However, the question might be referring to tribes from Central Europe (modern-day Germany, Austria, Low Countries) during the Roman era, such as Germanic tribes like the Marcomanni or Suebi. The Marcomanni invaded Roman territory around 167 AD, but the question asks for the end of the year of the invasion. The crossing of the Rhine in 406 AD is a significant event in the decline of the Roman Empire, initiated by Germanic tribes. Given that the Rossen culture is from the Neolithic period and cannot be the source of invasions of the Roman Empire, it is likely there is a misunderstanding or mix-up in the query. But if we interpret "tribes from the place where the Rossen culture is located" as Central Europe (Germany, Austria, Low Countries) during the Roman era, the most relevant invasion event is the crossing of the Rhine in 406 AD. The end year of that invasion (crossing of the Rhine) is 406 AD. Answer: 406


SERP Agent Rollout


The Goths first invaded the Roman Empire in 376 AD, and they defeated the Romans at the Battle of Adrianople in 378 AD.
The final answer to the question is 378


, as this is when the major battle and defeat occurred, marking the end of the initial phase of the invasion.


Incorrect Answer


378


Query


Response


To answer the question about the year when the tribes from the place where the Rossen culture is located invaded the Roman Empire, I need to follow these steps: 1. Identify the location of the Rossen culture. 2. Determine the tribes that originated from this location. 3. Find out when these tribes invaded the Roman Empire. First, I'll search for information about the Rossen culture and its location to understand where it is situated. Next, I'll look into the tribes that originated from this location and their historical interactions with the Roman Empire, particularly focusing on any invasions. Finally, I'll determine the specific year at the end of which such an invasion occurred. Let me start by searching for information about the Rossen culture and its location.


Tool call


{"query": "Rossen culture location and historical background"}


Results


1.


2.


[https://neolithicarchitecture.com/rossen-culture/](https://neolithicarchitecture.com/rossen-culture/)


3.


[https://arkeonews.net/stone-age-farmers-settled-near-dortmund-airport-7000-years-ago/](https://arkeonews.net/stone-age-farmers-settled-near-dortmund-airport-7000-years-ago/)


4.


[https://westfalen.museum-digital.de/index.php?id=1223&navlang=en&t=tag](https://westfalen.museum-digital.de/index.php?id=1223&navlang=en&t=tag)


5.


[https://celticlifeintl.com/on-the-trail-of-celtic-europe-2/](https://celticlifeintl.com/on-the-trail-of-celtic-europe-2/)


Response


The Rossen culture is a Central European culture of the middle Neolithic (4,600–4,300 BC). It is located in parts of Germany, Austria, the Low Countries, northeast France, and northern Switzerland. Since it is a Neolithic culture from around 4600 to 4300 BC, it predates the Roman Empire by several millennia. The Roman Empire existed from around 27 BC to 476 AD, so any invasion by tribes from the Rossen culture location would have occurred after the Neolithic period and during the time of the Roman Empire. However, the Rossen culture itself was not involved in invasions of the Roman Empire because it existed thousands of years before the Roman Empire. The tribes that later invaded the Roman Empire were not from the Rossen culture era. I need to search for information about tribes from the region of the Rossen culture that invaded the Roman Empire, and specifically when those invasions occurred.


Tool call


{"query": "tribes from Central Europe region of Rossen culture that invaded Roman Empire and when"}


Results


1.


[https://www.history.com/articles/goths-and-visigoths](https://www.history.com/articles/goths-and-visigoths)


2.


[https://en.wikipedia.org/wiki/Barbarian_invasions_into_the_Roman_Empire_of_the_3rd_century](https://en.wikipedia.org/wiki/Barbarian_invasions_into_the_Roman_Empire_of_the_3rd_century)


3.


[https://www.worldhistory.org/Goths/](https://www.worldhistory.org/Goths/)


4.


[https://europe.factsanddetails.com/article/entry-1089.html](https://europe.factsanddetails.com/article/entry-1089.html)


5.


[https://www.quora.com/Where-did-the-barbarian-tribes-that-invaded-the-Roman-Empire-come-from](https://www.quora.com/Where-did-the-barbarian-tribes-that-invaded-the-Roman-Empire-come-from)


Response


The tribes from the place where the Rossen culture is located (Central Europe) that invaded the Roman Empire were the Goths. The Goths first invaded the Roman Empire in 376 AD, and they defeated the Romans at the Battle of Adrianople in 378 AD. The question asks for the year at the end of which the invasion occurred. Since the invasion began in 376 AD and the major defeat occurred in 378 AD, the invasion was ongoing during the late 4th century. However, the key point is that the invasion started in 376 AD and continued, with the Battle of Adrianople in 378 AD marking a major turning point. The final answer to the question "At the end of what year did the tribes from the place where the Rossen culture is located invade the Roman Empire?" is 378, as this is when the major battle and defeat occurred, marking the end of the initial phase of the invasion. Answer: 378


**Figure 1:** Agent rollout example. Within the trained trajectories, both agents make two search calls with the same information. Only Exa retrieves a webpage with the correct information for the question:[https://en.wikipedia.org/wiki/Crossing_of_the_Rhine](https://en.wikipedia.org/wiki/Crossing_of_the_Rhine) (which states the year 406), which is necessary for the agent to synthesize with the first search results. Both untrained agents incorrectly respond with “never” after a single search call.


### # Training and Reward Design


Beyond the agent and its action space, each rollout is shaped by the training data, optimizer, and reward signal applied at the end of the trajectory.


We train on two multi-hop QA datasets, MuSiQue and HotpotQA15 ,16 , using Dr. GRPO17 .


The reward is a single binary signal at the end of each trajectory. We initially graded with exact substring matching, but found the agent receiving large gains in reward from formatting its responses differently, rather than improving on accuracyb b For example, the agent learned to output increasingly verbose responses that could match the correct answer by chance, or intentionally list alternative answers.See the Appendix for an example.


. We switched to the LLM grader from SimpleQA18 , and apply a -0.25 penalty when a trajectory exceeds context limits.


After training the models, we evaluate using pass@k19 ,c c Research has shown RL can concentrate probability mass on correct trajectories, reducing diversity and degrading pass@k at higher k. Evaluating across multiple values of k therefore helps distinguish genuine improvements in solution-finding from probability-mass concentration on a narrower set of trajectories.20 ,21 ,22


on MuSiQue and HotpotQA test splits and on OOD benchmarks 2WikiMultihopQA, FRAMES, BrowseComp, and SimpleQA15 ,16 ,18 ,23 ,24 ,25 . As a reference point, we compare against Qwen3-235B-A22B-Instruct-250726 with the same toolset. Every variable besides the search backend is fixed.


## # Results


### # Impact of Retrieval Backend


Across evaluations, Exa-trained agents outperform SERP-trained agents. Three patterns stand out:


(i) Post-RL Exa models outperform post-RL SERP models on pass@k across all values of k.


(ii) Training with Exa is more compute efficient than SERP: taking fewer tokens, turns, and search calls to reach the same performance. This also results in lower-token cost inference.


(iii) Exa-trained 4B agents outperform SERP-trained agents on every benchmark and often exceed the larger untrained 235B model.


Exa SERP


4B Base 4B+RL 235B 4B Base 4B+RL 235B


SimpleQA 0.651 0.767 0.730 0.579 0.692 0.689


2WikiMultihopQA 0.500 0.839 0.774 0.461 0.798 0.747


HotpotQA 0.521 0.694 0.632 0.491 0.684 0.620


FRAMES 0.316 0.566 0.604 0.272 0.521 0.578


MuSiQue 0.151 0.311 0.273 0.146 0.307 0.259


BrowseComp 0.020 0.043 0.039 0.008 0.039 0.055


**Figure 2:** Pass@1 accuracy with 200 questions and n=200 rollouts per question across QA and retrieval benchmarks. Highest scores per benchmark are highlighted in


dark blue


, and highest small model results are highlighted in


light blue


. Exa-trained 4B agents outperform SERP-trained agents on every benchmark and often exceed the larger 235B model. Average pass@k


SimpleQA, FRAMES, 2WikiMultihopQA, MuSiQue, HotpotQA, BrowseComp


Exa base


SERP base


Exa RL


SERP RL


pass@k


0.30


0.35


0.40


0.45


0.50


0.55


0.60


0.65


0.70


0.75


1


2


5


10


20


50


k


**Figure 3:** Exa improves pass@k across sampling budgets. Average pass@k across six benchmarks for 4B agents before and after RL with either Exa or SERP as the search backend. The Exa-trained agent achieves the highest performance across all sampling budgets before and after RL.See the Appendix for per-benchmark plots.


### # Token efficiency


We find that training with Exa requires fewer tokens for the same number of steps. This gap is largely due to the difference in turn count per rollout: during training, the SERP agent makes more search calls per rollout than the Exa agent. Because both agents start from the same base policy, they initially use similar numbers of turns and search calls, but this gap grows over training; in our training setup:


- It took 20% more total tokens to train with SERP to step 100 (1.58B vs. 1.89B).
- The Exa agent took 69% fewer tokens to achieve equal performance with the SERP-trained agent (prefill and decode) to match performance (0.58B vs. 1.89B).
- The Exa agent took 62% fewer search calls and 58% fewer turns to achieve equal performance with the SERP-trained agent (93k vs. 248k, 146k vs. 350k).


Per-rollout cost over training


total tokens / rollout


total tokens / rollout


0


5000


10000


15000


20000


25000


30000


35000


0


25


50


75


100


training step


turns / rollout


turns / rollout


1.5


2


2.5


3


3.5


4


4.5


0


25


50


75


100


training step


search calls / rollout


search calls / rollout


0.5


1


1.5


2


2.5


3


3.5


0


25


50


75


100


training step


Exa


SERP


**Figure 4:** Exa training uses fewer tokens and search actions per rollout. Average rollout cost over RL training for agents using Exa or SERP as the search backend. Both agents learn to take more turns and issue more searches over training; the SERP agent's rollouts grow longer, using more total tokens, turns, and search calls per rollout by the end of training. Average rollout cost before and after RL


Base model


Trained model


**Figure 5:** Average rollout cost before and after RL for agents using Exa or SERP. Base policies have equivalent token, turn, and search-call counts across backends. After training the SERP agent uses more tokens, turns, and search calls per rollout than the Exa agent, increasing inference cost despite lower benchmark performance.


### # Search engine at training vs. inference


To separate training-time and inference-time effects, we evaluate each trained agent with both search backends.


- After using Exa for RL, agents perform better than having used SERP for training, regardless of the final engine used.
- Using Exa makes search agents perform better at inference time, regardless of how they were trained.


Cross-backend pass@k


SimpleQA, FRAMES, HotpotQA, MuSiQue, 2WikiMultihopQA, BrowseComp


Exa-trained · Exa engine


Exa-trained · SERP engine


SERP-trained · Exa engine


SERP-trained · SERP engine


pass@k


0.50


0.55


0.60


0.65


0.70


0.75


1


2


5


10


20


50


k


**Figure 6:** Exa's training benefit transfers across inference backends. Average pass@k across six benchmarks when Exa- and SERP-trained agents are evaluated with matched and swapped retrieval methods at inference time. Using Exa at inference improves performance for both trained agents, and training with Exa outperforms SERP-trained agents regardless of inference backend.See the Appendix for per-benchmark plots. base (untrained)


matched (trained on this engine)


cross (trained on the other engine)


**Figure 7:** Pass@1 averaged across six benchmarks for (1) untrained agents (2) agents evaluated on the same backend used during RL (3) agents evaluated on the opposite backend at inference time. Exa improves the untrained base policy and yields stronger trained agents at inference time.


We hypothesize that this difference could be attributable to learning with Exa being more sample efficient: Exa retrieves results containing information to arrive at the correct answer in fewer actions than SERP.


In multi-turn agentic RL, higher per-step success makes rewards less sparse and helps the model learn from fewer rollouts. This may be due to the semantic nature of Exa search, which is designed specifically to handle non-keyword natural-language queries.


To measure per-action sample efficiency, we run the same base policy on the same prompts for roughly 240,000 rollouts per engine (480k total) between six benchmarks.


We see that for a search call from the same policy, Exa returns sites that contain the answer +10.7% more often than SERPd d 36.1±2.4% compared to 32.6±2.3%; Δ+3.50±1.55pp


, and for the first search call in the rollout, +11.5% more oftene e 34.0±2.5% compared to 30.5±2.5%; Δ+3.51±1.68pp


. We believe this indicates that Exa provides a denser signal per action, as the agent is able to see the correct answer more often in a rollout. This may give the agent more opportunities to see the correct answer and reason over it, which may also lead to a lower overall search count. Retrieval quality appears to matter at training time as much as at inference time: a denser per-step signal compounds over many rollouts.


## # Conclusion


In this work, we studied how the search backend shapes RL for large language models.


Holding all else fixed, we compared one agent trained with Exa against one agent trained with a SERP backend. Across our benchmarks, Exa-trained agents achieve higher pass@k at lower training and inference cost. These gains persist when the engine is swapped at inference time, showing that the model may be learning transferable skills for search when training with Exa. Exa also improves inference performance even for agents not trained on Exa, though the strongest results come from using Exa during both training and inference.


### # Future Work


Here is a list of ongoing questions we hope to address in upcoming research.


- Scaling up training with bigger models, longer runs, and larger batch sizes.
- Interpreting what learned characteristics result in improved search performance, particularly after an initial learning phase.
- Train with a more advanced harness including elements like content fetching, and context pruning.


Our results suggest that the retrieval engine is a crucial component for agents searching over the web, especially when paired with the same tool at inference time. A better search backend can both improve the final performance of a search agent and improve the learning signal during RL.


If you want to join us in training models to achieve perfect search, come work at Exa.


[See open roles](https://exa.ai/careers)


## # Acknowledgements


We thank Michael Fine, Joshua Ahn, Will Bryk, and William Guss for providing feedback on earlier drafts of this work. We also thank Nitya Sridhar, Song You, and Zoe Zheng for help creating this post.


## # Citations


1. 1.


Wu, J., Deng, Z., Li, W., Liu, Y., You, B., Li, B., ... & Liu, Z. (2025).[Mmsearch-r1: Incentivizing lmms to search](https://arxiv.org/abs/2506.20670) . arXiv preprint arXiv:2506.20670.


2. 2.


Nguyen, X. P., Pandit, S., Reddy, R. G., Xu, A., Savarese, S., Xiong, C., & Joty, S. (2025).[Sfr-deepresearch: Towards effective reinforcement learning for autonomously reasoning single agents](https://arxiv.org/abs/2509.06283) . arXiv preprint arXiv:2509.06283.


3. 3.


Zhang, J., Lv, X., Feng, L., Hou, L., & Li, J. (2026).[Chaining the Evidence: Robust Reinforcement Learning for Deep Search Agents with Citation-Aware Rubric Rewards](https://arxiv.org/abs/2601.06021) . arXiv preprint arXiv:2601.06021.


4. 4.


Jin, B., Zeng, H., Yue, Z., Yoon, J., Arik, S., Wang, D., ... & Han, J. (2025).[Search-r1: Training llms to reason and leverage search engines with reinforcement learning](https://arxiv.org/abs/2503.09516) . arXiv preprint arXiv:2503.09516.


5. 5.


Song, H., Jiang, J., Min, Y., Chen, J., Chen, Z., Zhao, W. X., ... & Wen, J. R. (2025).[R1-searcher: Incentivizing the search capability in llms via reinforcement learning](https://arxiv.org/abs/2503.05592) . arXiv preprint arXiv:2503.05592.


6. 6.


Chen, M., Sun, L., Li, T., Sun, H., Zhou, Y., Zhu, C., ... & Chen, H.[ReSearch: Learning to Reason with Search for LLMs via Reinforcement Learning](https://arxiv.org/abs/2503.19470) . arXiv 2025. arXiv preprint arXiv:2503.19470.


7. 7.


Zheng, Y., Fu, D., Hu, X., Cai, X., Ye, L., Lu, P., & Liu, P. (2025, November).[Deepresearcher: Scaling deep research via reinforcement learning in real-world environments](https://arxiv.org/abs/2504.03160) . In Proceedings of the 2025 Conference on Empirical Methods in Natural Language Processing (pp. 414-431).


8. 8.


Wei, Z., Yao, W., Liu, Y., Zhang, W., Lu, Q., Qiu, L., ... & Li, L. (2025, November).[Webagent-r1: Training web agents via end-to-end multi-turn reinforcement learning](https://arxiv.org/abs/2505.16421) . In Proceedings of the 2025 Conference on Empirical Methods in Natural Language Processing (pp. 7920-7939).


9. 9.


Sun, H., Qiao, Z., Guo, J., Fan, X., Hou, Y., Jiang, Y., ... & Zhou, J. (2025).[Zerosearch: Incentivize the search capability of llms without searching](https://arxiv.org/abs/2505.04588) . arXiv preprint arXiv:2505.04588.


10. 10.


Zhuang, S., Ma, X., Koopman, B., Lin, J., & Zuccon, G. (2025).[Rank-r1: Enhancing reasoning in llm-based document rerankers via reinforcement learning](https://arxiv.org/abs/2503.06034) . arXiv preprint arXiv:2503.06034.


11. 11.


Jin, B., Yoon, J., Kargupta, P., Arik, S. O., & Han, J. (2025).[An empirical study on reinforcement learning for reasoning-search interleaved llm agents](https://arxiv.org/abs/2505.15117) . arXiv preprint arXiv:2505.15117.


12. 12.


Qwen Team. (2025).[Qwen3-4B-Instruct-2507](https://huggingface.co/Qwen/Qwen3-4B-Instruct-2507) . Hugging Face model card.


13. 13.


Schulman, John and Thinking Machines Lab, "[LoRA Without Regret](https://thinkingmachines.ai/blog/lora/) ", Thinking Machines Lab: Connectionism, Sep 2025.


14. 14.


Thinking Machines Lab. (2026).[tinker-cookbook: search_env.py](https://github.com/thinking-machines-lab/tinker-cookbook/blob/c53f137e705e0933c5f058986a6c2e239cd2d003/tinker_cookbook/recipes/search_tool/search_env.py) . GitHub repository.


15. 15.


Trivedi, H., Balasubramanian, N., Khot, T., & Sabharwal, A. (2022).[♫ MuSiQue: Multihop Questions via Single-hop Question Composition](https://arxiv.org/abs/2108.00573) . Transactions of the Association for Computational Linguistics, 10, 539-554.


16. 16.


Yang, Z., Qi, P., Zhang, S., Bengio, Y., Cohen, W., Salakhutdinov, R., & Manning, C. D. (2018).[HotpotQA: A dataset for diverse, explainable multi-hop question answering](https://arxiv.org/abs/1809.09600) . In Proceedings of the 2018 conference on empirical methods in natural language processing (pp. 2369-2380).


17. 17.


Liu, Z., Chen, C., Li, W., Qi, P., Pang, T., Du, C., ... & Lin, M. (2025).[Understanding r1-zero-like training: A critical perspective](https://arxiv.org/abs/2503.20783) . arXiv preprint arXiv:2503.20783.


18. 18.


Wei, J., Karina, N., Chung, H. W., Jiao, Y. J., Papay, S., Glaese, A., ... & Fedus, W. (2024).[Measuring short-form factuality in large language models](https://arxiv.org/abs/2411.04368) . arXiv preprint arXiv:2411.04368.


19. 19.


Chen, M., Tworek, J., Jun, H., Yuan, Q., Pinto, H. P. D. O., Kaplan, J., ... & Zaremba, W. (2021).[Evaluating large language models trained on code](https://arxiv.org/abs/2107.03374) . arXiv preprint arXiv:2107.03374.


20. 20.


Shao, Z., Wang, P., Zhu, Q., Xu, R., Song, J., Bi, X., ... & Guo, D. (2024).[Deepseekmath: Pushing the limits of mathematical reasoning in open language models](https://arxiv.org/abs/2402.03300) . arXiv preprint arXiv:2402.03300.


21. 21.


Yue, Y., Chen, Z., Lu, R., Zhao, A., Wang, Z., Song, S., & Huang, G. (2025).[Does reinforcement learning really incentivize reasoning capacity in llms beyond the base model?](https://arxiv.org/abs/2504.13837) . arXiv preprint arXiv:2504.13837.


22. 22.


23. 23.


Ho, X., Nguyen, A. K. D., Sugawara, S., & Aizawa, A. (2020, December).[Constructing a multi-hop qa dataset for comprehensive evaluation of reasoning steps](https://arxiv.org/abs/2011.01060) . In Proceedings of the 28th International Conference on Computational Linguistics (pp. 6609-6625).


24. 24.


Krishna, S., Krishna, K., Mohananey, A., Schwarcz, S., Stambler, A., Upadhyay, S., & Faruqui, M. (2025, April).[Fact, fetch, and reason: A unified evaluation of retrieval-augmented generation](https://arxiv.org/abs/2409.12941) . In Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 1: Long Papers) (pp. 4745-4759).


25. 25.


Wei, J., Sun, Z., Papay, S., McKinney, S., Han, J., Fulford, I., ... & Glaese, A. (2025).[Browsecomp: A simple yet challenging benchmark for browsing agents](https://arxiv.org/abs/2504.12516) . arXiv preprint arXiv:2504.12516.


26. 26.


Qwen Team. (2025).[Qwen3-235B-A22B-Instruct-2507](https://huggingface.co/Qwen/Qwen3-235B-A22B-Instruct-2507) . Hugging Face model card.


## # Appendix


### # Agent learning behavior


As noted in **Figure 5** , we observe turn counts increasing during training: the mean query count rises from 1.18 to 2.75 for the Exa agent and from 1.21 to 3.23 for the SERP agent, and the fraction of rollouts issuing a single search call dropped from 61% at the base policy to 17% (Exa) and 12% (SERP).


While we see agent turn counts increase as rewards increase, we are curious whether the agent is learning to decompose and reason over multiple searches rather than re-attempting the same query multiple times. To measure this repetition, we compute the pairwise Jaccard overlap across search calls in a rollout on three metrics: (i) queries (ii) returned result URLs (iii) returned snippets (all stopwords removed). We measure a repetitive-question ceiling by computing the Jaccard overlap for queries made by the same policy for the same question.


We see that searches remain distinct across turns. Across 415k multi-call (n ≥ 2) rollouts pooled over the 6 benchmarks, all three overlap measures stay below the same-question ceiling, and remain roughly constant across turn counts, even in rollouts with n ≥ 7 search calls. This suggests that both agents have successfully learned improved search behavior, making multiple unique searches and synthesizing over results to produce a final response.


Search metrics across turns


n=1


n=2


n=3


n=4


n=5


n=6


n=≥7


(B) Query Token Jaccard


mean overlap (±1σ)


0.00


0.15


0.30


0.45


0.60


0.75


2


3


4


5-6


7+


# search calls per rollout


(C) Result URL Jaccard


mean overlap (±1σ)


0.00


0.10


0.20


0.30


0.40


0.50


2


3


4


5-6


7+


# search calls per rollout


(D) Result Snippet Token Jaccard


mean overlap (±1σ)


0.00


0.10


0.20


0.30


0.40


0.50


0.60


2


3


4


5-6


7+


# search calls per rollout


base policy


trained policy


Exa


SERP


same-question ceiling


**Figure 8:** RL teaches multi-search decomposition. Search-call distributions and overlap metrics for base and trained agents, pooled across six benchmarks. Training shifts rollouts from mostly single-search behavior toward multi-search trajectories, while query, URL, and snippet overlap remain well below same-question ceilings across search-call counts, suggesting the agent learns to issue distinct follow-up searches rather than simply repeating the same query.


### # Robustness to search queries


We noticed during training that the agents would issue certain queries that returned no results from SERP, but would have returned the desired 5 results from Exa.


For example, one such query was:` Chuck Essegian Varaz Samuelian baseball player position or Cleverdicks UK Privy Councillor political party history` , which returns zero results on SERP but returns the intended five from Exa.


To quantify this, we compare the per-search-call result-count distributions of Exa and SERP across all 476,474 search calls issued during training (224,462 Exa, 252,012 SERP). Exa returns fewer than the requested five results on 12 calls (0.005%; one such query was` Kyzyl-Kyshtak Kyrgyzstan location` ), whereas SERP returns fewer than five results for 6.74% of calls (n=16,991) and zero results for 0.19% of calls (n=484).


Inspection of all 16,991 deficient SERP queries (results n<5) shows a consistent failure mode: the policy issues short natural-language queries that combine multiple entities, e.g.` Marge Champion Disney stalking incident Peggy Ray` ;` David or Mona Lisa recreated in plaster imitation marble fiberglass snow` ; and` Christian Desmares or Luciano Lepinay involved in Timbuktu film` .


While returning fewer results may provide a form of useful signal to the agent, such as knowing that a certain topic is not documented, we believe that this may introduce more variance in action space for agents using SERP. Trajectories with deficient queries require more searches to reach useful information, reducing the sample efficiency and increasing rollouts during training and inference. This could contribute to both the difference in token cost and final quality we observe in our results.


Deficient search results over training


< 5 results returned


fraction of search calls


0.000


0.025


0.050


0.075


0.100


0.125


0.150


0


25


50


75


100


training step


0 results returned


fraction of search calls


0.000


0.005


0.010


0.015


0.020


0


25


50


75


100


training step


Exa


SERP


**Figure 9:** Fraction of search calls returning fewer than five results or zero results over RL training. SERP regularly returns fewer than the requested five results and occasionally returns no results, while Exa remains near zero across training steps. Cumulative deficient-return rates during training


Exa


SERP


**Figure 10:** Cumulative fraction of training search calls returning at most x results, pooled over all training iterations. Across 224k Exa calls and 252k SERP calls, Exa almost always returns the full requested result set, whereas SERP returns fewer than five results on 6.74% of calls and zero results on 0.19% of calls.


To test whether this failure mode is specific to Qwen3-4B-Instruct-2507, we run all queries across all 6 evaluations on varying open-source model families on SERP, showing that this is not an isolated result; models that have been post-trained to use keyword-based search effectively may even tend toward a higher failure rate due to their usage of advanced` site:` and` ""` operators, which can filter results but also may result in no-result searches.


model num queries <5 results 0 results site: operators "" operators


Qwen3-30B-A3B 457 2.63% 0.22% 0.66% 0.00%


Qwen3.5-397B-A17B 1,355 14.98% 2.51% 0.37% 23.25%


Qwen3.5-4B 1,787 13.49% 3.41% 0.34% 15.84%


Kimi-K2.5 1,641 23.58% 5.48% 0.85% 43.33%


Nemotron-Nano-30B-A3B 899 9.45% 0.78% 0.44% 4.78%


Nemotron-Super-120B-A12B 1,603 15.35% 1.75% 1.06% 31.00%


gpt-oss-120b 1,278 21.83% 4.85% 0.55% 40.92%


gpt-oss-20b 1,482 16.46% 3.58% 0.20% 29.08%


**Figure 11:** Fraction of SERP search queries returning fewer than five results or zero results when issued by different open-source models alongside the rate of advanced search-operator use. Deficient-result rates appear across model families and are often higher for models that frequently use quoted phrases.


### # System prompt


```text
You are an expert assistant who solves tasks using a search tool. Here are instructions for how to solve a problem:
1. Think step by step before calling the tool and after you receive the result of the tool call. Decide what queries to call the tool with.   2. Call the tool with the queries you have decided on.   3. Think step by step again after you receive the result of the tool call. If you have the information you need, you can stop here.   4. Otherwise, come up with new queries that combine information from the previous results.   5. Include your final answer after the "Answer:" prefix. The answer should be between one to five words.
```


### # Pass@k Estimation and Confidence Intervals


For each evaluation question, we sample nn


n


rollouts and observe cc


c


correct answers. We use the unbiased estimator for pass@ kk


k


19 :


pass^@k=1−(n−ck)(nk)\\widehat{\\mathrm{pass}}@k = 1 - \\frac{\\binom{n-c}{k}}{\\binom{n}{k}}


pass


​


@


k


=


1


−


(


k


n


​


)


(


k


n


−


c


​


)


​


which gives the probability that at least one of kk


k


sampled rollouts is correct.


Benchmark-level pass@ kk


k


is computed by averaging this estimate across all QQ


Q


evaluation questions. To construct error bars, we hold the evaluation questions fixed and model each question's number of correct rollouts as


ci′∼Binomial(n,p^i),p^i=cin,c_i' \\sim \\mathrm{Binomial}(n, \\hat{p}_i), \\qquad \\hat{p}_i = \\frac{c_i}{n},


c


i


′


​


∼


Binomial


(


n


,


p


^


​


i


​


)


,


p


^


​


i


​


=


n


c


i


​


​


,


We then compute the variance of pass@ kk


k


for each question and aggregate across questions under an independence assumption. Reported bands are 95%95\\%


95%


confidence intervals, ±1.96σ\\pm 1.96\\sigma


±


1.96


σ


, clipped to \[0,1\]\[0,1\]


\[


0


,


1


\]


.


### # Model and Hyperparameters


We train with a batch size of 64 and group size of 16 across 100 training steps. We use a constant learning rate of 2.35×10⁻⁴ and LoRA rank of 32, and operate with a context limit of 32k tokens and per-generation budget of 2048 tokens. Each rollout has a maximum of 10 assistant messages (reasoning then issuing a search call is one message).


As multi-turn rollouts are the main training bottleneck, we use streamed minibatch optimization and asynchronous training using Tinker, with a max staleness of 2 steps to limit off-policy drift; older rollouts are discarded. We also discard constant-reward groups, which provide no useful gradient signal.


### # Per-Benchmark Plots for Figure 3


FRAMES pass@k


pass@k


0.30


0.40


0.50


0.60


0.70


0.80


1


2


5


10


20


50


k


MuSiQue pass@k


pass@k


0.20


0.30


0.40


0.50


0.60


1


2


5


10


20


50


k


2WikiMultihopQA pass@k


pass@k


0.45


0.55


0.65


0.75


0.85


0.95


1


2


5


10


20


50


k


SimpleQA pass@k


pass@k


0.50


0.55


0.60


0.65


0.70


0.75


0.80


0.85


0.90


0.95


1


2


5


10


20


50


k


HotpotQA pass@k


pass@k


0.45


0.50


0.55


0.60


0.65


0.70


0.75


0.80


0.85


1


2


5


10


20


50


k


BrowseComp pass@k


pass@k


0.00


0.05


0.10


0.15


0.20


0.25


1


2


5


10


20


50


k


Exa base


Exa RL


SERP base


SERP RL


**Figure 12:** Pass@k improvements are consistent across individual benchmarks.


### # Per-Benchmark Plots for Figure 6


FRAMES pass@k


pass@k


0.55


0.60


0.65


0.70


0.75


0.80


0.85


0.90


1


2


5


10


20


50


k


MuSiQue pass@k


pass@k


0.40


0.45


0.50


0.55


0.60


0.65


0.70


1


2


5


10


20


50


k


2WikiMultihopQA pass@k


pass@k


0.80


0.85


0.90


0.95


1.00


1


2


5


10


20


50


k


SimpleQA pass@k


pass@k


0.70


0.75


0.80


0.85


0.90


0.95


1


2


5


10


20


50


k


HotpotQA pass@k


pass@k


0.60


0.65


0.70


0.75


0.80


0.85


1


2


5


10


20


50


k


BrowseComp pass@k


pass@k


0.00


0.05


0.10


0.15


0.20


0.25


0.30


1


2


5


10


20


50


k


Exa-trained · Exa engine


Exa-trained · SERP engine


SERP-trained · Exa engine


SERP-trained · SERP engine


**Figure 13:** Cross-backend gains hold across individual benchmarks. Per-benchmark pass@k curves for Exa- and SERP-trained agents evaluated with matched and swapped retrieval methods. The benchmark-level results generally mirror the pooled pattern: Exa at inference improves performance for both trained agents, and Exa-trained agents outperform SERP-trained agents regardless of inference backend.


### # Early reward hacking example:


In this example, the trained agent outputted a needlessly verbose response (by moving the` Answer:` text earlier), which hits the correct answer under Exact Match substring grading, but fails under LLM grading.


Query


“Who is Maurice De Berkeley, 4th Baron Berkeley's maternal grandfather?”


Expected answer


\['Roger Mortimer, 1st Earl of March'\]


Untrained Agent


Searches


\[...\]


Answer


"Edmund Mortimer, 2nd Baron Mortimer (1252–1304)"


Trained Agent


Searches


\[...\]


Answer


"Piers de Geneville (or Piers I de Geneville) Based on the search results, Maurice de Berkeley, 4th Baron Berkeley, was the son of Thomas de Berkeley, 3rd Baron Berkeley, and Margaret Mortimer, Baroness Berkeley. Margaret Mortimer was the daughter of Roger Mortimer, 1st Earl of March


, and Joan de Geneville, Baroness Geneville. Therefore, Margaret Mortimer's maternal grandfather was Joan de Geneville's father, who was Piers de Geneville of Trim Castle and Ludlow (also known as Piers I de Geneville). His mother was Jeanne of Lusignan. So the maternal grandfather of Maurice de Berkeley, 4th Baron Berkeley, was Piers de Geneville."
