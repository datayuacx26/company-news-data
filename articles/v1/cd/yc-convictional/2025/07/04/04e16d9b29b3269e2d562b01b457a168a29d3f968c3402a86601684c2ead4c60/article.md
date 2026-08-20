---
schema_version: "1.0.0"
document_id: "04e16d9b29b3269e2d562b01b457a168a29d3f968c3402a86601684c2ead4c60"
company_key: "yc-convictional"
company: "Convictional"
source_id: "yc-convictional-atom-2bd8494c6cc9"
canonical_url: "https://get.convictional.com/posts/the-human-ai-workforce/"
published_at: "2025-07-04T00:00:00+00:00"
first_seen_at: "2026-08-09T21:02:07.552290+00:00"
fetched_at: "2026-08-09T21:02:10.293363+00:00"
content_hash: "sha256:290c2b9f5dff97514aefa606c1e6c74d18ec94774173e44ec10c68dcbfe69fb9"
---

# The Human-AI Workforce: How Trust and AI Capability Could Reshape Our Teams

#


# The Human-AI Workforce: How Trust and AI Capability Could Reshape Our Teams


Artificial Intelligence, particularly advanced Large Language Models (LLMs) that can act as "agents," is rapidly evolving. Many of us are wondering: how will this technology change the way we work? If AI can handle more tasks, what does that mean for human jobs?


This post explores a simple mathematical model to help us think about these questions. Instead of predicting one specific future, we want to provide a way for you to explore different *possibilities* . How might the number of human workers and AI agents change as our trust in AI grows and its capabilities improve?


## Our Simple Model: The Core Ideas


We're making a few key assumptions to build our model:


1.


**Total Work (Demand):** We assume there's a fixed amount of work to be done. For this exploration, we've set this at **1000 units of work** . Think of this as the baseline number of people that would be needed if no AI was involved. This fixed number helps us see the relative impact of other changing factors.


2.


**Trust is Key ($T$):** The biggest factor is how much we *trust* AI to do tasks correctly and reliably. We represent "Trust in AI" ($T$) on a scale from near 0 (no trust, 0.01 in our charts) to near 1 (full trust, 0.99 in our charts).


3.


**Three Tiers of AI Use:** Based on this trust, work gets divided:


- **Fully Automated Tasks (AI Does It Alone):** When trust is high, some tasks ($T \\times \\text{Demand}$) are given entirely to AI agents.
- **Human-Only Tasks (Humans Do It Alone):** When trust is very low for certain tasks, or for tasks where AI isn't suitable, humans do the work directly. We model this as $(1-T)^2 \\times \\text{Demand}$. The squared term here was chosen simply for the sake of symmetry; here we are effectively saying that of the $(1-T)$ tasks that are not automated via Agents, $(1-T)$ of those are entirely completed by humans. In reality, these tiers would be far more complex (see the limitations section below for more on this).
- **AI-Assisted Tasks (Humans Review AI Work):** This is the middle ground. AI does the initial work, but a human reviews, guides, and approves it. The amount of work here is $T(1-T) \\times \\text{Demand}$.


4.


**Human Review Power ($\\phi$):** When humans shift from *doing* tasks to *reviewing* AI-generated work, they can often oversee multiple AI outputs. "Human Review Power" ($\\phi$) represents how many AI-completed tasks one person can effectively review and guide in the time it would have taken them to do one task manually. A $\\phi$ of 5 means one human can oversee 5 AI agents/tasks.


5.


**AI Agent Speed/Efficiency ($p_{A}$):** This parameter, "AI Agent Speed/Efficiency" ($p_{A}$), tells us how productive an AI agent is. If $p_{A} = 2$, one AI agent can do the work of two humans (or twice as fast).


## The Math (Simplified)


For a total demand $D$ (fixed at 1000 in our interactive charts):


$ H_{\\text{total}} = D(1-T)\\left(1-T\\left(1+\\frac{1}{\\phi}\\right)\\right) $


$ A_{\\text{total}} = \\frac{DT(2-T)}{p_A} $


## Important Limitations


This is a simplified model! It doesn't capture everything:


- It doesn't account for new jobs created by AI (e.g., AI trainers, ethicists, AI system builders).
- It assumes demand ($D$) is constant. AI could increase demand or create new types of demand.
- "Trust" ($T$) is a single number, but in reality, it's complex and task-specific.
- The parameters ($\\phi$, $p_{A}$) might change as AI evolves or as we get better at working with it.


