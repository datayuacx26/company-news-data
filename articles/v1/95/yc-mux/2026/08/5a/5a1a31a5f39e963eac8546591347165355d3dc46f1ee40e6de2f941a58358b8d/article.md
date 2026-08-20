---
schema_version: "1.0.0"
document_id: "5a1a31a5f39e963eac8546591347165355d3dc46f1ee40e6de2f941a58358b8d"
company_key: "yc-mux"
company: "Mux"
source_id: "yc-mux-atom-4708df60f240"
canonical_url: "https://www.mux.com/blog/how-to-generate-the-best-possible-title-description-and-tags-for-your-video-content-with-mux-robots"
published_at: "2026-08-12T16:29:31.765+00:00"
first_seen_at: "2026-08-12T21:33:51.259152+00:00"
fetched_at: "2026-08-12T21:33:53.537612+00:00"
content_hash: "sha256:a1df0913a2500329e21b82514463df067fdb61cbe81d7d0a2da01941144cc65c"
---

# How to generate the best possible title, description, and tags for your video content with Mux Robots

Published on


August 12, 2026


# How to generate the best possible title, description, and tags for your video content with Mux Robots


By[Dylan Jhaveri](https://www.mux.com/team/dylan-jhaveri) • 6 min read •[Product](https://www.mux.com/blog/category/product)


---


Summarizing a video is easy, right? Just chuck the transcript to an LLM and ask for a title, description, and tags.


To be honest with you, yes, pretty much. That will give you *something* , but it’s likely going to smell like AI and when you actually look at the output, you’ll ask yourself: *Do I want to show this to my users* ? *Is this title actually a good title for my video? Do these tags even make sense?*


And then you get problems like: I have a tag for *funny* and for *comedy* , why do I have that? That should be the same thing.


This is how you take[Mux Robots](https://www.mux.com/blog/mux-robots) from a plaything, an experiment, into an actual production workflow that delivers value to your users.


Let’s dive into how you can use the[Mux Robots Summarize workflow](https://www.mux.com/docs/guides/robots-summarize) to generate video titles, descriptions, and tags that you are excited to put in front of your users.


## Start with tone


The baseline input is tone


— that gets set up front, and you can choose from:


- neutral


for straightforward, factual analysis
- playful


for witty/conversational copy — for social media, for example
- professional


for polished, executive-level business language — for something like a report or an investor update


What you are going to get back is:


- title


- description


- tags


(defaults to 10)


Summarize is based on @mux/ai


, and we run and[publish evals you can check out here](https://evaluating-mux-ai.vercel.app/) .


## Start by providing some basic guidance


The first steps of basic guidance you can provide are:


- title_length


- description_length


- tag_count


(default 10)
- language_code


/ output_language_code


– if you have multiple text tracks


That tunes the basic shape, and you’re still getting the default output when it comes to the actual content.


## Use output_steering options for granular control


This is where you can really make the output your own:


- summary_style


( concise


, detailed


or editorial


)
- audience


to provide context on who the audience is, which will nudge the model toward the optimal tone
- brand_terms


to make sure spelling and capitalization is correct for words that you wouldn't find in the dictionary
- tag_taxonomy


- name


the name of this group of tags, for example "product categories"
- values


the list of tags to choose from


- label


- description


- aliases


## Let’s use a NASA space video example


Say you're running a space-mission video archive. Your users browse by topic, so your tags need to come from a fixed set.


Here's NASA's five-minute Artemis overview,[How We Are Going to the Moon](https://player.mux.com/qwDHZ7Yjplm02fSJT1xSOe501O78PVpsqz) , run through Summarize with the defaults:


bash


```text
mux robots summarize  $ASSET_ID    --wait
```


**Title:** NASA Artemis Mission Overview


**Description:** This overview explains NASA's Artemis program for returning humans to the moon. It highlights the Orion spacecraft, the Space Launch System rocket, and the use of the Gateway station in lunar orbit to facilitate lunar landings, scientific research, and preparation for future missions to Mars.


**Tags:**


- artemis


- nasa


- moon


- orion


- space launch system


- gateway


- lunar mission


- space exploration


- rocket


- astronauts


All of this is accurate and appropriate. Nothing is *wrong* here. A lot of what you want depends on the context of your product and how the tags are used. Are you using the tags for embeddings, search, and indexing? Or are you showing these tags to end users? Both have slightly different needs.


If you are a general purpose UGC platform where users are uploading all kinds of videos, those tags might be perfect. But if you are specifically operating a space-mission video archive, where all of your videos are roughly space related, then tags like space exploration and rocket are not so helpful.


Now the steered version. Same video, same job, updated request params.


We’re adding:


- Guidance for title_length


- Guidance for description_length


- A target tags_count


, because we show these tags to the user and don’t want to overwhelm them.
- editorial


summary style, because that’s the kind of vibe our video archive platform has.
- Guidance about the audience


and what they are doing.
- Specific tag taxonomy that we already use internally. We had a separate Claude agent run through all of our written content and video archives to suggest a good tagging taxonomy which we reviewed to make sure it was exactly what we wanted.


json


```text
{
"tone"  :    "professional"  ,
"title_length"  :    12  ,
"description_length"  :    60  ,
"tag_count"  :    6  ,
"output_steering"  :    {
"summary_style"  :    "editorial"  ,
"audience"  :    "Space fans browsing a mission video archive, deciding what to watch next"  ,
"brand_terms"  :    [  "NASA"  ,    "Artemis"  ,    "Orion"  ,    "Space Launch System"  ,    "Gateway"  ]  ,
"tag_taxonomy"  :    {
"name"  :    "Mission archive topics"  ,
"values"  :    [
{
"label"  :    "Moon & Artemis"  ,
"description"  :    "Lunar exploration: the Artemis program, lunar landers, and Moon science"  ,
"aliases"  :    [  "lunar"  ,    "Moon missions"  ,    "Artemis program"  ]
}  ,
{
"label"  :    "Rockets & Launch"  ,
"description"  :    "Launch vehicles, boosters, engines, and launch operations"  ,
"aliases"  :    [  "SLS"  ,    "launch vehicle"  ,    "liftoff"  ,    "boosters"  ]
}  ,
{
"label"  :    "Spacecraft & Hardware"  ,
"description"  :    "Crew capsules, space stations, and mission hardware like Orion and Gateway"  ,
"aliases"  :    [  "Orion"  ,    "capsule"  ,    "Gateway"  ,    "space station"  ]
}  ,
{
"label"  :    "Mars & Deep Space"  ,
"description"  :    "Mars exploration and missions beyond the Moon"  ,
"aliases"  :    [  "Mars missions"  ,    "deep space"  ,    "red planet"  ]
}
]  ,
"allow_other"  :    false
}
}
}
```


Let’s call that file steering.json


.


bash


```text
mux robots summarize  $ASSET_ID    -f   steering.json  --wait
```


**Title:** NASA Artemis Mission Overview


**Title:** Artemis: The Path to the Moon and Beyond


**Description:** An overview of the Artemis program, detailing the mission architecture designed to establish a long-term human presence on the Moon as a foundational step for future Mars exploration. This analysis covers the integrated systems approach, utilizing the Orion spacecraft, the Space Launch System rocket, and the orbital Gateway station to enable sustainable lunar surface operations and deep space capability development.


**Tags:**


- artemis
- nasa
- moon
- orion
- space launch system
- gateway
- lunar mission
- space exploration
- rocket
- astronauts


**Tags:**


- Moon & Artemis
- Spacecraft & Hardware
- Rockets & Launch


## The 3 things that happened here


1. The tags are ours now. Every tag is mapped to the canonical label. An important bit here was the aliases which collapsed the terms like "SLS", "liftoff", and "launch vehicle" into one Rockets & Launch tag instead of three near-duplicates.
2. No spelling mistakes for brand terms: *Artemis* , *Orion* , and *Space Launch System* are written precisely the way NASA writes them. This is also very handy for names.
3. The copy is aligned to the audience


and summary_style


.


This is how you take Mux Robots summarize and tagging and guide it to give you the exact output you want.


## Give this to your agent


If you’re like me, you’ll want to try and level this up a step. Instead of me hand-tuning and tweaking the output steering params, I’m going to fire up my coding agent to give this a first pass. After I get some feedback I’ll probably tweak the final steering parameters, but this gives me a quick way to iterate.


Try handing this prompt to your agent to help it figure out the best output steering parameters.


1. Run a coding agent session in the context of your project, so it has context of what your application is, what it does, who uses it, and how your videos are used.
2. Install the[Mux CLI](https://www.mux.com/docs/integrations/mux-cli) for your agent to use.
3. Give it a prompt along these lines.


prompt for tuning Robots summarize output


```text
I want to start using Mux Robots to generate titles, descriptions, and tags for my videos. See details here: https://www.mux.com/docs/guides/robots-summarize


Consider how we currently use videos, how we plan to use videos, and what kind of titles, descriptions, and tags we currently show, if any, and how those could be improved. Run a few isolated tests with different parameters and output_steering options to determine the exact parameters we should set on the summarize job when we run this for all of our videos. Poke around and make a plan, and then ask me questions before you get started. As you start to experiment with different settings, summarize what you are seeing, give me an easy way to review it, then give me your best recommendation, and ask me to confirm the final decision.
```


## Written By


### [Dylan Jhaveri – Director of Self Service](https://www.mux.com/team/dylan-jhaveri)


Software Engineer and cold water surfer. Previously startup co-founder. Trying to find the best cheeseburger in San Francisco.


## Leave your wallet
where it is


No credit card required to get started.


[Sign up Sign up](https://dashboard.mux.com/signup)
