---
schema_version: "1.0.0"
document_id: "0a914d184259ea48af686693d830d1743c10b5e9ec0892ad4f60a19734ab9ce4"
company_key: "pubmatic-inc-class-a-common-stock"
company: "PubMatic Inc."
source_id: "pubmatic-inc-class-a-common-stock-news-import-f533c4a789db"
canonical_url: "https://pubmatic.com/blog/rtb-pipe-efficiency-techniques-for-dsps/"
published_at: "2021-12-22T21:26:11+00:00"
first_seen_at: "2026-07-24T13:12:36.256063+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:bd020e9d17766c72677ce47fa822188042978e1f73cd7de493979b51d7fcf557"
---

# Improving the Overall RTB DSP Platform Efficiency | PubMatic Blog

With the proliferation of header bidding and the ever-increasing volume of ad impressions, demand-side platforms (DSPs) have seen their infrastructure costs rising. It has become important for DSPs to smartly utilize their infrastructure and utilize the signals PubMatic sends for bid decisioning with the highest winning opportunities and minimum infrastructure cost. PubMatic has focused on building and implementing solutions to help ease the load on DSP infrastructure and improve the overall RTB pipe efficiency.


Below are a few meaningful enhancements we rolled out across our top demand partners to improve the overall RTB DSP platform efficiency.


### GZIP: Compression bid request/bid response.


PubMatic supports the GZIP format for RTB bid requests and responses for our DSPs.


GZIP is a compression format that has found widespread adoption for bid request and bid response compression. It helps exchanges as well as DSPs to reduce by ~ 30% the network bandwidth utilized and the costs associated with it.


### Bcrid: Blocked Creative ID.


The bcrid field is a string array which contains blocked creative IDs which got blocked on PubMatic’s end due to ad quality-related bid filters. To improve the overall win rate of DSPs, PubMatic can pass top 100 creative IDs blocked by publishers so that DSPs can ingest the flag and bid with alternate creative ID — improving win rate on the PubMatic platform. We recently implemented this feature with some of our top partners and observed an overall 3-4% increase in win rate.


### Bid Request Attributes.


PubMatic believes that ingesting these signals will help optimize bid decisioning and improve overall DSP platform efficiency.


- **Badv** : Blocked list of advertisers by their domains.
- **Bcat** : Blocked Advertiser categories using IAB content categories.
- **Bapp** : locklist of applications by their platform-specific exchange-independent application identifiers. On Android, these should be bundle or package names (for example, com.foo.mygame). For iOS, these will be numeric IDs.


### Tmax: Maximum timeout value in real-time.


This is the maximum time in milliseconds that PubMatic allow their DSP partners to submit the bid to be eligible for the auction. This value will supersede default 130ms guidance from PubMatic.


PubMatic recently started passing tmax in bid requests for DSPs to ingest the value and respond to bid requests accordingly. We have observed double digit growth in drop-in timeout rates along with a double digit increase in bid rates with DSPs after adoption of tmax.


### Win/Loss Notification.


PubMatic win/loss notification allows DSPs to ingest the reason codes from RTB bid requests and understand why they did not win an impression. It gives DSPs a better understanding about their competitive landscape, along with insights to improve win rates.


### Multi-Bid Response.


PubMatic allows DSPs to submit up to 5 bids for a single impression.[Multiple bids](https://pubmatic.com/blog/dsp-can-increase-win-rates-multi-bid/) within the same bid response allow DSPs to bid on the same impressions with multiple buyers/advertisers and increase the win rate. Our multi-bid response implementation has helped DSPs to improve win rates by ~7%.


We believe that adoption of the above[RTB pipe efficiencies techniques](https://pubmatic.com/products/rtb-technologies/) can lower infrastructure costs and increase DSP revenue and profitability. This can also provide DSPs with better bid decisioning and user targeting capabilities, while utilizing a platform to fulfill programmatic budgets efficiently.
