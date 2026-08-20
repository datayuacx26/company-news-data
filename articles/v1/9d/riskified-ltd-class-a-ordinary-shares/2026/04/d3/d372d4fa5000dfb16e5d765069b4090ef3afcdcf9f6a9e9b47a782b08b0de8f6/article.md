---
schema_version: "1.0.0"
document_id: "d372d4fa5000dfb16e5d765069b4090ef3afcdcf9f6a9e9b47a782b08b0de8f6"
company_key: "riskified-ltd-class-a-ordinary-shares"
company: "Riskified Ltd."
source_id: "riskified-ltd-class-a-ordinary-shares-rss-dd7d0cc56e2d"
canonical_url: "https://medium.com/riskified-technology/data-exploration-for-software-engineers-evaluating-and-integrating-external-datasets-da81d3e67b4d"
published_at: "2026-04-13T11:57:04+00:00"
first_seen_at: "2026-07-20T23:18:31.853064+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:d370b1fd294291ed57635841fe423cb09488cbc2c86dca572842d5e4d81a0810"
---

# Data Exploration for Software Engineers: Evaluating and Integrating External Datasets

Press enter or click to view image in full size


Software Engineering


Data Exploration


Dataset


Databricks


Data Engineering


# **Data Exploration for Software Engineers by Example: Evaluating and Integrating External Datasets**


## Walk through a real-world example of evaluating external datasets and integrating one of them


