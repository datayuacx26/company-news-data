---
schema_version: "1.0.0"
document_id: "df66db57a5a78efc4e2caeef3c939b7c478c8c6a6bd6bfdeed2d31fbcd5cc066"
company_key: "yc-ionworks"
company: "Ionworks"
source_id: "yc-ionworks-news-import-f340ab61a31b"
canonical_url: "https://ionworks.com/blog/full-3d-battery-simulation-available"
published_at: "2026-04-17T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:30.651124+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:247b1e532bafa2ad80b9c50348027a567cfa76f78233a8ec154ec6fb1eb2da5a"
---

# Full 3D battery simulation is now available in Ionworks

A 1D[physics-based battery model](https://ionworks.com/models/types/electrochemical) tells you how a cell performs. It resolves the electrochemistry through the electrode stack and predicts voltage and capacity fade under load. For most questions during early[parameterization](https://ionworks.com/blog/battery-parameter-estimation) and chemistry screening, that is enough.


At some point the question changes. It stops being "how does this cell perform" and becomes "where in this cell does performance break down." That is a spatial question, and 1D cannot answer it.


Full 3D physics-based battery simulation is available in Ionworks, built on` PyBaMM` . If you already have a parameterized model, you can run it in 3D without rebuilding anything.


## What 1D cannot tell you


[DFN and SPMe](https://ionworks.com/models/types/electrochemical) resolve electrochemistry through the thickness of the electrode sandwich. They do this well. The assumption baked into both is lateral uniformity: every point across the cell face sees the same current density, potential, and temperature.


The assumption holds when the cell is small, the tabs are centered, and the C-rate is modest. It starts to fail when any of those change.


### Spatial gradients and current distribution


In a real pouch or prismatic cell, current enters through the tabs and spreads across the current collectors before passing through the electrode stack. Current density is highest near the tabs and decays toward the far edges of the cell.


A 1D model averages this distribution away entirely. It treats the full electrode area as if it were uniformly loaded. For a large-format cell at high rate, the difference between local current density near a tab and at the opposite corner can be substantial, and that gradient drives real differences in local overpotential, lithium concentration, and aging.


### Tab location and internal resistance


Tab position directly changes the effective current path through the collectors. A top-top configuration forces current to traverse the full collector length. A top-bottom or diagonal arrangement shortens the average path and reduces ohmic losses.


Tab placement has a measurable effect on[DC internal resistance](https://ionworks.com/blog/acir-vs-dcir) : two otherwise identical cells in different tab configurations produce different resistance profiles under the same operating conditions. A 1D model has no representation of tab geometry, so it cannot distinguish between these configurations at all.


### Thermal hotspots


Heat generation in a cell is proportional to local current density. Where current concentrates, heat generation is highest. In a pouch cell, that means the region near the tabs runs hotter than the rest of the cell.


A lumped[thermal model](https://ionworks.com/models/types/thermal) reports one temperature: the average. A 1D coupled thermal-electrochemical model can resolve through-thickness gradients but still assumes lateral uniformity. Only 3D predicts where the hotspot actually forms, and the temperature difference between that hotspot and the cell edge can be large enough to shift local[degradation rates](https://ionworks.com/models/types/degradation) .


### Large-format cell behavior


Pouch and prismatic cells with large electrode areas are where these effects compound. Lateral gradients in current density create lateral gradients in state of charge. One region of the cell can be at 85% SoC while another sits at 70%.


Those SoC gradients drive non-uniform aging. The high-current region near the tabs ages faster. Under cold-temperature fast charging, it is also the region most likely to[plate lithium](https://ionworks.com/models/types/degradation) first, because local overpotential is highest there. A 3D battery simulation captures all of these coupled spatial effects. A 1D model, by construction, cannot.


## 3D simulation, available in Ionworks


Ionworks supports full 3D electrochemical-thermal simulation. The solver couples` DFN` -class electrochemistry at each point on the cell face with 3D current conduction through the collectors and 3D heat transport, built on[PyBaMM](https://ionworks.com/solutions/pybamm-for-battery-teams) and the same underlying architecture described in[our PyBaMM vs COMSOL comparison](https://ionworks.com/resources/pybamm-vs-comsol-battery-simulation) .


You define your cell geometry (tab locations, electrode dimensions, collector properties) and run the same physics you have already been using for 1D studies. The equation set is` PyBaMM` , the parameter set is the one you already[fit in Train](https://ionworks.com/solutions/train) , and the environment is the one your team already works in. Dimensionality becomes a simulation setting rather than a tooling change.


Live simulation · available in Ionworks


### Full 3D DFN over a 1C discharge and relaxation


A browser-rendered time series from a full 3D DFN simulation of a pouch cell, discharged at 1 A for 30 minutes and then relaxed. Step through the profile to see where the thermal response localizes and how the current load drives it.


Loading` /models/pouch_dfn_polydata/pouch_dfn_0000.vtk`


Applied current


t=0s · I=1.00A


0.00 A


1.00 A
