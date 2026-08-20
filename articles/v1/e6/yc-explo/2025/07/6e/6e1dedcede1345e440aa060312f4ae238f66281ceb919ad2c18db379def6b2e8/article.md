---
schema_version: "1.0.0"
document_id: "6e1dedcede1345e440aa060312f4ae238f66281ceb919ad2c18db379def6b2e8"
company_key: "yc-explo"
company: "Explo"
source_id: "yc-explo-news-import-bdc97e193306"
canonical_url: "https://www.explo.co/blog/introducing-global-datasets"
published_at: "2025-07-08T00:00:00+00:00"
first_seen_at: "2026-07-23T09:10:29.629224+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:7584d7519ec1dc97a04db52290c73589823f1c608913057853586cc31a495f5b"
---

# Introducing Global Datasets

At Explo, our mission is to simplify how organizations create and embed customer-facing dashboards directly into their products and applications. Our drag-and-drop UI has enabled users to define datasets using only SQL and turn them into visualizations for their customers.


Now, we are excited to make Explo even more powerful by launching **Global Datasets** , a part of our data modeling layer that’s designed to make the dashboard creation and data definition management more efficient, scalable, and collaborative.


**Note: Global Datasets are currently in Early Release. If interested in using them, please reach out to support@explo.co.**


### What is ‘Global Datasets’?


Global Datasets enables teams to define datasets once and reuse them across Dashboards and Report Builders. Think of it as a data modeling layer – to ensure consistency across all Dashboards and Report Builders. This eliminates potential errors while maintaining duplicate datasets.


‍


### Why Global Datasets?


Here’s why teams are excited about Global Datasets:


- **Consistency:** You can define a dataset once and use it across multiple dashboards. This creates a centralized source of truth to minimize discrepancies so all dashboards align on definitions, logic, and numbers,
- **Collaboration:** By sharing datasets in the **Data Library** , everyone on your team can access it – and teams never have to wonder why dashboards have different numbers.
- **Version Control:** With the commits function, you can track changes, document updates, and confirm the changes before publishing for organization-wide use.


Global Datasets fosters DRY principle ("don't repeat yourself"): Write SQL once, in one place, and use it repeatedly across dashboards. This lets you focus on creating insightful charts, not on rewriting or debugging SQL logic.


### Getting Started with Global Datasets


Here’s a few steps to start using Global Datasets


‍ **Step 1: Go to the Data Library**


The Data Library is the central repository for all Global Datasets in your organization. Access the datasets that were created by your colleagues or organize them into folders by domains, themes, use-cases, or customers.


**Step 2: Create a Dataset**


Give the dataset a meaningful name and assign it to a destination schema. This ensures it is easily recognizable.


**Step 3: Define the Dataset**


Write the SQL logic for your dataset and define fields, conditions, and business logic.


**Step 4: Commit Your Changes**


Once you’ve written or updated the SQL, click the “Create Commit” button to save your changes. You will now document your version history and ensure every version update is traceable.


**Step 5: Import Global Datasets into a Dashboard**


You are now ready to build! You can use both Global Datasets and dashboard-specific “local” datasets to build dashboards. Navigate to the Datasets tab in the dashboard creation page to search and import the dataset(s) you need from the Data Library.


### Tips for Getting the Most Out of Global Datasets


- **Update Datasets** . Building reusable datasets is an iterative process – when SQL logic changes, visit the dashboards using the dataset and remember to update the version to reflect the most recent updates in the dataset.
- **Promote Local Datasets.** Your previous datasets – now called “Local Datasets – are still available within Dashboards and Report Builders. These datasets are specific to dashboards that allow users to quickly iterate within the dashboard builder. Once these datasets are ready to be used in other dashboards, you can “promote” it to a Global Dataset to enable other team members to use them.
- **Commit Messages Matter.** Always be sure to document any minor or major changes when saving a new version of code. This simple habit can save countless hours of potential confusion and enhance team collaboration.


### What’s Next


In addition to powering Dashboards and Report Builder, Global Datasets paves the way for Explo to facilitate additional methods for sharing data with your customer. Over the next 6 months, we’ll be launching products that leverage Global Datasets to further enable data sharing.


‍


### FAQ


**Global Datasets are currently in Early Release. What does Early Release mean?**


Global Datasets is available to be used in production environments, but you need to reach out tosupport@explo.co in order to have it enabled for your account.


**Which datasets should I expect to see in the Data Library?**


You’ll only see Global Datasets in your Data Library. Local Datasets will continue to be visible in the Dashboard and Report Builder views.


**Do Global Datasets work with Report Builder too?**


Today, Global Datasets are only available for use with Dashboards.


**How much do Global Datasets cost?**


There is no additional charge for using Global Datasets, just as there is no additional charge when creating Local datasets.


‍
