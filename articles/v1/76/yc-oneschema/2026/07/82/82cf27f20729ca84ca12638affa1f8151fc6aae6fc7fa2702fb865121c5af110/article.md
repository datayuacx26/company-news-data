---
schema_version: "1.0.0"
document_id: "82cf27f20729ca84ca12638affa1f8151fc6aae6fc7fa2702fb865121c5af110"
company_key: "yc-oneschema"
company: "OneSchema"
source_id: "yc-oneschema-news-import-43da02420b1d"
canonical_url: "https://www.oneschema.co/blog/mobile-recell"
published_at: null
first_seen_at: "2026-07-24T07:26:45.232367+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:b8e8fcb610d5d26fb9eb061c978f49183c82af0f25ae61ab3dba6443eee8ba10"
---

# 3 Hours to 3 Minutes: How Mobile reCell Is Importing Customer Data 60x Faster

## Background & Use Case


Mobile reCell is the pioneer of software-driven recovery for corporate-owned IT assets, such as laptops, tablets, and smartphones from employees. Using Mobile reCell’s platform, customers—like a leading US airline—can initiate workflows like replacing the iPads in every cockpit of their fleet.


Mobile reCell’s enterprise customers manage the hardware recovery of tens of thousands of devices. On a daily basis, each of their customers initiates hundreds of workflows to request employee device returns, send return kits or QR codes, and more through their platform.


‍


## Problem


As the Mobile reCell team was migrating from their legacy platform to their new software platform, they began experiencing increased challenges with their CSV import processes. In their old software platform, the backend had fewer validations and was more flexible. In their newer platform, they upgraded their database architecture in a way that required the customer’s data to be much cleaner. “Data imports were a nightmare,” says Chris Berry, VP of Customer Experience at Mobile reCell. “During a large refresh project, my team of four was processing 30 to 40 customer data files per week, with each file taking up to three hours to prepare for upload to the platform.”


> *“Before OneSchema, manually uploading customer data was an absolute nightmare. Now, I can upload thousands of records in under three minutes.”*


The problem wasn’t just painful for the customer experience team. The engineering team was also troubleshooting on a daily basis.


Berry shared, “It was just as miserable as it sounds. I would send customer data files to our engineering team, who would attempt to upload the CSV—only to have it fail. Failures were the result of any number of error types: misformatted dates, missing fields, duplicate entries, and more. The engineering team could typically identify one of the reasons the file was failing, but my team would then have to chase down the issue and reformat the data. It commonly took several back-and-forth exchanges with the engineering team before we had file upload success. In some cases, this was happening every single day. We had no validation until the data hit the database, where the upload would break and we’d do the file all over again. An extra space in a column header would cause the entire upload to fail.”


> *“An extra space in a column header would cause the entire upload to fail.”*


The Mobile reCell team first investigated whether building an internal tool to assist with the imports made sense, but they quickly ran into issues. “We stopped even thinking about building an internal tool once we realized how many possible error states existed,” recalls Berry. “We had multiple hour-long conversations just trying to figure out all the possible error states. It was exhausting to even think about building a system that would identify and address all of the possible error cases.”


> *“We didn’t even want to think about building it ourselves once we realized how many possible error states existed.”*


‍


## **Solution & Results**


“Our lucky day was when our VP of Marketing, I don’t even know how, stumbled upon OneSchema,” said Berry. “We started testing it out during the self-serve experience and thought, ‘Wow, this could actually solve this extremely painful problem.’” The Mobile reCell team moved quickly to purchase and implement OneSchema.


The team rolled out OneSchema on four key workflows: generating device return requests, adding device processing detail, adding external tracking numbers, and updating financial settlements.


“With OneSchema, cleaning a file takes me three minutes,” said Berry. “Can you believe it? From three hours down to three minutes. I was completely blown away.” OneSchema is able to determine a fix to the issue and highlights data requiring a closer look. “It’s so intuitive and easy to use. I love the smart suggestions. If there’s a dropdown item with an invalid input, OneSchema suggests changing all of the rows with the invalid value to the correct valid value, which I can do with one click.”


The Mobile reCell engineering team was able to fine-tune its implementation of OneSchema to exactly meet their needs. They configured validations to focus on important data points requiring validation in a very specific way, while still allowing the import of other customer-related data that required much less validation.


OneSchema was also a breeze to set up. By leveraging OneSchema’s pre-built validation library, the Mobile reCell team was set up in a couple of weeks, with minimal development resources.


‍


{{blog-content-cta}}


‍


## **Expansion**


Since their internal rollout has driven a massive impact, Mobile reCell wants their customers to be able to upload files themselves. “A lot of our customers have been asking to upload files themselves,” said Berry. “I’m very excited to launch OneSchema for them.”


Berry’s team also often receives files to an SFTP endpoint and is interested in leveraging[OneSchema Pipelines](https://www.oneschema.co/pipelines) to streamline their recurring workflows. “I’m excited to refresh our IT budget so we can keep expanding OneSchema!”
