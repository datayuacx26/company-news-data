---
schema_version: "1.0.0"
document_id: "788feb39a85fc0aed5255f96212518c9a6f126417bd23d1d80f8990cdce93828"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/location-invariant-functions"
published_at: null
first_seen_at: "2026-07-21T07:16:20.305331+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:61252800a4ea09ed4aa0031926fd5a2fc21ce3018d156019b9239a4a59a38bbe"
---

# Location-Invariant Properties of Functions Versus Properties of Distributions: United in Testing but Separated in Verification

[View publication](https://eccc.weizmann.ac.il/report/2025/105/)


A property of functions is called location-invariant (or symmetric) if it can be characterized in terms of the frequencies in which each value occurs in the function, regardless of the locations in which each value occurs. It is known that the (query) complexity of testing location-invariant properties of functions is closely related to the (sample) complexity of testing the (corresponding properties of the) corresponding distributions. The main message of the current work is that this close relationship is not maintained in the context of verification. This holds both when considering verification by general interactive proofs of proximity (i.e., IPPs) and when restricting attention to doubly-sublinear IPPs (ds-IPPs). Alternatively, one may view this work as a subsequent step in the study of doubly-sublinear IPPs (of properties of functions), where we say that an IPP is doubly-sublinear if (1) the query complexity of the verifier is sublinear in the query complexity of testing the property, and (2) the query complexity of the honest prover is sublinear in the query complexity of learning a function in the property. Specifically, we present doubly-sublinear IPPs for several natural location-invariant properties. Our results include: (1) We present doubly-sublinear IPPs for the set of functions from \[m\] to \[n\] in which each value occurs m/n times: For every α ∈ (0, 0.5), the query complexity of the verifier is O(n^{0.5−α}), and the query complexity of the honest prover is e^{O(n^{0.5+α}/ε^2)}. (2) We present doubly-sublinear IPPs for the set of functions from \[m\] to \[n\] in which each value occurs either m/k times or not at all: For every α ∈ (0, 1/3), the query complexity of the verifier is poly(1/ε) · k^{(2/3)−2α}, and the query complexity of the prover is poly(1/ε) · e^{O(k^{(2/3)+α})}. In contrast, in both cases, it is known that the corresponding properties of distributions have no doubly-efficient IPP (see Herman and Rothblum, 2025). Actually, the first property of distributions (i.e., uniformity over \[n\]) does not even have an IPP in which the verifier uses o(n^{1/2}) samples, regardless of other complexity measures.


- † Weizmann Institute of Science
