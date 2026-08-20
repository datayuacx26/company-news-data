---
schema_version: "1.0.0"
document_id: "8888a81158f294badef56b22897825904a0e22e189ef96a776bcbc0c961030f2"
company_key: "upland-software-inc-common-stock"
company: "Upland Software Inc."
source_id: "upland-software-inc-common-stock-rss-53a5709ba49e"
canonical_url: "https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2024-2/"
published_at: "2024-10-29T12:55:46+00:00"
first_seen_at: "2026-07-26T13:25:40.115128+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:109ad4ac7694df9362d236f5277f07b807170d9009aff84f74dce0210050d3f5"
---

# What’s new in OL Connect 2024.2

The OL Connect 2024.2 release brings significant enhancements to the OL Connect toolset, including new features, fixes, and updates to the OL Connect REST API. A key improvement in this release focuses on section cloning, a powerful technique used to replicate print sections multiple times, with each instance potentially featuring unique personalization or distinct background images. These updates further streamline and enhance the flexibility of the document creation process, making OL Connect even more efficient and versatile.


## Section Cloning enhancements


OL Connect 2024.2 introduces design-time preview of section clones and their personalization during the design phase, eliminating the need to generate output for each iteration. This feature streamlines the workflow, significantly saving time and enhancing efficiency.


Typical use cases for section cloning are: Insurance policies, Loan statements, Under-copies, Picking lists with multiple boxes/pallets, Textbooks and Course material, Exam information, Postal payment slips, Box and Shipping labels.


A new wizard simplifies the creation of print section clones from detail records, reducing the need for scripting. It also allows for specifying unique background images for each clone by providing the image path in a detail record field.


The new wizard automatically stores detail record table and index information with each clone, replacing the previous manual process. Enhanced functions in the scripting API and new Handlebars helpers facilitate easy retrieval of this data and simplify personalization efforts.


*The wizard-related options require detail tables and are available only in OL Connect Professional and Enterprise editions.*


## Address block, USPS ASE/IMb barcode


The address block feature is expanded to include the USPS ASE/IMb barcode for the US postal address format. This enhancement ensures that address blocks are properly formatted and automatically generate the required Intelligent Mail barcode (IMb), also known as OneCode Solution or 4-State Customer Barcode. This streamlines compliance with USPS mailing standards, enhancing accuracy and efficiency in mail processing.


*Available in all OL Connect editions.*


## REST API


Several new additions and improvements have been made to the OL Connect Server REST API. These updates introduce new and enhanced endpoints that streamline interactions and offer more detailed data retrieval capabilities.


*The REST API is only available in the OL Connect Professional and Enterprise editions.*


### Document Set level statistics for print jobs


The endpoint for retrieving information about print jobs has been enhanced to include details at the document set level.


### List Runtime Parameters


New endpoints have been introduced to retrieve the list of runtime parameters from templates, data mapping configurations, and job presets deployed on the OL Connect Server. This feature will be utilized in future versions of OL Connect Automate but also enables third parties to leverage this information in automation engines and portals.


### Job Output Location


In this release, endpoints that support the JSON Identifier with Output Parameters payload, such as *Process Output Creation* , now include a new **jobOutputFolder** parameter. This optional parameter allows you to specify the target output location for Output Presets, overriding the default Job Output Folder in the preset. This improvement makes it easier to deploy Output Presets across different environments, as the automation engine or API caller can define the target folder dynamically based on the environment’s role (e.g., test or acceptance), simplifying the setup and increasing flexibility.


### Operations end point enhancement


For all workflow-based endpoints in the Connect REST API that utilize data mapping configurations, templates, job presets, or output presets, both the resource name and the configuration ID (i.e., the *Managed File Name* or *Config ID* ) for each resource involved in the operation are now included in the JSON Operations List returned from REST API requests.


This enhancement enables applications calling these REST APIs to easily distinguish jobs and resources, facilitating the display of information on web pages that monitor activities on the OL Connect Server.


## Other highlights


The following outlines some smaller changes and improvements that are noteworthy.


