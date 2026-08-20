---
schema_version: "1.0.0"
document_id: "b70e11d50d12bffc9c0c2c274b68f384984660b2d9ae08ff85a1d779e575623e"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/web-scraping-without-getting-blocked/"
published_at: "2025-06-03T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:24.051508+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:d92d716719581c0f62953f3628dfe533d6d80ff5e3af565a9a5b44a8bd8a6f01"
---

# Web Scraping Without Getting Blocked

## Introduction


Web scraping, also known as web crawling, is the act of scraping data from a website by downloading and parsing the HTML to extract data. Search engines often do this to be able to surface search results. Often it's easier to find an API that can provide this data to you - this is because it already comes in a machine readable format that is much easier to store and manipulate.


We've seen numerous use cases where[axiom.ai](https://axiom.ai/) has helped individuals and businesses automate the scraping of web pages to help them with:


- Competitor monitoring
- Price monitoring
- Lead generation
- Research dataset collection


Scraping data from the web can be a powerful tool for generating datasets to be later used in wider applications and there are various tools out there that can help you achieve your goals. However, the downside of web scraping (with any of these tools) is sites generally don't like bots scraping their content and will put active blockers in your path, such as:


- CAPTCHA - where you may need to solve a puzzle to confirm you're human.
- CloudFlare verification - a verification tool that will only let you enter a site once it's confirmed you're human (or can convince them you are!).


Let's dive into some things that you can do to unblock yourself.


## Proxies


> A proxy is a server that acts as an intermediary for requests between clients and servers.


Using a proxy allows you to determine various factors on how your activity is perceived by the server that you are requesting resources from - this includes when you're just visiting a site. When making a large number of requests to a specific server, it's possible that they recognise the traffic and block the IP that you are accessing the site from. Using proxies allows you to get around this as it routes your traffic through an IP address that you can change if it gets blocked - this can allow your connection to appear as if it's coming through multiple sources, lessening the risk of being recognised.


Residential proxies are IP addresses that are generally provided to residential connections by ISPs, which means that they are less likely to trigger alarms as they are considered a safer connection - just like you may be accessing this article from! Stealth proxies are also available and are designed to be difficult to recognise as a proxy. Data center proxies are also available, but may be at a higher risk of identification - however, these may be cheaper to purchase. Choosing the right proxy type will depend on the amount of traffic and the level of security/spoofing that you require for your project.


If you have a large scale project, consider rotating your proxies. This practice allows you to have a single script that can rotate between various proxies. This dramatically reduces the risk of your proxies being blocked as if done right this will distribute the traffic between any proxy that you have access to within your script. This can also help you recover if one of the proxies is blocked.


## Headless


> A headless browser is a lightweight browser that lacks a user interface, primarily used for automated testing and web scraping.


While headless browsers lack a user interface, their ability to be controlled programmatically is their real superpower. They can render webpages, interact with them and execute JavaScript as it would be when run on a non-headless browser. Combined with a library like Puppeteer or Selenium this allows you to write scripts to control a headless browser - whether this is for testing or for web scraping. These libraries allow for user interactions, page navigations, handling cookies and other complex tasks like executing their own JavaScript scripts on pages.


The one downside of using a headless browser for testing and web scraping is the lack of user interface - this makes debugging more difficult and we would recommend that you add robust error handling within your scripts to avoid being in the dark when things do go wrong while testing.


## CAPTCHA Solving


> CAPTCHA is a tool that website administrators use to avoid spam or protect login/registration forms. There are many types of CAPTCHA that present puzzles to the website visitor.


We all know the dreaded "I'm not a robot" prompt that appears from time to time to check if you're human - CAPTCHA will stop your scripts in their tracks unless you account for it during your script runs. There are various services available that can help you solve CAPTCHAs on webpages when operating your browser programmatically -[2Captcha](https://2captcha.com/) , for example. It's worth noting that these services do not always have the ability to solve more complex CAPTCHA, or even custom ones that are created by the organisation who owns the site that you are looking to automate.


## Storing cookies and local storage


Sometimes, the best way to get around CAPTCHA prompts is to avoid what causes them to appear. CAPTCHA will often appear in login/registration forms and can prevent you from continuing with the script execution. Storing cookies and local storage means that you can carry over an authentication session into your scripts meaning that you can skip the login process completely and avoid the CAPTCHA showing. This has the benefit of speeding up your scripts.


## APIs


> An API (Application Programming Interface) is a set of rules and protocols that allows different software applications to communicate and exchange data with each other.


APIs are used all around us - websites, apps, and operating systems use them for a wide range of applications. Let's say you open the weather app on your phone, the app will reach out to the services API to retrieve the most up-to-date weather data to show it to you.


Using APIs to access the data that you are looking for tends to be a better path when you are looking to extract data from a website - however, finding an API that has the data that you need can be difficult as they are not always officially publicly available. Using an API provides an already formatted dataset that can be used within your script easier than scraping the data from a webpage.


As an example of this, you can check out the following website and compare scraping it via a script versus retrieving it using the API:[https://jsonplaceholder.typicode.com/posts](https://jsonplaceholder.typicode.com/posts) .


## Rate limits


> A rate limit is a control mechanism that defines the frequency or number of requests a user or client can make to a server or API within a specific period of time.


Rate limits apply to most APIs in order to protect them from attacks that may render the service unavailable to other users. We use it here at axiom.ai to protect our API. Understanding an APIs rate limit, or a services rate limit, is important to prevent you hitting the ceiling when visiting the website. Rate limits are often based on a per minute basis - for example, for axiom.ai this is 100 requests per minute.


## Bypass Bot Detection


This is an axiom.ai specific option - the ability to automatically bypass the Cloudflare verification screen. Cloudflare aims to protect sites from DDoS attacks and ensure that they remain available. This may be thought about as a more advanced CAPTCHA that can't be worked around with standard methods. When your script hits this roadblock it will not continue to run and will likely run into an error, or get stuck running. axiom.ai offers a[Bypass Bot Detection](https://axiom.ai/docs/no-code-tool/reference/settings/chrome/bypass-bot-detection) option that allows your scripts to get around these roadblocks.


## axiom.ai


While most roadblocks above can be solved using libraries and services that are available to add into your web scraping scripts, axiom.ai has a lot of these features built into it that can help you get going quickly. We have documentation on getting around all of these roadblocks:


- [Proxies](https://axiom.ai/docs/no-code-tool/reference/settings/run-options/proxy)
- [Headless](https://axiom.ai/docs/no-code-tool/reference/settings/run-options/headless)
- [CAPTCHA Solving](https://axiom.ai/docs/no-code-tool/integrations/2captcha)
- [Bypass Bot Detection](https://axiom.ai/docs/no-code-tool/reference/settings/chrome/bypass-bot-detection)
- [Store cookies and local storage](https://axiom.ai/docs/no-code-tool/reference/settings/run-options/store-cookies)


axiom.ai does not directly support APIs, but they can be used within theWrite JavaScript step that is available.


## Wrapping up


Encountering these roadblocks can seem like the end of the path when you first encounter them, but that's not the case! Getting around these roadblocks is actually quite easy if you know some of the tools that are available. From using proxies to using CAPTCHA solving services, there are method of getting around these bumps in the road.


Have any other methods that you like to use in your scripts?[Let us know!](https://reddit.com/r/axiom_ai)
