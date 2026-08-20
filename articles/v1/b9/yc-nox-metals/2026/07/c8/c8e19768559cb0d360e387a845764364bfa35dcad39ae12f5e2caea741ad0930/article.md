---
schema_version: "1.0.0"
document_id: "c8e19768559cb0d360e387a845764364bfa35dcad39ae12f5e2caea741ad0930"
company_key: "yc-nox-metals"
company: "Nox Metals"
source_id: "yc-nox-metals-news-import-4d0abe59faf1"
canonical_url: "https://noxmetals.co/blog/impossible-tolerance-callouts-on-machining-prints"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-29T03:20:12.621246+00:00"
fetched_at: "2026-07-29T03:20:13.973453+00:00"
content_hash: "sha256:3b21febcd4b470653a44b91f251cb6c17ae5f167b1c9394c2a490eb1120fe16d"
---

# The Print Says Plus or Minus Half a Thou: Reading Tolerance Callouts That Cost You Money

## The Problem Is Pricing, Not Capability


Ask a shop whether it can hold +/-0.0005 in and the honest answer is yes: on the right feature, in the right setup, in a temperature-controlled room, with a gauge resolving a quarter of that. Every clause in that sentence is money. Callouts turn into arguments because a drawing carries a requirement with no channel for carrying its cost, so the requirement travels for free and the cost lands on whoever is holding the part. A tolerance is a purchase and should be quoted like one. Price it the way you price a feature: name the process that has to hold it, the inspection that has to prove it, and whether the number is routine for that process, expensive, or sitting at the edge of it.


## Eight Signs the Print Was Never Priced


These are not signs of a bad engineer. They are signs of a drawing that went from CAD to PDF to email with no manufacturing review in the chain. Any one of them earns a phone call before you quote.


-


No datums, but geometric callouts that control location. Position and profile are meaningless without a datum reference frame under ASME Y14.5. Position 0.005 with no datums means the shop picks the inspection reference, and different picks give different answers on the same part.
-


Trailing zeros implying precision nobody asked for. In inch dimensioning the dimension and its tolerance carry the same number of decimal places, so 2.5000 under a title block that defines only three places is a contradiction.
-


No tolerance and no finish callout anywhere. The most expensive drawing in the pile, because the shop now decides the requirement and will decide in its own favor. Silence on finish is how you get a 125 microinch Ra sealing surface.
-


Flatness demanded on a part too thin to hold it. Flatness is a free-state form control, and a 0.190 in plate part is a spring: flat in the fixture, out of tolerance on the surface plate, and the surface plate is what the inspector uses.
-


A profile tolerance tighter than the size tolerance on the same feature. Profile of a surface is a total zone, bilateral and equally disposed by default, so profile 0.005 means +/-0.0025. Next to a +/-0.005 size tolerance it silently governs, four times tighter than the engineer intended.
-


Every dimension toleranced to the tightest column, just to be safe. Two features usually matter. The other thirty-eight are billed at the same rate.
-


Anodize on a toleranced feature with no growth allowance. Oxide grows on every surface, so a +/-0.001 in bore with a hard coat note has already spent its tolerance. Check the anodize growth calculator at /resources/anodize-growth-calculator.
-


A wall or pocket depth that implies a tool nobody owns. A cutter pushing off 0.004 in at the bottom of a 3.5 in pocket cannot hold a 0.001 in wall. Check the tool deflection and chatter risk calculator at /resources/tool-deflection-chatter-calculator first.


Note


The three cheapest questions on any drawing review: what mates to this feature, which surface does the inspector measure from, and which two dimensions actually matter. If the answers do not exist yet, the tolerances are placeholders.


## What Each Process Can Actually Hold


Below is a working capability table for aluminum. Two of its columns get confused constantly. The practical band is what a competent shop holds all day on a normal part with normal inspection. The best case is the same process on a small feature, in a dedicated setup, with somebody watching. Quoting the best-case column as if it were the practical one is how shops lose money.


Process Practical band Best case, dedicated setup Typical Ra (microinch) Relative cost


Bandsaw cut blank +/-0.010 to +/-0.030 in +/-0.005 in 250 to 1000 1.0


Abrasive waterjet +/-0.005 to +/-0.015 in +/-0.003 in 125 to 500 1.1


3-axis milling, general +/-0.005 in +/-0.002 in 63 to 125 1.0


3-axis milling, careful +/-0.002 in +/-0.001 in 32 to 63 1.3 to 1.8


Turning +/-0.002 to +/-0.005 in +/-0.0005 in 32 to 125 1.0


Reaming +/-0.0005 to +/-0.001 in +/-0.0003 in 32 to 63 1.4


