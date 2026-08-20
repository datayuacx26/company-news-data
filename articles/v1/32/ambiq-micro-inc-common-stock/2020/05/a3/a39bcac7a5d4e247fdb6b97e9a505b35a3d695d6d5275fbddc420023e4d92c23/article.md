---
schema_version: "1.0.0"
document_id: "a39bcac7a5d4e247fdb6b97e9a505b35a3d695d6d5275fbddc420023e4d92c23"
company_key: "ambiq-micro-inc-common-stock"
company: "Ambiq Micro Inc."
source_id: "ambiq-micro-inc-common-stock-rss-bc8fe30462b1"
canonical_url: "https://ambiq.com/news/tear-down-smartwatch-boasts-two-weeks-on-a-charge/"
published_at: "2020-05-11T14:51:00+00:00"
first_seen_at: "2026-07-25T01:11:44.521449+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:6c2e0e94ea662544a0550e9adcbd5e09b5eaac3c9b2366238f042d1bc3eda246"
---

# Tear Down: Smartwatch Boasts Two Weeks on a Charge

I was recently showing off my new[Garmin Forerunner 945 Smartwatch](https://buy.garmin.com/en-US/US/p/621922/pn/010-02063-00) . My friends were impressed, as it offers just about every feature that’s available on any other smartwatch. They were blown away by the one feature that I held out until last—it goes more than ten days on a charge. Their watches get three days at best.


##### **The Garmin Forerunner 945 packs just about every feature you can think of into this tiny space.**


The feature list is quite extensive. To be honest, I only use a handful of them. But those that I do use, I really enjoy. The two key applications for me both require use of the GPS which, as you’d expect from Garmin, work flawlessly. The first is the ability to track a run. The GPS knows exactly where I am, where I’ve been, and can map the route nicely. It tells me all the fitness-related stats associated with my run.


The second application that I take advantage of is the distance finder and scorecard when playing golf. With a game like mine, you take any advantage you can get. And knowing the distances to the hole within a yard or so is quite helpful. It also records all my scores, which helps keep me honest.


### **Maximized Battery Life**


Circling back to what really separates the Forerunner 945 from competitive devices is the battery life. The biggest reason for that, aside from some remarkable engineering, is the use of an[Ambiq® Apollo2 ultra-low power system on chip](https://ambiq.com/soc/) . The 2.5- by 2.5-mm Apollo2 is a second-generation part that operates at 10 μA/MHz, far better than the previous generation, which ran at 34 μA/MHz. Note that there’s a newer device now available, the Apollo3 Blue, that pulls the power spec even lower, as low as 6 μA/MHz and increases the amount of on-chip SRAM. The “Blue” designation relates to the device’s dedicated second core for ultra-low power Bluetooth® Low Energy 5 connectivity.


At the heart of the Apollo2 SoC is Ambiq’s patented[sub-threshold power optimized technology](https://ambiq.com/spot/) (SPOT®) and an Arm® Cortex®-M4 processor with a floating-point unit. The device integrates up to 1 Mbyte of flash memory and 256 kbytes of RAM to accommodate radio and sensor overhead while still leaving space for application code and algorithms.


I’m told that the[Xiaomi Mi Watch Color](https://www.firstbeat.com/en/consumer-product/xiaomi/mi-watch-color/) is designed with the same processor, but I haven’t had opportunity to dissect one of those (yet), so I can’t confirm. Note that it also boasts a two-week run time, so I’m guessing that has something to do with the presence of an Ambiq processor. But I’ll save that one for another article.


Interesting side note: The[Matrix PowerWatch](https://www.powerwatch.com/) , which also contains an Ambiq processor, is powered by an embedded solar cell. Hence, it never needs charging. That’s pretty cool. I got a peek at this one at the recent CES show.


##### **With space at a premium, the Garmin designers really packed the devices in. The first figure shows the front side of the board, while the second shows the back side, with the battery peeled away. The Ambiq device appears on the backside, in the upper left-hand corner of the figure.**


How do I know exactly which processor is inside the Garmin Forerunner 945? As you can see from the figure, I took the watch apart. As much as it pained me to do so, I had to see what was inside that allowed the watch to run so long on a single charge. The Forerunner 945 watch is mostly designed in-house by the team at Garmin, split between the U.S. and Canada.


I was impressed that the watch can be used underwater. The specs claim it can be used down to 50 m. I’ll take their word for that one. It’s also designed with Corning’s Gorilla Glass DX on the face. The bezel is constructed of fiber-reinforced polymer. I can see why it was so difficult to get apart. A vise, a hammer, and a sharp screwdriver were (unfortunately) the tools of choice.


### **Mostly Local Processing**


The Apollo2 SoC handles the sensor processing for the watch, and then processes that data locally. Being able to keep the data within the watch (and not have to go out to the Cloud for processing) offers many benefits. The biggest benefit is that the data can be processed and displayed in near real time. It also means that the watch needn’t always be in “connected” mode, chewing up battery power.


Keeping data local eases users’ security concerns, as the data needn’t ever go out to a third-party. Users can go for a run, play a round of golf, or do some other form of exercise, and have the fitness calculations displayed immediately upon completion. In general, the only time the watch ventures back to the Cloud is when it syncs with another device, like a phone, a tablet, or a PC to retain the data long term.


Note that there’s a second processor on the board as well, one provided by STMicroelectronics. That device drives the display, running a higher-level operating system, meaning the higher-level applications that are associated with the display. As you can see from Figure 1, the display provides ample information, and is controlled by the five buttons. They help switch between the various applications and control the backlight. The sunlight-visible, transflective memory-in-pixel (MIP) LCD can display 240 x 240 pixels.


One of the design considerations that the Garmin engineers had to keep in mind, as is often the case with a part that employs an internal voltage regulator, is that you want to have the capacitors and inductors as close to the processor as possible with very short traces.


### **Ample Memory**


Other components include a pressure sensor for altitude measurements, a heart-rate monitor, and the GPS. The Winbond 128-Mbit SDRAM offers some unique features to keep the power consumption to a minimum. For example, it supports special functions like partial-array self-refresh (PASR) and automatic temperature-compensated self refresh (ATCSR).


There’s a 128-Gbit NAND flash memory from[Kioxia America](https://www.kioxia.com/en-us/top.html) (formerly Toshiba Memory America) placed on the backside of the board. That’s how the watch can store its 1000 songs. The memory device runs in a fully synchronous mode, and operates from 1.8-V supply. And of course, there are antennas for each of the various radios (Wi-Fi®, Bluetooth, ANT+, and GPS).


According to the technical folks at Ambiq, the Garmin designers did their homework and really maximized the potential of the Apollo2 processor. They claimed that the two companies worked closely together throughout the design process, even on a weekly basis during the critical parts of the design. Note that Ambiq made it clear to me that they weren’t permitted to comment on its customers or their products. The company did, however, provide some high-level information for this article.


Ambiq provided the Garmin engineers with an SDK that included a hardware abstraction layer as well as the low-level interface drivers for the peripherals. Because the Apollo2 is based on a standard Arm Cortex-M4, it’s pretty familiar to most engineers. Ambiq uses standard compilers and development interfaces and tools such as a J-Link interface.


The majority of the power consumed by the Forerunner 945 comes from two areas: the backlight, which generally isn’t on for long, and the GPS, which could be on for extended periods during workouts like runs, walks, bike rides, and golf outings (yes, some people consider golf a workout). The device’s trans-reflective display doesn’t require a backlight when it’s used in bright lighting, so that’s a power savings. The LEDs used for heart monitoring are generally on all the time, although that is user configurable.


Finally, there’s a 3.8-V, 255mAh battery glued to the backside of the pcb. The designers did a nice job squeezing the battery in and ensuring that it doesn’t pose a heat problem to the watch, as it sits directly on top of the components.


The bottom line is that designing a watch with so many features into the available volume is tricky at best. Maximizing the potential of each of the components was a must, and the Garmin designers were clearly up to the task.


### **About the Author**


Richard Nass is the Executive Vice-President of OpenSystems Media. His key responsibilities include setting the direction for all aspects of OpenSystems Media’s Embedded and IoT product portfolios, including web sites, e-newsletters, print and digital magazines, and various other digital and print activities. He was instrumental in developing the company’s on-line educational portal, Embedded University. Previously, Nass was the Brand Director for UBM’s award-winning Design News property. Prior to that, he led the content team for UBM Canon’s Medical Devices Group, as well all custom properties and events in the U.S., Europe, and Asia. Nass has been in the engineering OEM industry for more than 25 years. In prior stints, he led the Content Team at EE Times, handling the Embedded and Custom groups and the TechOnline DesignLine network of design engineering web sites. Nass holds a BSEE degree from the New Jersey Institute of Technology.


[Follow on Twitter](https://twitter.com/rnass) |[Follow on Linkedin](https://www.linkedin.com/in/richnass/) |[Visit Website](https://www.embedded-computing.com/home-page/www.embedded-computing.com) |[More Content by Rich Nass](https://www.embedded-computing.com/authors/rich-nass)


Source:[https://www.embedded-computing.com/home-page/smartwatch-boasts-two-weeks-on-a-charge#](https://www.embedded-computing.com/home-page/smartwatch-boasts-two-weeks-on-a-charge#)


### **Maximized Battery Life**


Circling back to what really separates the Forerunner 945 from competitive devices is the battery life. The biggest reason for that, aside from some remarkable engineering, is the use of an[Ambiq Micro Apollo2 ultra-low power microprocessor](https://ambiq.com/mcu/) . The 2.5- by 2.5-mm Apollo2 is a second-generation part that operates at 10 μA/MHz, far better than the previous generation, which ran at 34 μA/MHz. Note that there’s a newer device now available, the Apollo3 Blue, that pulls the power spec even lower, as low as 6 μA/MHz and increases the amount of on-chip SRAM. The “Blue” designation relates to the device’s dedicated second core for ultra-low power Bluetooth Low Energy 5 connectivity.


At the heart of the Apollo2 SoC is Ambiq’s patented[sub-threshold power-optimized technology](https://ambiq.com/spot/) (SPOT) and an Arm Cortex-M4 processor with a floating-point unit. The device integrates up to 1 Mbyte of flash memory and 256 kbytes of RAM to accommodate radio and sensor overhead while still leaving space for application code and algorithms.


### **Mostly Local Processing**


### **Ample Memory**


There’s a 128-Gbit NAND flash memory from[Kioxia America](https://www.kioxia.com/en-us/top.html) (formerly Toshiba Memory America) placed on the backside of the board. That’s how the watch can store its 1000 songs. The memory device runs in a fully synchronous mode, and operates from 1.8-V supply. And of course, there are antennas for each of the various radios (WiFi, Bluetooth, ANT+, and GPS).


Ambiq provided the Garmin engineers with an SDK that included a hardware abstraction layer as well as the low-level interface drivers for the peripherals. Because the Apollo2 is based on a standard Arm cortex M4, it’s pretty familiar to most engineers. Ambiq uses standard compilers and development interfaces and tools such as a J-Link interface.


### **About the Author**
