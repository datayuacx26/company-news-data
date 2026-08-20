---
schema_version: "1.0.0"
document_id: "7680c24cdcc1d6ffac86df4e0a5f0a23780f0dadc39c317d2f6d609aad089537"
company_key: "yc-mars-auto"
company: "Mars Auto"
source_id: "yc-mars-auto-rss-2183801a7a92"
canonical_url: "https://blog.marsauto.com/marking-an-important-milestone-at-mars-auto-a864a77a668d"
published_at: "2020-12-07T14:28:47+00:00"
first_seen_at: "2026-07-25T13:21:51.802210+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:1fc67bba6515ffa01061a28e752e1f0bf1b1445f0322d87aa1ad616867fc20ed"
---

# Marking an important milestone at Mars Auto

# Marking an important milestone at Mars Auto


[Gyuri Im](https://medium.com/@imgyuri?source=post_page---byline--a864a77a668d---------------------------------------)


4 min read


·


Dec 7, 2020


--


One year ago in November 2019, our team gathered together for an all hands meeting. Although we have such meetings twice a week, this one was different. We were seriously considering stopping development of all new features in the pipeline and go back to the drawing board.


The motivation for such a drastic move was not from a single event, but rather a culmination of our experience within the company. At that time Mars Auto was part of the Y Combinator Winter 19 batch, and as Demo Day was approaching we wanted to show something to potential investors.


### Demo Day


Since bringing our vehicle from Korea to the U.S was not feasible, we opted to create a self driving video which could demonstrate that our technology was reliable enough to be used in public roads. At that time most of the self driving demos that were posted online were heavily edited, which basically makes them useless to properly assess whether the trip was done fully autonomously. With an anti-thesis approach we picked the longest route in Korea and created a video which is basically a fast-forwarded version of the 5.5 hour fully autonomous trip from Seoul to Busan. The video also happened to be the longest self driving drive that was online at the time (the record was surpassed afterwards).


Mars Auto’s first self driving video, created with iMovie.


Fortunately Demo Day was a success and we were able to reach our fundraising goal within a short amount of time. However upon self reflection we were getting concerned that we may hit scalability issues across many different areas. The truck had to be retrofitted since our system was using too much power, and the amount of data that was being collected vastly exceeded the amount of annotation we could do.


### The hard decision


At this point we had to decide whether to keep adding new “features” into our self driving system, or redesign the system with the knowledge we gained until now. Normally companies measure their progress in terms of the number of features shipped, and therefore naturally most companies focus on adding more features while only a subset of the team is tasked with maintaining the system. Also without new features to show, the public assumes that the company is making no progress, or worse default dead.


## Get Gyuri Im’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Fortunately for us we secured enough funding to keep us warm for a few years, and since nobody cares about a small startup in South Korea we didn’t need to think about what the public would think if we shipped nothing new for a year. In some ways this was a rare opportunity for us to get rid of all the technical debt we accumulated over a last two years.


We spent the bulk of 2020 revamping our architecture from the ground up. Trucks operate in the highway at a high speed and is extremely heavy compared to normal passenger cars, therefore it is critical that our system works extremely well within its operating domain. Therefore we focused on what we believed was the correct approach instead of just building something that would be changed or removed in the future.


Virtually everything from the data generation pipeline down to the hardware design was rewritten, and with the extremely hard work of our team we managed to improve our system in every metric we keep track of. The power consumption of the new system is just 4% of what it originally used and now the system can be reliably powered with just the existing battery inside the truck. The number of parameters inside our model has significantly increased while being more than 10 times faster. Our data generation pipeline now has no human in the loop, and our system is more fuel efficient than the average human driver.


### From testing to shipping


Press enter or click to view image in full size


Our newest truck


With the new system we obtained our second driverless permit. Mars Auto is the only company in Korea to operate self driving trucks, and fortunately many logistics companies were interested. We picked[LogisSquare](http://www.logisquare.co.kr/) , which is one of the top logistics company in Korea and handles over 400,000 shipments per year, as our first logistics partner.


[Starting from November 11th](http://clomag.co.kr/article/3521) Mars Auto is moving cargo everyday. There is an operator who manages the cargo and monitors the system, but the bulk of the route is driven autonomously. Mars Auto is the first company in Korea to brake out of the pilot phase and use autonomous technology in a commercial environment.


Press enter or click to view image in full size


Testing our system with real cargo


Going forward, we will continue to develop our system and test it across the country until it can be safely deployed to over half a million trucks operating in the industry.


If the work sounds interesting,[join our team](https://www.notion.so/Mars-Auto-is-hiring-de1d2a9da06f4845a6100b82d692a689) **** and build the truck for the future.