**Assumptions about Task Allocation:** The way tasks are divided between full AI automation, AI-assistance with human review, and human-only work is based on simplified functions of Trust ($T$) – specifically, the $(1-T)$ factor driving human-involved work and the $T(2-T)$ factor for AI-driven work. These specific mathematical forms ($H_{\\text{total}} = D(1-T)(1 + \\frac{1}{\\phi})$ and $A_{\\text{total}} = \\frac{DT(2-T)}{p_{A}}$) were chosen for their illustrative properties and tractability within this model. In reality, how tasks are allocated as trust evolves is far more complex and likely not static or describable by such simple universal ratios. For instance, the current model structure implies specific symmetries in task distribution based on trust. A more granular model might introduce additional parameters to capture these nuances, but this would significantly increase complexity and move beyond the scope of this exploratory post. Our aim here is to provide a foundational model to spark thinking, not to perfectly predict the future.


Despite these limitations, the model can help us see potential trends and understand the interplay between these factors.


## Plot 1: Workforce Dynamics as Trust in AI Grows


This first chart shows how the "Human Workforce Needed" (black line) and "AI Agents Deployed" (yellow line) might change as "Trust in AI" (shown on the horizontal axis) increases from nearly none (0.01) to almost full (0.99). Remember, the total demand for work is fixed at 1000 units.


Use the sliders below to see how things change:


- **Human Review Power ($\\phi$):** If you look at the formula for total human workforce again, you might notice the $\\frac{1}{\\phi}$ term, which is the only place human review capability comes into play. This means that when a single human can review more than 1 agent's work, the number of humans in the workforce becomes almost entirely determined by Trust. This means that you may not see much change, particularly as $\\phi$ starts get large (e.g. > 5).
- **AI Agent Speed/Efficiency ($p_{A}$):** Again, reviewing our formulas for total humans and agents above, you might also notice that $p_{A}$ doesn't appear in the human workforce calculation, implying that the efficiency of agents doesn't change how many humans are needed to oversee and work with them. This conclusion actually draws implicitly on our assumption that the cost of agents approaches the cost of energy, making them negligble in cost compared to an equivalent human expert - without this assumption, would require a parameterization of cost, in which case agent efficiency may tip the scales in favour of humans when it is very low. In our model, agent speed/efficiency only determines how many agents are ultimately needed given a trust, T.


Human Review Power (` Φ` ): 5.0


AI Agent Speed/Efficiency (` pA` ): 1.0


## Plot 2: Mapping the Human Workforce Landscape


This second visualization is a 3D surface plot showing "Human Workforce Needed" (height) vs. "Trust in AI" (x-axis) and "Human Review Power" (y-axis). The surface color indicates the "AI-to-Human Ratio," influenced by the "AI Agent Speed/Efficiency" ($p_{A}$) slider.


- Purples/cool colors indicate a lower ratio (fewer AI agents per human).
- Yellows/warm colors indicate a higher ratio (more AI agents per human).


The surface below looks relatively uninteresting, which is again due to that $1/\\phi$ term in the human workforce total. Once $\\phi$ is greater than 1, it starts to have very little impact on the human workforce total putting all accountable variance into the Trust variable.


AI Agent Speed/Efficiency (` pA` ) (affects Plot 2 color): 1.0


## Plot 3: The AI Agent Deployment Landscape


This 3D surface plot shows "AI Agents Deployed" (height) vs. "Trust in AI" (x-axis) and "AI Agent Speed/Efficiency" ($p_{A}$, y-axis). The surface color represents the "AI-to-Human Ratio," influenced by the "Human Review Power" ($\\phi$) slider.


- Purples/cool colors indicate a lower AI-to-Human ratio.
- Yellow/warm colors indicate a higher ratio.


As we saw above in the human workforce surface plot, human review power has limited effect on human workforce (and hence agent / human ratio) once it scales beyond 5(ish); which you can see in the changing colors as you adjust the slider.


The surface itself is relatively 'flat' once agents become more productive than humans as we assume fixed demand. A lot of posts talk about 'fleets' of agents being deployed by organizations - which may be true if agents are either less productive than humans, or demand keeps pace with increasing trust. In reality, demand elasticity may lead to no more agents than humans being deployed in a majority of scenarios. Note, we're assuming intelligent foundation models - effectively 'AGI' and are not talking about ensembles of small models working in consensus or via path exploration. Although these may be facets of any sufficiently intelligent agent, but that is beyond the scope of this post.


Human Review Power (` Φ` ) (affects Plot 3 color): 5.0


## Plot 4: Workforce Configurations - Humans vs. Trust and Agents per Human


This final 3D surface plot directly visualizes the relationship between the "Human Workforce Needed" ($H_{\\text{total}}$, Z-axis height), "Trust in AI" ($T$, X-axis), and the "Agents per Human Ratio" ($A_{\\text{total}} / H_{\\text{total}}$, Y-axis).


The surface itself is generated by exploring different combinations of "Trust" ($T$) and "Human Review Power" ($\\phi$). The color of the surface corresponds to this underlying "Human Review Power" ($\\phi$) value that generated each point:


