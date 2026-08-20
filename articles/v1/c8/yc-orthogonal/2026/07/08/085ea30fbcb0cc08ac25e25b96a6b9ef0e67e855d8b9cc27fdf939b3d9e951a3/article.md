---
schema_version: "1.0.0"
document_id: "085ea30fbcb0cc08ac25e25b96a6b9ef0e67e855d8b9cc27fdf939b3d9e951a3"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/influencers-club-creator-data-for-agents"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:9bfccb0dcae8c7d747b4f76f330a91fc9886bfa03c03ed790d8b041dfe7a5eaf"
---

# Influencers.club: 340M Creator Profiles for AI Agents

The creator economy is massive. Brands, agencies, and platforms need to find creators, verify their contact info, and understand their audience across platforms. But building a creator database from scratch means scraping dozens of social networks, validating millions of emails, and keeping everything up to date as creators grow and shift platforms.


Today we're excited to announce our partnership with[Influencers.club](https://influencers.club/) , a creator data platform with 340 million profiles and verified emails. Influencers.club powers creator discovery and enrichment for over 10,000 companies.


## What is Influencers.club?


Influencers.club is a creator data platform that provides discovery, enrichment, and outreach tools for the creator economy. Their database covers 340 million creators across Instagram, YouTube, TikTok, Twitter, Patreon, Twitch, Discord, Linktree, and 40+ more platforms.


Their core capabilities:


- **Creator Discovery** : Search and find any type of creator using 40+ filters including keywords, audience size, engagement, location, platform, monetization signals, and more. AI-powered search lets you describe the type of creator you need in natural language.
- **Data Enrichment** : Enrich your existing lists of emails, usernames, or phone numbers with full creator profiles. Match against 340 million profiles and get 40+ data points per creator.
- **Verified Emails** : Access verified contact details checked against multiple validation services to ensure deliverability.
- **Cross-Platform Analytics** : Get a 360-degree view of every creator with analytics and post data across all their social platforms.
- **Lookalike Discovery** : Find creators similar to ones you already work with.


## Key Features


### Creator Discovery API


Search the entire database of 340 million creators with advanced filters. Find creators by platform, audience size, engagement rate, content type, location, language, and more. The AI search lets you describe what you're looking for in plain English.


### Enrich by Handle


Pass a social media handle and get back the full creator profile: verified email, user info, post data, connected platforms, growth trends, posting frequency, and audience stats. Supports handles from any major platform.


### Enrich by Email


Already have an email address? Enrich it to find connected social media profiles, contact details, and platform account identifiers across Instagram, YouTube, TikTok, Twitter, and Twitch.


### Cross-Platform Social Graph


See every platform a creator is active on. One creator might have an Instagram, YouTube, TikTok, Twitter, and Patreon. Influencers.club connects them all into a single profile so you get the complete picture.


### Audience Demographics


Understand who follows a creator with audience data and filters. Essential for brands matching creators to their target audience.


### Integrations


Native integrations with HubSpot, Salesforce, Snowflake, Zapier, Clay, and n8n. Connect creator data directly into your existing workflows.


## Using Influencers.club with Orthogonal


### Enrich a Creator by Handle (Full)


Comprehensive creator enrichment by social media handle. Returns cross-platform presence, engagement analytics, follower growth, content performance, monetization indicators, niche classification, and optional lookalikes. Supports 11 platforms.


` import


Orthogonal


from


"@orth/sdk"


;


const


orthogonal


=


new


Orthogonal


(


{


apiKey


:


process


.


env


.


ORTHOGONAL_API_KEY


,


}


)


;


// Get full creator profile from a handle


const


creator


=


await


orthogonal


.


run


(


{


api


:


"influencers-club"


,


path


:


"/public/v1/creators/enrich/handle/full/"


,


body


:


{


handle


:


"charlidamelio"


,


platform


:


"tiktok"


,


include_audience_data


:


true


,


include_lookalikes


:


true


}


}


)


;


console


.


log


(


creator


.


data


)


;


`


### Enrich a Creator by Email


Enrich a creator by email address. Returns social media presence, contact details, and platform account identifiers across Instagram, YouTube, TikTok, Twitter, and Twitch.


` // Using the Orthogonal client initialized above


const


creator


=


await


orthogonal


.


run


(


{


api


:


"influencers-club"


,


path


:


"/public/v1/creators/enrich/email/"


,


body


:


{


email


:


"creator@example.com"


}


}


)


;


console


.


log


(


creator


.


data


)


;


`


### Discover Creators


Search and discover creators across multiple platforms using advanced filtering: location, gender, language, followers, engagement, audience demographics, brand deals, and more. Supports Instagram, YouTube, TikTok, Twitch, and Twitter.


