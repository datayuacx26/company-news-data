---
schema_version: "1.0.0"
document_id: "ea56c8ff2a8bb02e3c3d2387d6ae47eb4f1861f2bbda13036a4ba2ba443cae1a"
company_key: "yc-alba-orbital"
company: "Alba Orbital"
source_id: "yc-alba-orbital-rss-54d1f111c1bd"
canonical_url: "https://www.albaorbital.com/new-blog/2021/1/26/miniaturise-satellite-payload"
published_at: "2021-01-26T13:53:39+00:00"
first_seen_at: "2026-07-24T15:02:38.589009+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:e450a2972bc1efad63711d2362aeff7ac901c2bcb24d1eff428de7453293a66c"
---

# How to miniaturise your satellite payload

PocketQubes are small, and this can present design challenges; but the tips and tricks below should help you out when miniturising your payload or designing your satellite to the PocketQube standard.


Payload board for a ‘Unicorn-2’ 3p PocketQube Satellite (next to Lego figure for scale).


**Increase the number of layers**


This might seem obvious, but don’t be afraid to add layers, you can get up to 10 in a standard 1.6mm board. This will allow you to be far more compact in your layout. It also gives you far more flexibility for managing EMI on your now much smaller board. Try to define your layer stack at the beginning of the project.


Photot credit: (D. Carey, "Packaging for Portables; Going Vertical & Getting Small," Central Texas Electronics Association (CTEA) Electronics Design and Manufacturing Symposium, October 7, 2010.)


**Stack Boards**


When designing payloads for compact environments stacking PCBs on top of each other can be an easy way of creating more space. However, care must be taken with connector selection to ensure signal integrity is maintained throughout the PCB stack.


**Harmonise power rails** :


If possible, reducing the number of different voltages required by the circuit can save a lot of space, both internally and externally. Power planes take up space internally and power converters take up valuable real estate on the external layers. Additionally, the size of power converter passive components can be reduced by moving to a higher frequency power converter.


There is often a fear of using compact high frequency power converters because of the increased EMI, however this can be mitigated using appropriate design techniques.


**Alternative Packages or Components:**


Wherever possible opt for surface mount packages, this is particularly important if you have a board with a high number of layers.


Components should be checked to see if there are smaller packages available or if possible an alternative with a smaller package should be used.


**(Photo credit: Twitter @gregdavill)**


**Connectors:**


Unfortunately moving to a compact design might mean leaving behind your favourite chunky connectors. But there are a huge range of compact connector solutions available. The connector should be considered early in the design.


**EMI (Electromagnetic Interference):**


Managing EMI is a huge topic, so here I have listed some headline points and some resources if you want to learn more.


1.


Don’t just assume it will be fine because you are working from an existing design. You now no longer have the luxury of physical distance to attenuate radiated EMI.


2.


Evaluate your decoupling capacitors, then add more. Then add some more….then maybe just a couple more…


3.


Think about what components will be generating the EMI (i.e switch mode power supplies, inductors, FPGAs), and what components will be sensitive (i.e sensors). Keep these components and traces separate as much as possible. For example, route your high and low speed traces on separate planes.


4.


Use differential pairs for sensitive analog signals.


5.


Be extremely careful when splitting your ground plane across two or more layers. If you do, take care to use buried and blind vias to only connect to the appropriate ground planes. Multiple ground planes should be via-stitched together round the edge of the PCB and at the power supply. **I would highly recommend reading sources 4 and 5 linked below to get a good understanding of grounding in PCB design.**


### **Sources:**


1.


[7 Tips and PCB Design Guidelines for EMI and EMC](https://www.protoexpress.com/blog/7-pcb-design-tips-solve-emi-emc-issues/)


2.


[Reduce buck-converter EMI and voltage stress by minimizing inductive parasitics](https://www.ti.com/lit/an/slyt682/slyt682.pdf)


3.


[Low-EMI buck converter powers a multivariable sensor transmitter with BLE connectivity](https://www.ti.com/lit/an/slyt693/slyt693.pdf)


4.


[SUCCESSFUL PCB GROUNDING WITH MIXED-SIGNAL CHIPS - FOLLOW THE PATH OF LEAST IMPEDANCE](https://www.maximintegrated.com/en/design/technical-documents/tutorials/5/5450.html)


5.


[Staying Well Grounded](https://www.analog.com/en/analog-dialogue/articles/staying-well-grounded.html)


[Request Free Unicorn-2 payload sample](http://www.albaorbital.com/unicorn-icd)
