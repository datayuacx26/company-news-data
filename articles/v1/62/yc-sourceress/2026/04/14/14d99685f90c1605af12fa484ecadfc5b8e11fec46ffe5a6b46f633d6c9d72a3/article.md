---
schema_version: "1.0.0"
document_id: "14d99685f90c1605af12fa484ecadfc5b8e11fec46ffe5a6b46f633d6c9d72a3"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/2026-04-24-podcast-episode-38-jamie-simon-and-daniel-kunin"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:2f60c0967b3f9abacd2994b7a3b6ed61d7d1545e0625a631e991986fa2ad7f90"
---

# There Will Be a Scientific Theory of Deep Learning

For the last few years, Imbue has supported a small research team called Feature Lab (FLAB), led by research fellow Jamie Simon, to study the fundamental science of deep learning.


Deep learning works extraordinarily well, and we still largely don’t know why. Neural networks write code, diagnose disease, do work. The machinery is, in principle, legible: architecture, data, objective, learning rule. Yet we have no unified framework for why training works, what the resulting networks will do, or how to predict their behavior from first principles. The field trains by intuition, folklore, trial and error, scale.


Today, a new paper by Jamie Simon, Daniel Kunin, and twelve co-authors from Berkeley, Harvard, NYU, Stanford, the Flatiron Institute, Penn, and the Astera Institute are publishing a paper that argues this is about to change.


[There Will Be a Scientific Theory of Deep Learning](https://arxiv.org/abs/2604.21691) names an emerging discipline: *learning mechanics* . It consolidates five converging lines of evidence that a rigorous theory of deep learning is not merely desirable, but beginning to emerge.


Imbue is committed to supporting this research. Deterministic engineering of deep learning systems will make them easier to build openly, harder to monopolize. To accompany the paper, Jamie and Dan discuss the ideas on *Generally Intelligent* :


## Five converging threads


**Solvable toy models reveal real phenomena.** Deep linear networks admit exact dynamics. The solutions reveal a greedy low-rank bias: simpler components of a task acquired before complex ones. The same pattern recurs in realistic networks. Simple models that illuminate general principles are the load-bearing bricks of any physical science. They are starting to accumulate here.


**Taking limits clears away confounders.** As the ideal gas law emerges cleanly at infinite N, neural networks simplify dramatically at infinite width or depth. The infinite-width limit separates lazy training, where hidden representations don’t evolve, from rich training, where they do. Understanding that distinction, and finding the parameterisation that preserves feature learning at scale, produced µP, now used in production-scale LLM training. The limit is where the structure becomes visible.


**Macroscopic regularities demand explanation.** Scaling laws are empirically robust across architectures and modalities. Their origins remain open. Edge-of-stability, in which gradient descent sharpness reliably plateaus at 2/η, now has a partial first-principles account. These are the gas laws of deep learning: reproducible, quantitative, pointing at a simpler underlying picture.


**Hyperparameters are not irreducible mysteries.** µP identifies the non-dimensional quantities that remain stable as width scales, enabling hyperparameter transfer from small proxies to production scale. This follows from the mathematics of the infinite-width limit. It is also a template. If one set of hyperparameters can be understood and retired as a confounder, others can too.


**Diverse systems converge to similar solutions.** Two diffusion architectures, given the same random noise seed, generate the same image once large enough. Vision and language models, trained on entirely separate data, develop increasingly similar internal representations as they scale. Universality across systems is the strongest possible signal that a compact theoretical explanation exists.


## Scope and relationship to adjacent fields


Mechanistic interpretability studies the features, circuits, and algorithms behind model behaviour: the *biology* of deep learning. Learning mechanics asks why those structures emerge from training in the first place.


The natural sciences have long benefited from physics and biology operating in parallel, each illuminating what the other cannot reach alone.


The paper anticipates the same relationship here. Mechinterp has put data at the center of every problem it studies, in ways pure theory has often neglected. Learning mechanics can offer the mathematical foundations that mechinterp’s core working assumptions (linear representability, locality, sparsity) currently proceed without.


Conditions for this synthesis are better than five years ago. Practice has converged on reproducible training methods, which makes large models better objects of scientific study. The systems are genuinely large. Infinite-width approximations are meaningful at widths of 10,000 in a way they weren’t before. Researchers from physics, mathematics, neuroscience, and statistics are bringing tools that ML theory has historically underutilised.


## Open directions


The paper closes with 12 open research directions and advice for newcomers. The most tractable near-term work: solvable models of genuinely nonlinear feature learning, a theory that captures natural data structure, a formal definition of the features neural networks learn.


[learningmechanics.pub](https://learningmechanics.pub/) launches today with the open questions catalogue, interactive tutorials, and a Discord. Read the paper. Read the open directions. Reach out to the researchers!
