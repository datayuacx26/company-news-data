---
schema_version: "1.0.0"
document_id: "b25a2e25aca0ceca2c83828cdfd9aaab7af1ac3be71ecfeb49b543a9ece520ae"
company_key: "upland-software-inc-common-stock"
company: "Upland Software Inc."
source_id: "upland-software-inc-common-stock-rss-53a5709ba49e"
canonical_url: "https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2025-2/"
published_at: "2025-11-19T17:00:00+00:00"
first_seen_at: "2026-07-26T13:25:40.115128+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:239216f87cf528dd73f20647ed013bcead038188f3b115fceab7b790bdc65e97"
---

# What’s new in OL Connect 2025.2

**OL Connect 2025.2** brings a wave of powerful enhancements designed to make data mapping, printing, and automation more efficient than ever.


We’ve streamlined JSON data mapping with automatic detail table creation and improved array handling. The experimental grayscale output option and PostScript-to-PDF conversion provide cost savings and new automation capabilities. The DV-Freimachung address block for the German market ensures addresses are correctly formatted and automatically include the required postage imprint. Additionally, new OL Remote Print plugins for OL Connect Workflow and OL Connect Automate simplify building advanced IP printing solutions.


Let’s dive into what’s new!


## OL Connect Automate


The headline feature of 2025.2 is OL Connect Automate, which replaces OL Connect Workflow with a more flexible, intuitive, and scalable automation engine.


OL Connect Automate makes it easier for organizations to build and adapt automation, with faster setup, version control, and deployment options that fit both on-premises and customer-managed cloud environments. Pre-built examples and guided templates help teams hit the ground running, covering common use cases so users can learn by doing and quickly build on proven best practices.


### Key highlights


- **Visual, low-code design** : A browser-based flow editor with drag-and-drop nodes simplifies building and editing automation for business and IT users.
- **Modern architecture** : Built on a 64-bit architecture, Unicode and with JSON-first processing for improved speed, scalability, and reliability.
- **Flexible deployment** : Can be installed on-premises or deployed via container technology in customer-managed cloud or hybrid setups.
- **Seamless integrations** : Connects easily to OL Connect products and third-party tools like Docuware, Therefore, Xtendis, SharePoint, OneDrive, and leading email providers.
- **Git-based project management** : Enables version control, collaboration, and consistent deployment across development, testing, and production environments.
- **Migration made simple** : OL Connect Automate and OL Connect Workflow can run side by side, helping teams transition gradually without disrupting existing processes.


