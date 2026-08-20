---
schema_version: "1.0.0"
document_id: "fc437157c5c45442dd2b37e6017ca25289e5d7936ff40dc54de672bf21f0145d"
company_key: "yc-serif-health"
company: "Serif Health"
source_id: "yc-serif-health-news-import-98e88b19e297"
canonical_url: "https://www.serifhealth.com/blog/keeping-our-network-library-up-to-date-indexing-mrfs"
published_at: "2024-03-05T19:14:00+00:00"
first_seen_at: "2026-07-22T13:10:17.126339+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:0978ef1e965ecfb4902f3a477f1e21731372b6e2db14dd2f852866f50e51d5eb"
---

# Keeping Our Network Library Up to Date: Indexing MRFs

Having an up to date network library is paramount at Serif Health, as we pride ourselves on being able to offer our customers the most up to date price transparency data available.


We do this through a two stage process, first parsing the payor’s CMS defined[Table of Contents (ToC)](https://github.com/CMSgov/price-transparency-guide/tree/master/schemas/table-of-contents) and then from there grouping together the set of MRFs that constitute the different networks that that particular payor provides such as a PPO, HMO, et cetera. In this post, I’ll focus on the challenges we’ve had to overcome in the first stage of the process - parsing a TOC.


On its face this sounds pretty straightforward, but come follow me on a journey into the day-to-day workings of what it means to keep our library up to date given all the nuances of the current state of schema compliance.


### ToC: On the Rails


The majority of payors we consider to be “on the rails,” meaning they release a new ToC every month on a repeatable cadence, and most importantly they follow the CMS defined ToC schema.


For the ToC files that have a date stamp in the file name we were easily able to decipher which day of the month we can expect the new ToC to be released and schedule a job to go grab the updated file on that day.


Most post on the 1st of the month (such as[Priority Health](https://www.priorityhealth.com/landing/transparency)[https://priorityhealthtransparencymrfs.s3.amazonaws.com/2024_02_01_priority_health_index.js](https://priorityhealthtransparencymrfs.s3.amazonaws.com/2024_02_01_priority_health_index.js) ), while others like[Aetna](https://aetna.com/) post on the 5th of the month ([https://mrf.healthsparq.com/aetnacvs-egress.nophi.kyruushsq.com/prd/mrf/AETNACVS_I/ALICFI/2024-02-05/tableOfContents/2024-02-05_Aetna-Health-of-California-Inc-_index.json.gz](https://mrf.healthsparq.com/aetnacvs-egress.nophi.kyruushsq.com/prd/mrf/AETNACVS_I/ALICFI/2024-02-05/tableOfContents/2024-02-05_Aetna-Health-of-California-Inc-_index.json.gz) ), and some others like[Health Alliance](https://www.healthalliance.org/) post towards the end of the month ([https://hawebstorageprod.blob.core.windows.net/transparencymrf/2024-01-26_HEALTH-ALLIANCE-MEDICAL-PLANS-FULLY-INSURED_index.json](https://hawebstorageprod.blob.core.windows.net/transparencymrf/2024-01-26_HEALTH-ALLIANCE-MEDICAL-PLANS-FULLY-INSURED_index.json) ).


While we might prefer if everyone posted on the 1st of the month the important bit is that these payors are consistent with when they are posting their ToC month over month and they fully conform to the CMS defined ToC schema.


### ToC: Off the Rails


Which brings us to payors we consider to be “off the rails.” These payors either do not post on a reliable cadence or do not strictly follow the CMS defined ToC schema, which means we have to apply some extra engineering efforts to make them function as if they were “on the rails”.


We have had cases previously where the payor has shifted their posting schedule from say the 1st of the month to the 22nd of the month. So when we tried accessing their ToC file on the 1st we would receive a nice 404 NotFound exception for example. To get around this we implemented a shim that keeps checking for a new ToC file on a set cron schedule, stopping and then processing the index when a new file has been detected for that month.


As an engineer I find it a bit disheartening that some payors were unable to adhere to a pretty simple json schema definition that was already supplied to them by CMS.


A concrete example is[Hawaii Medical Services Association](https://www.hmsa.com/help-center/transparency-in-coverage-machine-readable-files/) who violates the[reporting_plans definition](https://github.com/CMSgov/price-transparency-guide/tree/master/schemas/table-of-contents#reporting-structure-object) by alternating between an array of reporting plan objects when they have more than one, and just a single object when they have only one reporting plan. The correct way to do this would be to always use an array and in some cases it’s just an array length of 1. You can see their JSON schema inconsistency here:


To get them back “on the rails” we had to create a shim to handle the JSON inconsistencies:


Unfortunately the hardest part about finding these deviations from the schema is that given the sheer size of the ToC files humans cannot easily verify their structure and instead we need to just try and parse them and see if we encounter any errors and then invest the time to debug and fix them.


### ToC: What Happened to the Rails?!?


Which finally brings us to the payors who are nowhere near the rails. These payors tend to fall into one of two categories - either they do not post a CMS defined ToC at all, or their website blocks the automated downloading of files.


An example of a payor who took their own path and does not publish a monthly ToC would be[Select Health](https://selecthealth.org/) who opted to just list out their MRFs in a public directory[https://ebu.intermountainhealthcare.org/selecthealth/transparencyincoverage/](https://ebu.intermountainhealthcare.org/selecthealth/transparencyincoverage/) . For these payors we create a “custom” index for them within our library and attached the listed MRFs to that “custom” index for that month so we are able to treat them within our system as if they had published one. It’s also possible to automate file gathering with a tool like Selenium headless browser; thankfully, the count of payers in this bucket are small enough we don’t yet need to.


Download blocking is a different story - while no one is supposed to block access to machines for the machine readable files, it happens a lot. Some of the download blocking is trivial to work around by adding a user-agent header - this will work for payors like[BlueCross BlueShield of South Carolina](https://www.southcarolinablues.com/web/public/brands/universal/transparency-in-coverage/?branding=sc) ; for others like[Blue Cross Blue Shield of Minnesota](https://www.bluecrossmn.com/transparency-coverage-machine-readable-files) we had to resort to proxying downloads and uploads through our ops team laptops in order to unblock ingestion.


Finally, for payors such as[AmeriHealth Caritas](https://www.amerihealthcaritasnext.com/index.aspx) , all requests return an HTTP 200 even when the files don’t exist which violates some basic web expectations. This creates an issue for automated extraction, since when you try to access a file such as[https://www.amerihealthcaritasnext.com/json/2024-01-10_amerihealthcaritas_vip_next_inc_index.json](https://www.amerihealthcaritasnext.com/json/2024-01-10_amerihealthcaritas_vip_next_inc_index.json) not only do you get an HTTP 200 indicating the URL is valid but it returns HTML when the JSON type was specified:


For this we had to implement custom logic to issue the request for a date, verify the returned contents are JSON, and if not continue onto the next possible date until all dates of the month are exhausted.


### ToC: Final Thoughts


It’s not all doom and gloom! With some engineering finesse as you can see, we’ve been able to automate the ingestion of these payor TOC files month over month with minimal human involvement for updating shims when needed.


In a follow on post I’ll tackle the issues we face in grouping MRFs together.


‍
