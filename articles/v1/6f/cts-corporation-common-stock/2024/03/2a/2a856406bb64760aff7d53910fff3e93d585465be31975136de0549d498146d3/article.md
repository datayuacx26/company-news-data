---
schema_version: "1.0.0"
document_id: "2a856406bb64760aff7d53910fff3e93d585465be31975136de0549d498146d3"
company_key: "cts-corporation-common-stock"
company: "CTS Corporation"
source_id: "cts-corporation-common-stock-news-import-47f21efce330"
canonical_url: "https://www.ctscorp.com/Resources/Blog/Frequently-Asked-Piezo-Questions"
published_at: "2024-03-26T15:54:37.750+00:00"
first_seen_at: "2026-07-21T15:29:46.953658+00:00"
fetched_at: "2026-07-28T22:26:05.625622+00:00"
content_hash: "sha256:f12647d9a028f712b58c4e19a2a30a348d9d0fa1e0fb284f08c75ab17cd29f46"
---

# Frequently Asked Piezo Questions

**On this page you can find a list of frequently asked questions about our products. The list is divided into the different product categories.**


## Linear Actuators


### I want to apply coating on my actuator; is that going to reduce motion?


Any additional material in parallel with the piezo element will result in increased load on the piezo element, thereby reducing performance. However in most cases the coating has a low stiffness compared to the piezo element so the impact will be minor (<5%).


### Can I use oil for cooling a piezo element?


Yes. For applications where self-heating is significant, it is possible to use air flow or oil flow for cooling purposes. The oil needs to be clean dielectric oil (transformer oil) as any impurities can provoke electro-chemical reactions ultimately leading to a failure of the actuator. Be aware also that for large cross-sections, the thermal gradient in the actuator can be significant so maintaining an acceptable surface temperature might not be sufficient.


### Can I apply a torque on my piezo stack?


It is not recommended to apply torque on a piezo stack. In the case where a stack is preloaded using a nut, it is important to make sure that the tightening torque is not transmitted to the stack. Shear stress above 1MPa can lead to failure of the adhesive layer between the ceramic elements.


### How much preload can I apply on my piezo stack?


We recommend that piezo stacks operate in the range 0 to 80 MPa. Preload must be designed to ensure that stress remains in this range in all conditions. In addition, preload ensures that all parts are pressed firmly in contact, thereby allowing the system to reach its nominal stiffness. Although stacks can safely be operated in static conditions without preload, usual values for dynamic applications are 10 to 20 MPa and some specific applications require preloads as high as 40MPa.


### Can I apply a negative voltage to a multilayer piezo element?


Yes, it is possible to increase the quasi-static displacement of a piezo element by applying a negative voltage. However in order to avoid the risk of de-poling, the electrical field should be limited: at room temperature and without load, -0,7kV/mm for NCE51 and NCE57, -0,25kV/mm for NCE59, -1,5kV/mm for NCE46. These recommendations have to be decreased at high temperature and high load.


## Bending Actuators


### I cannot measure the resonance frequency stated on the datasheet of my bending actuator. What could be the reason?


The specified resonance frequency is defined in clamped-free (cantilever) configuration, without added mass. First resonance will be higher in free-free configuration, so clamping has an impact. Similarly, adding a mass (mirror…) at the tip of the actuator will lower the first resonance. Please contact us so we can evaluate your requirements.


### Is it possible to customize an actuator through machining of the sintered ceramic?


It is possible to machine sintered ceramic using diamond tools. However it is easier to machine ceramic in green state (before sintering), so please contact us so we can do the machining within our process. In the case of multilayer products, if the internal electrodes are exposed, the maximum applied voltage needs to be decreased to avoid arcing. Alternatively, the exposed area will have to be coated, however coatings are not as reliable and tough as the original ceramic insulation.


## Shear Actuators


### The specification for shear plates does not mention blocking force. What is the value?


Blocking force is very difficult to measure for shear plates due to their very high stiffness. This is why we don’t mention it on the specifications. It is possible to estimate blocking force by multiplying free displacement with stiffness, obtained using a catalog value for s55E of 4,3E-11 m^2/N.


