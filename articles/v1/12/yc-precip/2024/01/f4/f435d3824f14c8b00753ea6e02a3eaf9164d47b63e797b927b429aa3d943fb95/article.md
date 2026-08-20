---
schema_version: "1.0.0"
document_id: "f435d3824f14c8b00753ea6e02a3eaf9164d47b63e797b927b429aa3d943fb95"
company_key: "yc-precip"
company: "Precip"
source_id: "yc-precip-rss-f59c0f39e1b1"
canonical_url: "https://precip.ai/blog/founders-story/"
published_at: "2024-01-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:06.643563+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:417aae6863ac7d8a6245a00ae2c1b85ab742c1257fb2f3af719d3d833f2c9747"
---

# Why we started Precip: the founders backstory

## Why we ended up building an AI weather analytics company


Sam, Michael, and I are longtime friends who first started working together nearly a decade ago. We previously built a modern farm management software application used by over 20% of farmers in the United States. Early on in the journey of building that product, we learned how much farmers cared about measuring rainfall at their fields. That single feature proved to be the most popular thing we ever built and led to the founding of Precip. This is that story.


## Discovering rainfall data for farmers


If you are a farmer or spend much time with any farmers, you’d know that rain amounts are really important to them. I instinctively knew this because I had grown up on a fifth-generation family farm and saw it first hand. Not enough rain and your crops suffer, too much rain and it interferes with your ability to get work done. Farmers really like to see how much it rained and that makes sense because of how much it impacts their daily lives and livelihood.


Knowing that, you can imagine my reaction when I first learned that you could use radar data to quantify how much it rained at a high resolution with high accuracy. I knew we had to get it to our customers. After some digging, we found the Advanced Hydrologic Prediction Service precip observations datatset. It contained rainfall estimates at 4km grid resolution updated daily. We immediately prototyped a feature that incorporated it into our app and then started marketing that feature with incredible results. Our daily signups quadrupled overnight and our users loved it.


## Building the most accurate rainfall tracking


With our new virtual rain gauge feature, we were able to quantify how much it rained at any location with just a few clicks. We could show 24 hour rain totals, rainfall history charts, and weather benchmarks. There was just one problem: the data we were using wasn’t quite good enough. While it was sometimes very accurate, we noticed that it could also be quite wrong at times. One issue is that while 4km resolution is better than using open gauge networks, it still isn’t precise enough to capture the details that emerge in precipitation patterns.


Thankfully, we had an incredible data science team that was able to tackle the problem. Precip’s cofounder, Michael Asher was even working on his PhD in groundwater simulation at the time. We determined that higher resolution machine learning models could produce more precise and more accurate measurement of precipitation. We updated our software and delivered the best rainfall history data available.


## Making high resolution weather history easier to access


After some time passed, we started noticing a peculiar pattern emerging. Thousands of users who were not farmers were signing up for our farming software just so they could get access to our accurate historical weather data. Loggers and forestry managers, construction workers, golf enthusiasts, mining operations, mosquito control, and all sorts of other businesses needed a way to see how recent weather conditions were going to impact their operations. It became clear that we had a discovered a unique insight about how useful accurate weather data could be for anyone, not just farmers.


Years later, this still rang true. The three of us couldn’t shake the idea that more people wanted something that we were uniquely qualified to build. At the same time, we started connecting the dots around how advancement in AI applied to complex weather datasets could be used to unlock hyperlocal weather observations. We set to work on a new platform that brings customers a hosted dashboard to monitor precision weather, an API for integrating precision weather analytics into any application, and a free mobile app to conveniently access high-resolution weather on the go. We call it: Precip.


Jesse Vollmar
