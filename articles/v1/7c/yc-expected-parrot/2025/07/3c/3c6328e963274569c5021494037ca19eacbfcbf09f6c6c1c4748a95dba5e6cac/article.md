---
schema_version: "1.0.0"
document_id: "3c6328e963274569c5021494037ca19eacbfcbf09f6c6c1c4748a95dba5e6cac"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/bring-your-own-keys-or-mix-and-match"
published_at: "2025-07-18T21:26:41+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:8b1c6b190f346c5455a0803d38226210793a50d5977510a72b0869b8d2bfa96b"
---

# Bring your own keys or mix and match

# Bring your own keys or mix and match


### A key feature of Expected Parrot is that you can always use your own keys for LLMs and human participant platforms, or you can use your Expected Parrot key for unified access.


[Expected Parrot](https://substack.com/@expectedparrot)


and[Robin Horton](https://substack.com/@yerkes)


Jul 18, 2025


Whether you are simulating surveys with AI agents and humans in code with our


[open-source package EDSL](https://docs.expectedparrot.com/) or working interactively with our


[no-code builder](https://www.expectedparrot.com/getting-started/build) , you have flexible options for API access to LLMs and human participant platforms:


-


Use your own keys


-


Use an Expected Parrot key to access all models and participant platforms


-


Mix and match keys for services as desired


# How it works


Your Expected Parrot account comes with a key that lets you run surveys with


[all available language models](https://www.expectedparrot.com/models) and human participant platforms (


[Prolific](https://www.prolific.com/) ). The key is automatically available and can be viewed at the “Keys” page of your account:


If you want to use your own keys for LLMs, scroll down the page to see the option to add keys. You can de/activate keys individually at any time, share access to them with team members, and set limits on their usage. Whenever a private key is activated it is used instead of your Expected Parrot key when you run jobs and this will be indicated in your results:


If you want to let other users run surveys with LLMs using your credits, you can also create temporary access tokens:


# Human respondents


We recently added features for launching your LLM surveys with human respondents. These features are available whether you are working in code with


[EDSL](https://docs.expectedparrot.com/) or using our


[no-code builder](https://www.expectedparrot.com/getting-started/build) (details below). You can choose whether to generate a shareable link for a web version of your survey to send to your own respondents, or you can create studies at Prolific and filter participants as desired (e.g., to align with your AI agents).


Watch how to launch a survey with AI agents and Prolific participants and compare responses:


When you launch studies with Prolific you will also use your Expected Parrot key by default, and transactions will appear in your account the same as with LLM survey jobs. If you instead want to use your own key for Prolific, simply add it to your Keys page as well:


This will ensure that charges are sent to your personal account, but your results will still be available at your Expected Parrot account.


# Working in code


If you’re working directly in EDSL code (in a notebook, instead of our no-code builder), you can call the


` humanize` method to generate a shareable web version of your survey, and use


` Coop` methods for launching Prolific studies and collecting responses. Human results are formatted the same as LLM results, so you use all the same built-in methods for analysis.


Learn more about humanize


[here](https://docs.expectedparrot.com/en/latest/humanize.html) , Prolific studies


[here](https://docs.expectedparrot.com/en/latest/prolific.html) , and analytical methods


[here](https://docs.expectedparrot.com/en/latest/results.html) .


# Questions?


Let us know if you run into any issues! Post a message at our


[Discord](https://discord.com/invite/mxAYkjfy9m) or send an email to


**[\[email protected\]](https://blog.expectedparrot.com/cdn-cgi/l/email-protection)** .


*Thanks for reading! Subscribe for free to receive new posts and support our work.*


A guest post by


[Robin Horton](https://substack.com/@yerkes?utm_campaign=guest_post_bio&utm_medium=web)


Co-Founder @ Expected Parrot


[Subscribe to Robin](https://yerkes.substack.com/subscribe?)
