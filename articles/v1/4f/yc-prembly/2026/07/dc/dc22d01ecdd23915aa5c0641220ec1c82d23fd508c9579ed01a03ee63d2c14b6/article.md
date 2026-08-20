---
schema_version: "1.0.0"
document_id: "dc22d01ecdd23915aa5c0641220ec1c82d23fd508c9579ed01a03ee63d2c14b6"
company_key: "yc-prembly"
company: "Prembly (formerly Identitypass)"
source_id: "yc-prembly-rss-2283cd5407c5"
canonical_url: "https://blog.prembly.com/bvn-verification-watchlist-status-update/"
published_at: "2026-07-14T14:41:19+00:00"
first_seen_at: "2026-07-25T19:45:51.668136+00:00"
fetched_at: "2026-07-27T08:50:03.759737+00:00"
content_hash: "sha256:5462e989a9796db603782fea4fc8a33f6bb2bfd1d9447b1abb6fcfe85d7f572e"
---

# BVN Verification Update: New Watchlist Response Fields

Prembly has introduced an enhancement to its BVN Verification endpoint to provide greater visibility into watchlist screening results.


As part of our ongoing commitment to improving identity verification and compliance workflows, we’ve added two new response fields that provide clearer insight into the watchlist status of a verified BVN. This additional context helps you better interpret verification results, supporting more informed risk assessments during customer onboarding and ongoing monitoring.


With this update, the endpoint now returns two additional response fields that help you better understand whether a verified BVN is associated with a watchlist.


## **What’s New?**


The following fields have been added to the BVN Verification response:


- **watchListed –** Indicates whether the verified BVN is associated with a watchlist (true or false).
- **watchListMessage –** Returns a descriptive message providing additional context about the watchlist screening result.


## **Why This Matters**


These new response fields make it easier for your team to interpret watchlist screening outcomes directly from the verification response, enabling faster and more informed risk and compliance decisions.


By including watchlist status and a descriptive message as part of the verification response, your team can streamline customer screening, automate decision-making where appropriate, and reduce the need for additional manual checks. This provides greater transparency into verification outcomes while supporting more efficient onboarding and compliance workflows.


## **Do You Need to Make Any Changes?**


No changes are required to your existing integration. The new response fields are now included in the BVN Verification endpoint response and will not affect your current implementation. If you’d like to display or process watchlist information in your application, you can begin consuming these fields immediately.


<p>The post[BVN Verification Update: New Watchlist Response Fields](https://blog.prembly.com/bvn-verification-watchlist-status-update/) first appeared on[Prembly](https://blog.prembly.com/) .</p>
