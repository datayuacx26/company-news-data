---
schema_version: "1.0.0"
document_id: "2eced581747d98779050ffbec869f3b0ea4ffa4ed07203419efe1d9dee0a3b9c"
company_key: "yc-sre-ai"
company: "SRE.ai"
source_id: "yc-sre-ai-news-import-a9b2aa0ab1a6"
canonical_url: "https://www.sre.ai/post/the-difference-between-salesforce-metadata-vs-data-deployments"
published_at: "2025-08-18T00:00:00+00:00"
first_seen_at: "2026-07-24T02:05:06.902612+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:c18cf41e69719f0617c360340535ccef5579906c47febee1fbb1a55c358811db"
---

# The Difference Between Salesforce Metadata vs. Data Deployments

Every Salesforce developer has been there: you build something beautiful in your development environment, everything works perfectly, and then you try to move it to production and... it doesn't work the same way. Sometimes it's a missing custom setting. Sometimes it's a field that exists in dev but not in prod. Sometimes it's more subtle. The functionality is there, but the business rules aren't quite right.


Understanding the difference between metadata and data deployments is crucial for any team working with Salesforce, especially as your implementation grows more sophisticated.


## **Metadata: The Blueprint**


Metadata is the structural layer of your Salesforce org. It defines what's possible: the objects, fields, classes, and configurations that determine how your org behaves.


public class LeadProcessor {


public static void assignLeadToQueue(List<Lead> leads) {


Group salesQueue = \[SELECT Id FROM Group WHERE Name = 'Sales Queue' LIMIT 1\];


for (Lead lead : leads) {


if (lead.Annual_Revenue__c > 1000000) {


lead.OwnerId = salesQueue.Id;


}


}


update leads;


}


}


This Apex class is metadata. It defines the logic for how leads should be processed, but it doesn't contain any actual lead records. When you deploy this class using a change set or the Metadata API, you're deploying the capability to process leads in a specific way.


## **Data: The Content**


Data is what fills your metadata structures. It's the actual records, values, and configurations that make your org work for your specific business needs.


Consider custom settings. A perfect example of where the line between metadata and data gets interesting:


// The custom setting definition is metadata


public class ConfigurationManager {


public static Decimal getTaxRate() {


Tax_Configuration__c config = Tax_Configuration__c.getOrgDefaults();


return config.Default_Tax_Rate__c;


}


}


The custom setting object Tax_Configuration__c and its fields are metadata. But the actual tax rate value stored in that setting? That's data. And here's the key point: **change sets and the Metadata API deploy the structure but not the values.**


## **Where This Matters in Practice**


Let me share a scenario that illustrates why this distinction is crucial:


A financial services company was implementing a complex approval process for loan applications. They had:


- Custom objects for loan applications (metadata)
- Approval processes with specific criteria (metadata)
- Custom settings that defined risk thresholds (metadata structure, data values)
- Permission sets for different user roles (metadata)
- Queue assignments for different loan types (data)


Their deployment looked perfect in the sandbox. Loan applications flowed through approval processes, risk calculations worked correctly, and users had appropriate access. But when they deployed to production, applications got stuck in the wrong queues, risk calculations used default values instead of their carefully configured thresholds, and the approval process didn't behave as expected.


The metadata deployment had succeeded completely. All the objects, fields, and processes were in place. But the data that made those processes work correctly for their business hadn't moved over.


## **The Configuration Data Challenge**


This gets particularly complex when you're working with configuration-heavy implementations. Consider these scenarios:


**Product Catalogs** Your Product2 objects and custom fields are metadata, but the actual products, their pricing, and their relationships are data. In B2B implementations, this product configuration often represents months of business analysis and setup.


**Territory Management** The territory model structure is metadata, but the actual territory assignments and rules are data. Miss this in a deployment, and your sales team suddenly can't see their accounts.


**Business Process Configurations** Your picklist values for opportunity stages are metadata, but the specific probability percentages and business logic that drive your sales process might be stored in custom settings or custom metadata types.


Here's what a territory-aware opportunity query might look like:


