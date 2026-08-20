---
schema_version: "1.0.0"
document_id: "37208a3fbc67601df130af4618904ceb853c9bd8b69f0884760207009f49647f"
company_key: "entegris-inc-common-stock"
company: "Entegris Inc."
source_id: "entegris-inc-common-stock-rss-153d19c6113a"
canonical_url: "https://blog.entegris.com/effect-of-pou-filter-pore-size-on-euv-bridge-defect-density"
published_at: "2026-04-30T12:00:00+00:00"
first_seen_at: "2026-07-20T03:31:27.985945+00:00"
fetched_at: "2026-07-28T20:51:50.549866+00:00"
content_hash: "sha256:c89f8df3e94e81d96caef3e0918b3b72b048c556d0a8ec1dfbc4301ab24b5d4b"
---

# Effect of POU Filter Pore Size on EUV Bridge Defect Density

Effect of POU Filter Pore Size on EUV Bridge Defect Density


One EUV bridge defect mechanism is well understood: residual particles present during resist coating locally block etch, creating unintended electrical connections between adjacent features. Because these defects are irreversible and carry a high yield impact, effective mitigation must occur upstream, particularly at resist delivery for an effective coating.


Study results presented at SPIE Advanced Lithography + Patterning 2026 provide on‑wafer data examining one such upstream control lever: point‑of‑use (POU) photoresist filtration, with a focus on membrane pore size and morphology. The study was designed to isolate filtration effects under tightly controlled EUV process conditions.


*Figure 1 Bridge formation mechanism from particle in EUV CAR through etch process.*


Defect Mechanism and Control Hypothesis


For dense EUV line/space patterning, a single residual particle embedded in the resist film can propagate through develop and etch, producing a conductive bridge defect with a high probability of electrical failure. This makes bridge defects particularly sensitive to contamination control strategies applied immediately prior to coating.


Prior work has shown that sieving‑dominant POU membranes (size exclusion) are more effective at particle removal than designs relying on non‑sieving mechanisms (adsorption), and that smaller membrane pore sizes correlate with reduced bridge defect density. The present work evaluates whether continued pore size scaling delivers a measurable on‑wafer benefit under representative EUV conditions.


Experimental Design Summary


Multiple generations of ultra‑pure polyethylene (UPE) POU membranes were evaluated, culminating in a next‑generation design with approximately 25% reduced pore size relative to earlier designs. All non‑filtration process variables were intentionally held constant to isolate pore size effects. The key tools, materials, and defect endpoint used in the evaluation in table 1.


**Experimental Aspect** **Implementation** **Relevance to Defect Results**


POU filtration UPE POU membranes across multiple pore sizes Membrane pore size was the single intentional experimental variable


Photoresist imec standard EUV chemically amplified resist (CAR) Representative CAR minimizes supplier‑driven defect bias


Track TEL Clean Track LITHIUS Pro Z Unified coat, bake, and develop conditions across all tests


Exposure ASML NXE:3400B EUV scanner (0.33 NA) Fixed imaging conditions isolate contamination effects


Pattern Dense 32 nm pitch line/space High sensitivity to particle‑driven bridge formation


Defect metric Post‑etch bridge defect density Electrically relevant defect endpoint


*All results discussed reflect relative performance under the controlled conditions summarized above.*


On‑Wafer Results: Bridge Defect Density


Under identical EUV process conditions, the tightest‑pore UPE membrane exhibited the lowest normalized bridge defect density among the filters evaluated. Importantly, the reduction was observed after full pattern transfer, confirming that the effect persists through etch rather than being limited to post‑develop inspection.


*Figure 2 Pore size – bridge density correlation*


When normalized bridge defect density is plotted against normalized membrane pore size across filter generations, a strong linear correlation is observed (R² ≈ 0.97). This result is consistent with historical observations and extends the trend into tighter pore regimes. Within the tested space, pore size scaling remains an effective control variable for particle‑driven bridge defects in EUV CAR processes.


Technical Interpretation and Scope


These results do not suggest that filtration alone can eliminate EUV defectivity, nor do they imply universal applicability across all resist chemistries or process flows. What the data supports is more specific:


- Particle retention via sieving remains effective as membrane pore size decreases
- On‑wafer bridge defectivity tracks membrane morphology under controlled conditions
- POU filtration performance must be evaluated using post‑etch defect metrics, not inferred solely from liquid particle counts


Equally important, the results reinforce that filter cleanliness is a first‑order requirement. As pore sizes shrink, intrinsic contamination from the filter itself becomes increasingly relevant, and retention gains must not be offset by new defect sources.


Implications for EUV Process Integration


For lithography and yield engineers, the takeaway is practical:


-


POU filter selection should be resist


‑


specific and node


‑


specific


-


Filtration performance should be validated using electrically relevant, post


‑


etch defect endpoints


-


As EUV margins tighten, POU filtration transitions from a contributor function to a vital element to pattern fidelity


Ongoing work is focused on repeatability, extended filter loading behavior, and deeper investigation into chemistry–retention interactions to further define the role of filtration in advanced EUV processes.


[Contact our experts](https://www.entegris.com/en/home/customer-service/contact-us.html?utm_campaign=31748515-clean-materials-delivery&utm_source=blog&utm_medium=website) to learn more about the Entegris collaboration approach.
