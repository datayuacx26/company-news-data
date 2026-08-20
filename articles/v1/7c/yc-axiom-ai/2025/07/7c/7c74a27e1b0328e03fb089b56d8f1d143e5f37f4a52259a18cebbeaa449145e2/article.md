---
schema_version: "1.0.0"
document_id: "7c74a27e1b0328e03fb089b56d8f1d143e5f37f4a52259a18cebbeaa449145e2"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/mastering-xpath-selectors/"
published_at: "2025-07-23T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:24.051508+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:1bb64a6061a17bc341d10d608b7a9b6a83757e35576e0ac869ce59b70f8a4eb5"
---

# Mastering XPath Selectors

[← All blog posts](https://axiom.ai/blog)


# Mastering XPath Selectors


July 23, 2025


## Introduction


You can think of XPath selectors as more powerful CSS selectors, addressing the limitations of standard CSS selectors for complex document navigation. As the web continues to move more towards frameworks that use automatically generated IDs and classes within their code, it has become more difficult to create appropriate CSS selectors that gets the content that you want without a selector that takes 2-3 business days just to read.


This is where xpath selectors come in. XPath (XML Path Language) is a powerful query language that can be used for navigating through nodes and selecting them based on the query. While XML is in the name, this works with HTML where it is often treated as XML (XHTML) or pared into a DOM (Document Object Model) tree.


## Benefits of XPath


XPath selectors allows you to identify and extract specific elements from a webpage document with far more flexibility than standard CSS selectors allow for. For example, xpath allows you to query elements within a webpage based on an attribute, which can be helpful to identify specific elements such as inputs based on an attribute like placeholder. This added flexibility allows you to locate elements based on a wider range of criteria.


On top of the flexibility that XPath offers, it can also be more effective for identifying content on dynamic pages that may change on each load or may not have consistent IDs or classes. In addition to this, it's also possible to partially match selectors to help locate dynamic content.


XPath is a W3C standard, which means that it's well-defined and widely adopted. This means that there is plenty of support out there for the standard and support through various programming languages - for example, this is built into Python libraries such as Scrapy, BeautifulSoup and web automation frameworks such as Selenium, Puppeteer and Playright, to name a few.


One of the biggest benefits that people love, including ourselves, is the ability to target an element based on the text that is contained within that element. This can be super helpful when trying to target an element such as a button, or a link, within a page, like a "Buy now" button.


## XPath Fundamentals


It can sometimes be beneficial to think about xpath queries as file paths as they allow you to specify not just where an element will be in the structure of the page, but also its attributes, content and it's relationship to other elements.


We start off with the standard elements that are available within standard CSS selectors, including` document` ,` <div>` ,` <p>` and all of the usual characters that you'll find in HTML. Attribute nodes are also available, such as the` href` attribute in the` <a>` tag.


When constructing an XPath selector there's a specific format that you are required to use:


- ` /` to start at the root of the document, for example,` /html/body/div` . Or` //` to target anywhere in the document,` //div\[@class='my-element'\]`
- The name of the node, for example,` /html/body/div` which selects all the` div` elements within the` body` node
- Any other element that you wish to target


There are multiple filters that are available within your XPath selector, we are not going to get into them in this article but it's good to be aware of what can be done. We already seen one above with the` @class` filter, but you can also filter by:


- ` @attribute` , for example,` @src`
- By index, if you are targetting a list, for example,` //li\[position() < 3\]`
- By text content using` text()` , for example,` //h2\[text()='Add to cart'\]`


You can combine these filters using the` and` ,` or` , and` not()` logic operators.


## Using XPath with JavaScript


The[document.evaluate()](https://developer.mozilla.org/en-US/docs/Web/API/Document/evaluate) function can be used to bring your xpath queries into your JavaScript code. For example, if you wanted to get all of the add to cart buttons on the page, the following code would work:


```text
const   buttons   =   document.  evaluate  (
"//button[text()='Add to cart']"  ,
null  ,
XPathResult.  ANY_TYPE  ,
null
)


```


You can see that the second argument provided is in the format that was provided in theXPath Fundamentals section, nothing about the formatting changes when you bring it into JavaScript. To iterate through these elements you can use` buttons.iterateNext()` to access each element individually.


## Combining XPath with Puppeteer


If you are using a library such as Puppeteer to run your web automations it also supports xpath. This takes advantage of the` document.evaluate()` function that is native to JavaScript, but has some slight differences on how it's written, for example, the query to select the add to cart buttons would look like:


```text
const   buttons   =   await   page.  waitForSelector  (  '::-p-xpath(//button[text()="Add to cart"])'  )


```


It contains a bit more boilerplate code to get the job done, but works the same - you may wish to skip this and just go straight for the standard implementation. You can then go ahead and use Puppeteers built in options to work with the elements, such as clicking on the button:


```text
for   (  var   i   =   0  ; i   <   buttons.  length  ; i  ++  ) {
buttons[i].  click  ()
}


```


## Using XPath with axiom.ai


While axiom.ai does not currently support the use of xpath selectors as custom selectors, it does offer the ability to use custom JavaScript, and access the Puppeteer library to run your custom scripts. This includes the ability to run your xpath queries in order to extract data from websites manually. You can even return data from the 'Write Javascript' step.


## Using XPath with Python


Interacting with your HTML content with Python and xpath queries is another good choice. This can be done in a similar method that we highlight in our[Web Scraping With Python Tutorial](https://axiom.ai/blog/web-scraping-with-python) - using BeautifulSoup. The only different in this method is making use of` lxml` and` requests` , let's review the code:


```text
import requests
from lxml import etree
from bs4 import BeautifulSoup


html = requests.get("https//axiom.ai")
bs = BeautifulSoup(html.text, "html.parser")
dom = etree.HTML(str(bs))


hero_title = dom.xpath('//h1')[0].text
print(hero_title)


# Output as of 22 July 2025: Browser Automation. Quickly, without code.


```


## Use cases


There are a lot of potential use cases for using xpath selectors with your content, this ranges from page interactions, through data extraction and automated testing.


### Automating add to cart


Using our example above, it's possible to automate the full process of purchasing through an online store. Using the text filter on an xpath selector allows you to directly target the add to cart button that is required in order for you to add an item to the cart - you may run into issues with the checkout requiring some manual intervention but this should be able to automate most of the process.


### Automated testing


Xpath queries can be used to automatically test web pages - for example, it can be used to click through user journeys in order to ensure that key features of your application work as expected, such as the ability to add new products to the card. When combined with a library such as Puppeteer can be a super power for your QA team.


### Data extraction


The most obvious use case for this is data extraction - the process where you automatically extract data from a website in order to save the data for research or analytics. Using xpath queries will allow you to have more flexibility over the elements that you are targetting - especially if you are attempting to extract data from multiple websites that do not have a consistent layout.


## Conclusion


Xpath queries can be used as a great companion to regular CSS selectors and offer an additional layer of flexibility that standard selectors just can't offer. This allows you to create selectors that can easily handle dynamic content, and can handle a great change of structure of web pages that you are looking to interact with. XPath is supported by a wide variety of languages, we have included JavaScript and Python in this example, but the options are pretty limitless, you may just need to search for a specific library that suits your needs.


---


We would love to hear what you do with this information, we would love to hear over in our[community](https://reddit.com/r/axiom_ai) . Got a suggestion on how we can improve this article? Let us know!


[Join discussion on r/axiom_ai](https://reddit.com/r/axiom_ai)


###### On this page


- Introduction
- Benefits of XPath
- XPath Fundamentals
- Using XPath with JavaScript
- Combining XPath with Puppeteer
- Using XPath with axiom.ai
- Using XPath with Python
- Use cases
- Conclusion


By


[Karl Jones](https://axiom.ai/authors/karl-jones) , Technical writer


Karl is a technical writer with axiom.ai with a computer science background and 10+ years of customer support experience. In his spare time he…
