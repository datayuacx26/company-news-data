---
schema_version: "1.0.0"
document_id: "302acf994e6fd30087567b08ab5e7eab75f851186a02b82835fbd734fcadab73"
company_key: "yc-korrai"
company: "KorrAI"
source_id: "yc-korrai-news-import-a686cf049d8d"
canonical_url: "https://www.korrai.com/blog/misconceptions-of-insar-for-infrastructure-monitoring"
published_at: null
first_seen_at: "2026-07-22T01:41:37.374471+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:aff8589fb3a78d30de9a4c6eb46cc645bf9715e5502442cb478bb3054a116faf"
---

# Misconceptions of InSAR for Infrastructure Monitoring

Infrastructure stability is more crucial today than ever, with the risks posed by ground deformation impacting everything from mines and pipelines to urban centers and critical public infrastructure. The emergence of Interferometric Synthetic Aperture Radar (InSAR) has proven revolutionary, providing powerful insights into ground motion with millimetric precision.


Yet, despite these capabilities, many infrastructure managers, geotechnical and safety engineers still harbor misconceptions about the technology. One reason is the technical jargon that service providers throw at you, or the misunderstanding of processes used to turn raw InSAR into polished insights.


These misconceptions often result in missed opportunities to proactively manage infrastructure risks, potentially leading to costly incidents that could have been anticipated.


Our goal here is straightforward: to clear up common misunderstandings about InSAR, providing clarity that empowers decision-makers to leverage it effectively.


## Misconception 1: InSAR Data doesn’t carry industry-grade accuracy


When considering InSAR, some infrastructure managers understandably question its accuracy. And we understand why. After all, how could a satellite hundreds of kilometers away precisely measure millimeter-level ground movements?


**Here’s the reality:**


Yes, it can. InSAR's accuracy isn't science fiction; it's a well-operated mix of satellite radar and data science. The accuracy becomes highly reliable for gradual movement when there's a robust processing chain in place.


**How does InSAR achieve millimeter precision?**


InSAR works by comparing radar signals reflected from the ground across multiple satellite passes. These signals behave like repeating waveforms. Even the slightest ground shift causes a change of phase in the returning wave.


