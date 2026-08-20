---
schema_version: "1.0.0"
document_id: "f913ba9ddb5f9faec03925c8b61047b7a8c489474733839219882a518b5e30e1"
company_key: "yc-polymath-robotics"
company: "Polymath Robotics"
source_id: "yc-polymath-robotics-news-import-0eb697031937"
canonical_url: "https://www.polymathrobotics.com/blog/polymath-and-foxglove"
published_at: null
first_seen_at: "2026-07-24T03:33:00.269851+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:73a3525eb05e9ae652be9b599d30a55734aea3478ec329cb7e5fa2b4ab097e07"
---

# Polymath Robotics Blog | How Polymath and Foxglove Simplify Fleet Autonomy

Polymath Robotics and Foxglove have been working together to streamline how robotic systems are deployed, debugged, and demonstrated in the field. This post highlights two tools that came out of that collaboration: a secure way to access live robot data through a browser and a fast, visual interface for building and managing routes.


Both are now part of Polymath’s standard workflow. They also help customers operate and understand their own fleets faster and with fewer headaches.


### About Polymath Robotics


Polymath Robotics, founded in 2021 and based in San Francisco, builds autonomy infrastructure for off-road and industrial vehicles. The platform provides modular planning, control, and safety systems designed to support real-world deployments across a wide range of environments. Polymath works with customers in sectors such as defense, logistics, mining, and agriculture – teams that need to operate safely at scale without relying on high-bandwidth connectivity or remote control rooms.


### Simple, secure, shareable access to live robot data


Polymath has robots operating on every kind of network: from cellular to enterprise, and even public WiFi. Getting live data off those systems used to require either sending an engineer onsite or giving a client full VPN access, which was clunky to set up and introduced security concerns.


Instead, Polymath uses Tailscale to connect internal systems and robots over a VPN based mesh, and Tailscale Funnel to expose specific services like Foxglove Web safely over the internet. This allows any authorized person to see live robot data in a browser, with no local tooling or VPN setup required.


This setup makes it easy for both Polymath sales teams and customers to pull up live robot data from anywhere, using just a browser. It’s especially powerful during demos or field evaluations, where seeing real-time performance matters.


‍


#### **How it works**


- Each robot runs a lightweight Foxglove bridge that dynamically subscribes to ROS topics
- Foxglove Web is hosted locally on the robot or a paired dev machine
- Tailscale Funnel exposes that instance on a unique public URL
- All data is transmitted using WebSocket Secure (WSS), preserving HTTPS compatibility
- The command to enable the tunnel is scripted and respects valid TLS certificates


> “The trick is exposing a live robot interface securely, without touching VPN permissions. Funnel makes that possible - and reversible. We can turn it on or off whenever we want. And because Foxglove is already handling all the visualization for us, we can just plug it in and go. No extra work on our side”
> — Ilia Baranov, Polymath Robotics


**The exact setup:**


To expose Foxglove Web over Tailscale Funnel securely, Polymath uses the following commands:


- On the robot:


- tailscale funnel --tls-terminated-tcp 443 tcp://localhost:8765


- In Foxglove on the laptop, open a connection to:


- wss://TAILSCALE_DNS_HOSTNAME:443
- Note: use wss:// instead of ws:// since Foxglove supports secure WebSocket connections out of the box.
- Note2: you have to use the DNS Hostname from tailscale, not the IP address


Feel free to try it for yourself!


‍


***Above:*** *Live robot data from a mining truck in Brazil*


#### **Why it matters**


- Clients don’t need access to Polymath’s VPN or infrastructure to view their live robot data
- URLs can be short lived and scoped to read-only
- It works on any device: phones, tablets, laptops
- Polymath can curate Foxglove panel layouts that show the exact data the client needs
- The setup is API-controllable, meaning it can be gated, triggered, or revoked on demand


This gives customers and stakeholders instant observability for testing, debugging, and presentations, without compromising security. It’s simple enough to use in a boardroom demo, but robust enough to support real development.


### Route planning: from manual steps to a visual, interactive interface


For autonomy teams, planning a route usually means dealing with waypoints, constraints, and service calls. Before, Polymath engineers wrote these by hand – specifying node IDs, coordinates, and edge logic directly in code. Now, they use a custom control panel built inside Foxglove to design and deploy routes in seconds.


***Above:*** *point and click implementation of routes for a client robot*


#### **Why a visual interface matters**


The panel lets engineers drop nodes and draw connections directly inside Foxglove, which already visualizes the robot’s state, environment, and sensor data. Instead of switching between tools, the same interface is used to plan and inspect the outcome.


> “It’s all in one place. You define the route and immediately see it rendered alongside your lidar, GPS, and map data. You’re not flipping between systems. It just works in one window.”
> *— Ilia Baranov, CTO & Co-Founder at Polymath Robotics*


#### **Technical setup**


- The panel calls ROS services already exposed by Polymath’s autonomy stack
- Routes are constructed using a node and edge graph structure
- Each node is a location in 2D or 3D space
- Edges define valid connections, like rails between stations
- Foxglove's 3D panel renders the full plan live as it’s being built


‍


#### **Where this helps**


Graph based planning is especially useful when space is tight, safety zones are strict, or rules of the road apply. For example:


- Yards and depots where robots must follow repeatable, validated paths
- Mines or other industrial sites where certain routes are blocked at specific times
- Flightline or warehouse scenarios where fine-grained control is required


This visual interface has now become the standard engineering tool for internal use and for partners. It’s not consumer facing, but it’s far more usable than a command line or raw ROS service calls.


And since it is built inside Foxglove, any route can be immediately verified against real sensor data. That removes guesswork and tightens the loop between planning and execution.


### Why Polymath chose to build on Foxglove


Polymath *could* have built a custom visualization engine, but that would have required years of development with no guarantee of matching what already exists in Foxglove. Foxglove supports pointclouds, images, topic / service integration, and shared layouts out of the box.


> “If we had to build this ourselves, it would take years, and still be worse. Foxglove lets us focus on core autonomy instead of reinventing visualization.”
> *— Ilia Baranov*


By building control panels and observability on top of Foxglove, Polymath gains a flexible, unified interface that works for both engineers and customers. Everything from sensor diagnostics to mission planning happens in one place.


### What's next


Polymath’s collaboration with Foxglove has produced two real, usable tools:


- Secure access to live robot data through a browser, powered by Tailscale Funnel
- Visual route planning inside Foxglove that plugs directly into existing ROS services


These tools are already helping Polymath and its customers move faster, debug more confidently, and deploy with fewer surprises. They are simple to use, robust in the field, and scale cleanly to fleets.


Polymath continues to build in this direction: real tools that lower the barrier to deploying autonomy in production. With partners like Foxglove, the goal is not just smoother workflows, but infrastructure that feels like it belongs in 2025, not 2015.


If you're building autonomy and you're tired of fighting your tools, we think you'll like where this is going.


*Watch the full discussion between Polymath CTO Ilia Baranov and Foxglove CEO Adrian Macneil:*[https://foxglove.dev/events/how-foxglove-and-polymath-simplify-fleet-management](https://foxglove.dev/events/how-foxglove-and-polymath-simplify-fleet-management)
