---
schema_version: "1.0.0"
document_id: "d98649d451202217c1812a614557e528952673fac34cd1948ef23b2096c22fff"
company_key: "weave-communications-inc-common-stock"
company: "Weave Communications Inc."
source_id: "weave-communications-inc-common-stock-rss-cb397ff18858"
canonical_url: "https://engineering.getweave.com/talk/fcfs-in-go/"
published_at: "2024-11-07T18:26:46+00:00"
first_seen_at: "2026-07-20T23:19:36.973215+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:1a9860b03e52692e675b6ba22271281684f0fa0c67591dc524a4a2cb08717b40"
---

# Stop Sleeping On First Class Functions In Go

# Stop Sleeping On First Class Functions In Go


Go has so many great features that draw us in: Channels, Goroutines, static compilation, a simple syntax, etc. But there is one feature that Go supports which needs more love: first class functions. This feature is not unique to Go, and with all the other shiny features to talk about, I think it is all too often overlooked and underutilized.


First class functions (functions as variables) and closures (capturing data in those functions) are two incredibly powerful features that are built right into Go. But even seasoned Go developers often forget these features. We need to fix this!


We are going to go over several cases where these two features massively improve code usability and readability:


- Dependency Injection
- Passing functions as arguments to make our code testable
- Cleaning up Go templates
- Reducing complexity in templates with closures
- Hiding channel usage
- Stop passing channels, pass functions
- Building an execution pipeline
- Making ordered operations easy and extensible


## Where


Go West 2024


## When


## Watch it
