---
schema_version: "1.0.0"
document_id: "3decf8cb637f5d2b989c0b9f5f51e98d04501d2a5c8b0c63bdc40bd27fec905e"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/semismooth-newton-optimal-transport"
published_at: null
first_seen_at: "2026-08-19T12:44:48.398250+00:00"
fetched_at: "2026-08-19T12:44:50.170902+00:00"
content_hash: "sha256:88eae3093eaa5c6dbf760db52ceb65c324da433113b136d75861e0fb23cdbb10"
---

# A Specialized Semismooth Newton Method for Kernel-Based Optimal Transport

[View publication](https://arxiv.org/abs/2310.14087)


Kernel-based optimal transport (OT) estimators offer an alternative, functional estimation procedure to address OT problems from samples. Recent works suggest that these estimators are more statistically efficient than plug-in (linear programming-based) OT estimators when comparing probability measures in high-dimensions \[Vacher et al., 2021\]. Unfortunately, that statistical benefit comes at a very steep computational price: because their computation relies on the short-step interior-point method (SSIPM), which comes with a large iteration count in practice, these estimators quickly become intractable w.r.t. sample size n. To scale these estimators to larger n, we propose a nonsmooth fixed-point model for the kernel-based OT problem, and show that it can be efficiently solved via a specialized semismooth Newton (SSN) method: We show, exploring the problem’s structure, that the per-iteration cost of performing one SSN step can be significantly reduced in practice. We prove that our SSN method achieves a global convergence rate of O(1/√k), and a local quadratic convergence rate under standard regularity conditions. We show substantial speedups over SSIPM on both synthetic and real datasets.


- † Massachusetts Institute of Technology
- ‡ University of California, Berkeley
