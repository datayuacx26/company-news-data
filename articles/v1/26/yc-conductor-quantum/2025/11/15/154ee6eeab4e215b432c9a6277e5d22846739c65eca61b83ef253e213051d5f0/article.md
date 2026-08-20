---
schema_version: "1.0.0"
document_id: "154ee6eeab4e215b432c9a6277e5d22846739c65eca61b83ef253e213051d5f0"
company_key: "yc-conductor-quantum"
company: "Conductor Quantum"
source_id: "yc-conductor-quantum-rss-fd18baa21fa7"
canonical_url: "https://blog.conductorquantum.com/p/persistent-measurements-live-plotting"
published_at: "2025-11-08T02:35:42+00:00"
first_seen_at: "2026-07-24T23:15:28.367668+00:00"
fetched_at: "2026-07-28T20:55:21.781978+00:00"
content_hash: "sha256:d3ddbe1e7e6e15e9ebc3c7bb70e0d96e7eae409a3b95f0d903a279c586052573"
---

# Persistent Measurements, Live Plotting, GPIO… - Stanza updates this week.

# Persistent Measurements, Live Plotting, GPIO… - Stanza updates this week.


### Product


Nov 08, 2025


This week, Stanza receives its most anticipated upgrade yet — one built around the workhorse of every quantum engineer’s toolkit: Jupyter notebooks.


At the core of this release is a completely reimagined notebook workflow. Engineers can now visualize results in real time with inline live plotting, and maintain persistent measurement instances that automatically manage logging and state. The result: no restarts, no data loss, and seamless continuity across experimental sessions.


Thanks for reading! Subscribe for free to receive new posts and support my work.


Plus, we’ve added extra benefits such as General-Purpose Input/Output (GPIO) control and charge carrier support straight from the device config. Read more below…


## Live plotting


Some engineers love to peek under the hood and see exactly what our algorithms are measuring. Others prefer to fine-tune their devices by hand — for the joy of it, or to chase down that one elusive anomaly. Either way, live plotting turns every experiment into an interactive experience.


Now, with real-time data streaming built directly into Stanza, you can watch electrons move as measurements unfold — right from your desktop.


## GPIO control


Hardware control just got easier. With built-in General-Purpose Input/Output (GPIO) support straight from the Stanza config, you can trigger relays,


[blink LEDs](https://journals.aps.org/prapplied/abstract/10.1103/PhysRevApplied.22.034044) ,


[control multiplexers](https://www.nature.com/articles/s42005-024-01806-3) or orchestrate complex control sequences without leaving Stanza. One config, one workspace full control.


Just add your gpio connectors to your device.yaml and away you go.


```text
gpios:


VSS:


type: INPUT


control_channel: 5


v_lower_bound: 0


v_upper_bound: 3.3


VDD:


type: INPUT


control_channel: 6


v_lower_bound: -3.3


v_upper_bound: 0
```


## Electrons or holes?


Are you working with an


[ambipolar device](https://www.nature.com/articles/s41928-022-00722-0) , or do you work with a


[range of semiconductor samples](https://www.nature.com/articles/s41598-024-67787-z) that use different charge carriers? If you’re struggling to keep track, don’t worry - you can set this straight in the device.yaml config. Also, Conductor’s pre-built tuning routines can read from this setting and set the search space parameters for tune-up based on your charge carrier setting automatically.


## Persistent measurements


Jupyter notebooks crash all the time, but they are the staple of the quantum engineer. If the notebook crashes, don’t worry - your measurements and automated tuning procedures will continue in the background. Plus you can now keep track of what is happening directly in the terminal with live measurement logging. Our persistent measurement logging includes a ipykernel manager that allows users to coordinate multiple jupyter notebook sessions within a single terminal window. It is particularly useful for running long-running processes in the background, detaching from a session without terminating the processes, and reattaching later. We see this being useful going forward as quantum engineers put multiple experiments into a fridge and control them with the same lab pc.


Got a feature idea? We’re all ears.


[Share your requests](https://github.com/conductorquantum/stanza/issues) and help us build the next generation of Stanza together.


Thanks for reading! Subscribe for free to receive new posts and support my work.