` // Using the Orthogonal client initialized above


const


results


=


await


orthogonal


.


run


(


{


api


:


"influencers-club"


,


path


:


"/public/v1/discovery/"


,


body


:


{


platform


:


"instagram"


,


paging


:


{


limit


:


25


,


page


:


0


}


,


sort


:


{


sort_by


:


"relevancy"


,


sort_order


:


"desc"


}


,


filters


:


{


keywords_in_bio


:


\[


"fitness"


,


"workout"


\]


,


number_of_followers


:


{


min


:


10000


,


max


:


100000


}


,


engagement_percent


:


{


min


:


2


}


}


}


}


)


;


console


.


log


(


results


.


data


)


;


`


### Find Similar Creators


Find creators similar to a reference creator by URL, username, or ID. Supports Instagram, YouTube, TikTok, and Twitch.


` // Using the Orthogonal client initialized above


const


similar


=


await


orthogonal


.


run


(


{


api


:


"influencers-club"


,


path


:


"/public/v1/discovery/creators/similar/"


,


body


:


{


handle


:


"charlidamelio"


,


platform


:


"tiktok"


}


}


)


;


console


.


log


(


similar


.


data


)


;


`


### Enrich by Handle (Raw)


Retrieve raw platform data for a creator by handle. Returns unprocessed data including profile info, post data, media counts, and platform-native metadata. Lower cost than full enrichment.


` // Using the Orthogonal client initialized above


const


creator


=


await


orthogonal


.


run


(


{


api


:


"influencers-club"


,


path


:


"/public/v1/creators/enrich/handle/raw/"


,


body


:


{


handle


:


"mkbhd"


,


platform


:


"youtube"


}


}


)


;


console


.


log


(


creator


.


data


)


;


`


### Using x402 Protocol


Influencers.club on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for creator data API calls directly using USDC stablecoins, no API keys required.


` // Install: npm install x402-fetch viem


import


{


wrapFetchWithPayment


}


from


"x402-fetch"


;


import


{


privateKeyToAccount


}


from


"viem/accounts"


;


// Ensure PRIVATE_KEY is set as a 0x-prefixed hex string in your .env


const


account


=


privateKeyToAccount


(


process


.


env


.


PRIVATE_KEY


)


;


const


fetchWithPayment


=


wrapFetchWithPayment


(


fetch


,


account


)


;


// Enrich a creator handle with automatic USDC payment


const


response


=


await


fetchWithPayment


(


"https://x402.orth.sh/influencers-club/public/v1/creators/enrich/handle/full/"


,


{


method


:


"POST"


,


headers


:


{


"Content-Type"


:


"application/json"


}


,


body


:


JSON


.


stringify


(


{


handle


:


"charlidamelio"


,


platform


:


"tiktok"


}


)


}


)


;


const


data


=


await


response


.


json


(


)


;


console


.


log


(


data


)


;


`


## Use Cases


### Influencer Marketing


Find creators that match your brand's target audience. Filter by niche, audience demographics, engagement rate, and platform. Get verified emails to reach out directly. Scale from manual research to automated discovery.


### Creator Platforms


Building a platform for creators? Use enrichment to pre-fill profiles during signup. Validate that a user actually owns the social accounts they claim. Understand your user base with cross-platform analytics.


### Sales Outreach


Selling to creators or creator-focused businesses? Enrich your prospect lists with creator data. Identify which of your existing customers are creators. Personalize outreach based on their platform presence and audience.


### Brand Safety


Before partnering with a creator, enrich their profile to get a complete picture across all platforms. Check engagement authenticity, audience demographics, and content alignment. Reduce risk in creator partnerships.


### CRM Enrichment


Keep your CRM up to date with fresh creator data. Automatically enrich new contacts with social profiles, follower counts, and engagement metrics. Identify creators among your existing customer base.


### Talent Agencies


Manage creator rosters with comprehensive cross-platform data. Track growth trends, compare creators, and identify rising talent before they blow up.


## Why We Partnered with Influencers.club


Companies that work with creators need reliable data to find the right partners, verify contact information, and understand audiences. But building and maintaining a database of 340 million creators across 40+ platforms is a massive infrastructure challenge.


Influencers.club has solved this at scale. They power over 10,000 companies, offer verified emails checked against multiple validation services, and provide cross-platform creator profiles with 40+ data points per creator.


With Orthogonal, you get instant access to Influencers.club's creator data through a single API key, with pay-per-request pricing and no separate account setup required.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Influencers.club and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/influencers-club) |[Influencers.club Website](https://influencers.club/)
