---
schema_version: "1.0.0"
document_id: "2665e4b0c14b2807a99a33f8ea3f92ae2973840453b6f3118c5c69f9e4a0adc7"
company_key: "yc-heycharge"
company: "HeyCharge"
source_id: "yc-heycharge-rss-50b4f64ce74c"
canonical_url: "https://www.heycharge.com/news/we-engineered-the-internet-out-of-ev-charging/"
published_at: "2026-03-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.777661+00:00"
fetched_at: "2026-07-28T22:17:35.086279+00:00"
content_hash: "sha256:01badea22c2d794b6f5e3cc8c9f17cb7ecaffe5bda9434a53368cc0b2773a493"
---

# Do EV Chargers Need Internet? We Engineered Ours So They Don't

Every “smart” EV charger on the market requires a permanent internet connection to function. Your charger talks to a cloud backend, which talks to your solar inverter’s cloud backend, which talks to your energy provider’s API. If any link in that chain goes down, the intelligence disappears and you’re left with a dumb charger.


We spent four years building a platform that eliminates that dependency entirely. Today we’re launching MagicBox, a retrofit adapter that brings that platform to any existing OCPP-compatible wallbox. This post is about the architecture behind it and why we think offline-first is the right foundation for home energy management.


## The Reliability Problem


We originally built our platform for underground parking garages — concrete bunkers with no mobile signal, no Wi-Fi, and no one running ethernet to every parking spot. When you’re deploying 50+ chargers in a basement for a company like Vonovia (Germany’s largest residential landlord), you can’t assume connectivity. You have to engineer it out.


This wasn’t just an engineering preference — it’s an economic one. At multi-charger sites, running network infrastructure to every parking spot is a significant cost: cabling, switches, ongoing maintenance, troubleshooting. Eliminating that connectivity requirement cuts installation costs by over 40% for our property customers. That’s one of the main reasons we exist and thrive in that market.


But here’s what we learned over four years of deployment: while communications infrastructure is extremely difficult and expensive when retrofitting underground parking garages, it’s not easy, cheap, or perfectly reliable anywhere. Your home Wi-Fi drops. Your router reboots at 3am. Your garage is at the far end of the house, behind a wall or two, maybe below grade. Anyone who’s tried to keep a smart garage door opener reliably connected knows what I’m talking about.


The charger in your garage is probably the single most expensive smart device you own, and it’s in the worst possible location for connectivity.


## SecureCharge: The Platform


SecureCharge is our offline-first platform for the full EV charger lifecycle — not just charging sessions, but commissioning, configuration updates, user access management, consumption retrieval, and even over-the-air software updates. All of it works through users’ devices without requiring any internet connectivity.


A technician commissions a charger with their phone over BLE. A property manager updates access permissions the same way. A fleet operator retrieves consumption data from chargers in a basement with zero signal. The internet is never assumed.


### BLE is the control plane


Your phone — or your car directly via Android Automotive OS (Polestar, Volvo, Rivian, GM, and others) — talks to the charger over Bluetooth Low Energy. Authentication, configuration, data retrieval, software updates: all over a local BLE connection. No cloud roundtrip. Sub-second response. Works in a Faraday cage.


### Zigbee mesh is the coordination layer


In our multi-charger apartment deployments, chargers discover each other over a Zigbee mesh and coordinate autonomously — dynamic load management, demand response, energy distribution — without any central controller or networking infrastructure. We run buildings with 50+ chargers on a single grid connection this way.


### The cloud is optional


The cloud gets the data eventually for analytics and billing, but it’s never in the critical path. We built the platform to work with both the charger and the user offline at the time and place of charging. We realised later that losing connectivity and our backend going down look identical from the charger’s perspective — so SecureCharge continues to work even if our own backend goes down.


That’s not a redundancy feature we engineered in. It’s a property that fell out of the architecture.


This has been running in production for four years. 700+ chargers deployed, and we’ve shipped real hardware into the European market: an industrial gateway for multi-charger sites and DC fast chargers, and an Eichrecht-compliant (German calibration law) 22 kW AC charger, with a new 22 kW AC charger launching with our partner HUMAX in summer 2026.


## MagicBox: SecureCharge for the Home


The MagicBox: a compact retrofit adapter that brings SecureCharge to any OCPP-compatible wallbox.


