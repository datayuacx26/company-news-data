---
schema_version: "1.0.0"
document_id: "c8e950f4d149423d545dba55571a664ebdb73ab5b553774fb9ca3f1a1632e3b8"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/math-behind-inventory-delivery-optimization"
published_at: "2025-11-18T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:00c2854df4b6e3b03d9a1d07c61afd9085fd23af1c7be484aedd36ebd7bab688"
---

# The Math Behind Target’s Inventory and Delivery Optimization

In today’s e-commerce world, consumers expect products to be available now and delivered quickly. To meet these expectations, companies like Target must carefully decide where to place inventory throughout their supply chain. Getting this right is critical for not only delighting guests but also staying competitive.


At Target, our data science team has developed an advanced decision-support system that helps our engineering, operations, and business teams optimize inventory placement and positioning—making these complex decisions as easy as clicking a button.


What Are Inventory Placement and Positioning— and Why Do They Matter?


Figure 1. Inventory Placement and Positioning


Inventory placement and positioning are two key concepts that shape how products flow through a supply chain:


1. Inventory Placement:


The list of items that should be kept at each warehouse or store location to efficiently cater to guest demand.


2. Inventory Positioning:


The quantity of each item that should be kept at each supply chain location to ensure minimal Out-Of-Stock cases and optimal delivery times.


Getting placement and positioning right means Target can fulfill guest orders quickly and reliably, which builds trust and repeat shopping. It also helps reduce shipping costs by placing inventory closer to the source of the demand. Often, it can help reduce the number of packages shipped, streamlining the delivery process.


Why Inventory Positioning and Placement Matters for Ship-To-Home Items


Let’s look at two cases that highlight the importance of positioning and placement for Ship-To-Home items (products shipped directly to guests):


Figure 2. Cases highlighting the importance of inventory positioning and placement


Case 1:


Consider a scenario where the nearest warehouse corresponding to a guest does not have the items that the guest wants. The order will then be shipped from a warehouse farther away, increasing the expected delivery date (EDD). If the delivery time is longer than the guest’s expectation, they may decide not to place the order. This shows that placement of inventory in each location is critical to meeting guest demand promptly.


Case 2:


Now, consider that the nearby warehouse has only a limited quantity of the item, but the guest wants more. The remaining quantity must again be shipped from warehouses farther away, increasing the EDD. Similar to case 1, if the delivery time is longer than the guest’s expectation, they may decide not to place the order. This shows that not only where the inventory is placed but also how much inventory is positioned in each location is critical to meeting guest demand promptly.


These examples illustrate why using optimization methods to determine inventory positioning and placement can significantly improve the guest experience, reduce supply chain costs, and enhance overall operations.


Mathematical Modeling of the Inventory Placement Problem


From a computer science and operations research perspective, deciding which items to stock at which warehouse—and in what quantities—is a classic optimization problem similar to the “Assignment Problem.” Here’s an overview of how we mathematically model this challenge:


The problem is more complex because businesses want to optimize multiple supply chain objectives at the same time and manage additional constraints, including:


- Volume limits: Warehouses can only store a limited cubic volume of products.


- Item handling: Some warehouses have restrictions on which items they process and store.


- Item affinity: Items typically bought together may need to be stored together to simplify fulfillment.


- Business rules: Operations teams might require certain items, like fast-selling products, to be stocked at specific warehouses to handle demand patterns better.


Challenges of Solving Large-Scale Optimization Models


Figure 3. Some challenges of the mathematical optimization model


Most optimization problems in operations research are categorized as NP-Hard. This means the time needed to solve these types of problems increases exponentially as the problem size increases.


Generally speaking, any optimization model is solved with the help of open-source or commercial optimization solvers. These solvers use specialized algorithms for intelligent pruning of the solution space and arriving at the solution.


Given Target’s scale of operations, the mathematical model described above will contain trillions of variables and billions of constraints which is computationally intractable. Even a commercial solver will potentially take weeks to find the optimal solution of such a problem, assuming we overcome the memory related constraints (optimization models need to be stored on a single machine). Below are a few more examples illustrating the issues that increase the computational complexity of optimization problems:


- Multiple conflicting objective


: For example, fulfilling an order from the nearest warehouse location to minimize delivery time could increase overall shipping cost to the company because of operational reasons. On the other hand, it’s possible that minimizing cost for the company could increase costs or the EDD for the guest. Balancing these competing goals


complicates


finding the optimal solution.


- Model update frequency


: Guest demand changes frequently because of seasonality, market trends, or global events. Deciding how often to run the model is critical to keep plans relevant without overloading computational resources.


- Complex constraints


: Certain business rules add layers of complexity. For instance, a “disjunction constraint” might require that if an item is stocked at a particular location, it must meet a minimum quantity threshold, or else it shouldn’t be stocked there at all. These logical conditions increase the difficulty of solving the model.


How We Reduced the Computational Complexity


As described above, the sheer size of the inventory placement problem is its major challenge. To solve it, we focus on scaling down using intelligent techniques. By doing so we can handle multiple objectives and complex constraints more easily. Here are the key approaches we use:


- Aggregating demand at the regional level


: Instead of planning based on individual guest locations, which is often unpractical because of scale and unpredictable operational factors, we group demand into regions defined by warehouse locations and demand density. Forecasting demand at this level makes the model more robust and manageable.


