---
schema_version: "1.0.0"
document_id: "bfa06c0c56e707bf50dead9a8073a92bb83da42fcf6560c9b206f1a74b54e577"
company_key: "veea-inc-common-stock"
company: "Veea Inc."
source_id: "veea-inc-common-stock-rss-002d85bedea1"
canonical_url: "https://blog.veea.com/findings-from-a-30-day-secureconnect-trial-at-a-single-hospitality-location"
published_at: "2026-05-05T21:29:29+00:00"
first_seen_at: "2026-07-27T06:31:41.614493+00:00"
fetched_at: "2026-07-28T21:55:57.170712+00:00"
content_hash: "sha256:3ef6b4dca1997abd107d1d215933fc592a9732fadd15728b67827cf6748bdc11"
---

# Findings From a 30-Day SecureConnect Trial at a Single Hospitality Location

Veea recently completed a 30-day SecureConnect trial at a high-traffic hospitality venue in order to quantify what actually moves across a guest WiFi network when every request is inspected. The deployment required no changes to staff workflow and was fully transparent to guests. The data below reflects activity captured during the trial period.


# Trial results


Over the course of 30 days, SecureConnect authorized 8,600 unique devices and processed 878,000 network requests. During the same window, AI-driven threat detection blocked 3,770 malicious requests, and the combined total of requests blocked through DNS filtering and policy enforcement reached 4,260.


**Metric**


**Count**


Devices authorized


8,600+


Network requests processed


878,000+


AI-detected threats blocked


3,770+


Total malicious activity and policy blocks


4,260+


# Traffic patterns were consistent with hospitality norms


The traffic profile matched what most operators in the sector would reasonably expect. Guests connected primarily from iPhone, Android, and macOS devices, and the highest-volume destinations were social platforms and cloud services, including Facebook, TikTok, iCloud, YouTube, and Google.


Destination geography extended well beyond the local region, with measurable volumes of traffic routed to the United States, Hong Kong, Singapore, and China. This distribution reinforces the point that guest WiFi rarely functions as a local-only concern and that global threat visibility is a practical requirement rather than a theoretical one.


# Mobile cryptocurrency mining was active on the network


The trial identified 31 devices, predominantly Samsung and Android handsets, actively performing cryptocurrency mining while connected to the guest network. In most cases, this behavior runs without the device owner's knowledge and is a reliable indicator that the handset itself has been compromised. For the operator, mining traffic consumes paid bandwidth and degrades performance for legitimate users.


SecureConnect's DNS filtering is capable of blocking traffic to known mining pools automatically, which would have neutralized all 31 devices without additional hardware, manual configuration, or direct guest interaction.


# Context for the threat-detection figures


The 3,770 blocked threats represent requests from guest devices to destinations classified as malicious, including phishing domains, command-and-control infrastructure, and known malware hosts. Each request was intercepted before the connection was completed, and the majority of these events were not visible to either guests or staff, which is the intended behavior for a properly functioning threat-protection layer.


# Implications for hospitality and retail operators


The trial demonstrates that meaningful network visibility and security outcomes can be achieved within a short evaluation window and without dedicated IT staffing. In 30 days, a single location produced a stable guest network under sustained load, automatic threat blocking at the DNS layer, device-level visibility across the SSID, and policy controls available for activation as needed.


For any venue offering guest Wi-Fi — including restaurants, hotels, retail, and mixed-use environments — the activity captured in this trial is broadly representative of what is currently occurring on comparable networks.


# Evaluating SecureConnect in your environment


Organizations interested in running their own evaluation can request a trial and receive a comparable 30-day report.


Visit:[https://veea.com/solutions/secureconnect](https://veea.com/solutions/secureconnect)