MagicBox is something different from our existing hardware. Instead of replacing your charger, it retrofits the one you already have. It’s a small adapter that connects to any OCPP-compatible wallbox — that’s most chargers sold in the last five years — and brings the SecureCharge platform to hardware we didn’t build.


### What’s in the box


- A **Silicon Labs EFR32** running a dynamic multiprotocol stack — Zigbee and BLE on the same silicon (the same approach Philips Hue uses). BLE handles local authentication; Zigbee provides the mesh for our multi-charger deployments.
- A **Silicon Labs EFR32FG23** sub-GHz radio for long-range 868 MHz backhaul — the bridge between a charger in a garage with no Wi-Fi and the rest of the home or building network.
- An **ESP32-S3** for IP-facing connectivity (Ethernet and Wi-Fi) when a network path is available.
- **RS-485** for Modbus-based energy management systems, as well as a direct interface to the energy management system sold by our launch partner Easee.


### What it does


**PV surplus charging.** Charges your EV with solar excess, adjusts dynamically as production and household consumption change. Works with any solar system — no proprietary inverter-wallbox pairing required. If you’ve been frustrated that your SMA inverter only talks to an SMA charger, or your Huawei system only plays nice with Huawei hardware, this is the interoperability layer that’s been missing.


**Dynamic tariff optimisation.** Integrates with variable electricity tariffs (Tibber, Awattar, Octopus, and others) and shifts charging to the cheapest hours automatically.


**Dynamic load management.** Balances charging with your household consumption to stay within your grid connection limits — no electrician upgrade needed just because you added an EV.


**Android Automotive OS.** Start and manage charging from the car’s infotainment system. No phone needed.


**Home Assistant integration** with a custom dashboard card, available now via a HACS repository. No YAML hacking required.


**Company car and fleet reimbursement.** kWh-accurate session tracking for tax-compliant home charging reimbursement. This is the feature that makes MagicBox interesting for fleet operators — deploy it to employees’ homes and you get standardised charging data from every garage, regardless of what wallbox each employee has, without requiring any of them to have reliable Wi-Fi.


## How It Compares to EVCC


If you know[EVCC](https://evcc.io/) , MagicBox solves a similar set of problems — PV surplus charging, load management, smart tariff integration — but the architecture is fundamentally different.


EVCC runs as software on a server on your network and talks to your wallbox over Wi-Fi or LAN via the charger’s API. That works well, but it requires the charger to be on your network, the server to be running, and the network path between them to be reliable.


MagicBox moves the OCPP server onto the hardware device itself. The intelligence runs on the box, right next to the charger. It can connect to your home network over Wi-Fi or Ethernet when available, but it can also operate completely disconnected from it — bridging back over long-range 868 MHz networking when Wi-Fi isn’t viable. The charger never stops being smart just because your router rebooted or your garage is out of Wi-Fi range.


## On Matter and Standards


We’re building Matter support as the universal, standards-based interface to consumer smart home ecosystems — it’s the right long-term path and we’re committed to it. But shipping a product means shipping today, not when every platform finishes implementing the spec. So our bridge is the Home Assistant integration, already live via HACS, with Matter as the next step as the ecosystem matures.


We’d rather give people something that works now and migrate to the standard when it’s ready than wait and ship nothing.


## Launching Today


MagicBox launches in partnership with Easee across DACH (Germany, Austria, Switzerland), with day-one access to their install base of over a million chargers in Europe. We’re an 11-person team in Munich, backed by BMW i Ventures, Statkraft Ventures, and Y Combinator, with a recent[EIC Accelerator grant](https://www.heycharge.com/news/heycharge-wins-eic-accelerator-grant) (9/9 evaluation score).


Whether you’re an EV owner who wants smarter charging, or a fleet manager who needs to deploy managed charging to hundreds of employees’ garages, MagicBox brings the same platform that runs reliably in the hardest RF environments in Europe to the charger already on your wall.


## Related guides


- [EV charging without internet: a complete guide](https://www.heycharge.com/guides/ev-charging-without-internet/)
- [EV charging load management without a grid upgrade](https://www.heycharge.com/guides/ev-charging-load-management/)
- [How to cut EV charging infrastructure costs](https://www.heycharge.com/guides/ev-charging-infrastructure-cost/)
