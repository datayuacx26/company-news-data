---
schema_version: "1.0.0"
document_id: "57ee843dcf912f32e439cac07bcddd33e549a2b8d980a15b4b4e309bfc88c0f6"
company_key: "upland-software-inc-common-stock"
company: "Upland Software Inc."
source_id: "upland-software-inc-common-stock-rss-53a5709ba49e"
canonical_url: "https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2025-1/"
published_at: "2025-04-28T13:50:34+00:00"
first_seen_at: "2026-07-26T13:25:40.115128+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:1a713a2cad9e7f3cfbb4c131d059c20f8b7101f8238360d94f7f78a4052f164f"
---

# What’s new in OL Connect 2025.1

The 2025.1 release, focuses on fulfilling long-standing customer requests, including new Microsoft 365 SharePoint tasks and a Job Statistics retrieval task for OL Connect Workflow. It also brings a range of smaller, customer-driven enhancements to the DataMapper and Designer components, all aimed at making your daily tasks easier. Let’s dive into what’s new in this release!


## Microsoft 365 SharePoint tasks


These new OL Connect Workflow tasks allow you to upload and download documents from Microsoft 365 SharePoint environments. They simplify integration by connecting directly to Microsoft 365 SharePoint through the user interface, eliminating the need for custom scripts.


## Job statistics task


A new task in OL Connect Workflow has been added to retrieve statistics for print jobs created through OL Connect Server in both XML and JSON format. The data includes the number of documents, sheets, and pages used, as well as the media types applied. This feature is valuable for accounting purposes and helps track costs for better budget management in a printing environment.


Additionally, the information includes custom properties assigned to documents and document sets, which can be used to enrich documents with relevant meta data when uploading them to a document management system for archiving.


## Section Clone naming


The Section Clone wizard in OL Connect Designer now lets you set section clone names based on a data field, such as an insurance policy name. This makes it easier during the design phase to quickly find and view a specific section in Preview mode, helping you validate the content presentation or check the results of certain business rules.


## Insert image through insertion point


The Insert Image dialog in OL Connect Designer now includes an option to specify the insertion point before placing the image on the page. In previous versions, images were always added as inline elements, requiring manual steps to move them to the correct location or place them in an absolute position.


## Browsable record copies


The ability to create additional copies of records in DataMapper is a powerful feature introduced in a previous version. However, until now, the results could only be viewed by generating output. We have now made the copies visible and browsable within the Data Model, and you can also preview them directly in the Designer.


## Other highlights


The following outlines some smaller changes and improvements that are noteworthy.


### Workflow


- The **XML/JSON Conversion task now converts XML attributes** into JSON properties, ensuring their values are stored in the job data for easy access in tasks like RunScript.
- The 2025.1 release introduces the **Microsoft XSLT engine** to the Open XSLT task, delivering a more efficient and performant transformation engine for converting XML content into other structures and document formats.
- The release now comes with a **64-bit version of the AlambicEdit PDF** library, allowing manipulations on larger files.
- Improvements have been made to the SFTP/FTPS output plugin to prevent file transfer issues for user accounts with limited permissions.


### DataMapper


- Methods have been added to **skip records or stop processing through Action scripting** , reducing the need for additional Condition and/or Action steps.
- You can now create, **read and write data-set properties** from post-processor scripts as an alternative to having to re-process entire data sets in an automation tool to set properties.


### Designer


#### **Copy Data Model**


An option is added to the contextual menu of the Data Model view allowing you to copy the model of the current template/data mapping configuration. This makes it easier to generate sample data using AI tools like ChatGPT and Microsoft Copilot. Copy the model, paste it into the AI, and request one or more sample records.


See the[streamlining template design with AI article](https://olresourcecenter.uplandsoftware.com/blog/streamlining-template-design-with-ai/) for more inspiration on using AI in your template design process.


#### **Improved double-click behavior for Image resources**


Double-clicking an image resource in the Resources view now offers a smoother experience. If OL Connect Designer doesn’t have a built-in viewer for the file type, a prompt appears, allowing you to open the image using the default application set by the operating system. This lets you not only view the image but also make edits that are saved directly to the file in the template, improving workflow efficiency


#### **Other enhancements**


- When **dragging and dropping data fields from a detail table** into sections that will be cloned using the Section Cloning Wizard, expressions are now automatically inserted with the correct detail record context. This reduces the need to manually adjust or remember the proper syntax.
- To prevent imported data, loaded via the ‘Open Data File…’-option, from overwriting existing field types in a template, a new dialog now appears when **data type differences** are detected. It lets you choose whether to keep the current field types or use those from the imported file.
- Various small enhancements have been made to the **source editors** , including zooming in and out with Ctrl+= and Ctrl+-, and moving lines up or down using Alt+Arrow Up and Alt+Arrow Down.


### Output


#### **Controlling the Title value in PostScript output**


The Printer Settings page for PostScript jobs now include a Title input field, which sets the %%Title value in the PS output. This value is commonly used by printers to display the job name on a print controller. With this addition, users no longer need to rely on automation tools like OL Connect Workflow to modify the title for PS jobs.


#### **Prompt for Printer Properties in all OL Connect versions**


For users of the Desktop edition, the “Prompt for Printer Properties”-feature allows users to submit print jobs through the printer driver and modify finishing options (such as orientation, simple or duplex, color, & etc.) within OL Connect Designer.


This functionality has now been extended to OL Connect Professional and Enterprise editions, enabling users to handle ad-hoc jobs more efficiently directly within OL Connect Designer.


#### **Improved stability for LPR printing through Output Creation**


The stability of LPR printing through OL Connect Server has been improved by implementing a dedicated job queue that collects output files generated by the OL Connect Server and submits them to the printer at a manageable pace.


We have also enhanced the error handling part where jobs that fail to LPR to the printers are now stored in a folder instead of being automatically purged. This allows users to manage the failed jobs manually without the need to regenerate the same output files from scratch.


Users also have the flexibility to configure the timeout period and retry attempts, allowing them to adjust these settings according to their specific scenario (network stability, printer performance, etc.).


#### **Other enhancements**


- Grayscale elements in the PCL output have been improved to more accurately reflect the intended design.
- The handling of large content sets during the Job Creation stage has been improved. For scenarios involving large volumes of data, the technique of executing Data Mapping and Content Creation for ad-hoc jobs, followed by using the “Retrieve Item” plugin to batch jobs into single output file, can now be used to efficiently process the content.
- The number of fields supported for the Grouping feature in Job Creation has been increased.
- Various improvements have been made to the output engine for better handling of non-ASCII characters. These enhancements resolve issues in environments where non-ASCII characters are used in Windows usernames and font names.


### Share this:


- [Share on Facebook (Opens in new window) Facebook](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2025-1/?share=facebook)
- [Share on X (Opens in new window) X](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2025-1/?share=twitter)
- [Share on LinkedIn (Opens in new window) LinkedIn](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2025-1/?share=linkedin)
-


### *Next best reads for you!*


### Leave a Reply[Cancel reply](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2025-1/#respond)
