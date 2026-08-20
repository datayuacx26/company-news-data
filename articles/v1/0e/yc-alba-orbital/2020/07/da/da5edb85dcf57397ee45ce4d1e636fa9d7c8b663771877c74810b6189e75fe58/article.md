---
schema_version: "1.0.0"
document_id: "da5edb85dcf57397ee45ce4d1e636fa9d7c8b663771877c74810b6189e75fe58"
company_key: "yc-alba-orbital"
company: "Alba Orbital"
source_id: "yc-alba-orbital-rss-54d1f111c1bd"
canonical_url: "https://www.albaorbital.com/new-blog/2020/7/23/how-to-write-software-for-pocketqubes"
published_at: "2020-07-23T17:36:43+00:00"
first_seen_at: "2026-07-24T15:02:38.589009+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:ff41630bcff86d449041f6834ff2d2cd7e81972f3518fe4515badf1774c7aae2"
---

# How to write software for PocketQubes

PocketQubes are much smaller than CubeSats but they need to deliver the same or similar functionality with regards to software. For a breakdown of what flight software is, have a look at this article on CubeSat FSW \[1\]. The key differences in implementation come from the consequences of smaller size, reduced power and storage space. While CubeSats can power a Raspberry Pi and run linux, PocketQubes might just have one microcontroller with Arduino code that does everything. PocketQubes therefore need to be much more efficient in every aspect to be able to deliver the same functionality. Computing power, electrical power, code storage, data storage and bandwidth, need to be carefully managed as a scarce resource.


*A number of Unicorn PocketQubes in production*


Firstly, you need a decent IDE like the Eclipse based Code Composer Studio, it helps if your chosen microcontroller comes with it. A decent IDE will help you organize and debug your code. Having separate files for all your modules and separate functions for all actions is a good start! For satellites that do more than say ‘hello world’ the basic Arduino editor won’t cut it. While Arduino is an invaluable tool, it is not sufficient for full system development. Which means, that yes, you will need to write your own drivers, or at least port them. There is an upside to this however: complete control over what your code does.


Secondly, you need hardware. I can’t stress this enough, having identical hardware to flight is the most important resource you can have. It allows you to do hardware in the loop development and testing and to test every line of code you write, as you write them. It allows you to play with your creation and discover unexpected fail cases, correct them and handle them safely if they happen again.


The amount of power generated and used on a satellite is a whole story of its own. From a software point of view, power management means turning things off when you don’t need them. Identifying power hungry peripherals and making them on-demand is key. And when you do run out of power, you need one or more emergency modes.


One usually does not have to worry too much about the size of the compiled code. However, when you want full ADCS functionality for example, you need to import a lot of libraries, and that’s when you run out of space on your microcontroller (even when all compiler optimizations are on). So you look for a chip with the biggest code storage space and in some cases end up having to use a separate chip for one task.


*2 Unicorn-2’s on their way to space*


Unicorn-2 has two microcontrollers, one solely dedicated to ADCS. There are a number of reasons for this: the code size for the ADCS is large enough to need a dedicated microcontroller, the time constraints on the ADCS control loop justify having a separate processing core and having two microcontrollers also allows us to develop and debug code for ADCS independent from OBC.


The time it takes to store your data, retrieve it, process it and transmit it are all important. One link in this chain can slow your entire system down, so it is key to optimize each one separately and find out what is it that you need to do to speed each one up.


Lastly, component selection for electronics can have a huge impact on software development. Some chips have great examples and software support while others do not. It could shave months off development if the electronics team consults the software team before selecting any component that requires software access.


\[1\] “Flight Software | Phoenix CubeSat.” http://phxcubesat.asu.edu/subsystems/flight-software (accessed Jun. 18, 2020).


[SATELLITE](http://www.albaorbital.com/unicorn-2)


[LAUNCH](http://www.albaorbital.com/launch)


[GROUND STATION](http://www.albaorbital.com/ground-station)
