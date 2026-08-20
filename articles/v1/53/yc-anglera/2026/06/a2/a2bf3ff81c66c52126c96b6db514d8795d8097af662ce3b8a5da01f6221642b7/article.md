---
schema_version: "1.0.0"
document_id: "a2bf3ff81c66c52126c96b6db514d8795d8097af662ce3b8a5da01f6221642b7"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/cold-start-forecasting-new-products"
published_at: "2026-06-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:f269ee93cb05e51a83d2cc5ea08ce8defc71fe59c174b619d751ecd2c09dc5cf"
---

# Forecasting a product with no history: cold starts and attribute similarity

A buyer commits to 4,000 units of a new SKU before a single one sells. The forecast that justified that number didn't come from the product's own history, because it has none. It came from some other product's history, borrowed and rescaled. The entire cold-start forecasting problem, whether it's done by a merchandiser eyeballing "last year's version" or a machine learning model scoring a thousand candidates, reduces to one question: which existing item is this new one actually like? Get that match right and the borrowed curve is a reasonable proxy. Get it wrong and you've planned inventory against a demand pattern that has nothing to do with the product in the box.


## Like-item forecasting is older than the software


Planners have always done this manually. A new colorway gets mapped to "the navy version from two seasons ago." A new SKU in a category gets assigned the average sell-through of its "class." This is why merchandising teams maintain reference or "like-item" lists in the first place: someone has to decide, by hand, what a brand-new item resembles before week one demand exists to check the guess against.


The problem with manual like-item selection is that it's implicit and unaudited. The planner making the call is running a similarity judgment in their head, based on whatever attributes happen to be visible to them at that moment: category, maybe price, maybe a photo. Two planners looking at the same new item can pick two different analogs and neither one can point to exactly why. When the forecast misses, nobody can trace the miss back to the matching decision because the matching decision was never written down as a rule.


## What machine learning changes, and what it doesn't


Algorithmic approaches to the same problem make the matching step explicit, which is both the appeal and the exposure. A common pattern: represent every SKU as a vector of attributes (category, material, price tier, silhouette, size run, launch timing, sometimes computer-vision features pulled straight from product imagery), then use nearest-neighbor search or clustering to find the historical items closest to the new one in that attribute space, and forecast the new item as a similarity-weighted blend of their curves.[Impact Analytics describes this pattern directly](https://www.impactanalytics.ai/blog/ai-retail-demand-forecasts-cold-start-modeling-for-new-retail-products) : AI clustering groups SKUs on attribute data, pricing, launch dates, and store penetration, then infers the new item's pattern from its cluster-mates, layering a Bass-style adoption curve on top to scale it. The vendor reports new-product forecasts running 25 to 30 percent more accurate than prior judgment-based methods across the fashion retailers it studied.


Academic work is pushing the same idea further. A 2025 dual-phase framework combining ML-based classification with similarity-driven analog forecasting and residual correction reported a 35.7 percent reduction in mean absolute error and a 41.8 percent improvement in residual stability against conventional ARIMA and plain analog methods, according to[research published in Mathematics (MDPI)](https://www.mdpi.com/2227-7390/13/10/1613) . More recent generative approaches go further still: a conditional diffusion framework for new-product life cycles,[described in a 2026 arXiv preprint](https://arxiv.org/abs/2604.20370) , explicitly fuses three inputs — static descriptors like category, price tier, and brand identity, reference trajectories borrowed from analogous products, and whatever early sales signal has trickled in since launch — into a full predictive distribution rather than a single point forecast.


Strip away the modeling machinery and the mechanism is identical across every one of these: a distance function computed over attributes decides which history gets borrowed. The model is more rigorous than a planner's gut call, but it inherits the exact same failure mode. It just fails at scale, silently, across every new item launched that quarter instead of one planner's picks.


## Borrowing the wrong curve


Attribute-based matching breaks in a specific, recurring way: two items get scored as similar because they agree on the attributes the system actually captures, while differing on the attributes that actually drove demand.


Consider a new dress classified only by category, price band, and color. Its true analog, in terms of how it will sell, might turn on silhouette and formality: a going-out mini and a relaxed daytime maxi are both "dresses" at $68 in navy, but they serve different occasions, different seasons, and different velocity curves. If silhouette and occasion aren't attributes in the system at all, or are buried in an unstructured description field the matching model never parses, the algorithm has no way to tell them apart. It picks whichever historical dress is closest on the attributes it has, borrows that curve, and the forecast is confidently wrong in a way nobody flags until the sell-through report comes in.


The same failure shows up anywhere attribute coverage is thin: a running shoe matched on brand and price while its cushioning platform and upper construction, the two things that actually predict repeat-buy velocity, sit as free text or blank fields; a private-label SKU whose spec sheet has a typo in the fiber content, quietly nudging it into the wrong cluster. None of these are model failures. They're data failures wearing a model's confidence.


## What a richer, validated attribute set buys you


Matching on Typical attribute depth What gets missed


Category + price only 2-4 fields Silhouette, materials, use-case, construction


Category + price + free-text description 2-4 structured fields, unparsed text Whatever isn't tagged consistently across SKUs


Full validated attribute set 15-30+ structured fields, quality-scored Little; disagreements between sources get flagged instead of hidden


The fix isn't a better algorithm. It's a wider, cleaner set of attributes to run the distance function over, and confidence that those attributes are actually correct. A model with silhouette, closure type, material composition, and use-case as real structured fields, extracted from tech packs and product imagery rather than left as prose, can distinguish that mini from that maxi before it ever pulls a historical curve. And when two source documents disagree on a spec, that needs to surface as a flag for a human to resolve, not get averaged away or silently overwritten, because a wrong-but-confident attribute is worse for a similarity match than a missing one.


This is the part of forecasting infrastructure that gets the least attention and does the most damage. Planning teams invest in better algorithms while the attribute layer underneath, the actual definition of "similar," stays thin, inconsistent, and unvalidated across the catalog. Anglera doesn't build forecasting models or replace the planning tools that run cold-start logic. It sits underneath them, extracting and validating the attributes those models match on, so that when a new item borrows a demand curve, it's borrowing the right one.
