---
schema_version: "1.0.0"
document_id: "a637daf1e72ffad947f4c7a6dbc249bb0d445e7141ec0b9365063d149dcf6e3f"
company_key: "silicon-laboratories-inc-common-stock"
company: "Silicon Laboratories Inc."
source_id: "silicon-laboratories-inc-common-stock-news-import-7b63a781b7d1"
canonical_url: "https://www.silabs.com/blog/amazon-sidewalk-location-services-is-redefining-iot"
published_at: "2026-01-05T13:14:21.831+00:00"
first_seen_at: "2026-07-26T00:14:15.933714+00:00"
fetched_at: "2026-07-28T22:24:12.939989+00:00"
content_hash: "sha256:cf02cd315e50582b1c11b23ef0823dc4de62d2d50c793d26bc18e1a1be92b455"
---

# Amazon Sidewalk Location Services is Redefining IoT - Silicon Labs

The way IoT devices understand and interact with their surroundings is evolving. From[asset trackers](https://www.silabs.com/applications/industrial-iot/asset-tracking) to[environmental sensors](https://www.silabs.com/sensors) , connected products increasingly need to know where they are. But GPS alone can’t meet the power or cost constraints of large-scale deployments.


##
What is Amazon Sidewalk Location Services?


With the introduction of[Amazon Sidewalk Location Services](https://aws.amazon.com/blogs/aws/introducing-aws-iot-core-device-location-integration-with-amazon-sidewalk/) and its integration into AWS IoT Core Device Location, developers can now access scalable, low-power, cloud-managed location resolution across a nationwide community network. For device makers, this represents a major step forward in reliable and efficient location awareness without the complexity or cost of traditional GPS systems.


##
Amazon Sidewalk Location Services Offers Location Without the Heavy Lifting


Accurate location data has traditionally required specialized hardware or energy-intensive GNSS solutions. The new location library, available in[Amazon Sidewalk SDK 1.19.1](https://community.silabs.com/s/share/a5UVm000000xgjBMAQ/silicon-labs-sidewalk-extension-v260-supporting-amazon-sidewalk-sdk-119?language=en_US) and later, changes that model by providing a unified API that determines device position using existing connectivity within the[Amazon Sidewalk](https://www.silabs.com/wireless/amazon-sidewalk) network.


The Location Library automatically selects the most efficient method based on the device’s hardware and current network conditions:


- **Amazon Sidewalk Network Location** : Uses Bluetooth Low Energy (LE) connections to determine device proximity within the Sidewalk network, consuming virtually no additional power.
- **Wi-Fi Scanning** : Detects nearby access points and sends identifiers to the cloud for location resolution.
- **GNSS Scanning** : Collects satellite data to calculate precise coordinates when higher accuracy is required.


This tiered approach allows devices to dynamically balance accuracy and energy use, maintaining responsiveness while extending battery life.


##
Built for Cloud-Native Locationing


Through integration with AWS IoT Core Device Location, Sidewalk-enabled devices can securely send Bluetooth LE, Wi-Fi, or GNSS scan data to the cloud for coordinate resolution. Developers can then visualize device locations, apply geofencing rules, and integrate mapping or analytics services using AWS.


By handling the location computation in the cloud, the system keeps endpoint devices lightweight and efficient, delivering accurate, real-time positioning without increasing hardware complexity or cost.


##
Designed for Power Efficiency


Sidewalk was designed with power optimization in mind, ensuring long-lasting performance for battery-operated IoT devices deployed across mixed environments. With the Amazon Sidewalk Power Optimization framework, endpoints can intelligently manage connection profiles and transmit power across Bluetooth LE, FSK, and LoRa (CSS) links, extending operational lifetime by months or even years in typical scenarios.


Together, these capabilities enable low-maintenance deployment of location-aware devices across residential, commercial, and municipal spaces.


##
Amazon Sidewalk Location Services Opens New Possibilities for Developers


The introduction of Amazon Sidewalk Location Services opens new opportunities for product innovation across a wide range of use cases. **Asset tracking** becomes more reliable, allowing tools, packages, and equipment to be monitored with coverage that reaches far beyond standard Wi-Fi networks. **Smart infrastructure** can take advantage of location-based automation for parks, campuses, and public utilities operating within Sidewalk’s network footprint. **Environmental and safety monitoring** also improves as sensor data can be paired with precise geographic context, strengthening both reporting and response.


Developers can easily integrate location capabilities using familiar APIs such as sid_location_init(), sid_location_run(), and sid_location_set_max_mode(). These functions give fine control over effort levels and power management, allowing each device to optimize performance for its specific use case.


##
What Hardware Can Deploy Ready for Amazon Sidewalk-based Location Intelligence?


Silicon Labs offers a full range of development hardware and software to help bring Sidewalk-based location applications to market quickly:


- **[EFR32xG28 SoCs](https://www.silabs.com/wireless/amazon-sidewalk/efr32sg28-dual-band-wireless-socs)** provide highly integrated support for **[Bluetooth Low Energy](https://www.silabs.com/wireless/bluetooth)** and **FSK Sub-GHz** , ideal for BLE location tracking and gateway connectivity within the Sidewalk network.
- The **[KG100S-PK6130A Pro Kit](https://www.silabs.com/development-tools/wireless/proprietary/pro-kit-for-amazon-sidewalk)** enables development across all Sidewalk protocols: Bluetooth Low Energy, FSK, and CSS, with Bluetooth LE and CSS both supporting location capabilities through the Amazon Sidewalk Location Library.
- The latest Silicon Labs Amazon Sidewalk Extension v2.6.1, supporting[Amazon Sidewalk SDK 1.19.1](https://docs.sidewalk.amazon/mobile-sdk/sidewalk-mobile-sdk-release-notes.html) , includes the new Location Library integration, updated APIs, and enhanced power management features to make implementation seamless.


Together, these solutions allow developers to prototype, evaluate, and deploy location-enabled IoT products that operate reliably across the broad coverage of the Amazon Sidewalk network.


##
A Connected Foundation for Smarter Devices


Amazon Sidewalk’s new location capabilities extend the platform’s value beyond connectivity into true context awareness. By combining long-range, low-power networking with intelligent cloud-based positioning, Amazon Sidewalk delivers a foundation for smarter, more efficient IoT products.


To download the SDK, view sample projects, or get started developing, check out the[Amazon Sidewalk Extension v2.6.1 release notes.](https://docs.silabs.com/sisdk-release-notes/latest/sisdk-amz-sidewalk-release-notes/)
