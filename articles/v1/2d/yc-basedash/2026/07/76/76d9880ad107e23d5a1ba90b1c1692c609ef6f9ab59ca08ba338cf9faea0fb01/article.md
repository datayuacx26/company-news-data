---
schema_version: "1.0.0"
document_id: "76d9880ad107e23d5a1ba90b1c1692c609ef6f9ab59ca08ba338cf9faea0fb01"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-choose-the-right-chart-for-a-dashboard/"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:02ca2ffa04e105ad25be5a1d80d11e0ccff602decb633b933dd7693f0c0c680f"
---

# How to choose the right chart for a dashboard

Pick a chart by naming the question you are answering, not by looking at the data. Almost every business chart answers one of six questions: how do values compare across categories, how does a value change over time, what is a whole made of, how is a value distributed, how do two values relate, or what is a single number right now. Once you name the question, the shortlist of good charts is small, and most of the “which chart?” agonizing disappears.


This guide is for anyone building dashboards or reports: analysts, founders, operators, and product managers. It maps each analytical question to the charts that answer it well, calls out the charts to avoid, and covers the dashboard-specific rules that generic chart guides skip.


## The one rule: start from the question, not the columns


The most common mistake is starting from the data (“I have a date column and a revenue column, so… a line chart?”) instead of the decision the viewer needs to make. The same two columns can justify a line chart, a bar chart, or a single KPI number depending on what the reader is trying to do.


So before you touch the chart picker, finish this sentence: *“The viewer looks at this to answer ___.”* Then match that question to a chart family below.


## Chart selection by question


The question you’re answering Best charts Usually avoid


Compare values across categories Horizontal or vertical bar chart Pie chart, radar chart


Track change over time Line chart, area chart Bar chart with many time points


Show composition (part-to-whole) Stacked bar, 100% stacked bar, treemap Pie chart with more than 3 slices


Show distribution Histogram, box plot Bar chart of raw values


Show relationship or correlation Scatter plot, bubble chart Line chart between unordered points


Show a single value or status KPI card / big number, gauge sparingly A one-bar bar chart


Show rank or a top-N list Sorted horizontal bar, table Pie chart


Show a flow or drop-off Funnel chart, sankey Stacked bar for stages


Use this as a shortlist, not a law. But if you find yourself reaching for a chart that is in the “avoid” column, you usually have a better option one cell to the left.


## Comparing values across categories: use bars


