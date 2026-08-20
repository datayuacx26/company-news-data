---
schema_version: "1.0.0"
document_id: "61cccbb2cc6974d1a6fbf698956b0bffa8c6622ae92a4d4c916f4ac4b845b802"
company_key: "synaptics-incorporated-common-stock-0-001-par-value"
company: "Synaptics Incorporated Common Stock $0.001 Par Value"
source_id: "synaptics-incorporated-common-stock-0-001-par-value-rss-c69120407f43"
canonical_url: "https://www.synaptics.com/company/blog/two-billion-fingertips-what-a-trillion-touch-events-taught-us-about-signal-and-noise"
published_at: "2026-08-06T21:32:44+00:00"
first_seen_at: "2026-08-06T22:05:28.097526+00:00"
fetched_at: "2026-08-06T22:05:28.929362+00:00"
content_hash: "sha256:be11064151a761491869fc64a8bd88ca12410993902b8e26e022389b83c48227"
---

# Two Billion Fingertips: What a Trillion Touch Events Taught Us About Signal and Noise

[Home](https://www.synaptics.com/) /[Company](https://www.synaptics.com/company/overview) /[Blog](https://www.synaptics.com/company/blog) / Two Billion Fingertips: What a Trillion Touch Events Taught Us About Signal and Noise


# Two Billion Fingertips: What a Trillion Touch Events Taught Us About Signal and Noise


August 06, 2026[Derek Solven Senior Director of Touch Systems](https://www.synaptics.com/company/blog?author=Derek%20Solven) Share this article


*Part 4 of a series on Physical AI, tactile sensing, and the road to dexterous machines.*


Press a finger to a touchscreen and, electrically, you've done something almost absurdly subtle. The change in capacitance your fingertip produces is measured in femtofarads — quadrillionths of a farad. It's a whisper. And it arrives against a background that is anything but quiet: the display stack humming beneath it, a charger pumping noise through the system, the temperature climbing in a sunlit car, a film of sweat or rain smeared across the glass.


The entire art of touch sensing lives in that gap — between a vanishingly small true signal and a loud, shifting, hostile world. Detecting a clean press on a lab bench is trivial. Detecting the right press, every time, on the billionth device and under conditions nobody designed for, is where the real engineering begins.


Synaptics has shipped more than two billion touch controllers. Over their working lives, that adds up to well past a trillion individual touch events — a number that sounds like a vanity metric but is really something else. It's a curriculum.


### The enemy was never the weak signal. It was noise.


There's a natural assumption that the goal of a sensor is sensitivity — to detect ever fainter signals. In touch, that's a trap. Crank sensitivity high enough and you'll happily detect a hovering hand, a smear of moisture, the electrical ghost of a nearby motor. Sensitivity without discrimination just means more false alarms.


A more useful measure is signal-to-noise ratio: not simply how faint a touch you can detect, but how confidently you can separate a true touch from everything pretending to be one. That distinction sounds academic until you've watched a screen misfire in the rain or register phantom presses while connected to a noisy charger.


A femtofarad signal is easily overwhelmed, and the same physics applies to a robot hand\[MT1.1\]\[DS1.2\]. A sensor that sensitive behaves like an antenna for every stray field in the room. The wiring to the controller carries its own parasitic capacitances; nearby metal becomes a parasitic ground plane; switching electronics inject interference. One countermeasure is active guarding: driving a conductive shield around the sensitive electrode at exactly the electrode's own voltage, so no charge can leak across the gap, and the sensor goes electrically blind to everything except the contact you care about.


### The hardest part: zero won't hold still


The truly difficult problem in touch isn't detecting the touch. It's knowing what no touch looks like — right now, this second, on this specific device.


Because "zero" is never fixed. The baseline moves with temperature. Humidity creeps in. The panel ages. A user sets the device down and the entire electrical environment changes around it. Pick a fixed threshold and it can quickly become wrong. So the system has to keep asking one question: What does untouched feel like under these exact conditions? It recalibrates itself as the answer changes. In capacitive sensing this is often done with a whole toolbox of orthogonal approaches and algorithms, with their own individual strengths: differential measurements, continuous baseline tracking, object estimations, and classification can all contribute to the whole, so that a slow environmental shift cancels out while a real touch still rings through.


A slow, deliberate press and a slow environmental drift can look almost identical to the sensor. Adapt too aggressively and you'll erase a real, slowly applied touch as though it were drift. Adapt too cautiously and you'll chase the temperature all afternoon. Engineers learn to thread that needle by watching systems fail across millions of units in thousands of real environments, not from first principles on a whiteboard.


### Not every contact is a command


A sensor reports contact; a *product* has to report meaning.


Your palm rests on the touchpad while you type, but you don't want the cursor leaping around the screen. Rain lands on an automotive display, but the navigation shouldn't reroute itself. A thumb wraps the bezel of a phone simply to hold it. Each of these is a real, physical contact the system must recognize and deliberately ignore — while never missing the intentional touch that comes a fraction of a second later.


Turning a raw stream of contacts into intended events requires filtering out the accidental and inferring what the user actually meant. That is the difference between a sensor and an interface.


### Why touchscreen experience matters to robotic touch


None of this was developed for robots, yet robotic touch now faces the same problems.


A robotic fingertip lives in a more hostile world than a phone ever did. The electric motors driving the fingers sit millimeters from the sensor, spraying electromagnetic interference straight into a femtofarad-scale measurement. Actuators heat the whole assembly, dragging the baseline around. The elastomer skin wears and changes with every grasp. The job is also broader: not just detecting whether something is there, but measuring normal force and shear and, as Part 2 explored, determining whether the grip is beginning to slip.


Strip it down, and high-fidelity force and tactile sensing for robots belongs to the same family of problems we've been solving for four decades: capture a tiny, true physical signal; reject the parasitic capacitance and motor noise trying to swamp it; hold a stable reference while the world shifts underneath it; and turn a raw measurement into a trustworthy event the rest of the system can act on immediately. The transducer is new. The discipline is not.


### You can't shortcut a trillion events


Anyone can buy a transducer. What no spec sheet can capture is the accumulated knowledge of every way real-world sensing goes wrong — the failure modes that only reveal themselves at the scale of billions of units and hundreds of distinct products, across heat and cold, moisture and noise, fatigue and age. That knowledge lives in firmware, in algorithms, in hundreds of patents, and in the hard-won instinct of teams who have watched the long tail of edge cases play out for years.


It also explains why owning the whole stack — silicon, firmware, sensor, and algorithm together — matters so much here. Noise rejection isn't a feature you can drop into a single layer. The analog front end, sensing architecture, sensor geometry, guarding, noise avoidance state machine, and classifier all contribute to noise rejection and have to be tuned together.


Those trillion touch events did more than build a product line. They taught us how to find signal in noise, and dexterous machines will demand the same skill.


*Next in the series: From Glass to Skin — the hidden craft of building a sensor that survives the real world.*


#### Derek Solven


Derek Solven leads the Systems Architecture and Systems Engineering teams responsible for the end-to-end specification, development, and roadmap execution of industry-leading touch technologies. With more than 15 years of experience and over 27 patents, Derek specializes in low-power, high-volume embedded sensing systems, bringing innovations from first-principles physics modeling and research through ASIC development and mass production. Throughout his career at Synaptics and BlackBerry, Derek has led cross-functional engineering teams that delivered advances in sensing architectures, touch/display coexistence, and tactile sensing technologies for robotics. His work has contributed to products shipping in excess of 100 million units annually. Derek holds a BASc in Systems Engineering from Simon Fraser University.


[Read more by Derek Solven](https://www.synaptics.com/company/blog?author=Derek%20Solven)


[AI](https://www.synaptics.com/company/blog?category=AI)[Edge Computing](https://www.synaptics.com/company/blog?category=Edge%20Computing)
