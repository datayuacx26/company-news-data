---
schema_version: "1.0.0"
document_id: "b078cc67ff9cfc8d9e9e98a6d21d543613837c93024bfcf9dde095b72c0e69d5"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/kibana-dashboards-esql-fast-mode"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T19:47:14.307175+00:00"
fetched_at: "2026-08-04T20:28:10.240209+00:00"
content_hash: "sha256:1db113e98a4cc5ec3fe0e7565c3ecd25588a35f6a3d0a0b23d3b468b32c284c4"
---

# Close enough is fast enough: How ES|QL Fast mode makes Kibana dashboards up to 100x faster

Elasticsearch Query Language (ES|QL) STATS queries on Kibana dashboards now run up to 100x faster. ES|QL Fast mode, in general availability (GA) in Kibana 9.5, samples a fraction of the data rather than scanning every row, and results stay within a 90% confidence interval. With Fast mode, ES|QL charts pick up click-to-filter and Discover drilldowns. Plus, controls can pull their values from an ES|QL query, and metric and bar chart defaults are cleaner. This builds on the dashboard improvements[shipped in 9.4](https://www.elastic.co/search-labs/blog/kibana-dashboards-improvements) . The[Dashboards API](https://www.elastic.co/search-labs/blog/dashboards-as-code-kibana-api) and[AI dashboards and Vega-Lite charts](https://www.elastic.co/search-labs/blog/ai-dashboards-kibana-vega-lite) also go GA in this release.


## ES|QL charts performance and interactivity in Kibana dashboards


### How ES|QL Fast mode runs dashboard queries up to 100x faster


For common analytical tasks, like trend tracking, top-host identification, and capacity overviews, trading a small margin of accuracy for dramatically faster results is the right call, especially since not every question needs an exact answer.


[Elastic Search 9.4 introduced approximate ES|QL queries](https://www.elastic.co/search-labs/blog/fast-approximate-esql-part-1) as a syntax-only command in technical preview. Now, 9.5 makes approximation GA and adds[Fast mode](https://www.elastic.co/docs/explore-analyze/query-filter/languages/esql-kibana#esql-kibana-fast-mode-toggle) , a UI toggle in Dashboards and Discover that enables[approximate ES|QL STATS queries](https://www.elastic.co/docs/reference/query-languages/esql/esql-query-approximation) without writing any query syntax. This makes Kibana one of the first tools to offer smart sampling with automatic extrapolation as a simple switch.


Fast mode is an Enterprise-only feature and is off by default. Dashboard authors can save their preferred state with the dashboard, and individual queries can override the toggle with` SET approximation=true` or` false` inline.


When switched on, ES|QL STATS queries target a fixed sample size (defaulting to 1,000,000 rows for grouped aggregations and 100,000 rows otherwise) rather than scanning the full dataset.[Benchmarks show heavy aggregations running up to 100x faster](https://www.elastic.co/search-labs/blog/fast-approximate-esql-part-1) on large datasets, with results that are typically highly **** accurate, defaulting to a 90% confidence interval. Approximation only applies to STATS commands where results can remain accurate. When accuracy cannot be ensured (such as with small datasets or aggregations like MAX, MIN, or COUNT_DISTINCT), Kibana automatically falls back to exact execution, even with Fast mode enabled.


Further improvements to how charts communicate that results are approximate are coming in future releases.


### Click-to-filter and Discover drilldowns for ES|QL charts


Two of the most popular interactions for data view charts are also landing now for ES|QL-based visualizations.


-


**Discover drilldowns** now work on ES|QL panels. When a user clicks a data point or uses Explore in Discover, filters are translated to ES|QL` WHERE` clauses and Kibana Query Language (KQL) queries are carried over automatically.


-


**Click-to-filter also works for renamed fields:** if your query renames a column (` STATS BY node = k8s.node.name` ), Kibana now resolves the alias back to the indexed field, so the filter applies correctly.


-


**Tooltips:** When filtering genuinely can't work (for example, because the field was computed entirely within the query and doesn't exist in the index), Kibana now shows a tooltip explaining why, so users know that it's a query limitation.Beyond interactivity, ES|QL layers now have the same **Use global filters** toggle (gear icon on the layer header) as data-view-backed visualizations. When you turn it off, the layer's query runs independently of dashboard-level filters, just like form-based layers already do. This is useful for reference lines, thresholds, or baselines that shouldn't change when you filter the dashboard. And ES|QL metric charts now support a background chart, matching the styling option already available for data view metrics.


Beyond interactivity, ES|QL layers now have the same **Use global filters** toggle (gear icon on the layer header) as data-view-backed visualizations. When you turn it off, the layer's query runs independently of dashboard-level filters, just like form-based layers already do. This is useful for reference lines, thresholds, or baselines that shouldn't change when you filter the dashboard. And ES|QL metric charts now support a background chart, matching the styling option already available for data view metrics.


Upcoming releases aim to keep adding the remaining functionality to ES|QL visualizations, such as multilayer support and saving visualizations to the library.


### Identify which Kibana panels use an ES|QL variable


[Variable controls](https://www.elastic.co/search-labs/blog/kibana-dashboard-interactivity-variable-controls-overview) are among the most popular ES|QL-only features, since they let you parameterize chart queries to switch between fields, time intervals, or groupings without duplicating panels. On a dashboard with many panels and controls, though, it can be hard to tell which visualizations a variable actually affects. In edit mode, you can now click an ES|QL variable control's label to identify all related panels that consume the variable. Variables with no related panels display a warning to make it easier to audit wiring before saving.


## Kibana metric and bar chart layout defaults


### Metric chart preset layouts and density options


The metric chart appearance panel now offers preset layouts: **Top** , **Middle** , **Bottom** , and **Custom** . When you pick a template, the layout snaps into place. If you need fine-grained control, switch to **Custom** .


Metrics used to pack values tightly, which is great for data-dense dashboards but hard to scan when a metric stands alone. Elastic Cloud 9.5 adds a **Density** style option under **Style > Details > Other** , with two presets: **Compact** (the previous layout) and **Default** (more padding, larger typography). Newly created metrics use **Default** , and existing saved charts keep **Compact** until you change them.


**Attribute**


**Compact**


**Default**


Padding


Tight, minimal spacing


More generous whitespace


Typography


Smaller text


Larger text


Best for


Data-dense dashboards with many metrics side by side


Standalone metrics or dashboards with fewer panels


New charts


Must be selected manually


Applied automatically


Existing charts


Preserved until changed


Must be selected manually


### Responsive bar chart labels in Kibana


Labels in horizontal bar charts used to grow unchecked, so on smaller screens, a chart with long category names could become unreadable. Bar labels now get a max width and middle-truncate automatically, so the beginning and end of a label stay visible even when the full text doesn't fit. This works by default, with no configuration needed. In 9.6, we’re planning many more improvements to bar charts and labels.


## Kibana dashboard controls populated by ES|QL queries


[Controls](https://www.elastic.co/docs/explore-analyze/visualize/add-controls#create-and-add-options-list-and-range-slider-controls) are the most user-friendly way to filter a dashboard, and most dashboards use them. One of the longest-standing requests from users has been the ability to prefilter the values that a control shows. ES|QL queries make that possible and open a much wider set of possibilities, like chaining controls in new ways using variables. Regardless of how the values are populated, controls filter every panel on the dashboard, including ES|QL and data view visualizations.


Controls can now be populated from an ES|QL query instead of selecting a data view field directly. The **Create control** flyout adds a **Select a field / Write a query** toggle. You can write an ES|QL query that returns a single column and run it, and then the control derives its options from the result. Queries can reference dashboard[variables](https://www.elastic.co/docs/explore-analyze/visualize/add-variable-controls) through the` ?variable` syntax, enabling flexible chaining between controls.


## Coming soon: Progress bar visualization for Kibana tables


A new progress bar visualization type is available in Elastic Cloud Serverless and is planned for general availability in 9.6. Progress bars show a value relative to a goal or maximum, which is useful for many O11y metrics, like CPUs, memory, or Service Level Agreement (SLA) tracking.


## What's next for Kibana dashboards and ES|QL visualizations


Upcoming releases will keep pushing on better defaults, improving the ES|QL visualization experience, and adding new chart types. If you have a pain point or a feature request, select **Submit feedback** in the top menu; we're listening.


## How to try ES|QL Fast mode and the new Kibana dashboard features


If you use[Elastic Cloud Serverless](https://www.elastic.co/cloud/serverless) , you may already be using these changes. Otherwise, upgrade to 9.5, and then create a dashboard or open an existing one. Many updates apply automatically to new visualizations, while layout and style options appear in edit mode. If you aren't on Elastic Cloud yet,[start a trial](https://cloud.elastic.co/registration) and explore the latest Kibana dashboards there.


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*
