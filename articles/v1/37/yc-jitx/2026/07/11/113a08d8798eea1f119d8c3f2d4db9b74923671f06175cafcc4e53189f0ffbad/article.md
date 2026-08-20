---
schema_version: "1.0.0"
document_id: "113a08d8798eea1f119d8c3f2d4db9b74923671f06175cafcc4e53189f0ffbad"
company_key: "yc-jitx"
company: "JITX"
source_id: "yc-jitx-news-import-aad5bf5f2004"
canonical_url: "https://www.jitx.com/blog/automating-everything-in-a-usb-cable-tester-pcb-design"
published_at: null
first_seen_at: "2026-07-24T05:01:33.070480+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:4b46ab2b6408852ca98917a9f77b1b0e47d896018ab517d6b70922eefd97e202"
---

# Automating Everything in a USB Cable Tester PCB Design

‍


#### **TL;DR**


I’m Cayden, an electrical engineer and transhumanist hacker. In this article, I explore the frustrations of repetitive manual tasks in PCB design and the benefits of automation. I demonstrate the automation of tasks such as connecting pins, generating test points and indicator LEDs, programmatically placing components, and autorouting.


## **Introduction**


If I’m writing a program, I’ll write a function one time, then use that function every time afterwards. Usually, if problems arise over and over again, we solve them once really well, then we don’t solve them again.


Yet as an electrical engineer designing circuit boards, I often burn a lot of time on repetitive tasks. These tasks can get annoying - they’re something I’d rather automate.


For example, to design a simple USB-C cable testing board I would have to:


• Calculate and source a different current limiting resistor to get the desired brightness for every color of LED.


• Type out, size, and place a custom text label that tells me what net each test point belongs to.


• Redraw supporting circuitry from scratch every time I reuse a component in a new design.


• Recheck datasheets many times as I connect various GPIOs and peripherals.


It’s annoying to repeat tasks like this, and each piece of manual work carries a little risk of making a mistake. For my first design with JITX, I wanted to see how many of these pains I could automate away, and I wanted to know what design would feel like when I did. Electrical design shouldn’t be death by 1,000 paper cuts. Let’s automate the simplest board possible and see what that does for our experience as designers.


