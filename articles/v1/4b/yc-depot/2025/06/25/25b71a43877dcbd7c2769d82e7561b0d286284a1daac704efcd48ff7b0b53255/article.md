---
schema_version: "1.0.0"
document_id: "25b71a43877dcbd7c2769d82e7561b0d286284a1daac704efcd48ff7b0b53255"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/how-to-calculate-github-actions-usage"
published_at: "2025-06-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:b3440daa636fef9a3ed40667f570bf4190b1b88c18e76129db7cd062ea81aa4a"
---

# How to calculate your real GitHub Actions usage in minutes

GitHub Actions usage reporting doesn't tell the full story. If you're running different types of runners (like 4-core, 8-core, or more), those minutes aren't equal, and your total usage number doesn't reflect it.


This guide will show you how to:


- Export your raw usage data
- Normalize it by runner type
- Get an accurate total you can actually budget against \\


## **Why normalizing GitHub Actions minutes matters**


Runner types in[GitHub Actions](https://www.depot.dev/products/github-actions) have different compute capacities and costs. For example, a 4-core runner can do twice the work of a 2-core runner. If you just sum the raw usage, you're underestimating your real consumption and leaving yourself open to surprise bills.


## **Step 1: Export your usage report**


1. In GitHub, click your avatar (top right) and go to **Settings**
2. Select **Billing and plans**
3. Under **Usage this month** , click **Get usage report**
4. Choose your date range (30/60/90 days) and download the CSV


You'll now have a CSV showing runner types, quantities, and costs.


[Stop burning money on slow GitHub Actions runners. See how much you could save with 10x faster builds. Calculate your savings →](https://depot.dev/github-actions-price-calculator)


## **Step 2: Normalize your minutes**


Open the CSV in any spreadsheet tool. Focus on the 'sku' and 'quantity' columns. Each runner type has a multiplier based on its compute capacity:


Runner Type Multiplier


actions_linux 1


actions_linux_4_core 2


actions_linux_8_core 4


actions_linux_16_core 8


actions_linux_32_core 16


To normalize your total usage:


- For each runner type, multiply the quantity by the multiplier
- Sum these normalized values across all runner types


This gives you the total compute-equivalent minutes your team used during the period.


## **Step 3: Use the data to control costs**


Without normalized data, you're likely underestimating usage. This normalization process gives you a real number to track, budget against, and compare over time.


## **Take it further with Depot**


Once you know how many minutes you're actually burning, the next step is cutting them down. Depot Runners can dramatically speed up your CI builds, reducing both time and cost.


If you're ready to stop guessing and start saving, give[Depot](https://depot.dev/) a try.


## Related posts


- [Making EC2 boot time 8x faster](https://depot.dev/blog/faster-ec2-boot-time)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Now available: Gocache v2 for improved Golang build performance](https://depot.dev/blog/now-available-gocache-v2-faster-improved-golang-build-performance)
- [Now available: Depot ephemeral registries](https://depot.dev/blog/depot-ephemeral-registry)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


John Stocks


Head of Solution Engineering at Depot