Fine or jig boring +/-0.0002 to +/-0.0005 in +/-0.0001 in 16 to 32 2 to 4


Wire EDM +/-0.0002 to +/-0.001 in +/-0.0001 in 32 to 100 3 to 6


Surface or cylindrical grinding +/-0.0002 to +/-0.0005 in +/-0.0001 in 8 to 32 2 to 5


Honing +/-0.0001 to +/-0.0003 in +/-0.00005 in 4 to 16 4 to 8


Lapping +/-0.00005 to +/-0.0002 in +/-0.00002 in 1 to 8 6 to 15


Note


Two caveats. Published capability charts disagree, sometimes by a factor of two, because Kalpakjian, Machinery's Handbook, and the DFM guides assume different part sizes and different definitions of achievable. Treat every number as the middle of a band. And the grinding, honing, and lapping rows are written for steel: aluminum loads abrasive wheels and smears instead of cutting, so for most aluminum features the practical floor is fine boring, reaming, and wire EDM, not grinding.


## The Cost Curve, and What Each Step Multiplies


From +/-0.030 in down to about +/-0.005 in, tolerance is nearly free in machining, because you are inside the natural capability of the process. Below +/-0.002 in the curve turns over, and it turns over because the cost driver changes. You stop buying a slower feed and start buying operations, gauges, and scrap.


Relative cost of a milled aluminum feature by tolerance band (+/-0.010 in = 1.0)


+/-0.030 in


0.9 x


+/-0.010 in


1 x


+/-0.005 in


1.2 x


+/-0.002 in


1.6 x


+/-0.001 in


2.5 x


+/-0.0005 in


4 x


+/-0.0002 in


8 x


Those bars are midpoints of wide bands: +/-0.005 in commonly lands at 1.1x to 1.3x, +/-0.002 in at 1.4x to 1.8x, +/-0.001 in at 2x to 3x, +/-0.0005 in at 3x to 5x, and +/-0.0002 in anywhere from 6x to 12x. The sources disagree, and they should, because the multiplier is a property of your shop rather than of the tolerance. The list of things you are buying does not vary.


What a tolerance band demands of process variation


sigma required = half tolerance band / 3.99 (for Cpk 1.33) +/-0.0005 in -> 0.0005 / 3.99 = 0.000125 in +/-0.005 in -> 0.005 / 3.99 = 0.00125 in


= One decimal step tighter on the print demands a ten times tighter process. That is the cost curve in one line.


Cpk 1.33The usual aerospace and automotive capability target, roughly 4 sigma to the nearest limit


3.993 x 1.33, the sigma count Cpk 1.33 puts between the process mean and the nearest limit


sigmaStandard deviation of the machined feature across the lot, including thermal and tool wear drift


-


Extra finish and spring passes. Cycle time, on every part in the lot.
-


A dedicated finish tool retired early, because a cutter with 0.0008 in of flank wear cannot produce a 0.001 in feature.
-


A second operation, often with a fixture built for it. This is the discontinuity: when a callout forces a second op the step is 60 to 100 percent in one jump, not 20 percent.
-


Inspection that changes category. A sampled caliper check becomes a CMM routine on every piece. Price that hour on your own floor with the shop rate calculator at /resources/shop-rate-calculator.
-


Scrap and rework. At Cpk 1.33 a +/-0.0005 in band demands a process sigma of 0.000125 in, which you reach by controlling temperature, wear, and fixturing, not by trying harder.
-


Temperature soak and gauge room time, which nobody quotes and the next section is about.


## The Thermal Argument, With Arithmetic


This is the argument that ends most half-thou disputes, because nobody negotiates with arithmetic. 6061 and 7075 both expand about 13.1 microinches per inch per degree F, and 7050 is a hair higher at 13.2. Aluminum is roughly twice as thermally lively as steel at 6.5. Take a 6.000 in dimension and a shop that swings 10 degrees F between the night shift and mid-afternoon, which is an ordinary, well-run shop.


13.1


6061 and 7075 CTE, microinch per inch per degree F


0.00079 in


Growth of a 6.000 in dimension over a 10 degree F swing


1.3 degF


Swing allowed if thermal is held to 10 percent of a 0.001 in band


68 degF


ISO 1 reference temperature for dimensional specification


Thermal growth of a 6 in aluminum dimension


delta L = L x CTE x delta T delta L = 6.000 in x 13.1e-6 per degF x 10 degF delta L = 0.000786 in


= 0.00079 in from temperature alone. A +/-0.0005 in callout is a 0.001 in total band, so the shop swing eats 79 percent of it before the cutter touches the part.


