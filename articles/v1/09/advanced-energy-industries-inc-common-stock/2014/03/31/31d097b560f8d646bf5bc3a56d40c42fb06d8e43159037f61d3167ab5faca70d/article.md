---
schema_version: "1.0.0"
document_id: "31d097b560f8d646bf5bc3a56d40c42fb06d8e43159037f61d3167ab5faca70d"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/choosing-between-dc-and-rf-for-sputtering-applications/"
published_at: "2014-03-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T22:27:47.856975+00:00"
content_hash: "sha256:5ec2c85b2cacf59c2e499332a24b68807a196f43efdbeb2eacc101f7e3fcc28e"
---

# Choosing Between DC and RF for Sputtering Applications

I get this question a lot: “How do I know when to use DC and when to use RF for a sputtering application?”


Of course, the first thing to consider is film requirements. Typically, RF makes a better thin film than DC, pulsed DC, or AC. The RF-sputtered film will be smoother and have better packing density. RF also deposits the film at about 20% of the DC rate. If you want to sputter using DC, pulsed DC, or AC, you must have a conductive (or semi-conductive) target. I always check the conductivity of a target by placing my ohm meter probes anywhere on the target surface. I need to see less than 650 kΩ.


We used to only have analog ohm meters with a needle. A “wiggle” on the meter confirmed that we could use DC, pulsed DC, or AC. Now that we have digital meters, it’s necessary to put a value on the “wiggle.” On the digital readout, 650 kΩ seems to coincide with an analog meter “wiggle.” This is why we need to use a dopant such as Al on the Si targets for DC-sputtered SiO2 films. The dopant makes the target surface semi-conductive enough (about 400 kΩ and less) to successfully sputter with DC, pulsed DC, or AC. We can also sputter this same doped target with RF. We would need RF to sputter SiO2 from a quartz target, as its resistance reading on the target surface is infinite. RF hasn’t commonly been used with rotatable magnetrons yet, as the end blocks tend to bind up and leak water with the 13.56 MHz RF signals. However, we are safe with DC, pulsed DC, and AC on rotatables.


Good luck and be safe.
