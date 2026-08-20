---
schema_version: "1.0.0"
document_id: "dd6426502e691fd316eb04c3418c0ce40919f16347672ab80a99f534e73a32bb"
company_key: "netflix-inc-common-stock"
company: "Netflix Inc."
source_id: "netflix-inc-common-stock-rss-c4c725f6f796"
canonical_url: "https://netflixtechblog.com/modeling-device-capabilities-for-analytics-e7607acebde8"
published_at: "2026-07-31T16:01:02+00:00"
first_seen_at: "2026-07-31T19:08:10.724042+00:00"
fetched_at: "2026-07-31T19:08:11.259273+00:00"
content_hash: "sha256:f65c99263faed289a6e5cf26ab8f356bf0930920ef52cb8d64b139b057516404"
---

# Modeling Device Capabilities for Analytics

Analytics


Data Modeling


Devices


Data Engineering


# **Modeling Device Capabilities for Analytics**


[Netflix Technology Blog](https://netflixtechblog.medium.com/?source=post_page---byline--e7607acebde8---------------------------------------)


2 min read


·


3 hours ago


--


by[Aarti Laddha](https://www.linkedin.com/in/aarti-laddha-70666557/) ,[Richard Diaz-Cool](https://www.linkedin.com/in/richardjcool/) ,[Rishika Idnani](https://www.linkedin.com/in/rishikaidnani/) ,[Venkatesh Selveraj](https://www.linkedin.com/in/venkatesh-selvaraj-88824137/)


Netflix supports a vast and evolving set of features and content types, ranging from 4K streaming and immersive audio to live streaming and cloud gaming, across a diverse ecosystem of devices. However, not all devices are created equal. Hardware limitations such as available RAM, CPU cores, display capabilities, or platform support mean that some features cannot be supported on certain device models. To ensure the best possible user experience, we rely on a deep understanding of device capabilities. We have invested in building a comprehensive device capability data model and integrating feature flags from internal systems, paving the way for smarter, more granular feature management across our global device landscape. This approach helps us identify bottlenecks in feature penetration and accelerates the pace of innovation.


## Get Netflix Technology Blog’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


We have designed our data storage and modeling strategies to efficiently support analytics at scale. We use a cumulative table to process information about the device’s capabilities. This table is structured to efficiently capture the latest state of each device and its associated capabilities (like Screen resolutions, Video Profiles Supported, Surround Sound, RAM size etc) making it ideal for analytics and reporting use cases.


```text
{  "Screen Height": ["720"],  "Screen Width": ["1280"],  "Video Profiles":   [  "playready",  "hevc",  ],  }
```


For aggregate analytics, we leverage a histogram table that captures active device counts over the past 28 days, broken down by device model and software version. This table also records the number of devices supporting specific capabilities, enabling detailed distribution analysis. One use case for this histogram data is to analyze the distribution of external display capabilities attached to streaming sticks. For example, the histogram below shows that out of total X number of devices, all supported the HD profile (playready), while only 20% devices supported the UHD profile (hevc).


```text
{  "Video Profiles": {        "playready": 100%, # HD profile        "hevc": 20% # UHD profile  }  }
```


We have built analytical products that leverage these datasets to provide a comprehensive view of feature reach such as 4K Ultra HD, Netflix Spatial Audio, Cloud Gaming and the latest UI. By relying on data-driven insights, we can make informed decisions about which features to enable on specific devices, ensuring both performance and reliability.
