---
schema_version: "1.0.0"
document_id: "b4bc049a636ebea69117fba09815ce101fd7d9aa8e8025b4d206e071b80077d7"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-get-yesterdays-date-in-mysql/"
published_at: "2025-01-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:42ee498a59de9ddbf156e4e8cc38ddaf7b7512ac3f48bb494aa6f0d2d3096bbc"
---

# How to Get Yesterday's Date in MySQL

Retrieving yesterday’s date in MySQL requires you to use the` CURDATE()` or` NOW()` functions combined with the` INTERVAL` keyword. This guide will show you how to calculate and manipulate dates in MySQL, focusing on obtaining yesterday’s date in different formats.


## Understanding date functions in MySQL


MySQL provides several functions for date and time operations. The two commonly used for getting the current date are` CURDATE()` and` NOW()` .` CURDATE()` returns the current date without time, while` NOW()` returns both date and time.


### Example: Current date and time


```text
SELECT CURDATE() as CurrentDate, NOW() as CurrentDateTime;
```


## Getting yesterday’s date


To get yesterday’s date, subtract one day from the current date. Use the` INTERVAL` keyword with` CURDATE()` or` NOW()` .


### Example: Yesterday’s date


```text
SELECT CURDATE() - INTERVAL 1 DAY as YesterdayDate;
```


## Formatting yesterday’s date


MySQL’s` DATE_FORMAT()` function allows formatting the date output. For instance, to get yesterday’s date in a specific format, combine` DATE_FORMAT()` with the date calculation.


### Example: Formatted yesterday’s date


```text
SELECT DATE_FORMAT(CURDATE() - INTERVAL 1 DAY, '%Y-%m-%d') as FormattedYesterdayDate;
```


## Use cases in queries


Retrieving records based on yesterday’s date is a common use case. For example, to find all entries from a` logs` table that occurred yesterday:


### Example: Filtering data by yesterday’s date


```text
SELECT *
FROM logs
WHERE DATE(log_date) = CURDATE() - INTERVAL 1 DAY;
```


## Working with time zones


When working with time zones, use` CONVERT_TZ()` in conjunction with date functions. This ensures the date calculations respect the desired time zone.


### Example: Yesterday’s date in a specific time zone


```text
SELECT CONVERT_TZ(CURDATE(), '+00:00', 'America/New_York') - INTERVAL 1 DAY as YesterdayDateInNY;
```


## Conclusion


Manipulating dates in MySQL is straightforward with functions like` CURDATE()` and` DATE_FORMAT()` . Remember to consider time zones in your calculations for accurate results. This guide should help you efficiently work with dates in MySQL, particularly in retrieving and formatting yesterday’s date.
