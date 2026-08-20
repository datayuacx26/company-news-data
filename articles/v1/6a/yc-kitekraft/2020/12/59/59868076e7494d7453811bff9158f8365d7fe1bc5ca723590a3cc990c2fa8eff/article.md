---
schema_version: "1.0.0"
document_id: "59868076e7494d7453811bff9158f8365d7fe1bc5ca723590a3cc990c2fa8eff"
company_key: "yc-kitekraft"
company: "Kitekraft"
source_id: "yc-kitekraft-news-import-fa2ef2c401a3"
canonical_url: "https://www.kitekraft.de/blog/how-to-develop-flying-wind-turbines-quickly-and-cost-efficiently"
published_at: "2020-12-14T00:00:00+00:00"
first_seen_at: "2026-07-24T01:33:59.655318+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:46fbd5db488e90a3cd0bb40c5ea1e6b4ed2f45e8d67bf11663f60d063bf1334d"
---

# How to Develop Flying Wind Turbines Quickly and Cost-Efficiently?

In August this year, we celebrated the milestone of[flying figure eights for the first time with our kite](https://www.kitekraft.de/blog/kitekraft-completes-flight-in-figure-eights) . In this blog post, we share some of the ingredients that enabled this fast pace development.


## Ingredient: Airframe Made of Aluminum Extrusions


This may come as a surprise to some readers: the Kitekraft kite currently has no single piece made from carbon fibers. Instead, the main airframe wings are made from customized aluminum extrusions. Below is a photograph we took in the wind tunnel with the shiny airframe visible.


Kitekraft engineers Chris, Flo, and André in the wind tunnel with the kite’s shiny aluminum main wings.


The benefits of aluminum extrusions over the alternatives like carbon fiber or similar compound structure approaches include:


- Low costs: Even with our currently low-volume manufacturing and the need for a tool for the customized aluminum extrusion, it is rather cheap. Once series production has started, the costs of the wings for a kite will fall further rapidly.
- Fast and safe design: Aluminum has the same strength in all directions and cannot de-laminate. There is also no need to worry about “the right” fiber directions.
- Fast prototyping: Aluminum allows quick changes and quick iterations, e.g., drilling an additional hole for some cables or attachments whose need was found after the first tests. This is hardly possible with a carbon fiber wing as damaged fibers can impair the strength of the entire wing.
- Tight tolerances: The airfoil shape can be produced with below-millimeter accuracy and there is no bonding edge.
- Product life cycle: Aluminum can be recycled easily. This avoids the recycling or landfill issues faced by conventional wind turbines when they reach their end of life ([Bloomberg article](https://www.bloomberg.com/news/features/2020-02-05/wind-turbine-blades-can-t-be-recycled-so-they-re-piling-up-in-landfills) ).


Every engineering decision comes with certain benefits and drawbacks. The first definitely outweigh the latter at the current stage: The planform of the wing can currently only be rectangular, which is however not a big problem with our design approach (see[here](https://doi.org/10.1016/j.renene.2017.10.073) for details). The mass of the kite may also be slightly higher than that of an optimized carbon fiber design, but the kite mass does not have a high impact on the efficiency. Another drawback is the lower fatigue life of aluminum compared to glass or carbon fibers, which however can be addressed by using adapted geometries or harder alloys for parts with high stress.


We have just produced an optimized two-element airfoil for our next-generation kite, also made from customized aluminum extrusions. We believe it is possible to use this low-cost approach also for much larger kites, which ultimately helps to generate clean electricity at a very low cost.


## Ingredient: Parachute Recovery


It is important to test improvements quickly under real conditions. This is why we flew our prototype very early in hovering mode with a 100m long tether and 800V power transmission to test the powertrain, communications, and other subsystems. For that, we secured the kite with a modular truss tower, visible in the photograph below. Once there was a fault, the kite was caught by the tether and the safety line.


Kite during hovering, secured by a 15m high modular truss tower (made with a truss system normally used for stages), a safety line, and the tether. This concept allows that the kite can even fly higher than the tower — the maximum it reached was around 25m.


For flying figure eights, however, such a concept is not feasible. Our solution to this is a ballistic parachute. Below are video clips showing the parachute saving the kite. The parachute can be released by the operators or by the flight control software in case it detects a critical malfunction. The parachute has proven to be effective, both at low speeds during launch as well as at high-speed crosswind flights. Two occasions in which the parachute rescued the kite after malfunctions. 1st: during launch, 2nd: during high-speed crosswind-flight.


Parachute activation during launch.


Parachute activation during flight.


The parachute mainly enables fast testing and iterating as well as keeping the development costs low. Once we have all critical parts of the system fully redundant, which is currently being developed for the next-generation kite, and once enough successful flight hours have validated the robustness and reliability, a parachute will not be required anymore.


## Further Ingredients


These are just two of many ingredients that helped us get where we are right now as fast as we did: completing our first figure eight flight roughly a year after company foundation. Of course, previous research of the team at Technical University of Munich gave us a kick-start and will play a huge role in future developments.


Stay tuned for further updates!


‍


Posted on


December 14, 2020


in


[Technology](https://www.kitekraft.de/blog-categories/technology) category
