---
schema_version: "1.0.0"
document_id: "81ef728233ef905634f9e84d9696a774f667b8fcaf9eaa1909d0140f492909ff"
company_key: "upland-software-inc-common-stock"
company: "Upland Software Inc."
source_id: "upland-software-inc-common-stock-rss-53a5709ba49e"
canonical_url: "https://olresourcecenter.uplandsoftware.com/blog/understanding-the-real-user-experience-enhancing-invoice-pdfs-for-accessibility/"
published_at: "2025-09-19T15:14:39+00:00"
first_seen_at: "2026-07-26T13:25:40.115128+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:7774bb45a729e078d70ae3dbe90b4c862845f1d91db979119cd9a05aac7c67ad"
---

# Understanding the real user experience: enhancing invoice PDFs for accessibility

I recently met with Jesse, who is blind, to review the accessible PDF output of OL Connect. My goal was to understand how visually impaired recipients experience our documents and to create practical guidelines for making invoice PDFs more accessible.


While we used various tools during development to ensure compliance, I felt it was important to go beyond these standard checks and observe first-hand how someone who is visually impaired reads and interacts with our files.


> **Note:** The PDF/UA output described in this tutorial requires the Chromium based version of OL Connect Designer 2026.1. This is a significant upgrade, incorporating the Chromium browser engine, which replaces the older Gecko engine. The upgrade brings the latest HTML, CSS, and JavaScript standards to the software, opening up new possibilities for future UI enhancements and introducing new output options, such as the ability to generate accessible PDF output using PDF/UA (Universal Accessibility). See:[OL Connect 2026.1 What’s new](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2026-1/)


## A simple question


We talked about digital invoice documents and their traditional paper counterparts. However, before we started, he asked me a simple question which I couldn’t answer:


> **“How do you think I read traditional paper mail? And by the way, asking someone to read it for me isn’t an option, I don’t want to depend on others.”**


I assumed he was using some assistive technology but this was untouched ground for me. I realized that while we are focused on creating PDF/UA compliant files, we may not have spent enough time considering the workflows of the recipients. What would his workflow look like when receiving email with PDF attachments? How do you go from the inbox to the PDF content?


