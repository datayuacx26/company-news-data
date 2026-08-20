---
schema_version: "1.0.0"
document_id: "a1febcd843a1ecb0a786ffbc798f57bdec23d4ba9977bd0a0c0e3938d3ab816f"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/combining-the-power-of-apis-with-browser-automation/"
published_at: "2025-05-02T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:24.051508+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:937cd51975c62236b133f418fcd711c0f5715413cbae66f67f97b68e4f128e38"
---

# Combining the Power of APIs with Browser Automation

Before we dive into how combining the power of APIs with browser automation can upgrade your workflows, let’s dive into what an API is, and what browser automation is.


An API (Application Programming Interface) is used to specify how applications interact with each other - it’s essentially a contract that outlines the requests, responses, and data formats that different applications can use to communicate. For example, when you use your axiom.ai account, we use our API to interact with our database to retrieve your automations and account information.


Browser automation is the process of using software tools to control and interact with web browsers automatically. Instead of manually navigating websites, filling out forms, clicking buttons, and extracting data, these tasks are performed by scripts or specialised software. They’re great at simulating user interactions with the browser.


When you combine these two to create workflows, you can unlock the ability to create dynamic workflows that can be used for a wider range of tasks. For example, if you use an API to trigger and send data to an automation that carries out a browser automation task, you can have this automation behave differently depending on what data is sent.


## More about APIs


You likely use tens of APIs a day and don’t even realise it - they power everything from weather apps, messaging services, social media, smart home products, and so much more. Some services even rely on multiple APIs - when you hit “Run” on an axiom.ai automation, our API is triggered to run the automation and it may then use the Google Sheets API to interact with sheets, if you have the steps in your automation.


They can be used for a wide range of applications, such as retrieving or storing data, triggering tasks, or sending analytics data for the application running the code that interacts with the API.


In all of these cases, the API has laid out a contract that the calling application is required to adhere to to be able to interact with the API. This contract often lays out the HTTP method that is required to be used, as well as the attributes that the API expects, such as an API key, or parameters.


## The axiom.ai API


The axiom.ai API can be used for a variety of tasks with your automations, the most common use of this would be to trigger your automations and send information to them. Triggering your automations via the API allows you to send dynamic data to your automations that allow for more dynamic use cases. The axiom.ai API allows you to include your automations as part of a larger workflow within your organisation, or personal projects.


We’d recommend checking out our[API](https://axiom.ai/docs/developer-hub/api) documentation for more details on getting started.


Currently, the API supports the following functions:


- Trigger an automation
- Check status


A future update to the API is planned and will introduce more features for your automations.


## Combining an API with browser automation


A great example of building a dynamic automation comes in the form of a web form filling automation - dynamic data can be sent to your automation over the API, triggering the automation and navigating to and filling in the web form that you are looking to automate. As part of a larger system, you may have a system that has the ability to trigger a webhook when a change occurs which would allow you to fully automate the process.[Airtable](https://axiom.ai/guides/post-data-to-airtable) and[Supabase](https://axiom.ai/guides/supabase) are examples of services that can do this. A library is also available for automatically triggering your automations from your website, this is not affiliated with axiom.ai but is available on npm


.


Let’s explore some possible use cases.


### CI/CD


Continuous integration and/or continuous deployment is a set of practices used by development and devops teams to automate the software development lifecycle, aiming for faster and more reliable software releases. Teams will create a “pipeline” that is a set of automated tasks that are required to be performed within the lifecycle of an application - for example, building the project from the source repository.


As part of the pipeline, you may wish to perform browser automation, or interact with APIs that may not be supported by the build system that you are utilising. One alternative for this would be to use the axiom.ai API to trigger an automation that can perform additional tasks - for example, triggering a[Zapier Zap](https://axiom.ai/guides/zapier) , notifying your team via[Slack](https://axiom.ai/guides/slack) , creating a new entry in[Supabase](https://axiom.ai/guides/supabase) or[Tadabase](https://axiom.ai/guides/tadabase) and much more.


### Data storage


Building out applications that can handle the storage of data in services such as[Supabase](https://axiom.ai/guides/supabase) or[Tadabase](https://axiom.ai/guides/tadabase) can be difficult when you are building browser automation. Combining the API and browser automation means you can store any data that you need to perform your automation, or any data that is produced during the automation - for example, data that you have scraped from the page.


### Social media


Most social media platforms offer APIs that allow you to retrieve insights from your profiles, or automatically post. For example,[Bluesky](https://axiom.ai/guides/bluesky) provide a very easy to use API that can be used to post to your profile - this can be very useful for taking data from a spreadsheet, such as[Google Sheets](https://axiom.ai/docs/no-code-tool/integrations/google-sheets) or[Excel](https://axiom.ai/docs/no-code-tool/integrations/excel) and pushing this to your account, combine this with axiom.ai[scheduling](https://axiom.ai/docs/no-code-tool/reference/settings/run-options/schedule) and you can have a fully automated social media presence that you can create a backlog for.


Certain CRMs offer an API, this could be utilised within your automations to draw on data that you have about a person before contacting them. For example, you could have a link to their LinkedIn and their name, you could then input this into your browser automation to automatically send them a message like, “Hey Karl, I see you’re still with axiom.ai. How are you finding it there? Do you need any automation solutions?”.


## Wrapping up


Combining APIs and browser automation gives you a great opportunity to add both into larger workflows - leveraging the power of APIs to perform more complex tasks within the context of a browser. This provides you with the opportunity to add additional functionality to your browser automation, or to other tools that you already use. It can be useful for CI/CD, managing data storage and automating social media accounts to level up your marketing or scheduling.


We would love to hear how you use APIs and browser automation (even if it’s not with axiom.ai!) - feel free to hop into our[community](https://reddit.com/r/axiom_ai) and share your ideas!
