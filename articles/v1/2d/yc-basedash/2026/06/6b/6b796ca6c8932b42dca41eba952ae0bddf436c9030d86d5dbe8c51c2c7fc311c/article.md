---
schema_version: "1.0.0"
document_id: "6b796ca6c8932b42dca41eba952ae0bddf436c9030d86d5dbe8c51c2c7fc311c"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/drill-down-vs-drill-through-in-bi-tools-what-they-do-and-when-to-use-each/"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:2f6147c8b4e86fb8191d8a047be21bd025687433db42185b75dd4a1cdd3055ca"
---

# Drill-down vs drill-through in BI tools: what they do and when to use each

Drill-down and drill-through sound like the same feature, and a lot of BI documentation treats them as interchangeable. They are not. Drill-down stays inside a single chart or report and exposes more granularity along a dimension you already chose. Drill-through jumps the user out of the current report and into a different one that is filtered by the row, segment, or value they clicked.


This guide is for analytics engineers, BI leads, and product managers who need to decide which exploration paths to build into their dashboards. It explains what each capability actually does, where the line between them sits, when each one matters, how the major BI tools implement them, and how AI-assisted exploration is changing the conversation.


## The short answer


- **Drill-down** is “show me more detail in the same view.” A user clicks` 2026 Q2` on a quarterly bar chart and sees the same chart broken out by month, then by week. The chart, the metric, and the report are unchanged. Only the granularity along the dimension hierarchy changes.
- **Drill-through** is “take me to a different view that is filtered by what I clicked.” A user clicks the` North America` slice of a revenue donut and lands on a separate report showing every closed-won deal in North America for the period.
- Drill-down is a navigation pattern within a chart’s dimension hierarchy. Drill-through is a navigation pattern across reports.
- Most modern BI tools support both, but the implementation, performance characteristics, and authoring effort differ enough that picking the wrong one for a given question is a common reason dashboards get abandoned.


## What is drill-down?


Drill-down lets a viewer move along a predefined dimension hierarchy without leaving the chart. The classic example is a date hierarchy: year, quarter, month, week, day. The chart’s metric stays constant. The grouping changes one level at a time.


A drill-down typically requires three things in the underlying model:


1. **A defined hierarchy.** Either a flat date column the BI tool can decompose, or an explicit hierarchy on a dimension like geography (region, country, state, city) or org (org, department, team, individual).
2. **A metric that aggregates cleanly at every level.** Sums, counts, distinct counts, and weighted averages aggregate up and down without surprises. Ratios, medians, and percentiles do not, and a chart that drills into a level where the calculation is no longer correct creates trust problems quickly.
3. **A query plan that runs at every level.** Most tools generate a new SQL query each time the user drills. On large fact tables without indexes or aggregates on the lower-level columns, a single drill click can turn a fast dashboard into a 40-second wait.


Drill-down is best for dashboards where the question is “the same metric, but more granular.” Revenue by quarter, by month, by week. Active users by country, by state, by city. Tickets by priority, by category, by sub-category.


It is not the right pattern when the question changes shape, for example when the user wants to see a different metric, switch to a row-level list, or pivot to a different fact table. That is what drill-through is for.


## What is drill-through?


Drill-through routes a click on a chart to a different report or page that is pre-filtered by the value the user clicked. The destination is a separate object in the BI tool. It can have its own metric, its own visualizations, and its own permission scope.


The defining property of drill-through is that the destination report is built independently. The link between the source and the destination is the filter context that gets passed when the user clicks. A click on` Acme Corp` in a customer revenue chart can pass` customer_id = 14219` to a customer detail page that shows that customer’s deals, invoices, support tickets, and product usage in one place.


Drill-through is the right pattern when the question changes shape:


- A summary chart shows a number, and the user needs the underlying list of records that contributed to it.
- A KPI shows that a metric moved, and the user needs a diagnostic report that breaks down the move by segment.
- A board-level dashboard surfaces an exception, and the user needs the operational dashboard for the team responsible for that line of business.


