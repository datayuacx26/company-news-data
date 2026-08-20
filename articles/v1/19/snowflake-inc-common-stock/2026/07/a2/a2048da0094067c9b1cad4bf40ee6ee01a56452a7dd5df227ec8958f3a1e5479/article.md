---
schema_version: "1.0.0"
document_id: "a2048da0094067c9b1cad4bf40ee6ee01a56452a7dd5df227ec8958f3a1e5479"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/fpa-ai-modernization-snowplan-v2"
published_at: "2026-07-08T16:58:32+00:00"
first_seen_at: "2026-07-24T01:53:21.113388+00:00"
fetched_at: "2026-07-28T22:07:07.393518+00:00"
content_hash: "sha256:96bb115f3c777bfb39bd65363698dc9d015474918ba183eef753c4ad7a3ada59"
---

# Transforming FP&A with AI Modernization

Long-range planning is one of the most important exercises a finance team runs, but it is also one of the hardest to scale.


At Snowflake, we needed to forecast out 10 years across a business that had become much more complex: more than 40 entities, each with more than 100 cost centers and hundreds of spend categories underneath that. We needed that level of detail because the long-range plan was not just a finance artifact. It had become a data set that other teams depended on.


Tax, for example, does not just need a high-level expense forecast. They may need to understand the split between goods and services, legal entities, jurisdictions, and other operating details that matter for planning. Treasury needs a view into cash. Workforce planning needs headcount assumptions. Executives need to understand the strategic tradeoffs across growth, margin, investment and free cash flow.


The planning model had to support all of that.


So, like many companies, we did what finance teams often do when the business outgrows the tooling: We built a gigantic Excel model.


And over time, it became what most large finance models eventually become. It worked, but it looked like Frankenstein’s monster. New tabs were added. New formulas were bolted on. New logic was layered onto old logic to accommodate a changing business. The model became incredibly valuable, but also increasingly difficult to maintain, govern and scale.


That was the first problem we set out to solve.


## From spreadsheet model to planning platform


More than a year ago, we rebuilt the long-range planning model in Snowflake and used Streamlit as the UI layer for analysts and executives to interact with the forecast.


That became Snowplan, our internal long-range planning application.


The goal was not to create a dashboard. It was to create a planning platform. We wanted an experience that still felt intuitive to finance users, but with the scale, governance and compute power of Snowflake underneath it.


In Snowplan, analysts can update assumptions through an editable Streamlit interface. Those changes are written directly back to Snowflake, where the model runs, and the updated outputs are immediately resurfaced in the app. No broken formulas. No file saves. No wondering which version of the model is the source of truth.


That architecture changed the planning process.


Instead of maintaining a massive offline workbook, we now had a governed application connected to the raw data sources that already power the business. Actuals could flow into the model without someone spending hours updating files. Assumptions could be versioned. Scenarios could be compared. Different users could interact with the same planning platform at the right level of detail based on their role.


For individual contributors and associates, Snowplan provides granular input pages, assumption management, scenario creation and version control. For directors and managers, it provides visibility into logic and assumption changes for review and approval. For executives, it provides a consolidated view of the P&L, free cash flow and key scenarios.


That matters because long-range planning is not just a modeling exercise. It is an alignment exercise. The more time finance spends maintaining the model, the less time we spend pressure-testing strategy with the business.


Data used in the image above is fully synthetic data.


## Why building in Snowflake changed the model


The most important decision we made was to build the model where the data already lived.


Because Snowplan runs on Snowflake, it is connected to our raw data sources and governed data model. That means we are not wasting time manually updating the model with actuals or reconciling offline data pulls. The model is tied into the same environment where finance data, permissions, logic and history already exist.


That creates several advantages.


First, the model can scale. A 10-year forecast across entities, cost centers, spend categories, headcount, revenue, balance sheet and free cash flow creates a significant amount of data. That is exactly the type of workload Snowflake is built to handle.


Second, the model becomes easier to govern. Access can be managed through Snowflake role-based permissions and row-level controls, so users see the right data and functionality based on their role. Executives do not need the same interface as the analyst building the assumptions, and analysts do not need to create separate exports for every stakeholder.


Third, the model becomes reusable. Once the planning logic is built in Snowflake, it does not have to be limited to the annual long-range plan. The same platform can support workforce planning, stock-based compensation modeling, treasury cash forecasting, hedging, legal entity forecasting, COGS planning and M&A scenarios.


That is the bigger story. Snowplan is not a one-off planning app. It is becoming a platform for finance planning.


## Snowflake CoCo made scenario planning conversational


Streamlit made Snowplan scalable and usable. Snowflake CoCo made it conversational.


Before CoCo, Snowplan already gave us a better way to manage long-range planning. Analysts could update assumptions in the app, run scenarios and compare outputs. But the user still had to know where to go, which assumption to adjust and how to interpret the downstream impact.


CoCo changes that interaction model.


Now, instead of navigating through pages of assumptions, I can ask a question in plain English. I can ask CoCo to compare two versions of the forecast and summarize the major drivers. I can ask what changed between the plan we showed the board last year and the latest version we are preparing now. I can ask for the net result of the changes, the key drivers of margin expansion or dilution, and the assumptions that matter most.


That is incredibly powerful in executive planning.


When preparing for a board discussion, the question is rarely “can you give me the latest forecast?” The question is usually, “what changed, why did it change and what does that imply for the story we are telling?” CoCo helps compress that analysis from a manual comparison exercise into a conversation.


