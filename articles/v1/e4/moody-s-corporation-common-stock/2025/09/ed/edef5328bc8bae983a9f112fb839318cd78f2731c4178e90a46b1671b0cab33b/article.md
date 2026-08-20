---
schema_version: "1.0.0"
document_id: "edef5328bc8bae983a9f112fb839318cd78f2731c4178e90a46b1671b0cab33b"
company_key: "moody-s-corporation-common-stock"
company: "Moody's Corporation"
source_id: "moody-s-corporation-common-stock-news-import-112754bd8765"
canonical_url: "https://www.moodys.com/web/en/us/creditview/blog/prompting-series-prompt-orchestration.html"
published_at: "2025-09-05T16:14:00+00:00"
first_seen_at: "2026-07-22T04:52:53.039531+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:5d188ec4d209a3e68659e01d2ab0f4c25ece1539dca627b8c067279a36ad9a7c"
---

# Prompting Part 12: Harnessing the Power of Prompt Orchestration

As we wrap up our deep dive into Moody’s Research Assistant, it’s time to bring together all the insights we’ve explored over the past 12 weeks. We’ve examined how tailored prompts can transform workflows for various personas, enhancing productivity and precision in financial analysis.


In this final post, we’ll showcase how orchestrating a sequence of specialized prompts can create comprehensive, custom solutions for different roles within an organization. Whether you’re a credit analyst, investment banker, or portfolio manager, understanding how to combine these prompts effectively can significantly elevate your analytical capabilities.


## The Art of Prompt Orchestration


Organizations, teams, and individual roles often require unique approaches to financial assessments. By strategically combining custom prompts, users can build workflows that address specific needs, ensuring that each step of the analysis is thorough and tailored.


Let’s explore how this orchestration works for three key personas: credit analyst, investment banker, and portfolio manager.


### **Credit Analyst - Issuer Report Workflow**


**Objective:** Conduct a deep-dive fundamental credit analysis on a specific company to prepare an internal report. This workflow transitions from a broad overview to detailed scrutiny of peers and financial metrics.


**Prompts and Their Impact:**


**Company Overview**


Prompt: *Write a company overview for \[ENTITY\] including its business model, revenue streams by segment, and management team.*


Sources: Moody’s Data


Value: This prompt synthesizes qualitative data from various reports into a cohesive narrative, providing a solid foundation of the company's operations and leadership.


**Rating History**


Prompt: *Show \[ENTITY\]'s rating history for the last 5 years in a table. Include the rating action date, type, and a one-sentence summary for each change.*


Sources: Moody’s Data


Value: By quickly pulling rating data and summarizing the qualitative reasoning behind each rating change, this prompt offers a clear view of the company’s credit trajectory over time.


**Credit Analysis (Up/Down Factors, Strengths and Weaknesses)**


Prompt: *Summarize the key credit strengths and weaknesses for \[ENTITY\], including factors that could lead to an upgrade or downgrade.*


Sources: Moody’s Data


Value: This prompt delves into Moody’s credit opinions, extracting core arguments to highlight the main factors influencing the company's credit status. It moves the analysis beyond raw data to provide actionable insights.


**Financials**


*Advanced Query*


Prompt: *List the 2023 financial performance for \[ENTITY\]. Calculate the year-over-year percentage change.*


Value: Automating the gathering and calculation of financial data streamlines the process, allowing you to focus on interpreting the numbers rather than compiling them.


**Peer Comparison**


*Advanced Query*


Prompt: *Compare \[ENTITY\]'s 2023 DEBT/EBITDA, EBITA MARGIN %, FCF/DEBT to the average of their peers.*


Value: This performs calculations across datasets, benchmarking the target company against its peers. It provides a comparative perspective on the company’s financial health.


**Recent Updates (Earnings)**


Prompt: *Summarize the key themes from \[ENTITY\]'s most recent earnings call, focusing on forward guidance and management's response to analyst questions on EV profitability.*


Sources: **** Moody’s Data and Earnings Calls


Value: By summarizing unstructured information from earnings call transcripts, this prompt offers the most up-to-date, forward-looking insights, enhancing the relevance of the credit assessment.


###


### **Investment Banker - Pitch Deck Creation Workflow**


**Objective:** Screen for potential acquisition targets and generate a high-level summary for an initial pitch deck. This workflow emphasizes speed and efficiency, beginning with broad screening before narrowing down to specific targets.


**Prompts and Their Impact:**


**Screen for Acquisition Targets**


