---
schema_version: "1.0.0"
document_id: "0058add3b4b60ac89b6e1854a090fa6c47a6275315284ff9de6dd1b61e928b7e"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/mastering-the-multi-selector-tool/"
published_at: "2025-09-10T00:00:00+00:00"
first_seen_at: "2026-07-24T09:27:21.536976+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:10da302c2317166ed09b6829416250f6482e13b4c2e82bac582d32d0d065d461"
---

# Mastering the Multi-selector Tool

The[axiom.ai](https://axiom.ai/) Selector tool allows you to select elements from a page to make use of within your automation. For example, when using the[Enter text](https://axiom.ai/docs/no-code-tool/reference/steps/enter-text) step this will allow you to select the text field that you want to enter text into. There are two versions of this tool depending on which step you are using it in. We are primarily discussing the Multi-Selector tool within this article, but it’s still important to know about the Single-Selector tool.


## Single-Selector tool


The Single-Selector tool will allow you to select a single target within the step that you have initiated the tool from. For example, this tool will open when selecting your target for the “Enter text” step, or the “Click element” step - steps where only a single target is used.


This tool will allow you to also use advanced features such as custom selectors, or using the text of an element as a selector. Setting custom selectors allows you to target more complex elements that the standard selector tool may not recognise.


## Multi-Selector tool


The Multi-Selector tool operates similar to the Single-Selector tool, but has a lot more functionality. This tool allows you to define multiple patterns of selectors to allow the step to identify a wider range of data from the page and multiple different sets of data. We’re going to dive into the features of this tool, and then we are going to give you some tips on really mastering the tool. This tool mostly appears within steps that are designed to scrape data from a webpage.


This tool will appear when you are using steps such as the “Get data from bot’s current page” step where you may wish to scrape multiple pieces of data from a webpage.


There are two key concepts to understand about the tool before making use of it: data types and columns. These two concepts make up the structure of the tool and understanding them ensures that you have the ability to select the elements you need to extract the data that fits your requirements.


## Data types


Selecting the correct data type is one of the first things that you will need to do when creating a new column of data. These data types will determine what data will be targeted by the automation. The data types available within the tool are:


- Text, the text of the element.
- HTML, the HTML of the element.
- Link, the URL behind the element.
- Image, the image within the element.
- Custom selector, a custom CSS selector.


Most data types are pretty self explanatory, the custom selector option being the outlier here - this allows you to write custom CSS selectors for more complex HTML elements that you may wish to use within your automation.


## Columns


One of the best features of the Multi-Selector tool is to be able to select multiple pieces of data. Within the tool itself, you will see the “Add column +” button once you have a data type set for the column before it. This allows you to collect multiple columns of data and output it for use within future steps of your automation.


For example, you would use separate columns if you were scraping a news website and wanted to grab the headline, the sub-heading and the news story itself. When output, this would include the information in separate columns ready to use or write to another service such as[Google Sheets](https://axiom.ai/docs/no-code-tool/integrations/google-sheets) or[Microsoft Excel](https://axiom.ai/docs/no-code-tool/integrations/excel) .


You can mix and match your data types across different columns - Column A may be set to extract text from a page, where Column B may be set to extract a link from the page. This can be useful for extracting lists of links from a page.


## Selecting elements


Understanding how to select elements with the Multi-Selector tool is very important - you can think of them as patterns. When you have the column selected, click on the element within the page that you want to select. Select another element that matches the first and the tool will automatically match any other elements on the page that match the same pattern.


## Using the tool


To get started with the tool, it’s important to understand the data that you are looking to extract. Let’s use a Google Search page as an example.


There are three pieces of information that we want to get from each result that is on this page: the result title, the result description and the URL to the result. Consider each of these pieces of information as a column of data within the Multi-Selector tool. We can create three columns that can be set up as follows:


1. Column A: Set to “text”, select the result titles
2. Column B: Set to “text”, select the result description
3. Column C: Set to “link”, select the result titles - this title contains the link to the result page


Once the automation is run this will output a list of results containing the data items that we are looking for - you can write these to a Google Sheet using the[Write data to a Google Sheet](https://axiom.ai/docs/no-code-tool/reference/steps/write-data-to-a-google-sheet-step) step.


## Wrapping up


Understanding the basic features of the Multi-Selector tool allows you to take advantage of the tool in order to be sure that the elements that you are targeting with your automation are the correct targets.


By setting different data types within the columns that you have created, this allows you to target different data from elements depending on your needs. Setting the column to “text” will allow you to grab the text from the element, but setting this to “link” will allow you to get a URL from the same element.
