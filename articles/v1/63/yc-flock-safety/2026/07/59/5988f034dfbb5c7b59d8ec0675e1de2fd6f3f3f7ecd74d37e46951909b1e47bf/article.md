---
schema_version: "1.0.0"
document_id: "5988f034dfbb5c7b59d8ec0675e1de2fd6f3f3f7ecd74d37e46951909b1e47bf"
company_key: "yc-flock-safety"
company: "Flock Safety"
source_id: "yc-flock-safety-news-import-d217fbb8aa63"
canonical_url: "https://www.flocksafety.com/blog/how-flocks-audio-detection-works"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-21T20:29:56.673955+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:14c8105250212b28259ad503d9fdd5fb27e13ff1dd21ce2588bd423719c0767e"
---

# How Flock's Audio Detection Works, and the Concerns We Hear Most

- Audio Detection is designed to recognize specific public safety events, not conversations, voices, or speech, and records only short clips when a qualifying event is detected.
- Most audio is analyzed on the device and immediately discarded, with only encrypted event clips from qualifying incidents retained.
- The article addresses common privacy concerns directly, explaining what the system can do, what it cannot do, and the safeguards built into its design.
- It also explains why Flock removed its pilot human distress detection feature, reinforcing a commitment to transparency, privacy, and community trust.


‍


Before explaining how Flock’s Audio Detection works, it’s helpful to start with what it is designed to do. Audio detection recognizes a limited set of public safety events, such as gunshots, fireworks, car crashes, and sideshows, to provide earlier awareness of critical incidents.


Acoustic sensors mounted in public spaces invite hard questions, and they should. People want to know whether a device near their street is listening to them. We'd rather answer that plainly than wave it away.


Here is how Flock's Audio Detection (formerly known as Raven) actually works, including the concerns critics have raised and the changes we've made in response.


## **How do audio detectors work?**


Audio detectors continuously analyze sounds in short rolling windows of about five seconds. The audio is processed on the device itself by a dedicated low-power processor that asks a single question: does this sound match a qualifying event, such as a gunshot, vehicle crash, or tires screeching?


If the answer is no, that audio is never recorded, stored, or sent anywhere.


If the answer is yes, the system records, encrypts, and saves only that short audio clip, determines the location of the event, and sends an alert to the appropriate users.


,Aside from limited, pre-scheduled diagnostic samples used to improve system performance, the system retains only the short clips associated with qualifying events. All other audio is discarded almost immediately after being analyzed.


Some critics correctly point out that a system must process audio in order to recognize a sound. That's true. Processing, however, is different from recording. The system briefly analyzes audio on the device to determine whether a qualifying event has occurred.


## **The change we're making to human voice detection**


We previously offered a pilot feature that detected sounds of human distress, including screaming. This feature was designed to help identify potential violent incidents in areas where other public safety tools were less effective. The feature was only available to a small number of customers as part of a limited trial, and was never broadly released.


After careful consideration and community consultation, we decided to remove the feature.


It’s important to understand what the system does and what it does not do. Audio detection is designed to recognize specific sound events, not people. The system cannot understand speech, analyze language, or identify voices. It doesn’t now, and it never has.


## **The concerns we hear most, answered directly**


**"The microphones are always on, listening to everything."** The sensors are always analyzing, in the rolling-window way described above. They are not always recording. There is no way for a customer, or for Flock, to open a live audio feed of a location or listen to a place in real time. The system surfaces short event clips, and nothing else. We built audio detection that way on purpose.


**"It can record my conversations."** Flock’s Audio Detection is designed to capture qualifying sound events, not conversations. In the rare instance that a qualifying event, such as a gunshot or vehicle crash, occurs while someone is speaking nearby, the short clip may include a few incidental words spoken in a public space immediately before or after the triggering sound at such a volume that it is incidentally captured. The system does not listen for conversations, identify voices, understand speech, transcribe what people say, or allow anyone to search for spoken words.


**"It's inaccurate, and false alerts put people in danger."** No acoustic system is perfect, and we don't claim otherwise. The sensors are built to tell fireworks and similar sounds apart from gunfire to reduce false alerts. When the system detects a gunshot or firework, it estimates the location using detections from multiple sensors rather than relying on a single device. Alerts provide information for law enforcement or security professionals to review and determine the appropriate response. They support human decision-making rather than replace it. When the system gets something wrong, those event clips are used as feedback to correct and improve the model.


**"An audio alert triggers an automatic dragnet of every nearby car."** A confirmed event can surface the vehicles seen entering or leaving the area around the time of the sound, using nearby Flock license plate reader cameras. That gives investigators a starting point when investigating events like gunshots. It is not an automatic enforcement action, and it doesn't decide anything. A law enforcement officer or security professional reviews the alert and any associated information and decides what, if anything, to do.


**"Deleting clips later doesn't matter, because the harm is in capturing them."** While the sensor must briefly process audio to determine whether a qualifying event has occurred, the vast majority of that audio is never retained. Only short clips associated with qualifying events are encrypted, retained, and deleted after the applicable retention period.


**"This will expand to monitoring protests, or anything else they choose."** That expectation comes from real history with other technologies and the way they’ve been used, so we won't dismiss it. The most concrete answer we can give is what we just did: we removed a capability rather than only adding one. Today the system is built to detect specific public safety events, not speech, gatherings, or behavior. Future capabilities will be evaluated against the same standards of public safety value, privacy, and community trust.


## **Our commitment to transparency**


You shouldn’t have to take our word for it. We’re committed to being clear about how the system works, what it does, and just as importantly, what it doesn’t do.


If we’ve gotten something wrong, we’d rather hear it and fix it than avoid difficult questions. If there’s something we haven’t addressed here, let us know. We welcome thoughtful scrutiny and open discussion because building effective public safety technology requires transparency, accountability, and trust.


‍
