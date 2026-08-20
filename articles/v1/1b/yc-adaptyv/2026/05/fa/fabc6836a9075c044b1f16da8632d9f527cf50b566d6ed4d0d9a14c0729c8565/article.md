---
schema_version: "1.0.0"
document_id: "fabc6836a9075c044b1f16da8632d9f527cf50b566d6ed4d0d9a14c0729c8565"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-news-import-3c8571c35bcb"
canonical_url: "https://www.adaptyvbio.com/blog/benchling"
published_at: "2026-05-06T00:00:00+00:00"
first_seen_at: "2026-07-24T14:21:15.816567+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:413ee6ae09da99e5d3521ff6e5dc0bf45d27def802ae8208ab47e1508d47715b"
---

# Test proteins with Adaptyv directly from Benchling

/


TL;DR


- -


• You can now submit protein candidates to the Adaptyv wet-lab directly from Benchling.


- -


• Submission goes through either a single Adaptyv app in your Benchling project or through Benchling AI, both wired to the Adaptyv public API.


Starting today, you can submit protein candidates to the Adaptyv wet-lab directly from


[Benchling](https://www.benchling.com/)


as part of


[Benchling's Direct Ordering Partners](https://www.benchling.com/blog/one-click-ordering-for-experiments-and-data)


. This can be done through a single app in your Benchling project or through


[Benchling AI](https://www.benchling.com/ai)


, both connected to our


[Adaptyv API](https://www.adaptyvbio.com/api)


.


If you’re unfamiliar with Benchling, you should know it's the R&D AI platform that most scientists all across industry and academia use to keep their notebooks, registries, sequence editors, and project context in one place, including most of the top 50 biopharma. Most of the bio R&D right now runs through Benchling. Benchling increasingly powers the analytical and AI layers that make R&D data meaningful.


However, for many R&D teams running protein design campaigns, the slow part of the workflow is the round-trip from and back to a platform. This involves exporting FASTAs, sending a quote request to a CRO, shipping samples or sequences, tracking an outside portal, and re-uploading results when they finally come back. We've spent the past few years turning the wet-lab side of biology into infrastructure that any computational environment can plug into, and earlier this year we


[opened it up as a public API](https://www.adaptyvbio.com/blog/adaptyv-api)


. Benchling is where most of our customers' workflows and data already live, and so integrating with them was the natural next step, avoiding the round-trip and keeping everything (from designs and analysis to wet-lab results) connected.


### **What you can do now**


From a Benchling notebook, you can now submit a panel of variants for expression testing, binding characterization, or thermostability. Expression yields come back from our standardized cell-free setup with QC attached. Binding characterization (BLI) returns full kinetics (KD, kon, koff) against any target in our public catalogue, or a binder/non-binder classification when you're screening larger panels, with custom targets supported on request. You can find more info on our assays in


[our docs](https://docs.adaptyvbio.com/docs/getting-started/introduction)


.


Prices show up inline before anything is submitted, running through the same pricing workflow as our customer-facing


[Foundry portal](https://foundry.adaptyvbio.com/)


, so the per-variant cost in Benchling always matches what you would see in Foundry without any back-and-forth. Once you confirm an order, you can track each sequence through gene synthesis, expression, purification, and characterization without leaving Benchling. You can also see all your submitted experiments and recent results.


### **Why we built this for Benchling**


We think


***provenance***


matters a lot in current protein design workflows, and that is what made Benchling such a natural place to embed our API. When a result is tied to its design from the start (model version, parameters, target, prior screens, project context, scientists’ in a notebook), the protein designer and/or agent can pick up with no context being lost. Benchling already enables this via its rich meta-data and biology-relevant primitives, and we’re providing the missing piece (a wet-lab!) to its users. Most of the protein design teams we work with already keep their primary R&D records in Benchling, so we’re reducing the overhead of having to manually aggregate our wet-lab results and copy-paste them back.


### **Getting started**


If you're already an Adaptyv customer and a Benchling user, your account team can turn this on as a Canvas app (write in /canvas in Benchling and select the Adaptyv app) or connect the Adaptyv API via MCP to Benchling AI. You will first need an Adaptyv API key, which you can access once you have your Adaptyv Foundry account created. Reach out to us at


***support@adaptyvbio.com***


to get onboarded. Sign up


[here](https://airtable.com/appd15HVMCC04bBE4/pagBuvYKCSSK8UEdK/form)


first to get access to Benchling’s Direct Ordering if you haven’t done so already.


The API documentation is at


[docs.adaptyvbio.com](http://docs.adaptyvbio.com/)


if you would like to see what the same endpoints look like outside Benchling, and the interactive showcase at


[adaptyvbio.com/api](https://www.adaptyvbio.com/api)


walks through every endpoint.