> Explore our[introductory guide to InSAR](https://www.korrai.com/blog/introduction-to-insar-guide-for-geotechnical-engineers-risk-analysts-and-safety-engineers) , the essential fundamentals every geotechnical engineer and infrastructure operator should understand.


At KorrAI, we effectively filtering out atmospheric disturbances, orbital variances, and other noise with our proprietary algorithms to isolate true ground deformation with consistent millimeter precision. With our[Ground Motion Monitor](https://www.korrai.com/products/ground-motion-monitor) platform, we make ground deformation analytics highly reliable for critical infrastructure operators.


## Misconception 2: InSAR Monitoring Is Too Expensive


If you've ever managed a monitoring budget, you've probably asked: *"Is InSAR going to burn a hole in it?"* It's a fair question. Satellite-based systems sound costly, and many assume they’re reserved for national space agencies or megaprojects.


**Reality check:** InSAR is not only cost-effective, it's often significantly cheaper than traditional ground-based monitoring, especially at scale. But there are expensive set of SAR data provided by private or tasked constellations.


### Let’s put it in perspective:


Traditional methods like inclinometers, tilt sensors, or extensometers are good for localized data. But when monitoring a large dam, a mining operation, or a city-wide utility corridor, costs balloon quickly. Equipment, maintenance, frequent site visits, and manual labor costs add up.


Some instruments are huge, like the Ground-based SAR (GBSAR) setup. For inspection of multiple locations, it’s difficult to carry the instruments everywhere.


In contrast, InSAR monitors vast areas (hundreds to thousands of square kilometers) from space, often using freely available satellite data such as from ESA's Sentinel-1 program. What you pay for are the data, processing architecture, analytics, simplified insights, and expertise, not an army of ground sensors or day/night shifts of extra workforce.


This affordability isn't just about cost savings; it’s about proaction. When you factor in the cost of failure (asset damage, insurance claims, regulatory fines) that early insights can prevent, InSAR becomes your front-line saviour.


**And for small to medium projects?** It still holds value. The price scales with the area, so even modest footprints can benefit from early warning of fatal risks.


## Misconception 3: InSAR Doesn’t Work in Forests, Hills, or Snow-Covered Terrain


It’s a familiar concern: *“Sure, satellites might see buildings in a city. But how can they possibly tell what’s happening under a forest canopy, across a rocky slope, or in the dead of winter under a thick blanket of snow?”*


**Reality: E** nvironmental conditions indeed pose challenges, but modern InSAR techniques are far more versatile than many assume. With the right wavelengths, data processing technique, and supplemental measures, InSAR can, and does, work reliably in complex terrains.


### Overcoming Nature’s “Noise” in InSAR


- **Forests and Vegetation:**


It's true that dense vegetation, especially with shorter-wavelength SAR like X-band, can reduce signal coherence. But that doesn’t mean InSAR is blind there. Techniques like stacking (multi-temporal analysis), temporal filtering, and the use of longer-wavelength L-band satellites help extract meaningful trends. Plus, placing a few low-cost **corner reflectors** on the ground in strategic spots can dramatically enhance signal quality.


> *To better understand which wavelength and technique suits right for your project, check our previous article on “*[Choosing the right Data and Method”](https://www.korrai.com/blog/insar-in-practice-choosing-the-right-data-for-your-project)


- **Rugged Terrain and Steep Slopes:**


While extreme topography can introduce some geometric distortions (like layover or shadow), there is no foolproof solution to it. But they aren’t showstoppers. Modern techniques get us to reliable data collection. By combining ascending and descending satellite passes, or integrating with GNSS and optical data, these distortions can be resolved. InSAR, when paired with smart modeling, becomes a strong performer even in hilly or mountainous regions.


> To improve reliability in these environments, KorrAI uses GNSS calibration to anchor InSAR measurements to real ground-truth. Our proprietary method, involving virtual points and correction surfaces, not only corrects geometric errors, but significantly boosts accuracy across uneven terrains.[Check out a graphical explanation of our GNSS calibration methodology](https://www.korrai.com/blog/enhancing-insar-accuracy-with-gnss-calibration)


- **Snow and Ice:**


Snow may alter surface reflectivity or affect radar backscatter. But again, this doesn’t render InSAR useless. Analysts often account for seasonal cycles, filter out low-coherence acquisitions, and fuse InSAR results with other sensors (like GNSS or optical change detection) for dependable insights. In fact, patterns of seasonal snow deformation often help differentiate between natural cycles and actual ground movement.


This misconception, that InSAR is limited to urban settings, often stems from legacy thinking. Today, it’s used effectively in forested mining regions, snow-covered alpine railways, and remote mountainous oil pipelines. With the recent NISAR launch, the dual-band (both L-band and S-band) platform of this ISRO-NASA maiden collaboration will push the envelope further for InSAR-based monitoring in snow-covered and forested areas.


## Misconception 4: InSAR Is Too Complex to Understand or Use


You’re busy. You’ve got aging assets to monitor, reports to submit, and a team that already has more dashboards than time to check them. The last thing you want is a space-age tool that feels like it requires a physics degree just to log in.


**Reality:** While the science behind InSAR is complex, using it doesn’t have to be. In fact, with modern platforms, InSAR insights are easier to access and act on than ever before. Platforms like KorrAI’s explore require just a few hours of training and 2-3 days of familiarity.


### The “Black Box” Problem


Much of the hesitation comes from what professionals call the “black box effect.” InSAR feels mysterious. The idea that satellites are measuring invisible shifts from space, turning them into colorful maps of deformation, sounds impressive, but also abstract.


This perception isn’t helped by many InSAR providers who lean heavily on radar jargon and algorithm talk, forgetting that end users want clarity, not a crash course in remote sensing.


The good news? That’s changing. Tools like Korrai’s[Ground Motion Monitor](https://www.korrai.com/products/ground-motion-monitor) are designed to deliver insights, not just data. With clear visualizations, color-coded risk overlays, and time-series graphs, engineers can quickly spot what’s stable, what’s shifting, and what needs attention. No radar PhD required.


Very soon, getting answers about infrastructure risks will be as simple as asking your AI co-worker. It will instantly analyze all relevant assets, files, live sensor data, and historical records, delivering source-backed insights through interactive maps, graphs, and summaries.


That’s the power of[TRAIL](https://www.korrai.com/products/trail) , our AI co-worker for mines and infrastructure risks.


While we prepare for TRAIL’s public launch, you can submit an expression of interest for a[pilot program](https://www.korrai.com/trail-pilot) .


### From Complexity to Confidence


- No need to “interpret” raw phase data, modern tools do that.
- No coding required: Dashboards are built for infrastructure professionals, not radar scientists.
- Minimal training needed: Most users get up to speed in under a day with guided tutorials or support.


Bottom line? InSAR’s complexity is front-loaded in the backend. For you, the user, it’s a matter of viewing clear, intuitive signals, making better decisions, faster.


## Misconception 5: InSAR Is Real-Time


When people hear the word “monitoring,” they often picture alarms triggered the moment something goes wrong. So it’s natural to wonder:


*“If InSAR doesn’t deliver data by the minute, how can it possibly help manage high-stakes infrastructure like tunnels, rail corridors, or data centers?”*


**Reality:** InSAR isn’t designed for reactive alerts. It’s built for **proactive foresight** . And that’s exactly what most infrastructure failures need.


### The Value of Seeing Trouble Coming Early


The goal isn’t to notify you after a slope collapses or a foundation begins to fail. It’s to help you see the early indicators, long before that moment arrives. InSAR excels at detecting **slow-moving deformation trends** that build over weeks or months. These are the subtle warning signs that often go unnoticed by conventional systems until it’s too late.


InSAR detects slow, subtle, cumulative movements way before cracks appear, roads buckle, or slopes collapse.


> Technical Note: Why InSAR Isn’t Real-Time
>
>
> Satellites like Sentinel-1 pass over a location every 6 to 12 days. Processing takes additional time to remove atmospheric noise, align orbits, and extract real deformation. This means new data arrive in days or weeks, not seconds. But ground motion, in nature, is gradual. That’s why this timeline fits the threat.


**So, do you totally discard real-time sensors for ground movement?**


No, they’re valuable, especially for sudden events or rapid movements - they take the charge. Think of it this way: InSAR indicates unusual acceleration near a critical region. We get the signal that there's need for deeper localized inspections. It helps task high-resolution data and strategically place ground sensors to capture continuous updates, often called 'Tip and Cue' approach. Because you noticed the acceleration trend in advance, you have a mitigation strategy already running, relocations done, labor safety ensured, and regulatory scrutiny escaped.


If you’re developing an infrastructure risk strategy and need a practical framework for combining these methods, check out our post on[Integrating InSAR into Infrastructure Strategy](https://www.korrai.com/blog/integrating-insar-into-infrastructure-strategy-a-playbook) .


## Quickfire


Not every misconception needs a long story. Some just need a straight answer. Here’s a roundup of smaller but persistent misunderstandings that deserve a reality check:


### “InSAR only detects vertical movement.”


**Not quite.**


InSAR measures displacement in the satellite’s line-of-sight (LOS), which is usually at an angle, not strictly vertical or horizontal. By combining ascending and descending passes or calibrating with GNSS data, both vertical and horizontal components can be inferred reliably.


[Read a longer explanation.](https://www.korrai.com/blog/introduction-to-insar-guide-for-geotechnical-engineers-risk-analysts-and-safety-engineers#:~:text=Sight%20(LOS).-,Line%2Dof%2DSight%20(LOS)%3A%20The%20Direction%20InSAR%20sees,helps%20track%20both%20motion%20reliably%2C%20even%20if%20the%20motions%20unfold%20slowly.,-Radar%20Viewing%20Geometry)


### “InSAR can only be applied periodically; long-term historical analysis isn’t possible.”


**It’s actually the opposite.**


InSAR shines when it comes to long-term trends. With satellite archives stretching back to the 1990s, InSAR lets you look backward, sometimes years, to understand how the ground has changed over time. Perfect for establishing baselines or identifying pre-failure signals.


### “InSAR doesn’t fit into our existing monitoring workflows.”


**Things have changed forever.**


Modern InSAR platforms offer GIS-compatible outputs, APIs, and dashboards that plug into your current systems with minimal disruption. It complements existing instruments, providing broad coverage where sensors can’t go.


## Wrapping Up


Misconceptions, when left unchallenged, can become blind spots. Infrastructure blind spots are expensive. As we've seen, InSAR is neither too complicated, nor too limited, nor too costly. It’s a powerful tool when understood correctly and applied thoughtfully.


Whether you’re managing assets beneath a forest, tracking motion along a rail line, or underwriting risk for facilities across varied terrain, InSAR can offer the early indicators you need to stay one step ahead. The key is knowing when, where, and how to apply it, and separating legacy myths from present-day reality.


We’ve built our platform with a suite of tools that simplify adoption.


Want to see how we can help?[Book a meeting with us today.](https://www.korrai.com/book-meeting)
