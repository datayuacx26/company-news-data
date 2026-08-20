---
schema_version: "1.0.0"
document_id: "bb1001e2a8aadca1d82537a38f4b1d6014cc340ea226ff9b8c0f0eddd4a37cf3"
company_key: "upland-software-inc-common-stock"
company: "Upland Software Inc."
source_id: "upland-software-inc-common-stock-rss-53a5709ba49e"
canonical_url: "https://olresourcecenter.uplandsoftware.com/blog/creating-an-accessible-letter-template-in-ol-connect-designer/"
published_at: "2025-06-24T13:30:00+00:00"
first_seen_at: "2026-07-26T13:25:40.115128+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:26b2804ad8c4ef5d6f476c63f86c177e3a82c6ae1ad1ae3e68974509da7993f1"
---

# Creating an accessible letter template in OL Connect Designer

In this article, you’ll learn how to configure an OL Connect template to generate accessible PDF output compliant with the PDF/UA standard. PDF/UA compliant output will be available as a paid option in an upcoming version of OL Connect Designer.


**An Early Access 60-day free trial version, which generates documents with watermarks, is now available via the Web Activation Manager portal (requires valid OL Care).** If you would like to purchase the PDF/UA output option immediately to remove the watermark limitation, please contact your authorized OL Connect reseller or Customer Success Manager.


> **Note:** The PDF/UA output described in this tutorial requires the Chromium based version of OL Connect Designer 2026.1. This is a significant upgrade, incorporating the Chromium browser engine, which replaces the older Gecko engine. The upgrade brings the latest HTML, CSS, and JavaScript standards to the software, opening up new possibilities for future UI enhancements and introducing new output options, such as the ability to generate accessible PDF output using PDF/UA (Universal Accessibility). See:[OL Connect 2026.1 What’s new](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2026-1/)


The tutorial guides you through preparing a business letter template, focusing on key aspects to ensure compliance with the PDF/UA standard (formally known as ISO 14289-1). Along the way, we’ll address key techniques for optimizing template elements for accessibility.


## Completed letter, before ensuring compliance


The following image shows the final layout of the letter. It consists of an address block, subject, body copy, and sender information. The background of the letter is a virtual stationery image.


The tutorial covers these steps to creating PDF/UA compliant output:


- Enable accessibility features.
- Configure the logical structure tree.
- Provide alternate text for images.
- Set the document language.
- Define the document title.
- Generate the PDF/UA compliant output.
- Validate the output.


## Enable accessibility features


To generate PDF/UA-compliant output, one of the most important elements is the structure tree, which defines the document’s logical reading order. The document must have a clear structure, including headings, paragraphs, lists, tables, and more. OL Connect automatically generates this structure tree based on the merged elements in the template during content creation.


To enable generating the structure tree:


1. Open the template and load the accompanying data.
2. In the menu bar, select **Context** and then **Accessibility** …
3. Check the option **Include PDF/UA metadata and tags tree** .


Once enabled, a small information icon will appear next to the Print Context in the **Resources** panel, confirming that accessibility features are active.


> **Note:** Enabling **Include PDF/UA metadata and tags tree** disables font ligatures, as some screen readers cannot process them. Ligatures are combined characters in certain fonts, such as “fi”, “ffi” or “ti”.


## Configure the logical structure tree


In PDF/UA, the proper use of **headings** is essential for accessibility. Headings organize the content and improve navigation for all users, including those relying on assistive technologies. Screen readers, for example, use heading levels (e.g.,` <h1>` ,` <h2>` ) to provide and overview of the important content in the document. Browsers use heading levels to appropriately size and space headings. Screen readers also allow users to quickly move between headings using keyboard shortcuts, so they can skip unnecessary content and go straight to the sections they care about.


In accessible letter documents, it’s common to format the **subject line** as a **Heading 1** (<h1>), making it the top-level heading. In the tutorial template, the subject is currently formatted as a regular paragraph.


To change the subject to Heading 1:


1. Place the text cursor inside the subject line.
2. In the **Formatting toolbar** , locate the **Paragraph Format** dropdown (first dropdown on the toolbar) and select **Heading 1** .


This adjustment ensures that the subject is recognized as a top-level heading, improving the document’s logical structure and accessibility for users of screen readers and other assistive tools.


### Review the tree


To conclude this part of the tutorial, open the **Outline** pane. This view displays the document’s content tree, showing the hierarchy of headings and paragraphs. This visual confirmation helps ensure that:


- Headings are properly nested (<h1> followed by <h2>, etc.).
- The document structure is logical.


When adding **positioned objects,** like an address block, they are manually positioned on the page visually. However, the <div> elements used for these elements are inserted at the **end of the DOM** , which means they appear at the **end of the PDF structure tree** , potentially disrupting the reading flow.


To fix the Logical Order, drag the respective object to the appropriate location in the **Outline** pane or make this adjustment in **Source view** .


## Provide alternate text for images


Alternate text (alt text) provides a textual description for visual elements such as images, logos, or signatures. This is essential for users relying on screen readers or braille displays, as it ensures that non-text content is accessible. Alt text also appears in place of images for any users that might have image display disabled. For accessibility compliance, any meaningful image in a document must include appropriate alt text.


In this tutorial, the letter includes a hand drawn signature image. Since it conveys information (i.e., identifying the sender), it must include alt text to ensure users with visual impairments can understand its purpose. For example: “Hand-drawn signature of Pierre Probst, sender”


To set alternate text for the image:


1. Select the signature image in the template.
2. In the **Attributes** pane, locate the **Image** section.
3. Enter a descriptive text in the **Alternate text** field.


> **Tip:** Keep alt text concise but meaningful. Avoid phrases like “image of” or “picture of.” Screen readers typically identify the element type.


