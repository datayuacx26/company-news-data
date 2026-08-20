---
schema_version: "1.0.0"
document_id: "cb062a08f453a79ee73e87c1f0e7e4204ed2ea672e5a3fe8c15a943cd4d680b6"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/hackathon-project-viewing-datadog-metrics-in-minecraft/"
published_at: "2017-06-15T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:4cfb0945e5bbb9ff213e67b19235cd3efaba3d74c00b158956d5be8e5eb89de6"
---

# Hackathon project: Viewing Datadog metrics in Minecraft

Christian Mauduit


Corentin Dancette


Datadog recently held an internal hackathon. We run these about twice a year across the organization. Each team hacks on the project of their choice for 2 days. Projects vary from process improvement, to new feature ideas, or just plain old fun hacks.


At Datadog we see and gather metrics everywhere by using Datadog to monitor our applications and infrastructure. So our team thought it’d be fun to come up with creative solutions to “where can we display metrics?”.


We decided to see if we could build a way to display Datadog graphs and dashboards inside the world of Minecraft. We know engineers are often gamers too, and while we do not expect our customers to monitor their production environments in Minecraft, we thought showing metrics inside the game world might help you get more game time while on-call.


Our goal with this was:


-


Programmatically access Minecraft elements. More precisely being able to create, remove and query information about blocks.


-


Programmatically retrieve real-time data from Datadog.


-


Glue it all together to read data from Datadog and pump it into Minecraft.


## Control Minecraft


The first step was to control Minecraft via Python. We picked Python because we use it heavily at Datadog and are familiar with the language. It also has lots of existing libraries and documentation to make Minecraft interactions straightforward. One way to do this was to use[Raspberry Juice](https://dev.bukkit.org/projects/raspberryjuice) , which implements a subset of the Minecraft API; and[PI Edition](http://minecraft.gamepedia.com/Pi_Edition) , a Minecraft server for Raspberry PI. We set this up on our laptops rather than installing dedicated PI boxes for this hack as it offered improved build times and performance. Once we had set up a dedicated server using Raspberry Juice, we simply had to import the Python library[py3minepi](https://github.com/py3minepi/py3minepi) and we were all set with a development environment.


Interacting with this environment (creating blocks) via Python code was as simple as:


```text
1   import mcpi.minecraft as minecraft    2   import mcpi.block as block    3   mc = minecraft.Minecraft.create("127.0.0.1")    4   mc.setBlock(10, 10, 10, block.STONE.id)
```


Once you can do this, you can do anything... The power of Python unleashed in Minecraft. Once you can do this, you can do anything... The power of Python unleashed in Minecraft.


Once you can do this, you can do anything... The power of Python unleashed in Minecraft.


## Access Datadog data


To access data from Datadog we installed the[Datadog Python library](https://github.com/DataDog/datadogpy) and followed the[API reference](https://docs.datadoghq.com/api/) .


```text
1   from datadog import initialize, api    2   Import time    3
4   options = {    5          'api_key':'01234abc01234abc',    6          'app_key':'0123456789abcdef0123456789abcdef'    7   }    8
9   initialize(**options)    10   now = int(time.time())    11   data = datadog.api.Metric.query(start=now - 300, end=now, query="avg:system.cpu.idle{*}")
```


Once we were able to both query data and draw objects, it was rather trivial to integrate the two pieces. We also started to implement some monitors, which would shine red when in alert mode, and resume to green in normal mode.


## Challenges


### Configuration


We rapidly found out we would need to describe our world with configuration files rather than rely on data mixed in function calls. So we set up some basic YAML files containing information such as:


-


Where a graph should be (size, position, orientation, ...)


-


How a graph should look (colors, transparency, ...)


-


What data it should display (metrics to get, time span, ...)


Here is a sample YAML configuration file, representing a dashboard with CPU usage and a monitor alert :


```text
1   - graph2:    2        type: wall    3        query: "avg:system.cpu.idle{*}"    4        pos1:    5          x: 81    6          y: 40    7          z: 170    8        pos2:    9          x: 92    10          y: 55    11          z: 170    12        layout: "xy"    13        border: true    14
15   - monitor_status:    16        type: monitor_status    17        monitor_id: 660012    18        pos1:    19          x: 81    20          y: 56    21          z: 170    22        pos2:    23          x: 86    24          y: 61    25          z: 170
```


A complete dashboard, displaying several metrics, the two dark squares represent monitors. A complete dashboard, displaying several metrics, the two dark squares represent monitors.


A complete dashboard, displaying several metrics, the two dark squares represent monitors. Everything is updated in real-time.


### Persistence


Another problem was that Minecraft is by nature persistent. That is once you create an object it lives on. Our graphs on the other hand change frequently as the data changes. More challenges was that as we iterated on the project our failed attempts to make graphs would populate the world with random cubes and and be cumbersome to navigate or clean up. To ease this pain we wrote vacuum functions to clean anything we drew.


### Displaying data


It feels awkward to draw graphs without being able to rely on JS and CSS as we do on the web. However as long as the data could be simplified to “one row of data”, we were able to build methods that would display the metrics from Datadog in a reasonably viewable manner. At some point, displaying large graphs would saturate the data pipeline, so we had to implement caching to save bandwidth. Interestingly enough, implement caching, saving bandwidth, is typically what we do in our daily job, as engineering for performance is commonplace at Datadog.


## How fun was this?


The first four hours were spent trying to set up the environment, and connecting various components together. It has the charm of random hacking – trying to find your way in technologies you usually ignore (ie Minecraft). Which is fun. Then we had up to 20 hours to play around like kids, build walls, view them, explore, destroy, rebuild, and try again. Is there anything more enjoyable than hacking for pure fun?


## See it in action!


To view your own metrics in Minecraft, fork[the repository on Github](https://github.com/ufoot/dogcraft/) .


Want to join us for our next hackathon? Datadog is hiring! Learn more on our[careers site](https://careers.datadoghq.com/) oremail us a description of your favorite hackathon project.


-
-
-