If the reader needs to compare revenue by region, tickets by team, or sign-ups by plan, a bar chart is almost always the right answer. Bars encode value as length along a common baseline, which is the encoding humans read most accurately. In the classic ranking of how precisely people judge visual encodings, position and length beat angle, area, and color by a wide margin ([Cleveland and McGill, 1984](https://www.jstor.org/stable/2288400) ). That single finding is why bars beat pies for comparison and why you should be skeptical of any chart that asks the eye to judge angles or areas.


Practical tips:


- Sort bars by value, not alphabetically, unless the category has a natural order (months, sizes, funnel stages).
- Use horizontal bars when category labels are long or you have more than about eight categories. They read top to bottom and never truncate labels.
- Start the value axis at zero. Truncating the axis exaggerates small differences and is the single most common way bar charts mislead.


## Tracking change over time: use lines


For anything measured repeatedly over time (daily active users, weekly revenue, monthly churn), a line chart is the default. Lines make the trend, direction, and rate of change obvious, and they handle many time points gracefully where bars get cluttered.


- Use an area chart only when the magnitude under the line matters (cumulative totals, volume) or when you are stacking a small number of series to show composition over time.
- Keep the number of lines low. Beyond four or five series, a line chart turns into spaghetti. Use small multiples (one small chart per series in a grid) instead.
- Bars are fine for time when the points are few and discrete (revenue by quarter for two years). Once you have dozens of points, switch to a line.


## Showing composition: stacked bars beat pies


When you want to show what a total is made of (revenue by product line, traffic by channel), reach for a stacked bar or a 100% stacked bar rather than a pie. Pies force the reader to compare angles and areas, which people do poorly, and they fall apart past a few slices.


A reasonable rule: a pie chart is acceptable only for two or three categories where one clearly dominates, and even then a single stacked bar usually reads better. For composition that changes over time, use a stacked area chart or a set of 100% stacked bars, one per period. For a hierarchy (categories within categories), a treemap handles more slices than a pie without becoming unreadable.


## Showing distribution: histograms and box plots


If the question is “what does the spread look like?” (order values, response times, deal sizes), you need a distribution chart, not an average. A histogram buckets the values and shows their shape: is it skewed, bimodal, full of outliers? A box plot compresses that into quartiles and is better when you want to compare distributions across several groups side by side.


This is the category people skip most often, and it hides real problems. An average order value of $80 looks healthy until a histogram reveals it is a pile of $20 orders plus a handful of $2,000 ones.


## Showing relationships: scatter plots


To see whether two numeric variables move together (marketing spend vs. sign-ups, account age vs. usage), use a scatter plot. Each point is one record, so patterns, clusters, and outliers become visible in a way no aggregate can show. Add a third dimension with bubble size or color if needed, but stop there; a fourth encoding usually creates more confusion than insight.


## Showing a single number: KPI cards


Not everything needs a chart. When the reader just needs the current value of one metric (MRR, active users, conversion rate), a big-number KPI card is the clearest option. Pair the number with a small comparison (vs. last period, vs. target) and optionally a sparkline for recent trend. Avoid gauges and speedometer visuals; they use a lot of space to show one number less precisely than plain text.


## Bar vs. line: the confusion worth settling


These two cover most dashboards and get mixed up constantly. The distinction is simple:


- Use a **bar chart** to compare distinct categories, even if one of those categories happens to be a date bucket you are treating as a label.
- Use a **line chart** when the x-axis is continuous and the connection between points is meaningful, so the slope tells a story.


If it would be nonsense to draw a line between two adjacent points (revenue for “Sales” and revenue for “Marketing”), use bars. If the line between points means something (revenue in January to revenue in February), use a line.


## When a table beats every chart


Charts are for spotting patterns. Tables are for looking up and comparing exact values. Use a table when:


- The reader needs precise numbers, not a shape (a finance reconciliation, a list of top accounts with several attributes each).
- You have many metrics per row and no single comparison dominates.
- The data is naturally a list people scan and sort.


A well-formatted table with right-aligned numbers, subtle row separation, and one or two inline indicators (a small bar or an up/down arrow) often communicates more than a busy chart. Do not force a visualization onto data that is really a lookup.


## Dashboard-specific rules generic chart guides miss


Choosing a good chart in isolation is not enough. On a dashboard, charts compete for attention, so a few extra rules apply.


- **Limit chart variety.** A dashboard with a bar, a line, two KPI cards, and a table reads cleanly. One with eight different chart types reads like a demo of your BI tool. Repetition helps scanning.
- **Use small multiples for many series.** Instead of one chart with ten lines, show ten small charts on a shared scale. The eye compares shapes far faster than it untangles overlapping lines.
- **Keep color meaningful.** Reserve color for encoding data (a status, a segment), not decoration. Use one accent color and shades of a neutral for everything else. A rainbow palette signals that color is doing no work.
- **Label directly when you can.** Put the series name next to the line instead of forcing a round trip to a legend. Fewer legends, faster reading.
- **Avoid dual y-axes.** Two different scales on one chart invites false conclusions because the reader can make the lines cross wherever the axis ranges are set. Use two stacked charts sharing an x-axis instead.
- **Sort and cap.** Sort bars by value and cap long lists with a “top 10 plus other” grouping rather than showing 40 tiny bars.


For more on assembling these into a layout that people actually act on, see[how to build dashboards that drive decisions](https://www.basedash.com/blog/how-to-build-dashboards-that-drive-decisions-a-practical-guide) and[how to build an executive dashboard](https://www.basedash.com/blog/how-to-build-an-executive-dashboard-metrics-layout-and-cadence) .


## Common mistakes


- **Pie charts with more than a few slices.** Once there are five or more wedges, no one can rank them. Use a sorted bar chart.
- **Truncated bar axes.** Starting a bar chart’s value axis above zero makes a 3% difference look like a 3x difference.
- **Dual axes to imply correlation.** Overlaying two metrics on separate scales manufactures relationships that may not exist.
- **3D charts.** Depth adds distortion and zero information. Never use 3D bars or pies.
- **Too many series on one chart.** More than four or five lines or stacked segments and the chart stops communicating. Split it.
- **Charting an average that hides a distribution.** If the spread matters, show the spread, not just the mean.


## A quick chart-selection checklist


Before you commit to a chart, run through this:


1. Can I finish the sentence “the viewer looks at this to answer ___”? If not, the chart has no job yet.
2. Does the chart family match that question in the table above?
3. Is the value axis starting at zero (for bars and areas)?
4. Are categories sorted by value or a natural order?
5. Are there five or fewer series? If not, can I use small multiples?
6. Is color encoding data, or just decorating?
7. Would a table actually serve the reader better here?


If a chart passes all seven, it will do its job on almost any dashboard.


## Where tooling fits


Modern BI tools increasingly pick the chart for you. Ask a question in natural language and the tool infers the chart type from the shape of the result. Basedash works this way: it generates a chart from your query or prompt and lets you adjust it, so the default is usually a sensible bar, line, or KPI card rather than a blank canvas. That removes the busywork, but it does not remove the judgment. Knowing why a bar beats a pie, or when a distribution matters more than an average, is what lets you correct the automatic choice when it is wrong. For a deeper look at how that automation works, see[how AI automates data visualization](https://www.basedash.com/blog/how-ai-automates-data-visualization-from-raw-data-to-the-right-chart) , and for a survey of tools, see the[best AI data visualization tools compared](https://www.basedash.com/blog/best-ai-data-visualization-tools-compared-2026) .


If you want a reference to keep handy while building, the[Financial Times Visual Vocabulary](https://github.com/Financial-Times/chart-doctor/tree/main/visual-vocabulary) and the[Data to Viz](https://www.data-to-viz.com/) decision tree are two well-regarded, freely available guides that organize charts by analytical intent.


## FAQ


**What chart should I use for data over time?**


A line chart is the default for a continuous time axis, because the slope communicates trend and rate of change. Use bars only when you have a small number of discrete periods (revenue by quarter for two years). Use an area chart when cumulative magnitude matters or when stacking a few series to show composition over time.


**When is a pie chart okay?**


Rarely. A pie is acceptable only for two or three categories where one clearly dominates and precise comparison does not matter. For anything more, a sorted bar chart or a single stacked bar communicates the same thing more accurately, because people judge length better than angle.


**What is the difference between a bar chart and a line chart?**


Use a bar chart to compare distinct categories, where the gaps between bars are meaningful. Use a line chart when the x-axis is continuous and the connection between points carries meaning, so the line’s slope tells a story. If drawing a line between two adjacent points would be nonsense, use bars.


**How many charts should a dashboard have?**


Fewer than you think. Most effective dashboards show three to seven visuals that answer a specific set of questions, with limited chart variety so the layout is scannable. If a dashboard has grown past that, it is usually trying to serve two audiences and should be split.


**When should I use a table instead of a chart?**


Use a table when the reader needs exact values rather than a pattern, when there are many attributes per row, or when the data is a list people scan and sort. Tables are for lookup and precise comparison; charts are for spotting shape and trend.


**How do I show a distribution rather than an average?**


Use a histogram to see the shape of a single variable’s spread, or a box plot to compare spreads across several groups. Averages hide skew and outliers, so any time the spread affects a decision, show the distribution directly.
