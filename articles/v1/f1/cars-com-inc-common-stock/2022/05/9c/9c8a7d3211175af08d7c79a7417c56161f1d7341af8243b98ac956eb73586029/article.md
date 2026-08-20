---
schema_version: "1.0.0"
document_id: "9c8a7d3211175af08d7c79a7417c56161f1d7341af8243b98ac956eb73586029"
company_key: "cars-com-inc-common-stock"
company: "Cars.com Inc."
source_id: "cars-com-inc-common-stock-rss-49a4db916ec1"
canonical_url: "https://tech.cars.com/cars-deal-badges-4dac3ad15bf5"
published_at: "2022-05-18T15:30:52+00:00"
first_seen_at: "2026-07-20T23:17:02.597310+00:00"
fetched_at: "2026-07-28T22:26:32.519238+00:00"
content_hash: "sha256:328e1f7ec2a636263d82f3c944945df5f927546b153c4ddef4fe463de316de4a"
---

# Deal Badges

# **De** al Badges: Simplifying the Used Vehicle Buying and Selling Experience


[Scott Bishop](https://medium.com/@scottandrewbishop?source=post_page---byline--4dac3ad15bf5---------------------------------------)


7 min read


·


May 18, 2022


--


Press enter or click to view image in full size


Photo by[Carles Rabada](https://unsplash.com/@carlesrgm) on[Unsplash](https://unsplash.com/photos/wGMm96WAq4E)


## **Introduction**


With the size of the used vehicle market it is difficult for dealerships to stand out among competition, and with the plethora of options available it is difficult for shoppers to identify the best used vehicle suiting their lifestyle, needs, and budget. Cars.com provides a platform for dealerships to advertise used vehicle listings for shoppers to view and consider for purchase.


Cars.com also provides deal badges, which serve as a tool for dealerships to gauge how competitive their listings are and for shoppers to gauge the quality of a listing’s deal. Deal badges on Cars.com make the used vehicle buying and selling experience convenient and informative, which removes some of the difficulty for dealerships to stand out and for shoppers to find the best vehicle.


## **Deal Badges**


A deal badge designates how well a used vehicle is priced relative to other closest comparable vehicles. A badge is assigned based on how far a dealership’s list price is from the estimated market value for a vehicle.


A deal badge will not be assigned to vehicles that are missing key details in their listing information or have issues or damages mentioned in their history. Also, a deal badge will not be assigned if Cars.com does not have enough information about a vehicle to confidently estimate an expected market value.


The bar plot below represents the average percent of deal badges assigned to a random sample of used vehicles on Cars.com between April 1–15, 2022.


Press enter or click to view image in full size


## **Modeling Vehicles’ Expected List Prices**


The deal badging tool leverages a boosted regression tree model, called[XGBoost](https://arxiv.org/abs/1603.02754) , trained on one year of used vehicle data. Data related to vehicle characteristics like body style (SUV, Sedan, etc.), brand (Honda, Toyota, etc.), and model (Civic, Camry, etc.), vehicle listing location, dealership characteristics, and seasonality are used by the model to establish used vehicle market value approximations. Data related to vehicle supply-demand is used by the model to refine these market value approximations to better capture the day-to-day price fluctuations within the used vehicle market.


It is important to note that we do not directly use a vehicle’s list price as input into our model to determine a vehicle’s market value. Our objective is not to predict the eventual sale or billing price; that can vary based on extra services that a shopper may be interested in. We instead predict the eventual list price agreed upon between the buyer and seller.


## **Capturing the Pandemic’s Price Volatility**


A challenge for modeling used vehicle list prices had been capturing the current price volatility of the used vehicle market in response to the COVID-19 pandemic, which has led to major disruptions in vehicle manufacturing. This[article](https://www.cbsnews.com/news/inflation-new-used-car-prices-2022/) from CBS News and this[article](https://fortune.com/2022/02/10/used-car-prices-soar-45-percent-in-a-year/) from Fortune are sources discussing some details regarding these disruptions. To capture the patterns of price volatility, post-processing logic is applied to the model’s predictions.


The line plot below represents the median price per day in 2021 for popular used vehicle brands on Cars.com. From the beginning of January to the end of August the median price steadily inflates, which correlates with the aforementioned vehicle manufacturing disruptions.


Press enter or click to view image in full size


## **Establishing Expected List Price Range**


Using the post-processed predictions from the model, a lower and upper bound threshold is calculated for each vehicle; these thresholds establish the expected market value range.


The model’s predicted market value would be the expected average market value (black point in the image below), and the lower and upper bound of the range represents the deviance from this average market value that is expected for other closest comparable used vehicles.


## Get Scott Bishop’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


A vehicle below the lower bound of the expected market value (left of purple bar) would be competitive for dealerships and a GREAT deal for shoppers; a vehicle above the upper bound of the expected market value (right of purple bar) would not be as competitive and a FAIR deal for shoppers.


## **Accuracy of the Model**


The median absolute percent error (MdAPE) was used to evaluate the model generating the expected market value predictions. The MdAPE represents the absolute percent difference between the model’s predicted market value and the market value listed by dealerships.


The goal was to achieve an MdAPE at or below 5% for each body style and brand. The data used for evaluation consisted of used vehicles sampled from the first quarter of 2022, and the model’s overall MdAPE for this data is ~4%.


The performance of the model for this sample is also evidence supporting the effectiveness of the post-processing logic used to capture price volatility.


For example, on February 24th[Russia invaded Ukraine](https://www.cnn.com/europe/live-news/ukraine-russia-news-02-24-22-intl/index.html) , and despite how this event could influence the global economy, the model continues to maintain its performance for the days following the 24th.


The table below exhibits the MdAPE by body style; green values reached the goal of achieving an MdAPE at or below 5%, and red values did not achieve the goal. The bar plot below displays what percent each body style represents among vehicles used for evaluation.


Press enter or click to view image in full size


One hypothesis for why vans report an MdAPE higher than the target 5% is related to commonality. The bar plot displays vans as representing less than 1% of all used vehicles on Cars.com. The model had not been exposed to enough samples in training to learn the price patterns for vans.


Another hypothesis could be related to an observed shift in van prices between the dates of data the model had been trained on and the dates of data the model had been evaluated on.


The bar plot below compares the median price of used vehicles by body style between November 2021 (last full month of data used in training) and April 2022 on Cars.com. The median price is similar between the two months for all body styles with the exception of vans, which had an approximate median price of $29,000 in November 2021 and an approximate median price of $36,000 in April 2022; an observed increase of about $7,000.


Press enter or click to view image in full size


## **Model Deployment**


The dependencies powering deal badges on Cars.com run on the Amazon Web Service (AWS) ecosystem, and the environment to run these dependencies is defined through Docker images to ensure all execution is consistent and reproducible by different teams throughout the organization.


Cars.com’s data science team also use their own technology called[Skelebot](https://carsdotcom.github.io/skelebot/) to facilitate communication with AWS and Docker in an efficient and effective manner, as well as abstract away many mundane tasks that would otherwise be conducted manually by Cars.com’s data science team. Skelebot is a powerful tool that is[open](https://github.com/carsdotcom/skelebot) to the data science community.


This approach also ensures that the retraining pipeline can be initiated simplistically and with confidence should the nature of the used vehicle market shift enough to influence the model’s predictive power to fall below a predefined threshold, thus encouraging the current model to learn new pricing patterns related to these market shifts using new data.


The health of the model is reported daily on a designated dashboard, and monitored by various teams throughout the Cars.com organization. Should a notable shift in the used vehicle market occur, the dashboard will alert anomalies and the protocols to respond can be enacted without delay.


In addition, the model also delivers explainability for vehicle features influencing the expected list price using LinkedIn’s new[FastSHAP](https://arxiv.org/abs/2107.07436) algorithm. This allows Cars.com to further break down the expected list prices for each vehicle reported by the model to understand what matters most when it comes to vehicle pricing, and to investigate how shifts in the used vehicle market are affecting the prices assigned to used vehicles.


## **Conclusion**


Cars.com recognizes that it is not easy for dealerships to stand out among competition, or easy for shoppers to decide on their next vehicle given the vast size of the used vehicle market. Cars.com strives to simplify the used vehicle buying and selling experience, and deal badges are a tool for such a task.


Deal badges indicate to dealerships the fairness of their vehicle prices relative to other closest comparable vehicles, and indicate to shoppers the quality of a deal for these vehicles. Deal badges relinquish some difficulty in the used vehicle buying and selling experience, and potentially enables dealerships and shoppers alike to focus on other important decision-making efforts.


Cars.com’s data science team will continue to build on the deal badge tool by looking for ways to improve the model performance to better serve its users. Some possibilities considered include using more fine-grained information from dealer reviews, embeddings for vehicles images, and a plethora of data now available as a result of Cars.com’s recent[acquisition](https://www.dealerinspire.com/videos/accutrade-joins-cars-family/) of[Accu-Trade](https://www.accu-trade.com/) .


Lastly, Cars.com is actively searching for new talent to help grow the data science team. If you are interested in working on similar projects at Cars.com head over to our[careers](https://www.cars.com/careers/#job-listings) page and browse the current opportunities!