Drill-through is also where row-level security gets interesting. The destination report is its own object, so the permissions that apply to it are usually the destination’s permissions, not the source’s. Row-level security and access controls have to be designed end to end, or a viewer can hit a destination that is more permissive than the chart they came from. We covered this pattern in more depth in[data governance for AI-powered BI](https://www.basedash.com/blog/data-governance-for-ai-powered-bi-row-level-security-access-controls-and-compliance) .


## The real difference, in practice


The two capabilities collapse into one in casual use because both feel like “click the chart to see more.” The differences that actually matter when you are designing a dashboard:


Aspect Drill-down Drill-through


Stays in the same chart Yes No, navigates to another report


Changes the metric No Often


Requires a hierarchy Yes No, just a passable filter


Authoring effort Low. Define hierarchy, drag dimension. Higher. Build a separate report and define link parameters.


Performance pattern New aggregate query at the lower level. Sensitive to indexes and pre-aggregations. New report query with filters applied. Sensitive to the destination’s complexity.


Permission boundary Same chart, same audience. Source and destination can have different permissions.


Best for “Same metric, more granular.” “Different question, prefiltered by what I clicked.”


These differences also explain why a dashboard that uses both well feels coherent, and one that picks the wrong pattern for a given question feels broken. A user who clicks a quarter and lands on a different report has lost their place. A user who clicks a customer name and watches the same chart re-aggregate by week has seen something that does not answer their question.


## When drill-down is the right pattern


Drill-down works well when:


- The chart’s job is to compare a single metric across a hierarchy that the audience already understands.
- The dimension has natural levels: time, geography, org, product taxonomy, deal stage.
- The metric stays meaningful at every level. Revenue, deal count, active users, tickets, sessions.
- The user’s next question is “show me the same thing one level finer,” not “show me a different thing.”


Concrete examples:


- Quarterly revenue by region. A click on` EMEA` drills to revenue by country in EMEA. A click on` Germany` drills to revenue by city.
- Monthly active users by product surface. A click on` Search` drills into MAU by feature within Search.
- Ticket volume by category. A click on` Billing` drills into ticket volume by sub-category, then by reason code.


Drill-down is the cheaper of the two patterns to implement well, and it is usually the first one a new BI team should reach for. It works as long as the model has the columns and the query plan stays fast. Pre-aggregating monthly and weekly rollups, or letting a[semantic layer](https://www.basedash.com/blog/what-is-a-semantic-layer-the-complete-guide-for-modern-bi) define the hierarchy and aggregation rules, are the two cheapest ways to keep drill-down responsive on large fact tables.


## When drill-through is the right pattern


Drill-through earns its complexity when:


- The user needs to leave the summary view and inspect individual records, transactions, or tickets.
- A KPI flagged a change and the next step is a diagnostic dashboard with different visuals.
- The audience for the detail view is different from the audience for the summary view, including different permissions.
- The summary report is shared widely and you want to keep it small, while detail lives in heavier reports that fewer people open.


Concrete examples:


- An executive dashboard’s revenue tile drills through to a deal detail report with one row per closed-won deal in the period, including owner, source, and notes.
- A support manager’s ticket SLA dashboard drills through from the SLA breach card to a ticket triage view with per-ticket actions.
- A finance team’s expense overview drills through from an outlier vendor to a transaction-level audit report scoped to that vendor.
- An embedded customer dashboard’s` unusual activity` chart drills through to a record-level audit log filtered to the customer.


The key design rule for drill-through is that the destination should answer a question the source could not. If the destination is just a longer version of the same chart, you wanted drill-down.


## How major BI tools implement each


Most BI tools support both capabilities, but the language, authoring model, and constraints differ enough that buyer evaluations should test the specific exploration paths the team needs.


Tool Drill-down Drill-through


[Tableau](https://help.tableau.com/current/pro/desktop/en-us/qs_drill_down.htm) Native, via dimension hierarchies on the shelf or` +/-` icons on dates. Works against any data source. Supported via[actions](https://help.tableau.com/current/pro/desktop/en-us/actions_filter.htm) (filter, set, parameter) that pass selections from one sheet or dashboard to another. Authored explicitly.


[Power BI](https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-drill-through-buttons) Native, via[drill-down icons](https://learn.microsoft.com/en-us/power-bi/consumer/end-user-drill) and dimension hierarchies. Works on dates and explicit hierarchies. First-class concept called[Drillthrough](https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-drillthrough) . Build a destination page, add fields to its drillthrough filters, and Power BI auto-generates the right-click menu.


[Looker](https://cloud.google.com/looker/docs/drilling) Implemented via drill fields defined in LookML at the field level. The model author controls what is drillable and to what depth. Implemented through[links](https://cloud.google.com/looker/docs/reference/param-field-link) on dimensions and measures, which can route to dashboards, looks, or external URLs with parameters.


[Metabase](https://www.metabase.com/docs/latest/questions/visualizations/drill-through) Native through the “drill-through” menu (despite the name, this includes drill-down behavior on date and category fields). Limited compared to Looker or Power BI. Click actions can navigate to filtered questions, but custom drill destinations require dashboard parameters and click behaviors set up per chart.


[Mode](https://mode.com/) Limited. Mode is notebook-first, so drill-down is usually authored as Python or SQL parameters rather than as an interactive control. Achieved by linking dashboard cells to parameterized reports. Heavier authoring lift than the other tools.


[Hex](https://learn.hex.tech/docs/explore-data/cells/visualization-cells/chart-cells#drill-down) Native on date and category dimensions inside chart cells. Achieved by chaining cells and parameters across an app, or linking to other apps with URL parameters.


[Sigma](https://help.sigmacomputing.com/docs/intro-to-drill-anywhere) “Drill anywhere” lets users add or change grouping dimensions ad hoc, which extends drill-down beyond a fixed hierarchy. Supported via[actions](https://help.sigmacomputing.com/docs/create-actions) that open another workbook, page, or external link with passed parameters.


[Basedash](https://www.basedash.com/) Supported on date and category fields in charts and tables. AI-driven exploration also lets users ask follow-up questions in natural language without leaving the dashboard, which covers many drill-down questions implicitly. Supported via cross-dashboard links and click actions. Filters from the source pass to the destination, with permissions enforced per dashboard.


A few observations from these patterns:


- Tools that emphasize a[semantic layer](https://www.basedash.com/blog/what-is-a-semantic-layer-the-complete-guide-for-modern-bi) (Looker, Sigma, Cube-backed tools) tend to give model authors more control over what is drillable, which protects performance and prevents misleading drill paths.
- Tools that emphasize a notebook or code-first surface (Mode, Hex) treat both capabilities as parameter-passing problems rather than first-class UI features. This is more flexible but requires more authoring effort.
- AI-native tools blur the line between drill-down and ad hoc exploration. If a user can ask “now break this down by city” in natural language and the tool generates the right query, the dashboard does not need a pre-defined hierarchy at all.


## Common pitfalls


Even when both capabilities are available, four mistakes show up consistently when teams build dashboards.


### Drill-downs that lie at lower levels


A chart that shows a 30-day rolling churn rate at the company level can produce nonsense when a user drills into a single segment with 12 customers. Rolling rates, medians, and percentiles do not aggregate cleanly across hierarchies. Either disable drill-down on those metrics, or replace the metric with one that is meaningful at every level (raw counts, simple ratios) before exposing the path.


### Slow drills


Drill-down generates a new query at the lower level. On a fact table without indexes on the drill column, that query can scan the entire table. Pre-aggregations, materialized views, or a[warehouse-side query layer](https://www.basedash.com/blog/how-to-cut-cloud-data-warehouse-costs-from-bi-dashboards) keep drills fast. Without one of those, the third or fourth drill click is when users decide the dashboard is broken.


### Drill-through destinations that are stale


If the source report runs every five minutes and the destination report runs once a day, a user who drills through to investigate a fresh anomaly lands on yesterday’s data. Either align the refresh schedules, or label the destination clearly so the audience understands the staleness.


### Drill-through that leaks permissions


The destination report has its own permission scope. If the source dashboard is shared widely and the destination shows fields the audience should not see, the drill-through link becomes the leak. Row-level security has to apply on the destination, not just the source. This is a common audit finding when companies first roll out customer-facing or partner-facing dashboards.


### Hierarchies that do not match how the team thinks


A geography drill that goes country -> state -> city is fine for the United States but breaks for countries that do not have states, or for businesses that organize by region first. A drill hierarchy that does not match the team’s mental model creates more frustration than no drill at all. Validate the hierarchy with the actual audience before building.


## How AI-assisted exploration changes this conversation


Two years ago, drill paths had to be designed up front, because the only way for a non-technical user to ask a follow-up question was to click something the dashboard author had pre-built. That constraint is loosening.


In a BI tool with a strong[AI data analyst layer](https://www.basedash.com/blog/how-to-evaluate-ai-data-analyst-tools-a-framework-for-2026) , a user who wants to break a metric down by city, by source, or by cohort can ask in natural language and get a chart back. The same is true for “show me the records that make up this number,” which is the question drill-through usually answers. The AI layer generates the SQL, applies the permissions, and returns the result without the dashboard author having to wire the path in advance.


This does not eliminate the need for designed drill paths. The most common questions still benefit from explicit, fast, governed drill-down and drill-through, especially in customer-facing or compliance-sensitive contexts. But it changes the buyer’s calculus: instead of designing every possible drill path, teams can design the top three or four and rely on AI exploration to cover the long tail. Tools like Basedash lean into this pattern by treating governed drills and natural-language follow-ups as the same surface.


## When you do not need either


Not every dashboard needs an exploration layer. If the dashboard’s purpose is to surface a small number of metrics for a recurring meeting, the right answer is often to keep it static and let users go to a separate exploration tool when they want to dig in.


Skip drill-down and drill-through when:


- The dashboard is a board pack or weekly review where the audience just needs the numbers.
- The team has a separate analyst-facing tool (Mode, Hex, a notebook) that is the right place for diagnostic work.
- The data model does not yet support fast drills and you would rather not ship a slow exploration path that erodes trust on the first click.
- The destination would just be a longer table of the same data, in which case a single “view all” link is enough.


The tradeoff between operational and analytical dashboards covered in our[operational vs analytical dashboards guide](https://www.basedash.com/blog/operational-dashboards-vs-analytical-dashboards-how-to-design-each) applies here. Operational dashboards rarely need drill-through. Analytical dashboards almost always do.


## FAQ


**Is drill-through the same as drill-across?**


Not quite. Drill-across is a related, less common term that usually refers to drilling between fact tables that share a conformed dimension, for example moving from a sales fact to a service fact for the same customer. Vendors use the term inconsistently. In most product UIs, drill-through is the implementation, and drill-across is a label for one specific use of it.


**Does AI-generated SQL replace drill-down and drill-through?**


For ad hoc questions, often yes. For governed paths that have to be fast, deterministic, and consistent across viewers, no. A board-level revenue dashboard still benefits from a designed drill-through to a deal detail report, even if the AI layer can answer most other follow-ups. Use AI exploration to cover the long tail and use designed drills for the spine of the dashboard.


**Does drill-down work on every chart type?**


Not natively. It is well supported on bar charts, column charts, line charts, donuts, treemaps, and tables. Scatter plots, heatmaps, and maps usually require explicit drill targets. Pivot tables behave the most like spreadsheets and tend to support drill-down on every axis.


**How do I keep drills fast on a large fact table?**


Three patterns. Pre-aggregate the lower granularities into materialized views or rollup tables, define them in a semantic layer so the BI tool picks the right one automatically, and put indexes on the columns that show up in drill filters. If those are not enough, the next step is a query acceleration layer or a smaller derived dataset for the dashboard.


**Should embedded dashboards expose drill-through?**


Sometimes. Drill-through inside an embedded dashboard is a real product surface, not just a BI nicety. Treat the destination as a feature that needs design and permissions like any other product page. We covered the data isolation side of this in our guide to[multi-tenant analytics architecture](https://www.basedash.com/blog/multi-tenant-analytics-architecture-how-to-isolate-customer-data-in-embedded-dashboards) .


**What is the simplest way to start?**


Pick one chart on your most-viewed dashboard, add a single drill-down on its primary dimension, and ship it. If users start asking “where can I see the records behind this,” that is the signal to add a drill-through. Building both for every chart up front is the fastest way to ship dashboards no one trusts.


Drill-down and drill-through are small features that quietly decide whether a dashboard is useful past the first glance. Treat them as design choices, not check-the-box capabilities, and the dashboards that use them will earn the trust that makes the rest of the work matter.
