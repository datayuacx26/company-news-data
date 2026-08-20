---
schema_version: "1.0.0"
document_id: "726ce42107b5b9c8f9b8aeffdb89b41ad03cbe071dc41dc0ca0252451ab8f31f"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/sql-maxis-how-to-get-a-character-from-a-codepoint-in-spark-sql/"
published_at: "2025-11-04T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:538215fffa06a977de3aac81dfe035050b39ec126d5dde84f8c72104f858f4a6"
---

# SQL Maxis: How to get a character from a codepoint in Spark SQL

At Prequel, we are obsessed with data integrity. It’s our whole thing: we move data between various systems, and our value prop is that we’ll do it fast and never miss a row. To ensure that’s the case, we’ve built an in-house data integrity capability that allows us to identify discrepancies between tables in different database systems. We use this in our tests and even expose it as a feature for our customers to leverage. This obsession means we’ve spent a *lot* of time testing the weird edge cases of data transfer between different systems. And let me tell you, the world of data systems is full of… *opinions* .


For example, we learned the hard way that you absolutely must test the full range of Unicode characters. Not all systems (or their drivers) are created equal. Postgres, for example, will famously barf if you try to put a null byte (` \\u0000` ) in a string. Some drivers get very creative with how they (mis)handle emoji.


This fixation on cross dialect integrity is also why we built our own universal hashing function. We needed a way to hash tables in two different databases and compare the results to spot inconsistencies. The problem? There is no single hash function that’s universally available. MD5 is close, but it’s not *everywhere* .


So, we built our own. Ours relies *only* on functions defined in ANSI SQL-92, which means we can run it in every single SQL database we support. This “run everywhere” philosophy means we’re constantly pushing the boundaries of what different SQL dialects can do.


And that brings us to a fun little discovery in Spark SQL.


## The Problem: CHR() vs ASCII()


When testing our character sets in Spark, we noticed a strange asymmetry.


If you have a character, you can get its decimal codepoint using the` ASCII()` function. And it works perfectly for *everything* , including emoji.


Logically, you’d assume the inverse function,` CHR()` , would take that decimal value and give you the character back. You would be wrong.


Why? We dove into the Spark documentation and found this gem:


> If n is larger than 256 the result is equivalent to` chr(n % 256)` .


That means` CHR(129424)` is evaluated as` CHR(129424 % 256)` , which is` CHR(144)` . This function is stuck in the ASCII/Latin-1 world.` ASCII()` is living in 2025, but` CHR()` is stuck in 1995.


So how do we get our shrimp?


## The Solution: Build It Ourselves


We’re SQL Maxis. We’re not going to be stopped by a missing function. If Spark won’t give us a proper` CHR()` function, we’ll build one ourselves. And we’ll build it out of pure, organic SQL.


The plan is simple:


1. Start with the decimal codepoint (e.g., 129424).
2. Manually implement the UTF-8 encoding algorithm using bitwise operators (` >>` ,` &` ,` |` ) to figure out the 1, 2, 3, or 4 bytes required.
3. Convert each of those byte’s integer values into a hex string using` HEX()` .
4. ` CONCAT()` those hex strings together (e.g.,` 'F09FA690'` for our shrimp).
5. Use` UNHEX()` to turn that hex string into raw binary.
6. Use` DECODE()` to interpret that raw binary as a` 'UTF-8'` string.


Drop this in a query, and you can generate any Unicode character you want, starting from *any* valid decimal codepoint.


And there you have it. With this snippet you will get the emoji you asked for. And unlike ChatGPT, you’ll get an actual error when you try to ask for a seahorse emoji.
