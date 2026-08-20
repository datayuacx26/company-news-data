---
schema_version: "1.0.0"
document_id: "919e2b605f304f27548bbb8774ff044fe34ac1287e15b28b7cc09f5639e0f788"
company_key: "mcdonald-s-corporation-common-stock"
company: "McDonald's Corporation"
source_id: "mcdonald-s-corporation-common-stock-rss-e3f7e88d5cc9"
canonical_url: "https://medium.com/mcdonalds-technical-blog/from-ideation-to-automation-the-scoop-on-outages-1ad0eab5cee1"
published_at: "2026-03-12T16:13:47+00:00"
first_seen_at: "2026-07-22T17:27:13.648832+00:00"
fetched_at: "2026-08-20T02:26:39.473331+00:00"
content_hash: "sha256:1766b06367f6eb910b43b13c06a62287c68c3fb5c53a02789d11275f9aa5a2c9"
---

# From Ideation to Automation: The Scoop on Outages

A process that once required a multi-click manual process is now fully automated in real time, with a Global Tech pilot restaurant testing the solution to boost efficiency and enhance the customer experience.


*by: Chloe Tominac, Manager, Engineering Tech Lead & Lauren Adamonis, Manager, Engineering Tech Lead*


**Quick Bytes:**


- Crew members had to manually mark ice cream items unavailable through a multi-click process, and later use the same process to restore items, which often led to missed updates and customer frustration
- A real-time automation pipeline was piloted to connect the ice cream machine to Sesame POS, instantly updating product availability
- The solution launched in McDonald’s Global Tech pilot restaurant, improving restaurant efficiency and ensuring customers see accurate menus across all ordering channels


