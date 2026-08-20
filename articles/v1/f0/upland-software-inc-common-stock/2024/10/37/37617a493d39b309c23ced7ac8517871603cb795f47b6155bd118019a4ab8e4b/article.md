---
schema_version: "1.0.0"
document_id: "37617a493d39b309c23ced7ac8517871603cb795f47b6155bd118019a4ab8e4b"
company_key: "upland-software-inc-common-stock"
company: "Upland Software Inc."
source_id: "upland-software-inc-common-stock-rss-53a5709ba49e"
canonical_url: "https://olresourcecenter.uplandsoftware.com/blog/streamlining-template-design-with-ai/"
published_at: "2024-10-25T14:42:07+00:00"
first_seen_at: "2026-07-26T13:25:40.115128+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:c130ee948d1d6b8a2cf05340c95f89dd006eed51a1edec93269778f7ed961812"
---

# Streamlining template design with AI

As someone who uses AI tools daily, I often wonder how I ever managed without them. Generative AI tools like ChatGPT not only help me improve my writing but also enhance my technical skills by analyzing my work and suggesting improvements.


For template designers using OL Connect Designer, AI can enhance productivity and serve as a valuable learning tool. It speeds up maintenance and aids in refactoring tasks while promoting best practices for creating efficient templates. AI is particularly helpful for HTML, CSS, and JavaScript related tasks, as well as for generating dummy data. In this article, I’ll provide practical examples to inspire you to incorporate AI into your daily work or use it more effectively.


## Cleaning up HTML


Imagine dealing with legacy OL Connect templates filled with outdated placeholders (text wrapped in` @` signs) which typically are placed inside` <span>` elements that have ID attributes for personalization scripts. Migrating to modern Handlebars expressions can be tedious. Instead of searching through the code and making manual edits, you can use ChatGPT to automate the cleanup process.


### Find and replace placeholders


You can easily instruct ChatGPT to process HTML by providing a clear task. For example:


```text
In the provided HTML, perform the following tasks:
- Remove @ signs from text and wrap the content in Handlebars expressions like this: {{value}}. Ignore email addresses.
- Remove the <span> elements wrapping Handlebars expressions.
```


With this straightforward *prompt* (the text describing the task for the AI), ChatGPT can quickly and accurately process your HTML, transforming it into a cleaner format using the latest personalization techniques.


> **Note!** In my examples, I separate the tasks onto individual lines for better readability, although this format is not a requirement for the AI.


> **Note!** Results may vary slightly with each AI request, so examples might not look exactly the same each time.


Let’s say you have the following HTML taken from the Source view of a template or snippet:


```text
<p>       Date: <span id="date">@date@</span><br>      Account Number: <span id="customer_number">@customer_number@</span><br>      Invoice No.: <span id="order_number">@order_number@</span>  </p>
```


After processing, you would receive this cleaned-up output:


```text
<p>      Date: {{date}}<br>      Account Number: {{customer_number}}<br>      Invoice No.: {{order_number}}  </p>
```


Now you can copy the code and paste it back into the Source view of your template or to a Handlebars snippet, *et voilà* !


> **Tip!** If you want only the resulting HTML without any comments or explanations, just add:
> – Output only the result without comments.


> **Note** ! When switching to Handlebars, remember to enable the evaluation of Handlebars expressions for your sections. You can find this option in the Properties dialog of the section, accessible via the **Section > Properties** menu.


### More clean-up tasks


The instructions can be expanded with tasks for removing unnecessary elements or cleaning inline styles. In my HTML I often encounter orphaned` <br>` elements, they typically sit before the closing tag of` <td>` (table cell) or` <div>` element. The following prompt removes these` <br>` elements.


```text
In the provided HTML, perform the following tasks:
- Remove <br> elements that appear immediately before closing tags.
```


An other task could instruct the AI to remove inline styles and turn these into CSS classes and create style rules. This promotes better separation between the HTML structure and presentation, greatly improving the readability of the HTML code and styling. You can also include instructions to group common CSS properties into a single rule and to create CSS variables for specific properties, allowing their values to be managed in a central location within the CSS file.


