---
schema_version: "1.0.0"
document_id: "3189b635d2a76e4dc1de047ac87e8b00f30fd743775bb14ac5d769aa7e5a5cbf"
company_key: "international-business-machines-corporation-common-stock"
company: "International Business Machines Corporation"
source_id: "international-business-machines-corporation-common-stock-news-import-da8ebfcd6a45"
canonical_url: "https://research.ibm.com/blog/hari-krovi-differential-equations"
published_at: "2026-08-03T13:00:00+00:00"
first_seen_at: "2026-08-04T00:22:03.300621+00:00"
fetched_at: "2026-08-04T05:15:07.724576+00:00"
content_hash: "sha256:782fd86f788a2313a35d945d644171349b4b37634320313d515528a28b5fb84a"
---

# How IBM is searching for quantum advantage in differential equations

[Differential equations](https://research.ibm.com/blog/differential-equations) study the mathematics of change. They describe everything from the waves crashing against a shoreline to the roiling plasma on the surface of the sun. They are immensely valuable with applications in drug discovery, financial modeling, structural engineering, and beyond. But as the systems they model grow in size and complexity, classical computing methods falter.


Recent and forthcoming work from a group led by IBM senior researcher Hari Krovi suggests that quantum computing could soon expand the scale at which we can analyze and solve certain types of differential equations. His team is developing new quantum algorithms that, in specific cases, offer potential for significant speedups over state-of-the-art classical approaches. These advances are part of a growing effort to achieve quantum advantage in solving differential equations—a long-standing goal in quantum computing.


Krovi’s group has made steady progress developing algorithms that use differential equations to efficiently crack useful problems that challenge classical computers. In a recent conversation, he explained how quantum computing might unlock new ways of solving differential equations, where quantum advantage might emerge, and what recent advances could mean for real-world applications.


**Why are differential equations important for quantum computing applications?**


Differential equations appear across a huge range of scientific and engineering domains. You see them in fluid dynamics, in plasma physics—an active research area for us—and in financial mathematics with models like the Black‑Scholes equation, a mathematical framework for pricing derivatives in finance. They show up in large electrical networks when you’re interested in time dynamics rather than steady-state behavior. Biology offers many examples as well—predator-prey systems, the spread of viruses. All these things are modeled as differential equations, but those equations can be challenging to solve with classical methods.


**Why do we believe quantum computers can achieve speedups over classical methods in solving differential equations?**


The speedup in quantum essentially comes from mapping these discretized differential equations to linear systems used to solve for energy or power in an electrical circuit, or using Hamiltonian simulation problems like those for finding the ground-state energy of a molecular system.


We can get a quantum speedup for these problems because we can encode the data, in a sense, into the amplitudes of a quantum state. Since Shor’s algorithm, we’ve known that certain operations on the amplitudes will give you an exponential speedup. You can’t use it for everything, but if the structures line up the right way, you can get exponential speedups for different kinds of problems.


**What role does the HHL algorithm play in quantum differential equation solvers?**


HHL—which stands for the algorithm’s three inventors Harrow, Hassidim, and Lloyd—is a quantum algorithm for solving systems of linear equations of the form Ax⃗=b⃗A\\vec{x} = \\vec{b}


A


x


=


b


, where AA


A


is a large matrix, x⃗\\vec{x}


x


is a large vector, and b⃗\\vec{b}


b


is another large vector. Classically, solving this scales with the dimension of AA


A


. Let's call that NN


N


. Quantumly, if you satisfy certain conditions, it can scale as log⁡N\\log N


lo g


N


, which is where the exponential advantage comes from.


This exponential advantage over classical methods can be translated to differential equations by mapping them to systems of linear equations and using HHL (and it’s improvements).


**Let’s shift gears to some of your latest research: To start, what are differential algebraic equations, and why are they important for modeling electrical circuits?**


Differential algebraic equations (DAEs) are essentially differential equations with algebraic constraints. RLC circuits are the quintessential example of DAEs. Current or voltage changes with time, and it has to satisfy specific constraints through this evolution. You can represent that as a DAE.


These equations can be really complicated, but you can use something called “projector-based methods” to write them in a way that separates the differential and algebraic part, and that’s where we use quantum algorithms. We solve the differential equation. Then we solve the algebraic equation, and we coherently combine them. After you get the solution to the whole thing, you can measure properties of interest.


**What did your RLC work show, and how does that relate to engineering tools like SPICE (Simulation Program with Integrated Circuit Interface)?**


Our latest research on RLC circuits is the first step toward a quantum circuit simulation algorithm analogous to[SPICE](https://bwrcs.eecs.berkeley.edu/Classes/IcBook/SPICE/) , a widely used tool for simulating electrical circuits. We showed that if you only have RLC elements—resistors (R), inductors (L), capacitors (C)—and sources of current and voltage, you can compute quantities like currents, voltages, energy, and power over time. We also showed that if we do this using quantum algorithms, then the runtime scales logarithmically in the number of components, whereas classically the runtime for any SPICE simulation algorithm typically scales linearly.


That’s a significant speedup, but this is just the first step. Our latest work only has linear elements. The classical algorithms in SPICE can simulate many other kinds of outputs, including nonlinear elements, which we plan to investigate next.


**Another area your group has been focusing on involves differential equations with “memory.” What are integral differential equations?**


Typically, when you look at the usual linear differential equations, the rate of change at a certain time depends only on the state at that exact moment. Integral differential equations modify this framework by adding a term that integrates over all previous solutions. So the equation kind of “remembers” the previous solutions. This models a lot of interesting phenomena, like in epidemiology or surface water waves under certain conditions.


In this research, we aren’t focusing on a specific application like we did for DAEs, but the core idea is that there’s memory in the equation. Just as with the DAEs, we convert this system into a purely differential equation in higher dimensions, and eventually we get a quantum speedup.


**When will algorithms like this run on real quantum hardware?**


We’re currently working to estimate the resources needed to run this algorithm on near-term and fault-tolerant devices. For certain problem instances that are hard classically, the algorithm is essentially a form of Hamiltonian simulation. This could mean that the algorithm can be implemented on near-term or early fault-tolerant devices. We’re investigating the circuit depth one can go to and the quantum advantage we can get at this depth and instance size.


**We’ve heard a lot about the Navier-Stokes equation—Are we on track to solve it?**


Navier-Stokes is a famous fluid dynamical equation. Anyone who does fluid dynamics has worked with it at some level. There are simpler versions, like the Viscous Burgers equation, and more complicated versions like magneto-hydrodynamics in plasmas. But Navier-Stokes is a very central equation that we'd like to see solved.


We can already solve Navier-Stokes quantumly in cases of weak non-linearity. But when things get turbulent, we don’t know how to solve it efficiently, and that's the regime that people are interested in. Turbulence is quantified by something called the Reynolds number. When the Reynolds number is very large, you're in a highly turbulent regime.


I wouldn’t say we are extremely close to being able to solve problems in that turbulent regime. Right now we can only solve it in the very mild regime, something called laminar flow, so we have some ways to go. One thing we’re interested in doing is building tools that are independent of any particular application. And one of those tools we'd like to build is something that can go beyond mild nonlinearity for nonlinear differential equations, which will be applicable to Navier-Stokes and other equations too.


**What are the main limitations of quantum approaches to differential equations?**


I would say **data loading** and **classical information extraction** . For whatever differential equation you’re trying to solve quantumly, you need to be able to encode the initial states into a quantum computer. For whatever quantity you’re interested in, you need a way to extract that quantity. I’m currently collaborating with Andrew Childs and others at University of Maryland to try and figure out what are the limitations of these quantum algorithms for differential equations.


**What are the most promising directions for future research in quantum differential equations?**


We’re going to continue exploring what you might call the “quantum SPICE” direction—RLC circuits, circuits with non-linear elements, etc. Then we want to find applications for these integral differential equations, maybe in materials or epidemiology. We also have projects on plasma physics and fluids. One area we haven’t gotten into yet is biology, where differential equations play a big role. There’s a lot to explore, but broadly, these are the different directions we’re interested in.
