---
schema_version: "1.0.0"
document_id: "e237b95e26a978579a0531cc1f5f6abf6011b96e5b50ada366b417c6118ab4f2"
company_key: "yc-thesis"
company: "Thesis"
source_id: "yc-thesis-rss-df316ac2148b"
canonical_url: "https://thesislabs.ai/research/discovering-rl-algos"
published_at: "2026-02-19T00:00:00+00:00"
first_seen_at: "2026-08-10T04:04:12.843518+00:00"
fetched_at: "2026-08-10T04:04:13.995165+00:00"
content_hash: "sha256:4b3f354de2823d81096b3bb791144ce56ba92e7233f5a953b9db354bec030b2f"
---

# Discovering State-of-the-Art Reinforcement Learning Algorithms

Every RL algorithm you've ever heard of was created by a human. This DeepMind *Nature* paper asks whether a machine can also do it. The answer is yes: **DiscoRL** beats every manually designed RL algorithm on Atari and generalizes to benchmarks it was never trained on.


## How it works


*Fig. 1: The full discovery loop, from agent architecture to meta-optimization.*


The setup involves two nested loops (Fig. 1). A population of agents interact with environments and update their parameters using the current learning rule. That rule is represented by a meta-network, which gets updated to make the agents learn better. The agent minimizes:


L(θ)=Es,a∼πθ\[D(π^,πθ(s))+D(y^,yθ(s))+D(z^,zθ(s,a))+Laux\]\\mathcal{L}(\\theta) = \\mathbb{E}_{s,a \\sim \\pi_\\theta}\\left\[D(\\hat{\\pi}, \\pi_\\theta(s)) + D(\\hat{y}, y_\\theta(s)) + D(\\hat{z}, z_\\theta(s,a)) + \\mathcal{L}_\\text{aux}\\right\]


L


(


θ


)


=


E


s


,


a


∼


π


θ


​


​


\[


D


(


π


^


,


π


θ


​


(


s


))


+


D


(


y


^


​


,


y


θ


​


(


s


))


+


D


(


z


^


,


z


θ


​


(


s


,


a


))


+


L


aux


​


\]


where DD


D


is KL divergence. The targets π^,y^,z^\\hat{\\pi}, \\hat{y}, \\hat{z}


π


^


,


y


^


​


,


z


^


come from the meta-network, not the researcher. The prediction types mirror RL's classic prediction/control split: y(s)∈Rny(s) \\in \\mathbb{R}^n


y


(


s


)


∈


R


n


is observation-conditioned (like a state-value function), z(s,a)∈Rmz(s,a) \\in \\mathbb{R}^m


z


(


s


,


a


)


∈


R


m


is action-conditioned (like a Q-function). This makes the search space expressive enough to rediscover existing RL concepts while remaining open to new ones.


The meta-objective maximizes cumulative agent returns across environments:


J(η)=EE,θ\[G(E,θ(η))\],∇ηJ(η)=E\[∇ηθ⋅∇θG\]J(\\eta) = \\mathbb{E}_{\\mathcal{E}, \\theta}\\left\[G(\\mathcal{E}, \\theta(\\eta))\\right\], \\quad \\nabla_\\eta J(\\eta) = \\mathbb{E}\\left\[\\nabla_\\eta \\theta \\cdot \\nabla_\\theta G\\right\]


J


(


η


)


=


E


E


,


θ


​


\[


G


(


E


,


θ


(


η


))


\]


,


∇


η


​


J


(


η


)


=


E


\[


∇


η


​


θ


⋅


∇


θ


​


G


\]


This meta-gradient backpropagates through 20 unrolled agent update steps. A second "meta-RNN" unrolls forward across parameter updates (θi→θi+1)(\\theta_i \\to \\theta_{i+1})


(


θ


i


​


→


θ


i


+


1


​


)


, giving the rule access to the agent's learning dynamics over its lifetime.


## Results


*Disco57 (blue) from Atari; Disco103 (orange) from Atari + ProcGen + DMLab-30. Dashed lines: MuZero, MEME, Dreamer, STACX, IMPALA, DQN, PPG, PPO, Rainbow.*


Benchmark Result


Atari 57 IQM **13.86** , best ever reported


ProcGen (zero-shot) Beats all published methods including MuZero


NetHack NeurIPS 2021 3rd of 40+ teams, no domain knowledge


Crafter Near human-level (Disco103)


Sokoban Approaches MuZero (Disco103)


ProcGen is the most striking result: DiscoRL never saw those environments during training and still beat MuZero. Scaling from 57 to 103 training environments improved performance on every benchmark, including held-out ones.


## What did it discover?


The yy


y


and zz


z


predictions carry more information about upcoming rewards and future policy entropy than the policy or value function. Gradient analysis in Beam Rider shows them attending to distant enemies, while the policy watches nearby threats and the value function tracks the scoreboard.


Most interestingly, perturbing zt+kz_{t+k}


z


t


+


k


​


strongly shifts the current target z^t\\hat{z}_t


z


^


t


​


, meaning the meta-network uses future predictions to construct current targets. It rediscovered bootstrapping on its own!


The rule quality scales with environment diversity and shows no sign of saturation. Same story as language models, now applied to algorithm discovery.


## Big Picture


If AI can infer RL rules from a population of agents interacting with an environment, can the same be done for general AI systems?


---


*Oh et al., Discovering state-of-the-art reinforcement learning algorithms, Nature 648, 312–319 (2025).[https://www.nature.com/articles/s41586-025-09761-x](https://www.nature.com/articles/s41586-025-09761-x)*
