---
schema_version: "1.0.0"
document_id: "cfa5b476e712a0438bc3b1c4b7330ba8b1160f4db8fd770ea3e8802640fd0f14"
company_key: "yc-empirical-health"
company: "Empirical Health"
source_id: "yc-empirical-health-news-import-485d82c6bcdb"
canonical_url: "https://www.empirical.health/blog/ai-math-sphere-packing/"
published_at: "2026-08-02T00:00:00+00:00"
first_seen_at: "2026-08-12T02:50:03.415509+00:00"
fetched_at: "2026-08-12T02:50:06.156943+00:00"
content_hash: "sha256:752e885972e5b98526370be3906008928f1081144ad3f1ebcd5f03e1ed7a5f7a"
---

# Stacking oranges in 24 dimensions

OpenAI announced that they’ve solved[ten open mathematical problems](https://openai.com/index/ten-advances-in-mathematics/) , including an improved Cohn-Elkies bound for sphere packing.


I briefly worked with Henry Cohn on computational experiments for sphere packing as an undergraduate. Sphere packing is notorious for hard open questions—Maryna Viazovska earned a Fields Medal in 2022 for work in dimensions 8 and 24—but you may be surprised at how approachable some of the basic results are. An amateur mathematician can get close to understanding both OpenAI’s latest advance and the work that led to the 2022 Fields Medal. Sphere packing also contains beautiful structures that link seemingly unrelated areas: discrete geometry, Fourier analysis, linear programming, and string theory.


*Sphere packing features highly symmetrical structures like the E8 lattice, visualized here with a Coxeter projection.*


People more qualified than me ought to judge whether this represents a “Move 37” moment for math, but in the meantime, it’s worth appreciating some of the inherent beauty of this field and the new results. Read on for a quick primer.


## Briefly defining sphere packing


The sphere packing problem asks a deceptively simple question: what’s the largest fraction of space you can fill with equal-sized balls that don’t overlap? One motivation for studying this problem is[error correcting codes](https://en.wikipedia.org/wiki/Error_correction_code) , since an optimal packing is also an optimal code.


In 2D, you can think of sphere packing as cutting circles from rolled dough as efficiently as possible:


*Sphere packing in two dimensions: surprisingly delicious!*


In 3D, sphere packing is a bit like stacking oranges in the grocery store. The intuitive choice, a pyramid, turns out to be optimal. However, *proving* optimality only happened in 1998 (the solution was guessed by Kepler in 1611).


Beyond three dimensions, sphere packing gets less intuitive.


## Density is jagged in high dimensions


What happens if we plot the best-known sphere packing density in each dimension? The result is surprisingly jagged:


*Best packing known (red) against the Cohn-Elkies linear programming bound (tan). Packing densities from Henry Cohn’s[survey of Maryna Viazovska’s proof](https://arxiv.org/abs/1611.01685) , which is excellent and worth reading.*


There’s no obvious way to interpolate a point from its neighbors. Knowing the answer in dimension 8 tells you almost nothing about 7 or 9. Not only that, our 3D intuition that good packings are crystalline fails starting in dimension 10, where the best known packing is 8% denser than the best known lattice.


Dimensions 8 and 24 are special: the best known packings (the E8 and Leech lattices) match the Cohn-Elkies bounds exactly. These two structures are highly symmetrical, and are two of the few to have been proven optimal. We don’t have any proofs of optimality *except* in dimensions 1, 2, 3, 8, and 24. Past dimension 36, our ignorance gets worse. The best upper bound was at 2^(-0.599n) until this year.


## How to visualize symmetries in high dimensions


We can’t really form a mental picture of eight dimensions, but we can “see” symmetries with surprisingly simple tools.


One such tool is a Gram matrix: a small table that captures the inner product of every pair of basis vectors. The diagonal of the Gram matrix is the squared lengths, and the off-diagonal entries encode the angles between basis vector. Symmetry in high dimensions shows up as repetition in the table.


*Three lattices and the Gram matrices of their bases. Equal values are colored alike to make symmetry is more visible.*


For example, in the gram matrix for the face-centered cubic lattice (the stack of oranges), every diagonal entry is 2 and every off-diagonal entry is 1. You can permute the basis vectors freely without changing the table. If you sheear that lattice slightly, the entries start appearing random, with no apparent symmetry.


In E8’s lattice, every basis vector has squared length 2. Every pair of vectors is either perpendicular or at 120 degrees. What’s more, every vector in E8 has even squared length, so all the inner products are integers. The fundamental cell also has volume 1, and those two facts together make E8 its own dual lattice. Its 240 shortest vectors are all the same length, so each sphere in the packing touches 240 others, which is the most possible in eight dimensions. The packing density ends up as π⁴/384 = 0.2537.


The projection at the top of this article is an alternate way to see the shape of the E8 lattice. The 240 vectors are arranged in eight concentric rings of thirty.


## What Maryna Viazovska proved in 2016


Prior to 2016, no proof of optimality had been known for any dimension above three. On Pi Day 2016, Maryna Viazovska posted a proof to the arXiv showing that E8 had the optimal density in dimension 8. Cohn, Abhinav Kumar, Stephen D. Miller, Danylo Radchenko, and Viazovska extended the technique to dimension 24 within a week, proving that the Leech lattice was optimal there. Viazovska received the Fields Medal in 2022.


## The connection between harmonic analysis, linear programming, and sphere packing


*This next section gets slightly more technical*


Sphere packing is discrete geometry, but the proof technique uses harmonic analysis and linear programming. These three topics would at first seem to be unrelated.


Te Poisson summation formula links discrete geometry to harmonic analysis. It says that summing a function over a lattice equals summing its Fourier transform over the dual lattice (up to a volume factor). Since the Fourier transform diagonalizes translation, it’s the natural tool for anything periodic.


Cohn and Elkies turned that into an upper bound in[2003](https://arxiv.org/abs/math/0110009) . Suppose you can find a function ff


f


on Rn\\mathbb{R}^n


R


n


, not identically zero, with three properties:


f(0)=f^(0)>0f^(t)≥0for every tf(x)≤0for ∣x∣≥r\\begin{aligned} &f(0) = \\hat{f}(0) > 0 \\\\ &\\hat{f}(t) \\geq 0 \\quad \\text{for every } t \\\\ &f(x) \\leq 0 \\quad \\text{for } |x| \\geq r \\end{aligned}


​


f


(


0


)


=


f


^


​


(


0


)


>


0


f


^


​


(


t


)


≥


0


for every


t


f


(


x


)


≤


0


for


∣


x


∣


≥


r


​


Then no sphere packing in Rn\\mathbb{R}^n


R


n


beats the density of balls of radius r/2r/2


r


/2


.


Each of these three conditions is an inequality that depends on ff


f


linearly. Optimizing a linear objective under linear inequality constraints is… linear programming. What’s unusual is that we’re optimize not a list of numbers, but a continuous function.


To actually compute a bound, we need shrink the search space. Cohn and Elkies look only at functions of the form p(∣x∣2)e−π∣x∣2p(|x|^2)e^{-\\pi|x|^2}


p


(


∣


x


∣


2


)


e


−


π


∣


x


∣


2


, a polynomial times a Gaussian. The Gaussian form makes it tractable since e−π∣x∣2e^{-\\pi|x|^2}


e


−


π


∣


x


∣


2


is its own Fourier transform, so f^\\hat{f}


f


^


​


is another polynomial times that same Gaussian, with coefficients you can compute from pp


p


.


When you optimize ff


f


numerically, the bound matches the density of E8 in dimension 8 and the Leech lattice in dimension 24. Why? Cohn and Elkies conjectured that exact “magic functions” existed in those two dimensions. For E8, the magic function has to vanish at every distance √(2k) for k = 1, 2, 3, and so on, and its Fourier transform has to vanish at the same places. All the roots except the first have to be double roots, so the function never changes sign. Controlling a function and its Fourier transform simultaneously is what the uncertainty principle forbids, so this is a fundamental obstruction rather than a technical one. Maryna Viazovska’s proof involved a very clever mathematical trick involving sin^2 that solved this problem.


## What OpenAI’s model actually proved


The first chapter of OpenAI’s[249-page anthology](https://cdn.openai.com/pdf/ten-proofs-oai.pdf) is about this linear program, the Cohn-Elkies bounds. It answers not “what is the densest packing?” but “how good can this method possibly get?” The Cohn-Elkies bound in dimension dd


d


decays like 2−αd2^{-\\alpha d}


2


−


α


d


, and α\\alpha


α


converges exactly to 12log⁡2(2π/e)=0.6044…\\tfrac{1}{2}\\log_2(2\\pi/e) = 0.6044\\ldots


2


1


​


lo g


2


​


(


2


π


/


e


)


=


0.6044


…


That improves the general high-dimensional exponent from 0.59906 to 0.6044, the first improvement since 1978.


## All that for oranges, huh?


The next time you see a stack of oranges in the grocery store, hopefully it will remind you of some of the deep connections here to fourier analysis, linear programming, and highly symmetric structures. Whatever the future of AI in pure math, this is an interesting and beautiful field, and something that is hopefully a bit more approachable than it seemed initially.


## Get your free 30-day heart health guide


Evidence-based steps to optimize your heart health.