```text
In the provided HTML, perform the following tasks:
- Convert inline styles to CSS classes.
- Group common CSS properties where applicable, and create CSS variables for font-size and font-family to allow centralized management.
```


Let’s apply all these tasks to the following table structure from an insurance document. The template uses a table structure to lay-out information and has a lot of inline styling, unnecessary` <br>` elements, and legacy placeholders.


The complete prompt:


```text
In the provided HTML, perform the following tasks:
- Remove the <span> elements wrapping Handlebars expressions.
- Remove <br> elements that appear immediately before closing tags.
- Convert inline styles to CSS classes.
```


```text
<table id="table_3" class="table-grid" cellpadding="0" cellspacing="0">
<tbody>
<tr>
<td style="width: 28%; font-size: 8pt; vertical-align: top; padding-bottom: 5px; padding-left: 0px; border-style: none; border-color: transparent; border-bottom: 0px none transparent; border-right: 0px none transparent;">
<b>Participant</b><br>
<i>Deelnemer</i><br>
</td>
<td style="width: 2.31%; font-size: 8pt; vertical-align: top; padding-bottom: 5px; border-style: none; border-color: transparent; border-bottom: 0px none transparent; border-right: 0px none transparent; padding-left: 0px;">
:<br>
</td>
<td style="font-size: 8pt; vertical-align: top; padding-bottom: 5px; border-style: none; border-color: transparent; border-bottom: 0px none transparent; border-right: 0px none transparent; padding-left: 0px;">
@ContractOwnerName@<br>
</td>
</tr>
</tbody>
</table>
```


Following these instructions, you would receive the cleaned-up HTML output along with the corresponding CSS style rules.


```text
<table id="table_3" class="table-grid" cellpadding="0" cellspacing="0">
<tbody>
<tr>
<td class="table-cell participant">
<b>Participant</b><br>
<i>Deelnemer</i>
</td>
<td class="table-cell separator">
:
</td>
<td class="table-cell contract-owner">
{{ContractOwnerName}}
</td>
</tr>
</tbody>
</table>
```


```text
:root {
--font-size: 8pt;
--font-family: Arial, sans-serif; /* Specify your font family here */
}


.table-cell {
font-size: var(--font-size);
vertical-align: top;
padding-bottom: 5px;
padding-left: 0;
border: none;
border-color: transparent;
border-bottom: 0 none transparent;
border-right: 0 none transparent;
}


.participant {
width: 28%;
}


.separator {
width: 2.31%;
}


.contract-owner {
/* No additional width specified */
}
```


Copy the code and paste it into the Source view of your template or a Handlebars snippet. Put the style information in a CSS file, like` context_print_styles.css` , or create your own. The output might not be perfect and could need some tweaks, but it will help speed up your workflow.


These are just a few ways to clean up HTML and create CSS styles. Be creative and think of new tasks that fit your needs.


### **Analysis**


I’ve discussed some basic ways to instruct an AI with specific tasks. However, you can also start by asking the AI to analyze the HTML. It will explain the structure and point out things like inline styles and placeholder syntax. This analysis can help you craft your next prompt or even better ask the AI for a prompt to clean up the HTML, which you can then modify to fit your needs.


```text
Analyze the HTML and suggest improvements for cleanup.
```


## Create dummy data


An other great way to utilize AI is by generating dummy data. When creating an OL Connect template, one typically works with sample/test data. This data simulates the data used in a live/production environment and is designed to cover personalization variations, capture exceptions, and test output performance without risking exposure of real customer information.


It’s crucial to avoid sharing actual customer data. Using anonymized data protects customer privacy and makes it easier to seek tech support or post questions on forums. You can share templates and sample data without worrying about revealing sensitive information.


Crafting dummy data can be time-consuming, often requiring adjustments to data exports from business applications or manual data creation.


This is where AI tools like ChatGPT come in handy. You can instruct it to create sample data based on the data model of your template or Data Mapping configuration, providing formatting guidelines for specific fields. Below is a prompt that generates 10 sample records in JSON format with specific formatting instructions:


