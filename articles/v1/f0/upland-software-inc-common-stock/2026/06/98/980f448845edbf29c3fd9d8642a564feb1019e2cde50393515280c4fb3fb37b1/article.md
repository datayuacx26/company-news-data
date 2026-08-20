---
schema_version: "1.0.0"
document_id: "980f448845edbf29c3fd9d8642a564feb1019e2cde50393515280c4fb3fb37b1"
company_key: "upland-software-inc-common-stock"
company: "Upland Software Inc."
source_id: "upland-software-inc-common-stock-rss-53a5709ba49e"
canonical_url: "https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2026-1/"
published_at: "2026-06-05T13:37:56+00:00"
first_seen_at: "2026-07-26T13:25:40.115128+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:03b28136f464dc497df8aa853b55995d034cf5cada1193c42f2c5bdb15b4a441"
---

# What’s new in OL Connect 2026.1

The 2026.1 release (scheduled for June 9, 2026) is a significant milestone for OL Connect, with a major advance in OL Connect Designer and its new Chromium-based rendering engine. This enhancement establishes a modern, standards-based foundation for document design, enabling support for the latest HTML, CSS, and JavaScript standards and the generation of PDF/UA-compliant output. Organizations can utilize OL Connect to meet growing accessibility requirements and deliver more inclusive communications.


This release includes both the existing Designer and a new Chromium-based Designer, allowing organizations to begin adopting these capabilities while maintaining stability in existing production environments. Chromium is planned to become the default rendering engine in a future release, providing a clear path toward modernization.


Alongside this advancement in OL Connect Designer, OL Connect 2026.1 delivers targeted enhancements to DataMapper, input processing, and a new release for OL Connect Automate, helping simplify document workflows and improve consistency across key processing scenarios.


## Chromium-based rendering engine and accessible output


The headline feature of 2026.1 is the evolution of OL Connect Designer through the adoption of a Chromium-based rendering engine. This provides a modern foundation for document design while enabling the creation of accessible, PDF/UA-compliant documents.


Delivered alongside the existing Gecko engine, this dual-engine approach enables teams to evaluate and adopt the Chromium engine at their own pace (both versions are available for download on the WAM). This release represents the next step in a broader transition toward a Chromium-based architecture, making it more broadly available for production use.


### Key highlights


- **Standards-based design foundation:** Supports modern HTML, CSS, and JavaScript for building advanced document layouts
- **Accessible document output:** Enables tagged PDF generation aligned with PDF/UA requirements
- **Dual-engine flexibility:** Allows teams to work with Gecko or Chromium based on their environment and readiness
- **Future transition path:** Establishes the direction toward Chromium becoming the default engine


The following sections highlight some Chromium-specific features that enhance the design and development experience.


### Object Fit and Object Position


One of the practical advantages of the new Designer is support for the CSS **` object-fit`** and **` object-position`** properties. These capabilities make it much easier to control how images are displayed within set dimensions, regardless of the size of the source image.


This addresses a common challenge in personalized and data-driven communications where image sizes can vary significantly from one record to another. Instead of manually resizing or creating multiple layouts, designers can define how images should scale and align within a predefined space.


Typical use cases include:


- Product images in picking lists
- Signature images in letters
- Marketing content that uses dynamically sourced imagery


By allowing images to automatically scale and position themselves relative to their container, designers can create more flexible templates while maintaining a consistent visual appearance across all generated documents.


### Vertical Content Alignment


The Chromium-based Designer also introduces enhanced vertical layout control through support for modern CSS alignment properties such as **` align-content`** and **` justify-content`** .


Historically, vertically aligning content within a fixed-size container could require complex workarounds. With Chromium, content can be positioned more intuitively within a box, making it easier to create layouts that adapt gracefully to varying amounts of content.


Examples include:


- Aligning content to the top, center, or bottom of labels
- Distributing content evenly within fixed-height containers


This feature helps designers solve a wide range of layout challenges while reducing the amount of custom CSS/HTML.


### Enhanced Source Editor Experience


The Designer based on Chromium also delivers significant improvements to the source editing experience for CSS, JavaScript, and scripting resources. The updated Source Editor provides a more modern development environment designed to improve productivity and reduce errors when working with advanced templates.


Key enhancements include:


- Improved syntax highlighting for better code readability
- Enhanced JavaScript code completion and suggestions
- Real-time validation for CSS, SCSS, and JavaScript resources
- Direct links to online documentation for CSS properties, JavaScript functions, and the OL scripting API
- “Go to Definition” functionality for quickly navigating to functions and variables using **Ctrl+Click** or **F3**
- Context-sensitive tooltips that display comments and documentation for functions and variables


## General enhancements in version 2026.1