LNominal length of the controlled dimension, 6.000 in


CTE13.1 microinch per inch per degree F for 6061-T651 and 7075-T651, 13.2 for 7050-T7451


delta TTemperature change from the reference condition, 10 degrees F here


68 degFStandard reference temperature for dimensional specification, 20 degrees C under ISO 1


Note


Basis for the CTE values: these are average coefficients of linear thermal expansion over the 68 to 212 degree F range, the figures used by /resources/thermal-expansion-calculator. Instantaneous room-temperature values run slightly lower, around 12.9 microinch per inch per degree F for 6061, so treat 13.1 as a planning number rather than a measured constant for your part.


Run it backwards and it gets sharper. Holding the thermal share to 10 percent of that band means 0.0001 in, which on a 6 in dimension allows an excursion of about 1.3 degrees F. No general-purpose shop holds 1.3 degrees F on the floor. That is a metrology lab requirement, which is why half-thou callouts on long dimensions belong to parts that soak in a gauge room and get priced for it. It also explains a category of inspection disputes: a steel-framed micrometer reading an aluminum part at 85 degrees F carries a differential error, because tool and part expand at different rates. Check your own geometry in the thermal expansion calculator at /resources/thermal-expansion-calculator before arguing about a 0.0004 in disagreement, because both parties are often right and simply measured at different temperatures.


Note


The rule that falls out of this: a tolerance tighter than about +/-0.001 in on a dimension longer than about 4 in is a temperature specification in disguise. If the drawing states no measurement temperature, it has not fully specified the dimension.


## The Material Argument: The Part Moves After the Cut


The second unholdable category has nothing to do with the machine. Rolled plate is quenched after solution heat treating, which locks a residual stress field into the section: compression near the surfaces, tension through the core. While the plate stays symmetric those stresses balance and nothing moves. Machine a pocket, face one side, or cut a thin part from a thick blank, and the part relaxes. Published work puts as-quenched, unstretched 7xxx plate at 15 to 30 ksi of peak residual stress. The stretched Tx51 tempers are pulled 1.5 to 3 percent to knock that down and remove roughly 80 to 90 percent of it. That is a large improvement and it is not zero.


Plate product Stretched Indicative peak residual stress What that means for a tight callout


6061-T651 Yes, 1.5 to 3 percent About 1.5 ksi The most stable rolled option. Still moves on asymmetric cuts.


7075-T651 Yes, 1.5 to 3 percent About 3 ksi Roughly twice the movement of 6061-T651 for the same cut.


7075-T7351 Yes, overaged About 1.8 ksi More stable and more SCC resistant, about 12 percent less strength.


7050-T7451 Yes, overaged About 2 ksi The thick-section standard, holds properties through 6 in.


7075-T6 not stretched No About 14 ksi Worst case. Do not machine plate parts from it.


5000 series cast tool and jig plate Not applicable About 0.3 ksi Cast and thermally stress relieved. Essentially stress free.


Note


Those are indicative peak core tension values for modeling movement, not specification values you can hold a mill to. They are the numbers behind /resources/aluminum-plate-warp-calculator, which predicts bow for your own blank, cut depth, and alloy.


Skim 0.500 in off one face of a 24 by 12 by 1.000 in piece of 6061-T651, the most stable stretched rolled plate on that list, and it bows roughly 0.065 in. That is 65 times the entire 0.001 in band of a +/-0.0005 in callout, from the most benign material and the most ordinary operation in the shop. In 7075-T651 it is roughly double. No spindle or fixture fixes it, because the movement happens after the cut. Every mitigation costs schedule: rough both sides and leave stock, let the part rest, balance the finish passes, keep the cut symmetric about the midplane, or start from a product with no stress to release. That last one is why cast tool and jig plate exists. See the cast tool and jig plate page at /materials/cast-tool-jig-plate.


## Flatness Is the Most Commonly Impossible Callout


Flatness is where the print and the plate collide hardest. Commercial-grade rolled plate allowances are far looser than most engineers assume: a 0.500 to 0.999 in thick plate at 48 in wide is allowed roughly 0.200 in of total deviation over the whole plate, per the distributor-published reproductions of the ASTM B209 and Aluminum Standards and Data tables. Cast tool and jig plate is typically guaranteed near 0.015 in, precision ground plate about 0.010 in per foot. So a 0.002 in flatness callout on a part cut from commercial rolled plate asks a shop to remove a hundred times the incoming error, in a part that loses stiffness with every pass. Check what your material is allowed to be in the plate flatness and bow tolerance chart at /resources/aluminum-plate-flatness-bow-chart first.


Free-state flatness a shop can realistically hold, 12 by 12 in part from rolled plate


