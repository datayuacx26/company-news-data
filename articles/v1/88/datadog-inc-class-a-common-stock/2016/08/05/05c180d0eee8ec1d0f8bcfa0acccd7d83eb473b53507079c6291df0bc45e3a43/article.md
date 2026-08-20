---
schema_version: "1.0.0"
document_id: "05c180d0eee8ec1d0f8bcfa0acccd7d83eb473b53507079c6291df0bc45e3a43"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/restroom-hacks/"
published_at: "2016-08-23T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:27:31.756931+00:00"
content_hash: "sha256:092f4d8ccc9ad62d80c36e73ea45472327f194fac75c51c0a9e6139a0e7be2be"
---

# Restroom hacks

Chris Hoey


Daniel Benamy


Do you ever walk to the bathroom across the office only to discover that it’s in use? Then you’ve got to decide if you want to awkwardly hover right outside, or hold it in for a while and try again later. This is obviously a first world problem, but bathroom contention was getting to be a challenge as we quickly outgrew our office space.


At Datadog we are makers and hackers at heart. We like to tinker with software, but also with hardware and embedded systems. You will often find us digging into the latest microcontrollers, 3d printers, and more during our regular maker nights. Recently a new tinkerer joined our team and suggested solving the distributed bathroom state problem with monitoring, while we waited to move into our new office space this fall. We all thought that’d be a fun project!


## Goals


When we first sat down, we clarified what we wanted to do:


*Do not be creepy*


-


A bathroom is a private space; no video (obviously), or sensors which could be perceived as intrusive even if they’re not.


*Be reliable*


-


Minimize false positives and false negatives.


-


Try to use door lock status as the indicator of occupied.


-


Do not interfere with existing functionality of the doors or locks.


-


Low maintenance of the devices themselves with an easy way to update them.


*Keep it professional*


-


The office is still a place of business with frequent visitors so keep things neat and clean looking.


-


Keep any network-connected devices secure.


*Have fun!*


## The Real World is Complicated


As we approached each bathroom we found differences in everything from the style of locks, to location of power outlets. For example, some of the bathrooms have a single room with a push button lock on the handle, while others have multiple stalls with rotary style locks on each door for the stall. In some cases we found power outlets near the door, but in others the nearest outlet was in the ceiling, or there was no power source at all. One bathroom has terrible wifi reception because the walls are cement and there’s a scary array of decades old power equipment creating interference.


Locks are not standard across bathrooms. Each bathroom required a different solution. Locks are not standard across bathrooms. Each bathroom required a different solution.


## Solving Problems


We decided to go with Raspberry Pi 2 Model Bs as the brains of the project. Being able to run Linux and connect via SSH through WiFi greatly simplified the management of the devices and made it easy for us to try new ideas. The Raspberry Pi is quite small so hiding them out of sight was easy. We would have loved to use Raspberry Pi Zeros but they were in high demand (read: out of stock) so we used what we had around.


For the actual sensors we used a mix of alarm-style magnetic reed switches, pin switches, and bought, but wound up not yet using, photoresistors:


Magnetic reed switch Magnetic reed switch


pin switch pin switch


Photoresistor Photoresistor


Sensing the lock position is the most direct and reliable way to know the availability for a given bathroom, but the bathrooms with the push button locks on the handle are tricky to measure, while keeping the sensor out of the way.


As a quick workaround for these we used magnetic reed switches to detect if the door is open or closed. We didn’t think this would be very reliable since people might close the door when they leave so we also purchased photoresistors thinking if the bathroom is dark, we could tell that it wasn’t in use even if the door is closed. We “launched” our MVP without the photosensors being used though, and it turned out to work perfectly well in practice!


To connect the sensors to the pi, we cut a[f/f jumper wire](https://www.adafruit.com/categories/468) in half, and spliced it with some wire that we attached to the sensors:


Here’s one sensor hooked up between GPIO 14 and ground:


We installed it in the first bathroom, hiding the wires in wiremolding and the Raspberry Pi in an outlet box:


Some of the other bathrooms had stall doors with a locking mechanism that slides out. This begged to have some sort of sensor that the lock slide would come in contact with. The trick was how to hide the switch and wires while not interfering with the locking behavior.


We noticed that the metal stall panels are hollow so we wanted to hide the sensors in them. Turns out there’s a special pin switch commonly used for things like car doors and trunk latches which was the perfect size, and well suited for contact with the stall lock.


We could have 3d printed or machined a nice fitting to mount it, but in the end a quick wood block drilled and carved to hold the switch worked nicely and was faster to make.


## Everything is a File


Now that we had the hardware working we needed a way to read the value of the sensors and provide the availability in an easy to access manner. To do this we set up the Raspberry Pi to interact with the GPIO pins as files via <code>[/sys/class/gpio/](https://www.kernel.org/doc/Documentation/gpio/sysfs.txt) </code>. This makes it rather trivial to get the sensor status via a python script. Since the status of the GPIO pins can vary by the sensor being of type normally open or normally closed we made a simple config to define the bathroom and expected sensor states. After we put the script behind <code>[tcpserver](https://cr.yp.to/ucspi-tcp/tcpserver.html) </code> and ran it under <code>[daemontools](https://cr.yp.to/daemontools.html) </code> we were good to go!


## Keep it Simple, Stupid


<code>[Netcat](http://linux.die.net/man/1/nc) </code> is a fine way to check if the bathroom is available :-)


<pre> $ nc 11.bathrooms.datadog-internal.com 50


Bathroom Status: 11 left: Available 11 right: Available </pre>


Some people have even gotten fancy and added[TextBar](http://www.richsomerfield.com/apps/) entries:


Oops, missed my chance! Oops, missed my chance!


And of course we added bathroom status widgets on our[Datadog](http://www.datadoghq.com/) dashboards throughout the NY office.


Bathroom Status via TextBar Bathroom Status via TextBar


## That Was Easy


It’s a testament to the wonderful state of hackable embedded hardware and unix tools how little code we had to write; most of the work that went into this project was in figuring out the right kinds of sensors, mounting everything neatly, and yelling at wifi. The little bits of software and config we wrote are open source on Github as[datadog/bathroom](https://github.com/DataDog/bathroom) .


[Let us know](https://twitter.com/datadoghq) if you set one up or if you want to[come hack with us](https://careers.datadoghq.com/) !


-
-
-
