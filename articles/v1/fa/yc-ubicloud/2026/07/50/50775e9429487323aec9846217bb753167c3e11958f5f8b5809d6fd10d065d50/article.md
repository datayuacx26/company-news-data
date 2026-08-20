---
schema_version: "1.0.0"
document_id: "50775e9429487323aec9846217bb753167c3e11958f5f8b5809d6fd10d065d50"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/ubicloud-price-adjustment-2026"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:6536f3269168d112a30019410f76773aabec72c2a0d0dae2eb984b13138e245c"
---

# Ubicloud Price Adjustment

## Ubicloud Price Adjustment


April 27, 2026 · 2 min read


Ozgun Erdogan


Co-founder / Co-CEO


Ubicloud is an open source alternative to AWS. We offer managed cloud services that build on top of PostgreSQL, Kubernetes, vLLM, and others.


We haven’t increased prices since setting them in 2024. Since then, our infrastructure costs have risen by more than 30%. As a result, we’re increasing prices for most cloud services by 26%. New deployments will see this price increase on May 1, 2026. Existing resources will be charged at the new rate starting June 1, 2026.


If you’re using our GitHub Actions product, premium runner prices will remain unchanged.


### Why prices are changing


Ubicloud uses infrastructure providers such as Hetzner, Leaseweb, and AWS for its non-AI services. Over the past two and a half years, we saw our costs increase by 35.3% due to a combination of factors.


- Increase in RAM add-on prices by 5x
- Increase in server setup costs by up to 7x
- Increase in server lease prices by 12-17%
- US dollar depreciating against the Euro. We pay part of our infrastructure bill in Euros while charging customers in USD


We initially absorbed much of this increase, but recent RAM pricing changes made a price adjustment necessary. To continue investing in the Ubicloud platform, we decided to apply a 26% increase to virtual machines (VMs), PostgreSQL databases, Kubernetes clusters, and GitHub Actions standard runners. GitHub Actions premium runner prices remain unchanged.


New deployments will be charged at the new rate starting May 1, 2026. Existing resources will move to the new rate on June 1, 2026.


This price change affects our Germany, Finland, and Virginia regions. AWS-powered regions are unchanged. We’re also aligning Finland pricing with Germany. Below, you can find examples of price updates. We calculate resource consumption at per-minute granularity and invoice monthly.


### New prices


#### Virtual Machines
‍


Family Location Old Price New Price


burstable-1


Germany $6.65/mo $8.38/mo


standard-2


Germany $26.60/mo $33.52/mo


standard-2


Finland $25.40/mo $33.52/mo


standard-2


Virginia $33.40/mo $42.08/mo


Drag table left or right to see remaining content


#### Managed PostgreSQL
‍


Family Location Old Price New Price


hobby-1


Germany $12.41/mo $15.63/mo


standard-2


Germany $49.62/mo $62.52/mo


standard-2


Virginia $59.76/mo $75.30/mo


standard-2


AWS No change No change


Drag table left or right to see remaining content


#### Managed Kubernetes
‍


Family Location Old Price New Price


**Single node** ‍ **(non-HA)**


Germany $45.60/mo $57.46/mo


**3 nodes (HA)**


Germany $136.80/mo $172.37/mo


**Single node** ‍ **(non-HA)**


Virginia $51.52/mo $64.92/mo


**3 nodes (HA)**


Virginia $154.56/mo $194.75/mo


Drag table left or right to see remaining content


#### GitHub Action Runners
‍


Family Old Price New Price


standard runner


$0.0008/min $0.0010/min


premium-2 runner


No change No change


Drag table left or right to see remaining content


Standard x64 runners are no longer available for new customers. New customers can use premium runners, which offer better performance, and whose prices remain unchanged.


If you have any questions, please reach out to us at[\[email protected\]](https://www.ubicloud.com/cdn-cgi/l/email-protection#c4b7b1b4b4abb6b084b1a6ada7a8abb1a0eaa7aba9) .


Next up


[Quick Start - Build your own cloud](https://www.ubicloud.com/docs/quick-start/build-your-own-cloud)
