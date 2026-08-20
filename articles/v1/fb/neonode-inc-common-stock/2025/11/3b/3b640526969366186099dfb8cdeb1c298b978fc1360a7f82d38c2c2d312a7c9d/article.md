---
schema_version: "1.0.0"
document_id: "3b640526969366186099dfb8cdeb1c298b978fc1360a7f82d38c2c2d312a7c9d"
company_key: "neonode-inc-common-stock"
company: "Neonode Inc."
source_id: "neonode-inc-common-stock-news-import-8a3623f522b6"
canonical_url: "https://neonode.com/newsroom/news/how-the-software-defined-vehicle-affects-driver-monitoring"
published_at: "2025-11-27T14:39:23+00:00"
first_seen_at: "2026-07-23T22:09:27.196375+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:2f7a5f340bc52a7c34b3a3cd1035b3ad5b3065ad76aa32c32fbdb969be3e8755"
---

# How the Software Defined Vehicle Affects Driver Monitoring

# How the Software Defined Vehicle Affects Driver Monitoring


Published November 27, 2025 at 09:39 AM ET Driver Monitoring


Automotive


As vehicles shift to Software Defined Architectures, ADAS and DMS will share compute, creating both opportunity and risk. The driver monitoring warnings must stay reliable while competing with systems that control steering and braking. How can this balance be achieved?


As vehicle architectures move from decentralized control units (ECU) to the[centralized E/E architecture](https://neonode.com/newsroom/article/centralizing-ee-and-software-to-power-the-future-of-commercial-vehicles?hsLang=en) of Software Defined Vehicles (SDV), functions which previously resided on their own ECU’s now need to compete for the same computational resources.


Two functions which are likely to run on the same high performance computing unit (HPC) is advanced driver assistance (ADAS) and diver monitoring (DMS) systems. Running these systems on the same HPC opens up opportunities for combining the information gathered about the exterior of the vehicle with the state of the driver. This can, for example, be used to understand if the driver has seen an oncoming obstacle or not, and to optimize when warnings are shown to avoid additional stress in the driver.


### Same Unit. Different Ratings.


However, ADAS and DMS have different safety ratings and running them in parallel also introduces challenges. The ADAS system controls the steering and braking of the vehicle and a failure of the system may actively cause the vehicle to crash. The DMS on the other hand is (in most implementations) only a warning system and does not hold any control of the vehicle. As such, when the two functions share the same computational resources, it is not always possible to guarantee a fixed execution rate for the less critical driver monitoring software. Despite this, the DMS still needs to provide timely warnings in order to adhere to legislation.


How can these conflicting interests be handled?


### Dynamic Scheduling.


Neonode’s[MultiSensing®](https://neonode.com/multisensing?hsLang=en) software has been developed to handle such scheduling challenges. The software architecture has been designed for flexibility, allowing dynamic scheduling of the different features. Rather than one large AI model, the software is modularized with several small and efficient neural networks. That way, time critical subfunctions such as the eye analysis used for distraction warnings can be prioritized and handled at a higher frame rate than less time critical functions such as the seat belt routing detection.


This intelligent approach reduces the total execution time and resources needed for the driver monitoring system. MultiSensing is also built to handle fluctuating frame rates, even for time critical functions, while still maintaining accurately timed warnings.


### Over-the-air Updates.


Another aspect of MultiSensing is it allows for continuous software modifications through over-the-air (OTA) updates. In order to support this, Neonode’s rigorous test framework allows for manufacturers to directly and thoroughly test and verify new features and updates, which ensures software changes will not affect the previous performance.


### The Result.


The result is a driver monitoring solution that is both future-proof and SDV-ready: it delivers the required safety performance under real-world scheduling constraints, consumes significantly fewer compute resources than traditional monolithic DMS implementations, and enables richer data fusion with ADAS for smarter, less intrusive human-machine interaction.


In the Software Defined Vehicle, where flexibility and over-the-air updatability are paramount, architectures like MultiSensing demonstrate that robust, legislation-compliant driver monitoring can not only coexist with, but actively enhance—advanced driver assistance systems, paving the way for safer and more intuitive mobility.


[Learn About MultiSensing](https://neonode.com/multisensing?hsLang=en)
