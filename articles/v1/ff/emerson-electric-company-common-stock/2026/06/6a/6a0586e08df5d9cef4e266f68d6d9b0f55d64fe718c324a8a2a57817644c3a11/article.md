---
schema_version: "1.0.0"
document_id: "6a0586e08df5d9cef4e266f68d6d9b0f55d64fe718c324a8a2a57817644c3a11"
company_key: "emerson-electric-company-common-stock"
company: "Emerson Electric Company"
source_id: "emerson-electric-company-common-stock-rss-55c4887b1417"
canonical_url: "https://www.emersonautomationexperts.com/2026/measurement-instrumentation/tank-gauging/how-2-in-1-radar-technology-fulfills-requirement-for-independent-tank-gauging-and-safety-systems-on-liquefied-gas-carriers/"
published_at: "2026-06-03T11:00:03+00:00"
first_seen_at: "2026-07-25T01:09:56.979097+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:39569f599e39e10c15b2d23ce08141b0fede6288feb681a03eb9a73f72cd42a6"
---

# How ‘2-in-1’ Radar Technology Fulfills Requirement for Independent Tank Gauging and Safety Systems on Liquefied Gas Carriers

Vessels transporting liquefied gases at sea incorporate specialized storage tanks, which must be equipped with both a tank gauging system and a safety system. The tank gauging system provides continuous measurement of liquid levels for inventory management and custody transfer purposes, while the safety system includes alarms to help prevent overfills. These systems must be designed and implemented in compliance with the International Maritime Organization’s (IMO) International Code of the Construction and Equipment of Ships Carrying Liquefied Gases in Bulk (IGC Code).


Historically, gas carriers have maintained a clear separation between tank gauging and safety instrumentation. This preserves a critical layer of protection by ensuring that a failure in one system does not compromise the other. However, perceived ambiguity within the wording of the IGC Code has led to the emergence of some solutions where safety functionality is integrated within the tank gauging system. This shift has largely been driven by efforts to reduce installation complexity and costs but it also introduces significant safety risks.


Tank gauging systems and safety systems on liquefied gas carriers must be designed and implemented in compliance with the IMO’s IGC Code.


### **What the IGC Code Currently States**


The current version of the IGC Code, adopted in 2016, stipulates that every cargo tank on board a liquefied gas carrier must be equipped with at least one level indicator, one high-level alarm and one overfill alarm. It also specifies that the high-level alarm must operate independently of the level indicator, and that the overfill alarm must be independent of the high-level alarm. However, it does not explicitly state that the overfill alarm must also be independent of the level measurement system. Some stakeholders have interpreted this omission as a justification for integrating overfill alarm functionality within the tank gauging system.


### **SIGTTO Recommendations**


The Society of International Gas Tanker and Terminal Operators (SIGTTO) represents a broad range of gas tanker and terminal operators, and one of its primary objectives is to promote safety and operational reliability across the industry. SIGTTO holds consultative status with the IMO and has observed that integrating safety-related functionality within tank gauging systems represents a misinterpretation of the intent of the IGC Code. SIGTTO’s 2021 publication, ‘ESD Systems – Recommendations for Emergency Shutdown and Related Safety Systems’, states that to minimize risk, gas carriers should not be allowed to employ solutions where safety functionality is integrated into the tank gauging system.


The Society of International Gas Tanker and Terminal Operators (SIGTTO) has stressed the need for clear independence between safety functionality and the tank gauging system on gas carriers.


### **IGC Code Clarification**


In response to concerns about the current wording of the IGC Code and the potential for misinterpretation, the IMO is expected to introduce a clarification. The forthcoming revision, scheduled to be published in 2028 and applied to vessels constructed on or after July 1, 2028, will explicitly require that high-level and overfill alarms are independent not only of each other but also of any level measurement devices. By making this requirement explicit, the IMO aims to eliminate ambiguity and ensure consistent application across the industry. The clarification effectively closes the door on integrated solutions that combine measurement and safety functions.


### **Technology Considerations**


The choice of level measurement technology plays a crucial role in achieving compliance with the IGC Code while maintaining operational efficiency. Traditional float-based devices, while simple and relatively inexpensive, present several limitations in cryogenic environments. Their reliance on mechanical components makes them susceptible to wear, sticking or failure, particularly under extreme temperature conditions. Maintenance can be complex and often requires intrusive procedures, including gas-freeing the tank, which introduces additional operational challenges and safety risks. Proof-testing, an essential requirement for safety systems, can also be time-consuming and labor-intensive when it involves manual manipulation of mechanical components.


Because of these issues and challenges, radar-based measurement technology has become the preferred solution for modern gas carriers. By using microwave signals to determine liquid levels, radar gauges eliminate the need for moving parts within the tank, significantly improving reliability. They are capable of delivering high accuracy even in challenging conditions and can be proof-tested remotely from the control room, allowing operators to verify functionality without direct interaction with the tank.


### **Designing Compliant System Architectures**


The[Rosemount TGU 53 Tank Radar Gauge](https://www.emerson.com/en/measurement-instrumentation/products/rosemount-tgu-53-tank-radar-gauge) is a ‘2-in-1’ device that can be used to provide level measurement and independent high-level and overfill alarms, in compliance with the IGC Code.


When it comes to system architectures, achieving full independence can be approached in different ways. One option is to install four separate radar devices per tank, each dedicated to a specific function: primary level measurement, secondary level measurement for redundancy, high-level alarm and overfill alarm. This approach provides a straightforward path to compliance, as each function is clearly segregated. However, it also increases system complexity, installation effort and cost. For example, each device generally requires its own still pipe to isolate the instrument from turbulence, foam or obstructions that could weaken the signal and cause inaccurate readings. On large gas carriers still pipes can extend more than 50 meters in length, adding to both structural complexity and material expense.


An alternative approach is to implement a solution based on ‘2-in-1’ radar technology. In this innovative concept, dual radar level gauges – such as Emerson’s[Rosemount™ TGU 53](https://www.emerson.com/en/measurement-instrumentation/products/rosemount-tgu-53-tank-radar-gauge) and OFC Tank Radar Gauges – can be used for primary and secondary level measurement, while a second dual unit provides independent high-level and overfill alarms. Although the units share a physical housing, their measurement functions and internal electronics remain completely independent, ensuring compliance with the IGC Code’s upcoming requirement for separation. This approach reduces the number of required installations and requires only two still pipes – one for measurement and one for safety – thereby simplifying design and reducing costs.


To learn more, read the Emerson white paper ‘[Separation for Increased Safety: Independent Level and Overflow Control Compliant with the Forthcoming IGC Code](https://www.emerson.com/is/content/emerson/en/measurement-instrumentation/marketing/products/marine/documents/dl-rmt-MS-00802-0200-2828.pdf) ’.
