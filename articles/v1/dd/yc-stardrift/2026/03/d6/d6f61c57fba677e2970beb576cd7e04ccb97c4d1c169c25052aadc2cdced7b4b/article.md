---
schema_version: "1.0.0"
document_id: "d6f61c57fba677e2970beb576cd7e04ccb97c4d1c169c25052aadc2cdced7b4b"
company_key: "yc-stardrift"
company: "Stardrift"
source_id: "yc-stardrift-news-import-0d01feb63e61"
canonical_url: "https://stardrift.ai/blog/why-is-starlink-so-good"
published_at: "2026-03-18T00:00:00+00:00"
first_seen_at: "2026-07-22T14:49:24.360366+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:34b353e8340ab36181ca27b23d06684f2ab6ffc06e11acab370d63a2c25bfbdf"
---

# Why is Starlink on planes so good?

This is the year of good wifi on planes. If you've been on a plane with Starlink recently, you understand why. It beats every other airline wifi option, hands down.


Why is it so good? Because it's a true technological leap over every other form of airline wifi. In particular, older forms of satellite wifi are fundamentally constrained by the laws of physics.


In this article, I'll first explain the two factors behind wifi quality, then walk through the history of how airline wifi has worked. Then I'll use both these ideas to explain why Starlink is so much better than the alternatives.


### What makes airline wifi 'good' or 'bad'?


To understand airline wifi, it helps to understand two concepts that affect wifi speed: latency and throughput.


*Latency* describes how long it takes for a request to go to a server and back. Gamers often know this as ‘ping’. For everyone else, you can think of it as the delay between typing a character in Google Docs and seeing it appear on your screen.


*Throughput* is how much data can be sent at once - this is basically ‘download speed.’


These are usually, but not always related. A good connection has high throughput and low latency. But because of the way airline technology has developed, you’ve historically had to pick one or the other. With Starlink, you don't, and that's why it's so great.


#### Air-to-Ground: the first form of wifi


Air-to-ground towers were the first form of in-flight wifi, starting around 2008. It works via specialized cell towers that beam wifi up to the plane. (In the US, this is most commonly offered by Gogo.)


These have very good latency, but poor throughput, so it feels like websites take forever to load. It’s also very patchy over places where we can’t place towers, like oceans and deserts. To use it, your plane also has to be able to physically see the tower, so it needs to be fairly high up above the horizon before you can start using the wifi.


It was fine for checking email, but not much else. Solving the throughput problem would require looking up — literally.


#### Geostationary Satellites


The next leap came from geostationary satellites. At 36,000 km above the earth, a satellite orbits the Earth at exactly the same speed that it rotates. This makes it look like it's not moving, from the ground.


This is incredibly useful! It means you can predict where a satellite will be. Because it's so far away, you can see it from almost anywhere on the Earth. And so you can blanket the Earth with internet coverage with just 3 satellites. In fact, this is what the Viasat-3 constellation - a real satellite internet constellation - does.


The downsides are many. 36,000 km is no joke:


*Viasat-3's orbit around the Earth.*


This is roughly 10% of the way to the moon. So they're huge, expensive devices, costing hundreds of millions of dollars to launch and run. They're worth it, because the *throughput* (or download speed) it enables is really impressive. On Jetblue's version of this, you can stream Netflix. I've done it!


But the *latency* is terrible. 36,000 km is so far. *Light* travels at about 300,000 km/s. And when you make a request, it has to go from the plane, to the satellite, to earth, back to the satellite, to the plane. That's 4 hops, or about half a second, for your request to just traverse space.


And this lines up with the observed experience of browsing on a plane with satellite wifi - using Google Docs is incredibly painful, because every keystroke takes about a second to sync.


#### 2022+: Starlink


Until now, ATG and geostationary satellites were your only options. Enter Starlink: the only option that doesn't make you choose.


You can stream Netflix. You can use Google Docs. You can call your grandmother while playing Candy Crush as your AI agent auto-scrolls Instagram Reels in the background for you (though this may cause you to get kicked off the plane; caveat emptor).


It has great throughput *and* great latency. But this is only possible because Starlink handles the problems of geostationary satellites with a completely different approach.


What you might think of as one ‘Starlink satellite’ is actually a constellation of 10,000 small satellites orbiting the Earth, in low orbit:


*The Starlink Constellation, surrounding the Earth.*


This has a bunch of interesting implications. For one, because each one is so close to the ground, it can only see a small section of the Earth; this means you *need* a lot of satellites. It also means that as you're moving across the earth, you're often switching between satellites, which requires more complicated engineering to manage these handoffs seamlessly.


More importantly, the satellites are way closer to your plane! So the physical distance your Instagram Reels download has to go is literally 50x shorter than ViaSat, which means latency is correspondingly 50x better. *And* you get better throughput than the ATG towers.


*A distance comparison of ATG towers, Viasat, and Starlink.*


Amazing!


So why didn't we do this sooner? Because the closer a satellite orbits to Earth, the harsher the atmosphere, and the faster it degrades. Starlink satellites only last about 5 years, after which they deorbit themselves and burn up on re-entry.


Yes, you read that right - Starlink satellites are **disposable!** This makes sense if satellites and launches are cheap, and Starlink's are. A Starlink satellite only costs $1-2m to launch into space, making it 100 times cheaper than a Viasat satellite. This is only possible today because SpaceX developed the technology to manufacture and launch rockets at a scale and cost no one else has matched.


And it’s also *incredibly* profitable! It turns out there’s huge demand for good wifi in areas poorly served by fibre-optic. SpaceX made $8b of profit on $15b of revenue[last year](https://www.reuters.com/business/finance/spacex-generated-about-8-billion-profit-last-year-ahead-ipo-sources-say-2026-01-30/?utm_source=chatgpt.com) ; Starlink was 50-80% of that.


So the next time you're scrolling on Instagram - I mean, finishing up a work document - on a plane, look up, and spare a moment to think of the constellation of disposable satellites that makes this all possible.


---


*Want to see which specific flights on a given route have Starlink? Try our Starlink flight search at[stardrift.ai/starlink](https://stardrift.ai/starlink/search) !*


## Stay in the loop


Subscribe to get future posts about Stardrift.
