---
schema_version: "1.0.0"
document_id: "82a09dc64d90ac0ed41cd01c191c2d93e0c2fbf354c77e0ee3ae4179fbf6b237"
company_key: "aeye-inc-class-a-common-stock"
company: "AEye Inc."
source_id: "aeye-inc-class-a-common-stock-rss-1927df7d28ee"
canonical_url: "https://www.aeye.ai/blog/coffee-talk-dr-allan-steinhardt/"
published_at: "2026-05-01T21:59:29+00:00"
first_seen_at: "2026-07-20T23:23:00.433930+00:00"
fetched_at: "2026-07-28T22:15:27.754583+00:00"
content_hash: "sha256:6fa27bc889138db0b72463907d294c2ad6ac344b2b636e012f4fa6e166b39dc7"
---

# Coffee Talk: Dr. Allan Steinhardt

-
-
-
-
-


This week we sat down with AEye Chief Scientist Dr. Allan Steinhardt to talk about how AEye’s system reconfigurability enables customized lidar performance, and why some of Dr. Steinhardt’s favorite physics lessons can be found at sea.


> “…we can define multiple zones across the field-of-view and configure different ranges and resolutions for each zone. This ability to customize the scan for each customer using just software, without any hardware changes, is very powerful – and gives customers a valuable tool to make sure that the lidar is optimized well for their application.”


### Tell me about your role


My role as Chief Scientist at AEye is twofold. Firstly, I oversee the patent portfolio and intellectual property. Secondly, I develop new innovations with the AEye team to enhance product capabilities and features, using advanced algorithms and technology.


### We’ve heard about AEye’s unique software-defined technology. Can you shed some light on this and comment on how customers can leverage this?


Let me first walk through the things that we have control over in our AEye product. Let’s start with what we call a “shot” – which is basically the unit of information we obtain from the lidar when a lidar pulse comes out and illuminates an object and then returns to the lidar. In our product, we can control a few key items on a per shot basis. We can control the timing and spatial location of the shot, and the energy of the laser pulse that is emitted (which influences how far the light travels). We can also control how long we listen for a return from that pulse, and additionally, we can control the angular resolution by, for example, varying the separation between shots and the effective ‘scan speed’. All of these can be varied in software in microsecond to millisecond timeframes – and AEye has built a sophisticated and innovative software tool in-house that enables us to quickly create and analyze different variations of these scan patterns for customers.


In a typical automotive Advanced Driver Assist System (ADAS) or Autonomous Vehicle (AV) application, there are all kinds of objects and markings on the road and in the surroundings which the vehicle needs to be aware of. Typically, objects that are closer don’t need as much angular resolution and range for accurate detection as objects farther away. One key benefit for customers using AEye’s lidar is that we can simply configure, via software, how exactly the sensor samples the scene in a way that is fine-tuned for the customer’s application and key use cases. For instance, we can define multiple zones across the field-of-view and configure different ranges and resolutions for each zone. This ability to customize the scan for each customer using just software, without any hardware changes, is very powerful – and gives customers a valuable tool to make sure that the lidar is optimized well for their application.


### Why is that important?


It’s important for a few reasons. First, customers incur cost with adding a new sensor to the vehicle – and it is therefore important that the sensor is as optimized as possible for the customer’s critical use cases. Second, customers may mount the sensor(s) at different locations depending on the vehicle model. Different sensor locations have different views of the road and the surroundings – so the ability to adjust the scan pattern configuration based on location, purely using software, makes it much more likely that a customer gets optimal results, regardless of mounting location, compared to other sensors where such customization is not possible. Third, customers may want to cover different operational design domains (ODDs) with the same vehicle – for example, high speed driving on the highway as well as lower speed driving in urban and suburban areas. Software configurability of the scan pattern enables a customer to switch from a scan pattern optimized for one ODD, to a different scan pattern optimized for a different ODD, without changing hardware. Finally, customers may not always know the most optimal scan pattern for their ODD when they first start working with the lidar – so giving them an opportunity to experiment and try different configurations early on makes it more likely that they end up with an optimal configuration at the end.


### What’s your favorite mode of transportation?


I love sailing. It’s enjoyable and offers a mental challenge. Sailing involves understanding physics in action. I used to participate in sailboat racing. While I’m not as active in racing nowadays, sailing remains my preferred mode of transportation. The past saw the glory days of sailing, and perhaps, with the emphasis on green energy, we’ll witness a global resurgence in sail power for global commerce.
