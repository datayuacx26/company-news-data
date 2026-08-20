---
schema_version: "1.0.0"
document_id: "675b02802ae0bcd1c70be80a2bdbe6775ffcceaa4ee71a2681b5185c321d6e66"
company_key: "life360-inc-common-stock"
company: "Life360 Inc."
source_id: "life360-inc-common-stock-rss-25c02f0e1eb8"
canonical_url: "https://medium.com/life360-engineering/how-we-leveraged-metrickit-to-gauge-our-new-releases-3675376410b6"
published_at: "2021-08-12T00:52:38+00:00"
first_seen_at: "2026-07-20T04:36:48.728062+00:00"
fetched_at: "2026-07-28T22:26:35.155530+00:00"
content_hash: "sha256:c1ca04bbee93cb562033eb49cd8658a75f85416f306b87c0fd8e51fd2719ccde"
---

# How We Leveraged MetricKit to Gauge Our New Releases

# How We Leveraged MetricKit to Gauge Our New Releases


[Michael Garza](https://medium.com/@mike_garza?source=post_page---byline--3675376410b6---------------------------------------)


6 min read


·


Aug 12, 2021


--


Press enter or click to view image in full size


Delivering the best possible app experience to users has always been of the utmost importance at Life360. Ironically, it can be rather difficult to truly gauge how an app is performing when finally released in the wild. Our codebases are living documents that are constantly changing, yet we expect the reliability of the finished app to be consistent.


We all try our hardest to fill every check box to ensure a stable release: automatic testing, manual testing, alpha builds, beta testers, etc., however, sometimes issues make it past all of our checkpoints. What really concerns us the most is there are issues our users are experiencing that we don’t know exist yet. This is where the introduction of[MetricKit](https://developer.apple.com/documentation/metrickit/) with iOS 13 came in to help fill in the gaps.


Integrating MetricKit is a very straightforward task. Simply import MetricKit, add a` MXMetricManagerSubscriber` , and wait for callbacks with func` didReceive(_ payloads: \[MXMetricPayload\])` . Once Apple sends a payload to the device, the data will need to be uploaded somewhere to be aggregated. There were some hiccups along the way during our integration though. As we experienced issues, it was nearly impossible to find any information on the internet, even in Apple’s Developer forums. Below are some oddities I discovered that will hopefully save you frustration in the future when it becomes time for implementation.


## **Implementation Pitfalls**


Press enter or click to view image in full size


### Junk Data


Apple will send junk data in MXMetricPayloads. We started noticing some weird values in our data such as negative numbers and MXAverages with an average, but a count of 0. I dug into my implementation on the iOS side and could not find any errors. Only after finally deciding to log the raw payloads during alpha testing did I discover that this data was sent to us malformed. MXMetricPayload’s jsonRepresentation() function isn’t always perfect either, as sometimes we would see values encoded this way 69,940 ms and also 69 440 ms . For the best results, clean up any junk on your end before sending the data along.


### Duplicate Payloads or no Payloads at All


MXMetricPayloads are finicky and can also be sent twice. Testing the receiving of payloads requires an actual iOS device and Xcode. I found that I would not receive live payloads on my device after I have been testing debug payloads (test payloads sent through Xcode) for some time. Eventually after a few days to a week, live payloads would start being sent to my device again. I couldn’t figure out any rhyme or reason, just something I had to work around. If you have a designated debug device, I would highly recommend using that for your MetricKit testing. I’ve also seen payloads get sent twice, sometimes within the same delegate callback. Apple says we only ever get one report per 24 hours, but there is a bug out there where you will receive duplicates. Luckily, duplicate payloads will have the same start and end timestamps. A simple workaround is to keep a light cache of these timestamps for already processed payloads.


### Missing Certain Properties Within a Payload


Are you missing some of your histograms at times and scratching your head as to why? Histograms are only sent when they have data for that 24 hour period. While the key/value pairs will return 0 for values with no data during a period, histograms are omitted entirely. It took a little bit of digging into payloads and the event stream of users to figure this out. If there are no metrics for` histogrammedApplicationResumeTime` for example, it is because the user did not resume the app during that period.


### Unit Tests Failing Due to iOS Version


Unit tests will fail with a cryptic error message if not on iOS 13+. This one will drive other developers insane as they try to figure out how they broke the MetricKit tests. Turns out they just ran the tests on iOS 12 or lower. Even though the tests around MetricKit are wrapped in iOS 13 checks, running all unit tests locally with CMD + U will run everything, even if the conditions are wrong.


## **Let’s talk about the good**


Press enter or click to view image in full size


After we process and normalize the payloads on the device, we send the data as an analytics event to our provider. From there, we can create graphs of MetricKit data that we use to monitor our releases as they ramp up to 100% as well as some of our other use cases. Below are a few examples of these graphs.


The real strength comes during our new app version’s release. If any of our battery or performance graphs are off unexpectedly, we can pull back a potentially harmful release before the majority of our userbase is even aware of it. The graphs show that our 21.1 release went smoothly with basically zero change in our Peak Memory Usage and Cumulative Logical Writes.


Peak Memory Usage


Press enter or click to view image in full size


Cumulative Logical Writes


Press enter or click to view image in full size


MetricKit also enabled us to finally answer questions with more certainty for our users and testers. If a tester noticed irregular battery drain on a certain day over others, we could see that their brightness was set to 100% that day instead of their usual 30%. If a user is worried about our app sending data over cellular and not wifi, we can prove not only how much we sent during that period, but over which type of connection.


While we do look at MetricKit data during our Beta testing, the small sample sizes can create some false positives in the data. A few in-house testers doing days of regression tests can easily skew numbers in such a small sample size. I’d recommend not taking any oddities at full face value when using a small sample size.


We still have more we can do with MetricKit in our future. iOS 14 introduced MXDiagnostics which look promising. We also want to roll out MetricKit to more, if not all of our users. While the team I’m a part of is in charge of monitoring the app and MetricKit, I would like to create a mechanism for other developers to utilize MXSignPosts. Creating an elegant way to consume histograms while not prematurely normalizing the data is also on my radar.


All in all, implementing MetricKit has proven to be a positive endeavor and I’d highly recommend adopting it to any iOS developer.


## Come join us


Life360 is the first-ever family safety membership, offering protection on the go, on the road, and online. We are on a mission to redefine how safety is delivered to families worldwide — and there is so much more to do! We’re looking for talented people to join the team: check out our[jobs page](http://www.life360.com/jobs) .