public class OpportunityTerritoryService {


public static List<Opportunity> getTerritoriesOpportunities(Id userId) {


// This works if territory assignments (data) are properly configured


User currentUser = \[SELECT Id, Territory__c FROM User WHERE Id = :userId\];


return \[SELECT Id, Name, Amount, Territory__c


FROM Opportunity


WHERE Territory__c = :currentUser.Territory__c


AND StageName != 'Closed Won'\];


}


}


This code is metadata, but it depends on territory data being properly configured for each user. Deploy the code without the data, and the query returns empty results.


## **Real-World Impact: The Partial Deployment Problem**


I worked with a manufacturing company that had a sophisticated CPQ implementation. Their deployment process looked like this:


1. Export metadata changes (objects, fields, Apex classes)
2. Deploy to production using change sets
3. Manually configure price books and product rules in production
4. Test and validate


This worked for small changes, but became unsustainable as their product catalog grew more complex. They had hundreds of products with intricate pricing rules, bundling logic, and discount structures. Each deployment required hours of manual configuration in production to recreate the data relationships that made their pricing work correctly.


The breaking point came during a major release that included both new CPQ functionality and significant product catalog changes. The metadata deployment succeeded, but recreating the data configuration took so long that they had to roll back the entire release.


## **Modern Solutions: Custom Metadata Types**


Salesforce introduced Custom Metadata Types partly to address this challenge. Unlike custom settings, custom metadata types can be deployed through the Metadata API:


public class PricingService {


public static Decimal calculateDiscount(String productCategory, Decimal orderAmount) {


// Custom metadata types are deployable configuration


Pricing_Rule__mdt rule = \[SELECT Discount_Percentage__c


FROM Pricing_Rule__mdt


WHERE Product_Category__c = :productCategory


AND Min_Order_Amount__c <= :orderAmount


LIMIT 1\];


return rule != null ? rule.Discount_Percentage__c : 0;


}


}


This approach lets you deploy business configuration alongside your code, but it requires planning and often means refactoring existing implementations that rely on custom settings or data records.


## **The Integration Data Dilemma**


Modern Salesforce implementations rarely exist in isolation. They integrate with ERPs, marketing platforms, financial systems, and data warehouses. These integrations often depend on reference data—lookup values, mapping tables, and configuration records that need to be consistent across environments.


Consider an integration with an external order management system:


public class OrderSyncService {


@future(callout=true)


public static void syncOrderToERP(Id opportunityId) {


Opportunity opp = \[SELECT Id, AccountId, Amount, Product_Category__c


FROM Opportunity WHERE Id = :opportunityId\];


// Lookup ERP mapping - this is data, not metadata


ERP_Mapping__c mapping = \[SELECT ERP_Category_Code__c


FROM ERP_Mapping__c


WHERE Salesforce_Category__c = :opp.Product_Category__c\];


// Make callout with mapped values


ERPService.createOrder(opp, mapping.ERP_Category_Code__c);


}


}


The Apex class deploys fine, but if the ERP mapping records aren't in your target environment, the integration breaks. These mapping tables often contain dozens or hundreds of records that represent critical business knowledge.


## **How SRE.ai Addresses the Metadata vs. Data Challenge**


At SRE.ai, we've designed our platform to handle both metadata and data as part of unified deployment processes. Here's how we approach this challenge:


**Intelligent Change Detection** Our system identifies both metadata changes (new classes, modified fields) and data changes (updated custom settings, new configuration records) in a single view. Teams can see the complete picture of what's changing in their deployment.


**Configuration-Aware Collections** When you create a Collection in SRE.ai, you can include both metadata components and data records that support your feature. For example, a new pricing feature might include:


- Apex classes (metadata)
- Custom objects and fields (metadata)
- Price book entries (data)
- Custom setting values (data)


**Data Migration Workflows** Our Flows feature includes steps specifically designed for data migration and configuration deployment. You can create automated workflows that deploy your metadata and then immediately populate the required data configurations.


**Environment Synchronization** We provide tools to compare data configurations across environments, helping teams identify when reference data or configuration records are out of sync.


The result is that teams can treat their entire Salesforce configuration (both metadata and data) as a cohesive deployment unit, rather than managing separate processes for different types of changes.


‍