0.125 in thick


Do not put a flatness number here. The part is a spring and reads differently in every fixture.


0.250 in thick


0.010 to 0.015 in free state. Tighter means a lapping plate or a shim stack.


0.500 in thick


0.005 to 0.010 in free state. 0.002 in needs a second op and a rest cycle.


1.000 in thick


0.002 to 0.005 in free state. 0.001 in needs grinding or lapping.


2.000 in thick


0.001 to 0.002 in is realistic. Stiffness finally works for you.


Shop-experience bands for a part machined on both sides with no grinding operation, measured free state on a surface plate. Not specification values. A clamped reading always looks better than a free-state reading, and free state is what the inspector takes.


## Title Block Defaults Are a Contract Nobody Read


ASME Y14.5 does not define a default title block tolerance and never has. The decimal-place scheme in your title block is company practice, so two shops reading the same drawing can be legitimately five times apart on what a three-place dimension means. Both schemes below are common and defensible. The gap between them is where the money hides.


Dimension format Common loose scheme Common tight scheme Spread


X.X (one place) +/-0.030 in +/-0.010 in 3x


X.XX (two places) +/-0.010 in +/-0.005 in 2x


X.XXX (three places) +/-0.005 in +/-0.001 in 5x


X.XXXX (four places) Not defined, must be explicit Not defined, must be explicit Undefined


Angles +/-0.5 deg +/-0.25 deg 2x


Fractional +/-1/64 in +/-1/64 in Same


Note


Get the default confirmed in writing, on the PO or in an email you can attach to the job packet. One line does it: we are quoting three-place dimensions at +/-0.005 in and four-place dimensions as noted on the drawing face, please confirm. Confirm loose and you saved the job. Confirm tight and you found the reason the quote goes up, before cutting metal instead of after.


## Be Fair: Some Tight Callouts Are Completely Real


None of this says engineers over-tolerance out of ignorance. Plenty of tight callouts are load bearing, and a shop that pushes back on all of them stops getting invited into the design conversation. Accept these, price them, and do not argue.


-


Bearing and press fits. A 50 mm H7 bore is a 0.025 mm band, about 0.001 in, one-sided. That is the fit class, not padding, and loosening it changes the interference and the load path.
-


Sealing surfaces. O-ring groove depth sets squeeze directly, and sealing surface finish is commonly specified near 32 microinch Ra static and 16 dynamic. Loosen either and the joint leaks. If the print states no finish, predict what your process produces with the surface finish predictor at /resources/surface-finish-predictor, because the surface finish converter at /resources/surface-finish-converter only converts between Ra, Rz, and RMS.
-


Tolerances doing stack duty. A +/-0.001 in feature in a six-part chain may be tight because somebody did the arithmetic. Redo it in the stack tolerance calculator at /resources/stack-tolerance-calculator before proposing a change, and bring the new stack with you.
-


Optical mounts, flexures, and anything that assembles with no adjustment. There is no shim in the design, so the tolerance is the adjustment.
-


Interchangeability. If the part is a fleet spare, the tolerance is not about this part, it is about every part.
-


Anything behind a qualified process. On a first-article-approved drawing under an AS9100 flow, changing a tolerance is an engineering change order, not a phone call.


## What to Actually Do About It


Four moves, in order. None of them is refusing to quote, and none is quoting the tight number silently and hoping.


What a two-price quote is made of, +/-0.005 in versus +/-0.0005 in on the same part


Base machining at +/-0.005 in


100 % of base price 50%


Extra finish and spring passes


22 % of base price 11%


Second setup plus a dedicated fixture


18 % of base price 9%


CMM on every piece instead of a sampled check


30 % of base price 15%


Scrap and rework allowance


20 % of base price 10%


Temperature soak and gauge room time


10 % of base price 5%


200 percent of base, and this is the floor: the published multipliers put the same step at 3x to 5x


200.00 % of base price


-


Ask what the tolerance is for. Not whether it can be loosened, which invites a defensive no. Ask what mates to the feature and what fails if it drifts. Half the time nothing mates to it and the number came from the template.
-


Propose the functional tolerance with a number attached. Never send back a question mark. Send this: at +/-0.0005 in the bore adds a jig bore op and CMM inspection, at +/-0.002 in it runs in the existing setup, and here is the price difference.
-


Offer a two-price quote. Two line items on one PDF, the drawing as written and the drawing with two tolerances relaxed. This converts an argument about capability into a purchasing decision the customer is qualified to make.
-


Confirm the title block default in writing, and the measurement condition with it. Under +/-0.001 in, ask for inspection temperature and method. If nobody has an answer, that is the finding.