*Advanced Query*


Prompt: *List of European airline companies with a rating of Ba1 or lower, revenue over $2 billion and positive free cash flow in 2024. Include 2024 Revenue, Net Debt/EBITDA, and EBITA Margin.*
**


Value: This builds a potential target list by layering financial thresholds, generating a clean table that’s ready for presentations and discussions.


**Business Profile**


Prompt: *Write a one-paragraph business profile for \[ENTITY\].*


Sources: Moody’s Data


Value: By generating a qualitative summary, this prompt prepares essential content for the "Target Overview" slide in the pitch deck, ensuring a concise and informative introduction to each potential target.


**SWOT Analysis**


Prompt: *Perform a SWOT analysis for \[ENTITY\] and its top 2 peers in a table format.*


Sources: Moody’s Data


Value: This prompt synthesizes Moody’s research into a structured SWOT framework, providing a quick and clear view of the competitive landscape and strategic positioning of each company.


**Latest News**


Prompt: *Summarize the latest news for \[ENTITY\] from the last 3 months with a positive or neutral sentiment.*


Sources: Moody’s Data & News


Value: Acting as a final check, this prompt ensures there are no recent negative headlines that could impact the attractiveness of the acquisition target, safeguarding the pitch’s integrity.


###


### **Portfolio Manager - Risk Monitoring Workflow**


**Objective:** Monitor macro sector risks and assess the health of a specific holding within the portfolio against key risk indicators. This workflow focuses on understanding overarching risks and pinpointing specific vulnerabilities within portfolio holdings.


**Prompts and Their Impact:**


**Market Risk Analysis**


Prompt: *What are the top 5 risks facing the UK banking sector currently*


Sources: Moody’s Data


Value: Starting at the macro level, this prompt addresses broad, thematic questions to grasp the current operating environment, providing context for the portfolio’s risk assessment.


**Benchmark Analysis**


*Advanced Query*


Prompt: *Calculate the 2024 median key indicators for the UK banking sector. In a column, include \[ENTITY\]'s reported and adjusted 2024 key indicators*


Value: Establishing a quantitative baseline, this prompt benchmarks the health of the sector and identifies whether specific holdings are underperforming or outperforming relative to their peers.


**Rating Change Triggers**


Prompt: *In a table, detail HSBC's upgrade and downgrade thresholds. Include most recent adjusted performance in relation to these thresholds.*


Sources: Moody’s Data


Value: Visualizing key risk metrics against critical thresholds provides an instant view of the company's position, highlighting its proximity to potential rating changes.


**Credit Event Analysis**


*Advanced Query*


Prompt: *List of any UK banking companies that have had a rating downgrade or outlook change to negative in the past 6 months.*


Value: This portfolio monitoring query scans for recent negative credit events, signaling potential contagion risks that could affect the broader sector or specific holdings.


**Outlook**


Prompt: *What is the outlook for the European banking sector and why*


Data: Moody’s Data


Value: Ensuring that the latest comprehensive research from Moody’s analysts is reviewed, this prompt confirms the overall findings and projections, providing a forward-looking perspective on sector health.


****


## Bringing It All Together


Prompt orchestration isn’t just a technical tool — it’s a smarter, faster way to work with greater confidence. Moody's Research Assistant integrates seamlessly into your workflows, optimizing processes, automating repetitive tasks, and empowering you to tackle complex challenges. Whether it’s credit analysis, pitch deck creation, or portfolio risk monitoring, it adapts to your needs so you can focus on what matters most.


Throughout our Summer Prompting Series, we’ve shown how targeted prompts can simplify complex tasks and deliver actionable insights in no time. As the financial landscape evolves, mastering prompt orchestration will help you make better-informed decisions and uncover new opportunities.


While this series concludes, your journey with Moody’s Research Assistant is just beginning — and we’re here to help you every step of the way.


**About the author:**


Caroline Hedgcock is an Assistant Director of Customer Success at Moody’s, where she specializes in helping clients optimize their use of Moody’s Research Assistant. A leader in AI prompting strategies, Caroline works closely with financial institutions to streamline workflows, accelerate analysis, and unlock deeper insights leveraging Moody’s GenAI-powered Research Assistant.


With extensive expertise in applying advanced prompting techniques, Caroline ensures that clients harness the full capabilities of the Research Assistant to enhance decision-making and operational efficiency. Her approach bridges technical innovation with practical application, helping organizations unlock the benefits of embedding GenAI into their daily workflows.