## Set the document language


Defining the document’s language is a core requirement of PDF/UA, as it enables screen readers to pronounce text correctly and apply language-specific rules for speech and braille output.


In OL Connect, the document language is determined by the **[Locale](https://help.uplandsoftware.com/objectiflune/en/olconnect/2025.1/designer/Styling_Formatting/Locale.html)** setting of the template. This not only defines the language used but also controls formatting for dates and numbers, playing a key role in OL Connect’s translation feature.


To view and change the document language:


1. Toggle to **Preview** mode to see how the date is currently formatted.
2. Navigate to the **Edit** menu and then select **Locale** .
3. Select a different Locale (e.g., switch from English, United States (en-US) to for example French, France (fr-FR).
4. Click **OK** to apply the change. See how the date format and month name update with the new locale setting.


## Define the document title


To comply with PDF/UA standards, it’s essential to define a meaningful **document title** . Screen readers and other assistive technologies rely on the title to provide users with an understanding of the document’s content or purpose before they begin reading.


In OL Connect, the document title can be set different ways, depending on the needs of the template:


- Via the **** template’s **Properties** to set a static title (choose File > Properties).
- Through a **Control Script** in the template for dynamic titles and static titles, for example:


```text
merge.template.properties.title = `Letter for ${record.fields.firstName}`
```


- Or via the **PDF Options section** in the Output Preset PDF (for both static and dynamic titles).


## Create PDF/UA output


Once accessibility features are configured and applied, the final step is to generate the PDF/UA-compliant documents. OL Connect allows you to do this via the **Print Wizard** and **Print dialog** . Here’s how to generate PDF/UA output using the **Print dialog** :


1. Go to **File** > **Print/Proof Print** , or select **Print/Proof Print** from the toolbar.
2. Click the **Advanced** button.
3. Select the **Generic PDF** printer model. Make additional settings, like printing Virtual Stationery, and click **Next** .
4. On the **PDF Options** page, set **Type** to **PDF/UA-1**


- Selecting PDF/UA will automatically enable document-level separation. Separate PDF/UA files will be created for each recipient.


5. Optionally, enter a custom title for the output file or use the` ${template.title}` variable to apply the title defined by the template.
6. Click **Print** .


What happens behind the scenes:


- The **Merge Engine** generates the structure tree for the merged documents and set properties like the language and document title.
- **XMP metadata** required for PDF/UA is automatically added by OL Connect’s Output Engine.
- **All fonts are embedded** to ensure consistent rendering and accessibility across platforms.


## Validate the output


Once your PDF/UA documents are generated, it’s essential to **validate** them using specialized tools like Adobe Acrobat Pro or the free[PAC Accessibility Checker](https://pac.pdf-accessibility.org/en) to ensure they meet accessibility standards.


This final step confirms that key elements, like the structure tree, metadata, alt text and reading order, are correctly implemented and recognized by assistive technologies.


However, it’s important to note that warnings and errors from these tools don’t always mean the document is inaccessible. For example, PAC may flag missing tags on list items or display content incorrectly, such as missing table headers or concatenated text, in its screen reader preview. However, the document may still be perfectly accessible when tested with actual assistive technologies.


These tools focus on technical aspects and don’t cover all accessibility issues. A document might be technically correct but still not accessible. For example, if a bank account number is in a background image of an invoice (virtual stationery), screen readers will not be able to read it. The document may meet all required aspects but is not properly accessible.


## Conclusion


This tutorial outlined the key steps for configuring an OL Connect letter template to meet the PDF/UA standard, including structuring the content, adding alt text, setting document properties for the title and language, and validating the output. These steps are the foundation for generating accessible PDF output.


Many of the features covered, such as setting the Locale or using the Outline view, may already be familiar, but template designers must review and adjust their designs to ensure proper application and enhancement.


The ability to generate PDF/UA output is enabled by the move to the Chromium engine in OL Connect, along with OL Connect’s Output Engine capabilities. However, achieving true accessibility requires additional effort, and the[WCAG](https://www.w3.org/TR/WCAG20-TECHS/pdf) guidelines provide valuable guidance.


Accessibility is not only about the technical aspects of templates but also includes considerations like good color contrasts and readable typefaces and fonts. Layout should include ample white space between lines and paragraphs, and avoid larger blocks of text. Finally, even the words used in document improve readability. Content creators should use plain language and avoid complex sentence structures. These techniques help ensure documents are accessible to everyone.


Resources used in this article:


[Letter accessibility demo](https://olresourcecenter.uplandsoftware.com/wp-content/uploads/2025/06/Letter-accessibility-demo.OL-package)[Download](https://olresourcecenter.uplandsoftware.com/wp-content/uploads/2025/06/Letter-accessibility-demo.OL-package)


> **Important!** Please note that this package was designed in the new OL Connect Designer using Chromium, so it cannot be opened in other versions of the software.


### Share this:


- [Share on Facebook (Opens in new window) Facebook](https://olresourcecenter.uplandsoftware.com/blog/creating-an-accessible-letter-template-in-ol-connect-designer/?share=facebook)
- [Share on X (Opens in new window) X](https://olresourcecenter.uplandsoftware.com/blog/creating-an-accessible-letter-template-in-ol-connect-designer/?share=twitter)
- [Share on LinkedIn (Opens in new window) LinkedIn](https://olresourcecenter.uplandsoftware.com/blog/creating-an-accessible-letter-template-in-ol-connect-designer/?share=linkedin)
-


### *Next best reads for you!*


### Leave a Reply[Cancel reply](https://olresourcecenter.uplandsoftware.com/blog/creating-an-accessible-letter-template-in-ol-connect-designer/#respond)
