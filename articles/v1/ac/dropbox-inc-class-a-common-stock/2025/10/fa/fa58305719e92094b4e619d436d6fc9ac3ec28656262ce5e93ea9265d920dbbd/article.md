---
schema_version: "1.0.0"
document_id: "fa58305719e92094b4e619d436d6fc9ac3ec28656262ce5e93ea9265d920dbbd"
company_key: "dropbox-inc-class-a-common-stock"
company: "Dropbox Inc."
source_id: "dropbox-inc-class-a-common-stock-news-import-0470a1f405f8"
canonical_url: "https://dropbox.tech/machine-learning/halfquadratic-quantization-of-large-machine-learning-models"
published_at: "2025-10-22T00:00:00+00:00"
first_seen_at: "2026-07-25T01:59:28.029954+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:f9473a6a98574216761bbf236aeb57a127b72888d1bd4e2c165370af9d746245"
---

# Half-Quadratic Quantization of large machine learning models

Basic quantization often results in a loss of model accuracy. This is because the weights in these models can have a wide range of values that can be significantly altered after the quantization process. Weights that deviate from the distribution, notably known as outliers, pose a particular challenge.[Group-wise Precision Tuning Quantization (GPTQ)](https://arxiv.org/abs/2210.17323) and[Activation-Aware Layer Quantization (AWQ)](https://arxiv.org/abs/2306.00978) are algorithms that try to overcome this issue by relying on calibration data to minimize the error on layer outputs.


Unlike these approaches, our method focuses specifically on minimizing errors in the *weights* rather than the layer activation. Additionally, by incorporating a sparsity-promoting loss, such as the \\( {l_{p<1}} \\)-norm, we effectively model outliers through a hyper-Laplacian distribution. This distribution more accurately captures the heavy-tailed nature of outlier errors compared to the squared error, resulting in a more nuanced representation of error distribution.


We propose a robust optimization formulation to find the quantization parameters (zero-point \\( z \\) and scaling \\( s \\)). More specifically, we use a sparsity-promoting loss function \\( \\phi() \\) such as the \\( {l_{p}} \\) norm between the original weights \\( W \\) and their dequantized version:


$$\\underset{z,s}{\\text{argmin}}\\,\\phi(W-Q_{z,s}^{-1}(Q_{z,s}(W)),$$


where \\( Q_{z,s}() \\) is the quantization operator which depends on the \\( z \\) and \\( s \\) parameters and generates the quantized weights \\( W_{q} \\). \\( Q_{z,s}()^{-1} \\) is the de-quantization operator:


$$\\begin{array}{c} Q_{z,s}(W)=\\text{round}(W/s+z)=W_{q}\\\\ Q_{z,s}^{-1}(W_{q})=s(W_{q}-z) \\end{array}$$


The use of the \\( {l_{p<1}} \\)-norm makes the problem non-convex. To find a solution, we adopt a[Half-Quadratic solver](https://ieeexplore.ieee.org/document/120331) by introducing an extra variable \\( W_{e} \\). This additional parameter allows us to split the main problem into sub-problems that are easier to solve. Moreover, to make the problem simpler, we fix the scaling parameter \\( s \\) and only optimize for the zero-point \\( z \\).


$$\\underset{z,W_{e}}{\\text{argmin}}\\,\\phi(W_{e})+\\frac{\\beta}{2}||W_{e}-(W-Q_{z}^{-1}(Q_{z}(W))||_{2}^{2}$$ We then form sub-problems which are solved via alternate optimization: $$\\begin{array}{cc} \\text{(sp}_{1}) & W_{e}^{(t+1)}\\leftarrow\\underset{}{\\underset{W_{e}}{\\text{argmin}}\\,\\phi(W_{e})+\\frac{\\beta^{(t)}}{2}||W_{e}-(W-Q_{z}^{-1}(Q_{z}(W))||_{2}^{2}}\\\\ \\text{(sp}_{2}) & z^{(t+1)}\\leftarrow\\underset{z}{\\text{argmin}}\\,\\frac{1}{2}||Q_{z}^{-1}(Q_{z}(W))-(W-W_{e}^{(t+1)})||_{2}^{2}\\\\ & \\beta^{(t+1)}\\leftarrow\\kappa\\beta^{(t)},\\end{array}$$ where \\( \\beta \\) and \\( \\kappa \\) and strictly positive parameters.


#### Sub-problem \\( \\text{(sp}_{1}) \\)


This problem takes the form of a[Proximal Operator](https://web.stanford.edu/~boyd/papers/pdf/prox_algs.pdf) . When \\( \\phi() \\) is the \\( l_{1} \\) norm, the solution is the[soft-thresholding operator](https://sparse-plex.readthedocs.io/en/latest/book/opt/soft_thresholding.html) . There exists a more general thresholding solution for the \\( l_{p}\\)-norm with \\( 0 \\le p \\leq 1 \\) that we adopt known is as the[generalized soft-thresholding operator](https://inria.hal.science/hal-01317151/file/lowrank_ieee_tip.pdf) :


$$\\begin{array}{c} W_{e}^{(t+1)}\\leftarrow\\text{shrink}_{l_{p}}\\left(W-Q_{z}^{-1}(Q_{z}(W)),\\beta\\right)\\\\ \\text{shrink}_{l_{p}}(x,\\beta)=\\text{sign}(x)\\text{relu}(|x|-\\frac{|x|^{p-1}}{\\beta}) \\end{array}$$


#### Sub-problem \\( \\text{(sp}_{2}) \\)


The second sub-problem can be rewritten as follows:


$$\\begin{array}{c} z^{(t+1)}\\leftarrow\\underset{z}{\\text{argmin}}\\,\\frac{1}{2}||z-\\left(W_{q}^{(t+1)}-\\frac{(W-W_{e}^{(t+1)})}{s}\\right)||_{2}^{2}\\\\ W_{q}^{(t+1)}=\\text{round}(W/s+z^{(t)}) \\end{array}$$


The solution is simply the average over the axis the quantization grouping is performed on:


$$z^{(t+1)}\\leftarrow\\langle W_{q}^{(t+1)}-\\frac{(W-W_{e}^{(t+1)})}{s}\\rangle$$


In our implementation, we work with the inverse of the scale \\( 1/s \\) instead of \\( s \\) which we found to be a bit more stable with the half-precision calculations.


Note that, contrary to using gradient descent with[autograd](https://pytorch.org/tutorials/beginner/blitz/autograd_tutorial.html) , the approach that we propose relies on closed-form solutions, which means that there are no gradients calculated. This allows us to run all the calculations in inference mode with half-precision. Moreover, it only takes a few iterations for the solver to converge. Conversely, using the AdamW optimizer and Pytorch’s autograd takes thousands of iterations to achieve good results. It also fails with \\( p < 1 \\), which is what we actually use to promote sparsity. Thanks to the Half-Quadratic solution, our quantization method achieves significant speed-up (over **100x** faster vs. autograd to quantize Llama-2-7B), being able to process even the largest models in only a few minutes.