The following enhancements are available in both the Chromium-based Designer and the existing Designer.


### Defining document boundaries based on barcodes


OL Connect 2026.1 introduces enhancements in DataMapper that simplify document processing and reduce reliance on external steps. The Document Boundaries option has been extended with a new trigger type: Barcodes. With this capability, DataMapper can now detect document boundaries directly from barcodes embedded in print-ready input files such as PDF and PostScript.


This simplifies the solution architecture and reduces reliance on external processing steps and scripting, addressing a long-standing requirement that was previously handled through intermediary workflows.


#### Scripting


In addition, barcode extraction capabilities have been extended to the scripting API for both boundary detection and data extraction use cases. This provides greater flexibility for advanced implementations that require programmatic control over barcode processing.


```text
// Boundaries script  var barcodeVal = boundaries.getBarcode(      region.createRegion(4, 68, 13, 78), "DATAMATRIX"  ).join();   if (barcodeVal && barcodeVal.startsWith("B")) {      boundaries.set(2);  }
```


```text
// Action script  var barcodeVal = data.extractBarcode(4, 13, 70, 10, "DATAMATRIX")r>record.fields.Barcode = barcodeVal
```


### Improved PCL input handling


A new PCL input engine based on GhostPCL improves consistency when processing PCL print streams, particularly for large files or scenarios where rendering inconsistencies were previously encountered.


GhostPCL is a widely used technology in the document imaging and printing industry and is already adopted across other Upland products. Both the existing and new PCL engines remain available, allowing users to select the approach that best meets their requirements.


### Auto-generate Detail Tables for XML


The Create Detail Tables functionality is now extended to XML data models, allowing users to automatically generate detail tables from repeating structures in a single step. This reduces manual configuration and provides a more efficient starting point for working with line-item data.


### Expanded API capabilities


OL Connect 2026.1 introduces a new REST API endpoint that enables XPS-to-PDF conversion, supporting Windows Printer Queue inputs in OL Connect Automate 1.1. This enables downstream processing such as data extraction and document boundary detection, while moving OL Connect Automate closer to parity with OL Connect Workflow.


### Usability and reliability improvements


OL Connect 2026.1 includes key enhancements that improve usability, stability, and consistency:


- Improved email handling: Enhanced retry logic for Microsoft 365 email output reduces unnecessary failures and duplicate drafts
- Merge engine improvements: More efficient handling of email batch processing
- Improved DataMapper stability: Resolves issues when working with large or complex input files
- OpenAPI documentation: Introduces standardized API documentation as a first step toward a more structured API framework
- Installer enhancements: Provides warnings for unsupported Windows versions to support more reliable deployments


## OL Connect Automate


OL Connect Automate 1.1 introduces several enhancements designed to expand integration capabilities, simplify document capture workflows, and streamline deployment in enterprise environments. These updates help organizations build more connected document processes while reducing implementation complexity and increasing operational flexibility.


### Native MariaDB Support


One of the most significant additions in OL Connect Automate 1.1 is the introduction of native MariaDB nodes. This provides a fully supported alternative to Workflow’s Data Repository and eliminates the need to rely on third-party MariaDB/MySQL integrations.


Because MariaDB is already installed with typical OL Connect installations, organizations can leverage an existing, scalable database platform for job management and dashboard integrations.


Common use cases include:


- Managing day and week production sets
- Creating virtual job bags for incoming documents
- Storing data submitted through Capture OnTheGo
- Monitoring job status and lifecycle information through dashboards and reporting tools


By providing native support, Automate 1.1 simplifies database integration while offering a more robust and supported foundation for workflow-driven processes.


### FileBound Integration


New FileBound nodes enable workflows to upload documents directly to FileBound using REST-based integration. This helps automate document archiving, metadata management, and retrieval as part of end-to-end document processes.


### Windows Printing Input


*Windows Printing Input was originally planned for OL Connect Automate 1.1. However, based on findings during the final stages of development, this feature has been deferred and will be included in a subsequent release.*


A new Print Capture node allows Automate to receive print jobs from Windows applications and convert captured XPS output into PDF for further processing. This extends automation capabilities to legacy applications and print-based workflows.


### Offline Installer


Automate 1.1 also introduces an offline installer that bundles required dependencies into a single package. This simplifies deployment for customers operating in environments where internet access is restricted.


### Share this:


- [Share on Facebook (Opens in new window) Facebook](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2026-1/?share=facebook)
- [Share on X (Opens in new window) X](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2026-1/?share=twitter)
- [Share on LinkedIn (Opens in new window) LinkedIn](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2026-1/?share=linkedin)
-


### *Next best reads for you!*


### Leave a Reply[Cancel reply](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2026-1/#respond)
