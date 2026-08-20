---
schema_version: "1.0.0"
document_id: "88134de9b04df88668595e6546d1a5590f30abde1002a4f198d54083316c8778"
company_key: "yc-bodysim-by-well-principled"
company: "BODYSIM by Well Principled"
source_id: "yc-bodysim-by-well-principled-news-import-e31519db14e6"
canonical_url: "https://bodysim.com/blog-posts/withingsblogpost/"
published_at: "2025-02-21T00:00:00+00:00"
first_seen_at: "2026-07-26T23:53:55.714143+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:e82f761d44247b7b568141f2ee992a6da7968ff11deeba66e217e0d64c543d85"
---

# Not All Body Composition Scales are Alike

# The Quest for Accurate Body Composition: Why We Chose the Withings Body Scan


## Measuring Body Composition with Confidence


When we started building BODYSIM, we knew we’d have to start from very accurate baselines for our users’s body fat and lean mass. It’s the only way to get a decent starting point for knowing your TDEE (total daily energy expenditure), and it’s the variable that our users are looking to manipulate. What’s the point of a really great model if you can’t measure the outcome to any level of accuracy? So we faced a crucial decision - which consumer-grade device could provide the daily measurements we needed to build accurate predictive models of body composition?


The answer wasn’t immediately obvious. The market is flooded with bioelectrical impedance (BIA) scales, each claiming superior accuracy, and some as cheap as $20. But as scientists and engineers obsessed with precision, we needed more than marketing claims. We needed a device that could provide research-grade data quality while being practical for daily home use.


To start, to get the absolute best-in-class measurement for body composition, we turn to the DEXA scan. We dive a little deeper into why the DEXA is so important[here](https://bodysim.com/blog-posts/focus-on-body-composition/) , but it’s not practical to get a DEXA every day, or even once a week, so we focus on a quarterly DEXA scan to adjust our user’s baseline. In addition to the DEXA scans every quarter, we need to provide the model with a daily signal, and for that we chose the Withings Body Scan.


## The Technical Edge: Why the Body Scan Stands Out


After extensive research and testing, we discovered there are 4 key requirements for a body composition scale that works excellently with BODYSIM:


1. Octopolar
2. Multifrequency(3+)
3. Multi-weight sensors
4. WiFi


The key differentiator lies in the Body Scan’s sophisticated octopolar BIA technology. Unlike traditional scales that only measure through your feet, this device analyzes both sides of your body independently through multiple contact points. Think of it as the difference between a basic camera and a high-resolution 3D scanner - both can capture an image, but the level of detail and accuracy is dramatically different.


This matters because our bodies aren’t symmetrical columns of tissue. We’re complex organisms with varying distributions of muscle, fat, and other tissues throughout our frame. The Body Scan’s ability to measure these variations provides crucial data that helps BODYSIM create more accurate simulations of your body’s composition.


But the technology goes even deeper. The scale employs multiple (three in fact) electrical frequencies to analyze different aspects of body composition. Most consumer scales use only 1 or 2 frequencies resulting in lower accuracy. Here’s how they work:
- Lower frequencies help us understand extracellular fluid distribution
- Higher frequencies penetrate cell membranes to assess intracellular composition
- The combination provides insights into everything from muscle hydration to glycogen storage


When you’re weighing-in everyday, collecting that data is important. Many competing scales have either no connectivity–forcing you to manually type in your measurements each day. Some have Bluetooth connectivity which adds another manual syncing step to your routine–which we’ve found to be flakey and impractical for reliably capturing your data each day. The Withings Body Scan is one of very few with full WiFi connectivity that automatically uploads your daily measurements to the cloud even without manually syncing on your phone or even having your phone close in range. These measurements then sync from the cloud to your Withings app and Apple Health.


Finally, the Withings Scale uses multiple weight sensors and gives you real-time feedback if you’re not distributing your weight evenly. This allows your body weights to be accurate within 50 grams or 0.11 pounds, which is exceptional.


When integrated with periodic DEXA scans and our advanced physiological and metabolic models, these daily measurements allow us to track subtle changes in body composition that would be impossible to detect with conventional devices.


## Real-World Impact: Beyond the Numbers


What really sealed our decision to partner with Withings was seeing how this technology translated into real-world results for our users. The consistency and reliability of the Body Scan’s measurements combined with their BODYSIM digital twin have been crucial in helping people understand how their bodies respond to different nutrition and training interventions.


For instance, we’ve observed that users can track meaningful changes in regional muscle development, helping them validate whether their training programs are effectively targeting specific areas. The scale’s ability to detect subtle shifts in body composition has also proven invaluable for identifying early signs of successful interventions, long before they become visible in the mirror, or even on the 3D scan.


---
