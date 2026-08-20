---
schema_version: "1.0.0"
document_id: "a50b43a859f95af40e5757f6e5fa661ae96420ea467d1aacdf6126848bfd323a"
company_key: "yc-siphox-health"
company: "SiPhox Health"
source_id: "yc-siphox-health-news-import-f1c8444f7b36"
canonical_url: "https://siphoxhealth.com/hub/blog/exploring-wearable-data-oura/"
published_at: null
first_seen_at: "2026-07-22T13:37:50.766839+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:7bad1cdf7efd1b2deacee49949d336062f0220809a62f6a9b6f2d9528fcb5ccb"
---

# Exploring Connected Wearable Data, Part 1: Oura

**In this article**


- First, a quick explanation of our data exploration methodology and a few definitions
- The cholesterol-sleep relationship in women over the age of 40
- How does this differ for men?
- So, why are cholesterol and sleep related?
- Supplementary Data


Are you finding it difficult to achieve restful sleep or feeling drained despite using your sleep tracker? Discover how combining sleep data from devices like Oura rings or Apple Watches with blood biomarker data can transform your health and well-being.


Many of us rely on wearable devices to monitor our sleep, hoping to uncover the secrets to better rest and more energy. However, these devices often leave us with more questions than answers. It’s not just about tracking steps or sleep stages; it’s about understanding the deeper connections between different aspects of our health. If you’re looking to improve your diet, exercise, or sleep habits, the following insights can help you determine what changes might enhance your overall well-being.\[1\]


To explore these connections, we analyzed SiPhox user data to identify relationships between blood biomarker data and connected wearable device data. S


urprisingly, we discovered a strong correlation between lipids and sleep data, particularly in women over the age of 40. These findings could provide valuable insights for those looking to make informed changes to their health routines.\[2\]


## First, a quick explanation of our data exploration methodology and a few definitions


### Methodology


Wearable data was averaged over the two weeks preceding the associated blood tests.


For correlation analysis, we used Pearson’s correlation coefficient (a measure of the strength and direction of the linear relationship between two variables) to measure the strength and direction of the linear relationship between the two datasets. We further visualized these relationships by creating plots with an Ordinary Least Squares (OLS) regression line (a straight line that best fits the data points), which helped illustrate the trend and strength of the correlations. This methodology allowed us to effectively identify and visualize meaningful correlations within the data.


\[3\]


\[4\]


### Cholesterol — the Good, the Bad, and the Transport:


Cholesterol can be categorized into HDL-C (high-density lipoprotein cholesterol) and LDL-C (low-density lipoprotein cholesterol), commonly referred to as “good” and “bad” cholesterol.


LDL-C (“bad”) cholesterol is deposited into the arteries, which increases the risk of cardiovascular disease.


\[5\]


HDL-C refers to the cholesterol carried by Apolipoprotein A1 (APOA1) (a type of protein that carries cholesterol in the blood) -containing HDL particles. HDL particles pick up harmful cholesterol in the arteries and then carry everything to the liver (directly or indirectly), where cholesterol is repurposed or removed.


\[6\]


\[7\]


### Rapid Eye Movement


Sleep


:


