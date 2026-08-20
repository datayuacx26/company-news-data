---
schema_version: "1.0.0"
document_id: "bc0beb36002cecdae66bb454289cc48176e0e27f7747fc67baaa1b552b0fcb9a"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/frequentist-protective-experiments"
published_at: "2019-12-17T20:19:26+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T22:26:49.714113+00:00"
content_hash: "sha256:865ef780c94682652a6524510d635019b4f451cc50a8b99ff52b25becda95d14"
---

# Running Protective Experiments Using a Frequentist Framework

Rent the Runway has amazing Product and Engineering teams that dream up, design, build, and launch features to improve our user experience and revolutionize the fashion industry. In alignment with that, most A/B experiments are designed to test whether we can observe statistically significant evidence that a new feature improves acquisition, retention, engagement, and other key metrics. However, sometimes we have an alternative reason to make a change to the user experience that is not intended to directly improve any key metric. In such scenarios, we would consider it a win if we could launch the feature while maintaining the status quo. Some examples include:


-


Paying off technical debt by removing or replacing old web services


-


Refreshing the look and feel of a site or email campaign so it doesn’t become visually stale


-


Improving site security


-


Launching a feature that collects data with some strategic value


In such cases, the win criteria must be proof of equivalence (or better), not improvement. A naive — and incorrect — approach to the problem would be to simply run a normal A/B test with a learning plan to call to treatment barring any evidence of a negative outcome; the so-called “flat result.” This is incorrect is because the old saying, “the absence of evidence is not evidence of absence” holds very much true in A/B experimentation. Said another way, *the lack of a statistically significant negative result is not proof of equivalence* .


Described more rigorously, a typical experiment setup has 95% statistical confidence (the expected rate of true positives) and 80% statistical power (the expected rate of true negatives), so it is *not* equally likely that you will encounter a false positive as it is that you will encounter a false negative. Calling an experiment “on flat” overlooks this important detail, and increases your chances of an incorrect outcome by 4x even if you hold true to all other testing assumptions. The flip side of statistical confidence and power is that the rate of Type I errors (false positives) is 5%, while the rate of Type II errors (false negatives) is 20%. You will, over the long run, fail to reject the null hypothesis of equivalence 20% of the time when there was in fact a real difference. Experiments are set up this way because hypothesis testing has been designed by convention to be conservative and prevent us from claiming that effects exist when they in fact do not. These assumptions lead us to make false conclusions when they are not understood.


A final nail in the coffin of a “flat results” learning plan is: An experiment that is considered a success even on the lack of statistical evidence means that your chances of winning are inversely correlated to the sample sizes. The smaller your sample size, the wider your test statistic probability distribution (continue reading for more on that). Since more numbers fall in the range of a flat result with less population, a sure-bet way to win your experiment would be to run it against only a very small number of users. Just call it after the first 100 site visits and check the *p* -value; it will almost certainly not be statistically significant, or flat. This knowledge alone should be a huge red flag about the validity of the experiment results with such a setup.


It’s tempting to read over the last two paragraphs and decide to launch the feature without experimentation. This option risks worsening the user experience by an undetectable amount, and should not be chosen simply because the standard setup doesn’t work. The best solution is to run a protective experiment, also referred to as an equivalency, non-superiority, or do-no-harm experiment.


I should note here that I repeatedly use “equivalence” in this article in a technically wrong way, since true equivalence testing must prove that treatment is *neither better nor worse* than control, whereas the experiments being discussed here simply prove *not worse* . A common technique for a true equivalence test is TOST, which stands for two-one-sided-tests. Since a true equivalence test is usually not necessary in eCommerce, the two are frequently used interchangeably. Incorrectly so. Fair enough for any critics of my loose terminology.


