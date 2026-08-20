---
schema_version: "1.0.0"
document_id: "cf52eb856c930d1fdd1807a3f30590e75cf993a0c4396bfd8150c10873327af1"
company_key: "similarweb-ltd-ordinary-shares"
company: "Similarweb Ltd."
source_id: "similarweb-ltd-ordinary-shares-rss-2bd7dd1f88a6"
canonical_url: "https://medium.com/similarweb-engineering/pyspark-pitfalls-a-comedy-of-errors-and-how-to-dodge-them-b26e14ff987c"
published_at: "2023-07-10T15:57:25+00:00"
first_seen_at: "2026-07-20T23:19:16.581763+00:00"
fetched_at: "2026-07-28T22:26:24.228240+00:00"
content_hash: "sha256:8e1f5d83e6bbe4c3492d63eb09875d743eb8e616eb704dbedf48d2f0c515049a"
---

# PySpark Pitfalls: A Comedy of Errors and How to Dodge Them

# PySpark Pitfalls: A Comedy of Errors and How to Dodge Them


[Dor Amram](https://medium.com/@doramram210?source=post_page---byline--b26e14ff987c---------------------------------------)


4 min read


·


Jul 5, 2023


--


Press enter or click to view image in full size


Me waiting for my query to finish


If you’ve ever danced with PySpark, you know it can be like tangoing with a hungry bear. While it can be a powerful partner, if you step on its toes, you’re in for a wild ride. Buckle up, as we traverse the “sparkling” landscape of PySpark mishaps, and learn how not to end up as the comedic relief in your own coding journey.


### Misusing Collect — A Memoir of Lost Memory


Picture this: It’s late at night, and you’ve just run your PySpark job. Suddenly, the silence is shattered by the howling of your computer, begging for mercy. You’ve used ‘collect()’ to get all the elements of a DataFrame, only to realize that you’ve just tried to cram a terabyte-sized monster into your laptop’s memory.


Avoid this disaster with actions like ‘take()’, ‘first()’, or ‘count()’. Only use ‘collect()’ when necessary, and when you’re sure your machine can handle it. Here’s an example:


Press enter or click to view image in full size


### Ignoring Data Partitioning — A Tale of Endless Shuffling


Not partitioning your data in PySpark is like trying to find your favorite book in a library where books are randomly scattered. You’ll end up running around (or in Spark’s case, shuffling data) until you’re out of breath.


Do your Spark job a favor and arrange those ‘books’ with data partitioning:


Press enter or click to view image in full size


Now, Spark knows exactly where to find the data it needs, saving you time and computational resources.


### Overusing Python UDFs — The Tortoise and the Hare Redux


PySpark allows you to use Python User Defined Functions (UDFs), which feels like home for Pythonistas. But remember the tale of the Tortoise and the Hare? In this version, Python UDFs play the slow-and-steady tortoise. However, unlike the classic fable, the hare (PySpark’s built-in functions) gets the job done faster and doesn’t nap on the job.


Consider using PySpark SQL’s built-in functions over Python UDFs, like so:


Press enter or click to view image in full size


### Neglecting Broadcast Variables — Sharing is Caring


In PySpark’s world, sharing variables is akin to handing out flyers. By default, PySpark hands out a flyer (copies of a variable) to every worker for each task. If you’re dealing with a hefty variable, that’s a lot of wasted paper (network bandwidth).


Broadcast variables come to the rescue like a Spark superhero, giving each worker one copy of the ‘flyer’, saving on resources:


Press enter or click to view image in full size


### The Misadventures of Window Functions — A Window with A View


Window functions in PySpark are a fantastic tool, offering insights on data with respect to a specific frame or ‘window’ of data. But just like that tempting open window on a summer day, it can let in a swarm of bugs if not used properly.


Suppose you’re working with a DataFrame of daily sales and you want to calculate a running total. You might decide to use a window function to get the job done. However, if you neglect to specify the window frame, you’ll get unexpected results.


Press enter or click to view image in full size


At first glance, this looks okay. But there’s a catch! The ‘orderBy()’ in the window definition sorts the data, but without a specified frame, it calculates the ‘running_total’ from the first row to the current row. Not exactly a “running” total, more like a “stumbling” total.


To get a proper running total, you need to specify the frame. In this case, the frame is all rows between the start of the DataFrame and the current row:


Press enter or click to view image in full size


Now that’s a running total that would make Barry Allen proud!


### Conclusion


Remember folks, PySpark is like a wild horse — majestic and powerful, but it’ll buck you off if you’re not careful. Navigate through the PySpark wilderness with caution, respecting its unique quirks and features. When in doubt, remember these comedic tales and their lessons. After all, you wouldn’t want to become the next comic strip in the PySpark universe, would you? Happy Sparking and avoid the pratfalls!
