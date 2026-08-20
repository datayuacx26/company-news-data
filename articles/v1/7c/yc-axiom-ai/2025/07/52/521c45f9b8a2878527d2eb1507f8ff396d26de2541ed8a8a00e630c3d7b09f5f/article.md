---
schema_version: "1.0.0"
document_id: "521c45f9b8a2878527d2eb1507f8ff396d26de2541ed8a8a00e630c3d7b09f5f"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/web-scraping-with-python/"
published_at: "2025-07-15T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:24.051508+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:76d097d17a63a6cf091666328dedca6577b124af75cfa3b0d3812d0a5b19253c"
---

# Web Scraping With Python Tutorial

[← All blog posts](https://axiom.ai/blog)


# Web Scraping With Python Tutorial


July 15, 2025


There are many code and no-code methods of interacting with the web automatically - whether that's to extract data from websites, perform tasks automatically, or any other interaction that you can think of. From extracting prices from a website, automatically filling in websites, or keeping track of social media trends automatically, web scraping can help you with the data side of things.


We are going to be looking at Python in this tutorial and how we can take advantage of it to scrape data from a website. We recommend that you have a basic understanding of Python before attempting to implement this as this tutorial will get into the code side of things.


## The process


The process of web scraping can be split into a few different stages.


First, we need to identify the data that we are looking for from the page. This might be something like a price, or a description of a product, or really anything that is present on the page that we are going to retrieve. We are going to need to understand some of the underlying HTML that is used to identify these elements on the page.


*Tip: check out our article[The Best Custom CSS Selectors to Use for Automations](https://axiom.ai/blog/best-custom-css-selectors-for-web-scraping) to help you identify good CSS selectors to use.*


Remember, it's always wise to respect a websites *robots.txt* file in respect to what they allow for scraping and what they don't - this is a common method that sites use to protect their content. You should also consider the legal ramifications on scraping the data, for example, if you are scraping copyrighted content that you intend to use for commerical purposes this may be a legal no-no. We can't offer advice on legal issues but we would recommend reaching out to a legal professional if you are unsure.


## Retrieving website data


We are going to want to start by retrieving the data from the website. We are going to use the` urllib3` library that is installable using the following command:` pip install urllib3` . Once this has been installed we can make use of this to make HTTP requests that we can use in order to retrieve data from the website. This library will keep things simple when making these requests. Let's look at an example:


```text
import urllib3


pool_manager = urllib3.PoolManager()
web_data = pool_manager.request('GET', 'http://www.google.com')


print(web_data.data)


```


We're just printing out the data for now but later in the tutorial we will make use of this data and extract from it. If we want to take this one step further and take advantage of proxies, which is recommended for large scale scraping, we can do the following:


```text
import urllib3


user_agent_header = urllib3.make_headers(user_agent="<USER_AGENT>")
pool_manager = urllib3.ProxyManager('<PROXY_IP>', headers=user_agent_header)
web_data = pool_manager.request('GET', 'http://www.google.com')


print(web_data.data)


```


## Extracting website data


Now that we have the data from the website we can work with it to extract the data that we are looking for. For the time being this data includes everything from the website, which we can't really do much with for the time being - unless that's your goal, if so, you can stop here. There are a couple of methods that we can use in order to extract data from the website data, including usingRegular Expressions or additional libraries such as BeautifulSoup. To keep things simple, we are going to make use of BeautifulSoup as it offers an easier to understand method.


To get started, we will need to install` BeautifulSoup` using the following command:` pip install beautifulsoup4` . We are going to take advantage of the snippet above that we used to retrieve the data to build out a script that looks like the following:


```text
import urllib3
from bs4 import BeautifulSoup


pool_manager = urllib3.PoolManager()
web_data = pool_manager.request('GET', 'http://www.google.com')


soup = BeautifulSoup(web_data.data, 'html.parser')


for link_tag in soup.find_all('a'):
href = link_tag.get('href')
if href:
print(href)


```


In the sample above we are just retrieving the links from the page and printing them, but this can be changed to more complex selectors. We can take advantage of the` select` function available in BeautifulSoup, for example:


```text
# ... including the code above


# Return elements based on classname
classname_text = soup.select('a.my-favourite-class')


# Return based on attribute
attribute_text = soup.select('[data-id="1234"]')


```


You can use this code for all CSS selectors that you can use in the` document.querySelector` function available in JavaScript.


## Real world example


Now that we have some of the basics down it's time to look at a real world example. Let's say that we want to go to rte.ie and download the current news headlines that they are displaying on their homepage. To do this we will first need to retrieve the website data itself, and then we want to extract the headlines from the data. Our code would look something like the following:


```text
import urllib3
from bs4 import BeautifulSoup


pool_manager = urllib3.PoolManager()
website_data = pool_manager.request('GET', 'http://www.rte.ie')


soup = BeautifulSoup(website_data.data, 'html.parser')


for article_titles in soup.select('.article-title span'):
title = article_titles.get('title')
if title:
print(title)


```


Once this has been run, we will end up with a list of headlines being output in the console, as of the time of writing, this is a subset of the results that were retrieved:


```text
New documentary highlights inspiring stories from Irish educators
'I have to have hope' says Tuam relative amid excavation
EU says could target €72bn of goods if tariff talks fail
Over half of Wallace allegations substantiated - report
Five young siblings rescued from sea by off-duty nurses
Status Yellow thunderstorm warning for 14 counties
Reliance on Clifford doesn't bode well for Kerry
Why we made it - The Last Irish Missionaries
Bryan Dobson on the joy of retirement and his advice to Joe Duffy
Local students win national robotics championship
...


```


## Helpful tips


We've compiled some helpful tips that you can use within your scripts to help you get the most out of it.


### Regular Expressions (RegEx)


Regular Expressions, commonly known as RegEx, are patterns that can be used to match data without knowing the exact data. RegEx is commonly used to match data based on the characters it includes, or the format of the text. A common example of this is using RegEx to check if a string of text is an email, checking for things like the @ symbol and that it looks like an email. These RegExs are not very human-readable, but they are readable by the programming language that you are working with, let's look at a basic one for email:


` email_regex = r"^\[a-zA-Z0-9._%+-\]+@\[a-zA-Z0-9.-\]+\\.\[a-zA-Z\]{2,}$"`


This looks like a mess but does check for various important pieces of an email address, including: the @ symbol, the format of the domain and that there are enough characters in it to ensure that it is a valid email. It won't catch everything, but should catch most common emails.


*Tip: LLMs are fantastic for this type of work, try: "Can you create a RegEx for checking valid email addresses in Python?"*


### SSL error


When running this code for the first time, you may experience the following error from urllib3:


` urllib3.exceptions.MaxRetryError: HTTPSConnectionPool(host='<SITE>', port=443): Max retries exceeded with url: / (Caused by SSLError(SSLCertVerificationError(1, '\[SSL: CERTIFICATE_VERIFY_FAILED\] certificate verify failed: unable to get local issuer certificate (_ssl.c:1028)')))`


This most often occurs with macOS ans can be resolved by first installing` certifi` using the` pip install --upgrade certifi` command. Once this has been done, head into your Python installation, often located at` Applications/Python 3.x/` and run the` Install Certificates.command` file (you may not see the` .command` part but that's okay).


Try running your code once more and this should be resolved.


## Wrapping up


Web scraping with Python can be pretty straight forward and requires little experience with Python - depending on how in-depth you want to get with your script, of course. We introduced one method of retrieving data from a website and then extracting data from it using BeautifulSoup and urllib3. We're excited to hear about what you do with this - let us know over in our[community](https://reddit.com/r/axiom_ai) .


[Join discussion on r/axiom_ai](https://reddit.com/r/axiom_ai)


###### On this page


- The process
- Retrieving website data
- Extracting website data
- Real world example
- Helpful tips
- Wrapping up


By


[Karl Jones](https://axiom.ai/authors/karl-jones) , Technical writer


Karl is a technical writer with axiom.ai with a computer science background and 10+ years of customer support experience. In his spare time he…
