---
schema_version: "1.0.0"
document_id: "d30dad403c64d93a9e1cffdd1a3cf5b4000777348b47725dee40d15a1775cc26"
company_key: "zoominfo-technologies-inc-common-stock"
company: "ZoomInfo Technologies Inc"
source_id: "zoominfo-technologies-inc-common-stock-rss-ef9b8d7bcd4f"
canonical_url: "https://engineering.zoominfo.com/analyzing-data"
published_at: "2024-12-06T18:16:32+00:00"
first_seen_at: "2026-07-20T23:24:42.992125+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:e3eb9493d8754c00d1853343c7dedda9f07c74b40ec85175c86b85d97496240c"
---

# Analyzing Data

Managing customer and prospect data can be challenging due to large volumes of information from multiple sources. Maintaining and updating data for each individual becomes more complicated once there is the added factor of numerous accounts and contacts. My internship project this summer dealt with monitoring the health of our CRM Contacts to allow for a better user experience for our customers.


Our Sales Leadership team wrote a detailed article called[Analyze: Assessing Your CRM Data for Actionable Insights](https://pipeline.zoominfo.com/operations/analyze-crm-hygiene) where the discussion focused on keeping company CRM data hygienic and broke down the process of doing so into five different stages: Define, Analyze, Purge, Enhance, and Maintain. My project focused on the Analyze stage in order to assess the data issues and identify the changes that need to be made in the future.


The Analyze stage is essential to ensure any crucial changes made to data within the Purge, Enhance, and Maintain stages are truly necessary and will not have a negative impact on the state of the data later on. Completeness, Uniqueness, and Accuracy are the three data governance standards that are utilized during the Analyze phase to determine which data needs to be purged fully or enhanced. Quality of data plays a crucial role in a user’s experience and can inhibit a company’s plans for future development.


### **Completeness:**


In order to maintain relevant data, a Contact needs to have numerous fields populated before a Seller can communicate with them. If information fields are left blank (Ex: email, phone number, etc) there is a new inhibitor in the communication between Sellers and Customers. In general, marketing emails cannot be sent to Customers, Sellers cannot communicate with contact key purchasers, and company teams struggle to enrich employees with the right data.


To prevent any issues regarding communication between both parties, companies need to define key attributes and ensure availability. However, before jumping in, it is important to verify that the data being analyzed is relevant and still in use otherwise the analysis loses its significance.


As there may be a large number of contacts that are missing data, it is crucial to sort these contacts by priority such as their job title. Being able to identify contacts that have a buying influence elevates the importance of these contacts, so we know they need more attention. ZoomInfo provides the ability to tag contacts as key Buying Committee members, which allows for more thorough prioritization. By performing an analysis of what needs to be rectified for all contacts in general, it helps identify common areas for application and process improvements. Potential action plans and solutions are enriching the contact data, partnering with other sellers to improve or limit manual processes, and comparing relevant data to link it to the missing contact field.


### **Uniqueness:**


At first glance, it may seem like there is an overwhelming amount of relevant data within the system, but in addition to incomplete information, there is a risk of having duplicate contacts. A duplicate contact can occur when there are two contacts for one person in either the same account or different accounts. This may seem harmless at first, but it skews the data and influences the ability to perform the correct analysis. Not to mention it affects the customer’s experience as well because now the Sales and Marketing teams can contact a person multiple times, which can cause frustration and give an overall negative perspective of the company.


The complications that occur due to duplicate contacts can be reduced by identifying how many are currently active. Then to clean the company’s data, you would need to figure out a method to merge duplicate contacts and decide which Contact has the most accurate information such as a Surviving Contact and should be kept.[RingLead](https://help.zoominfo.com/s/article/Overview-of-RingLead) allowed us to define duplicate contacts and merge the contacts using our own survivorship logic.


### **Accuracy:**


Contact data accuracy is essential to ensure potential customers have a positive experience with a seller. If a seller has outdated or incorrect information, they may contact people that are no longer at a company, they may contact the wrong decision maker, or potentially make no contact as the phone/email are unavailable. This not only frustrates the customers but also wastes the seller’s time as they are unable to reach the correct audience. Therefore, you want to ensure contacts are under the correct correlating account and are actively being updated. To verify the accuracy of the contact and account relationship, compare the email domains of the contact with the Account website domain to ensure they match or not. In some cases, the account website can be different from the email domains, so we derived an Account Majority Email Domain value.


A majority email domain is the most common email domain in an Account. The account’s majority email domain can be identified by computing the number of each type of email domain and using the highest domain count. Since the Account’s majority email domain is likely accurate, you could also compare the contacts’ email domains to the Account Majority email domain and Account Website. Non-matching domain contacts should be reviewed and rectified.


A whitelisted domain list was created for each account to identify valid email domains for that account. This helps to provide consistency and allows for improved automation.


### **Moving Forward:**


The Analyze stage is necessary to figure out the size and scope of the problems in your customer data and to ensure that you have a better understanding of where to start handling the data quality issues. By identifying the areas most affected and prioritizing them accordingly, you can create an action plan for each data governance standard. The plan will help make decisions regarding which contacts need to be purged fully or just enhanced. Ensuring that your data is cleaner and accurate will enhance the overall customer experience and prevent strategic plans from being negatively affected.