There are lots\[[1](https://multithreaded.stitchfix.com/blog/2015/05/26/significant-sample/) ,[2](https://hookedondata.org/guidelines-for-ab-testing/) ,[3](https://medium.com/airbnb-engineering/experiments-at-airbnb-e2db3abf39e7) \] of excellent articles and blog posts out there about how to run great A/B experiments - and equally excellent articles on what *not* to do when running A/B experiments\[[4](https://www.evanmiller.org/how-not-to-run-an-ab-test.html) ,[5](https://towardsdatascience.com/chasing-statistical-ghosts-in-experimentation-3f393323a1c1) \]. I have only encountered one other\[[6](https://multithreaded.stitchfix.com/blog/2019/05/06/noninferiority/) \] eCommerce-focused article about protective experiments to date. In addition, most out-of-the-box experimentation software does not support protective experimentation at all. Fear not, for the concepts are all the same as in a typical experiment. Here we will explain how we set up and run them at Rent the Runway using a real example.


## **The scenario**


[In 2019, Rent the Runway opened its second warehouse in Arlington, TX.](https://www.dallasnews.com/business/retail/2019/06/05/rent-runway-opens-arlington-fulfillment-center-500-plus-employees-year-end) Our goal is to get items to customers more quickly and to have inventory spend less total time in transit. In order to do this, we want to optimize which warehouses send and receive inventory from users based on their zip codes. Zip codes are easy enough to collect at checkout when the customer submits their shipping details. However, having correct zip codes while users are browsing before checkout gives us the ability to filter out inventory that is unavailable at the warehouse that services their location. This allows us to prevent the negative user experience of adding an item to the bag that turns out to not be available. The most straightforward way to do this is to simply ask the user to confirm their zip code early in the acquisition funnel. This poses a risk because modals are intrusive to the browsing experience, especially before checkout. Would this modal be so annoying to pre-checkout users that they would exit the page instead of confirming their zip code? We can validate this with an A/B experiment. Since the goal is to maintain status quo even though we are introducing the data collection modal, this is a great candidate for a protective experiment!


Below is an image of the proposed treatment with a zip code confirmation modal:


## **Experiment setup**


The basic experiment design is the same regardless of whether it is a typical or a protective A/B experiment. At Rent the Runway we follow these steps:


1.


Establish some user behavior we care about and find a way to measure it


2.


Envision a product idea around a hypothesis of how to influence said behavior


3.


Set a winning threshold; in this case, we will set a threshold of equivalence


4.


Create a learning plan


5.


Calculate the expected experiment runtime with a statistical power analysis


6.


Document everything


7.


Run the experiment and wait for results


For the purpose of this protective experiment we will focus on conversions of prospective subscription customers. The behavior that we are potentially impacting is reducing the conversion rate by adding one extra step in the acquisition funnel. The metric we will use is, therefore, conversion rate of users who get to the page when the zip code collection modal first appears.


Instead of the usual winning threshold, we must set a threshold of equivalence — that is, some conversion rate that we would be happy considering “pretty much the same” as the control group conversion rate. What this threshold is depends on your business’ appetite for risk and the importance of the feature. The longer you can leave the protective experiment running for, the smaller your test statistic’s standard error is and therefore the more likely your entire confidence interval (CI) is to be above any given negative threshold. This practically translates into accepting less risk. The more risk you’re willing to accept, the shorter your experiment needs to run for.


Our hope is that the point estimate, represented by *treatment - control* (p2 - p1) is exactly 0. Even then, we must pick a negative value as the equivalence threshold. That is because although we observed no difference between the treatment and control samples, our inference is on the general population; the probability distribution of the test statistic will necessarily have some spread, even if we leave the experiment running for a very long time. The plots below show this relationship visually.


Of note, although most of the math is based on p2 - p1, which is the *absolute difference* , in the world of eCommerce we actually represent and talk about results more commonly as (p2 - p1) / p1, which is the *relative difference* . This is also referred to as the “lift of treatment over control.” When the absolute difference equals zero, the relative difference also equals zero.


Below is a probability distribution plot of the test statistic after different experiment durations, assuming a difference of zero:


As we can see, the spread of the 95% CI becomes smaller and smaller as we collect more data. The win criteria is to have the entire confidence interval above the threshold of equivalence (whatever we choose it to be), so if we run the experiment for only one day we would only be able to guarantee protection against a possible negative conversion rate of -4.2% or worse. A less risky scenario is to let the experiment run for 30 days and protect against a negative conversion rate of -0.8% or worse. It’s always a good idea to give yourself a small amount of buffer too, in case your test statistic lands on, say, -0.2%. This will shift your entire distribution down by the same amount.


Remember, if your equivalence threshold is anything above 0, you are back in the world of running a normal A/B experiment. In fact, the layman’s definition of “statistical significance” the way it is typically used in experiments is simply that the chosen confidence interval of the test statistic’s probability distribution is entirely above 0. For this to be true, your observed data must show evidence of an *improvement* on your key metric.


After crunching some numbers with the Finance team, we decided to make sure the entire 95% CI is above -2.5%. This means we are comfortable moving forward so long as there is evidence to suggest that the likely population effect of collecting zip code before checkout is *no worse than* a relative 2.5% hit on conversion.


Given this information, how long do we need to run the experiment for? We can use a power plot to visually inspect the trade-off between experiment runtime and detectable effect. In this case we will use a baseline conversion rate of 15% and expected 50,000 daily visitors who would be eligible to become subscribers. Please note that we never disclose our actual traffic and conversion rate metrics, so numbers in this article are illustrative. Since we only care about the left side of the 95% CI, because we are only concerned with proving that it is above a certain threshold, we can also do the power analysis assuming a one-sided proportion test in order to give us a little bit more statistical power. In this case, if the equivalence threshold is not met then we don’t really care about the right side of the distribution.


I mentioned earlier that I always like to give myself a little buffer to play with, so in this case we would recommend running the experiment for seven days based on the power plot, giving us a minimum detectable effect of relative 2%. The learning plan therefore is:


We will run the experiment until we have 350,000 exposures between both experiment groups (expected experiment runtime is one week) and call this experiment to treatment if we are 95% confident that the relative difference between treatment conversion and control conversion is -2.5% or better, based on a one-sided proportion test.


H0: (p2 - p1) / p1 <


-.025


HA: (p2 - p1) / p1 > -.025


For the sake of context, a 2.5% relative decrease to the baseline conversion rate of 15% would mean the new conversion rate is approximately 14.6%. Which means we will consider this a win so long as the treatment conversion rate is equal to or better than approximately that value, assuming the control conversion rate stays stable. Why couldn’t we just launch the treatment to everyone and monitor that rate? Because we’re always on either a seasonal upswing or downswing and so we would not expect our actual control conversion rate to remain at exactly that value. Without a controlled experiment such as this, we would not be able to establish a causal relationship. Keep reading to see what happens.


We document this proposal, create an experiment ID using[Rent the Runway’s open source experimentation framework](http://dresscode.renttherunway.com/blog/8/26-1) , get concurrence from our key stakeholders, and march onwards!


## **One week later: Reading out a protective experiment**


We have collected data and are ready for an experiment read-out. We query our experiment results database and see the following:


We had a few more visitors than we expected, and the conversion rate was higher than anticipated - probably due to seasonality. The treatment group has a slightly smaller conversion rate than control (uh oh) but it’s not clear what that means yet.


We can calculate the point estimate of to be is a relative -0.97%. Thankfully, we gave ourselves that little buffer during the power analysis! Inspecting the plot below, we see that the 95% CI is entirely above -2.5%, meaning that this experiment is a statistical success and we can say with a high degree of confidence that we can maintain equivalence even with this zip code confirmation modal.


## **Zipping up the garment bag**


To sum it up, protective experiments are a statistically robust way to validate equivalence when there is no expectation that a product change improves any of our key metrics. It validates our ability to maintain the status quo.


This feature launch was an excellent candidate for a protective experiment. If we had run a traditional A/B experiment, we would have received a “flat” result which is often misinterpreted as being evidence of no effect, but which in reality means that you can’t use your learning to make an informed and data-driven decision. This protective experiment gives us the data-driven confidence to move forward with collecting zip codes before check out, allowing us to grow our warehouse infrastructure to improve operational efficiency and get physically closer to our customers so that we can better service them.


Passionate about tech and fashion? Interested in revolutionizing the way the world gets dressed in the morning? Want to run your own experiments (protective or otherwise)?[Rent the Runway is hiring!](https://www.renttherunway.com/about-us/careers?action_type=footer_link)
