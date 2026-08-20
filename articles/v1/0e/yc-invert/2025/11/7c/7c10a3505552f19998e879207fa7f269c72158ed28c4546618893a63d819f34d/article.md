---
schema_version: "1.0.0"
document_id: "7c10a3505552f19998e879207fa7f269c72158ed28c4546618893a63d819f34d"
company_key: "yc-invert"
company: "Invert"
source_id: "yc-invert-news-import-968576ef11e8"
canonical_url: "https://invertbio.com/blog/integrations-with-julia-miller"
published_at: "2025-11-11T00:00:00+00:00"
first_seen_at: "2026-07-24T00:25:27.297188+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:f10d7bcbe09385ebe635e7d7bb929de2acf3954b3672a20bef2fd20c2edb4a67"
---

# Engineer Blog Series: Integrations with Julia Miller

*Welcome to Invert's Engineer Blog Series! This series is a behind-the-scenes look into the product and how it's built. For our first post, senior software engineer Julia Miller speaks to us about Invert's integrations — how they're implemented, what makes them special, and what goes into making them work for bioprocess.* **
‍**


## What can a customer expect during a brand new Invert implementation?


First, we learn about the data that the customer already has. Every new customer comes with historical data in different formats, a different set of equipment, and software that might have data on it already. We learn about how we need to transform this data to fit into Invert so all the data can be compared and used together.


Once we know how data is going to fit, we import historical data via file ingestion and connect integrations to the customer’s equipment so that we can get live data from it.


## Some of Invert’s integrations are pre-built. What does that mean?


It means that we already know how the data from that equipment, for example, a DASGIP bioreactor, is going to look like. After that, we just need to work out how to configure the integration—where the equipment is running, how it’s connected, what the IP address is, and most importantly, what concrete information they want out of this equipment.


Once we know what information they care about, then it’s basically just configuring what we’ve already built for that customer.


## What makes Invert’s integrations unique?


We have a unique data model that works for all the processes in biotech and biopharma. One thing that’s enabled us to fit all that data into our data model is the development of Invert’s data streams. Previously, we only were able to ingest “batch aware” data—which meant that whenever there was incoming data, we needed to know which batch or process it belonged to.


With data streams, we can save all the data that we don’t have a batch or experiment or process for yet. Later, the customer can say, “This data from yesterday from 2PM to 4PM belonged to this experiment,” and they can retrospectively assign that data to that experiment. I don't think there are many companies that can do that.


Julia at Invert's 2025 team offsite at Zion National Park


## What’s challenging about building integrations?


Historically, the most challenging thing was file ingestion for customer historical data.


For example, we might get a batch record that’s an Excel file, but the information is not often in a format you can easily read and put in your database. There might be information all over the file, with the name of a run in one corner of the first page and the pH in a different tab, so you have build code to map that information to Invert’s database. You have to account for data that isn’t there or is in a different format as well. Also, as with every Excel file, people change stuff. This breaks the map and leads to an error if someone tries to upload an altered file.


However, this process is getting a lot easier with large language models (LLMs). Usually the process of building file ingestion maps would take much longer, but now our CX team can use LLMs to build new mappings for each customer, which has made it much faster.


## Tell us about a memorable real-life problem you had to solve.


Once, we had to integrate with a specific bioreactor with a data structure that was very different from anything we’d ever seen before. We also didn’t have a lot of documentation, and there was no UI to explore that would allow us to see how the data looked like.


To get around that, we built a little program that we installed on the machine where our agent was running. Its job was to walk the whole data tree, scan it, and give it to us in a text file format. That allowed us to open the bioreactor up and make decisions based on the data we wanted to have, all without having a UI or ever seeing any of the real data.


‍


*Julia Miller is a Senior Full-Stack Software Engineer at Invert, former business analyst, and product owner. She has an M.Sc. in Financial Services and Risk Management. At Invert, Julia works on integrating Invert with all kinds of other systems in bioprocess.*