- Parallelizing the model runs


: We forecast demand at various time granularities (daily, weekly, monthly, etc.). Rather than running separate models one after another, we run them in parallel to save time.


- Reducing the item set with clustering


: Target handles about 200,000 different items throughout its supply chain daily. Treating each item separately to generate an optimal solution would be virtually impossible. To tackle this, we apply novel clustering algorithms to group similar items together, which reduces the problem’s size without sacrificing solution quality.


Figure 4. General ways of reducing complexity in mathematical optimization problems


The Final Computationally Efficient Model


With the techniques described earlier, we were able to reduce the size of the original model—from trillions of variables to a few million—and constraints down to a few hundred thousand. This makes the problem computationally feasible while maintaining accuracy.


The updated model keeps the same basic structure but with these changes:


- The item set is replaced by clusters of similar items, reducing dimensionality.


- Guest locations and demands are aggregated into regions with forecasted regional demand.


- Shipping costs are recalculated for each item cluster across all region-location pairs.


The objective function and the constraints are adjusted accordingly to reflect these changes. For developing a better understanding, we encourage interested readers to formulate the revised model, as it is very similar to the mathematical model described earlier.


Results and Business Impact


We used PySpark, the Apache Spark framework for the Python programming language, to build the computationally efficient model because it handles large datasets effectively. The model was solved on a high-performance machine with sufficient RAM for large-scale optimization. For all the possible use cases with different types of constraints, solution times never exceeded an hour, with most cases completing within minutes.


The business impact has been significant:


- Stockouts at warehouses decreased by about 25-30%, ensuring better product availability.


- Shipping costs were lowered by ~0.4%, which is an enormous improvement in an area that’s already incredibly efficient.


- Excess packaging reduced by about 1.7%.


Additionally, the decision support system integrates seamlessly with Target’s Middle-Mile and First-Mile teams, allowing smooth adoption and driving supply chain improvements.


Enhancing the End-User experience


To make the decision support system valuable to business users, we built in flexible and configurable features that increase usability and model robustness:


- Multiple objectives


: A scoring function balances various goals, such as item affinity and upstream availability challenges, to help optimize for several objectives at once.


- Customizable options


: The tool is designed to adapt to user needs, with features such as:


- Configuring the inventory in the network using attribute- and cluster-level constraints


- Choosing from multiple configurations because of faster run times


- Applying exclusion rules to ensure inventory plans remain consistent from week to week


These enhancements empower Target’s teams to tailor inventory plans to evolving business needs while maintaining reliability.


Conclusion and Next Steps


Our approach has enabled us to solve the inventory placement problem at scale while giving business users the flexibility to adapt their plans to a rapidly changing retail landscape. Looking ahead, we plan to further expand and enhance this methodology by:


- Incorporating more supply chain nodes


: Any supply chain network is made up of different types of nodes, with each node serving a specific purpose. As we incorporate more nodes in the optimization model, the problem size will grow exponentially and may become computationally challenging beyond a certain point.


- Exploring metaheuristic algorithms


: While optimization solvers have difficulties with the scale of the original problem, we plan to investigate the use of metaheuristics, which have shown strong performance on complex problems like ours.


By continuing these efforts, we are confident we will improve Target’s supply chain agility, cost-efficiency and guest satisfaction.


Former Target team member Achal Goyal also contributed to this work.


References


1. [https://www.sciencedirect.com/science/article/abs/pii/S0377221705007137](https://www.sciencedirect.com/science/article/abs/pii/S0377221705007137) (Pentico, D.W., 2007. Assignment problems: A golden anniversary survey.


European Journal of Operational Research


,


176


(2), pp.774-793.)


2. [https://www.sciencedirect.com/science/article/pii/S0377221723005581](https://www.sciencedirect.com/science/article/pii/S0377221723005581) (Pop, P.C., Cosma, O., Sabo, C. and Sitar, C.P., 2024. A comprehensive survey on the generalized traveling salesman problem.


European Journal of Operational Research


,


314


(3), pp.819-835.)


3. [https://docs.mosek.com/latest/cxxfusion/tutorial-djc-fusion.html](https://docs.mosek.com/latest/cxxfusion/tutorial-djc-fusion.html)
4. [https://developers.google.com/optimization/introduction](https://developers.google.com/optimization/introduction)
5. [https://github.com/coin-or/pulp](https://github.com/coin-or/pulp)
6. [https://www.gurobi.com/resources/open-source-mixed-integer-and-linear-programming-solvers/](https://www.gurobi.com/resources/open-source-mixed-integer-and-linear-programming-solvers/)
7. [https://cloud.google.com/vertex-ai](https://cloud.google.com/vertex-ai)


## RELATED POSTS


### Predictive Modeling for Availability of Inventory


By Sohini Mitra and Atul Goel, July 24, 2024


Read more on how Target's data sciences team uses data segmentation to improve product availability


### Elevating Guest Repurchasing Behavior Using Buy It Again Recommendations – Part 2


By Amit Pande, Pushkar Chennu, David Relyea, Rankyung Park, and Prathyusha Kanmanth Reddy, March 4, 2025


This article introduces a new model for BIA recommendations using item- and category-level Hawkes processes.
