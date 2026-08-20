---
schema_version: "1.0.0"
document_id: "7deb788931cd071bade0dc8c68fd0280b1b59c151549c0b7eeda59f361d873c5"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-rss-9175e36df81e"
canonical_url: "https://blog.roboflow.com/what-is-a-unified-namespace/"
published_at: "2026-07-27T19:31:00+00:00"
first_seen_at: "2026-08-10T21:40:53.899770+00:00"
fetched_at: "2026-08-10T21:40:55.760295+00:00"
content_hash: "sha256:d36a215eeac1fc967d7b088dacbbb4ba17bf81a3e4f2f211722168f13e25f5ea"
---

# What Is a Unified Namespace? Where Vision AI Fits in the UNS

[Erik Kokalj](https://blog.roboflow.com/author/erik/)


Published Jul 27, 2026 • 9 min read


SUMMARY


**A unified namespace (UNS) is a single hierarchical data model, usually carried over MQTT, where every system in a plant publishes its current state once and every consumer subscribes to the topics it needs, replacing brittle point-to-point integrations. Cameras become first-class citizens in it once vision AI reduces raw video to structured events.**


A unified namespace (UNS) is a single, report-by-exception data model that gives every system in a plant one place to read and write events, instead of forcing point-to-point integrations between every pair of systems. Most UNS diagrams stop at[PLCs](https://blog.roboflow.com/programmable-logic-controller/) , sensors, and[ERP systems](https://blog.roboflow.com/what-is-erp-in-manufacturing/) .


They leave out[cameras](https://ai1.roboflow.com/?ref=blog.roboflow.com) , even though vision is the highest-bandwidth sensor on the floor. Vision AI closes that gap by turning camera output into structured events (a defect found, a part counted, a PPE violation flagged) that publish straight into the UNS topic tree over[MQTT](https://blog.roboflow.com/mqtt-in-manufacturing/) .


This post covers what a UNS is, why cameras get left out, how to design a topic tree and payload schema for vision events, and how to wire[Roboflow Workflows](https://roboflow.com/workflows/build?ref=blog.roboflow.com) into an MQTT broker so any downstream system, from MES to a Grafana dashboard, can subscribe to what the camera saw.


## What Is a Unified Namespace?


Strip away the vendor slide decks and a unified namespace is a simple idea: every piece of real-time state in your plant lives in one hierarchical, always-current data model, and every system reads from or writes to that one model instead of talking directly to each other.


The term was popularized by Walker Reynolds, a systems integrator who spent years arguing that plant data belongs in one hub instead of a hand-built web of integrations, and most of the industry has since come around.


Compare that to how most plants are wired today, though. The[SCADA system](https://blog.roboflow.com/what-is-scada/) polls the PLC. A separate integration polls the SCADA system for the historian. Another custom script pulls from the[MES](https://blog.roboflow.com/manufacturing-execution-system/) to update a dashboard. Each of those is a point-to-point connection, built and maintained by hand, and each one breaks a little differently when a tag changes or a system gets upgraded. Add ten data sources and twenty consumers, and you're maintaining a web of brittle, one-off integrations that nobody fully understands anymore.


A UNS replaces this web with a hub and spoke. Every source publishes its current state to a topic in the namespace. Every consumer subscribes to the topics it cares about. Nobody polls anyone. This is the report-by-exception model because a device doesn't get asked, "What's your status?" every second. Instead, it announces its status the moment that status changes, and the broker holds the current value so anything subscribing later gets the latest state immediately.


The result is a single source of truth. For example, if a line's changed to "running," every subscriber, whether it's a dashboard, a historian, or an andon light, sees "running" from the same origin at the same time. No reports drift out of sync with any other reports because there's only one place the fact lives.


MQTT is usually the transport layer under a UNS, largely because of that report-by-exception design and its lightweight publish-subscribe model. If you want the deeper mechanics of why MQTT fits manufacturing so well,[MQTT in Manufacturing](https://blog.roboflow.com/mqtt-in-manufacturing/) covers the protocol itself in more depth. And a UNS isn't a replacement for OPC UA; it often sits alongside it, with OPC UA handling the PLC-facing side of the equation. See[What Is OPC UA](https://blog.roboflow.com/what-is-opc-ua-how-factories-connect-vision-ai-to-plcs/) for how that piece fits together.


## Why Cameras Are Missing from Most UNS Diagrams


Examine almost any UNS architecture diagram (the kind you find in whitepapers and vendor pitch decks) and you'll see PLCs, sensors, ERP, MES, historians, maybe even a robot cell. But you'll rarely see a camera. Which is strange, because a camera is arguably the richest sensor on the floor.


A single[vision system](https://blog.roboflow.com/vision-inspection-systems/) watching a line generates orders of magnitude more information per second than a temperature probe or a proximity switch. It tells you not just that something happened, but what happened, where on the part it happened, and what it looked like when it did.


The reason that cameras get left out of UNS architecture diagrams isn't that the data isn't valuable, of course. It's that raw camera output isn't the kind of thing that a UNS topic tree was built to carry. A UNS moves small, structured, event-shaped messages. A video stream is the opposite of that: continuous, unstructured, and enormous. Nobody wants to publish raw RTSP frames into an MQTT broker, and nobody's dashboard wants to subscribe to one.


This gap is exactly what vision AI closes. A model doesn't publish frames, it publishes findings. "Defect detected on part 44821, station 3, confidence 0.94" is a message that a UNS was built to carry. The camera becomes a sensor like any other once its output has been reduced to structured events, and that reduction is the job of the inference layer sitting between the lens and the broker.


## Adding Vision Events to a Unified Namespace


To make a detection a proper UNS citizen, you need two things: a payload schema and a topic tree that other engineers can guess without reading documentation.


A reasonable detection payload looks something like this:


```text
{
"timestamp": "2026-08-07T14:32:11Z",
"site": "elkhart",
"area": "paint",
"line": "line3",
"cell": "inspection-station-2",
"camera_id": "cam-07",
"class": "surface_defect",
"confidence": 0.94,
"part_id": "44821",
"bounding_box": [212, 88, 340, 210],
"image_ref": "s3://plant-vision/elkhart/line3/44821.jpg"
}


```


Keep the payload flat and predictable. Every consumer, whether it's a human reading logs or an MES parsing JSON, should be able to guess the field names before opening a spec sheet.


The topic tree matters just as much as the payload, because the topic is how consumers filter without ever touching the message body. A workable hierarchy for vision events follows the same site-down-to-equipment logic every other UNS topic uses:


```text
site/area/line/cell/quality/defect
```


This is the ISA-95 equipment hierarchy (enterprise, site, area, line, cell) that the rest of your namespace already follows, and that's the point. Vision events slot into the model your plant already uses instead of inventing a parallel one.


Worked out for a real plant, that becomes something like:


```text
elkhart/paint/line3/inspection-station-2/quality/defect
elkhart/paint/line3/inspection-station-2/quality/pass
elkhart/assembly/line1/cell-04/quality/missing-component
elkhart/assembly/line1/cell-04/count/units-produced


```


Notice that the pattern holds whether the event is a[defect](https://roboflow.com/solutions/defect-detection?ref=blog.roboflow.com) , a pass, a[count](https://blog.roboflow.com/object-counting/) , or a safety flag. quality/defect and count/units-produced are just different leaves under the same cell. A dashboard team can subscribe to elkhart/+/+/+/quality/defect and catch every defect across every line in the plant without knowing in advance how many lines exist. That wildcard subscription is the entire point of building the tree this way: consumers can be broader or narrower than the publishers, and nobody has to renegotiate an integration when a new line gets added.


## Walkthrough: Roboflow Workflows to MQTT to the Unified Namespace


Getting detections into the namespace doesn't require a custom integration project. Roboflow Workflows already has an MQTT publish step, which means the path from "camera sees something" to "event lives in the UNS" is short.


1. **Build the inference workflow.** Point a Workflow at your camera stream and run whatever model handles the task, defect classification, part counting,[PPE detection](https://blog.roboflow.com/ppe-detection/) , whatever the cell needs.
2. **Add a filter or logic block** if you only want to publish on specific outcomes (a defect found, a count threshold crossed) rather than every frame.
3. **Add the MQTT publish block.** Point it at your broker's address, set the topic using your site/area/line/cell convention, and map the detection output into the JSON payload shape you've standardized on.
4. **Publish to your broker.** Whether you're running a broker like EMQX, HiveMQ, or Mosquitto as the hub of your UNS, the Workflow publishes there like any other source in the namespace.
5. **Let consumers subscribe.** Nothing on the consuming side needs to know a camera was involved. MES sees a quality event on a familiar topic. The historian logs it. A dashboard renders it.


That last step is the whole payoff. For more detail on routing vision detections to specific downstream systems (PLCs, MES, and Slack), see[Detections Into Alerts: PLC, MES, Slack](https://blog.roboflow.com/vision-detections-into-plc-mes-slack-alerts/) , and for how MQTT fits into the broader integration picture,[The CV Integration Stack](https://blog.roboflow.com/the-cv-integration-stack/) walks through the layers above and below it.


## Benefits of a Unified Namespace with Vision


Once vision events live in the UNS, the camera stops being a single-purpose tool bolted to one application and becomes a shared plant resource. For example, the same defect event that trips an andon light also updates a quality dashboard, feeds a historian for trend analysis, and triggers a hold in the MES, all without a single new point-to-point connection. Add a new consumer next quarter and it just subscribes. Nobody touches the camera's integration again.


This is the same principle behind the broader industrial IoT push toward connected, addressable plant data. See[Industrial IoT](https://blog.roboflow.com/industrial-internet-of-things/) for the wider context on why that connectivity matters beyond vision specifically.


## Why UNS-Readiness Belongs on Your Vendor Checklist


When you're evaluating a[machine vision vendor](https://roboflow.com/industry/machine-vision?ref=blog.roboflow.com) , ask directly whether their system publishes structured events to MQTT using a topic convention you control, or whether you're locked into their own dashboard and their own database as the only place your detection data lives.


A vendor that only offers a proprietary UI and a closed API is asking you to build a point-to-point integration for every downstream system you'll ever want, and to rebuild it every time you switch systems. A vendor that publishes into your namespace lets your MES, your historian, and your next dashboard vendor all read the same events without asking the camera vendor's permission first. That's the difference between a camera as an isolated tool and a camera as a first-class sensor in your plant's data architecture. Learn more about how Roboflow improves[manufacturing environments](https://roboflow.com/industries/manufacturing?ref=blog.roboflow.com) .


### Do I need a full UNS platform before I can publish vision events to MQTT?


No. You can publish structured detection events to any MQTT broker today and adopt a formal UNS topic convention later. Starting with a clean topic tree and payload schema now makes that migration painless.


### What's the difference between a UNS and just using MQTT?


MQTT is the transport. A UNS is the data model and topic convention layered on top of it, the organizing logic that makes the messages meaningful and consistent across every publisher and subscriber in the plant.


### Do I need Sparkplug B to publish vision events?


No. Sparkplug B adds typed metrics, birth and death certificates, and state management on top of MQTT. If your broker stack already speaks it, publish vision events as Sparkplug metrics like any other device. If it doesn't, plain JSON on a well-named topic tree works and is easier to debug. Start with JSON and adopt Sparkplug when the rest of your namespace does.


**How is a UNS different from a historian or a data lake?**


A UNS holds current state: it answers: "What is true on the floor right now?" A historian or data lake subscribes to the namespace and keeps the record: it answers "What was true over time."


### Does every camera need to publish raw video into the UNS?


No, and it shouldn't. The UNS carries structured events (detections, counts, classifications), not video streams. Store raw footage separately and reference it by URL in the event payload if you need traceability back to the original image.


### Can vision events trigger actions in a PLC through the UNS?


Yes, indirectly. A PLC-facing integration layer, often via OPC UA, can subscribe to the relevant UNS topic and translate a detection event into a PLC-readable action.


### What happens if my topic naming convention doesn't match my UNS provider's default?


Nothing breaks. MQTT topics are just strings; you control the hierarchy. The key is consistency across your own plant so wildcard subscriptions work predictably.


## Your Plant Floor Already Generates the Richest Data You Have


The camera on your line already sees every defect, every count, every near miss. The only question is whether that data stays trapped in a single-purpose dashboard or joins the rest of your plant's real-time state where MES, historians, and every future system can read it. Once a detection is a message on a topic, it behaves like every other fact in your namespace: current, addressable, and available to whoever needs it next.


You don't need to rebuild your architecture to get there, either. You simply need a Workflow that publishes to the broker you already run.


Want to see this on your own line?[Book a demo](https://roboflow.com/industries/manufacturing?ref=blog.roboflow.com) and we'll walk through connecting a Roboflow Workflow to your MQTT broker, using your own topic convention, on a use case pulled from your floor.


**Further reading:**


- [How to Use AI With Your Existing Cameras](https://blog.roboflow.com/how-to-use-existing-cameras/)
- [Process RTSP Streams for Real-Time Video Analytics](https://blog.roboflow.com/process-rtsp-streams/)
- [Shop Floor Data Collection with Computer Vision](https://blog.roboflow.com/shop-floor-data-collection/)


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Erik Kokalj](https://blog.roboflow.com/author/erik/) . (Jul 27, 2026). What Is a Unified Namespace? Where Vision AI Fits in the UNS. Roboflow Blog: https://blog.roboflow.com/what-is-a-unified-namespace/*


### Written by


Erik Kokalj


Developer Experience @ Roboflow


[View more posts](https://blog.roboflow.com/author/erik/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [Manufacturing](https://blog.roboflow.com/tag/manufacturing/)
