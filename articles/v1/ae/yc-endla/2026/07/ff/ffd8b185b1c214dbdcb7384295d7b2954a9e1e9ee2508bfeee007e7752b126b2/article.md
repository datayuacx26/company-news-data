---
schema_version: "1.0.0"
document_id: "ffd8b185b1c214dbdcb7384295d7b2954a9e1e9ee2508bfeee007e7752b126b2"
company_key: "yc-endla"
company: "Endla"
source_id: "yc-endla-news-import-c259074e8750"
canonical_url: "https://endla.com/stories/packer-placement/"
published_at: null
first_seen_at: "2026-07-23T08:41:52.802729+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:f3ec15eed85819ba723c42e3d325e86344993e2afdec38852e1b2fec47fbd917"
---

# Packer Placement Zone Analysis

Swellable packers are used to achieve isolation in multi‑zone coal seam gas completions. The packers can only be placed on constrained regions of the casing parts – due to part and/or practical limitations. Simply, we can consider this constraint to be if a packer can be placed on the bottom end of a part (pin end), top end (box end). Generally, a packer can be placed on the pin end of any part. However, the part, packer type, completion design and rig practicalities determine if a packer can be placed on the box end of a given part.


## More packer zones gives better results


Relaxing the constraints of packer placement by increasing the number of possible placement zones will result in improved completion performance and greater savings. This is due to the better interburden isolation provided by more possible packer locations for a given part count. More performant isolation leads to less solids production in wells, returning savings over the life of the well, particularly in regions where solid productions are a known issue. However, to make a decision about the allowable packer placement zones the benefit of adding more zones versus the cost of doing so needs to be quantified.


We’re able to quantify this benefit using our software, and in particular with a tool we call AlphaSpace. AlphaSpace is a powerful solution space analysis and optimization tool that sits on‑top of our casing tally generation and isolation tools. Here we take a preliminary look at the effects of swellable packer placement options. Five different options of packer placement were explored, these scenarios have been tabulated below:


## Quantifying the improved performance


These scenarios have been explored on four different real‑world wells previously drilled by Senex that we have investigated as part of our ongoing collaboration. Using AlphaSpace we found the highest value tally for each well under each scenario. We’ve presented this information as savings against the baseline scenario of packer zones only on pin ends. As seen in the graph each incremental relaxation of the constraint on allowable packer placement zones on the parts improves the value of the completion.


Savings vs baseline across packer zone scenarios.


As expected the more flexible the packer placement zone philosophies lead to greater savings. There is a big step in performance up to allowing packer placement on all >3m length parts. The real benefit of running with packer placement zones on blank pup boxes and/or slotted part box ends is in the upper quartile cases, where the position of the coals results in challenging spacing.


## Explaining the savings


Looking at the results we see a pin end only packer placement philosophy leading to a good well completion relies on the spacing between isolated zones “fitting” to the sizes of joint in inventory. An example situation for this is where a blank joint has been positioned just above an exposure zone, which is then spanned nicely by a single slotted joint.


However, optimal production zone groupings do not always result in ‘nicely’ spaced packer placement targets. Below is an example of box packer placement zone on the joints improving the isolation and reducing the part count.


Generally, if you can combine the joints on hand to get the pin ends close to the desired isolation zone boundaries, the completion will perform well, for pin end only packer placement. This situation is less common when targeting higher isolation in the completion as more production zones are present, which naturally makes achieving alignment of pin ends in all production zones more challenging.


## Savings warrant further investigation


In practical terms, this means that investing into more diverse packer placement techniques will likely have greater returns on wells with greater interburden isolation requirements. The initial assessment indicates it would be beneficial for operators to conduct this analysis across their history of wells with their own economics, well‑design and equipment constraints applied. We hypothesize an optimal implementation is a by‑needs hybrid placement strategy that uses a slide on packer as a default with a simple to place wrap‑around packer type for use where it will save on part count.


> Disclaimer: We modelled the economics of the savings presented using our estimates of market averages and not reflective of any true costs or values. Therefore, the information presented should not be considered in any decision making. The purpose of the presented information is to highlight packer placement strategy as an important consideration. For this reason, Endla recommends development bespoke analysis of packer placement philosophies for a given development prior to any decision making.
