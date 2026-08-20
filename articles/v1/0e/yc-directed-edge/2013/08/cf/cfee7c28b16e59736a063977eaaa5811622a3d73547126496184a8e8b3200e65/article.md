---
schema_version: "1.0.0"
document_id: "cfee7c28b16e59736a063977eaaa5811622a3d73547126496184a8e8b3200e65"
company_key: "yc-directed-edge"
company: "Directed Edge"
source_id: "yc-directed-edge-rss-7c8e8fe81473"
canonical_url: "https://blog.directededge.com/2013/08/06/support-screenshots-with-monosnap-and-s3/"
published_at: "2013-08-07T01:35:55+00:00"
first_seen_at: "2026-07-27T01:55:50.875550+00:00"
fetched_at: "2026-08-20T02:30:11.821086+00:00"
content_hash: "sha256:f162f457b7b7e1773884dca5d7ac2c223a583b7aca029966af9ec53d36da1384"
---

# Support screenshots with Monosnap and S3

I remember the first day that I discovered[Skitch](https://evernote.com/skitch/) . Skitch let you take screenshots of an application or region on your screen and quickly post those online with a handy link that you could show to others. You could draw arrows or other simple annotations before flipping the switch to send the image to the interwebs.


Since Directed Edge moved from being an API- and developer-only company into also supporting[e-commerce platforms](https://shopify.directededge.com/) , taking screenshots to help out our customers is a regular part of our customer support process. Sometimes a screenshot is worth a thousand (well, or at least several dozen) words.


Skitch was eventually bought by[Evernote](https://evernote.com/) , and as an Evernote user I appreciate the Skitch integration, but as the interface has gradually gotten clunkier for the good old taking of support screenshots, and since we’ve come to lean heavily on them, I’d been on the lookout for a more elegant solution.


**Hello, Monosnap.**


I then stumbled across **[Monosnap](https://www.monosnap.com/welcome)** , which has free integration with **Amazon’s[S3](https://aws.amazon.com/s3/)** . I thought I’d give a quick run down of how we’ve moved from Skitch to using Monosnap + S3 to serve up “show.directededge.com”


Monosnap itself is pretty straightforward. You take screenshots, potentially annotate them and it uploads them somewhere. Â It hearkens to the Skitch of old. Â However, instead of only uploading images to its own server, it also allows you to upload images to various cloud services.


We were primarily interested in S3, since we’re using it for image storage in another recommendations product that we have coming down the pipeline.


So, first we created a new S3 bucket. Â We typically just name our buckets after the site that they’ll be backing:


The rest of the default bucket settings are fine.


**The “show”.**


Now, with s3 you can typically access your buckets at a URL like:


https://s3.amazonaws.com/show.directededge.com


However, you can also rearrange that URL to a DNS CNAME friendly format:


http://show.directededge.com.s3.amazonaws.com/


So, now we need to head over to our DNS provider (Rackspace in our case) and enter in an appriate CNAME record, to get those lovely, Directed Edge-looking, URLs.


As is usual with CNAME records, we simply had to enter a record pointing from:


show.directededge.com


To:


show.directededge.com.s3.amazonaws.com.


(Note the period on the end, standard with CNAME records.)


With that the DNS and storage bits are done.


**Configuring Monosnap**


From there we just needed to add a couple values to Monosnap. Â As seen in the configuration interface above, we had to enter our AWS API key and access token. Â Unfortunately, creating a special account for Amazon’s[Identity and Access Management](https://aws.amazon.com/iam/) (IAM) didn’t seem to get along well with Monosnap, so we were forced to use our master token. Â Were we using S3 for more than image storage, this would probably be somewhat troubling.


However, once we entered our master credentials, we were able to select a bucket and begin uploading support screenshots to our new, pretty URL. Â For example:


[http://show.directededge.com/Directed_Edge_for_Shopify_20130807_012624.png](http://show.directededge.com/Directed_Edge_for_Shopify_20130807_012624.png)
