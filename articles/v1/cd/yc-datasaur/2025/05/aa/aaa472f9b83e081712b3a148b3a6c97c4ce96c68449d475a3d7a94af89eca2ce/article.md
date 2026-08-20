---
schema_version: "1.0.0"
document_id: "aaa472f9b83e081712b3a148b3a6c97c4ce96c68449d475a3d7a94af89eca2ce"
company_key: "yc-datasaur"
company: "Datasaur"
source_id: "yc-datasaur-news-import-f7082d4871d2"
canonical_url: "https://datasaur.ai/blog-posts/efficient-labeling-and-review-the-search-extension"
published_at: "2025-05-08T00:00:00+00:00"
first_seen_at: "2026-07-25T01:27:49.393704+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:21840ef241a39b87db4bd90735ce1bb50cebbf8d5bb756989ce0e831069dd2bd"
---

# Efficient Labeling and Review: The Search Extension

Datasaur's enhanced Search extension gives data labeling teams the ability to instantly surface the information they need—when they need it. With powerful filtering, advanced logic, and bulk actions, the feature improves both the speed and accuracy of annotation work from the ground up.


Data labeling is often slowed by the simplest challenge: locating the right piece of information at the right time: We solved that problem. Using the Search extension, labelers and reviewers can instantly view and label relevant text, reducing the time spent navigating extensive documents.


## Advanced Search


The Search extension has an Advanced Search function which further refines these capabilities by allowing complex queries that combine multiple criteria: a logic builder. With the Logic Builder, users can visually construct sophisticated queries without requiring technical knowledge. In the example below, a reviewer identified in the beginning of their review that their labeling team did not label CNN correctly as an ORG. Therefore, they opened up the Logic Builder and searched for all instances of CNN where ORG was not applied. This targeted approach enhances precision, enabling users to effectively manage sophisticated labeling tasks.


Also, note how intuitive the interface is below; we have designed this capability with the knowledge that many reviewers are not technical users. They can be lawyers, doctors, etc – subject matter experts should have access to advanced features without requiring the help of data scientists.


## Viewing the Results


So how do the results appear? The Search extension will provide a list immediately of all the results but you can also choose to filter your data by those results. Our latest update introduces automatic filtering based on search results. With this enhancement, the Search extension can now filter and display only the lines of text that match your search criteria. This targeted view helps reviewers and annotators efficiently focus on pertinent data, accelerating the review and annotation process.


It’s not just text you can search for, as reviewers can search based on labels applied by the labeling team. Consider the case of a reviewer who notices that the "Plaintiff" label has been misapplied several times early in their QA session. Rather than manually combing through the full dataset, the reviewer can use the Search extension to instantly retrieve all instances where the "Plaintiff" label was used. This allows them to quickly assess the accuracy of those annotations and concentrate their QA efforts on that specific portion of the data, improving both efficiency and precision.


## Bulk Labeling


But the search extension also supports labelers ability to efficiently apply labels. The extension supports bulk labeling actions directly from search results, allowing users to efficiently annotate multiple data points simultaneously. The best part of this function is you are able to label across all of your documents, not just the document you are currently viewing. Create your criteria, activate the search, and then select “label all” instances that were the results of the function. You can imagine how beneficial the time-saved is if you have a project with 500+ documents.


Bulk labeling capability not only saves valuable time but also ensures consistency across the dataset: speed and quality assured.


By incorporating these search enhancements, Datasaur continues to improve labeling efficiency and accuracy, supporting streamlined workflows for annotation teams and QA teams.
