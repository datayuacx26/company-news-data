---
schema_version: "1.0.0"
document_id: "58acd5713d80bb241cdced2aff8d70d6253252990fbaa953ebf2fa829ba956e5"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/how-to-retrieve-combine-and-query"
published_at: "2025-05-12T14:28:33+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:dab0dbf3da9d4ab09a48d0bd8faa30f285abe78e1ec11e80dc86d764b7b9dcf0"
---

# How to retrieve, combine & query your saved results with SQL

# How to retrieve, combine & query your saved results with SQL


### Quick methods for accessing and analyzing your automatically stored results from anywhere.


[Expected Parrot](https://substack.com/@expectedparrot)


and[Robin Horton](https://substack.com/@yerkes)


May 12, 2025


When you run a


[survey](https://docs.expectedparrot.com/en/latest/surveys.html) at Expected Parrot the


[results](https://docs.expectedparrot.com/en/latest/results.html) are automatically stored at your


[Coop](https://www.expectedparrot.com/login) account where you can


[view, share and export](https://docs.expectedparrot.com/en/latest/coop.html) them at any time. You can also retrieve them at your workspace to analyze them locally using a variety of


[built-in methods](https://docs.expectedparrot.com/en/latest/results.html#creating-tables-by-selecting-columns) for working with results.


Below we share some quick examples of methods for retrieving multiple sets of results at once, combining them, and analyzing them with SQL queries. This can be useful when you are re-running an experiment with different AI agents and/or language models, or want to compare fresh responses over time.


*Thanks for reading! Subscribe for free to receive new posts and support our work.*


# Retrieving saved results


When you run an


[EDSL](https://github.com/expectedparrot/edsl) survey, details about the survey job and results are displayed in your workspace while the job is running (you can also hide this output by passing


` verbose=False` ):


When the job is completed the results are automatically stored at your Coop account, where you can view, organize, share and export them (a link to Results is in the job status details shown above):


You can retrieve stored results at any time by passing the unique identifier to the


` pull` method:


You can also give the results a unique alias, if desired:


This allows you to retrieve by alias instead:


# Multiple results


If you want to retrieve several sets of results at once you can use the


` list` method to identify and select them. For example, say you reran the survey above multiple times with different models:


You can then specify which results to retrieve, e.g., the 3 most recent:


The


` page` specifies which set of 10 objects to look back to (1 by default, the most recent page) and


` page_size` specifies how many objects to return (10 by default). If you want to search results by survey job status or description instead you can call the method on


` Jobs` (all parameters are optional):


Once you’ve identified the relevant jobs or results you can use the fetch methods for instantiating the results. These are equivalent:


This returns a list of results objects. You can combine them to analyze them together (the columns must be identical):


# Methods for analyzing results


Now all of the methods for working with results are available to us. For example, you can use the


` sql` method to run queries on the combined results:


If you want to share an analysis, or a notebook of code for reproducing it, you can post that to Coop as well:


Please see our


[docs](https://docs.expectedparrot.com/en/latest) for more examples of analytical methods and instructions for


[getting started](https://www.expectedparrot.com/getting-started) . If you don’t see a method that you need, please post a message at our


[Discord channel](https://discord.com/invite/mxAYkjfy9m) or


[send us an email](https://blog.expectedparrot.com/cdn-cgi/l/email-protection#0a63646c654a6f727a6f697e6f6e7a6b7878657e24696567) to request it.


A guest post by


[Robin Horton](https://substack.com/@yerkes?utm_campaign=guest_post_bio&utm_medium=web)


Co-Founder @ Expected Parrot


[Subscribe to Robin](https://yerkes.substack.com/subscribe?)
