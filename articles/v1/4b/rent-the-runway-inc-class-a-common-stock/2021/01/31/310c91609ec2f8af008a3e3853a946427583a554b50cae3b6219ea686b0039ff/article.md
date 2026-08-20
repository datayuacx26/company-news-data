---
schema_version: "1.0.0"
document_id: "310c91609ec2f8af008a3e3853a946427583a554b50cae3b6219ea686b0039ff"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/howtobuytherunway"
published_at: "2021-01-08T21:12:21+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:90cebf0e3534eac0f3107f9faa5a5886223d76c256db22c966b385e9f0fbb0b6"
---

# How to Buy the Runway at Rent the Runway

Is it better to rent or buy? At Rent the Runway, this isn’t a just question we ask ourselves when our apartment leases are ending. Our primary business is renting out clothing through our monthly subscription platform, but resale is an increasingly popular member benefit. Today, our customers can rent from our virtual closet for almost any occasion ranging from[yoga class, to ski vacation](https://www.forbes.com/sites/margauxlushing/2019/12/04/rent-the-runway-launches-athleisure-and-ski-apparel-just-in-time-for-ski-season/?sh=7c66f02642ef) , to a high-powered executive luncheon. Most of the time our customers return their rentals, but if they really love them they can choose to purchase them. Now, customers can even elect to buy a secondhand piece at any point in their journey, even before it’s at home with them.


As part of the data science team, I work on setting fair purchase prices. We think about selling items for a fair price from a few perspectives. First, we want to deliver the best possible virtual closet to our subscribers, which means we don’t want to sell too much of what our subscribers love to rent. And second, we intend to rent out our clothing for a significant period of time.


Achieving these goals requires a granular understanding of both purchase price elasticity and expected future rental demand for every style. Balancing rental demand and price elasticity helps us maintain a high-quality virtual wardrobe for our customers by keeping newer and more highly demanded items on-site while selling well-loved clothing at attractive prices.


If you are unfamiliar with our business model, the image below illustrates the state of my subscription today (Figure 1). I've rented these four items and have them at home. Each of them has an associated purchase price, and I can decide whether to return or keep each one individually. I am pretty tempted to buy that first dress at a 50% discount, and I’ll keep an eye on that second dress, which has only a 10% discount.


Figure 1: The current state of my RTR subscription (brands redacted) showing what clothing I have the option to return or purchase.


### Price Elasticity


Price elasticity is a measure of the change in the quantity demanded or purchased of a product in relation to its price change. As the price increases customers are generally less likely to make a purchase, but the magnitude of this change in demand varies by product. Intuitively, the factors that matter the most for price elasticity are:


-


Availability of substitutes


-


Share of income (Percentage of personal income needed for purchase)


-


Whether the good is luxury or necessity


-


How narrowly-defined the market is (For example, food is a broad market without substitutes. Vanilla ice cream is a very narrow market with many substitutes.)


Rent the Runway’s inventory mix is such that we see fairly high price elasticity.


Traditional approaches to understanding price elasticity use regression models to predict how price affects the quantity sold. There are many examples and tutorials online about how to apply these approaches to commodities like milk and beef. However, we found these traditional approaches challenging to apply to our use case. Primarily, RTR was founded as a rental business, so sales data has historically been limited. Secondarily, our virtual closet has a large breadth of fairly heterogeneous styles and we rent them over time, typically several years. Modeling that well requires a lot of decisions about how to build taxonomies, what time frames to consider, and how to manage outliers. Below you can see how nicely this regression approach works for US beef sales, compared to a less satisfying approach using our sales data (Figure 2).


Figure 2: Thank you to Susan Li for a great[tutorial](https://towardsdatascience.com/calculating-price-elasticity-of-demand-statistical-modeling-with-python-6adb2fa7824d) on modeling price elasticity with Python. On the left, we have regression plots for US quarterly beef sales from her blog. On the right, is a sample of the quality of regression plots we made with our own sales data which produced pretty dismal results.


Luckily, we are in a unique situation where we can convert our pricing regression problem into a classic machine learning classification problem. Instead of trying to predict the volume of sales, we can predict the probability an individual rental will result in a sale. This change does us a couple of favors: it increases our training dataset from the fairly sparse sale events to all rentals, and it allows us to easily add granular features about the style’s attributes and past performance. With a regression model, each feature added shrinks the taxonomic group we are predicting the sales volume for. In contrast, adding a feature to a classification algorithm is simply an additional column in our training set.


Once we have a classification model we are happy with, we can average predictions across a range of “likely rentals” sampled from user and style pairs with high affinity scores from our personalization algorithm. We then vary the discount level for each “likely rental” to estimate the expected purchase probability for each style at each price (Figure 3). This provides us a granular view of price elasticity at the individual style level which we can validate in production by using metrics like[PR AUC](https://machinelearningmastery.com/tour-of-evaluation-metrics-for-imbalanced-classification/) .


Figure 3: The curves above are the estimated conversion rates for each style I have currently rented.


### Remaining Rental Profit


We attribute rental profit based on the number of days a customer has an item at home during their subscription minus shipping and labor costs. Popular items, logically, have a higher rental profit than potential sales profit, based solely on the number of times they can hypothetically be rented.


Our future rental profit model (created by Hugo Ducruc) uses end-of-life data from retired styles and their associated performances and basic attributes to train a model to predict the total number of days a new style will spend with customers. Because demand varies in a fairly predictable way throughout the life of a style (Figure 4), we can use this end-of-life prediction plus how many days the style has been rented to date to estimate how many rental days remain (Figure 5). These remaining days along with estimated costs can then be allocated to the remaining number of units for the style to get an estimate of the remaining profit per unit. The prediction improves over time as we update the model input with the style’s performance to date.


Figure 4: The blue line represents a typical utilization curve for a season-less style, and the red line represents a typical utilization curve for a seasonal style.


Figure 5: At the time of forecast, we compare the predicted remaining rental demand (the area under the full curve) with the rental days to date (in dark red) to estimate the remaining rental demand in light red.


Applying this to my rentals shows the patterned dress has the highest predicted end-of-life days at home (Table 1). But the yellow dress is not that much farther behind, and it’s much newer. Comparing the predicted end of life to the current state shows the patterned dress has already realized much of it’s rental profit, whereas the profit from the yellow dress is still forthcoming.


The blue coat and red dress are roughly the same age as the patterned dress, but we do not expect to see as much rental profit from them. The red dress appears to be a little behind compared to its expected days at home at this age. This could be due to the rather seasonal nature of a red velvet dress (hello, holiday Zoom party season).


Table 1: Relative differences in age, current days at home and predicted days at home, using the patterned dress as our baseline


### Marrying our models


With these two models, we can compose an objective function that returns the price that will maximize our expected rental profit plus sale profit. But as any econ 101 student can tell you, a bird in the hand is worth two in the bush. We don’t know exactly when this future rental profit will hit our bank accounts or what future events might disrupt this revenue stream. We account for this by taking a lexicographic multi-objective optimization approach (thanks to Vivienne Chan).


This approach entails:


Step 1: Maximizing profit objective


Step 2: Maximizing conversion objective (purchase probability), subject to a profit constraint threshold


The profit threshold is set by us, and is the lever we use to determine how much profit we are willing to forgo to drive a sale today. We use this lever to bring profit forward when we need to.


We use heat maps of discount level by inventory age to gut-check our prices (Figure 6). Inventory age serves as a proxy for the remaining rental profit. At the lowest profit threshold, 0.1, discounts accumulate at the highest possible discount level except for our brand new inventory which is set at a small discount. As the profit threshold increases, the diagonal yellow line moves up as discounts decrease as a function of inventory age. This intuitively matches our intention, which is to sell the inventory that has already realized it’s rental profit and hold onto inventory our customers are still renting.


Figure 6: Heat maps of discount level by inventory age.


### Testing: Pedal to the Metal


We are lucky to have extremely talented planners at Rent the Runway and when it came to testing our new pricing model, we had a robust baseline model of planner intuition (key planning pricing partner, Emily Tabas) to test against. Typically, we run A/B tests when rolling out new algorithms at Rent the Runway. That entails randomly bucketing customers into treatment and control, serving different experiences, and comparing the results. However, when it comes to pricing there is a laundry list of[ethical and legal concerns](https://hbr.org/2017/10/how-retailers-use-personalized-prices-to-test-what-youre-willing-to-pay) that make that approach a non-starter. Instead, we bucketed on the style level stratified on[Original Retail Price](https://help.renttherunway.com/en_us/how-does-rtr-price-items-on-site--rkQ926BdD) and conducted a series of simulated A/A tests across our historical dataset to estimate our false positive rate before proceeding with style level A/B tests.


Happily, the results of our series of tests repeatedly demonstrated that our new methodology improved the overall RTR customer experience. Purchase rates are higher which means more customers are finding items at the right price. The age of inventory sold is higher which means we are preserving our new inventory that our customers want to rent. Both of these metrics mean we are providing a better customer experience to our subscribers, which we hope to be continuously improving.


### Going Forward


Today, we are working to make our pricing even better. We’re thinking about things like:


-


The impact of selling styles on customer experience. We want to provide a best-in-class virtual closet, and we don’t want to sell off so many styles that it degrades our subscriber experience.


-


How to incorporate style substitutability. Some styles are very unique and have few obvious substitutes while others are fairly interchangeable. This means we might be able to sell more of some styles with limited impact on customer experience.


-


How to build curves for applying rental profit non-linearly across remaining style depth. We know that the last dozen or so units of a style are more important than the first dozen, but understanding exactly how many units we need to meet customer demand is a tough challenge.


These problems will require ingenuity and creativity (and lots of math!). If you are excited by these challenges, check out our[careers page](https://boards.greenhouse.io/renttherunway/jobs/2540791) ,


and join our data team!
