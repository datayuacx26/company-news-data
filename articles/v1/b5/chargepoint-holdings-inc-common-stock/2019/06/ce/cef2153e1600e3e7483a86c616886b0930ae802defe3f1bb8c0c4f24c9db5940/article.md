---
schema_version: "1.0.0"
document_id: "cef2153e1600e3e7483a86c616886b0930ae802defe3f1bb8c0c4f24c9db5940"
company_key: "chargepoint-holdings-inc-common-stock"
company: "ChargePoint Holdings Inc."
source_id: "chargepoint-holdings-inc-common-stock-news-import-2405547b27fc"
canonical_url: "https://www.chargepoint.com/engineering/how-we-toughened-touchscreens-machine-learning"
published_at: "2019-06-10T17:10:11+00:00"
first_seen_at: "2026-07-21T13:13:26.870476+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:776a814cbb51df19b11c8263ec71716e7dc061425aded4918782c68aa9a0f19f"
---

# How We Toughened Up Touchscreens with Machine Learning

When the team set out to design our latest fast charging platform, the CPE250, we knew that implementing a 10-inch touch display would be crucial to improving the overall user experience. Simple enough task, right? For a product that lives indoors or inside your pocket, maybe. For one that lives outdoors? Now that's more of a challenge.


### When Touchscreens Get Touchy


Touchscreens are typically bonded to glass or other low-durability materials. This works well for the majority of use cases, but certainly not for a charging station out in the wild. Consider the sheer number of elements in the natural environment that could crack a less-durable touchscreen display: hail, a rock launched by a car tire, wind-strewn sticks, or other materials, to name a few. Worse, the bonding used to attach touchscreens to glass isn't suited to handle large changes in temperature or direct UV exposure for extended periods of time. The traditional touchscreen simply cannot brave the weather.


To address these limitations, we implemented a new approach: an electric field (E-field)–based controller with four sensors positioned on the edges of the display.


Unlike the touchscreen, the E-field controller enables the usage of a highly durable polycarbonate shield, thereby ensuring that the display remains protected against extreme weather conditions. An E-field controller operating on a simple algorithm, however, poses a unique challenge: it sometimes results in a distorted reading of the user's XY touch location on the display.


### Confusion in the Field


Unlike a touchscreen which instantaneously detects contact, the E-field controller first emits an electric field and then detects the *impact of an object* on that field. This operational mechanism gives rise to a number of potential issues: detecting a user's entire finger or other fingers instead of just the fingertip/point of contact, reacting inconsistently to differing hand sizes and struggling with the variability associated with different interaction styles. Examples of this: some users hold their fingers entirely perpendicular to the display, some hold pinkies or other fingers out, some hold their hands nearly flat against the screen, etc.


To ameliorate these distortions, there were two possible pathways. Option one: create a basic algorithm for the controller and constantly tweak it to cover edge cases. This methodology felt misguided and inefficient—not a true fix for the underlying issue, and instead more of a band-aid in need of repeated reapplication.


Option two: let the E-field system *teach itself* how to accurately pinpoint the user's location.


### A TensorFlow-Based Machine Learning Model


Before any kind of machine learning model could take effect, we needed provide the model with enough data to "teach" it. This is called "training," and it requires both the raw data in question (the readings from the E-field controller) as well as the correct outputs (the correct XY positions associated with the user's touch). To train the model, we developed a simple tile matching game that covered the whole of the screen. A diverse group of users then played this game while we captured the E-field controller's readings along with data sourced from additional sensors that could provide the correct associated XY positions.


The training data was then fed into TensorFlow, an open-source software library developed by Google to design, build and train machine learning models. TensorFlow enabled the creation of a neural network model that mapped E-field data to correct XY touch locations.


From there, because the stations are embedded systems, the team used the TensorFlow C library to load the exported model. At this point, the model became "frozen" (no longer learning) and was able to take in new sensor values so it could "predict" entirely new XY touch locations.


The result? High-performance, real-time predictions of XY touch locations, and a display that is simultaneously robust and accurate—both guarded adequately from the natural elements, and intelligently able to read and respond to a user's touch and gestures.


So robust and accurate, in fact, that even winter, rain or driving gloves won't faze it: so the next time you find yourself out in the cold and standing in front of a CPE250, keep the gloves on, and charge on.


Want to tackle challenges like these and help build out the fueling network of the future? Come join our team!


[See Openings](https://www.chargepoint.com/about/opportunities/)


The information contained herein is considered proprietary and for informational purposes only and is not to be used or replicated in any way without the prior written authorization of ChargePoint, Inc.
