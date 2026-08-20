---
schema_version: "1.0.0"
document_id: "58e7c4fb3cf1b1a1d691582945b551f926b351fe8444728c9954f4aac0999066"
company_key: "ww-international-inc-common-stock"
company: "WW International Inc."
source_id: "ww-international-inc-common-stock-rss-f4e3db081dfb"
canonical_url: "https://medium.com/ww-tech-blog/predicting-food-composition-using-machine-learning-dff0641973b3"
published_at: "2022-11-02T16:02:43+00:00"
first_seen_at: "2026-07-20T23:20:13.695109+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:bd6224248720c245bb30ec29aac4a0082aca980b9aa7e6378cdfcba895b3cd6c"
---

# Predicting food composition using machine learning

# Predicting food composition using machine learning


[Betsey Corey](https://medium.com/@betsey.corey?source=post_page---byline--dff0641973b3---------------------------------------)


4 min read


·


Nov 2, 2022


--


Last January the Data Science team partnered with Columbia University to host an information session that included pitching a capstone project to a class of Business Analytics Masters students. In return, a team of five students selected the WeightWatchers® project to research and ultimately present their findings back to our organization at the end of their spring semester. The project entailed weekly meetings between the students and the Data Science project lead and mentor to ensure guidance around project parameters and to create a sense of community and connectivity throughout. At the conclusion of the project, the Data Science team extended a summer internship to one of the students, Shuning Yang, majoring in Business Analytics. In this post, Shuning highlights her experience interning at WeightWatchers and shares the details of her internship project work that focused on the food composition prediction using machine learning. Shuning was an extremely valuable contribution to the data science team during her internship this summer, and we are looking forward to welcoming her back to WeightWatchers as a full time data scientist after graduating this December!


Betsey Corey, Director, Strategic Talent Programs & Partnerships


Personally, I believe that one of the best parts of the WeightWatchers program is the concept of ZeroPoint® foods. These are foods that members can eat without tracking them or deducting any Points® from their daily Points Budget, giving them even more flexibility in what they choose to eat in a day. Earlier this year, WeightWatchers enabled flexible ZeroPoint categories, which adds logic in our food database for determining which foods should be valued at 0 Points.


This posed a complex problem. Each food has to be reviewed by the nutrition team to calculate its composition and be assigned to the right ZeroPoint foods category. Then a discount value is applied so that tracking these foods won’t deduct Points from a member’s daily Budget if the foods are in the member’s ZeroPoint foods list. For example, if “nonfat yogurt” is a ZeroPoint food for the member, then “vanilla nonfat yogurt” should also be considered as a ZeroPoint food and take no Point off for that member. In addition, this deduction should be applied even if the ZeroPoint food is only *partially* present in the food. For example, if avocado is a ZeroPoint food for the member, then the corresponding Points for avocado should be deducted from the total points of Chicken Avocado Salad.


Currently, we have millions of foods in our database. Manually evaluating each food and then assigning it to a category can be a huge undertaking. However, we hypothesized that this is the exact type of complex problem that is actually well suited for machine learning. We set out to discover if our predictions may be true by having Shuning, our data scientist intern, take this on as her summer research project. Our objective was to evaluate and confirm if we can solve this issue using machine learning tools or not. Shuning was able to solve this by using a combination of features and advanced techniques of stacking multiple machine learning models to create a proof of concept that worked really well. So what were the data, models, and results from Shuning’s research? Let’s break it down further:


**Data**


Data used in this project came from multiple sources. WeightWatchers has a presence globally, but to keep things manageable, this project focused on the U.S. market, where we collected a dataset of more than 50,000 foods. Variables used are food type, category, nutrition information, and ingredient statements.


**Model**


The four machine learning models used were decision tree, random forest, XGBoost, and LGBM. Prediction outputs from the four models, harmonic mean of the prediction outputs from all four models (harmean 4), and harmonic mean of the prediction outputs from random forest, XGBoost, and LGBM (harmean 3) are evaluated.


The metrics used for evaluation were R-squared, RMSE, and accuracy based on different residual thresholds (0.05, 0.1, 0.15, and 0.2). R-squared finds how much of the spread of the data can be explained by the features. The closer to one, the better it is. RMSE gives the difference between predicted value and actual value on average. The closer to zero, the better it is. The prediction output is considered accurate if the difference between actual value and predicted value is within a threshold level that allows for small errors in the estimation.


**Results**


Metrics are compared among different models and prediction outputs. Overall, harmean 3 gives the best result. In addition, two sets of variables are run — one without the ingredient statement and one with the ingredient statement. This is done to check if adding ingredient statements will improve the performance as this feature increases model complexity and places additional demands on data quality. It turned out that ingredient statements are very helpful. There are improvements on all metrics. See the below table for the corn category as an example.


Press enter or click to view image in full size


To test model performance on new data, 55 foods were selected for testing. In general, the prediction outputs are good from an outsider perspective, even though it can sometimes underestimate the percentage of ZeroPoint foods within a dish. But since there are many rules behind food adjustment, further evaluation by the nutrition team is needed.


Press enter or click to view image in full size


**Next steps**


This **** research project has proven that the problem is solvable. The next step is to make modifications and put them into production.


Shuning Yang — Data Science Intern


**Interested in joining the WW team?** Check out our[careers page](https://www.weightwatchers.com/us/corporate-careers) to view technology job listings as well as open positions on other teams.
