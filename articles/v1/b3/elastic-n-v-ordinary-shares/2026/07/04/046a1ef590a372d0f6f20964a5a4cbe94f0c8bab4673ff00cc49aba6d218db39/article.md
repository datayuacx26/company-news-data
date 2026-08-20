---
schema_version: "1.0.0"
document_id: "046a1ef590a372d0f6f20964a5a4cbe94f0c8bab4673ff00cc49aba6d218db39"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/vector-quantization-auto-calibration-elasticsearch"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-21T17:41:41.822757+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:36eca27c5f97fd6617bd0b067d274af13dd06f669bf49212e2395e064d45c15e"
---

# How Elasticsearch auto-tunes vector quantization to hit your recall target

Try out vector search for yourself using this[self-paced hands-on learning](https://www.elastic.co/demo-gallery/vector-search) for Search AI. You can start a[free cloud trial](https://cloud.elastic.co/registration?onboarding_token=search&cta=cloudregistration&tech=trial&plcmt=cross%20module&pg=search-labs) or try Elastic on your[local machine](https://github.com/elastic/start-local?cta=local-machine&tech=github&plcmt=cross%20module&pg=search-labs) now.


## What makes a good vector store?


Avector store that achieves good performance without tuning is more valuable than one that requires expert tuning. In fact, our contention is a data store that can be coaxed to exceptional performance by an expert who spends a week hand-tuning it is less useful than one that[beats](https://www.elastic.co/beats) it consistently out of the box. In other words, easily achieving good performance is a first class property, not a nice to have. We can see this clearly in our telemetry. The great majority of users will never tune the internals of[vector search](https://www.elastic.co/search-labs/blog/introduction-to-vector-search) at all, and why should they: it is just an enabler for what they're trying to build.


This is the imperative behind features like auto-calibration. The system as a whole should look at your data and your quality target and choose good parameters for you. Indeed we think this is a win-win, since it has far more nuanced information available to it to make these choices than we expose.


To make "good performance" precise, it helps to name the three attributes that characterize any vector search system, because they trade off against one another and you can't talk about one without fixing the others:


1. Performance:throughput (QPS),latency , and so on.
2. Hardware cost: a fair comparison always holds cost fixed. It's trivial to buy your way to more QPS or better recall by throwing hardware at the problem; the interesting question is what you achieve *per dollar* .
3. Search quality: recall,[nDCG](https://www.elastic.co/search-labs/blog/evaluating-search-relevance-part-1#main-takeaways-&-next-steps) , and related measures of whether you're returning the right results.


The three form a frontier. Push one and, at fixed budget, you pay in another. Any honest comparison of approaches pins two down and measures the third. What we describe in this post is the mechanism we're introducing to pick[quantization](https://www.elastic.co/search-labs/blog/better-binary-quantization-lucene-elasticsearch) parameters for a fixed recall budget. It is a step on a longer journey towards a vector store that configures itself well across the board.


### Why recall is the right quality metric for vector search


Search quality is tricky, because the "right" results depend on relevance labels you usually don't have at index time. So we lean on recall as a safe proxy. The argument is simple: recall measures how well the approximate index reproduces the results of exact search over the *same[embeddings](https://www.elastic.co/what-is/vector-embedding)* . If recall is high, you have not degraded search quality relative to what the underlying model can do; you can be confident you’ve faithfully preserved the baseline. You might still wish for a betterembedding model, we've got you[covered](https://www.elastic.co/search-labs/blog/jina-embeddings-v5-text) , but that's a separate concern from the index not damaging what the model already gives you.


This is why controlling recall matters so much, and why you should be wary of any system that doesn't reliably control it. If a vendor can't control recall, they can silently degrade your search experience, achieving impressive QPS numbers while quietly returning worse results, and you'd have no way to know without a labeled evaluation set. The method in this post is about maximizing performance while keeping a firm, predictable grip on quality **.**


### Why vector quantization parameters must be chosen at index time


What makes the problem genuinely hard is that vectors are quantized *as they are indexed* , so the parameters that govern quality (how many bits, how deep to rerank, whether to[precondition](https://www.elastic.co/search-labs/blog/robust-optimized-scalar-quantization) ) have to be evaluated before we've seen the data laid out in its final form. We can't index everything, measure recall, and iterate; by then the quantization is baked in.


So we need to estimate what we'll need from a small sample, cheaply and in advance. Fortunately the[Elasticsearch](https://elastic.co/elasticsearch) gives us natural moments to do this: segment merges are exactly such an opportunity. When segments are combined we have to rewrite the data anyway and can assess the data and (re)choose parameters. And as we'll see, models fit to small random samples give excellent estimates of the quantities we actually need to control. They’re typically good enough to set parameters once, with a small margin, and trust them as the index grows.


## How vector quantization affects nearest-neighbor recall


With that motivation in place, let's start to dig into the details.


Vector quantization is a critical component for making approximate nearest-neighbor (ANN) search affordable at scale; it's an area we've[innovated](https://www.elastic.co/search-labs/blog/scalar-quantization-optimization) in the past. Instead of storing and comparing full-precision embeddings, we store a lossy, compressed representation and search over that. The catch is the one above: lossy representations move distances around, so the "nearest" neighbors under quantized distances are not always the true nearest neighbors and recall suffers.


The standard fix is to over-retrieve and rerank. We use the cheap quantized distances to pull back the top nn


n


candidates, then recompute exact distances for those nn


n


and keep the best kk


k


. As long as the true top- kk


k


are present somewhere in the retrieved top- nn


n


,[reranking](https://www.elastic.co/search-labs/blog/elastic-semantic-reranker-part-1) recovers them exactly.


Reranking isn’t free, we have to fetch high precision vectors from disk. However, we can precisely characterize the performance of reranking based on hardware characteristics alone. This reframes the whole problem. The question is no longer "how much does quantization distort distances?" in the abstract, but something which relates back to the attributes we care about:


> Given a quantization scheme with some error magnitude, and a rerank budget of nn
>
>
> n
>
>
> candidates, what recall@ kk
>
>
> k
>
>
> should we expect. As an immediate consequence, what is the *cheapest* set of parameters that hits our recall target?


This post derives a model that answers exactly that. The core of it is a single, surprisingly clean idea: if we can characterize the *distribution of distances to the kk k -th nearest neighbor* , and we have a model of the *quantization error distribution* , then we can compute expected recall after reranking in closed form (up to a one-dimensional integral). Everything else – bit counts, rerank depth, whether to precondition – becomes a search over a model we can fit cheaply from a small sample, instead of an expensive empirical sweep over full indices built with those parameters.


We build it up to this in three stages: the geometry of nearest-neighbor distances, the scaling law that falls out of it, and then the recall model that ties quantization error to recall given a reranking budget. Be warned, the following gets a little bit involved, but to give you intuition about what is happening see the video below.


Illustration of how quantizing and rerank affects recall


## Quantization error vs. the nearest-neighbor distance gap


Fix aquery qq


q


and rank the database vectors by their true distance to it: R(1)≤R(2)≤…R_{(1)} \\le R_{(2)} \\le \\dots


R


(


1


)


​


≤


R


(


2


)


​


≤


…


, so R(i)R_{(i)}


R


(


i


)


​


is the distance to the ii


i


-th nearest neighbor. Reranking the top nn


n


succeeds for the true kk


k


-th neighbor whenever it is not pushed past rank nn


n


by quantization noise.


Two competing quantities govern this:


- The quantization error that is essentially *fixed* for a given scheme and dataset: it depends on the embeddingdimension , the vector distribution, and the number of bits, but not on how big the index is.
- The criticality gap Δk,n=R(n)−R(k)\\Delta_{k,n} = R_{(n)} - R_{(k)}


Δ


k


,


n


​


=


R


(


n


)


​


−


R


(


k


)


​


, which is the distance between the kk


k


-th and the nn


n


-th nearest neighbor. This is the margin we have to absorb error. Crucially, it *shrinks as the index grows* : pack more vectors into the same region and neighbors crowd together.


There’s a detail here we’ll gloss over for the sake of presentation: for IVF style indices, we’re quantizing the residual from a cluster’s centroid. This does in fact couple the quantization error to the index size, but we can handle it much the same way we handle the distance to the ii


i


-th nearest neighbor.


For reranking to recover the recall lost to quantization, we need the error to only rarely exceed the gap. If we can write down the distribution of Δk,n\\Delta_{k,n}


Δ


k


,


n


​


and the distribution of the error, we can make that statement quantitative. The first job is to estimate the distribution of nearest-neighbor distances.


## Deriving the nearest-neighbor distance distribution


Real embeddings don't fill their ambient space; they concentrate on a lower-dimensional[manifold](https://en.wikipedia.org/wiki/Manifold) . Near a query, though, we can make a mild local assumption: in a small neighborhood Ω⊂Rd\\Omega \\subset \\mathbb{R}^d


Ω


⊂


R


d


around the query, the data density is roughly uniform. Here dd


d


is the intrinsic dimension of the manifold; it is unknown and generally far smaller than the embedding dimension. How to estimate it is the subject of Section 4.


Let X={Xi}\\mathcal{X} = \\{X_i\\}


X


=


{


X


i


​


}


be the NN


N


vectors falling in Ω\\Omega


Ω


, modeled as[i.i.d.](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables) uniform on Ω\\Omega


Ω


, and define the distance from qq


q


to its nearest neighbor:


R=min⁡Xi∈X∥Xi−q∥.R = \\min_{X_i \\in \\mathcal{X}} \\lVert X_i - q \\rVert\\text{.}


R


=


X


i


​


∈


X


min


​


∥


X


i


​


−


q


∥


.


To get the distribution of RR


R


we use the standard order-statistics trick: rather than ask where the minimum is, ask for the probability it exceeds some radius rr


r


. The event {R>r}\\{R > r\\}


{


R


>


r


}


is exactly the event that every point lands outside the dd


d


-ball centered on the query Br(q)B_r(q)


B


r


​


(


q


)


.


A single point lands inside Br(q)B_r(q)


B


r


​


(


q


)


with probability equal to the ratio of the ball's volume to the region's volume VV


V


P(Xi∈Br(q))=ωd rdV,ωd=πd/2Γ(d/2+1),(1)P(X_i \\in B_r(q)) = \\frac{\\omega_d\\, r^d}{V}, \\qquad \\omega_d = \\frac{\\pi^{d/2}}{\\Gamma(d/2 + 1)}\\text{,} \\tag{1}


P


(


X


i


​


∈


B


r


​


(


q


))


=


V


ω


d


​


r


d


​


,


ω


d


​


=


Γ


(


d


/2


+


1


)


π


d


/2


​


,


(


1


)


where ωd\\omega_d


ω


d


​


is the[volume](https://en.wikipedia.org/wiki/Volume_of_an_n-ball) of the unit dd


d


-ball. (We assume NN


N


is large enough that the relevant rr


r


is small, so the ball doesn't spill outside Ω\\Omega


Ω


and boundary effects are negligible.) Because the points positions are assumed to be independent, the[survival function](https://en.wikipedia.org/wiki/Survival_function) is


P(R>r)=(1−ωd rdV)N.(2)P(R > r) = \\left(1 - \\frac{\\omega_d\\, r^d}{V}\\right)^{N}\\text{.} \\tag{2}


P


(


R


>


r


)


=


(


1


−


V


ω


d


​


r


d


​


)


N


.


(


2


)


What we're really interested in is how R behaves on average. To compute this, we use the identity that the expectation of a non-negative random variable is the integral of its survival function, E\[R\]=∫0∞P(R>r) dr\\mathbb{E}\[R\] = \\int_0^\\infty P(R > r)\\, dr


E


\[


R


\]


=


∫


0


∞


​


P


(


R


>


r


)


d


r


. Evaluating this with (2) gives the headline result:


E\[R\]≈(Vωd N)1/d \\boxed{\\;\\mathbb{E}\[R\] \\approx \\left(\\frac{V}{\\omega_d\\, N}\\right)^{1/d}\\;}


E


\[


R


\]


≈


(


ω


d


​


N


V


​


)


1/


d


​


(The exact integral carries an extra Γ(1+1/d)\\Gamma(1 + 1/d)


Γ


(


1


+


1/


d


)


factor; it's an O(1)O(1)


O


(


1


)


constant that we can fold into a fitted coefficient later, so we drop it here.)


### Glacial scaling: why neighbor distances barely change as your index grows


It is interesting to consider what this formula tells us about how distances change with dataset size: E\[R\]∝N−1/d\\mathbb{E}\[R\] \\propto N^{-1/d}


E


\[


R


\]


∝


N


−


1/


d


. The exponent is −1/d-1/d


−


1/


d


, and in high intrinsic dimensions that is a *very* small number. This is a property the method leans on, so it's worth plugging in some numbers:


- If d=2d = 2


d


=


2


then doubling NN


N


multiplies E\[R\]\\mathbb{E}\[R\]


E


\[


R


\]


by 2−1/2≈0.7072^{-1/2} \\approx 0.707


2


−


1/2


≈


0.707


, so distances drop by ~30%.
- If d=50d = 50


d


=


50


then doubling NN


N


multiplies E\[R\]\\mathbb{E}\[R\]


E


\[


R


\]


by 2−1/50≈0.9862^{-1/50} \\approx 0.986


2


−


1/50


≈


0.986


, so distances drop by a little over 1%.


In high dimensions, neighbor distances barely move even if you add a lot of data; call it glacial scaling **.** It's the reason we can choose quantization parameters *once* from a tiny sample, with a small safety margin, and trust them to remain valid even after the index grows substantially before the next re-quantization.


## Expected distance to the k-th neighbor and the criticality gap


We actually care about the whole sequence of order statistics R(k)R_{(k)}


R


(


k


)


​


, R(n)R_{(n)}


R


(


n


)


​


, not just the minimum. There's a simple way to get them.


Map each radius to the *cumulative volume* it encloses by defining


Ui=ωd RidV.U_i = \\frac{\\omega_d\\, R_i^d}{V}\\text{.}


U


i


​


=


V


ω


d


​


R


i


d


​


​


.


By (1), each UiU_i


U


i


​


is exactly the probability of landing within radius RiR_i


R


i


​


, so the {Ui}\\{U_i\\}


{


U


i


​


}


are uniform on \[0,1\]\[0,1\]


\[


0


,


1


\]


. The order statistics of uniforms are[textbook](https://en.wikipedia.org/wiki/Order_statistic#Order_statistics_sampled_from_a_uniform_distribution) : the ii


i


-th smallest of NN


N


uniforms follows a Beta distribution,


U(i)∼Beta(i,N−i+1),E\[U(i)\]=iN+1.U_{(i)} \\sim \\mathrm{Beta}(i, N - i + 1), \\qquad \\mathbb{E}\[U_{(i)}\] = \\frac{i}{N+1}\\text{.}


U


(


i


)


​


∼


Beta


(


i


,


N


−


i


+


1


)


,


E


\[


U


(


i


)


​


\]


=


N


+


1


i


​


.


Inverting the volume map, R(i)∝U(i)1/dR_{(i)} \\propto U_{(i)}^{1/d}


R


(


i


)


​


∝


U


(


i


)


1/


d


​


, gives the scaling of the ii


i


-th neighbor distance:


E\[R(i)\]∝(iN)1/d.\\mathbb{E}\[R_{(i)}\] \\propto \\left(\\frac{i}{N}\\right)^{1/d}\\text{.}


E


\[


R


(


i


)


​


\]


∝


(


N


i


​


)


1/


d


.


That's all we need for the expected gap:


E\[Δn,k\]=E\[R(n)\]−E\[R(k)\]≈α((nN)1/d−(kN)1/d)=E\[R(k)\]((nk)1/d−1).\\begin{align*} \\mathbb{E}\[\\Delta_{n,k}\] &= \\mathbb{E}\[R_{(n)}\] - \\mathbb{E}\[R_{(k)}\] &\\approx \\alpha\\left(\\left(\\tfrac{n}{N}\\right)^{1/d} - \\left(\\tfrac{k}{N}\\right)^{1/d}\\right) &= \\mathbb{E}\[R_{(k)}\]\\left(\\left(\\tfrac{n}{k}\\right)^{1/d} - 1\\right)\\text{.} \\tag{3} \\end{align*}


E


\[


Δ


n


,


k


​


\]


​


=


E


\[


R


(


n


)


​


\]


−


E


\[


R


(


k


)


​


\]


​


≈


α


(


(


N


n


​


)


1/


d


−


(


N


k


​


)


1/


d


)


​


=


E


\[


R


(


k


)


​


\]


(


(


k


n


​


)


1/


d


−


1


)


.


​


(


3


)


​


The last form is the intuitive one: the gap between the kk


k


-th and nn


n


-th neighbors is the distance to the kk


k


-th neighbor, scaled by ((n/k)1/d−1)\\big((n/k)^{1/d} - 1\\big)


(


(


n


/


k


)


1/


d


−


1


)


. Widening the rerank depth nn


n


relative to kk


k


opens the gap; higher intrinsic dimension dd


d


closes it (the exponent 1/d1/d


1/


d


pushes (n/k)1/d(n/k)^{1/d}


(


n


/


k


)


1/


d


toward 1).


### Why the expected gap is sufficient to predict recall


Working with an expectation is only legitimate if the gap doesn't fluctuate wildly around it. It doesn't because concentration of measure saves us. Applying the[delta method](https://en.wikipedia.org/wiki/Delta_method) to R(k)∝U(k)1/dR_{(k)} \\propto U_{(k)}^{1/d}


R


(


k


)


​


∝


U


(


k


)


1/


d


​


and using Var(U(k))≈k/N2\\mathrm{Var}(U_{(k)}) \\approx k/N^2


Var


(


U


(


k


)


​


)


≈


k


/


N


2


from the Beta distribution, a little algebra gives


Var(R(k))≈E\[R(k)\]2d2 k.\\mathrm{Var}(R_{(k)}) \\approx \\frac{\\mathbb{E}\[R_{(k)}\]^2}{d^2\\, k}\\text{.}


Var


(


R


(


k


)


​


)


≈


d


2


k


E


\[


R


(


k


)


​


\]


2


​


.


So the[coefficient of variation](https://en.wikipedia.org/wiki/Coefficient_of_variation) is about 1dk\\frac{1}{d\\sqrt{k}}


d


k


​


1


​


. For any reasonable intrinsic dimension this is negligible, which justifies modeling only the expected distances. (If you're worried about the delta method approximation, you can check the results numerically: the delta-method variance and the resulting 1/(dk)1/(d\\sqrt{k})


1/


(


d


k


​


)


coefficient of variation match the exact expressions to several significant figures.)


### Extending the model to cosine similarity and inner product search


The derivation is for the Euclidean metric, but the other common metrics reduce to it:


- Forcosine similarity , the equidistant surface is the intersection of a sphere around the query with the unit sphere. This is called a[hyperspherical cap](https://en.wikipedia.org/wiki/Spherical_cap) , whose volume scales as sin⁡(θ)d−1≈θd−1\\sin(\\theta)^{d-1} \\approx \\theta^{d-1}


sin


(


θ


)


d


−


1


≈


θ


d


−


1


for small θ\\theta


θ


. Therefore, the analysis carries over unchanged up to constants, with the dimension reduced by one.
- For MIPS (maximum inner product), some extra care is needed, because nearest neighbors aren't confined to a compact region. A distant vector can still win on inner product if its norm is large enough, so the gap is really governed by the tail of the norm distribution. However, there is a clean fix, which is to use the[Neyshabur–Srebro transformation](https://proceedings.mlr.press/v40/Neyshabur15.pdf) . This lifts vectors onto a unit hypersphere in d+1d+1


d


+


1


dimensions. After this operation, it's just the cosine case.


## Fitting intrinsic dimension and scale from a small sample


Equation (3) has a known functional form but two unknown parameters: the intrinsic dimension dd


d


and the scale α\\alpha


α


. Both are easy to fit, and it's more convenient to fit them from raw neighbor distances than from gaps directly.


Sample several subsets of database vectors {Di}\\{D_i\\}


{


D


i


​


}


of sizes Ni=∣Di∣N_i = |D_i|


N


i


​


=


∣


D


i


​


∣


and a set of query vectors QQ


Q


. For each query q∈Qq\\in Q


q


∈


Q


and each subset, measure ri,j(q)r_{i,j}(q)


r


i


,


j


​


(


q


)


, the distance to the jj


j


-th nearest neighbor of qq


q


within DiD_i


D


i


​


. Taking logs of the scaling law E\[R(j)\]≈α(j / Ni)1/d\\mathbb{E}\[R_{(j)}\] \\approx \\alpha (j\\,/\\,N_i)^{1/d}


E


\[


R


(


j


)


​


\]


≈


α


(


j


/


N


i


​


)


1/


d


linearises it:


log⁡(1∣Q∣∑q∈Qri,j(q))≈log⁡α+1d(log⁡j−log⁡(Ni/N0)).\\log\\left(\\frac{1}{|Q|}\\sum_{q \\in Q} r_{i,j}(q)\\right) \\approx \\log\\alpha + \\frac{1}{d}\\Big(\\log j - \\log(N_i/N_0)\\Big)\\text{.}


lo g


​


∣


Q


∣


1


​


q


∈


Q


∑


​


r


i


,


j


​


(


q


)


​


≈


lo g


α


+


d


1


​


(


lo g


j


−


lo g


(


N


i


​


/


N


0


​


)


)


.


Specifically, this is linear in log⁡j\\log j


lo g


j


and log⁡(Ni/N0)\\log(N_i/N_0)


lo g


(


N


i


​


/


N


0


​


)


, so ordinary least squares recovers α^\\hat{\\alpha}


α


^


and d^\\hat{d}


d


^


. Varying the subset size NiN_i


N


i


​


is what makes it possible to estimate dd


d


: it's precisely the rate at which distances shrink with data volume. With the fitted parameters, the whole-index expected gap is


E\[Δn,k\]≈α^(kN0N)1/d^((nk)1/d^−1).(4)\\mathbb{E}\[\\Delta_{n,k}\] \\approx \\hat\\alpha\\left(\\frac{kN_0}{N}\\right)^{1/\\hat d}\\left(\\left(\\frac{n}{k}\\right)^{1/\\hat d} - 1\\right)\\text{.} \\tag{4}


E


\[


Δ


n


,


k


​


\]


≈


α


^


(


N


k


N


0


​


​


)


1/


d


^


(


(


k


n


​


)


1/


d


^


−


1


)


.


(


4


)


Figure 1 shows how well this fits in practice (and it’s remarkably good): predicted versus actual average distance to the kk


k


-th neighbor, across a range of datasets and metrics, have R2R^2


R


2


between 0.996 and 0.999.


**Figure 1** Goodness of fit of the average distance to the k-th nearest neighbor estimates. Each panel is a different dataset/metric; points are (actual, estimated) mean distances.


## Modeling vector quantization error as Gaussian


With the nearest-neighbor distance model established, the second component is the quantization error distribution. For every metric we use, the quantized distance estimate differs from the true distance by an error that is a sum of many independent per-dimension contributions. By the[Central Limit Theorem](https://en.wikipedia.org/wiki/Central_limit_theorem) that sum tends to Gaussian, so we model the error as normal with a variance we estimate empirically:


σb2=1Z∑q∈Q∑i,j(ri,j(q)−r~i,j(q∣b))2,\\sigma_b^2 = \\frac{1}{Z}\\sum_{q \\in Q}\\sum_{i,j}\\big(r_{i,j}(q) - \\tilde{r}_{i,j}(q \\mid b)\\big)^2\\text{,}


σ


b


2


​


=


Z


1


​


q


∈


Q


∑


​


i


,


j


∑


​


(


r


i


,


j


​


(


q


)


−


r


~


i


,


j


​


(


q


∣


b


)


)


2


,


where r~i,j(q∣b)\\tilde{r}_{i,j}(q \\mid b)


r


~


i


,


j


​


(


q


∣


b


)


is the quantized distance estimate using bb


b


-bit vectors and ZZ


Z


is the total number of (query, neighbor) pairs in our sample set. In other words: sample, quantize, measure the squared distance errors, average.


Figure 2 shows the empirical basis for the Gaussian assumption: measured quantization error densities against best-fit Gaussians across a variety of datasets. The fit is good, which is what lets the rest of the model stay analytic.


**Figure 2** Quantization error density plots (query bits 1, doc bits 1) with the best-fit Gaussian overlaid. The near-Gaussian shape is what the CLT argument predicts.


We could stop here and take a[minimax](https://en.wikipedia.org/wiki/Minimax) view: threshold the probability that the kk


k


-th and nn


n


-th neighbors swap, using the expected gap (4) against the error scale σb\\sigma_b


σ


b


​


. But that controls a worst-case event, and what we actually want to control is average recall. The outcome would be overly conservative quantization parameters and we'd pay some performance. The next section estimates expected recall properly.


## Predicting expected recall after reranking


Combining the distance model and the error model gives a closed-form estimate of expected recall after reranking. Model the *noisy* distance of the ii


i


-th true neighbor as a Gaussian centered on its true distance:


Xi=N(R(i),σb).X_i = \\mathcal{N}\\big(R_{(i)}, \\sigma_b\\big)\\text{.}


X


i


​


=


N


(


R


(


i


)


​


,


σ


b


​


)


.


The ii


i


-th neighbor survives reranking, i.e., lands in the retrieved top nn


n


, if fewer than nn


n


other vectors have a smaller noisy distance. Condition on Xi=xX_i = x


X


i


​


=


x


and count the competitors closer than xx


x


:


S(x)=∑j≠i1(Xj<x).S(x) = \\sum_{j \\ne i} \\mathbf{1}(X_j < x)\\text{.}


S


(


x


)


=


j





=


i


∑


​


1


(


X


j


​


<


x


)


.


Then the probability of recalling neighbor ii


i


integrates over where its own noisy distance lands:


P(recall(i))=∫0∞P(S(x)<n) fXi(x) dx.P(\\text{recall}(i)) = \\int_0^\\infty P\\big(S(x) < n\\big)\\, f_{X_i}(x)\\, dx\\text{.}


P


(


recall


(


i


))


=


∫


0


∞


​


P


(


S


(


x


)


<


n


)


f


X


i


​


​


(


x


)


d


x


.


The terms of S(x)S(x)


S


(


x


)


are independent Bernoullis but not identically distributed, since every neighbor jj


j


sits at a different true distance R(j)R_{(j)}


R


(


j


)


​


, so each has its own probability of intruding on the top- kk


k


set:


pj(x)=P(Xj<x)=Φ ⁣(x−R(j)σb),(5)p_j(x) = P(X_j < x) = \\Phi\\!\\left(\\frac{x - R_{(j)}}{\\sigma_b}\\right)\\text{,} \\tag{5}


p


j


​


(


x


)


=


P


(


X


j


​


<


x


)


=


Φ


(


σ


b


​


x


−


R


(


j


)


​


​


)


,


(


5


)


with Φ\\Phi


Φ


the standard normal CDF. This makes S(x)S(x)


S


(


x


)


a[Poisson-binomial](https://en.wikipedia.org/wiki/Poisson_binomial_distribution) variable. Since we sum many of them (because N≫nN \\gg n


N


≫


n


), the Lyapunov CLT applies and we approximate


S(x)∼N(μS(x) ,σS2(x)),S(x) \\sim \\mathcal{N}\\big(\\mu_S(x)\\,, \\sigma_S^2(x)\\big)\\text{,}


S


(


x


)


∼


N


(


μ


S


​


(


x


)


,


σ


S


2


​


(


x


)


)


,


with the standard Poisson-binomial moments


μS(x)=∑j≠ipj(x),σS2(x)=∑j≠ipj(x)(1−pj(x)).\\mu_S(x) = \\sum_{j \\ne i} p_j(x), \\qquad \\sigma_S^2(x) = \\sum_{j \\ne i} p_j(x)\\big(1 - p_j(x)\\big)\\text{.}


μ


S


​


(


x


)


=


j





=


i


∑


​


p


j


​


(


x


)


,


σ


S


2


​


(


x


)


=


j





=


i


∑


​


p


j


​


(


x


)


(


1


−


p


j


​


(


x


)


)


.


The survival probability then has a clean closed form:


P(S(x)<n)≈Φ ⁣(n−μS(x)σS(x)).P\\big(S(x) < n\\big) \\approx \\Phi\\!\\left(\\frac{n - \\mu_S(x)}{\\sigma_S(x)}\\right)\\text{.}


P


(


S


(


x


)


<


n


)


≈


Φ


(


σ


S


​


(


x


)


n


−


μ


S


​


(


x


)


​


)


.


This is where the two halves of the post so far finally meet. We don't need to know the individual R(j)R_{(j)}


R


(


j


)


​


because the manifold scaling law from Section 3 supplies them: R(j)=α (jN0/N)1/dR_{(j)} = \\alpha\\,(jN_0/N)^{1/d}


R


(


j


)


​


=


α


(


j


N


0


​


/


N


)


1/


d


. So the moments become explicit sums over ranks, which we truncate at a safe cutoff (say 10n10n


10


n


, since distant neighbors contribute negligibly):


μS(x)≈∑j=110nΦ ⁣(x−α(jN0/N)1/dσb),\\mu_S(x) \\approx \\sum_{j=1}^{10n} \\Phi\\!\\left(\\frac{x - \\alpha(jN_0/N)^{1/d}}{\\sigma_b}\\right)\\text{,}


μ


S


​


(


x


)


≈


j


=


1


∑


10


n


​


Φ


(


σ


b


​


x


−


α


(


j


N


0


​


/


N


)


1/


d


​


)


,


σS2(x)≈μS(x)−∑j=110nΦ ⁣(x−α(jN0/N)1/dσb)2.\\sigma_S^2(x) \\approx \\mu_S(x) - \\sum_{j=1}^{10n} \\Phi\\!\\left(\\frac{x - \\alpha(jN_0/N)^{1/d}}{\\sigma_b}\\right)^{2}\\text{.}


σ


S


2


​


(


x


)


≈


μ


S


​


(


x


)


−


j


=


1


∑


10


n


​


Φ


(


σ


b


​


x


−


α


(


j


N


0


​


/


N


)


1/


d


​


)


2


.


Finally, average recall@ kk


k


given rerank depth nn


n


sums the per-neighbor recall over the top kk


k


:


P(recall@k∣n)=∑i=1kP(recall(i)∣n)≈∑i=1k∫0∞Φ ⁣(n−μS(x)σS(x))ϕ ⁣(x−R(i)σb)dx. \\boxed{\\;P(\\text{recall}@k \\mid n) = \\sum_{i=1}^{k} P(\\text{recall}(i) \\mid n) \\approx \\sum_{i=1}^{k} \\int_0^\\infty \\Phi\\!\\left(\\frac{n - \\mu_S(x)}{\\sigma_S(x)}\\right)\\phi\\!\\left(\\frac{x - R_{(i)}}{\\sigma_b}\\right) dx.\\;}


P


(


recall


@


k


∣


n


)


=


i


=


1


∑


k


​


P


(


recall


(


i


)


∣


n


)


≈


i


=


1


∑


k


​


∫


0


∞


​


Φ


(


σ


S


​


(


x


)


n


−


μ


S


​


(


x


)


​


)


ϕ


(


σ


b


​


x


−


R


(


i


)


​


​


)


d


x


.


​


Here ϕ\\phi


ϕ


is the standard normal density. Each integral is smooth and one-dimensional, so Gauss–Legendre quadrature evaluates it in microseconds. The entire recall prediction for a set of candidate parameters costs a handful of quadrature evaluations, not index build andbenchmark run.


Figure 3 validates the end-to-end model: predicted average recall against measured recall across many parameter settings and multiple datasets has R2=0.982R^2 = 0.982


R


2


=


0.982


.


**Figure 3** Estimated versus actual average recall across a range of parameter settings and datasets. Points stay close to the ideal diagonal.


## How the recall model selects vector quantization parameters


With a fast recall predictor available, parameter selection becomes a cheap ordered search. Given a target recall and a rerank budget nn


n


(typically expressed as a multiple of kk


k


), we can find the *minimum*document and query bit counts, and other knobs, that clear the target. There are a few things to note that are practically important:


1. Glacial scaling gives us some safety because R(k)R_{(k)}


R


(


k


)


​


moves so slowly with NN


N


for even moderate intrinsic dimension. A small margin in the calculation means the chosen parameters stay valid if a lot of vectors are added before parameters are restimated.
2. Small kk


k


is the worst case if nn


n


is a fixed multiple of kk


k


. The gap \\mathbb{E}\[R_{(k)}\]( (n/k)1/d−1)(n/k)^{1/d} - 1)


(


n


/


k


)


1/


d


−


1


)


is smallest for small kk


k


so if a parameter choice satisfies the recall target at k=10k = 10


k


=


10


then it will for larger kk


k


will too.
3. We can treat quantization as a black box because the error model only needs the empirical error variance. This means we can test *any* configuration, including preconditioning, the same way and we can simply order candidate parameter tuples by increasing index and query cost, and stop at the first choice that hits the target recall. For tuples of (query bits, doc bits, rerank depth, precondition) a sensible search sequence increases query precision first, then document precision (1,1)(1,1)


(


1


,


1


)


, (2,1)(2,1)


(


2


,


1


)


, (3,1)(3,1)


(


3


,


1


)


, (4,1)(4,1)


(


4


,


1


)


, (2,2)(2,2)


(


2


,


2


)


, (3,3)(3,3)


(


3


,


3


)


, (4,2)(4,2)


(


4


,


2


)


, (4,4)(4,4)


(


4


,


4


)


, (7,4)(7,4)


(


7


,


4


)


and (7,7)(7,7)


(


7


,


7


)


each combined (via an outer product ⊗\\otimes


⊗


) with rerank depths like (1.5k, 2k, 3k)(1.5k,\\,2k,\\,3k)


(


1.5


k


,


2


k


,


3


k


)


and precondition ∈{true, false}\\in \\{\\text{true},\\, \\text{false}\\}


∈


{


true


,


false


}


, exiting as soon as the target is met.


### Results: auto-selected quantization parameters and recall across datasets


In this section, we discuss the results of the initial experiments on the end-to-end behavior. We’ve made some further refinements as part of the work to fully integrate with Elasticsearch that we discuss in our other post.


The table below shows auto-selected parameters targeting recall 0.97, measured with brute-force search, so the number reflects loss due to quantization *alone* (64 query clusters, targeting document clusters of size 384, which matches the settings of[DiskBBQ](https://www.elastic.co/search-labs/blog/diskbbq-elasticsearch-introduction) ).


Dataset Query bits Doc bits Precondition Depth Recall


FiQA E5 small 4 2 false 30 0.97


FiQA arctic 2 2 false 30 0.95


FiQA GTE 2 1 true 30 0.98


MNIST 3 1 true 30 0.99


Fashion MNIST 3 1 true 30 0.99


Quora E5 small 2 2 false 30 0.99


Quora arctic 2 1 false 30 0.97


Quora GTE 1 1 false 30 0.98


Dbpedia E5 small 4 2 false 30 0.99


Dbpedia arctic 2 1 false 30 0.94


Dbpedia GTE 2 1 false 30 0.96


Wiki Cohere 2 2 false 30 0.99


Hotpot E5 small 4 2 false 30 0.97


Hotpot GTE 2 1 false 30 0.96


Glove 100 4 2 false 30 0.87


Glove 200 4 2 false 30 0.89


SIFT128 4 4 false 20 0.99


There are a few things worth highlighting:


- The recall is very sensitive to rerank depth. This is why we nearly always end up choosing the maximum depth available: a step up in rerank depth from 20 to 30 is typically what pushes us to hit the recall target for fewer bits and we prefer fewer bits. In the real system, we tuned this behavior based on a more representative reranking cost.
- Glove underperforms partly we approximate the query distribution with random samples from thecorpus , but Glove is also less well characterized by the model than the other datasets. A plausible explanation is that the approximately uniform local density assumption from Section 2 is less reliable for Glove embeddings, which would show up as higher recall variance between queries. However, Glove embeddings are not representative of the actual vectors we need to store.
- The FiQA GTE preconditioning choice is a knife-edge case: preconditioning produced only a tiny expected recall improvement, but the prediction sat right at the recall cutoff and allows us to drop the query from 3 to 2 bits. If we'd rather only keep preconditioning where its benefit is clear-cut, we can enforce a minimum uplift threshold. This sort offine-tuning of the decision logic leaves all the heavy lifting to estimate recall unaffected.


## Key takeaways: auto-tuning vector quantization from first principles


We presented a method to pick optimal quantization parameters to achieve a target recall. It rests on two models that compose cleanly:


1. A geometric model of neighbor distances that follows from a local uniform density assumption. We use this to derive the nearest-neighbor distance, the N−1/dN^{-1/d}


N


−


1/


d


glacial scaling law of the expected distance, and the expected distance profile R(j)=α(jN0/N)1/dR_{(j)} = \\alpha(jN_0/N)^{1/d}


R


(


j


)


​


=


α


(


j


N


0


​


/


N


)


1/


d


. We show that fitting α\\alpha


α


and dd


d


by a simple log-linear regression to average distances in small random samples from the corpus gives an extremely accurate predictive model.
2. A Gaussian quantization error model that is justified by the CLT. Its only parameter σb\\sigma_b


σ


b


​


is an empirical variance we estimate by comparing quantized and raw vector similarities for a sample of the corpus.


Finally, we show that it is possible to feed the estimated distance model into a Poisson-binomial count of neighbors that intrude on the top- kk


k


set. Applying the Lyapunov CLT the expected recall@ kk


k


after reranking to depth nn


n


falls out as a one-dimensional integral we evaluate by quadrature.


The outcome is an accurate ( R2>0.95R^2>0.95


R


2


>


0.95


) predictive model of recall as a function of the quantization parameters. Choosing quantization parameters then becomes an ordered search with a predictive model telling us if we’ve hit the recall constraint. And nicely one that also comes with a built-in argument (glacial scaling) for why the chosen parameters remain safe even when estimated from a relatively small fraction of the data.


We’ve built this entire mechanism into Elasticsearch using segment merges as an opportunity to reassess our quantization choices. Aside from the peace of mind this brings (that you’ll achieve good recall whatever vectors you throw at it), it also allows us to chose near optimal parameters from a performance perspective. This closes the loop on our original objective: near optimal performance out of the box, at least as far as quantization goes. We’re pretty excited about the advantages that model based tuning can bring to vector search and look forward to sharing other work we have in this direction in the near future.


#### How helpful was this content?


Not helpful


Somewhat helpful


Very helpful


[Report an issue](https://discuss.elastic.co/c/elastic-community-ecosystem/elasticsearch-labs/101)


## Related Content


[Jina AI](https://www.elastic.co/search-labs/blog/category/jina-ai)[Relevance](https://www.elastic.co/search-labs/blog/category/relevance) +1


July 27, 2026


#### [56% faster, up to 50% better retrieval performance: What's inside Jina's new 600 million parameter listwise reranker](https://www.elastic.co/search-labs/blog/jina-reranker-35-legal-medical-structured-data)


Jina Reranker 3.5 beats v3 by 50%+ on case law, closes the gap with models 7x its size on legal, medical, and financial benchmarks, and beats them outright on structured data. It's a drop-in replacement for v3, with no API changes.


FWSM


By:[Felix Wang](https://www.elastic.co/search-labs/author/felix-wang)


and[Scott Martens](https://www.elastic.co/search-labs/author/scott-martens)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)[ML Research](https://www.elastic.co/search-labs/blog/category/ml-research) +1


July 28, 2026


#### [17% faster search, zero config: auto-calibrating vector quantization in Elasticsearch](https://www.elastic.co/search-labs/blog/vector-quantization-auto-calibration-diskbbq)


Automatic calibration at merge time picks vector quantization parameters for each segment by predicting recall from a small sample. Here's how we built it into Elasticsearch's merge path.


TTTV


By:[Tommaso Teofili](https://www.elastic.co/search-labs/author/tommaso-teofili)


and[Thomas Veasey](https://www.elastic.co/search-labs/author/thomas-veasey)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)[Relevance](https://www.elastic.co/search-labs/blog/category/relevance) +1


July 16, 2026


#### [A picture is worth 1.5x the words: What we learned benchmarking product search embeddings](https://www.elastic.co/search-labs/blog/multimodal-embeddings-ecommerce-product-search)


We benchmarked two embedding models on 5,000 real products and found that combining image and text beats either alone by up to 50%. Here's the data and the model that won.


SV


By:[Sofia Vasileva](https://www.elastic.co/search-labs/author/sofia-vasileva)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)


July 13, 2026


#### [The disk that never woke up: what actually decided our Qdrant vector search benchmark rematch](https://www.elastic.co/search-labs/blog/vector-search-benchmark-elasticsearch-qdrant)


On the same hardware, Elasticsearch and Qdrant land in the same range at 56 QPS. The io_uring disk scorer and memory claims turned out to be the two things that mattered least.


JF


By:[Jim Ferenczi](https://www.elastic.co/search-labs/author/jim-ferenczi)


[Integrations](https://www.elastic.co/search-labs/blog/category/integrations)[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database) +1


July 21, 2026


#### [4 NVIDIA AI tasks, 1 Elasticsearch API: Embeddings, chat, completion, and rerank](https://www.elastic.co/search-labs/blog/elasticsearch-nvidia-inference)


Set up NVIDIA hosted models in Elasticsearch with one API key and a model ID. No custom integration code needed.


K


By:[Jan Kazlouski](https://www.elastic.co/search-labs/author/jan-kazlouski)
