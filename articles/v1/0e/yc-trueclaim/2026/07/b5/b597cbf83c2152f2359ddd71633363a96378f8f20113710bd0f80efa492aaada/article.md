---
schema_version: "1.0.0"
document_id: "b597cbf83c2152f2359ddd71633363a96378f8f20113710bd0f80efa492aaada"
company_key: "yc-trueclaim"
company: "TrueClaim"
source_id: "yc-trueclaim-news-import-4b72f46bdbde"
canonical_url: "https://www.trytrueclaim.com/post/hurry-up-and-wait-new-5500-dataset-just-dropped"
published_at: null
first_seen_at: "2026-07-26T03:06:33.956797+00:00"
fetched_at: "2026-07-28T21:16:48.751829+00:00"
content_hash: "sha256:b12bce496d4825ccea596fca17b411a0a6482c12266f8139fe28cb087d2e9ec1"
---

# Hurry up and wait: New 5500 dataset just dropped

The Department of Labor recently released[the 2023 aggregated 5500 form submissions](https://www.dol.gov/agencies/ebsa/about-ebsa/our-activities/public-disclosure/foia/form-5500-datasets) on their website! Now we just have to wait… 2 years until Congress gets a report on this data and the trend of self-insured plans in the U.S.


What should you do if you don’t want to wait? Learn more about the dysfunctional 5500 form and how you can recreate the analysis two years ahead of schedule.


‍


## What is a Form 5500?


For the lucky folks who have no idea what a form 5500 is, this is an annual report filed with information on companies’ employee benefit plans. The Department of Labor aggregates Form 5500 filings[on their website](https://www.dol.gov/agencies/ebsa/about-ebsa/our-activities/public-disclosure/foia/form-5500-datasets) . The filing is a requirement under the[Employee Retirement Income Security Act (ERISA)](https://www.dol.gov/general/topic/health-plans/erisa) . While the filings do contain information on retirement plans and benefits, they also contain information about health plans utilized by the companies.


‍


Individuals who sell benefits or services in the retirement, health, pension, and finance sectors can gain remarkable insights utilizing data from the 5500 filings, however, there are several key limitations in the form that make it difficult to analyze the data. Before ERISA, there were limited regulations on how employee benefit plans were managed. This led to mismanagement and even abuse of these plans, jeopardizing the retirement security of workers. While the form was originally developed to protect retirement funds, it evolved to also protect health benefit plans, which had increased in both complexity and risk. The Department of Labor decided they could use this same form to make sure that employers were responsibly handling health plan administration.


‍


[The Patient Protection and Affordable Care Act (Affordable Care Act)](https://www.hhs.gov/healthcare/about-the-aca/index.html) required the Secretary of Labor to provide Congress with an annual report ([2023 example here](https://www.dol.gov/sites/dolgov/files/EBSA/researchers/statistics/retirement-bulletins/annual-report-on-self-insured-group-health-plans-2023.pdf) ) on self-insured employee health benefit plans and financial information regarding employers that sponsor such plans. This report is generated using the data from the 5500 filings, which many (but not all) self-insured health plans are required to file annually with the Department of Labor.[The first report](https://www.dol.gov/sites/dolgov/files/ebsa/researchers/statistics/retirement-bulletins/annual-report-on-self-insured-group-health-plans-2011.pdf) of this kind was provided to Congress in March 2011.


‍


So, as you might guess, we’re stuck with an analytics problem using a separate report not designed for the purpose of the report we are trying to create in order to fill in the details on the health of employers’ healthcare coffers. **A common misconception is that every company that submits a Form 5500 is self-insured, but as you can tell from the annual reports to Congress, fully-insured and mixed-funded employers also have to submit the report in many cases.** Unfortunately, all three of these categories do not contain complete data for many reasons,[some of which are documented in the appendix](https://www.dol.gov/sites/dolgov/files/EBSA/researchers/statistics/retirement-bulletins/annual-report-on-self-insured-group-health-plans-2023-appendix-b.pdf) compiled by the data analytics contractors that review the filings on a 3-year retrospective basis before compiling the report for Congress.


‍


## How does the government know if a company is self-funded?


The most interesting thing about the utilization of 5500 form data to compile the report for Congress on self-insured employers is that **nowhere on the form is the company required to disclose whether they are providing self-funded healthcare benefits.** The analysts that compile the report use a very complex algorithm (shown below in flow-chart format) to decide whether an employer is self-funded, mixed-funded or fully insured.


‍


2023 algorithm utilized to compile funding status from Form 5500 filings (From 2023 Appendix B - Analyzing 2020 Form 5500 filings)


‍


The flowchart requires the user to have access to the core 5500 filing (full or short-form version), and any filed Schedule A’s, H’s, and I’s. The algorithm has been significantly advanced since the first report to Congress when they showed the actual algorithm used to determine funding status:


‍


Initial attempt to determine plan funding status from Form 5500 status for report to congress. Used in 2012 and included in Deloitte's report outlining the limitations of this dataset on identifying plan funding status.


‍


Nowadays the algorithm’s code is not included in the report, but can be reconstructed using the details provided by the analysis company. The only piece of the algorithm not fully explained in the report is how the analysts determine if a company is “level-funded” ( **S4** ). From the report:


> “Consider plans that provided evidence of health insurance and of a plan trust that listed payments both directly to participants and to insurance carriers. Depending on the magnitude of certain trust payments and insurance premiums, such plans may be self-insured, mixed-funded, or fully insured. The algorithm sequentially checked for various scenarios, including the possibility that the Schedule A reflected a level-funded plan contract.”


‍


This is helpful information, but leaves open quite a significant question: What magnitude of payments is the analyst using to determine level-funded status? A stop-loss protection indication is a good starting point, but this might not be marked by the employer since the level-funded plan insurer often pays this using the “level premium.”


‍


## How private data sources calculate funding mechanism


Several companies ([BenefitFlow](https://benefitflow.com/) ,[Zywave](https://www.zywave.com/) , etc.) offer 5500 data that is cleaned and usually loaded into a web UI for prospecting and analysis, including a pre-processed funding mechanism determination. In all the offerings I have seen, these private data sources only distinguish between self-funded and fully insured, abstracting out mixed-funded companies in different ways. Also, we’ve found in our experimentation that sometimes the funding mechanism classification in these tools is incorrect (most often in the direction of listing large, fully-insured companies as self-insured).


‍


Interestingly, none of these companies use a method similar to the analysis above to determine funding mechanism. From what I’ve learned, most of these companies combine heuristics from basic 5500 filings (ex: did the submitter check “Stop Loss” coverage?) with Schedule C data in order to determine funding status. Schedule C is designed to report service providers who earn more than $5,000 annually for the benefit plans. It is required for large plans (over 100 members), and sometimes can list TPA names as a service provider. In my experience, many times employers omit service providers from these filings. In fact, 27.11% of companies with large plans that filed a full Form 5500 in 2022 did not include a Schedule C, which seems….improbable at best?


‍


## Where do we go from here?


So we have an inaccurate data form being used by both the private and public sectors in different ways to calculate something that should be as simple as a form with 3 options on the sheet, but instead we continue to try to squeeze insights out of what we have instead of fixing the root problem: the data source sucks.


‍


The CAA is requiring self-insured employers (along with insurance companies) to report many additional data points about their spending annually to assess both health trends and plan expenditures. It seems like instead of just continuing to pile on the forms on busy benefits leaders, we could use this as an opportunity to **simplify** the process while actually retaining **even more accurate data.** Until then, if anyone wants to release an open-source version of the annual report on self-insured employers that runs as soon as data is posted, I think many consultants, brokers, and benefits leaders would be keen to have trend insights in real-time.


‍


‍
**Thanks** to[Brendan Keeler](https://www.linkedin.com/in/brendan-keeler/) for suggestions and for reading draft versions of this post.


‍
