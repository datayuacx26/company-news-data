---
schema_version: "1.0.0"
document_id: "2273f86b8f00fbbdafaadefc0374ce641b4e072a99a180281a76311acc1d217a"
company_key: "yc-ctgt"
company: "CTGT"
source_id: "yc-ctgt-news-import-358f70d55d44"
canonical_url: "https://www.ctgt.ai/research/training-data-eigenvector-dynamics-in-the-eigenpro-implementation-of-the-neural-tangent-kernel-and-recursive-feature-machines"
published_at: "2023-05-06T00:00:00+00:00"
first_seen_at: "2026-07-27T01:23:44.579296+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:1ae9986331bb4578cdf6209097d5b9b43178fa5a5115434f05467cd1e95e53e2"
---

# Training data eigenvector dynamics in the EigenPro implementation of the neural tangent kernel and recursive feature machines.

#### Abstract


There has been much recent work on kernel methods as a viable alternative to deep neural networks (DNNs). The advent of the Neural Tangent Kernel (NTK) has brought on renewed interest in these methods and their application to typical deep learning tasks. Recently, kernels have been shown to be capable of feature learning similar to that of DNNs, termed Recursive Feature Machines (RFMs). In accordance with the growing scale of kernel models, the EigenPro 3 algorithm was proposed to facilitate large-scale training based on preconditioned gradient descent. We propose an accessible framework for observing the eigenvector dynamics of EigenPro’s training data in its implementation of these kernel methods, and find empirically that significant change ceases early in training along with apparent bias towards equilibrium. In the case of RFMs, we find that significant change in the training data eigenvectors typically curtails before five iterations, in accordance with findings that RFMs achieve optimal performance in five iterations. This represents a path forward in gaining intuition for the inner workings of large-scale kernel training methods. We provide an easy to use Python implementation of our framework at[https://github.com/cgorlla/ep3dynamics](https://github.com/cgorlla/ep3dynamics) .


[Link to paper](https://openreview.net/pdf?id=8WiNDyXgj6)


‍