In restaurant operations, every second counts — especially when equipment goes offline. That’s why McDonald’s tech teams set out to automate the ice cream product outage process, transforming a manual workflow into a seamless, real-time system as part of our ongoing[digital transformation.](https://corporate.mcdonalds.com/corpmcd/our-stories/article/digitizing-the-arches.html)


To explore how this could work in a live environment, we put the solution to the test in one of our Global Tech pilot restaurants. This pilot is helping us learn how real-time equipment data can improve restaurant efficiency and customer experience. While it’s not yet rolled out broadly, the insights from this test are shaping how we think about scaling automation across restaurant operations.


Previously, a multi-click process was required to take ice cream items off the menu during machine downtime. Without automatic recovery, items often remained unavailable even after the machine was back online until a crew member restored them, resulting in customers hearing that the ice cream machine was “broken.” Automation now ensures menu items are immediately re‑enabled as soon as the machine is operational.


Now, thanks to a collaboration between the Internet of Things (IoT) team **** and the Global Technology Service & Solutions (GTSS) Innovation team, outages are detected and managed automatically in real time at our Global Tech pilot restaurant. Customers at the test location see accurate product availability directly on kiosks, mobile apps, and POS systems.


**Is the ice cream machine really broken?**
McDonald’s ice cream machines aren’t usually “broken,” they’re often in the middle of a mandatory four-hour heat treat cycle (HTC), which ensures food safety. The reality is more technical than tragic.


If mix levels are too high or too low, the cycle fails, putting the machine into a standby or error state. Without clear alerts or automation, crew members may default to just saying, “The ice cream machine is broken.”


**From manual clicks to automated insights**
This journey began with a challenge: when the machine entered HTC or failed HTC, crew had to manually mark each ice cream product, by size and type, as unavailable. The process was time‑consuming and easy to miss items, and without automatic recovery, products often remained unavailable until a crew member manually restored them. With automation, menu items are automatically re‑enabled as soon as the machine is back online.


Product, Supply Chain, Operations, Innovation, and Technology teams came together to automate the product outage process for ice cream items when the ice cream machine enters standby or heat treat mode. From ideation to proof of concept (POC), the project took about three months, with the bulk of development completed in just one month.


Two engineering teams worked in parallel. The GTSS Innovation team enhanced Sesame POS to automatically react to machine status codes by updating menu item availability. Meanwhile, the IoT team developed a component within IoT Platform’s rule engine to interpret device codes from the ice cream machine and configure logic for communicating with Sesame POS. This included setting message frequency and defining rules like: “If the shake machine reports a problem, automatically trigger a product outage.”


**How the system works: From machine to menu**
The IoT engineers developed a robust communication pipeline that connects the ice cream machine → Edge server → Sesame POS, enabling real-time automation of product outages:


1. **The ice cream machine:** The machine sends JavaScript Object Notation (JSON) payloads that indicate its operational status — such as entering or exiting a lockout error state.
2. **IoT Edge stack:** These payloads are received by the Edge server, which includes a broker that manages message distribution.
3. **Message Queuing Telemetry Transport (MQTT) gateway:** The gateway contains both Publisher and Subscriber components that handle message flow between the Edge Stack and Sesame POS.
4. **Message logic:** The Edge stack component monitors device state. When the machine is operational, it takes no action and ensures products remain available; when non‑operational, it places the products into outage. When a code is received, it generates a JSON payload with:
- topic: e.g., AutoEquipmentOutage
- machineType: e.g., IceCreamMachine
- deviceCode: e.g., off
This payload is published to the MQTT Broker.
5. **Sesame POS integration:** Sesame subscribes to the topic via its Product Outage Plugin, which processes the message and updates the file across all relevant systems — POS, kiosk, GMA, etc.


This modular design ensures that future equipment types can be integrated with minimal changes, making the system scalable and adaptable.


**Scaling smart: Reusing and enhancing existing tech**
On the Sesame POS side, the GTSS Innovation engineers reused and enhanced code from a previous project, enabling faster development and delivery. Key enhancements included:


- **MQTT Gateway Plugin:** Upgraded with new security certificates to securely connect to the Edge server’s MQTT broker.
- **POS Extension:** New functions were added to bypass manual Product Outage workflows and automatically update menu item availability.
- **Topic Subscription:** Sesame POS is configured to subscribe to the topic. When a matching JSON payload is received, it flows through:
- POS Message Hub → Product Outage Plugin → productoutage.xml
This updates all relevant nodes: POS, Kiosk, GMA, etc.


**From lab to live simulation**
Testing was embedded throughout development to ensure the system worked as intended. Initial validation used MQTT Explorer to confirm Sesame POS could subscribe to and consume the JSON payload.


In McDonald’s test lab — connected to an ice cream machine in the mock restaurant — teams conducted end-to-end testing. Early on, Sesame POS was overwhelmed by the frequency of incoming messages, which impacted the user experience. To resolve this, the system was refined to respond only when a status change was detected, reducing unnecessary load.


With this adjustment, shakes, sundaes, cones, and McFlurries automatically entered outage state — no button clicks required when the machine entered standby or heat treat mode. This real-time automation proved the concept and showcased the power of cross-team collaboration.


**Putting it to the test**
After successful lab validation, the Auto Product Outage feature was deployed to a Global Tech pilot restaurant for real-world testing. This limited pilot helps us evaluate the solution’s impact on crew operations and customer experience before considering broader deployment.


**One unified solution, many happy customers**
The result? A unified solution that removes manual steps, improves crew efficiency, and keeps menu availability accurate in real time. Items are automatically taken offline during outages and restored as soon as the machine is back online, so customers always see the right availability whether ordering at a kiosk, on mobile, or in‑store.


This proof of concept shows how thoughtful engineering can drive innovation — one scoop at a time. While not yet deployed at scale, it lays the groundwork for future automation across restaurant operations.


---


[From Ideation to Automation: The Scoop on Outages](https://medium.com/mcdonalds-technical-blog/from-ideation-to-automation-the-scoop-on-outages-1ad0eab5cee1) was originally published in[McDonald’s Technical Blog](https://medium.com/mcdonalds-technical-blog) on Medium, where people are continuing the conversation by highlighting and responding to this story.
