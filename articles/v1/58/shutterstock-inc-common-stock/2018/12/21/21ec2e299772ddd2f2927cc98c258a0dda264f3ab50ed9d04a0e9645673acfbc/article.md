---
schema_version: "1.0.0"
document_id: "21ec2e299772ddd2f2927cc98c258a0dda264f3ab50ed9d04a0e9645673acfbc"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2018/12/11/opentree-visualizing-organizations"
published_at: "2018-12-11T05:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:6cd9a20b9edb78b06b8cd34701a638a955bc7840e55c257067f0de6cfc166540"
---

# OpenTree: Visualizing Organizations

*tl;dr:* Does your company use Workday? Ever wish you could see your company’s entire org chart in a single view? You can use[OpenTree](https://github.com/shutterstock/opentree) to fill this gap.


## Background


We use[Workday](https://workday.com/) at Shutterstock and it serves as one of the key sources of employee data within our company. Though employees are able to view their immediate reporting hierarchies (i.e. to whom they report and who, if any, reports into them) and can navigate up and down the hierarchy one level at a time, there is no way to visualize their entire organization in a single, all-inclusive view.


We, however, wanted a tool that allowed us to do exactly that—to view the entire company in a single glance while also being able to drill down as needed and to filter based on location, title and/or department. At the same time, we didn’t want to fuss with a store of employee data that had to be managed independently of our employee data source, Workday.


Fortunately, Workday provides the ability to define API endpoints that return preconfigured reports in various formats, so we’re able to utilize that functionality to extract the data we need for purposes of building our own org chart.


## OpenTree Project


Last week, we released an open source, Java-based project called[OpenTree](https://github.com/shutterstock/opentree) , derived from an internal application we developed, and currently use, at Shutterstock, that leverages a Workday reporting endpoint to generate an organizational chart from flat, CSV data. Provided the necessary data in a Workday report to link rows (e.g. parent/manager id), OpenTree handles the construction of the hierarchy and displays it on a web front-end.


This is the sample data set provided with the project, which defines reporting relationships through ID mappings:


NB: each employee must be unique (e.g. there can only be one employee identified by a given ID) for the data structure to be properly constructed. In other words, matrix (non-hierarchical) reporting lines are not currently supported.


The following is the output:


---


Feel free to[report issues](https://github.com/shutterstock/opentree/issues/new) , contribute enhancements to the project or let us know in the comments below how you’re using this in the wild.
