---
schema_version: "1.0.0"
document_id: "8ddaa7daadb8a7f4554f3a827c8c0c9d7b96a0045a5b0ffd255dbc7d30869734"
company_key: "yc-redbrick-ai"
company: "RedBrick AI"
source_id: "yc-redbrick-ai-news-import-4b943afcd495"
canonical_url: "https://www.redbrickai.com/blog/2023-12-15-update"
published_at: "2023-12-15T00:00:00+00:00"
first_seen_at: "2026-07-24T11:23:16.380780+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:80a91e7a0d4ba8571fa5c67746cb972d81eb8c37838204dcea7c637c52b8826b"
---

# Select balanced dataset with cohort creation, and new annotation tools!

# Select balanced dataset with cohort creation, and new annotation tools!


We're thrilled to announce the Beta release of "Cohort Creation," a toolset that provides our customers with extensive search and filter capabilities for their DICOM datasets. This enables the selection of high-quality, balanced datasets for annotation.


Many radiology AI teams have told us they spend a significant amount of time writing scripts to sift through large DICOM datasets. They need to find data that meets certain criteria for annotation. Our aim with cohort creation is to provide an intuitive user interface to streamline this process.


To try out the product, please schedule a[time for us to chat here](https://calendly.com/shivam-redbrick/30-minute-chat) .


### View high level summaries of your dataset


Import large datasets, along with DICOM tags and other metadata, conveniently summarized in a single table. This allows you to easily visualize the distribution of each tag and metadata field within your dataset.


### Filter and search through your dataset


Metadata fields and DICOM tags contain valuable information that aids in the decision-making process for including or excluding specific data points from your annotation projects and training datasets. Utilizing our search and filtering capabilities, you can effectively manage your dataset to include only the images relevant to your project.


### Quickly tag your images and organize them into cohorts


Sometimes, it's crucial to enrich your dataset with custom fields, such as image quality or artifact presence. You can create a custom schema for your dataset and then add classifications to each data row.


Furthermore, you can organize your enriched datasets into cohorts to systematically start using them in your annotation projects.


## Improved loading experience with Lazy Loading


For DICOM tasks, we will adopt a lazy loading approach for images. Initially, we will load only the meta-data, then progressively load the pixel data. This method reduces the "first-paint" time, enabling quicker interaction and annotation on your images.


## Adaptive Brush


We've introduced a new addition to our segmentation toolset — the adaptive brush! As the name suggests, this brush isn't circular but adjusts to the boundaries of the structures you're annotating. Lowering the "adhesion" will make it less "sticky" to the object's boundaries you're segmenting. Give it a try and share your feedback!


## Project Manager Role


We've introduced a new role within projects — the Project Manager. This role bridges the gap between Project Members (labelers and reviewers) and Project Admins (who have the ability to manage all aspects of a project).


Specifically, Managers can view, annotate, and participate in the "management" aspects of projects, such as viewing project progress and assigning tasks to users. However, they cannot modify project settings, add data, or delete tasks.
