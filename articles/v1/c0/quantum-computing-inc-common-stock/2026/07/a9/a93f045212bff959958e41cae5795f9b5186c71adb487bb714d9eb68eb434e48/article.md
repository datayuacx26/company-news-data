---
schema_version: "1.0.0"
document_id: "a93f045212bff959958e41cae5795f9b5186c71adb487bb714d9eb68eb434e48"
company_key: "quantum-computing-inc-common-stock"
company: "Quantum Computing Inc."
source_id: "quantum-computing-inc-common-stock-news-import-a8d2b20c6439"
canonical_url: "https://quantumcomputinginc.com/news/blogs/can-quantum-loss-aid-computing"
published_at: null
first_seen_at: "2026-07-25T20:21:29.849576+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:7cc5dd015dae1925cebee1f2ba165a30ad755ed6842c590850e3408580bdc7b0"
---

# Can quantum loss aid computing? - Quantum Computing Inc

Default


# Can quantum loss aid computing?


This blog is a less-technical summary of our team’s recent publication in[Grover Speedup from Many Forms of the Zeno Effect](https://arxiv.org/pdf/2305.11146) . In this paper, we discuss how various different mechanisms based on Zeno effects, can achieve Grover-like speedup for solving unstructured search problems. This includes both demonstration of previously known results (for example, the speedup for adiabatic quantum computation), but also new mechanisms, including ones based on the complete destruction of the system if it is found in an excited state. The findings are important not only because it shows a promising application, but it is also a way of assessing the potential of emerging technologies that aren’t yet fully developed. This is similar to the case in the early days of quantum computing in the 1990s, where algorithms like Grover’s and Shor’s demonstrated quantum computing’s potential. Grover’s algorithm, for example, showed how quantum computing could significantly speed up search tasks, sparking interest in its potential for optimization. This same challenge of evaluating possibilities will arise whenever a new paradigm emerges, such as our current work with entropy computing. In this paradigm, one key concept is using photon loss to create a quantum effect called the Zeno effect, which can be realized photonically[through the Zeno blockade](https://quantumcomputinginc.com/learn/lessons/the-zeno-blockade) . So how can photon loss aid in unstructured search?


## Unstructured search


A common example used to demonstrate potential quantum advantages in optimization is the unstructured search problem. In this artificial scenario, you can verify a proposed solution (usually represented as a bitstring) and find out if it’s correct, but you receive no feedback on how close an incorrect answer is to the target. Classically, solving this problem requires either random guessing or an exhaustive search, both of which, on average, involve checking about half of the possible bitstrings. There’s no way to leverage partial information because an incorrect bitstring might differ from the solution by a single bit or by many, and there’s no way to tell. Remarkably, quantum computers can solve this problem faster: they can identify the correct answer with a number of checks proportional to the square root of the total number of bitstrings. This speedup was first demonstrated by[Lov Grover in 1996](https://arxiv.org/abs/quant-ph/9605043) with a gate-model quantum computer and was later shown to apply to other quantum paradigms, such as[adiabatic quantum computing](https://arxiv.org/abs/quant-ph/0107015) (a theoretical form of quantum annealing) and[similar algorithm families](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.99.022339) . This leads to the research question our theory team sought to answer: *Can a loss-based quantum Zeno effect provide the same advantage?*


## Answering the question


To answer this question, we need to formulate an idealized but still physically realistic mathematical model of computing with Zeno effects from loss. By performing theoretical analysis of this model we can assess whether a quantum speedup based on loss mechanisms would be possible. Our team’s findings were ultimately affirmative, indicating that our approach is well-founded. Had the results been negative, it would have suggested a need to reconsider our assumptions. The initial step, therefore, was to develop a model that aligned with physical reality.


## A physically realistic model


To construct a mathematical model, our team looked at what had already been done in the literature, in particular to[this paper](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.66.032314) . The literature model our team started with had the advantage of being well motivated and having strong arguments that it is realistic, based on the similarity to an existing gate model algorithm. This was a good starting point, but it was a model for estimating how long it would take to perform a measurement to distinguish two quantum states with a given set of properties. Our team’s contribution was to modify the mathematical model to represent the destruction of one of the states, transforming it from a description of discrete events into a continuous process (the details of which can be found[in the paper](https://arxiv.org/pdf/2305.11146) ). Once the new model was created, the next step was to determine how quickly it could solve the problem. A key insight in solving this problem comes from the “single avoided crossing model”, as[proposed in section E of this paper](https://arxiv.org/abs/1709.00371) . In summary, the single avoided crossing model allows the problem to be reduced to two states, one corresponding to the correct answer and one to everything else.


An energy diagram which shows how the single avoided crossing model arises in unstructured search.


Conceptually, this model is based on the key realization that the computation needs to slow down when the energy of the first “excited” state of the system becomes very close to the energy of the lowest energy state, known as the “ground” state. The approach to this model can be seen in the picture above (or figure 10 in the paper). Here we see that in this search example, a close approach only happens within the box, so everything outside of the boxed area can be ignored. This greatly reduces the complexity of the calculations. In fact, it reduces it to a point where calculations can be done by hand. Within this two-state system, our team could actually compute the time it would take to find the answer, and show that a quantum speedup was indeed possible.


This is shown conceptually below as a system that is slowly changed and prevented from leaving a “ground state” and changed until it corresponds to the solution. This is achieved in a very unconventional way, using a quantum[Zeno effect](https://quantumcomputinginc.com/learn/lessons/the-quantum-zeno-effect) . This can be achieved by coupling to another system that determines if the system is in the excited state and aborts the calculation if it is. Because of some quirks of quantum mechanical theory, doing this rapidly enough can lead to the system being prevented from entering the excited state and thus solving the problem. This process is depicted in the diagram below.


A cartoon representation of the system which is used to show a Zeno effect from loss


It is worth taking a moment to appreciate just how strange this is from a classical perspective. We are essentially solving a problem by designing a system that self-destructs if it ever finds itself in the wrong answer. Without the laws of quantum mechanics, such an approach would obviously never work, but thanks to the[quantum Zeno effect](https://quantumcomputinginc.com/learn/lessons/the-quantum-zeno-effect) , not only can we solve the problem reliably, we can avoid the system ever self-destructing.


## Careful application of the theory


Theoretical work of this kind must be conducted with great care. For instance, our team identified an unrealistic assumption that, if included, would have falsely suggested a speedup beyond the limits of a quantum setting. Equally important is building a model implementation that genuinely supports a speedup, as shown in the figure below (or Figure 5 of the paper). Here, the open symbols represent one method of defining operations that fail to achieve a Zeno effect, causing the success probability to approach zero as the number of operations increases. In contrast, the filled symbols represent a slightly modified approach, where the success probability approaches 100%. While we won’t go into detail here, this example highlights the level of precision required.


Marked state probability versus number of operations for two effective Zeno implementations (filled symbols) and two ineffective ones (hollow symbols).


## General results


Although the main motivation behind our team’s effort was to theoretically justify the entropy computing paradigm, they realized that the results could be further generalized. In fact, they systematically identified nine paradigms which would all yield an optimal quantum speedup. Of these, only three had been examined previously in the literature. These models are also likely to be useful outside the context of Zeno effects. The nine routes to a speedup are succinctly given in a table in the paper, which we have reproduced below.


The mathematical formulae for the nine mechanisms which have been found to achieve a speedup in the paper.


## The big picture


The main point of this blog was to show an example of how theoretical justification for a new paradigm can be constructed and why it is important. To illustrate this, we give a high-level overview of[our recently published paper](https://arxiv.org/pdf/2305.11146) . The point is not to show all the mathematics in full detail, but rather to conceptually walk the reader through the steps involved. This also makes another important point that scientific research, especially more theoretical research, can often lead to interesting results which were not part of the original motivation. This indeed happened with the work we are studying. Not only did it help motivate the utility of the mechanism behind entropy computing, it also uncovered nine separate routes to theoretical advantage, most of which have not been previously explored.


[Previous page](https://quantumcomputinginc.com/news/blogs/from-the-desk-of-the-ceo-november-2024-message)


[Next page](https://quantumcomputinginc.com/news/blogs/from-the-desk-of-the-ceo-october-2024-message)