### Designer


#### Remote Control Scripts


This feature enables you to share a central file containing custom functions across your templates.


#### Metadata properties for print sections


Use the Control Script API to define custom metadata properties for sections, often utilized in section cloning. These properties can then be accessed in standard User Scripts or through the @meta helper in Handlebars.


#### Handlebars lookup helper


The **Handlebars**[lookup](https://handlebarsjs.com/guide/builtin-helpers.html#lookup) **helper** enables access to an object’s property using a dynamic key. For example, {{lookup products code}} retrieves the value of products\[code\], where ‘code‘ is a string data field and ‘products’ is a JSON object or JSON field containing key/value pairs for each product


#### Handlebars counter helper


The **Handlebars counter helper** allows for easy insertion of running numbers into documents. It includes optional parameters such as *start* , *step* , *pad* , and *padChar* , enabling customization of the starting number, step size between numbers, and the padding format. The default padding character is “0,” but this can be adjusted as needed via *padChar* . Some examples:


```text
{{counter start=10 step=5}}
```


Outputs the counter starting from 10 with increments of 5 for each iteration.


```text
{{counter pad=4}}
```


Outputs the counter starting from 1 and pads the number to 4 digits with leading zeros (e.g., 0001, 0002, …).


#### ExtraData fields


In new installations, the Data Model view no longer displays ExtraData entries, as they are superseded by Runtime Parameters. However, an option is added to the Preferences to view **ExtraData fields** for environments still using this technique.


### DataMapper


#### Auto detection of data types


The DataMapper Wizard for CSV and Excel files now auto-detects the data type of each column and creates the corresponding type in the data model. Since CSV files do not contain data type information, and Excel data types do not map one-to-one with DataMapper data types, the Wizard determines the type by reading the first ten rows of the sheet.


In some cases, the Wizard may be unable to determine a specific type, in which case it defaults to the *String* type. In all cases, you can change a field’s type from the Data Model pane.


#### ODBC-JDBC library


Connecting to databases with ODBC is now handled via a new, more modern internal library (aka the ODBC-JDBC *bridge* ). In most cases, this will provide you with better and more stable performance but in other cases there may be some discrepancies between how the data was handled previously and the new library.


By default, existing data mapping configurations will keep using the old library but new DM configurations that require ODBC will use the new one. There is, however, an option to switch between libraries. It is recommended that you always use the new library as the old one will be deprecated in a future release.


### Workflow


#### JS Includes preference panel


You can now add *JScript Include* modules from a new panel in the Workflow preferences. The user interface allows you to add, remove and re-order the modules.


This makes it easy for you to manage the various external JS modules you want to use in your Workflow processes.


#### LPD Input improvements


The LPD Input task now stores the entire content of the LPD Control file in Job Info 7. This allows you to access fields in the control file (like the C field for banner page classes) that were otherwise not available.


In addition, a fix has been implemented so that Job Infos are properly reset in between multiple jobs.


#### SSL Security


The OpenSSL version used by various Workflow plugins has been upgraded to 3.0.14 LTS. This will provide you with enhanced security and will prevent security monitoring systems from flagging OL Connect Workflow as a vulnerability.


Don’t forget to check out our[Online Resource Center](https://olresourcecenter.uplandsoftware.com/) . There, you can learn the fundamentals of OL Connect, enhance your expertise through blog posts and interactive tutorials and discover best practices. Additionally, you can access sample documents for inspiration via the Welcome Screen in OL Connect Designer.


### Share this:


- [Share on Facebook (Opens in new window) Facebook](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2024-2/?share=facebook)
- [Share on X (Opens in new window) X](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2024-2/?share=twitter)
- [Share on LinkedIn (Opens in new window) LinkedIn](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2024-2/?share=linkedin)
-


### *Next best reads for you!*


### Leave a Reply[Cancel reply](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2024-2/#respond)


##### All Comments (1)


- Pete Andrews November 15, 2024


great updates. The JS Includes preference panel sounds fantastic.


[Reply](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2024-2/?replytocom=461#respond)