As it turns out, to read traditional paper mail, he uses assistive technology on his phone ( **[Seeing AI](https://www.microsoft.com/en-us/garage/wall-of-fame/seeing-ai/)** from Microsoft) which helps guide him in scanning the document and then reads it aloud.


## A smooth digital workflow, at least in the browser


Next, we explored his workflow for receiving digital invoice documents sent by email. He uses a browser-based email client in Firefox (which provides a better ad-blocking experience compared to other browsers) to streamline his online experience. This setup makes it relatively easy for him to read incoming messages and open attached PDFs using the browser’s built-in PDF viewer. The screen reader ([NVDA](https://www.nvaccess.org/) ) transitioned smoothly from the email interface to the message content and then to the attached PDF, creating a seamless reading experience.


For comparison, and just for fun, we tried the same workflow using a typical desktop PDF reader. This quickly proved to be challenging. The user interface was difficult to navigate, and dismissing the pop-ups about new features was frustratingly tedious. At that point, I had to provide some help, highlighting how even small design choices in products can create real barriers for visually impaired users.


## Accessible invoice design


We reviewed an invoice he had received from an energy company. Visually, it looked clean and professional, and perfectly fine for someone with full sight. But when he used a screen reader, the experience completely fell apart. The reading order of the tabular information was jumbled, key links were missing, and the payment instructions were inaccessible. I was honestly shocked. It made one thing very clear: accessibility isn’t optional — it’s essential to thoroughly test the output to ensure all recipients can easily access and understand the information.


### The proof is in the ‘reading’


We then reviewed letter and invoice documents generated with the PDF/UA output option in the Early Access version of the new OL Connect Designer. The results were much better. The documents had a logical structure and a proper reading order. Images included alternative text, and the information in the dynamic invoice table was correctly picked up, allowing the screen reader to announce each cell by reading the column header, making the content understandable.


We were both impressed. While there were still areas to improve, these were minor adjustments, such as moving some elements, adding extra headings, or choosing a different layout technique within the template design. Overall, the documents were accessible and easy to navigate.


From this exercise I constructed following shopping list for creating accessible invoices templates in OL Connect:


### Structure tree


Use headings to clearly define the sections in your document. Headings simplify navigating the content in assistive technology, allowing the user to jump to or skip information. Headings act as a table of contents for the document.


Start with **Heading 1** to indicate that the document is an invoice. You can optionally include details like the invoice number or the recipient’s name. Example: *Invoice 12345 for Peter Parker*


Use **Heading 2** for the main sections under the heading to establish a logical content hierarchy. For example:


- Customer’s address information
- Business information
- Invoice details
- Products table with line items
- Payment instructions
- Terms and conditions


Use **Heading 3** for key sections under the **Heading 2** sections.


### Layout techniques


When using **positioned boxes** , a common approach in OL Connect templates to visually arrange content, make sure to check the **Outline pane** . This allows you to verify that the order of` <div>` elements follows a logical reading sequence. Remember, screen readers rely on the order of the content, not the visual layout, so proper structure is critical for accessibility. For more information, check out the article on[Setting the logical reading order for Positioned Boxes](https://olresourcecenter.uplandsoftware.com/tutorials/setting-the-logical-reading-order-for-positioned-boxes/) .


Use **tables strictly for tabular data** , such as dynamic product line items. Avoid using tables for layout purposes. While it may not completely break the user experience, using tables for layout can lead to confusing navigation as screen readers may first explain the structure of the table before processing the actual content. This can disrupt the flow and make it harder for users to understand the information presented.


For best results, use **inline** or **positioned boxes** . Inline boxes are a bit trickier to set up, but they provide more flexibility and better accessibility. Useful techniques for aligning text for accessibility are described in the article[Aligning Text for Accessibility](https://olresourcecenter.uplandsoftware.com/tutorials/aligning-text-for-accessibility/) .


### Products table


Use OL Connect’s dynamic table feature to properly lay out the products, goods, or treatments listed on the invoice. The dynamic table feature automatically adds table headings, which are crucial for screen readers. When navigating the table, the screen reader will announce the column name for each cell, providing context for users.


Ensure your column names are concise, clear, and easy to pronounce. Avoid using abbreviations whenever possible. Clear column names help ensure that the content is easily understood and navigable for all users.


Consider adding a **caption** for the table using a **Heading 2** . This complements the logical order of the document and provides a shortcut to the section. A caption can help users quickly understand the purpose of the table before they begin navigating through the content.


### Payment details


Always clearly state the **amount due** , **bank account number** , and **payment reference** using **selectable text** , not images. Avoid embedding critical details like the bank account number into a background image, which is a common but inaccessible practice.


Including a **QR code** is optional, but it must not be the only way to access payment details. QR codes are not always accessible to users, particularly those relying on screen readers or lacking the technology to scan them. If possible, provide a **clickable link** to an online payment page. This allows users to quickly and easily make payments, enhancing accessibility.


To make navigation easier, **group all payment-related information** under a single heading. This reduces the need to scan the document for information and helps all users quickly find the relevant payment details.


### Terms and Conditions


Ensure that the terms and conditions are included as part of the main content flow, rather than being embedded in a background image or as part of virtual stationery. This practice guarantees the content remains accessible to screen readers and is properly integrated into the document’s logical structure. Begin with a **Heading 2** for this section and use **Heading 3** for any sub-sections to maintain a clear content hierarchy.


If the full text is too lengthy or doesn’t fit the document layout, consider providing a link to an accessible web page where the terms can be read. Ensure that the linked page also follows accessibility best practices to maintain a consistent and inclusive user experience.


### Recap


A well-structured invoice layout not only improves readability, it also significantly enhances navigation for users who rely on assistive technologies like screen readers. When documents are designed with accessibility in mind, they become easier to understand, faster to process, and more inclusive for all users, not just those with disabilities.


Here’s a recommended outline for structuring your invoice using proper heading levels and content blocks:


Heading level Topic Notes


Heading 1 Invoice title This main heading establishes the document’s purpose.


Heading 2 Address information Provide the recipient’s name and full address. Add a subheading and make this screen-reader only text.


Heading 2 Business information Use this section for additional recipient information, depending on your industry. Examples are: contract or policy number, the tenant or insured party details or patient information.


Heading 2 Customer details Use this section for additional recipient information, depending on your industry. Examples are: contract or policy number, the tenant or insured party details or patient information.


Heading 2 Products table Use OL Connect’s dynamic table to list line items. Provide each column with a clear, descriptive header.


Heading 2 Payment instructions Clearly state all payment-related information in one place: total amount due, due date, bank account details, clickable link to pay online.


Heading 2 Terms and Conditions Include the terms and use Heading 3 for subsections or provide a link to online terms and conditions (assure this is online information is accessible too).


## Conclusion


We tested all of this using sample invoice templates, and with just a few small design adjustments, the result was well-structured and easy to navigate using assistive technology like[NVDA](https://www.nvaccess.org/) and[JAWS](https://support.freedomscientific.com/JAWSHQ/JAWSHeadquarters01) . From my experience, many existing templates need editing to meet accessibility standards, and yes, it may take a bit of extra effort for some.


But these improvements don’t just apply to print templates. The same principles can help you create accessible invoice emails and web-based invoice pages too. Designing with accessibility in mind helps *everyone* by making important information easy to find and understand.


I encourage you to apply these steps to your own invoice templates. With just a few thoughtful changes, you can make your documents more accessible and enhance the experience for all users.


### Share this:


- [Share on Facebook (Opens in new window) Facebook](https://olresourcecenter.uplandsoftware.com/blog/understanding-the-real-user-experience-enhancing-invoice-pdfs-for-accessibility/?share=facebook)
- [Share on X (Opens in new window) X](https://olresourcecenter.uplandsoftware.com/blog/understanding-the-real-user-experience-enhancing-invoice-pdfs-for-accessibility/?share=twitter)
- [Share on LinkedIn (Opens in new window) LinkedIn](https://olresourcecenter.uplandsoftware.com/blog/understanding-the-real-user-experience-enhancing-invoice-pdfs-for-accessibility/?share=linkedin)
-


### *Next best reads for you!*


### Leave a Reply[Cancel reply](https://olresourcecenter.uplandsoftware.com/blog/understanding-the-real-user-experience-enhancing-invoice-pdfs-for-accessibility/#respond)