[Igor Shurmin](https://medium.com/@igor.shurmin?source=post_page---byline--da81d3e67b4d---------------------------------------)


9 min read


·


Apr 13, 2026


--


I work on the team responsible for various analytical systems at Riskified, and we have a project with a database that stores addresses. These addresses consist of country, state, street, house number, and apartment — essentially all necessary values to identify a household in the United States.


At some point, I faced the task of expanding our database with an external dataset, which required me to apply some data exploration techniques. I’m not a data scientist, and my background is mostly pure software engineering. This kind of work felt unusual to me, and I thought it would be interesting to share.


This article walks through a real-world example of evaluating external datasets and integrating one of them. Whether you’re a software engineer who doesn’t typically work with data exploration or someone looking for a repeatable methodology to assess external datasets, this article provides a practical framework. I’ll show how I approached this task, what I discovered, and how I integrated the external dataset.


## What We Had in the Beginning


In one of our internal systems, we have a database of street addresses from the United States. At some point, we decided to expand the existing dataset to cover more addresses and considered external candidates, whether commercial or free.


We noted the existence of commercial datasets like the US Master Address List and Global Address Database, each containing over 210 million US addresses. Still, we focused our research on freely available options because we quickly identified two promising open datasets for U.S. addresses: the[National Address Database (NAD)](https://www.transportation.gov/gis/national-address-database) from the U.S. Department of Transportation and[OpenAddress](https://openaddresses.io/) , a community-driven collection of address data from various open sources.


I’ll give you more context on our existing dataset and the infrastructure around it: we store addresses for quick access as a key-value set. “Source of truth” is a dataset in S3 that we can update and propagate to the key-value database. We use Databricks on it, which lets us easily explore and transform the data.


## How I Analyzed External Datasets


For each external dataset, I followed a consistent analysis approach: understanding the format and structure, calculating key statistics, assessing data quality, and comparing it with our existing dataset. This methodology ensures we can make informed decisions about which datasets provide genuine value.


### National Address Database (NAD)


This dataset was created by the U.S. Department of Transportation. With few exceptions, it is mostly aggregated from state data, which in turn is aggregated from local data provided by city halls, for example. Coverage is partial — some states are not covered at all (this and all following information is valid for August 2024).


Press enter or click to view image in full size


New versions were coming out every several months, and the format was a zipped CSV file with headers and 60 columns.


First of all, I downloaded the whole dataset to my laptop and quickly reduced it with awk by keeping only the necessary columns, like this:


```text
awk 'BEGIN {FS=","; OFS=","} {print $3,$5,$14,$21,$25,$34,$35}' NAD_r17.txt > NAD_r17_reduced.txt
```


This gave me a plain 3.5 GB CSV file that I could upload to S3 to make it accessible in Databricks. Here is a snippet from the notebook:


```text
SELECT * FROM read_files(    's3://…/NAD_r17_reduced.txt',     format => 'csv',     header => true  ) LIMIT 100
```


With Spark powers unleashed, I filtered out:


- rows with null/empty values in each of the columns,
- rows with state code length different than 2 (the US has only two-letter state codes),
- rows with ZIP code length different than 5 (the US has five-digit ZIP codes).


Also, for each column, I got the top-100 values to see if there are any patterns to filter on — for example, I found that many values in the city field are equal to “Not stated”.


This cleanup left me with 90% of the dataset, or about 72 million addresses.


Comparing with the existing dataset required me to look into the formatting of the street field:


> existing “e 10th ave”, NAD “East 10TH Avenue”


Clearly, I had to normalize the data in the external dataset first to compare it with the existing one by simplifying street suffixes (helpful mapping is[here](https://pe.usps.com/text/pub28/28apc_002.htm) ), renaming “East” to “e”, “West” to “w”, etc.


Exact rules were specific to our database, I won’t list all of them — after all, finding an intersection left me with 79% percent of the filtered NAD dataset (this is the amount of “new” addresses, about 57 million, which is 71% of the original dataset). I summarized the impact in a diagram:


Press enter or click to view image in full size


- 90% of the NAD dataset was left after cleaning
- 71% of the dataset was left after intersecting with the existing dataset
- Integrating NAD potentially increases our addresses dataset by 55%


Ok, let’s look at another dataset!


### OpenAddress Dataset


This worldwide dataset of addresses is collected by volunteers from various open data sources. It has partial coverage of the US and an unknown quality of data.


Press enter or click to view image in full size


Data is updated regularly; you can follow commits in their[GitHub repository](https://github.com/openaddresses/openaddresses/commits/master/) . The downloadable format was JSON with fields including number, street, unit, city, district, region, and postcode, grouped in folders by province (the “state” in the US).


Again, I decided to transform the data locally to make it accessible in Databricks, bringing it to plain CSV. Due to JSON and subfolders structure, I came up with a small bash script:


```text
echo 'province;number;street;unit;city;district;region;postcode' > 'us.csv'  for folder in $(ls -d */ | xargs -n 1 basename); do   echo "Processing folder: $folder"   cat ./"$folder"/*addresses*.geojson | jq --arg folder "$folder" -r '[$folder,(.properties.number),(.properties.street),(.properties.unit),(.properties.city),(.properties.district),(.properties.region),(.properties.postcode)] | join(";")'  >> 'us.csv'  done
```


After uploading the resulting 9 GB file to S3 and accessing it via Databricks notebook, I filtered the data the same way I did with the NAD dataset and was left with 69% of the data, or about 163 million addresses. Obviously, there was a lot of garbage in the data, but anyway, the amount of data was huge.


By the way, Databricks provides a nice visualization for the US data by state:


Press enter or click to view image in full size


You can use it to see data “density” and compare it to the existing dataset, for example, to observe the difference state-by-state.


To intersect with the existing dataset, I checked the formatting for a few addresses. Here is one example:


> existing “e 10th ave”, OpenAddress “E 10TH AVENUE”


You see, it’s different from both the existing data and the NAD dataset, where this address is stored as “East 10TH Avenue”?


Applying the same normalization techniques as before, I was able to intersect the datasets, leaving me with only 56% of the filtered dataset (or 38% of the original). However, due to its size, the impact on our existing dataset was higher anyway!


Press enter or click to view image in full size


- 69% of the OpenAddress dataset was left after cleaning
- 38% of the dataset was left after intersecting with the existing dataset
- Integrating OpenAddress potentially increases our address dataset by 88%


## Comparing Datasets and Making a Decision


After analyzing both datasets, I decided to check how they correlate and intersect:


Press enter or click to view image in full size


This showed me that the OpenAddress dataset was indeed richer and covered more unique addresses than the NAD dataset. I was almost convinced to integrate the dataset, but felt that I needed one last sanity check of the data. Throughout the analysis, I looked into the values of addresses, of course, but it’s not humanly possible to review all the millions of them!


Our database key consisted of state, ZIP, and house number concatenated, like “US_NY_10001_220”. So every key corresponds to an array of addresses. It was interesting to me to check some statistical characteristics of arrays and what they look like. I picked the following metrics to describe the distribution:


- Minimum number of addresses for a key — I’d expect to see 1 here
- Mean, or in other words, arithmetic average
- Standard deviation
- Median or 50% percentile
- 90%, 95%, and 99% percentiles


Databricks with Spark SQL provides nice instrumentation to check these metrics:


```text
WITH g AS (SELECT key, size(addresses) AS cnt FROM dataset)   SELECT     min(cnt), max(cnt), mean(cnt), std(cnt), percentile(cnt, array(0.5,0.75,0.9,0.95,0.99))   FROM g
```


Here is the final distribution profile for each of the datasets:


Press enter or click to view image in full size


Just by comparing these numbers, I concluded that OpenAddress provides more addresses per key, meaning it potentially stores more street names and unit indicators (like apartment numbers). More importantly, it doesn’t stand out by any unusual metric values that would be hard to explain.


One could think about more granular and sophisticated data distributions, for example, group them by state, ZIP code, or house number (which is a distribution by itself!). Data exploration is indeed exciting, and you could come up with many interesting ways to “understand” the data.


## Integration Efforts and a Useful Data Partitioning Trick


Would integration efforts be different for NAD and OpenAddress? Actually, no, because I already converted both datasets to plain CSV files with the same columns. After incorporating the chosen dataset (OpenAddress), we could easily repeat the same steps with NAD, adding more unique addresses to our database!


Remember, I mentioned in the beginning that we had a “source of truth” layer of our data in S3, available for the distributed processing in Databricks? In terms of external dataset integration, we just had to “merge” it into the existing dataset. I won’t go into the details — the actual code is some boring Spark transformations — but I would like to share one technique that helps divide a big dataset into smaller chunks of roughly similar size.


It is called equi-depth (or histogram-based) range partitioning, splitting data based on the actual distribution rather than arbitrary fixed boundaries. Taking street names, for example, if you naively split addresses by first letter (A-D, E-H, etc.), you’d get wildly uneven buckets because some prefixes are far more common than others: streets starting with “MA” (think of “Main Street”) outnumber those starting with “QU” or “ZZ”. Instead, you apply three steps:


**(1)** Profile the distribution — count how many records fall under each two-letter prefix, you could sample 20% for efficiency.


```text
SELECT     UPPER(SUBSTRING(street, 1, 2)) AS first_letters,     COUNT(*) AS cnt   FROM streets TABLESAMPLE (20 PERCENT)   GROUP BY UPPER(SUBSTRING(street, 1, 2))   ORDER BY first_letters
```


**(2)** Compute cumulative percentiles — the running total divided by total count tells you what percentage of the dataset you’ve “passed through” at each prefix. For example, by the time you reach “MA”, you might be at 45% of all records.


```text
SELECT     first_letters,     cnt,     SUM(cnt) OVER (ORDER BY first_letters) AS running_total,     SUM(cnt) OVER () AS total_count   FROM first_letters_and_counts
```


**(3)** Map percentiles to buckets — for example, multiply that fraction by 6 and round to assign each prefix to one of 6 buckets. Prefixes at 0–16% go to bucket 0, 17–33% to bucket 1, and so on.


```text
SELECT     first_letters,     cnt,     running_total,     ROUND((running_total / total_count) * 6) AS bucket_number   FROM running_totals   ORDER BY first_letters
```


This way, you could work with each bucket of data separately, uniformly spreading the load — nice trick!


## A Repeatable Strategy for Assessing External Datasets


Based on this exploration, I came up with the framework you can apply when evaluating any external dataset for integration:


**Understand the distribution format, update frequency, and sources of data**


- How is the data delivered? (e.g., API, flat files, database dump, cloud storage link). This helps to understand the necessary data transformations and whether you could do them locally.
- How often is the data refreshed? (e.g., real-time, hourly, daily, weekly, ad-hoc). This is crucial for understanding the data relevance.
- Where does the provider get the information? This is key to assessing the data’s reliability and potential biases. Also, do not forget to check the licensing!


**Build a “picture” of the external dataset**


- Schema and fields: keep only necessary fields.
- Data quality: check for missing values and correctness.
- Data volume and granularity: understand the dataset size and how data can be grouped.
- Sample analysis: review a representative subset of the data to uncover unexpected patterns, outliers, or issues.


**Measure the value of your existing data and estimate integration efforts**


- How will this external dataset augment or improve your data? Show the impact.
- Would it be difficult to integrate? Estimate the technical effort and identify potential bottlenecks.


I hope you had fun reading this and maybe even learned something new! Feel free to ask questions in the comments and share your own tips and tricks about data exploration that could help other software engineers.