> You can see the full code and design files for this project in the JITX Cookbook:[https://github.com/JITx-Inc/jitx-cookbook](https://github.com/JITx-Inc/jitx-cookbook)


### **The Project**


A USB-C cable tester board tests every wire in a USB cable. It has two ports which connect to either end of the same cable. The board sends a voltage through each independent wire in the cable, through an indicator LED, and to ground. If the LED lights up - there is a connection. If it doesn’t light up - no connection.


**Indicator LED Schematic**


‍


Bad USB cables cause headaches when doing board bring-up - I’ve personally spent hours debugging the UART on an embedded system only to find that the USB cable had a bad data connection. A USB cable tester can indicate the type and health of a USB cable, saving us from this pain.


I’ve purposefully chosen a simple, easy design as I want to explore PCB design automation, and this will help me focus on trying to automate common, repetitive tasks.


### **Automation TODO**


What can one automate here? I’ll try:


1. Connection of nets to the USB connectors.


2. Calculation of 18 LED ballast resistors given a desired color and brightness.


3. Naming all 18 test pads.


4. Placement


Let’s do it.


### **How?**


JITX EDA Tool in VS Code


I’m going to use JITX to automate these tasks. JITX is a software defined electronics CAD tool. It’s similar to KiCad, Altium, and the other EDA packages, but instead of *drawing* PCBs, we *define* PCBs in code. That gives one the ability to turn manual tasks into code.


### **Pin It To Win It - Connecting Pins**


Let’s create two USB connectors and a power net:


```text


net POWER (power)
...
public inst in-usb : components/USB-C-1054500101/component
public inst out-usb : components/USB-C-1054500101/component


```


``


One side of the USB should be fully connected to our positive voltage rail. Instead of drawing a wire from every pin, let’s just connect them all at once:


```text


; connect all pins of in-usb to power
net (POWER pins(in-usb))


```


``


### **Test Points, Test Points Everywhere**


I’m going to create an indicator LED for every connection in the USB cable, so I’ll start with a text label and test point for each net. In the traditional EDA process, these are made and placed manually. I’ll automate it with a function that identifies the connected net and generates a silk screen label for the test point. This is a general helper function which I’ll write once now and expect to use over and over again in future designs.


Below, I define a function which makes a test point and places a silk screen label with custom text:


```text


; place a testpad
defn my-testpoint-strap (tp:JITXObject, tp-name:String, diameter:Double) -> JITXObject:
inside pcb-module :
public inst tp-pad : gen-testpad(diameter)
net (tp-pad.p tp)
; name the test point
value-label(tp-pad) = Text(tp-name, 1.0, W, loc(1.2, 0.0))
inst my-label : ocdb/artwork/board-text/text(tp-name, 1.0, 0.0)
place(my-label) at loc(1.2, -0.4, 0.0) on Top (relative-to tp-pad)
tp-pad


```


I can call this function in the above loop that generated the LEDs. I have now generated testpoints with custom silk screen labels.


```text


; add a testpoint for this pin
val test-pad = my-testpoint-strap(IN, net-name, 2.0)


```


### **Next Generation LED Generation**


To test all the USB connections, the other USB connector must be split into separate nets. I’ll set up those nets:


```text


; make nets for all of the pins on the usb C that we want to test
for p in pins(out-usb) do : ; for each pin of the USB, we make a net
make-net-from-pin-name(p)


```


Each of these nets should then connect to an indicator LED. But first, to help with routing, I can order the nets based on their physical location.


```text


; get all pins of the USB, and order them based on how they physically appear
val ordered-pins = get-pins-physically-ordered(out-usb)
; convert from ordered list of pins to ordered list of nets
val ordered-nets = map(get-named-net{self, _}, ordered-pins)


```


**Routes are made in order of pads' physical location.**


‍


Routes are made in order of pads’ physical location.


Now to generate an indicator LED for every USB pin. It’s a common issue for different LEDs on the same board to have vastly different brightnesses, like this:


**Uneven LED Brightness (Credit: SparkFun)**


‍


To automate this common task and to make sure I don’t hit the common uneven LED brightness “gotcha”, I define a function called generate-test-led which takes in a color and an input voltage and uses those values to pick an LED and a suitable ballast resistor to achieve a nominal brightness.


```text


; take a Tuple of pins we want to add status LEDs to, and the GND pin. Add status LEDs and test pins to each of the input pins.
defn generate-test-leds-array (in-pins:Tuple, gnd-pin:JITXObject, voltage:Toleranced) :
inside pcb-module :
val colors:Seq = generate-rainbow(length(in-pins))
for (in-pin in in-pins, color in colors, place in 0 to false) do :
; create test LED and connect it
inst test-led : generate-test-led(voltage, to-string(ref(in-pin)), color)
net (in-pin test-led.in)
net (gnd-pin test-led.out)


...


; generate an array of test LEDs for each net we just made
generate-test-leds-array(ordered-nets, GND, property(battery.power.vdd.voltage))


```


‍


### **Laziness Redefined - Programmatic Placement**


I now have 18 LEDs, 18 test points, and 18 ballast resistors. If I were to export now and open it in a legacy CAD tool, it’s just a big pile of components:


Usually, the strategy now is to grab every component and manually drag them wherever we want. Or, one might use some basic alignment automation in existing CAD tools, but this still requires manual effort and time.


Let’s do it better with automation. I can programmatically place components or submodules in code, which happens faster and works better than manual placement. In the for loop where I created the testpoints and LED modules, I can programmatically place anything I want on the board, with a bit of iterative arithmetic each time through the loop:


```text


val colors:Seq = generate-rainbow(length(in-pins))
for (in-pin in in-pins, color in colors, place in 0 to false) do :
; create test LED and connect it
inst test-led : generate-test-led(voltage, to-string(ref(in-pin)), color)
net (in-pin test-led.in)
net (gnd-pin test-led.out)


; place components here
...


```


Now everything is automatically placed. Better yet, if I change the LED type, test point size, board size, etc., it doesn’t matter, this will rerun to automatically yield the layout I want. Cheers to laziness!


### **Routing in Your Sleep**


JITX has a built in autorouter, which automates a significant amount of the routing process. I can route most of this board in 30 seconds with a couple of clicks:


‍


Notice these autorouted traces look different than the traces on the physical board. In the first pass of this design, I manually routed everything in an external CAD tool. Afterwards, JITX released an autorouter, so I redid the routing with the autorouter, which is what you see above.


### **Conclusion**


This is some very simple automation, but it does feel a bit magical to have the test points automatically created and properly named. The manual process of creating silk screen text and sliding it around in EDA board tools is always annoying, so I’m glad to have it automated away now and in the future. The LED generation module probably didn’t save me time *yet* , as I had to write it from scratch, which takes time, but now it exists forever, and I’m looking forward to reusing it many times in the future.


It’s not all perfect though - one might notice that the LEDs on this board are **bright** . While all of the LEDs were the same brightness - they were all brighter than I’d want. I manually chose 50 mcd as the intensity, and this was way too high. An automation improvement would be to accept an enum of lighting options (e.g. nightime, indoor, outdoor) that could automatically choose the correct intensity for the application.