Rapid eye movement (REM) sleep is a phase that lasts approx. 25% of your total sleep duration, where (alongside other activities) your brain rests and repairs itself so you can[wake up feeling refreshed and energized.](https://www.sleepfoundation.org/stages-of-sleep/rem-sleep)\[8\]


## The cholesterol-sleep relationship in women over the age of 40


There was a strong positive correlation between HDL-C and REM sleep for women over 40 years of age. Interestingly, there was no significant correlation between the same markers in neither men of the same age group ([correlation coefficient = 0.08](https://www.notion.so/Exploring-Connected-Data-Part-I-Oura-b1855dfdf6554ffaa645c73782e92edd?pvs=21) ) nor in younger women ([correlation coefficient = -0.13](https://www.notion.so/Exploring-Connected-Data-Part-I-Oura-b1855dfdf6554ffaa645c73782e92edd?pvs=21) ).


Unsurprisingly, we saw a similarly positive correlation between APOA1 and REM sleep, which is to be expected given the relationship between HDL-C (passenger) and APOA1 (vehicle).


For additional graphs on the relationship between bad cholesterol (LDL-C, APOB) and sleep data in the same age group, please refer to the Supplementary Data section.


## How does this differ for men?


Given the strong correlation in women, one might assume that there’d be a similar correlation in men. But our data says otherwise.


The correlation between HDL-C and REM, for men over 40, was only **0.08,** which is essentially a non-existent correlation and certainly a far weaker one than the 0.53 correlation coefficient in women of the same age group! For all intents and purposes, our data indicates that there is no correlation between cholesterol biomarkers and sleep data in men over 40.


## So, why are cholesterol and sleep related?


It is unclear whether the relationship we’re observing is causative or correlative. Perhaps one of the two, HDL-C or REM sleep, impacts the other. Just as likely, they’re both independently impacted by another set of biomarkers or activities, such as an active lifestyle or hormone status.


That being said, here are a few mechanistic connections between REM and HDL-C:


- HDL-C particles have significant[anti-inflammatory properties](https://my.clevelandclinic.org/health/articles/24395-hdl-cholesterol#:~:text=HDL%20cholesterol%20also%20works%20against%20inflammation%20and%20oxidants) . Chronic inflammation is known to disrupt sleep patterns. So this reduced inflammation could contribute to more stable and prolonged REM sleep.
- HDL-C particles support endothelial function, which is vital for healthy blood circulation. Improved endothelial function could improve circulation to the brain during sleep, and subsequent REM sleep duration.\[9\]


These connections exist in both men and women, though. If either or both of these are the root cause of the correlation, then why don’t we see this in men? Or a younger cohort of women?


If you’re interested in seeing how your cholesterol and sleep are correlated, pick up an at-home blood testing kit from[SiPhox](https://home.siphoxhealth.com/) . You’ll receive results and insights in your SiPhox dashboard within a few days of collecting your sample.


## Supplementary Data


Correlation between HDL-C and REM sleep among males aged 40 and older.


Correlation between LDL-C and REM sleep among females aged 40 and older.


Correlation between hbA1C and Oura sleep score among females aged 47 and older.


Correlation between HDL-C and REM sleep among females aged 20 to 40 years.


Correlation between APOB and REM sleep among females aged 40 and older.


References


\[+ \]


References1 https://aasm.org/one-in-three-americans-have-used-electronic-sleep-trackers-leading-to-changed-behavior-for-many/


2[https://siphoxhealth.com/blogs/news/what-we-measure#:~:text=associated biomarkers below.-,Blood Biomarkers,-Inflammation¹](https://siphoxhealth.com/blogs/news/what-we-measure#:~:text=associated%20biomarkers%20below.-,Blood%20Biomarkers,-Inflammation%C2%B9)


3 https://www.scribbr.com/statistics/pearson-correlation-coefficient/


4 https://medium.com/@VitorCSampaio/understanding-ordinary-least-squares-ols-the-foundation-of-linear-regression-1d79bfc3ca35


5 https://siphoxhealth.com/pages/ldl


6 https://siphoxhealth.com/pages/hdl


7 https://siphoxhealth.com/pages/apoa


8 https://www.webmd.com/sleep-disorders/sleep-101


9[https://my.clevelandclinic.org/health/articles/24395-hdl-cholesterol#:~:text=HDL](https://my.clevelandclinic.org/health/articles/24395-hdl-cholesterol#:~:text=HDL)[cholesterol also works against inflammation and oxidants](https://my.clevelandclinic.org/health/articles/24395-hdl-cholesterol#:~:text=HDL%20cholesterol%20also%20works%20against%20inflammation%20and%20oxidants)