### Can shear elements be used at high frequency?


Thanks to their high stiffness and high resonance, shear elements can be used for rapid operation (fast response). However continuous operation at high frequency can be an issue. This is because the non-linearity implies high losses (higher than for d33 actuator), so the element will heat-up. Typically, a few minutes at a few kHz is not a problem. If higher frequency or longer duration is required, the operating voltage has to be reduced, otherwise the actuator will slowly and irreversibly degrade (loss of poling).


### Is there a recommended preload for shear actuators?


Usual d33 stack actuators require a preload for high frequency operation because the inertial force can become substantial, creating tensile stresses in the ceramic. For shear actuators, the inertial force is a shear force, so an axial preload will have limited effect. It can however be beneficial in some cases in order to improve stiffness (by compressing the bond lines) or in order to avoid bending (if center of gravity is far from the fixed end of the stack).


### What is the maximum axial load that can be applied on shear elements?


Our shear plates are usually tested under 3,5 MPa. Piezo elements can withstand high compressive pressures (>50 MPa) however in practice, we recommend not to apply more than 5 MPa axial load on shear elements. This is because small defects in the contact surfaces can lead to stress concentrations and break the element. If high pressure is required, we recommend to use contact surfaces (both on the shear plate and on the structure) with superior flatness.


### What is the maximum shear load that can be applied on shear elements?


The ceramic can withstand very high loads. In practice it is the interface between the ceramic and the substrate that defines the maximum shear load.


If the movement is transmitted by friction, the maximum load will depend on the pressing force and the coefficient of static friction at the interface. For example 2 MPa pressing force * 0,2 friction coefficient = 0,4 MPa, i.e. about 10 N for the 5×5 mm shear plate.


If the element is bonded using epoxy, the maximum load will depend on the strength of the epoxy that is used. Typically epoxies can be used safely at 5 MPa in shear stress, i.e. about 125 N on a 5×5 mm shear plate.


## Amplified Actuators


### What materials are used for Amplified Actuators?


The piezoelectric material is NCE51F and mechanical parts are stainless steel (equivalent AISI303) and titanium alloy. Standard components (preload screw…) are steel but can be changed for low-magnetic applications.


### Why is there a force limit on Amplified Actuators?


A high preload on the piezoelectric stacks keeps the components in place through friction. If the external load reaches the point where it overcomes the preload, the stacks may leave their positions, which will affect operation. Note that maximum recommended load is specified in “active” conditions (bias voltage applied). It is possible to increase the preload or use positive locking of the stacks for specific applications; please contact us.


## Partner with a Piezoelectric Specialist


CTS can help decide on material properties to deliver products that meet the unique requirements of your project. Work with a technical expert to begin your design today.


[Let's Talk](https://www.ctscorp.com/Contact-Us)


## Check Out Our Latest Blogs


Please wait while we gather your results.


#### [Sensing Module Designed for High-Power Battery Systems](https://www.ctscorp.com/Resources/Blog/Optimizing-EV-Battery-Safety-and-Life-with-Precise-Sensing-Technology)


Discover how CTS's advanced Battery Monitoring System (BMS) utilizes a redundant shunt-hall sensor architecture to help optimize EV battery safety, capacity, and lifetime.


#### [5 Features of the New CTS Website](https://www.ctscorp.com/Resources/Blog/5-Features-of-the-New-CTS-Website0Copy1)


The revamped website showcases an entirely new navigation system, intuitive product selection tables, extensive technical resource downloads, and a dynamic blog section.


#### [Solutions for Forklifts and Material Handling Applications](https://www.ctscorp.com/Resources/Blog/Solution-for-Forklifts-and-Material-Handling-Applications)


CTS offers rugged Hall-effect sensors, optical encoders, and current sensors for modern forklifts. Learn how CTS Series 285, 286, and 003 optimize steering, safety, and battery systems.


[View All Blogs](https://www.ctscorp.com/Resources/Blog)
