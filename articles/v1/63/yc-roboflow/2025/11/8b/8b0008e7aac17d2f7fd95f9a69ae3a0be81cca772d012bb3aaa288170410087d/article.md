---
schema_version: "1.0.0"
document_id: "8b0008e7aac17d2f7fd95f9a69ae3a0be81cca772d012bb3aaa288170410087d"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/better-consumer-experiences-vision-ai/"
published_at: "2025-11-05T17:59:28+00:00"
first_seen_at: "2026-07-28T00:08:32.096830+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:729a0287d36eca65d8c790dd104a460b4ec3565d292a21e03331782644487bcd"
---

# How to Deliver Better Consumer Experiences with Vision AI

[Contributing Writer](https://blog.roboflow.com/author/contributing-writer/)


Published Nov 5, 2025 • 5 min read


SUMMARY


**Ogilvy Spain built Audi Reader on Roboflow to let drivers point their phone at a dashboard icon and instantly see its meaning, the relevant manual excerpt, and a path to book service or contact support. The system uses a two-stage computer vision pipeline trained on more than 10,000 photos spanning 27 Audi models, one model locating each symbol and a second classifying it. The same point-and-explain pattern extends to any product with a visual interface, from appliances to industrial equipment.**


You've seen that mysterious icon on a dashboard or a cryptic symbol on an appliance and thought, "I'll look it up later." Most people never do. That's the gap consumer experience AI can close: turning static products into interfaces that explain themselves the moment you point a camera.


One of our favorite examples:[Audi Reader](https://roboflow.com/case-studies/ogilvy-spain?ref=blog.roboflow.com) , built by Ogilvy Spain using[Roboflow](https://roboflow.com/?ref=blog.roboflow.com) . Today drivers can open the MyAudi app, aim their phone at a button or warning light, and instantly see what it means, with the right snippet from the owner's manual and a path to take action (message support, book service). As Ogilvy's Lorenzo Spadoni put it, "Innovation isn't just about the technology. It's about how we connect with our customers."


## The New Interface: Point, Recognize, Explain, Act


Products are getting more capable every year. But capability without comprehension becomes cost. Surveys around the Audi Reader project revealed a telling pattern: only about a quarter to a third of drivers open the manual, and a majority report being unaware of many features. That's not a reading problem; it's an interface problem.


Printed manuals live far from the moment of need. Consumers expect "show me, right now," not "search and decode later." Vision AI fixes this by bringing the manual to the object: no searching, no guessing, just point-and-know.


So Audi Reader reimagined product help as a camera-native flow:


1. **Detect** the right region (e.g., dashboard icon or button).
2. **Classify** the specific symbol across model years and trims.
3. **Explain** the meaning in natural language pulled from official documentation.
4. **Let me act** (book service, open chat, or learn more) in the same UI.


## Inside the Vision AI Behind Audi Reader


Audi Reader was trained on over 10,000 photos of 27 Audi models, covering both interiors and exteriors. The Ogilvy team created a model capable of identifying even the smallest dashboard icons in varying lighting conditions.


The system uses a two-stage vision pipeline: one model detects the location of each symbol, and another classifies what it is. "We created a detection model trained on every single symbol, button, and dashboard light," said Spadoni. "Dividing detection into stages worked a lot better performance-wise."


The result feels effortless. A driver points their phone at an unfamiliar icon, and within milliseconds, the app recognizes it and displays a short, clear description, sometimes even offering the option to schedule service or learn more.


As Spadoni put it, "Every driver has this experience. You see a warning light and think, 'I don't really know what that means.' Now you can point your smartphone and instantly get information."


0:00


/ 0:06


The result: an experience that meets consumers where they are through the camera, and closes the comprehension loop instantly.


## This Isn't Just for Cars


Everywhere a customer asks, "What does this mean?" you can let the product answer accurately, visually, and in the moment. For example, the same pattern applies across consumer products:


- **Kitchen appliances** : Identify cycle icons on dishwashers, ovens, microwaves. Explain what a setting does and recommend the right one for the task at hand.
- **Laundry** : Decode washer/dryer symbols; suggest a stain cycle based on a photo of the garment tag.
- **HVAC & thermostats** : Explain modes, energy-saving settings, and filter alerts by pointing the camera at the panel.
- **Consumer electronics** : Teach remotes, sound bars, routers, and printer error codes on sight.
- **Power tools & equipment** : Identify controls and consumables; surface maintenance steps in-line.


## A Practical Build Playbook (No Research Lab Required)


You don't need a research org to ship a polished vision experience. You need a tight loop:


### 1. Capture


Photograph your product's controls and indicators as customers will—on phones, not studio rigs. Vary lighting (kitchen shadows, garage fluorescents, bright sun), angles, and distances. Include multiple model years or product revisions.


### 2. Label


Use[Roboflow Annotate](https://roboflow.com/annotate?ref=blog.roboflow.com) to mark icons, buttons, and lights. Start with a few dozen images per class; let AI-assisted labeling propose boxes/masks to speed you up. Keep labels specific (e.g., "Washer: Eco 40–60" vs "Washer icon").


### 3. Train


Fine-tune a detector like[RF-DETR](https://roboflow.com/model/rf-detr?ref=blog.roboflow.com) on your labeled data. If classes are tiny and varied (icons), consider the same two-stage pattern Audi used: detect regions → classify crops.[Roboflow Train](https://roboflow.com/train?ref=blog.roboflow.com) handles experiments, metrics, and model versions for you.


### 4. Evaluate & Expand


Ship a small pilot model to an internal app. Watch failure cases: reflections, off-axis angles, obscure variants. Add 100–300 more examples specifically for those misses. You'll often see step-function gains from targeted data rather than sheer volume.


### 5. Deploy


Wrap the pipeline in[Roboflow Workflows](https://roboflow.com/workflows/build?ref=blog.roboflow.com) or call your model from your app via API/SDK. Attach actions: "Open Support," "Book Service," "Show Manual Section," "Order Replacement Part."


**Design Notes Learned from Audi:**


- **Bootstrap with public data** :[Roboflow Universe](https://roboflow.com/universe?ref=blog.roboflow.com) helped validate feasibility using community images of dashboards and interiors.
- **Collect the right images** : Early DSLR photos looked great, but models trained on them underperformed on phone captures. The fix? Train on smartphone photos across lighting conditions, device types, and angles.
- **Vision first, language second:** Detect precisely; then let an LLM tailor the explanation in the brand voice and the user's language.
- **Compose a simple pipeline** : In[Roboflow Workflows](https://roboflow.com/workflows/build?ref=blog.roboflow.com) , the team wired up detection → dynamic crop → classification, all running in milliseconds. When a serious warning is detected, the app lets drivers book service or chat without context-switching.
- **Tie to action:** Don't just explain the icon. Let the customer do the next thing (book, chat, reorder) without leaving the screen.


## Where Consumer Experience AI Is Heading


We're moving toward context-aware products: you point your camera, the product recognizes itself, understands its state, and offers the right next step. Multimodal models will continue to tighten the loop: vision grounds *what* is present; language models shape *how* to help; action modules make it *doable now* . AR overlays, voice prompts, and on-device inference will make the whole flow feel invisible.


The story Audi and Ogilvy started in the car easily extends to the dishwasher, thermostat, printer, or drill press. Furthermore, Ogilvy Spain's team didn't come in as career ML researchers, and that's the point. "Roboflow was instrumental in accelerating our computer vision development, empowering our team to quickly convert the user manuals into an intuitive, instantly accessible experience."said Lorenzo Spadoni, Ogilvy Spain.


Turn your product manual into a camera-native experience in weeks[with Roboflow](https://roboflow.com/sales?ref=blog.roboflow.com) , not quarters.


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Contributing Writer](https://blog.roboflow.com/author/contributing-writer/) . (Nov 5, 2025). How to Deliver Better Consumer Experiences with Vision AI. Roboflow Blog: https://blog.roboflow.com/better-consumer-experiences-vision-ai/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Contributing Writer


[View more posts](https://blog.roboflow.com/author/contributing-writer/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [Case Studies](https://blog.roboflow.com/tag/case-studies/)
