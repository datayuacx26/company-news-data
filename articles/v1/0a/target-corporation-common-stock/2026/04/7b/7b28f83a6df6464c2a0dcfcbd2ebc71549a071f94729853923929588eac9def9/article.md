---
schema_version: "1.0.0"
document_id: "7b28f83a6df6464c2a0dcfcbd2ebc71549a071f94729853923929588eac9def9"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/balancing-speed-and-accuracy-digital-fulfillment"
published_at: "2026-04-09T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:c9882707455837c2b5737df6ae8ac68475297ed5ceca0d35be6a1c0044021805"
---

# Balancing Speed and Accuracy in Digital Order Fulfillment: The Next-Gen Solution

Sometimes your online Target order arrives in a single tidy box, while another shows up in two or three deliveries. Why is that? Behind that experience is order allocation, the decision engine that figures out where your items are sourced, how they’re grouped, and how quickly they get to your door. In just seconds it weighs inventory availability, delivery speed, shipping cost, packaging constraints, and business priorities to fulfill orders efficiently and cost-effectively — at massive scale.


Imagine a guest orders diapers, paper towels, headphones, and sneakers. Do all four ship together from one store or distribution center, or do some items come from different locations? Sometimes one box is cheapest and fast enough. Other times, splitting the order into two shipments will allow the items to sooner and cost less overall.


Order allocation is the science of delivering on time at the lowest cost. The objective is simple to say and hard to do: minimize total ship cost while meeting the expected delivery timeline.


In this post, we’ll explain how allocation makes those calls. We’ll explore the trade-offs it considers and the algorithmic fine-tuning that helps us move faster, reduce cost, and improve the guest experience at scale.


Traditional approaches come in two common flavors of algorithms: brute force (enumerate all valid solutions; exact but combinatorially explosive) and greedy (choose the locally best move; fast but myopic). We’ve relied on both, but at scale one fails on time and the other on solution quality.


Recognizing these challenges, Target's data science team developed an innovative, hybrid approach that balances speed and efficiency. It uses advanced algorithms to handle complex fulfillment decisions quickly while ensuring alignment with business objectives, improving cost-effectiveness and guest satisfaction.


Figure 1: A schematic representation of the various digital Fulfillment process at Target


Today, we use brute force search to find optimal solutions for small online orders. As the item count grows, we switch to a greedy heuristic that minimizes shipments—it’s fast, but it can miss cheaper options. Our hybrid allocator closes ~91% of the fulfillment-cost gap between brute-force and greedy approaches while using only ~8% of the brute-force compute time—and it still meets the delivery promise. Below, we unpack that gap and how the hybrid balances speed with solution quality.


Shortcomings of the Brute Force Method


The brute force approach (Figure 2, Ref: 1) involves exploring all possible fulfillment options to identify the most cost-effective solution. The process comprises four steps:


1. Shipment Generation:


Each shipment represents either a single item or a set of items that can be sourced from any fulfillment center. For an order containing two items, the items can either be supplied separately from two different fulfillment centers (two shipments) or grouped and supplied from a single fulfillment center. For example, with two items


(I1, I2)


, the three potential shipments are:


{I1}, {I2},


and


{I1, I2}


. From these shipments, two possible fulfillment options can be created:


\[ {I1}, {I2} \]


and


\[ {I1, I2} \]


. This step systematically generates all potential shipment combinations for an order.


2. Optimal Node Allocation:


Each generated shipment can be supplied by multiple fulfillment centers, each with an associated fulfillment cost. These costs might include factors like shipping charges and additional business-specific priorities, such as location-based costs, item-location priority, or specific cost weights. The algorithm assigns the most cost-effective fulfillment center to each shipment, ensuring the lowest possible shipping cost for every shipment.


3. Fulfillment Option Generation:


After selecting the optimal node (fulfillment center) for each shipment, the algorithm proceeds to generate all valid combinations of shipments to ensure complete order fulfillment. This step considers all permutations of assignment to meet the entire order's requirements.


4. Optimal Fulfillment Selection:


From the generated fulfillment options, the approach selects the one with the least total cost, ensuring that the complete order is fulfilled most cost-effectively, considering both individual shipment costs and overall fulfillment costs.


This brute force method systematically examines all possible combinations to arrive at the solution that minimizes the total fulfillment cost.


Figure 2: Brute force method for Order Fulfillment


As the number of items in an order increases, the complexity of the brute-force approach (Figure 3) to fulfillment allocation grows in two key ways:


- Number of Shipments:


The number of possible shipments grows exponentially with the number of items. For an order with n items, the number of non-empty subsets (shipments) is 2


n -1, which increases rapidly. For instance, 5 items result in 31 shipments, while 10 items yield 1,023.


- Number of Fulfillment Options:


The number of possible fulfillment options is determined by the Bell Number, B


n , which represents the number of ways to partition n items into non-empty subsets.


Figure 3 : Complexity of the brute-force approach


As the number of items and fulfillment centers increases, brute-force methods become computationally impractical, requiring optimization heuristics to handle larger orders efficiently.


Shortcomings of the Greedy Approach


The greedy approach is a problem-solving method that makes locally optimal choices at each step with the hope of finding a globally optimal solution. It doesn't backtrack or reconsider previous decisions, prioritizing speed and simplicity. While it doesn’t always guarantee the best solution, it often provides an efficient approximation in a fraction of the time required by exhaustive methods. This makes it especially useful for large, complex problems where computational resources are limited, and quick decisions are necessary.