The value is not just speed. The value is that finance can iterate while the strategy discussion is still happening.


## A real example: Planning for a potential tax change


One of the best examples is scenario planning around a potential tax change.


In the old world, this kind of question would likely start with a meeting. We would discuss the issue with tax, define the impacted sales, pull the data, build an assumption, update the model, review the output, create sensitivity tables and then decide who else needed to be involved.


With CoCo connected to Snowplan, the process becomes much more fluid.


I can start by asking for an overview of the potential tax change. From there, I can ask CoCo to create a new version of the forecast assuming the tax change passes.


That immediately turns into the kind of back-and-forth a finance team would normally have in a meeting. Should we assume the tax is passed on to customers? Should we assume it is a margin hit? What percentage of the tax could realistically be passed through? Which sales would be subject to the tax? What is the impact on revenue, gross margin, operating margin and free cash flow?


Because the analysis is grounded in Snowflake tables, CoCo can identify the sales that would be subject to the tax, output the financial impact and provide backup on the key metrics driving the amount. It can also create a sensitivity table showing operating margin dilution under different assumptions for how much of the tax is passed on to customers.


Just as important, it can surface risks and considerations. For example, the first-order model may not include additional overhead costs the company would incur to support filings, maintain compliance data sets or manage new reporting obligations. That is the type of caveat a strong finance partner needs to raise before a scenario gets treated as complete.


Then CoCo can help draft the next step: an email to the right people in the tax department summarizing the analysis, the assumptions, the open questions and the decision points. That means the system is not just producing a number. It is helping diagnose the issue, identify the domain expert and move the workflow forward.


That is where this becomes more than AI-assisted modeling. It becomes AI-assisted planning.


## Why this matters for finance teams


Finance teams are often asked to answer strategic questions faster than traditional planning processes allow.


What happens if we accelerate growth? What if we open a new office? What if cloud costs improve by 25 basis points? What if compensation inflation is higher than expected? What if a tax or regulatory change affects a portion of our sales? What if we shift investment between functions?


These are not theoretical questions. They are the questions executives ask in real time.


The challenge is that traditional planning tools and large spreadsheet models were not built for that level of iteration. They were built to produce a plan, not to support a continuous strategy conversation.


By building Snowplan in Snowflake with Streamlit, we created a planning platform that could scale with the complexity of the business. By adding CoCo, we made that platform conversational.


That combination changes what finance can do.


Instead of spending time updating actuals, maintaining formulas, reconciling scenarios or manually comparing versions, finance can spend more time doing the work that actually matters: pressure-testing assumptions, aligning executives, evaluating tradeoffs and shaping long-term strategy.


## Why trust matters in AI-driven planning


For finance, conversational planning only works if the numbers can be trusted.


That is why the architecture matters. CoCo is not producing a disconnected forecast in isolation. It is interacting with the same governed data, assumptions and logic that power Snowplan. When it compares versions, explains drivers or creates a scenario, it is doing that against the Snowflake data model and planning logic we already use.


Every scenario can be versioned. Every change can be reviewed. Access can be governed through the same role-based model as the app. Analysts and executives can compare before and after, understand what changed, and roll forward or roll back scenarios as needed.


That is a critical distinction. We are not asking finance leaders to trust a black box. We are using AI to interact with a governed planning platform where the data, business logic, permissions and outputs are visible, explainable and auditable.


That is what makes AI usable in enterprise finance.


## From planning tool to strategic platform


The most exciting part of Snowplan is that it has become bigger than the original long-range planning use case.


Once the model moved into Snowflake, the architecture became reusable. The same foundation now supports, or can support, multiple planning workflows across finance: workforce headcount planning, stock-based compensation modeling, treasury cash forecasting, hedging, legal entity forecasting, COGS planning and M&A scenario modeling.


That is the advantage of treating this as a platform rather than a one-off app.


Each new planning workflow can build on the same governed foundation. Each one can connect to the relevant source data. Each one can expose a Streamlit interface that is easy for finance users to understand. And with CoCo, each one can become easier to interrogate, adjust and explain through natural language.


This is the pattern I think more finance teams will follow:


-


First, move the model to the data.


-


Then, build an intuitive app layer for users.


-


Then, use AI to make the planning process conversational.


## The real ROI


The real ROI of Snowplan is not that it makes finance more technical. It is that it gives finance more time for judgment.


Long-range planning should not be about maintaining a massive workbook. It should be about understanding the future of the business. It should help leaders decide where to invest, how to balance growth and profitability, what risks are emerging and what tradeoffs matter most.


Snowflake and Streamlit gave us the platform to make long-range planning scalable, governed and connected to live data. CoCo is helping us make it faster, more interactive and more strategic.


That is the shift.


Finance can spend less time updating the model and adjusting assumptions manually, and more time iterating with executive leaders on the long-term strategy of the company.


For FP&A teams, that is the real promise of AI in planning. Not replacing the finance function, but removing the manual work that slows it down — so finance can do more of the work it was meant to do.


*Note: If your team is interested in financial forecasting applications on Snowflake , we offer Forward Deployed Finance Specialists who work directly with customers on enterprise resource planning (ERP) migration, data infrastructure, applications and AI workflows — including templates for Finance Applications and CoCo Skills. Reach out to your Snowflake account team to learn more.*