- Cooler colors (e.g., purples/blues) typically represent lower $\\phi$ values (humans can review fewer AI tasks).
- Warmer colors (e.g., greens/yellows) represent higher $\\phi$ values (humans are more leveraged in their review capacity).


The "AI Agent Speed/Efficiency" ($p_{A}$) slider will primarily reshape the surface along the Y-axis (Agents per Human). Higher AI speed means fewer agents are needed for a given workload, which generally decreases the $A_{\\text{total}}$ component of the ratio, thus affecting the Y-value. Observe how this parameter stretches or compresses the surface in that dimension.


This plot allows you to see how different levels of human review power ($\\phi$, shown by color) create different trade-off surfaces. As you adjust the 'AI Agent Speed/Efficiency' ($p_{A}$) slider, observe how the 'Agents per Human' ratio (Y-axis) changes, and how that corresponds to the 'Human Workforce Needed' (Z-axis) for any given 'Trust' level (X-axis).


You might be wondering why we have such a weird shape? The reason is that both agents (and hence agents per human) and humans are functions of (primarily) Trust and so we 'solve' the parameter combinations at each point, leading to the limited surface defined by what is allowed by the model.


AI Agent Speed/Efficiency (` pA` ) (affects Plot 4 Y-dim & shape): 1.0


## Evolutionary Paths: How Trust and Capability Might Change Over Time


The model above gives a hypothetical relationship between trust, productivity and review metrics but says nothing about how trust might evolve. That specifically, is beyond the scope of this post, but we can look at hypothetical functions of time for trust, productivity and review that show how human staffing might change. You can think of this as a path being traced across the surfaces above.


We may see unpredictable changes such as:


- Trust increases at first, but sees frequent pull backs due to 'incidents'; these pull-backs could vary in amplitude, 'damping' (e.g. how quickly it pulls back and rebounds) and timing
- Trust increases in an accelerated way, such as polynomial, exponential, etc.
- Trust evolves in a sinusoidal pattern, increasing then decreasing back and forth as human trust in agent capability and judgement oscillates while increasing
- Human review capability may actually decrease over time as model's intelligence surpasses human's leading to longer periods required to review an agent's work
- On the other hand, new tools may be made available allowing one human to review more agent's work even as the complexity and stakes of tasks increase


Below we show different potential path parameterizations of trust, review capability, and productivity. For each scenario, we display two charts:


1. A 2D graph showing how parameters (trust, review capability, etc.) evolve over time
2. A 2D graph showing how workforce composition (humans and AI agents) changes as a result


### Accelerating Trust × Static Review Capability


In this scenario, trust in AI accelerates over time (growing slowly at first, then more rapidly), while human review capability remains constant. This represents a world where people become increasingly comfortable with AI systems as they prove themselves, but the fundamental human ability to review AI work doesn't change significantly.


### Accelerating Trust × Decreasing Review Capability


Here, trust accelerates as in the previous scenario, but human review capability decreases over time. This might occur if AI systems become increasingly complex and sophisticated, making their work harder for humans to effectively evaluate. At a certain point, reviewing AI output becomes more cognitively demanding than doing the work directly.


### Oscillating Trust × Static Review Capability


This scenario models trust that follows a cyclical pattern while overall increasing. The oscillations represent periodic "trust crises" where AI incidents or failures temporarily reduce trust, followed by recovery periods where trust rebuilds. This pattern acknowledges the reality that progress often isn't linear, especially with emerging technologies.


### Linear Trust × Decreasing Productivity


In this scenario, trust increases linearly, but AI productivity decreases over time. This counterintuitive situation might emerge if AI systems are initially applied to "easy" tasks where they excel, but are gradually tasked with more complex work where their relative advantage over humans diminishes.


### Trust with Incidents × Static Review Capability


This final scenario models trust that generally increases but experiences sudden dramatic drops following specific "incidents" - major AI failures or errors that damage trust. Each incident causes a sharp drop followed by a recovery period. This pattern reflects how catastrophic events can cause lasting but not permanent shifts in trust.


## Conclusion


The mathematical model we've explored is a theoretical tool, a way to structure our thinking about the complex interplay between human expertise and artificial intelligence. It doesn't predict one single future but rather illuminates a landscape of possibilities. The trajectory we ultimately follow will be defined not by abstract forces, but by the specific systems we design to manage information, automate complexity, and augment human judgment.


This is where theory meets practice. The abstract challenge of balancing trust ($T$), human oversight ($\\phi$), and AI efficiency ($P_\\text{A}$) is an evolving problem that we are seeing play out in real time as organizations adopt AI. Convictional is building the essential infrastructure for this new era of work, focusing directly on the operating system for aligning the increased productivity of humans and AI to organizational goals.
