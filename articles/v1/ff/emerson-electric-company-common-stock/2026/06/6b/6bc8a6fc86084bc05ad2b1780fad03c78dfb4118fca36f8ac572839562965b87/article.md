---
schema_version: "1.0.0"
document_id: "6bc8a6fc86084bc05ad2b1780fad03c78dfb4118fca36f8ac572839562965b87"
company_key: "emerson-electric-company-common-stock"
company: "Emerson Electric Company"
source_id: "emerson-electric-company-common-stock-rss-55c4887b1417"
canonical_url: "https://www.emersonautomationexperts.com/2026/measurement-instrumentation/analytical/modernizing-cold-dry-emissions-monitoring-chemical-refining-operations/"
published_at: "2026-06-10T13:00:17+00:00"
first_seen_at: "2026-07-25T01:09:56.979097+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:3cc74da6bd71ca37a692d5967e196e4fbaac46bed4e384daebadcc9df57bd857"
---

# Modernizing Cold/Dry Emissions Monitoring in Chemical and Refining Operations

Continuous Emissions Monitoring Systems (CEMS) remain a regulatory necessity across chemical and refining operations, yet the technology inside the cabinet keeps evolving. A recent ControlGlobal.com webinar, hosted by Editor-in-Chief[Len Vermillion](https://www.controlglobal.com/home/contact/11414861/len-vermillion) and titled[Modernizing Cold/Dry Emissions Monitoring in Chemical & Refining Operations](https://www.controlglobal.com/webinars/webinar/55364439/modernizing-cold-dry-emissions-monitoring-in-chemical-refining-operations) , examined how a newly launched analyzer and a disciplined sample-system approach can modernize Cold/Dry CEMS without sacrificing reliability or budget.


Presented by[Dr. Beth Livingstone](https://www.linkedin.com/in/beth-livingstone-252280293/) , Global Product Manager for the Quantum Cascade Laser Process Gas business at Emerson, and[Keith Linsley](https://www.linkedin.com/in/keith-n-linsley-193ab138/) , Senior Global Product Manager for CEMS and Process Analyzer Systems at Emerson, the session covered both the analyzer itself and the sample handling system that determines whether compliance data is trustworthy.


### Why It Matters


CEMS reporting is mandatory and enforced by local authorities, with fines or penalties when reporting is interrupted. Plants must balance upfront and lifetime cost against regulations that vary by region, emitter type, and gas list. Reliability and serviceability carry as much weight as raw measurement accuracy, because any unplanned downtime is a compliance risk. A modern Cold/Dry CEMS should be configurable, easy to integrate into brownfield or greenfield sites, and capable of multiple measurements within a single analyzer.


### Key Takeaways


-


[Rosemount QX1000 Continuous Gas Analyzer](https://www.emerson.com/en-us/catalog/rosemount-sku-qx1000-continuous-gas-analyzer)


The[Rosemount QX1000 Continuous Gas Analyzer](https://www.emerson.com/en/measurement-instrumentation/products/rosemount-qx1000-continuous-gas-analyzer) is purpose-built for Cold/Dry CEMS and general-purpose applications, typically supporting one to four measurement channels.


- It uses Quantum Cascade Laser (QCL) spectroscopy with a patented pulsed “chirp” method to measure gases.
- The platform is modular: the QCL bench, the O2 bench, the optional front display, and the Input/Output (IO) cards can be included or omitted based on need.
- A new web-based Graphical User Interface (GUI) supports remote setup, monitoring, calibration management, and troubleshooting, with role-based user access.
- Some presented customer examples include a European gas-turbine strategic reserve facility and a United Kingdom waste-to-energy plant.
- Emerson’s analyzers in this portfolio meet 40 Code of Federal Regulations (CFR) Part 60, Performance Specifications 2 and 3.


### The Drivers Behind a Modern Cold/Dry CEMS


Beth opened with the recurring pain points she sees across emitters: mandatory regulatory requirements, minimal downtime, varying gas measurement needs across emitter types, seamless integration with existing or new infrastructure, and rising demand for multi-measurement analyzers. Because penalties can follow any reporting break, downtime is a compliance risk, not just an operational one. Optimizing upfront purchase costs and lifetime costs while still meeting evolving regulations is the central balancing act for end users.


### Inside the Rosemount QX1000


The QX1000 is a 19-inch rack-mounted analyzer built on a modular and configurable platform. Its QCL bench is a self-contained module that can hold up to three lasers feeding a single detector. Each laser is pulsed in sequence; as the laser heats during its roughly 10-microsecond chirp, the emitted wavelength scans across the mid-infrared region where target gases absorb strongly. Pulsing yields a low duty cycle, low power consumption, long laser life, and a fast update rate suitable for signal averaging.


Concentration is derived directly from the[Beer-Lambert law](https://en.wikipedia.org/wiki/Beer%E2%80%93Lambert_law) , an absolute measurement grounded in the unchanging spectroscopic properties of each gas. That foundation provides long-term stability, immunity to cross-interference, and reduced calibration burden, with no need for complex chemometrics or database lookups. A multipass mirror arrangement inside the gas cell extends the optical path length to improve signal-to-noise without enlarging the analyzer footprint, and multiple cell configurations allow the user to purchase an analyzer tailored to their measurement sensitivity needs.


For oxygen, a separate paramagnetic bench exploits the paramagnetic susceptibility of O2, a unique physical property that distinguishes it from most other gases, and where it has unpaired electrons that cause it to be strongly attracted to magnetic fields. In the paramagnetic bench chamber, a small glass dumbbell is suspended in an inhomogeneous magnetic field. As sample gas enters the chamber, O2 is attracted to the strongest region of the magnetic field, causing the dumbbell to rotate. A restoring current is required to return the dumbbell to its position, and this current is proportional to the oxygen concentration, over a range of 0 to 25 per cent. Combined, the two benches deliver up to a four-channel system, and certain configurations can reach five when two gases share a single laser.


### Operation, Communication, and Field Use


The QX1000 is available with or without a front display. Day-to-day status is visible through LED indicators and, when fitted, a push-button display for alerts, calibrations, validations, and general health checks. IO options include 4- to 20-mA outputs, Modbus, and configurable relays for status alarms and valve control during calibration or validation. The new web-based remote interface supports initial factory setup, live measurement data, scheduled validations, service functions, historical alerts and logs, and configuration changes from a browser, with permissions tailored to each user role.


Beth shared two customer examples. A European facility serving as a strategic winter reserve power source selected the QX1000 for full multi-gas monitoring on its gas turbine, replacing aging technology with a multi-measurement system. A United Kingdom energy-from-waste plant adopted a two-channel configuration measuring oxygen and carbon monoxide for combustion control, again to retire obsolete instruments while keeping costs in check.


### The System Around the Analyzer


Keith focused on what surrounds the analyzer, noting that the sample system is usually the weak link when results look wrong. In a Cold/Dry design, the sample is heated above 250 degrees Fahrenheit via the heated sample line, then conditioned to roughly 39 degrees Fahrenheit in a dual-impinger chiller before reaching the analyzer on a dry basis. A peristaltic pump removes condensate, and a closed-contact moisture sensor downstream of the pump shuts the system down if the chiller fails, protecting the analyzer’s mirrors.


Keith recommended dual-pump designs to keep flow and pressure constant during sample and calibration phases, and a “smart system, dumb analyzer” philosophy that lets the Programmable Logic Controller (PLC) and Compliance Data Acquisition System (DAHS), such as Emerson’s[CEMS Data Acquisition Solution](https://www.emerson.com/en/measurement-instrumentation/catalog/engineered-systems-and-solutions/continuous-emissions-monitoring-systems) , which includes gas analyzer, PLC, I/O, Industrial PC, and Software, handle sequencing and calibration while the analyzer focuses on measurement. Design choices for probe materials, filter sizing, blowback frequency, sample line temperature, and dew point must reflect the specific process, whether a Fluid Catalytic Cracking (FCC) unit, a process heater, a simple-cycle gas turbine, or a Heat Recovery Steam Generator (HRSG). His rule of thumb on dew point: design for upsets and outages, not ideal conditions, and stay well above the limit that keeps the sample in gas phase.


He also noted that the United States Environmental Protection Agency (EPA) and Emerson favor extractive CEMS over in situ measurements because extractive designs allow true zero, calibration, and linearity checks “without relying on dynamic spiking or bump testing”. Emerson’s Engineered Solutions group integrates the analyzer, sample system, data acquisition, and supporting instrumentation into a turnkey package with a single accountability partner, covering both 40 CFR Part 60 and Part 75 requirements.


### Next Step


For specifications, configurations, and ordering details on the analyzer discussed above, visit the[Rosemount QX1000 Continuous Gas Analyzer](https://www.emerson.com/en/measurement-instrumentation/products/rosemount-qx1000-continuous-gas-analyzer) product page.
