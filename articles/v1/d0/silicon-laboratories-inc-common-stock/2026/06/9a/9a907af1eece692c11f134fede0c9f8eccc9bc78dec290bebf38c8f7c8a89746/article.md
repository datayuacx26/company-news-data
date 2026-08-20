---
schema_version: "1.0.0"
document_id: "9a907af1eece692c11f134fede0c9f8eccc9bc78dec290bebf38c8f7c8a89746"
company_key: "silicon-laboratories-inc-common-stock"
company: "Silicon Laboratories Inc."
source_id: "silicon-laboratories-inc-common-stock-news-import-7b63a781b7d1"
canonical_url: "https://www.silabs.com/blog/matter-1-6-spec-smarter-thermostat-control-and-more"
published_at: "2026-06-24T15:52:44.572+00:00"
first_seen_at: "2026-07-27T21:12:16.622200+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:932280f9abee42dd917a1ecae9e397495ac55696cff2d438bf1c126ffcffbb3a"
---

# Matter 1.6 Spec: Smarter Thermostat Control and More - Silicon Labs

The Connectivity Standards Alliance (CSA) has released Matter 1.6, marking another important step in the evolution of interoperable[smart home](https://www.silabs.com/applications/smart-home) experiences. While previous releases expanded Matter’s device ecosystem, this core specification release focuses on making connected devices more intelligent, context-aware, and capable of delivering a more seamless user experience across ecosystems.


Two of the most significant additions in this[Matter](https://www.silabs.com/wireless/matter) release are **Thermostat Suggestions** and a collection of core enhancements that improve device communication, safety visibility, and ecosystem security.


##
Matter 1.6: Smarter Thermostat Control Through Coordinated Automation


Thermostat Suggestions is a standout feature of Matter 1.6, and changes how smart thermostats interact with ecosystems and automation services. Traditionally, smart home platforms send direct commands to thermostats, which typically execute those instructions without considering broader user preferences or recent user actions. Matter 1.6 introduces a more sophisticated approach by enabling ecosystems to send recommendations instead of direct commands. These suggestions are tied to supported thermostat presets and are evaluated by the thermostat before any action is taken.


This feature allows thermostats to make more informed decisions based on factors such as user preferences, current operating conditions, and recent interactions. For example, imagine a homeowner uses a smart thermostat connected to multiple ecosystems, such as Google Home, Apple Home, and a utility company's energy-saving program. During a peak energy event, the utility may recommend an energy-saving temperature setting while a home automation routine may attempt to increase cooling when the homeowner arrives home. If the homeowner has recently adjusted the thermostat manually, Matter 1.6 allows the thermostat to recognize that preference and determine which recommendation best aligns with the user's intent. Rather than automatically following the latest command, the thermostat can intelligently evaluate competing requests and choose the most appropriate action.


Similarly, users who prioritize energy efficiency, humidity control, or indoor air quality can have those preferences consistently respected across multiple connected services.


An equally important aspect of Thermostat Suggestions is transparency. When a thermostat chooses not to follow a recommendation, it can provide a standardized explanation that helps both users and ecosystems understand the reasoning behind the decision. This creates a smarter and more predictable comfort experience, enabling thermostats to act as intelligent decision-makers rather than simply executing every incoming command.


##
Matter 1.6 Brings Core Enhancements to Security, Reliability, and User Experience


Beyond thermostat intelligence, Matter 1.6 introduces several core enhancements that strengthen device interoperability and ecosystem reliability. Devices can now communicate their capabilities and operational limits in a standardized manner, allowing controllers and applications to represent device functionality more accurately. This improvement helps ensure that users receive a consistent experience regardless of which ecosystem or platform they choose to use.


Security-focused devices also benefit from enhanced interoperability. Matter 1.6 makes it possible for security sensors to indicate whether they provide access to historical event information, making it easier for ecosystems to build comprehensive security experiences that incorporate both current status and past activity. This capability helps users gain better visibility into sensor behavior and security-related events over time.


Safety devices receive important updates as well. Smoke and carbon monoxide alarms can now report when they have been removed from their installed location. This unmounted-state awareness gives users and ecosystems a more accurate understanding of whether a safety device is actively protecting the home, helping prevent situations where a disconnected alarm may be mistakenly assumed to be operational.


The release also strengthens the Matter security infrastructure through improvements to Certificate Revocation List (CRL) management. Matter 1.6 introduces partitioned CRLs, allowing revocation information to be managed in smaller, independently updated segments rather than as a single large dataset. This approach improves scalability and efficiency as the number of Matter-certified devices continues to grow, ensuring that security mechanisms can evolve alongside the expanding ecosystem.


##
Matter 1.6 Updates Improve User-Centric Smart Homes


Together, Thermostat Suggestions and the new core enhancements demonstrate Matter’s continued focus on delivering intelligent, user-centric experiences while strengthening interoperability, security, and device transparency. As device manufacturers and ecosystem providers begin adopting Matter 1.6, these capabilities will help create smarter homes that better understand user intent, communicate more effectively across platforms, and maintain the reliability and trust that consumers expect from connected devices.
