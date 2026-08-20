---
schema_version: "1.0.0"
document_id: "03f3ca20c404ef49407d07355f78209d4e859b2630f795b55b9028ccfce69c11"
company_key: "yc-helicone"
company: "Helicone"
source_id: "yc-helicone-news-import-836d1e549638"
canonical_url: "https://www.helicone.ai/blog/tree-of-thought-prompting"
published_at: "2025-01-14T00:00:00+00:00"
first_seen_at: "2026-07-21T22:44:44.297475+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:b10640ae3cce2e0d7a7266dd039ab6a6b831bfa2eeb142e2cd47c11ddd74928f"
---

# Tree-of-Thought Prompting: Key Techniques and Use Cases

# Tree-of-Thought Prompting: Key Techniques and Use Cases


Jan 14, 2025


· 12 minute read


Lina Lam


· Jan 14, 2025


While the Chain of Thought (CoT) prompting technique allows LLMs to thrive at step-wise reasoning problems, its inability to strategically look ahead and weigh different alternatives makes it fall short on tasks requiring decision-making.


The Tree of Thought (ToT) prompting technique unlocks new neural pathways for LLMs, encouraging them to explore multiple thoughts and self-evaluate at each step, even as they look ahead or backtrack to determine the next best move.


According to[Yao et el (2023)](https://arxiv.org/abs/2305.10601) , the Tree of Thought (ToT) is a prompting framework that generalizes over the[Chain-of-Thought (CoT)](https://www.helicone.ai/blog/chain-of-thought-prompting) technique breaking the token-level, left-to-right decision-making barrier. This technique combines advanced search algorithms with the innate self-evaluative properties of LLMs to implement deliberate decision-making.


In this article, we will explain Tree of Thought prompting, examine how it works, and compare its performance to other prompting techniques.


## What You'll Learn


- What is Tree of Thought Prompting?
- How Tree of Thought Prompting works?
- Tree of Thought Frameworks
- How to Use Tree of Thought Prompts
- How to Evaluate ToT Prompts Using Helicone for Better Accuracy
- Tree of Thought Prompting vs. Other Methods
- When to Use Tree of Thought


## What is Tree of Thought Prompting?


Tree of Thought (ToT) prompting is a new framework for language model inference proposed by Yao et el (2023) and[Long (2023)](https://arxiv.org/abs/2305.08291) , amongst other researchers in May 2023. The novel prompting technique uses intermediate reasoning steps to give LLMs complex, strategic reasoning power.


This technique takes on a human-like approach to problem-solving (trial and error), exhaustively working through every possible outcome in a problem/solution space. The computation progresses in a tree-like manner, following the most likely step at each turn, and backtracking when necessary until it finds the correct solution.


*Image source:[cameronrwolfe.substack.com](https://cameronrwolfe.substack.com/p/tree-of-thoughts-prompting)*


## How Tree of Thought Prompting Works?


The main idea behind ToT prompting is enhancing LLMs to solve complex problems using tree search to map out a solution space and engage in a multi-turn conversation with the model. However, as you explore different ToT techniques, you'll find slight differences in the search algorithms they use to sort through intermediate steps.


Breath-first search (BFS) and depth-first search (DFS) are the most popularly used algorithms for traversing tree or graph data structures in ToT. Other powerful search strategies include[binary tree](https://medium.com/@arjunprakash027/binary-searchtree-and-its-algorithms-in-python-1a116c852c1b) ,[beam search](https://deepgram.com/ai-glossary/beam-search-algorithm) , and[uniform cost search](https://www.geeksforgeeks.org/uniform-cost-search-ucs-in-ai/) .


### Depth-First Search (DFS) Algorithm


```text
def    dfs_iterative  ( graph, start  ):
visited =  set  ()
stack = [start]
while   stack:
vertex = stack.pop()
if   vertex  not    in   visited:
visited.add(vertex)
stack.extend( set  (graph[vertex]) - visited)
return   visited


# Example usage
graph = {
'A'  : [ 'B'  ,  'C'  ],
'B'  : [ 'A'  ,  'D'  ,  'E'  ],
'C'  : [ 'A'  ,  'F'  ],
'D'  : [ 'B'  ],
'E'  : [ 'B'  ,  'F'  ],
'F'  : [ 'C'  ,  'E'  ]
}


print  (dfs_iterative(graph,  'A'  ))
# Output: {'A', 'B', 'D', 'E', 'F', 'C'}


```


*Source:[medium.com/@kapildevkhatik2](http://medium.com/@kapildevkhatik2)*


*Source: Yao et el. (2023)*


At a high level, ToT helps LLMs achieve deliberate reasoning by:


1. Generating diverse intermediate "thought" pathways geared toward problem-solving.
2. Leveraging a tree search strategy to explore the problem space.
3. Self-evaluate thoughts via deliberate reasoning


## Tree of Thought Frameworks


In an attempt to surmount the limitations of chain-of-thought reasoning techniques, several AI researchers have proposed the concept of Tree of Thought.


In Yao et el. (2023)'s proposal, the ToT framework leverages Depth-first search (DFS), Breadth-first search (BFS), or Beam search algorithms to traverse the tree. As generic search strategies, DFS/BFS/beam search algorithms can only be applied to general solutions such as crossword puzzles, the game of 24, creative writing, and other non-trivial type problems.


Here's a schematic demonstration of how the framework combines the DFS search strategy with self-evaluation to solve The Game of 24 puzzles.


*Source: Yao et el. (2023)*


Long (2023) augments LLMs with several modules including a ToT controller, which enables it to solve more specific problems. More precisely, this contraption combines reinforced learning with ToT, encouraging the controller to self-learn as it consumes data sets over time.


In addition to the ToT controller, this suped-up LLM uses a memory module to track preceding token sequences. That way, it can easily retrace its steps and explore new directions. ToT-enabled LLMs can solve more complex puzzle games like Sudoku.


Based on the demonstrations shown in these papers, the tree of thought frameworks allows developers to build LLM-enabled applications with advanced reasoning capabilities such as planning, strategy, and decision-making.


## How to Use Tree of Thought Prompts


As you would instruct ChatGPT to solve a problem using CoT, Dave Hubert proposed[a simpler way to implement ToT](https://github.com/dave1010/tree-of-thought-prompting) by distilling its core concepts into a single prompt.


### Original Question


Large Language Models sometimes struggle to answer more complex questions. For example:


```text
Bob is in the living room.
He walks to the kitchen, carrying a cup.
He puts a ball in the cup and carries the cup to the bedroom.
He turns the cup upside down and then walks to the garden.
He puts the cup down in the garden and then walks to the garage.
Where is the ball?


```


Even after applying CoT, GPT 3.5 gave a wrong answer when asked the above question. Now introducing a ToT-style prompt corrected the error.


### Here's how Hubert put together the ToT prompt:


```text
Imagine three different experts are answering this question.
All experts will write down 1 step of their thinking,
then share it with the group.
Then all experts will go on to the next step, etc.
If any expert realizes they're wrong at any point then they leave.
The question is…


```


Instructing ChatGPT to provide diverse chains of thought that reach a consensus at each step did the trick.


## How to Evaluate ToT Prompts Using Helicone for Better Accuracy


Since the precision of an LLM output depends strongly on how well the prompt is constructed, using well-refined ToT prompts improves its performance.


Helicone's **[Experiments](https://docs.helicone.ai/features/experiments)** help users simplify the prompt creation and optimization process. This tool provides a playground where you can safely experiment with prompts and measure their success before pushing your code to a live environment.


Speed up prompt iteration with Helicone's Experiments using these simple steps:


### Step 1: Create a new experiment


If you have an existing prompt in Helicone, you can use it as a starting point.


Otherwise, you can **create a prompt from scratch** . Then paste your ToT prompt into the prompt field.


### Step 2: Add input rows


The next step is to import golden datasets or real-world data to test your prompt on.


There are a few ways to do this. But if you don't have any request data, you can **manually add input rows** for each variable you used in your prompt.


### Step 3: Add an evaluator


An evaluator is a metric used to assess and score thequality of the model outputs for your prompts.


Helicone currently supports LLM-as-a-judge and Python evaluators. TypeScript support is coming soon!


### Step 4: Run the evaluator and see scores


Once the evaluator is created, you can run it to see the scores.


Evaluators provide feedback that allows you to adjust your prompt until you get the desired score or output. Helicone allows you to view the average scores across all your inputs, as well as the individual scores for each input.


### Step 5: Iterate on your prompt


If you are unsatisfied with the scores, create a new prompt variation, then run evaluators again.


Create as many prompts as you need until you get the desired scores.


You can always add more input rows or evaluators to your experiment for more comprehensive testing.


Above is an example of forking an existing prompt to create a new variation.


## Tree of Thought Prompting vs. Other Prompting Techniques


Compared to[other prompting techniques](https://helicone.ai/blog/prompt-engineering-tools) , ToT shows massive improvement at planning/searching problems such as the game of 24, crossword puzzles, and creative writing, according to Yao et el. (2023).


The CoT prompt only passes` 4.0%` of the benchmark tests, failing` 60%` at the first three words. The ToT prompt, on the other hand, achieved` 74%` accuracy across all game tests as seen in the table below.


Despite ToT's unprecedented success rate at strategy games, researchers see further room for LLM enhancements with better search algorithms and heuristics.


## When to Use Tree of Thought


ToT's ability to encourage LLMs to work through multiple reasoning paths simultaneously makes them applicable to a new range of problems. Besides the puzzle solvers implemented by researchers in the pioneering papers, ToT-based LLMs can be engineered to solve other complex tasks.


You can apply the Tree of Thought technique when:


- Chain of Thought doesn't work: CoT takes a linear approach to problem solving so it may give incorrect responses when faced with a complex problem
- Lookahead and strategic decision making is required
- The problem has multiple, related variables
- You need creative problem-solving


## Bottom Line


Tree of Thought framework emulates an organizational decision-making process. It allows LLMs to divide into separate but coherent thinking entities within themselves and deliberate the best thought process every step of the way.


Based on Yao et el. (2023) benchmark tests, the Tree of Thought framework/prompting is 10 times more accurate than CoT. This expands the applications of LLMs to more sophisticated tasks where planning, strategic thinking, and deliberate decision-making are essential.


## Ready to optimize your prompts? 💡


Join thousands of developers who use Helicone to monitor performance, trace reasoning paths and optimize their prompts.


### You might also be interested in


- [Chain-of-Thought Prompting: Key Techniques and Benchmarks](https://www.helicone.ai/blog/chain-of-thought-prompting)
- [How to Test Your LLM Prompts with Helicone](https://www.helicone.ai/blog/test-your-llm-prompts)
- [Prompt Evaluation Explained: Random Sampling vs. Golden Datasets](https://www.helicone.ai/blog/prompt-evaluation-for-llms)


---


### Questions or feedback?


Are the information out of date? Please[raise an issue](https://github.com/Helicone/helicone/issues) or[contact us](https://www.helicone.ai/contact) , we'd love to hear from you!
