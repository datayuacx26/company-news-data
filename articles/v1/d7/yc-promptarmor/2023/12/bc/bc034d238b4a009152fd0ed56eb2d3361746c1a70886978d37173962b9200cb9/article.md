---
schema_version: "1.0.0"
document_id: "bc034d238b4a009152fd0ed56eb2d3361746c1a70886978d37173962b9200cb9"
company_key: "yc-promptarmor"
company: "PromptArmor"
source_id: "yc-promptarmor-rss-057c96aa2b5f"
canonical_url: "https://promptarmor.substack.com/p/data-exfiltration-from-writercom"
published_at: "2023-12-15T14:29:24+00:00"
first_seen_at: "2026-07-25T19:53:47.024087+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:c63a4996a0deda5600f1cc04ce09e23ad348a382f8b5602b38312baf7117ed3b"
---

# Data exfiltration from Writer.com with indirect prompt injection

# Data exfiltration from Writer.com with indirect prompt injection


### Authors: PromptArmor and Kai Greshake


[PromptArmor](https://substack.com/@promptarmor)


Dec 15, 2023


*This vulnerability can allow attackers to steal a user’s private documents by manipulating the language model used for content generation. As of now, it has not been fixed as it was not triaged as a security vulnerability by Writer.com after disclosure (more details in Responsible Disclosure section at the end). Edit 12/19/2023: It is no longer possible to render markdown images or clickable links in Writer.com* .


(the attack, disguised in white text on an attacker controlled website)


---


Writer.com is an application that can be used by enterprises and consumers alike. Users can upload data files, share links, and ask questions in order to generate tailored content for their business needs. It has access to your brand and knowledge base and as such can maintain consistency when writing articles for you. They emphasize its data security given the sensitivity of information its clients upload throughout its website:


[https://writer.com/product/data-security-privacy/](https://writer.com/product/data-security-privacy/)


(screenshot from writer.com/security on Dec 13)


(screenshot from writer.com on Dec 5)


### **1. The vulnerability:**


In Writer, users can enter a ChatGPT-like session to edit or create their documents. In this chat session, the LLM can retrieve information from sources on the web to assist users in creation of their documents. We show that attackers can prepare websites that, when a user adds them as a source, manipulate the LLM into sending private information to the attacker or perform other malicious activities.


The data theft can include documents the user has uploaded, their chat history or potentially specific private information the chat model can convince the user to divulge at the attacker's behest.


This type of attack is called


[indirect prompt injection](https://arxiv.org/abs/2302.12173) , initially coined by Kai Greshake.


To prove the feasibility of such an attack, we uploaded a file that contains mocked sensitive information (SSN numbers, revenue figures, salary information), and were able to exfiltrate all of it:


(screenshot from our pentesting exfiltration server)


The website that hosts the payload looks like any other website, and the payload is hidden from any user visiting it. In the following screenshot, the hidden text of the payload is highlighted (it has white font, but there are other methods to hide it or embed payloads on other websites such as social media platforms).:


(screenshot from the website with the injection)


Note that *.cloudfront.net is one of the locations allowed by the CSP.


### 2. A Complete Attack Chain


A typical user use case would be the following:


However, here’s what actually happens in the background with the injection


A) They ask Writer to write a report for them based on some sources and some data they upload.


(screenshot from our chat session at writer.com)


B) They find a nice source on the web which has the information they need


C) They upload some sensitive data


D) They get Writer to write the report


(screenshot from our chat window at writer.com)


E) Writer reads the webpage, but it contains a hidden injection in small white text:


F) Writer follows the instructions, and overrides the initial instructions of the user and any security filters Writer.com has enforced. The user never asked for this image and it was not on the webpage that they initially asked for. Nevertheless, Writer.com has automatically rendered the attacker-controlled image in markdown and you can see in the network activity that it has appended the contents of the uploaded client data file to the HTTP parameters, just as instructed:


(screenshot of our chat session at writer.com with network activity expanded)


Here’s a side by side comparison between the uploaded client data file, and a zoomed in image of the HTTP parameters from the above screenshot:


G) Without their knowledge, their data has now been exfiltrated to the attacker’s server. Rendering the image in markdown automatically created a GET request with the HTTP parameters including the content of the file. The attacker can read their logs to extract the sensitive client data from the file.


(screenshot from pentesting exfiltration server logs)


Rendering an image to exfiltrate data is only one method of exfiltrating data. Note that while, to our knowledge, Writer does not use OpenAI for text generation,


[OpenAI has said the same issue in their system is a “won’t fix.”](https://embracethered.com/blog/posts/2023/chatgpt-webpilot-data-exfil-via-markdown-injection/#:~:text=Responsible%20Disclosure)


Please see below for other example attacks which use other mediums (like links) to exfiltrate data.


### Additional Examples:


##### Example 1: Exfiltration of uploaded files


In this example, an attacker is able to exfiltrate a confidential file that the user uploads via a link, using this injection:


[Video explanation (sent with disclosure)](https://www.loom.com/share/d8ffe66d9dfb44429be8a60182372f38)


##### Example 2: Exfiltration of chat history


In this example, an attacker is able to exfiltrate the chat history from a user via a link, using this injection:


[Video explanation (sent with disclosure)](https://www.loom.com/share/678d76f03c5244a1aee592ba41e744dd)


### Putting this in context:


These type of attacks have been done in other LLM surfaces, such as the


[Bard attack by Thacker, Rehberger and Greshake](https://embracethered.com/blog/posts/2023/google-bard-data-exfiltration/) , which was resolved promptly by the Google Security and Bard team.


For more information on these attacks and relevant information here are some great sources:


-


[https://kai-greshake.de/](https://kai-greshake.de/) (twitter: @KGreshake)


-


[https://embracethered.com/blog/index.html](https://embracethered.com/blog/index.html) (twitter: @wunderwuzzi23)


-


[https://josephthacker.com/](https://josephthacker.com/) (twitter: @rez0_)


-


[https://promptarmor.com/](https://promptarmor.com/) (twitter: @promptarmor)


And to learn more about LLM security risks feel free to check out:


-


[https://owasp.org/www-project-top-10-for-large-language-model-applications/](https://owasp.org/www-project-top-10-for-large-language-model-applications/)


-


[https://atlas.mitre.org/](https://atlas.mitre.org/)


Responsible Disclosure Timeline


-


Nov 29: We disclose issue to CTO & Security team with video examples


-


Nov 29: Writer responds, asking for more details


-


Nov 29: We respond describing the exploit in more detail with screenshots


-


Dec 1: We follow up


-


Dec 4: We follow up with re-recorded video with voiceover asking about their responsible disclosure policy


-


Dec 5: Writer responds “We do not consider this to be a security issue since the real customer accounts do not have access to any website.”


-


Dec 5: We explain that paid customer accounts have the same vulnerability, and inform them that we are writing a post about the vulnerability so consumers are aware. No response from the Writer team after this point in time.


-


Dec 15: This post is published on Hacker News


-


Dec 19: We are informed that this particular data exfiltration method (markdown image rendering and clickable links) is no longer viable.


See more research from PromptArmor’s threat intelligence team at promptarmor.com/#threatintel, and from Kai Greshake at kai-greshake.de/about


---


###### Disclaimer: The content of this blog is intended solely for research and educational use, aimed at enhancing knowledge, understanding, and awareness regarding attacks and their countermeasures to bolster the security of Large Language Models (LLMs)