When dealing with large orders, the complexity of reducing shipping costs can be overwhelming. A common greedy method focuses entirely on minimizing the number of shipments, as fewer shipments often correlate with lower costs. Below is how:


Figure 4 : Greedy Approach in fulfillment.


1. Initial Factor Calculation:


Each fulfillment center is evaluated based on the number of items it can provide. This availability factor (AF) acts as a guide for assigning items.


2. Create First Shipment:


The fulfillment center with the highest AF is chosen, and a shipment is created by assigning as many items as possible to that center.


3. Reevaluate Factors:


After the first shipment, the AFs are recalculated for all centers, considering only the remaining unassigned items.


4. Repeat the Process:


Steps 2 and 3 are repeated iteratively, creating shipments until all items are assigned to a center.


The greedy approach in fulfillment allocation minimizes shipments by assigning items to fulfillment centers based on availability. However, in reality, the fulfillment cost function is stepwise, with costs drastically increasing after certain thresholds. As a result, this method often leads to suboptimal outcomes. While quick and efficient, it may overlook dynamic costs and nuanced factors, leading to higher expenses.


An optimal solution considers stepwise cost jumps, strategically distributing items to avoid triggering expensive thresholds. Though computationally efficient, the greedy approach focuses on local optimization and may miss more cost-effective solutions achievable by other techniques.


A Smarter Path with the Hybrid Allocation Method


As we have seen in the previous two sections, both brute-force and greedy approaches have significant limitations in effectively solving the fulfillment allocation problem. From an optimization perspective, the fulfillment problem is a typical set-covering problem (Figure 5), where the objective is to fulfil a set of items in order by selecting from multiple shipment options with associated costs. The goal is to create the most cost-effective fulfillment option by optimally combining available shipments.


Figure 5 : An illustration of the hybrid approach.


Based on our research, local search algorithms are best suited for efficiently solving this problem while balancing computational complexity and cost optimization. Local search begins with an initial feasible solution, often derived using the greedy approach, and then explores neighborhood solutions to improve the cost function. This hybrid approach leverages the speed of the greedy method and refines the solution with local search. Local search helps refine the initial fulfillment option (FO) generated using a greedy approach by iteratively improving the solution through two main techniques: Set Reassignment and Set Merging.


In Set Reassignment, elements within sets are reassigned to reduce costs:


- A new set


S1'


is created by reassigning elements from


S1


to other sets


S2, S3, ..., Sm.


- Each feasible element reassignments of


S1


is evaluated based on cost improvement,


ΔC


. For example reassigning element of


S1


to set


S2


,


ΔC = C(S1) - C(S1') + C(S2) - C(S2')


.


- If


ΔC > 0


, the move is executed, progressively optimizing the FO.


- This process continues until no further cost reductions are possible, after which the algorithm moves to the next set and repeats the procedure.


By iterating through set reassignments, smaller incremental cost reductions accumulate, leading to a more optimized FO.


In Set Merging, entire sets are evaluated for potential consolidation:


- A set


S1


is considered for merging with another set


S2


if the combined cost is lower than the individual costs, i.e.,


C(S1 ∪ S2) < C(S1) + C(S2).


- If merging results in a cost improvement, the sets are merged into a new set


S1' = S1 ∪ S2


.


- If merging doesn’t yield savings


(ΔC ≤ 0)


, the sets remain unchanged, and the process is repeated for other sets.


By Set Reassignment and Set Merging, local search systematically refines the FO, achieving a balance between feasibility and cost minimization. This hybrid approach leverages the initial speed of greedy search and the thoroughness of local adjustments to achieve an effective and optimized solution for the fulfillment problem.


Outcomes and Path Forward


Today, brute force methods help us find optimal solutions for smaller orders. But as order size grows, we rely on greedy heuristics that prioritize fewer shipments—trading off cost efficiency for speed. Our hybrid approach bridges that gap.


It recovers 91% of the fulfillment cost difference between brute force and greedy methods, while using just 8% of the compute time required by brute force—all while meeting delivery timelines. In other words, we’re no longer forced to choose between speed and cost because we can effectively balance both.


Looking ahead, we’re exploring parallel, multi-start local search techniques to evaluate more high-quality solutions within the same latency. This will help the system move beyond local optima, further reducing unnecessary shipment splits and improving on-time delivery.


So the impact is smarter allocation decisions, more consistent performance at scale, and a better experience for guests, no matter how complex the order.


Reference:


1. Nagarajan, V, “Set Cover (Greedy) and Matching (Local Search)” \[Lecture Notes\], January 18, 2017,


[https://viswa.engin.umich.edu/wp-content/uploads/sites/169/2016/12/lec4.pdf](https://viswa.engin.umich.edu/wp-content/uploads/sites/169/2016/12/lec4.pdf)
2. Agrawal, A, Pradhan, A, Chokhani, R, “Method For Cost Efficient Fulfillment” \[Patent\], June 17, 2015,


[https://patents.justia.com/patent/20160371629](https://patents.justia.com/patent/20160371629)


## RELATED POSTS


### The Math Behind Target’s Inventory and Delivery Optimization


By Vinayaka Yaji, Rajshekhar Singhania, and Aashimi Bhatia, November 18, 2025


Learn how Target leverages advanced mathematical models to optimize inventory placement and speed up delivery.
