---
schema_version: "1.0.0"
document_id: "2c3ed92376fdf4f07838776ea92593961196758bfca473f55f029d0ada17e5e4"
company_key: "emerson-electric-company-common-stock"
company: "Emerson Electric Company"
source_id: "emerson-electric-company-common-stock-rss-55c4887b1417"
canonical_url: "https://www.emersonautomationexperts.com/2026/valves-actuators-regulators/sizing-a-control-valve-for-liquid-flow-a-step-by-step-walkthrough/"
published_at: "2026-06-10T18:00:51+00:00"
first_seen_at: "2026-07-25T01:09:56.979097+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:9f01a4f25b4edb885981f7322bbb97fc2801f253e387b8d22e53c6ffd48954a7"
---

# Sizing a Control Valve for Liquid Flow: A Step-by-Step Walkthrough

Sizing a control valve for liquid service follows a defined procedure from the International Society of Automation (ISA) and the International Electrotechnical Commission (IEC). This post walks through that procedure using a worked example for liquid propane service from the video, “[How to Size a Control Valve for Liquid Flow](https://www.youtube.com/watch?v=ldLkbV3W7Pk) .” I viewed the video and caught up with Emerson’s[Reid Youngdahl](https://www.linkedin.com/in/reid-youngdahl-29626570/) to review the insights distilled from it.


### Why It Matters


An undersized valve cannot pass the required flow, and the correct selection depends on more than the published flow coefficient. Pipe reducers, choked flow conditions, and the interaction between pressure drop and vaporization all influence the result. Working through the full procedure, including recalculations as needed, confirms that the valve will meet the stated service conditions.


### Key Takeaways


- The procedure has five steps: specify variables, determine equation constants, calculate the piping geometry factor (FP) and adjusted liquid pressure recovery factor (FLP), determine the pressure drop to use for sizing, and calculate the required flow coefficient (Cv).
- Concentric reducers reduce effective capacity, captured through FP and FLP.
- When the actual pressure drop (ΔP) is lower than the choked-flow pressure drop (ΔP choked), the actual ΔP is used for sizing.
- If the assumed valve is too small, repeat the steps with the next larger size and refine until the calculated Cv stabilizes.


### Setting Up the Problem: Variables and Constants


The example specifies a Class 300 globe valve with an assumed 3-inch size and an equal percentage cage, installed with standard concentric reducers. At 100% open, the flow coefficient (Cv) is 121, and the liquid pressure recovery factor (FL) is 0.89. The process fluid is liquid propane, with service conditions held constant for reference throughout the calculation. From the equation constants table in the[Control Valve Handbook](https://www.emerson.com/en/final-control/catalog/products-and-software/valves/control-valves/control-valve-handbook) , N1 equals 1.0, and N2 equals 890.


### Accounting for Fittings and Choked Flow


With the upstream and downstream pipe sizes equal, the resistance coefficients are calculated in sequence: K1 equals 0.37, KB1 equals 0.98, and ΣK equals 1.11. These feed into the piping geometry factor (FP), which comes out to 0.90, and the liquid pressure recovery factor adjusted for the attached fittings (FLP), which comes out to 0.81.


Next, determine the pressure drop to use for sizing (ΔP sizing). When the difference between upstream and downstream pressures is sufficiently large, the liquid may start to vaporize, causing choked flow. If the actual ΔP across the valve exceeds the pressure drop that causes choked flow, the choked-flow pressure drop (ΔP choked) must be used instead of the actual pressure drop. The liquid critical pressure ratio factor (FF) is 0.83, and ΔP choked works out to 171 pounds per square inch (psi). Because the actual ΔP of 25 psi is lower than 171 psi, ΔP sizing equals the actual ΔP of 25 psi.


### Calculating Cv and Iterating to the Right Size


Plugging the variables into the standard Cv equation gives a required Cv of 125.7. That exceeds the published valve’s equal-percentage characteristic max rated Cv of 121, so the next larger size should be evaluated if an equal-percentage characteristic is required. If a linear flow characteristic is acceptable, then the additional max rated Cv of the Linear characteristic could allow for us to stay in a NPS 3” valve, however for this example an equal percentage flow characteristic is desired, therefore the next candidate is a Class 300 nominal pipe size 4 (NPS 4)[Fisher ES globe valve](https://www.emerson.com/en/final-control/products/fisher-es) with an equal percentage cage, with a published Cv of 203 and FL of 0.91.


Recalculating with the new assumed Cv of 203, ΣK becomes 0.84, and FP becomes 0.93, which yields a new required Cv of 121.7. This result indicates only that the NPS 4 valve is sufficient to meet the service conditions. When a more accurate prediction is needed, the required Cv should be redetermined using a new FP value based on this updated Cv. Using Cv of 121.7 produces an FP of 0.97, which yields a final required Cv of 116.6. Because 116.6 is very close to the 121.7 used in the prior pass, the sizing procedure is complete. The conclusion: an NPS 4 valve opened to about 75% of total travel should be adequate for the required specifications.


### Go Deeper


For the full procedure, equations, and reference tables behind this walkthrough, visit the[Control Valve Handbook](https://www.emerson.com/en/final-control/catalog/products-and-software/valves/control-valves/control-valve-handbook) .