Check out the[OL Connect Automate online help](https://help.uplandsoftware.com/objectiflune/en/olconnect-automate/1.0/index.html) and get started!


## Streamlined JSON data mapping


The 2025.2 release introduces major enhancements to JSON data mapping in the DataMapper component. These updates bring JSON detail table capabilities closer to those available for XML, while adding new options that significantly reduce the effort required for repetitive, manual tasks.


JSON is increasingly becoming the preferred format for system integrations, often replacing XML as the standard for data exchange. Many third-party applications now deliver and consume data in JSON, making it the go-to format for endpoints and APIdriven integrations.


The changes in OL Connect 2025.2 focus on simplifying the creation of detail tables, a common requirement for handling line items in invoices, statements, and other transactional documents.


### Add Repeat step improvement


The behavior of creating a repeat step for objects within a JSON array has been improved. Instead of looping through the fields inside an object, the Add Repeat option now iterates over sibling objects in the array. This provides a clearer and more intuitive starting point for building detail tables and extracting line-item data.


### Convert Array to Detail Table


A new Create Detail Table function is now available directly from the JSON viewer’s contextual menu. With a single action, it creates the required repeat step, automatically generates the corresponding extraction steps, and detects and sets data field types.


The feature supports nested details up to three levels deep (including the main level), making it much easier to configure data mappings for complex, structured documents such as invoices, statements, and insurance policies.


This enhancement is a real time saver, reducing manual setup and streamlining the overall process of working with JSON data.


### Dynamic top-level object


Added support to dynamically set the element used by the Document Boundaries On Element trigger for JSON data sources. The JsonPath can be constructed using automation variables and runtime parameters, providing flexibility when overall data structures are consistent across job types but the parent elements holding the records differ. This enhancement removes the need for separate data mapping configurations per job type, allowing a single configuration to efficiently handle multiple scenarios.


## Output to Grayscale


An experimental output option is introduced to generate documents in true grayscale, reducing print costs by ensuring printers apply grayscale clicks instead of more expensive color clicks.


When enabled, this feature automatically converts all content, including text and images, into grayscale, removing embedded RGB or CMYK color data. By using this option, organizations can produce more cost-effective output.


> **Note:** As this feature is experimental, some limitations apply. It may not work with all color spaces or transparency effects, and content added via the Additional Content option specified in an Output Preset will not be converted.


## Address block, DV-Freimachung


The address block feature now supports DV-Freimachung for the German market. This enhancement ensures address blocks are correctly formatted and automatically include the required postage imprint. It streamlines compliance with German mailing standards, improving both accuracy and efficiencying in mail processing.


> **Tip!** If you are interested in adding local address block formats for your country or region, please file a feature request via our[Support portal](https://support.uplandsoftware.com/portal/ss/login) so we can analyze your request and work with you to add support.


## Output Preset variables


The Additional Content interface in Output Presets lets users add elements like text and barcodes to generated content. This is commonly used for OMR codes or jobrelated information on a banner page. The drop-down menu for inserting variables has been expanded with commonly used entries, for example, to make it easier to set conditions for adding content to banner or cover pages.


## OL Remote Print (OLRP) tasks and nodes


Gain full control over your OLRP print jobs with the new OL Remote Print (OLRP) plugins. These plugins provide seamless integration with OL Connect Workflow processes and OL Connect Automate flows, enabling you to build robust, remote printing solutions and integrate with OL Connect technology.
Download print jobs from the OLRP Server, retrieve detailed job information, update job statuses, and delete jobs, all within your automation environment.


## PostScript to PDF conversion API


OL Connect Server now supports converting PostScript files to PDF through a new REST API endpoint. PostScript files uploaded to the OL Connect File Store can be seamlessly transformed into PDF, extending the automation capabilities of Workflow and Automate. Inspired by the Create PDF task in Workflow, this initial implementation provides straightforward conversion, with additional functionality planned for future releases.


**Endpoint:**


```text
http://localhost:9340/rest/serverengine/converter/pstopdf/{fileId}
```


When a PostScript file is submitted for conversion, OL Connect Server returns a Link header containing URLs to check progress, retrieve the PDF, or cancel the task.


## Other highlights


The following lists some smaller changes and improvements that are noteworthy.


### Designer


- **Enhanced Preview mode** : Positioned boxes can now be moved and resized directly in Preview, making layout adjustments faster and more intuitive.
- Fixed an issue with output that could (in rare cases, depending on the sheet configuration) cause missing or overlapping content.


### DataMapper


- **Credential support for ODBC sources** : Previously limited to MSSQL, credential
handling is now available for all ODBC database providers.
- **Custom connection parameters** : Connection strings now support custom
key/value pairs, providing greater flexibility and compatibility across different
ODBC drivers.


### Output


- Fixed an issue with rotated form being printed incorrectly in IPDS when a printer
is configured to use overlays.
- When creating PDF output with separation and using PDF Security settings,
image resources were missing from the second and subsequent output files,
while the first output file included them. This issue has been resolved.


### Workflow


- Added support for **CDATA in XML/JSON Conversion** plugin. Previously, CDATA
content was ignored, which could result in data loss.
- When using the metadata sequencer, an extra iteration was incorrectly added
when using a “split before” conditional logic and the condition was met on the
last item of the metadata level being sequenced.


### Installer


- **OL Connect’s default memory and system settings** , including memory
allocation, internal communication, and data partitioning, have been
modernized to reduce support issues and align with current hardware
standards.
- **Stricter database account permissions** are enforced to protect data, prevent
unauthorized changes and reduce security risks.


### Share this:


- [Share on Facebook (Opens in new window) Facebook](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2025-2/?share=facebook)
- [Share on X (Opens in new window) X](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2025-2/?share=twitter)
- [Share on LinkedIn (Opens in new window) LinkedIn](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2025-2/?share=linkedin)
-


### *Next best reads for you!*


### Leave a Reply[Cancel reply](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2025-2/#respond)