```text
Using the provided data model for a variable data printing job of a university admission letter, perform the following tasks:
- Generate 10 sample records in JSON format.
- Use dummy addresses located in the Netherlands (Nederland), ensuring that the street name precedes the house number.
- Use typical Dutch first and last names.
- Use fictive city names inspired by famous Dutch painters.
- Format the zip code as four digits followed by a space and two uppercase letters (A-Z).
- Set ages to range from 17 to 29 years.
- Make sure there is at least one recipient where admitted is true and age is 17.
- Create the KIX postal barcode by concatenating the zip code with the house number, removing any spaces.


[
{
"first_name": "",
"last_name": "",
"address": "",
"zip": "",
"city": "",
"country": "",
"admitted": false,
"age": 0,
"kix": ""
}
]
```


To copy the data model of your template:


1. Open the template.
2. In the Data Model view, click the JSON Sample Data icon on the toolbar (without opening a Data Mapper configuration).
The JSON Sample Data editor will open, displaying the Data Model in JSON format.
3. Copy the Data Model and paste it after the instructions in the AI prompt (as shown above).


Below are sample records in JSON format that meet the specified instructions. To use JSON data, you can paste it directly into the JSON Sample Data editor. For other formats, it’s best to process them through the Data Mapper configuration or load them via the Data options in the File menu if you’re using OL Connect Desktop.


```text
[
{
"first_name": "Emma",
"last_name": "Smit",
"address": "Schoolweg 35",
"zip": "3062 IJ",
"city": "Mondriaanwijk",
"country": "Nederland",
"admitted": true,
"age": 17,
"kix": "3062IJ35"
},
{
"first_name": "Finn",
"last_name": "de Boer",
"address": "Zandpad 35",
"zip": "1673 AB",
"city": "Ruisdaelberg",
"country": "Nederland",
"admitted": false,
"age": 22,
"kix": "1673AB35"
},
...
]
```


> **Tip!** In this example we used JSON data as the data format but this can easily be changed to formats like XML and CSV. Simply update your prompt.


> **Tip!** When generating invoice data, you can instruct the AI to create detailed line items using products from your industry, such as consumer electronics, dental products, flowers, etc.


## Conclusion


Although OL Connect doesn’t have built-in AI features right now, tools like ChatGPT can be a big help today. You can use it to optimize templates, clean up code, generate sample data, or analyze and rewrite JavaScript, saving time in your design process.


I hope this article inspires you to incorporate AI into your daily work or explore its potential further. Feel free to share your favourite OL Connect prompts and how you see AI fitting into the application!


> **Important Reminder, always validate AI output!** While AI can greatly assist in streamlining tasks, it’s important to remember that AIs can make mistakes or produce results that may not work properly. It’s advised to validate the output and not use it without verification to ensure the best results.


### Share this:


- [Share on Facebook (Opens in new window) Facebook](https://olresourcecenter.uplandsoftware.com/blog/streamlining-template-design-with-ai/?share=facebook)
- [Share on X (Opens in new window) X](https://olresourcecenter.uplandsoftware.com/blog/streamlining-template-design-with-ai/?share=twitter)
- [Share on LinkedIn (Opens in new window) LinkedIn](https://olresourcecenter.uplandsoftware.com/blog/streamlining-template-design-with-ai/?share=linkedin)
-


### *Next best reads for you!*


### Leave a Reply[Cancel reply](https://olresourcecenter.uplandsoftware.com/blog/streamlining-template-design-with-ai/#respond)


##### All Comments (3)


- Pingback:[OL Resource Center](https://olresourcecenter.uplandsoftware.com/blog/whats-new-in-ol-connect-2025-1/)


- Uomo Del Ghiaccio November 04, 2024


Great article! Can't wait for AI to be incorporated into Designer.


[Reply](https://olresourcecenter.uplandsoftware.com/blog/streamlining-template-design-with-ai/?replytocom=437#respond)
