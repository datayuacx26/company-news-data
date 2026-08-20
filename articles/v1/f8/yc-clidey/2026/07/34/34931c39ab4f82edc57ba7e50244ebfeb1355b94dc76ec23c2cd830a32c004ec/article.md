---
schema_version: "1.0.0"
document_id: "34931c39ab4f82edc57ba7e50244ebfeb1355b94dc76ec23c2cd830a32c004ec"
company_key: "yc-clidey"
company: "Clidey"
source_id: "yc-clidey-rss-03eb479b7595"
canonical_url: "https://blog.clidey.com/a-reproducible-proof-against-the-jacobian-conjecture/"
published_at: "2026-07-21T00:25:34+00:00"
first_seen_at: "2026-07-24T22:35:58.719229+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:48957f1965450458577ed824797c72234884ed5964af59458a5edcd3d98d484f"
---

# A Reproducible Proof Against the Jacobian Conjecture

[report report.pdf 75 KB](https://blog.clidey.com/content/files/2026/07/report-1.pdf)


[report.py report.py.zip 1 KB](https://blog.clidey.com/content/files/2026/07/report.py.zip)


## An exact symbolic check of a proposed polynomial map in three variables


> **Attribution.** The polynomial map and the claimed observation discussed here are credited to **Levent Alpoge** , based on the supplied announcement and post. That source also describes the result as arising from a prompt involving **Akhil Mathew** . This post documents an independent check of the displayed formulas; it does not claim authorship, priority, or credit for the construction.


The Jacobian Conjecture says that every polynomial map with a nonzero constant Jacobian determinant has a polynomial inverse. In particular, such a map must be injective.


This post records an exact symbolic and algebraic verification of a proposed map from ℂ³ to ℂ³. The calculation gives a constant Jacobian determinant of` -2` , while three distinct points map to the same point.


That combination would contradict the Jacobian Conjecture over ℂ, provided the defining formulas have been transcribed correctly and the verification below is independently reproduced.


## Attachments


Available above.


The PDF contains the full derivative derivation, exact fibre elimination, reproducibility record, and audit of the earlier manuscript. The Python file is the minimal executable check.


## The proposed polynomial map


Set **u = 1 + xy** .


Define` F = (f1, f2, f3)` by:


- **f₁ = u³z + y²u(4 + 3xy)**
- **f₂ = y + 3xu²z + 3xy²(4 + 3xy)**
- **f₃ = 2x − 3x²y − x³z**


The key exact symbolic result is:


> **det(J_F) = −2**


## The claimed collision


Using only the formulas above, exact rational substitution gives:


- **F(0, 0, −¼) = (−¼, 0, 0)**
- **F(1, −³⁄₂, ¹³⁄₂) = (−¼, 0, 0)**
- **F(−1, ³⁄₂, ¹³⁄₂) = (−¼, 0, 0)**


The three inputs are distinct. Direct elimination of **F(x, y, z) = (−¼, 0, 0)** gives exactly these three complex solutions and no others.


## Verify it yourself


The attached` report.py` uses SymPy with exact integers and rational numbers. It does not use floating-point arithmetic or numerical sampling.


Install SymPy if needed, then run:


```text
python3 report.py


```


A successful run prints:


```text
factor(det(J_F)) = -2
F(0, 0, -1/4) = (-1/4, 0, 0)
F(1, -3/2, 13/2) = (-1/4, 0, 0)
F(-1, 3/2, 13/2) = (-1/4, 0, 0)
F^(-1)(-1/4, 0, 0) = [{x: -1, y: 3/2, z: 13/2}, {x: 0, y: 0, z: -1/4}, {x: 1, y: -3/2, z: 13/2}]
All exact symbolic checks passed.


```


The program contains assertions. It exits with an error if the determinant, any point evaluation, or the complete fibre differs from the stated result.


For the version supplied with this post, the SHA-256 checksum is:


```text
5af0275b45e21f68a5c659b67492a54d2d74a82a2f423c9da45e76c27c67dac3  report.py


```


To check the downloaded file before running it:


```text
shasum -a 256 report.py


```


## What has been checked


The attached report independently derives all nine partial derivatives from the original formulas, forms the Jacobian matrix, and solves the target fibre by a complete case split.


Separately,` report.py` reconstructs the map from scratch, computes and factors the determinant symbolically, evaluates the three points exactly, and solves the polynomial system.


The verifier was executed in this workspace using Python 3.14.6 and SymPy 1.14.0. That execution is useful evidence, but it is not a substitute for an independent rerun by readers in their own environment.


## Reproduce for yourself


This is an extraordinary mathematical claim. A web post and a displayed terminal transcript should not be treated as a replacement for independent reproduction, code review, and expert scrutiny. The attached script is provided precisely so that anyone can check the formulas and results directly.


If independent reruns agree with the exact calculation, then this polynomial map is a Keller map with a nontrivial fibre, and therefore a counterexample to the Jacobian Conjecture over ℂ.
