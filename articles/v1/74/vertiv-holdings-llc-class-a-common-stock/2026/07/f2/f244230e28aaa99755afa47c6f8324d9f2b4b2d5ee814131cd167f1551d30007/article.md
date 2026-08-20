---
schema_version: "1.0.0"
document_id: "f244230e28aaa99755afa47c6f8324d9f2b4b2d5ee814131cd167f1551d30007"
company_key: "vertiv-holdings-llc-class-a-common-stock"
company: "Vertiv Holdings LLC"
source_id: "vertiv-holdings-llc-class-a-common-stock-news-import-f0997059a52b"
canonical_url: "https://www.vertiv.com/en-us/insights/articles/educational-articles/vertiv-power-innovation-day-2026-adapting-power-infrastructure-to-aidr2.1-workloads/"
published_at: null
first_seen_at: "2026-07-22T18:48:52.120325+00:00"
fetched_at: "2026-07-28T22:07:07.393518+00:00"
content_hash: "sha256:e2d4acde94435d4128ef8711d20cf7a006769acba8d385ccdf459a4ab941619a"
---

# Vertiv Power Innovation Day 2026: Adapting power infrastructure to AI workloads

AI workloads are exposing limits in today’s data center power infrastructure. In this episode of[Vertiv Power Innovation Day 2026](https://www.vertiv.com/en-us/about/news-and-events/events/2026/vertiv-power-innovation-day-2026/) ,[Tony Gaunt](https://www.linkedin.com/authwall?trk=bf&trkInfo=AQGiT9OxkYDhlQAAAZ63F1wwvmCDcr4MaOP1hVw7Zz9QSGvTgtEZuPJG6CweHmml0wCmQRe_qIUnpNZJS1Gxz9xfEf1MZYhEvs0SJmTUGb7rrPwjc4jHjUBHoaTvNbw-j45tKfY=&original_referer=&sessionRedirect=https%3A%2F%2Fwww.linkedin.com%2Fin%2Ftonygaunt1%2F) (Vertiv) and[Peter Gaston](https://www.linkedin.com/authwall?trk=bf&trkInfo=AQH8M11NREF3QAAAAZ63F2QAMVBUDVtLnUc0-9Gw7GLHSKurSOhv4SibPilTv89JuCL24Y-HflNBXEWKoruoKcWdNj-cPoHP9FMo1uJoGyii3oKWn3io1Nz4wjxDVAmAZ30zP2o=&original_referer=&sessionRedirect=https%3A%2F%2Fwww.linkedin.com%2Fin%2Fpeter-gaston-191a9a29) (ST Telemedia Global Data Centres), with DatacenterDynamics’[Tam Pledger](https://www.linkedin.com/authwall?trk=bf&trkInfo=AQH8M11NREF3QAAAAZ63F2QAMVBUDVtLnUc0-9Gw7GLHSKurSOhv4SibPilTv89JuCL24Y-HflNBXEWKoruoKcWdNj-cPoHP9FMo1uJoGyii3oKWn3io1Nz4wjxDVAmAZ30zP2o=&original_referer=&sessionRedirect=https%3A%2F%2Fwww.linkedin.com%2Fin%2Ftam-pledger%2F) , break down what live testing uncovered about rapid, unstable load changes, and why current designs can fail under these conditions.


#### Tam Pledger, Digital Content Manager, DatacenterDynamics (DCD): AI workloads are introducing a new level of variability and power demand. What has changed inside the powertrain —and how does that translate into operational risk?


##### Tony Gaunt, Vice President of Product Management for Asia, Vertiv:


For years, central processing unit (CPU) workloads ran stable, typically at around 5–10 kilowatts (kW) per rack. With graphics processing units (GPUs) driving racks through 140 kW, the transients — and the speed at which they're switching — are creating volatile steps in load. This takes us back to the fundamentals of electrical engineering, tuning everything effectively right the way through — from generators, common points of coupling, and the impacts of any other services connected. GPU loads at high densities drive the same considerations across all facility services, including cooling.


#### Tam: You’ve run real-world AI workload simulation on live power infrastructure. What did the test involve—and what did it reveal?


##### Peter Gaston, Senior Director, ST Telemedia Global Data Centres:


No one has seen loads that turn off within milliseconds. Our particular concern was how the generators would respond to this and when in line with the other power systems. We tested real generators, UPS systems, and batteries using an AI simulator based on actual data center workloads.


The generators kept supporting the load but couldn't keep 100% stable. The UPS is looking for a stable point to transfer to and it never saw that, so it would never allow the transfer. You could go to bypass for maintenance and never be able to go back, because the generator load just isn't stable.


#### Tam: Why is system-level testing essential — and why is testing components in isolation no longer enough?


##### Peter:


We’re talking about scenarios where the effect of a load on an equipment changes the response of that equipment, which will then have an impact on the equipment either upstream or downstream. If you take just one piece of equipment — a generator, a UPS, a battery — and test it in isolation in a lab, you’re not getting those knock-on effects that you see when you join them all together. We have to do real-world testing with all the systems together. Ultimately, it’s much better than simply looking at it on paper and hoping it works.


##### Tony:


Generational changes in technology and firmware could introduce mismatches in operating parameters and transfer times. We used to design data centers purely on volume of racks and kilowatts per rack — just get the power in there and it’ll work. Now we’ve got different IT stacks, different cooling technologies — liquid, air, blended — depending on whether we’re inferencing or training. The fact that ST Telemedia Global Data Centres performed these tests shows how forward-thinking they have been in bringing Vertiv, the generator company, and a load simulator together in a controlled off-site environment.


#### Tam: What technologies are stabilizing AI data center power, and what did testing reveal about smoothing?


##### Peter:


One approach is putting energy storage in the racks themselves, like supercapacitors or ultracapacitors in the rack. That has some advantages, but it also creates challenges in terms of potential inrush load on the system. At the other end, we're looking at the UPS systems.


In the test we did with Vertiv, we had their Vertiv™ UPS Power Smoothing mode ready and used that to smooth out the load as it goes back onto the generator. We found that taking about 20% off the peak of each load step strikes a good balance between not impacting the battery and not putting too much impact on the generator. Taking just 20% off these load peaks stabilizes the generator enough that we're not seeing wild changes in frequency.


The smoothing didn't impact battery life. After endurance and repetition testing over 300 hours, we didn't notice any degradation in any of the systems. This 20% smoothing seemed to be the sweet spot for all of those.


#### Tam: AI workloads span the full stack—does this call for standardization, and what does collaboration look like in practice?


##### Tony:


We as an industry now have a duty to clarify, communicate, and understand these workloads. Standardization implies authority, regulation, document control, and time. Normalization — changing the way we collaborate and work — is a critical aspect we need to bring to bear now.


ST Telemedia Global Data Centres has really started it — bringing primary powertrain suppliers and equipment together in a real environment to test. Now the question is how we educate and influence designers, so everybody can learn. It's not just a design and install conversation — it's a full lifecycle conversation, with a ripple effect across the ecosystem.


#### Tam: AI workloads are reshaping the link between IT load and facility design. What assumptions no longer hold—and how should that relationship be reconsidered?


##### Tony:


You have to optimize what's in the IT stack and what else is connected. You're going to need your long-lead, primary equipment actually being tuned and collaboration taking place on site as well as at the design level. All teams across the power stack — from generators to switchgear to UPS to distribution — need to be on site, tuning their operating parameters to suit the actual application. We can't suddenly bolt all these pieces together like Lego and expect everything will be perfect.


We have to think of the data center as the unit of compute — not the IT load. It's not a building, it's not a collection of disparate critical infrastructure with IT hanging off the end. It is the unit of compute.


We're doing designs now for data centers that won't have IT stacks in them for another 18 months. There has to be flexibility in the data design and what used to be an integrated site acceptance test. There has to be another step where we can say: I've commissioned my equipment, now let's tune the site.


##### Peter:


Engineers are going to IT conferences. The sooner we get this information, the better. Some chip makers are releasing hints of what's coming ahead of time, but every month new information comes out and you have to stay on the ball. That's a real change in the way people think.


Our testing showed that having other loads mixed with the IT loads helps dilute these load steps. The HVAC loads aren't so bad anymore and maybe they can be more help than hindrance when dealing with GPU loads.


The stakes are higher than they've ever been before. Going and doing real-world testing on what you're planning to deploy in multiples of hundreds of megawatts is more important than there's ever been before.


### Watch the full conversation:[Adapting power infrastructure to AI workloads](https://www.vertiv.com/en-us/about/news-and-events/events/2026/vertiv-power-innovation-day-2026/)
