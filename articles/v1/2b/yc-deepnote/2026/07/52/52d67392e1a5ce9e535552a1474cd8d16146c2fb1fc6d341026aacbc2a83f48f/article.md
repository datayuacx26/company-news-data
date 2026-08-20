---
schema_version: "1.0.0"
document_id: "52d67392e1a5ce9e535552a1474cd8d16146c2fb1fc6d341026aacbc2a83f48f"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-1c599340c4cd"
canonical_url: "https://deepnote.com/blog/ai-data-visualization-tools"
published_at: null
first_seen_at: "2026-07-26T08:11:39.657869+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:5f095b447d32872956117a5ee8235fdb9b873072263277f76c31b1f8d421606b"
---

# Top AI data visualization tools in 2026

[← Back to all posts](https://deepnote.com/blog)


AI data visualization tools aren’t about who draws the nicest chart. They’re about how they handle definitions, calculations, transparency, and sharing when it counts. In this article, we run the same small benchmark across top tools to see how they compute churn, explain changes, show their work, and package results. The goal isn’t to crown a winner. It’s to see how hard each tool makes it to ship a wrong answer.


Last quarter, a colleague asked an AI tool to explain why churn spiked in March. The tool produced a confident chart: churn was up 18% month over month, concentrated in the Enterprise segment, with a narrative pointing to a pricing change as the likely cause. The chart looked right. The explanation sounded plausible. But the conclusion was wrong.


The pricing change happened in April, not March. The tool grouped by invoice date instead of cancellation date and quietly included trial accounts in the Enterprise segment. None of these assumptions were surfaced. The result looked polished but was built on definitions nobody had verified. This illustrates the core challenge with AI data visualization tools: they produce charts quickly, but speed does not guarantee correctness.


In this article, we benchmark leading AI data visualization tools using the same dataset and prompts to see how they handle basic analytical tasks.


Tools reviewed in this benchmark:


- Tableau


- ThoughtSpot


- Qlik Sense


- ChatGPT by OpenAI


- Claude by Anthropic


- Devin DANA


- Databricks AI/BI Genie


- Polymer


- Graphy


- Deepnote


### **The landscape, without the hype**


Before diving into results, here's how AI data viz tools actually break down. We're not ranking these categories; each optimizes for different workflows.


**Enterprise BI platforms with AI bolted on.** Tableau, Power BI, ThoughtSpot, Qlik Sense, and Looker all sit here. These tools started as traditional business intelligence; dashboards, semantic models, governed data access; and have added AI features on top. Tableau is pushing toward agentic analytics. Power BI has Copilot. ThoughtSpot built its identity around natural-language search. Looker leans on its semantic layer plus Gemini integration. The AI in these tools tends to be constrained by the platform's existing data model, which is both their strength (answers stay within governed definitions) and their limitation (you can only ask questions the semantic layer supports).


**Conversational and agent-based analysis.** ChatGPT's advanced data analysis, Claude's artifact workflows, Devin's Data Analyst Agent (DANA), Narrative BI, and Databricks AI/BI Genie fall into this group. These tools let you upload data or connect to a source and ask questions in natural language. They're optimized for speed and flexibility; "here's a CSV, what's interesting?"; and they often produce charts alongside code and narrative explanations. The tradeoff is governance: most of these tools don't enforce metric definitions, don't track data lineage, and don't distinguish between "I'm confident in this answer" and "I'm guessing."


**No-code and low-code dashboard builders.** Polymer, Akkio, Looker Studio (with Gemini features in preview), Domo, and Sisense occupy this space. The pitch is "upload your spreadsheet, get a dashboard." Some emphasize embedded analytics (Sisense), some emphasize AI-generated insights (Akkio), and some are simply free and good enough for basic reporting (Looker Studio). These tools work well when the data is clean, the questions are straightforward, and the audience just needs a visual summary they can filter and share.


**Specialized and emerging tools.** Graphy, Powerdrill Bloom, Obviously AI, and MonkeyLearn are narrower. Graphy focuses on AI chart generation. Powerdrill Bloom packages analysis into slide decks and auto-summaries. Obviously, AI does no-code predictive modeling from CSVs. MonkeyLearn targets text analytics. These tools can be genuinely useful for specific tasks, but they're not general-purpose; you'd use one of these alongside a broader platform, not instead of it.


### A quick benchmark you can run


Instead of building a massive bake-off that only a full-time analyst could reproduce, we used a simple, repeatable test.


Each tool receives the **same dataset and the same prompts** . The goal is to see how quickly the tool produces a correct answer and how easy it is to verify the result.


### Dataset


We use a small SaaS-style churn dataset called **tiny_churn.csv** .


Columns:


• month
• region
• plan
• users
• churned


The dataset intentionally stays simple:


-


no joins


-


no external connectors


-


no transformations required


You can copy the table below into a CSV file and upload it into any tool in under a minute.


```text
month,region,plan,users,churned  2026-01,NA,Pro,120,12  2026-01,EU,Pro,80,8  2026-01,NA,Enterprise,60,3  2026-01,EU,Enterprise,40,2  2026-02,NA,Pro,130,15  2026-02,EU,Pro,85,9  2026-02,NA,Enterprise,62,4  2026-02,EU,Enterprise,45,3  2026-03,NA,Pro,125,18  2026-03,EU,Pro,90,10  2026-03,NA,Enterprise,70,8  2026-03,EU,Enterprise,50,4
```


### Prompts used


Each tool is asked the same three questions.


**Prompt 1: Churn chart**


Compute churn rate (` churned / users` ) and visualize it by month and plan.


**Prompt 2: What changed**


Identify what changed from February to March and determine which plan drove the increase. Provide the numbers.


**Prompt 3: Calculation logic**


Show the calculation steps behind the result (SQL, code, or transformation logic).


**Prompt 4: Export and sharing**


If supported, export the result or generate a stakeholder-ready artifact.


### Evaluation criteria


Each tool is evaluated on five criteria:


Metric What it measures


Speed How quickly can a correct chart be generated


Correctness Whether the calculation and grouping are correct


Transparency Whether the logic or code can be inspected


Iteration Whether prompts can be refined without breaking results


Shareability Ease of exporting or sharing results


That’s the baseline we’ll apply across the 10 tools. Same data. Same prompts.


### **Tableau**


- Category: Enterprise BI (governed)


- OSS: No


- Free tier / trial: **Yes** .[Source](https://www.tableau.com/products/trial) .


- Pricing range: ~approximately **$15–$75 per user/month** depending on role (Viewer, Explorer, Creator):[Source](https://www.tableau.com/pricing) .


- Best for: governed dashboards + shared metrics


**Prompt 1: Churn chart (monthly by plan)**


We've created a line chart to show the monthly churn rate by plan. The churn rate is calculated as` Churned / Users` .


****


**Prompt 2: February to March change (with numbers)**


Tableau returned a structured explanation of how to determine the increase (calculate churn rate per plan in Feb and Mar, compare differences), but it did not compute the exact differences directly in the response. It essentially guided the user to derive the answer from the visualization.


Generated response:


```text
Explanation:
To determine which plan's churn rate increased the most:
Calculate the churn rate for each plan in February and March.
Compare the differences in churn rates between these months for each plan.
The plan with the largest positive difference has the highest increase in churn rate.


Unfortunately, I cannot compute the exact numbers or differences directly. You can use the above viz to visualize and calculate the differences.
```


**Prompt 3: Calculation logic (SQL / code)**


Tableau exposes the churn calculation as a calculated field, and it can be inspected or edited directly.


**Export and share options (if supported)**


Export and sharing are robust but enterprise-oriented. You can export visuals, publish dashboards to Tableau Server/Cloud, or share via links if deployed. However, this assumes Tableau infrastructure is already in place. It’s powerful, but not frictionless in the way lightweight, link-first tools are.


**Experience**


- Setup was mostly smooth, but we had to manually enable AI features in settings after digging through the[docs](https://help.tableau.com/current/online/en-us/setup_tabAI_site_setting.htm) for a bit. Not hard; just not obvious.


- Prompt 1 worked well. Tableau correctly created the churn rate calculation and visualized it without guessing. That’s a good sign.


- Prompt 2 was more conservative. Instead of directly computing the February-to-March difference, it explained *how* to find it using the chart. That feels very “traditional BI”; helpful, but not fully agentic.


- Verification is straightforward because this is Tableau. You can always inspect calculated fields and see exactly how metrics are defined. Nothing feels hidden.


- The UI is unmistakably classic BI software. Dense, powerful, slightly enterprise-heavy. If you’ve used Tableau before, it feels natural. If not, there’s a learning curve.


**Score (out of 5)**


Speed:` 4` · Correctness:` 4` · Transparency:` 4` · Iteration:` 4` · Shareability:` 4`


Benchmark signal Tableau


Churn chart generation Correct churn rate calculation and line chart by plan


Feb → Mar change analysis Explained method but did not compute deltas directly


Calculation visibility Calculated field visible and editable


Export / sharing options Strong publishing and dashboard sharing via Tableau Server/Cloud


Setup friction AI features had to be manually enabled


Verification path Inspect calculated fields in the data model


Primary limitation Conservative. Avoids computing comparisons directly


### **Polymer**


- Category: No/low-code dashboard builder


- OSS: No


- Free tier / Pricing range: 7-day trial; ~approximately **$50–$250 per month** , depending on plan (Starter, Pro, Teams), with **enterprise pricing available on request** .[Source](https://www.polymersearch.com/pricing) .


- Best for: quick spreadsheet-to-dashboard workflows and lightweight sharing


****


We uploaded the` tiny_churn.csv` directly into Polymer and ran the same prompts. Polymer’s UI is much lighter than traditional BI tools; more guided, less modeling-heavy. It feels designed for “upload → ask → share” rather than semantic modeling or calculated fields.


**Prompt 1: Churn chart (monthly by plan)**


When asked to compute churn rate as` churned / users` and show a chart by month and plan, Polymer didn’t render the block at all. The error stated:


> Columns are missing or disabled (monthly_churn_rate)


In other words, the tool didn’t successfully create the derived metric on the fly. Instead of guessing or miscalculating, it failed to render, which is safer than hallucinating, but still friction.


**Prompt 2: February to March change (with numbers)**


Interestingly, Prompt 2 produced a result; but in a different style. The output leaned more narrative, summarizing what changed rather than tightly computing and presenting the deltas in a structured numeric comparison.


It felt closer to an AI summary layer on top of a dashboard than a calculation-first BI engine.


****


**Prompt 3: Calculation logic (SQL / code)**


When asked to show the calculation steps, Polymer surfaced Python-style code showing how churn rate and the February-to-March difference were computed:


```text
df['monthly_churn_rate'] = df.groupby(['month', 'plan'])['churned'].transform('sum') / ...


```


Even though the first chart failed, the transparency response clearly showed grouping logic and delta calculation.


However, this code exists in the explanation layer; not as something you directly manage in a modeling interface. You’re seeing what the AI claims it did, not necessarily inspecting a persistent transformation in the dashboard layer.


Generated response:


-


```text
Certainly! Here's the code used to compute the churn rate and the change from February to March:


import pandas as pd


# Load the dataset
df = pd.read_csv('./download_tmp/6996cb406fda030008d81693.csv')


# Calculate monthly churn rate for each 'month' and 'plan'
df['monthly_churn_rate'] = df.groupby(['month', 'plan'])['churned'].transform('sum') / df.groupby(['month', 'plan'])['users'].transform('sum')


# Extract February and March data
february_data = df[df['month'] == '2026-02'].groupby('plan')['monthly_churn_rate'].mean()
march_data = df[df['month'] == '2026-03'].groupby('plan')['monthly_churn_rate'].mean()


# Calculate the change in churn rate from February to March
churn_rate_change = (march_data - february_data).sort_values(ascending=False)


(february_data, march_data, churn_rate_change)
Explanation:
Load the dataset: Read the CSV file into a pandas DataFrame.
Calculate the churn rate: Compute the monthly_churn_rate by dividing the sum of churned users by the sum of users grouped by month and plan.
Extract monthly data: Filter the dataset for February and March, then calculate the mean churn rate for each plan.
Compute changes: Determine the change in churn rate from February to March for each plan, then sort in descending order to see which plan had the largest increase.
This code gives a clear view of how churn rates are calculated and how they change over a specific period, such as from February to March.


```


**Export and share options (if supported)**


Polymer’s export story is straightforward and modern. You can share dashboards via link, export charts as images, and embed views externally. This part feels native to the product; sharing is clearly a first-class use case.


Generated response:


```text
To share or export insights as stakeholder-ready artifacts, you have several options within the Polymer tool suite:


Dashboard Sharing: Create and share an AI-generated dashboard that can be accessed by stakeholders. This allows real-time interaction and exploration of the data.
Export as Image: Export charts or graphs as images that can be easily embedded in presentations or reports.
Shareable Links: Generate shareable links for dashboards or specific insights that stakeholders can view directly in their browser.
Embedded View: Embed interactive charts or dashboards into websites or portals that stakeholders can access.


Would you like me to guide you through any of these options or perform any specific one for you?
```


**Experience**


-


Polymer feels lightweight and approachable compared to traditional BI tools. There’s very little setup overhead, and the interface doesn’t assume you’re a data modeler.


- That said, Prompt 1 exposed a weakness quickly. The derived metric didn’t render, which means either the tool struggled with on-the-fly calculation or expected the metric to be predefined. It didn’t hallucinate; which is good; but it also didn’t gracefully recover.


- Prompt 2 leaned more into narrative explanation than precise numeric breakdown. That’s fine for exploration, but if you’re looking for tight metric control, it feels less deterministic than Tableau or SQL-backed tools.


- The transparency response was surprisingly strong. Seeing actual grouping logic in code form helps build trust; but because it’s generated, you still have to assume the execution matches the explanation.


- Overall, Polymer feels optimized for quick insight packaging and sharing rather than precise metric governance. It’s fast when it works; but less robust when you push into calculated fields.


**Score (out of 5)**


Speed:` 4` · Correctness:` 3` · Transparency:` 2` · Iteration:` 3` · Shareability:` 3`


Benchmark signal Polymer


Churn chart generation Failed; tool did not create the derived` churned / users` metric and returned an error (` monthly_churn_rate` missing)


Feb → Mar change analysis Partial; produced a narrative explanation rather than a clear numeric comparison


Calculation visibility Moderate; surfaced Python-style code explaining the aggregation and delta logic


Export / sharing options Strong; dashboards can be shared via link, images exported, and views embedded


Setup friction Low; CSV upload and prompting were straightforward with minimal configuration


Verification path Indirect; generated code explains the logic, but it is not tied to a persistent modeling layer


Primary limitation Weak handling of derived metrics; struggled to compute churn rate on the fly


### **ThoughtSpot**


****


- Category: Enterprise BI (search/AI)


- OSS: No


- Free trial: Yes.[Source](https://www.thoughtspot.com/trial?tsref=webtopnav) .


- Pricing range: approximately **$25–$50 per user per month** , depending on plan (Essentials or Pro), with **Enterprise pricing available on request** .[Source](https://www.thoughtspot.com/pricing) .


- Best for: self-serve search on governed data


****


We started by uploading the CSV. ThoughtSpot does a nice job of easing you into the workflow. You get a quick dataset preview and a chance to sanity-check columns before you even start asking questions, which already reduces the chance of asking something vague and getting a confident but wrong chart.


****


**Prompt 1: Churn chart (monthly by plan)**


ThoughtSpot computed monthly churn rate by plan using churned divided by users, then surfaced both a visualization and a narrative summary. The output was actually pretty clean here. It gave me the exact churn rates per month for Pro and Enterprise, and the chart matched the numbers.


Generated response:


```text
I’ve computed the monthly churn rate by plan using the formula: churned ÷ users. Here’s what the data reveals:
Key Insights:
Pro Plan shows an increasing churn trend:


January 2026: 10.0% churn rate
February 2026: 11.2% churn rate
March 2026: 13.0% churn rate


Enterprise Plan shows more volatility:


January 2026: 5.0% churn rate (lowest)
February 2026: 6.5% churn rate
March 2026: 10.0% churn rate (sharp increase)


Summary:
Pro plan consistently has higher churn rates than Enterprise, with a steady upward trend over the 3-month period


Enterprise plan experienced a significant jump in March 2026, doubling from its January baseline
Both plans show concerning upward churn trajectories that may warrant investigation into customer satisfaction and retention strategies
```


****


**Prompt 2: February to March change (with numbers)**


Prompt 2 is where ThoughtSpot looked strongest. It did not just wave at the chart and tell me to eyeball it. It computed the delta explicitly and called out that Enterprise had the biggest February to March increase. The numbers were clear and the explanation was reasonable.


Generated response **:**


```text
Enterprise Plan Had the Largest Churn Rate Increase
The Numbers:


Enterprise Plan: Increased by 3.5 percentage points (from 6.5% to 10.0%)
Pro Plan: Increased by 1.8 percentage points (from 11.2% to 13.0%)


Explanation:


Enterprise plan's churn rate increased the most from February to March, nearly doubling the increase seen in the Pro plan. While the Enterprise plan started with a lower churn rate in February (6.5% vs Pro's 11.2%), it experienced a dramatic spike of 3.5 percentage points in March. This represents a 53% relative increase in churn rate for Enterprise customers.


This sharp jump is particularly concerning because Enterprise customers typically represent higher value and are expected to have more stable retention. The sudden acceleration suggests a potential issue affecting Enterprise customers specifically that warrants immediate investigation.


```


****


**Prompt 3: Calculation logic (SQL / code)**


ThoughtSpot did not expose SQL or generated code, but it did give a clear explanation of the grouping and the formula it applied. The “Show Work” style output feels a lot like a reasoning toggle in LLM tools. It is not lineage, but it is enough to see what it thinks it computed.


This is a good middle ground. You get transparency at the metric and grouping level, even if you do not get the underlying query.


Generated response:


```text
I appreciate your interest in understanding the methodology! However, I don't generate or display SQL code or programming scripts directly. Instead, I work with the data through analytical queries that compute the metrics you need.


Here's what happened behind the scenes:


Churn Rate Calculation
Formula Applied: Churn Rate = Sum(churned) / Sum(users)


Grouped By:


Month (2026-01, 2026-02, 2026-03)
Plan (Enterprise, Pro)


This produced the monthly churn rates for each plan that you saw in the chart.


February → March Change Calculation
For Enterprise Plan:


March churn rate: 0.1 (10.0%)
February churn rate: 0.06542056074766354 (6.5%)
Change: 0.1 - 0.06542056074766354 = 0.0346 or 3.5 percentage points


For Pro Plan:


March churn rate: 0.13023255813953488 (13.0%)
February churn rate: 0.11162790697674418 (11.2%)
Change: 0.13023255813953488 - 0.11162790697674418 = 0.0186 or 1.9 percentage points


The comparison showed Enterprise had the larger absolute increase (3.5 pp vs 1.9 pp).


Note: These calculations were performed directly on the dataset using the churned and users columns, aggregated by month and plan. The system handles the computational work while I focus on interpreting and presenting the insights to you.


```


****


**Export and share options (if supported)**


On export and sharing, the assistant response itself claims it cannot generate links or files from “the conversation.” That is a little confusing because ThoughtSpot as a platform obviously supports sharing dashboards and exporting in normal workflows.


For the purpose of this benchmark, we treated this as a minor ding. The core product is shareable, but the AI layer did not make the pathway explicit.


Generated response **:**


```text
I don't have the capability to export files, generate download links, create dashboards, or produce shareable artifacts directly from our conversation.


## **What I've Provided:**


✅ **Analysis & Insights**: Complete churn rate analysis with calculations and business insights


✅ **Visualizations**: Interactive chart embedded in this conversation showing monthly churn rate trends by plan


✅ **Data Tables**: Formatted tables with the underlying numbers


## **What I Cannot Do:**


❌ Export to PDF, PowerPoint, or Excel


❌ Generate shareable links or URLs


❌ Create downloadable images or files


❌ Build or publish dashboards


❌ Send emails or share to external platforms


# **Monthly Churn Rate Analysis by Plan**


**Period:** January - March 2026


## **Executive Summary**


**Key Finding:** Enterprise plan experienced the most significant churn rate increase from February to March 2026, with a **3.5 percentage point jump** (6.5% → 10.0%), representing a 53% relative increase.


---


## **Monthly Churn Rate Trends**


![thoughtspot-prompt4-churn_rate_by_month_and_plan.png](attachment:68170d89-a226-4ab2-9bf5-656769d6991d:thoughtspot-prompt4-churn_rate_by_month_and_plan.png)


[thoughtspot-prompt4-churn_rate_by_month_and_plan.png](attachment:435a4e74-ffdd-4b39-bd58-898809020d66:thoughtspot-prompt4-churn_rate_by_month_and_plan.png)


### **The Data**


| Plan | January 2026 | February 2026 | March 2026 | Feb→Mar Change |
| --- | --- | --- | --- | --- |
| **Enterprise** | 5.0% | 6.5% | 10.0% | **+3.5 pp** |
| **Pro** | 10.0% | 11.2% | 13.0% | +1.9 pp |


---


## **Key Insights**


### **1. Enterprise Plan - Alarming Acceleration**


- Started with the **lowest churn rate** in January (5.0%)
- Experienced a **dramatic spike** in March (10.0%)
- **3.5 percentage point increase** from Feb→Mar is the largest change observed
- This doubling of the churn rate in one month warrants immediate investigation


### **2. Pro Plan - Steady Upward Trend**


- Consistently **higher baseline churn** than Enterprise (10-13% range)
- Shows a **gradual, steady increase** over the three-month period
- 1.9 percentage point increase from Feb→Mar indicates ongoing retention challenges


### **3. Calculation Methodology**


- **Churn Rate Formula:** Churned Users ÷ Total Users
- Computed monthly for each plan using aggregated user and churn data


---


## **Recommendations**


⚠️ **Immediate Action Required for Enterprise Customers:**


- Investigate what changed in March 2026 affecting Enterprise retention
- Review Enterprise customer feedback and support tickets from this period
- Analyze if pricing, product changes, or competitive factors drove the spike


📊 **Pro Plan Retention Strategy:**


- Address the consistent upward churn trend with targeted retention programs
- Identify common characteristics of churning Pro customers


---


This analysis is ready for stakeholder review and provides both the visual trends and detailed numbers to support strategic decision-making.
```


****


**Experience**


- ThoughtSpot has one of the best first-run experiences of the tools we tried. Uploading the CSV felt guided without being annoying. The dataset preview and light editor step reduces mistakes early, especially when your columns are small but easy to misread.


- Prompt 1 was smooth. It computed churn rate correctly and gave us both the chart and the numbers in a way that was easy to verify without extra work.


- Prompt 2 was even better. It did the actual comparison math and surfaced Enterprise as the biggest February to March mover with clear deltas. This is the kind of behavior you want when you ask “what changed” because it saves you from manual eyeballing.


- Transparency was decent but not full. We did not get SQL or code, but we did get a clear breakdown of the formula, the groupings, and the exact values used in the delta. The “Show Work” vibe is similar to what people like in ChatGPT or Claude, just applied to BI style queries.


- The one awkward part was sharing. The AI response framed export as something it cannot do, even though ThoughtSpot the product is built around shareable answers and dashboards. That feels like the assistant boundary, not a platform limitation, but it still creates confusion in the moment.


**Score (out of 5)**


Speed:` 5` · Correctness:` 4` · Transparency:` 4` · Iteration:` 4` · Shareability:` 3`


Benchmark signal ThoughtSpot


Churn chart generation Correct; computed churn rate (` churned / users` ) and generated a chart with matching values


Feb → Mar change analysis Correct; calculated deltas directly and identified Enterprise as the largest increase


Calculation visibility Moderate; explained the formula and groupings but did not expose SQL or code


Export / sharing options Moderate; platform supports dashboard sharing, but the AI assistant did not expose export pathways


Setup friction Low; guided CSV upload and dataset preview reduced schema errors


Verification path Partial; formula, groupings, and values are visible, but underlying query is not


Primary limitation Limited query transparency; reasoning is visible but underlying SQL is not exposed


### **Qlik Sense**


****


- Category: Enterprise BI (associative exploration)


- OSS: No


- Free trial: Yes.[Source](https://www.qlik.com/us/trial/qlik-cloud-analytics) .


- Pricing range: approximately **$30–$70 per user per month** , depending on plan (Qlik Sense Business or Enterprise SaaS), with enterprise contracts often negotiated based on scale.[Source](https://www.qlik.com/us/pricing) .


- Best for: exploration across complex relationships


We uploaded the CSV and used Qlik’s Insights Advisor to run the prompts. Qlik’s interface feels very much like traditional BI software. There is a strong modeling layer underneath, and the AI sits on top of that rather than feeling like a primary interface.


****


**Prompt 1: Churn chart (monthly by plan)**


Instead of computing churn rate directly, Qlik generated multiple visualizations based on users by plan, users by plan and month, and users by plan and region.


It did not immediately compute churn rate as churned divided by users. The assistant seemed to default to existing fields rather than constructing the derived metric we asked for. That means you either need to define the metric yourself in the model or guide it more explicitly.


****


**Prompt 2: February to March change (with numbers)**


The February to March comparison did produce an output, but it felt less deterministic. It did not clearly compute and surface the delta in a straightforward way. The insight required more interpretation than the ThoughtSpot example.


**Prompt 3: Calculation logic (SQL / code)**


Qlik’s transparency exists at the script and expression level, but it is not surfaced through the AI layer. You can inspect load scripts and expressions manually, but the assistant itself does not clearly show its computational logic. The reasoning does not feel visible in the way code-based tools do.


**Export and share options (if supported)**


Export and sharing are native platform features. However, they were not meaningfully integrated into the AI interaction during this test.


**Experience**


- This felt like using a powerful BI engine with an AI helper attached, rather than an AI-first workflow.


- The first prompt struggled. Instead of constructing churn rate, it defaulted to showing users across dimensions. That suggests the assistant prioritizes existing measures and fields over building new derived metrics on the fly. For this benchmark, that is a noticeable limitation.


- The second prompt was serviceable but not sharp. It did not clearly compute and present the February to March delta in a way that felt definitive. You could get there, but it required interpretation rather than delivering a crisp numeric answer.


- Transparency was the weakest part. While Qlik absolutely supports deep modeling and scripting, none of that is surfaced conversationally. If you want to verify logic, you drop down into expressions and load scripts yourself. The AI does not guide you through it.


- Overall, Qlik feels built for teams that already have a data model and want to explore associations across it. For lightweight CSV upload and quick metric construction, it felt heavy and less fluid compared to the other tools in this test.


**Score (out of 5)**


Speed:` 2` · Correctness:` 3` · Transparency:` 1` · Iteration:` 1` · Shareability:` 3`


Benchmark signal Qlik Sense


Churn chart generation Failed; did not compute churn rate (` churned / users` ) and instead generated charts based on existing fields such as users


Feb → Mar change analysis Partial; produced a comparison but did not clearly compute or present the churn delta


Calculation visibility Low; AI layer did not expose expressions, scripts, or calculation logic


Export / sharing options Moderate; dashboards and visuals can be exported or shared through the platform


Setup friction Moderate; CSV upload worked but meaningful analysis required additional modeling or metric definition


Verification path Indirect; logic can be inspected through load scripts and expressions outside the AI assistant


Primary limitation Weak derived metric generation; assistant prioritized existing measures instead of constructing churn calculations


### **ChatGPT**


****


- Category: Conversational analysis


- OSS: No


- Free tier: Yes.


- Pricing range: approximately **$20–$200 per month for individual plans** (Plus at about $20/month, Pro around $200/month). Team plans start around **$25–$30 per user/month** , with **Enterprise pricing available on request** .[Source](https://chatgpt.com/pricing/) .


- Best for: quick EDA + code-first charts


*This evaluation was conducted using ChatGPT running on the GPT-5.3 model (as of March 2026).*


We uploaded` tiny_churn.csv` directly into Advanced Data Analysis and ran the prompts exactly as written. The workflow is simple. Upload file. Ask question. It generates Python, runs it, and shows you the chart inline.


The output style is consistent across prompts. You get code, a chart, and a written explanation.


****


**Prompt 1: Churn chart (monthly by plan)**


When asked to compute churn rate as churned divided by users and visualize it by month and plan, ChatGPT wrote the full pandas aggregation logic and plotted the result using matplotlib.


The key detail is that it did not simply divide row by row. It grouped by month and plan, summed churned and users, then computed churn rate correctly at the aggregate level. That matters.


The chart rendered cleanly and matched the underlying numbers.


-


Code


```text
# Load necessary libraries
import pandas as pd
import matplotlib.pyplot as plt


# Load the dataset
file_path = "/mnt/data/tiny_churn.csv"
df = pd.read_csv(file_path)


# Ensure proper date format if a date column exists
# Assuming there's a 'month' column already aggregated monthly
# If not, try parsing a date column named 'date'
if 'date' in df.columns and 'month' not in df.columns:
df['date'] = pd.to_datetime(df['date'])
df['month'] = df['date'].dt.to_period('M').astype(str)


# Compute churn rate
df['churn_rate'] = df['churned'] / df['users']


# Aggregate monthly churn rate by plan (if multiple rows per month/plan)
monthly_churn = (
df.groupby(['month', 'plan'], as_index=False)
.agg({'churned': 'sum', 'users': 'sum'})
)
monthly_churn['churn_rate'] = monthly_churn['churned'] / monthly_churn['users']


# Pivot for plotting
pivot_df = monthly_churn.pivot(index='month', columns='plan', values='churn_rate')
pivot_df = pivot_df.sort_index()


# Plot (single chart, multiple lines)
plt.figure()
for plan in pivot_df.columns:
plt.plot(pivot_df.index, pivot_df[plan])


plt.xlabel("Month")
plt.ylabel("Churn Rate")
plt.title("Monthly Churn Rate by Plan")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()


```


****


**Prompt 2: February to March change (with numbers)**


For the February to March comparison, it again generated code to compute the deltas explicitly. It returned a small comparison table with exact churn rates and increases, then clearly identified Enterprise as the largest mover.


-


Code


```text
import pandas as pd


# Load the dataset
df = pd.read_csv("/mnt/data/tiny_churn.csv")


# Ensure month column exists
if 'date' in df.columns and 'month' not in df.columns:
df['date'] = pd.to_datetime(df['date'])
df['month'] = df['date'].dt.to_period('M').astype(str)


# Aggregate monthly churn rate by plan
monthly = (
df.groupby(['month', 'plan'], as_index=False)
.agg({'churned': 'sum', 'users': 'sum'})
)
monthly['churn_rate'] = monthly['churned'] / monthly['users']


# Filter for February and March
feb = monthly[monthly['month'] == '2026-02'][['plan', 'churn_rate']].set_index('plan')
mar = monthly[monthly['month'] == '2026-03'][['plan', 'churn_rate']].set_index('plan')


comparison = feb.join(mar, lsuffix='_feb', rsuffix='_mar')
comparison['increase'] = comparison['churn_rate_mar'] - comparison['churn_rate_feb']


comparison


```


-


Explanation


-


```text
### Increase in Churn Rate (February → March)


| Plan | Feb Churn | Mar Churn | Increase |
| --- | --- | --- | --- |
| Enterprise | 0.0654 | 0.1000 | **0.0346** |
| Pro | 0.1116 | 0.1302 | 0.0186 |


### Answer


**Enterprise** had the largest increase in churn rate from February to March.


- Enterprise churn increased by **0.0346 (3.46 percentage points)**
- Pro churn increased by **0.0186 (1.86 percentage points)**


### Short Explanation


While both plans experienced higher churn in March, Enterprise’s churn rate rose nearly **twice as much** as Pro’s. This indicates a sharper deterioration in retention among Enterprise customers during that period.
```


**Prompt 3: Calculation logic (SQL / code)**


This is where ChatGPT stands out.


It did not just provide the Python code. When prompted, it also gave an equivalent SQL version of the same calculation. The aggregation logic, grouping, churn rate formula, and February to March delta were all visible and inspectable.


There is no hidden semantic layer. The logic is *explicit* .


-


Explanation


-


```text
# 1️⃣ Python Code Used


### Step 1 — Aggregate monthly totals by plan


```python
import pandas as pd


df = pd.read_csv("/mnt/data/tiny_churn.csv")


# If needed, derive month from date
if 'date' in df.columns and 'month' not in df.columns:
df['date'] = pd.to_datetime(df['date'])
df['month'] = df['date'].dt.to_period('M').astype(str)


monthly = (
df.groupby(['month', 'plan'], as_index=False)
.agg({
'churned': 'sum',
'users': 'sum'
})
)
```


---


### Step 2 — Compute churn rate


Churn rate formula:


[


\text{churn rate} = \frac{\text{churned}}{\text{users}}


]


$$


\text{churn rate} = \frac{\text{churned}}{\text{users}}


$$


```python
monthly['churn_rate'] = monthly['churned'] / monthly['users']
```


---


### Step 3 — Compute Feb → Mar change


```python
# Extract Feb and Mar


comparison = feb.join(mar, lsuffix='_feb', rsuffix='_mar')


# Change calculation


comparison
```


Formula used:


\text{Increase} = \text{Churn Rate}*{Mar} - \text{Churn Rate}*{Feb}


$$
\text{Increase} = \text{Churn Rate}{Mar} - \text{Churn Rate}{Feb}
$$


---


# 2️⃣ Equivalent SQL Version


### Step 1 — Aggregate monthly totals


```sql
WITH monthly AS (
SELECT
month,
plan,
SUM(churned) AS churned,
SUM(users)   AS users
FROM tiny_churn
GROUP BY month, plan
)
```


---


### Step 2 — Compute churn rate


```sql
, churn_calc AS (
SELECT
month,
plan,
churned,
users,
churned * 1.0 / users AS churn_rate
FROM monthly
)
```


---


### Step 3 — Compute Feb → Mar change


```sql
SELECT
f.plan,
f.churn_rate AS feb_churn_rate,
m.churn_rate AS mar_churn_rate,
m.churn_rate - f.churn_rate AS increase
FROM churn_calc f
JOIN churn_calc m
ON f.plan = m.plan
WHERE f.month = '2026-02'
AND m.month = '2026-03';
```


```


****


**Export and share options (if supported)**


ChatGPT allowed exporting the results as HTML and PDF artifacts. The workflow is not the same as a dashboard platform, but it is capable of generating shareable files directly from the analysis.


****


**Experience**


- This felt like the most frictionless workflow of the entire benchmark. Upload the file, ask the question, and you immediately see both the reasoning and the result.


- Prompt 1 was strong. It correctly grouped before computing churn rate, which is something many tools get subtly wrong when working row by row. That is an important detail for trust.


- Prompt 2 was equally clean. The delta calculation was explicit, and the answer was direct. There was no need to interpret a chart manually.


- Transparency is where this workflow shines. There is zero mystery about how the numbers were computed. The grouping logic, aggregation, and delta formula are right there in the code. When asked for SQL, it translated the same logic without hesitation.


- Iteration is smooth because everything is code driven. If you refine the question, you see exactly what changed in the script.


- The only structural difference compared to BI platforms is that collaboration and governance are not native in the same way. You can export artifacts, but there is no built-in permission model or shared semantic layer. For solo analysis or quick exploratory work, this is extremely strong. For governed reporting, it would need to plug into something else.


**Score**


Speed:` 5` · Correctness:` 5` · Transparency:` 5` · Iteration:` 5` · Shareability:` 5`


Benchmark signal ChatGPT (Advanced Data Analysis)


Churn chart generation Correct. Generated Python code to aggregate by month and plan, computed churn rate correctly, and produced a chart matching the underlying numbers


Feb → Mar change analysis Correct. Computed churn deltas explicitly and returned a clear comparison table identifying Enterprise as the largest increase


Calculation visibility High. Full Python code shown and equivalent SQL provided when requested


Export / sharing options Moderate. Results can be exported as HTML or PDF artifacts, but dashboard style sharing is limited


Setup friction Low. Upload CSV and run prompts directly with no configuration required


Verification path Direct. Users can inspect the full Python and SQL logic used for aggregation and delta calculations


Primary limitation Limited governance and collaboration features compared with enterprise BI tools


### **Claude**


****


- Category: Conversational analysis


- OSS: No


- Free tier: Yes


- Pricing range: approximately **$20 per month for Claude Pro** , with **Max plans at about $100–$200 per month** for higher usage limits. Team plans typically start around **$30 per user per month** , with enterprise pricing available on request.[Source](https://claude.com/pricing) .


- Best for: explanation + app-like artifact outputs


*This evaluation was conducted using Anthropic's Claude (*` *claude-opus-4-6*` *) as of March 2026).*


We uploaded \`tiny_churn.csv\` directly and ran the same three prompts. Claude immediately shifted into artifact mode for the first chart, generating an interactive visualization rather than just a static image.


The workflow feels slightly different from ChatGPT. Instead of simply returning code and a chart, Claude tends to package the output as something closer to a mini app. There is more emphasis on presentation and structure.


**Prompt 1: Churn chart (monthly by plan)**


Claude correctly aggregated churned and users across regions, computed churn rate as churned divided by users, and displayed a clean interactive chart. It also surfaced the exact churn percentages in a small summary table.


The numbers matched expectations:


- Pro: 10.00% → 11.16% → 13.02%


- Enterprise: 5.00% → 6.54% → 10.00%


The artifact view felt polished. It looked ready to embed without additional formatting.


Generated response:


```text
Here's the chart. The churn rates (churned ÷ users, aggregated across regions) are:


| Month | Pro | Enterprise |
| --- | --- | --- |
| Jan | 10.00% | 5.00% |
| Feb | 11.16% | 6.54% |
| Mar | 13.02% | 10.00% |


Both plans show an upward trend in Q1, but Enterprise churn nearly doubled from Jan to Mar — worth investigating. Hover over the summary cards to isolate each line.
```


-


[Claude artifact](https://claude.ai/public/artifacts/5d986900-c055-4062-ab9b-2ae41087faaf) .


**Prompt 2: February to March change (with numbers)**


For the February to March comparison, Claude responded clearly and directly. It identified Enterprise as having the larger increase and quantified the delta precisely.


The explanation leaned slightly interpretive, adding possible business causes such as renewal cycles or onboarding issues. This is useful for storytelling, but it is also something to watch if you want strictly descriptive analysis.


-


Output:


-


```text
Enterprise had the larger increase — its churn rate jumped +3.46 percentage points (from 6.54% to 10.00%), compared to Pro's +1.86 pp increase (from 11.16% to 13.02%).
So while Pro still has the higher absolute churn rate, Enterprise is the one accelerating faster. That nearly 3.5 pp month-over-month spike suggests something changed for Enterprise customers in March — could be a contract renewal cycle, a product issue hitting larger accounts, or onboarding problems with newer Enterprise cohorts.


```


**Prompt 3: Calculation logic (SQL / code)**


When asked to show the calculation steps, Claude walked through the logic step by step and provided full Python code.


The breakdown was structured:


1. Load the data


2. Aggregate by month and plan


3. Compute churn rate


4. Compute February to March delta


The transparency level is high. You can inspect the aggregation logic, verify the churn formula, and confirm the delta calculation. There is no hidden transformation layer.


The explanation format is slightly more verbose than ChatGPT’s, but it is very readable.


Generated response:


```text
Here's the full walkthrough:


Load the 12 raw rows (2 regions × 2 plans × 3 months).
Aggregate by summing users and churned across NA + EU for each month/plan pair.
Churn rate = churned ÷ users × 100 — e.g. Enterprise Feb = 7 ÷ 107 = 6.54%.
Feb → Mar delta
= March rate − February rate:


Pro: 13.02% − 11.16% = +1.86 pp
Enterprise: 10.00% − 6.54% = +3.46 pp


Enterprise's increase is nearly double Pro's, confirming it's the plan to investigate.


```


-


Code:


```text
import csv


# ── Step 1: Load the raw data ──────────────────────────────────
rows = []
with open("/mnt/user-data/uploads/tiny_churn.csv") as f:
for r in csv.DictReader(f):
r["users"] = int(r["users"])
r["churned"] = int(r["churned"])
rows.append(r)


print("Step 1 — Raw data:")
print(f"  {'month':<10} {'region':<5} {'plan':<12} {'users':>6} {'churned':>8}")
for r in rows:
print(f"  {r['month']:<10} {r['region']:<5} {r['plan']:<12} {r['users']:>6} {r['churned']:>8}")


# ── Step 2: Aggregate by month + plan (sum across regions) ─────
agg = {}
for r in rows:
key = (r["month"], r["plan"])
if key not in agg:
agg[key] = {"users": 0, "churned": 0}
agg[key]["users"] += r["users"]
agg[key]["churned"] += r["churned"]


print("\\nStep 2 — Aggregated (summed across NA + EU):")
print(f"  {'month':<10} {'plan':<12} {'users':>6} {'churned':>8}")
for (month, plan), v in sorted(agg.items()):
print(f"  {month:<10} {plan:<12} {v['users']:>6} {v['churned']:>8}")


# ── Step 3: Compute churn_rate = churned / users ───────────────
rates = {}
print("\\nStep 3 — Churn rate = churned ÷ users:")
print(f"  {'month':<10} {'plan':<12} {'churned/users':<16} {'churn_rate':>10}")
for (month, plan), v in sorted(agg.items()):
rate = v["churned"] / v["users"] * 100
rates[(month, plan)] = rate
print(f"  {month:<10} {plan:<12} {v['churned']:>3} / {v['users']:<6}       {rate:>9.2f}%")


# ── Step 4: Feb → Mar change per plan ──────────────────────────
print("\\nStep 4 — Feb → Mar change:")
for plan in ["Pro", "Enterprise"]:
feb = rates[("2026-02", plan)]
mar = rates[("2026-03", plan)]
delta = mar - feb
print(f"  {plan:<12}  {mar:.2f}% − {feb:.2f}% = {delta:+.2f} pp")


print("\\n✓ Enterprise churn increased +3.46 pp vs Pro's +1.86 pp")


```


**Export and share options (if supported)**


This is where Claude stands out.


The artifact can be shared publicly via a link and embedded directly using an iframe. It also supports exporting to formats like HTML and PDF. That makes it feel closer to a lightweight dashboard tool than a simple chat interface.


****


**Experience**


- This felt extremely smooth from a UX perspective. Upload the file, ask the question, and you get something that already looks stakeholder ready.


- Prompt 1 was strong. The aggregation was correct and the percentages matched the math. The visual output was more polished than a basic matplotlib chart and required no formatting tweaks.


- Prompt 2 added a layer of business interpretation on top of the math. That is helpful for context, but it also introduces the possibility of narrative drift if you are not careful.


- Transparency is solid. The logic is explicit and inspectable. You can see exactly how churn was computed and how the February to March delta was derived.


- Iteration is fast and predictable. Refining the question does not change unrelated parts of the analysis.


- The main distinction compared to traditional BI tools is governance. There is no semantic model, no role-based metric definition layer. For exploration and presentation, this is very strong. For formal reporting pipelines, it would need additional structure around it.


**Score**


Benchmark signal Claude


Churn chart generation Correct. Aggregated users and churned across regions, computed churn rate correctly, and produced an interactive artifact chart with matching values


Feb → Mar change analysis Correct. Computed churn deltas explicitly and identified Enterprise as the largest increase


Calculation visibility High. Provided step by step explanation and full Python code showing aggregation and delta calculations


Export / sharing options High. Artifacts can be shared via public link, embedded with iframe, or exported to formats such as HTML and PDF


Setup friction Low. CSV upload and prompting worked immediately with no configuration required


Verification path Direct. Users can inspect the full Python logic used for aggregation and churn calculations


Primary limitation Limited governance and metric control compared with enterprise BI platforms


### **Devin DANA (AI data analysis agent)**


- Category: Agent-based analysis


- OSS: No


- Free tier: No.


- Pricing range: approximately **$20/month for the Core plan (usage-based)** , **$500/month for the Team plan** , with **Enterprise pricing available on request** .[Source](https://devin.ai/pricing) .


- Best for: structured agent workflows + reproducible steps


****


We uploaded` tiny_churn.csv` into the DANA workflow and ran the same prompts. The interaction feels slightly more procedural than ChatGPT or Claude. It reads like an analyst walking through steps rather than a chat model reacting instantly.


The output combines tables, short narrative commentary, and visible Python.


**Prompt 1: Churn chart (monthly by plan)**


DANA aggregated users and churned across regions, then computed churn rate correctly as churned divided by users. The table format made the aggregation explicit before even looking at the chart.


The numbers matched expectations:


- Enterprise: 5.0% → 6.5% → 10.0%


- Pro: 10.0% → 11.2% → 13.0%


It also surfaced intermediate totals such as Users and Churned, which helps verification.


-


Output


```text
## On it — loading the CSV and computing monthly churn rate by plan.


Here's the monthly churn rate (churned / users) by plan, aggregated across regions:


| Month | Plan | Users | Churned | Churn Rate |
| --- | --- | --- | --- | --- |
| 2026-01 | Enterprise | 100 | 5 | **5.0%** |
| 2026-01 | Pro | 200 | 20 | **10.0%** |
| 2026-02 | Enterprise | 107 | 7 | **6.5%** |
| 2026-02 | Pro | 215 | 24 | **11.2%** |
| 2026-03 | Enterprise | 120 | 12 | **10.0%** |
| 2026-03 | Pro | 215 | 28 | **13.0%** |


Both plans show rising churn over the 3 months. Enterprise doubled from 5% → 10%, while Pro climbed from 10% → 13%. Enterprise's March jump is particularly sharp (+3.5pp in one month).


```python
import pandas as pd


df = pd.read_csv("tiny_churn.csv")
grouped = df.groupby(["month", "plan"])[["users", "churned"]].sum().reset_index()
grouped["churn_rate"] = grouped["churned"] / grouped["users"]
```
```


****


**Prompt 2: February to March change (with numbers)**


The February to March comparison was clear and numerically precise. It explicitly stated:


- Enterprise: +3.5 percentage points


- Pro: +1.8 percentage points


What stood out here is that DANA referenced the underlying counts in the explanation. It mentioned churned rising from 7 to 12 for Enterprise and from 24 to 28 for Pro. That grounds the analysis in raw numbers, which builds trust.


Generated response:


```text
**Enterprise** had the larger increase: churn jumped from **6.5% → 10.0%** (+3.5pp) between Feb and Mar. Pro went from **11.2% → 13.0%** (+1.8pp).


Enterprise's spike is driven by churned users nearly doubling (7 → 12) while the user base only grew modestly (107 → 120). Pro's increase was more gradual — churned rose 24 → 28 on a flat base of 215.
```


****


**Prompt 3: Calculation logic (SQL / code)**


Transparency is strong.


DANA displayed the exact grouping logic and churn rate calculation in Python. It clearly separated steps:


1. Aggregate by month and plan


2. Compute churn rate


3. Compute February to March delta


It also showed intermediate results in comments, which makes auditing easy. You can reconstruct the entire result directly from the code.


It feels closer to a reproducible script than a conversational guess.


Generated response **:**


```text
Here are the full calculation steps:


```python
import pandas as pd


df = pd.read_csv("tiny_churn.csv")


# Step 1: Aggregate users and churned by month + plan (summing across regions)


# Step 2: Compute churn rate = churned / users
grouped["churn_rate"] = grouped["churned"] / grouped["users"]


# Results:
#   month       plan  users  churned  churn_rate
# 2026-02 Enterprise    107        7    0.065421   (6.5%)
# 2026-02        Pro    215       24    0.111628   (11.2%)
# 2026-03 Enterprise    120       12    0.100000   (10.0%)
# 2026-03        Pro    215       28    0.130233   (13.0%)


# Step 3: Feb→Mar change per plan
# Enterprise: 10.0% - 6.5% = +3.5pp
# Pro:        13.0% - 11.2% = +1.8pp
```


**Walkthrough:**


1. The CSV has rows per month/region/plan. First, sum `users` and `churned` across regions (NA + EU) to get plan-level totals per month.
2. Divide `churned / users` for each row to get the churn rate.
3. Subtract Feb rate from Mar rate to get the month-over-month change:
- **Enterprise**: 12/120 − 7/107 = 10.0% − 6.5% = **+3.5pp**
- **Pro**: 28/215 − 24/215 = 13.0% − 11.2% = **+1.8pp**
```


****


**Export and share options (if supported)**


DANA generated exportable artifacts in HTML and PDF format. The exports were structured reports rather than raw notebook outputs.


This makes it more presentation-ready than a plain chat response, though it is still not a governed dashboard environment.


****


**Experience**


- The workflow felt more methodical than purely conversational tools. There is a sense that the agent is executing a sequence rather than just generating an answer.


- Prompt 1 was correct and careful. The aggregation logic was explicit and visible, which reduces ambiguity around how churn rate was computed.


- Prompt 2 went a step further by tying the percentage change back to absolute counts. That detail helps prevent overinterpreting percentage swings.


- Transparency is good. The code is readable and structured, though slightly less polished in presentation compared to Claude’s artifact view.


- Speed is slightly slower than ChatGPT or Claude, mostly because the interaction feels more step driven. It is not sluggish, but it is not instant.


- Iteration is predictable. Adjusting the question leads to controlled updates rather than unexpected changes.


- This feels strongest when you want a reproducible analytical trace. It feels slightly less fluid for rapid exploratory back and forth.


**Score**


Speed:` 3` · Correctness:` 5` · Transparency:` 4` · Iteration:` 4` · Shareability:` 4`


Benchmark signal Devin (DANA)


Churn chart generation Correct. Aggregated users and churned across regions, computed churn rate correctly, and presented results in a structured table with supporting chart


Calculation visibility High. Provided structured Python code showing aggregation, churn rate calculation, and month to month delta


Export / sharing options Moderate. Generated exportable HTML and PDF reports but lacks dashboard style sharing


Setup friction Moderate. Workflow requires structured agent steps rather than simple conversational prompts


Verification path Direct. Users can inspect the Python code and intermediate aggregation results


Primary limitation Slightly slower and more procedural workflow compared with conversational analysis tools


### **Databricks AI/BI Genie**


****


- Category: Lakehouse-native analytics


- OSS: Partial ecosystem, but product is not OSS


- Free tier / trial: Yes (typically 14-day trial with usage creditsn).[Link](https://www.databricks.com/signup?dbx_source=www&itm_data=dbx-web&l=en-EN&itm_source=www&itm_category=product&itm_page=pricing&itm_location=body&itm_component=hero&itm_offer=signup)


- Pricing range: **usage-based starting around $0.07–$0.65 per DBU (Databricks Unit)** depending on workload type and tier, plus separate cloud infrastructure costs. Typical teams spend **$500–$5,000+ per month** depending on compute usage and scale.[Source](https://www.databricks.com/product/pricing) .


- Best for: SQL-first orgs + governed lakehouse


**Run notes**


We created the` tiny_churn.csv` table inside Databricks and ran the prompts through the AI Assistant. The workflow feels very SQL native. Instead of generating Python, it produces SQL queries directly against the table in the workspace.


The output includes the SQL and a chart preview built from the query result.


**Prompt 1: Churn chart (monthly by plan)**


For the churn rate calculation, the Assistant generated structurally correct SQL:


```text
SELECT
month,
plan,
SUM(churned)/ SUM(users)AS churn_rate
FROM workspace.default.tiny_churn
GROUPBYmonth, plan
ORDERBYmonth, plan


```


The aggregation logic was correct. It grouped by month and plan, summed churned and users, and computed churn rate properly at the aggregate level. The chart rendered cleanly from the result.


No conceptual issues here.


Generated response:


```text
To analyze the monthly churn rate by plan, I will compute the churn rate as the ratio of churned users to total users for each month and plan in the dataset. Visualizing this information can help to identify trends and patterns in user behavior and evaluate the performance of different plans over time. Below is the SQL query to retrieve the necessary data:


```sql
SELECT
`month`,
`plan`,
SUM(`churned`) / SUM(`users`) AS churn_rate
FROM
`workspace`.`default`.`tiny_churn`
GROUP BY
`month`,
`plan`
ORDER BY
`month`,
`plan`


```


```


This query groups the data by month and plan, calculates the sum of churned users and total users for each group, and then computes the churn rate by dividing the sum of churned users by the sum of total users. The results are ordered by month and plan, making it easy to visualize the trend over time.


****


****


****


**Prompt 2: February to March change (with numbers)**


This is where things got slightly messy.


The Assistant generated SQL to compare February and March, but it assumed the wrong month format and year. It filtered using values like` '2022-02-01'` , while the dataset used` '2026-02'` . The logic was technically sound, but the context was wrong, which meant the query returned no rows.


After correcting the month values manually, the logic worked perfectly and produced the correct result: Enterprise had the larger increase.


So the reasoning pattern was right. The schema validation step was missing.


Generated response:


To determine which plan's churn rate increased the most from February to March, we will calculate the churn rate for each plan in both February and March, then compare the differences. The query will show the churn rates for both months and the resulting increase. Here is the SQL query:


-


```text
WITH churn_rates AS (
SELECT
`month`,
`plan`,
SUM(`churned`) / SUM(`users`) AS churn_rate
FROM
`workspace`.`default`.`tiny_churn`
WHERE
`month` IN ('2022-02-01', '2022-03-01')
GROUP BY
`month`,
`plan`
)
SELECT
`plan`,
MAX(CASE WHEN `month` = '2022-02-01' THEN churn_rate ELSE NULL END) AS feb_churn_rate,
MAX(CASE WHEN `month` = '2022-03-01' THEN churn_rate ELSE NULL END) AS mar_churn_rate,
(MAX(CASE WHEN `month` = '2022-03-01' THEN churn_rate ELSE NULL END) - MAX(CASE WHEN `month` = '2022-02-01' THEN churn_rate ELSE NULL END)) AS increase
FROM
churn_rates
GROUP BY
`plan`
ORDER BY
increase DESC
LIMIT 1


```


The` WITH` clause calculates the churn rates for February and March. The main query then selects the churn rates for each plan in these months and computes the increase. The results are ordered by the increase, and the plan with the highest churn rate increase is returned.


This query compares data across specific months, revealing which plan experienced the most significant rise in churn rate from February to March.


**Prompt 3: Calculation logic (SQL / code)**


Transparency is strong here because everything is SQL. There is no hidden transformation layer.


However, the initial version missed a few practical details. It forgot to partition correctly by plan in one version and did not validate the actual month values in the dataset before filtering.


Once corrected, it was clean and fully reproducible.


Generated response:


```text
To calculate churn rate and the month-to-month change from February to March, we can look into the ratio of churned users to total users for each month, and then determine the difference in churn rates between February and March. Below is the step-by-step SQL query to achieve this.


First, we need to compute the churn rate for each month. The churn rate is defined as the ratio of churned users to the total number of users.


```sql
SELECT
`month`,
SUM(`churned`) / SUM(`users`) AS churn_rate
FROM
`workspace`.`default`.`tiny_churn`
GROUP BY
`month`
ORDER BY
`month`
```


Next, to determine the change in churn rate from February to March, we calculate the churn rates and then compute the difference between the two months.


```sql
WITH monthly_churn_rate AS (
SELECT
`month`,
SUM(`churned`) / SUM(`users`) AS churn_rate
FROM
`workspace`.`default`.`tiny_churn`
GROUP BY
`month`
)
SELECT
`churn_rate` AS feb_churn_rate,
LEAD(`churn_rate`) OVER(ORDER BY `month`) AS mar_churn_rate,
LEAD(`churn_rate`) OVER(ORDER BY `month`) - `churn_rate` AS change_rate
FROM
monthly_churn_rate
WHERE
`month` = '2023-02-01'
```


In this query, we use a common table expression (CTE) to compute the churn rate for each month and then leverage window functions to calculate the month-to-month change specifically between February and March.
```


****


**Export and share options (if supported)**


When asked to export or share, the Assistant suggested creating dashboards or using *built-in* sharing features rather than directly packaging the existing analysis. It also drifted slightly from the churn-by-plan context and generated a new churn-by-region example instead.


The SQL ran correctly, but it shifted scope rather than building directly on the previous work.


**Experience**


-


Databricks AI generated structurally correct SQL and handled the aggregation logic properly. The issue was that it assumed the wrong month values and date format, which caused the query to return no rows. The logic wasn’t wrong; the context was. It didn’t validate the actual data shape before filtering, so I had to manually adjust the month values. Once corrected, the output matched perfectly.


- What I ended up using:


-


```text
WITH churn_rates AS (
SELECT
month,
plan,
SUM(churned) * 1.0 / SUM(users) AS churn_rate
FROM workspace.default.tiny_churn
WHERE month IN ('2026-02', '2026-03')
GROUP BY month, plan
)
SELECT
plan,
MAX(CASE WHEN month = '2026-02' THEN churn_rate END) AS feb_churn_rate,
MAX(CASE WHEN month = '2026-03' THEN churn_rate END) AS mar_churn_rate,
MAX(CASE WHEN month = '2026-03' THEN churn_rate END)
- MAX(CASE WHEN month = '2026-02' THEN churn_rate END) AS increase
FROM churn_rates
GROUP BY plan
ORDER BY increase DESC
LIMIT 1;


```


-


Got the *concept* right, but missed “by plan,” used a mismatching month format/year, and the snippet wasn’t runnable as given (missing the full CTE +` PARTITION BY plan` ).


- What I ended up using:


-


```text
WITH churn_rates AS (
SELECT
month,
plan,
SUM(churned) * 1.0 / SUM(users) AS churn_rate
FROM workspace.default.tiny_churn
WHERE month IN ('2026-02', '2026-03')   -- adjust if your month is a DATE
GROUP BY month, plan
)
SELECT
plan,
MAX(CASE WHEN month = '2026-02' THEN churn_rate END) AS feb_churn_rate,
MAX(CASE WHEN month = '2026-03' THEN churn_rate END) AS mar_churn_rate,
MAX(CASE WHEN month = '2026-03' THEN churn_rate END)
- MAX(CASE WHEN month = '2026-02' THEN churn_rate END) AS increase
FROM churn_rates
GROUP BY plan
ORDER BY increase DESC;


```


-


Databricks AI handled this one better conceptually, it acknowledged its limitations (can’t directly export artifacts) and suggested realistic in-product options like dashboards and shareable links. However, it drifted from our original churn-by-plan analysis and instead generated a new churn-by-region query. The SQL ran correctly and produced valid results, but it didn’t directly package or transform my prior analysis into a stakeholder-ready artifact; it shifted scope rather than building on the existing work.


-


This feels strongest if your organization already lives in SQL and governed lakehouse workflows.


-


Prompt 1 was solid and structurally correct. Prompt 2 revealed the main weakness. The Assistant did not validate the actual data values before applying filters. The logic was fine, but the context assumption broke the query.


-


Once corrected, the results were exactly right.


-


Transparency is high because everything is explicit SQL. There is no mystery about how churn is computed. The downside is that you are responsible for catching schema mismatches.


-


Iteration works, but it requires careful review. You cannot blindly run and trust the first query without checking filters and formats.


-


This is powerful in a governed environment, but it assumes technical literacy.


**Score (out of 5)**


Speed:` 4` · Correctness:` 3` · Transparency:` 4` · Iteration:` 3` · Shareability:` 3`


Benchmark signal Databricks AI/BI + Assistant


Churn chart generation Correct. Generated SQL that grouped by month and plan and computed churn rate correctly from aggregated values


Feb → Mar change analysis Partial. SQL logic was correct but used the wrong month format and returned no rows until manually corrected


Calculation visibility High. All computations are visible as SQL queries executed directly against the table


Export / sharing options Moderate. Suggested creating dashboards or links but did not package the current analysis as a shareable artifact


Setup friction Moderate. Dataset had to be created as a table in the workspace before running prompts


Verification path Direct. Users can inspect and run the generated SQL queries directly


Primary limitation Weak schema validation. The assistant assumed incorrect date formats and required manual correction


### **Graphy**


-


Category: No/low-code dashboards


-


OSS: No


-


Free tier / Pricing range: **Free plan available** , with paid plans starting around **$16 per user per month** for Plus and **$36 per user per month** for Business.[Source](https://graphy.app/pricing) .


-


Best for: quick dashboards + Google ecosystem


-


What it looks like


We uploaded the CSV and ran the same prompts. The interface is clean and very visual. It feels more like a dashboard builder than a query engine.


There is less emphasis on underlying logic and more emphasis on generating a presentable view quickly.


**Prompt 1: Churn chart (monthly by plan)**


Graphy produced a churn visualization, but it was not immediately clear how the churn rate was computed. The chart looked reasonable, but there was no visible calculation logic behind it.


****


****


**Prompt 2: February to March change (with numbers)**


It returned a visual comparison for February to March. The numbers looked aligned with expectations, but again, there was no visible formula or aggregation breakdown.


**Prompt 3: Calculation logic (SQL / code)**


When asked to show SQL or calculation steps, it explicitly stated it could not provide the underlying code.


After pushing further, it attempted to regenerate logic and then failed with a “No recipe generated” response.


This is the core limitation here. You get a result, but you cannot inspect how it was derived.


Generated response:


```text
I can't show the calculation steps in SQL or code. However, I can help you visualize the churn rate and the change from February to March using the dataset.
When I asked the above (what it said it can help me doing): it started hallucinating which resulted in “No recipe generated”.
```


****


**Export and share options (if supported)**


Export options are strong from a presentation standpoint. You can export an image, generate an embeddable link, and embed dashboards. The free export includes watermarking.


This is clearly built for distribution.


Exported via the options provided in graphy (image but with watermark, embeddable link, and embed code).


**Experience**


- This is fast for producing something that looks dashboard ready.


- The main issue is verification. There is no easy way to inspect how churn rate was computed or whether aggregation happened correctly across regions. That makes it risky for anything beyond surface level reporting.


- Prompt 3 was effectively blocked because transparency is not part of the workflow. When asked for SQL or logic, the system either declined or hallucinated and failed.


- Iteration is visual rather than analytical. You adjust views, not logic.


- Shareability is strong. Governance and traceability are weak.


- This feels optimized for quick presentation, not analytical rigor.


**Score (out of 5)**


Speed:` 4` · Correctness:` 3` · Transparency:` 1` · Iteration:` 2` · Shareability:` 4`


Benchmark signal Graphy


Churn chart generation Partial. Produced a churn visualization but did not show how the churn rate was calculated


Feb → Mar change analysis Partial. Displayed a visual comparison that appeared correct but did not expose the underlying calculation


Calculation visibility Low. The system does not expose SQL, formulas, or calculation logic


Export / sharing options High. Dashboards can be exported as images, shared via links, or embedded


Setup friction Low. CSV upload and chart generation worked immediately


Verification path None. Users cannot inspect how metrics are computed


Primary limitation No calculation transparency. Results cannot be audited or reproduced


### Where notebooks fit and how Deepnote changes things


There's a middle ground that the tool categories above don't fully cover: the analyst who wants the speed of a conversational tool, the transparency of seeing the actual code, and the ability to share and iterate with a team.


That's the notebook workflow. You write a few cells of Python, generate a chart with Plotly or Matplotlib, and you can see every transformation, every filter, every grouping decision in plain code. The verification problem disappears because there's nothing hidden; the code *is* the audit trail.


The problem with notebooks has always been everything around the code. JupyterLab gives you a strong local editing experience, but sharing a notebook means emailing a file or pushing to GitHub and hoping your colleague has the same environment. Turning a notebook into something a non-technical stakeholder can use means reaching for Voila, Streamlit, Dash, or Panel; each with its own framework, deployment story, and maintenance burden.


Deepnote closes that gap. To see how it behaves in practice, we ran the same benchmark used throughout this article.


The workflow typically looks like this: select **Generate with AI** , provide a prompt, and Deepnote inserts the required cells. That usually includes a callout block with the prompt text, one or more code cells, and sometimes a markdown explanation.


The result feels less like a conversation and more like the AI helping construct a reproducible notebook.


Deepnote also provides an AI sidebar alongside the notebook interface. This sidebar supports two modes: **Edit** and **Ask** . In **Edit mode** , the AI can directly modify the notebook by inserting or updating cells, generating charts, or refactoring existing code. In **Ask mode** , the interaction is conversational and works more like a typical AI chat assistant.


During this benchmark, we used two AI workflows in Deepnote. Prompt 1 used the *inline* **Generate with AI** flow inside cells, while Prompts 2, 3, and 4 were completed through the **AI sidebar** . In the sidebar, **Edit** mode can add or modify notebook blocks directly, while **Ask** mode is conversational and better suited for analysis, explanation, and planning without changing the notebook structure.


****


**Model used:** Deepnote was tested with a[custom model setup](https://deepnote.com/docs/ai-custom-models) using **Claude 4.6** for the AI-assisted benchmark steps.


- Category: Collaborative notebooks


- OSS: Yes.[Link](https://github.com/deepnote/deepnote) .


- Free tier: Yes


- Pricing (as of 2026): approximately $0–$39 per user/month, depending on plan, with team and enterprise pricing available.[Source](https://deepnote.com/pricing) .


- Best for: collaborative notebooks + reproducible analysis


**Prompt 1: Churn chart (monthly by plan)**


The first prompt triggered Deepnote’s **Generate with AI** interface. Because the CSV file was already visible in the Files panel, the assistant suggested chart and analysis actions related to that dataset.


After entering the prompt, Deepnote generated several notebook blocks automatically:


• A callout block containing the prompt text
• A Python code cell implementing the calculation
• A chart output cell


The generated code aggregated` churned` and` users` by **month and plan** , then computed churn rate as` churned / users` . The chart output displayed the churn trend for both plans across the three months.


The code and output were placed directly in the notebook, and the cell block could be collapsed so only the visualization remained visible.


**Prompt 2: February to March change (with numbers)**


For the second prompt, we used Deepnote’s **AI sidebar** rather than the inline generation flow. The agent first outlined a short execution plan, then proposed notebook edits to validate the existing` monthly` DataFrame, filter to February and March, pivot churn rates by plan and year, and compute the month-over-month deltas.


The result was correct and clearly summarized. The agent identified **Enterprise in 2026** as the largest mover, with churn rising from **6.54% in February to 10.00% in March** , a **3.46 percentage point increase** . It also surfaced the next result for context, showing **Pro** increasing from **11.16% to 13.02%** , or **1.86 percentage points** .


**Prompt 3: Calculation logic (SQL / code)**


Instead of jumping straight to an answer, the agent broke the work into explicit notebook steps: add explanatory text blocks, generate the code for churn-rate computation, generate the code for the February-to-March comparison, execute both blocks, and then review the outputs.


The response made the calculation logic easy to audit. It clearly separated the workflow into two parts: **A) churn rate computation** and **B) February-to-March change analysis** . For the first part, it described loading` tiny_churn.csv` , resolving expected columns, normalizing the date field to monthly granularity, aggregating by month and plan, and computing` churn_rate = churned / users` . For the second part, it validated the` monthly` DataFrame, filtered to February and March, pivoted the monthly rates, calculated the delta, ranked the results, and produced a formatted summary.


**Export and share options (if supported)**


****


Instead of generating notebook blocks, the agent returned a structured explanation of the main ways this analysis could be shared in Deepnote. The response covered both notebook-style sharing and more stakeholder-facing formats such as apps and static exports.


For this benchmark, the most relevant recommendation was to publish the notebook as an app, keeping the churn chart, summary text, and key metrics visible while hiding the underlying code.


```text
1. Share as a live notebook link
- Share a read-only link to the notebook directly
- Stakeholders can view outputs, scroll through results, and see formatted tables


2. Publish as a Data App
- Convert the notebook into a cleaner dashboard-style view
- Hide code blocks and keep only charts, tables, text, and big numbers visible
- Share the app through its own URL or embed it elsewhere


3. Add Big Number blocks
- Highlight key metrics such as Enterprise churn increase
- Useful for stakeholder-facing dashboards and summaries


4. Export to PDF or HTML
- Download a static snapshot of the notebook
- Better suited for email, point-in-time reporting, or archiving


5. Schedule and auto-refresh
- Run the notebook automatically on a recurring cadence
- Keep the published output updated without manual reruns
```


That guidance matches one of Deepnote’s biggest advantages over traditional notebook environments. Sharing is not limited to exporting a file. Teams can choose between notebook links, published apps, embeds, and static outputs depending on who the audience is and how interactive the final artifact needs to be.


Deepnote’s sharing model goes beyond notebook export. A notebook can be published as an app, with only the relevant blocks visible to stakeholders while code, setup, and intermediate steps remain hidden. Input blocks such as dropdowns, sliders, and date pickers are native and reactive, which makes it easier to turn an analysis into a lightweight interactive deliverable without adding a separate framework.


**Experience**


- Prompt 1 generated correct code and a clean visualization immediately through the inline **Generate with AI** workflow. Because the code lives directly in the notebook, it is easy to verify or modify.


- Prompts 2, 3, and 4 ran through the **AI sidebar** , which made the workflow feel more structured and agentic. Instead of only returning answers, the sidebar explained its plan, proposed notebook edits, and then summarized the output in a way that was easy to review.


- Prompt 2 showed that Deepnote could correctly compute the February-to-March change while also presenting the logic in a concise, readable way. Prompt 3 highlighted the biggest strength of this workflow: transparency. The full calculation pipeline remained visible, reproducible, and easy to audit. Prompt 4 showed that Deepnote’s sharing model is broader than most conversational tools, with practical options for notebook links, published apps, embeds, and static exports.


- Another advantage here is that Deepnote’s notebook workflow is not just collaborative, but also easier to review and version over time. Projects include built-in[history](https://deepnote.com/docs/history) and[code reviews](https://deepnote.com/docs/code-reviews) , and teams can also connect notebooks to[GitHub](https://deepnote.com/docs/github) or[GitLab](https://deepnote.com/docs/gitlab) for a more standard Git-based workflow. That makes it easier to inspect changes, discuss edits, and restore an earlier state when needed. Compared with most conversational analysis tools, the work is much easier to review and roll back in a structured way.


- Compared to conversational tools like ChatGPT or Claude, the workflow is slightly less instant but much more structured. Compared to traditional BI tools, it offers more transparency because the underlying code remains visible. Overall, this setup works especially well for teams that want a reproducible analytical environment that can also be shared with collaborators and stakeholders.


One advantage of the notebook approach is that the AI layer isn’t locked to a single model. Deepnote supports[custom AI models](https://deepnote.com/docs/ai-custom-models) through a bring-your-own-key (BYOK) workflow, which lets teams run their own OpenAI or other supported models inside the notebook environment.


This matters for evaluation and production workflows. Instead of relying on whatever model a tool ships with, teams can control which model runs the analysis, adjust prompts, and compare outputs across models, all while keeping the code and results in the same reproducible notebook.


**Score (out of 5)**


Speed:` 4` · Correctness:` 4` · Transparency:` 3` · Iteration:` 4` · Shareability:` 4`


Benchmark signal Deepnote


Churn chart generation Correct. Generated Python code and produced the chart directly in the notebook


Feb → Mar change analysis Correct. Computed churn deltas and surfaced the correct comparison


Calculation visibility High. Full Python code and intermediate DataFrame outputs visible


Export / sharing options Medium. Supports notebook export, shareable links, and app publishing


Setup friction Low. CSV upload and AI generation work directly inside the notebook


Verification path Direct. Users can inspect and modify the generated code


Primary limitation Requires familiarity with notebooks compared to chat-only tools


### Why AI visualization tools fail in practice


The interesting question isn’t which tool has the best AI model. It’s which tool makes it hardest to ship a wrong answer.


In practice, AI data visualization tools fail in predictable ways. Even a small dataset like the one used in this benchmark surfaces them quickly.


1. Ambiguous definitions treated as resolved


- Ask for “top customers” and most tools will give an answer. But top by what?


- Revenue, lifetime value, account size, or something else?


- Many tools guess a definition and move forward without clarifying it. In this benchmark, the definition check is churn rate. Tools need to compute **churned / users** correctly and label it clearly. If they do not, that is an *immediate* red flag.


2. Incorrect grouping and hidden transformations


- With real datasets, most mistakes come from grouping and transformations.


- Dates get bucketed incorrectly. Cohorts get mixed. Filters apply in unexpected ways.


- The benchmark intentionally avoids joins, but it still tests whether the tool exposes the calculation logic. If you cannot inspect how the metric was computed, it becomes difficult to trust the chart.


3. Overconfident summaries


- Many tools generate narrative explanations alongside charts.


- The risk is not that the sentence is wrong. The risk is that it sounds causal when it is only descriptive.


- Prompt 2 in the benchmark reveals this behavior quickly. Tools that mix explanation with speculation can easily drift from analysis into storytelling.


4. No audit trail for iteration


- When you refine a prompt, two things can change:


- the visualization


- the metric definition


- If the tool does not show its logic or track the analytical steps, it becomes difficult to know what changed.


- Tools that expose SQL, code, or visible calculation steps make this much easier to audit.


### **How to think about the decision**


The right tool depends on two questions most people skip: who is the audience for this output, and what happens if it's wrong?


- If the chart is for your own exploration; a quick look at the data before a deeper dive; speed matters more than governance. Conversational tools shine here. Upload a CSV to ChatGPT or Claude, ask your question, and treat the answer as a draft to verify. The risk is low because you're the one verifying.


- If the chart is going into a report that executives will use to make budget decisions, the calculus changes. Now you need governed definitions (so "revenue" means the same thing every time), audit trails (so you can explain how the number was calculated), and repeatability (so next month's report uses the same logic). Enterprise BI platforms are built for this. The AI features speed up authoring, but the governance layer is what makes the output trustworthy.


- If the work is collaborative, multiple analysts contributing, stakeholders reviewing, comments, and iterations before something gets shared, the tool needs to support that workflow natively. Most BI tools handle collaboration through permission models and shared dashboards. Conversational tools mostly don't; a ChatGPT session is personal, ephemeral, and non-shareable in any structured way.


Factor AI-first (conversational) Governed (enterprise BI)


Data readiness Clean tables, known columns Messy data, unclear metric definitions, lots of prep needed


Audience and stakes Internal exploration, reversible decisions External-facing, exec decisions, compliance-adjacent


Transparency needs Good enough if you can inspect the code Must show lineage, SQL, and governed definitions


Speed vs. rigor Need a first answer in minutes Will invest time for correctness and repeatability


Collaboration Lightweight sharing is sufficient Many stakeholders, review cycles, and versioning


### Which AI data visualization tools to use


AI visualization tools are good for first drafts. They reduce the time between "I have a question" and "I have a chart" from hours to seconds. That's genuinely valuable for exploration, hypothesis generation, and getting a rough sense of what the data looks like.


They are not good at being right without supervision. The failure modes: ambiguous definitions, silent grouping choices, overconfident narratives, and opaque iteration, are structural, not bugs that will get patched in the next release. They exist because producing a confident answer quickly is fundamentally at odds with producing a verified answer carefully.


Based on running the same simple test across 10 tools, here's what we'd do:


**For quick exploration:**
ChatGPT or Claude. Fast, code-first answers with minimal setup. Great for exploratory analysis, but you still need to verify results before sharing.


**For governed reporting:**
Tableau, Power BI, Looker, Deepnote, or ThoughtSpot. These tools enforce shared metric definitions and semantic layers, which makes them reliable for dashboards and executive reporting.


**For collaborative analysis:**
Deepnote. The analysis stays transparent because the code is visible, but it’s also easy to share notebooks, turn them into apps, and collaborate with teammates and AI agents in the same environment.


Srihari Thyagarajan


Technical Writer


Follow Srihari on[Twitter](https://x.com/hari_leo03) ,


[LinkedIn](https://www.linkedin.com/in/srihari-thyagarajan/)


and[GitHub](https://github.com/Haleshot)
