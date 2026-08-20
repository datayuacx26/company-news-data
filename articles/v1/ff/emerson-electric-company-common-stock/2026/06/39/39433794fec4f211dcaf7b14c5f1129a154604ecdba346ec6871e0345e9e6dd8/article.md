---
schema_version: "1.0.0"
document_id: "39433794fec4f211dcaf7b14c5f1129a154604ecdba346ec6871e0345e9e6dd8"
company_key: "emerson-electric-company-common-stock"
company: "Emerson Electric Company"
source_id: "emerson-electric-company-common-stock-rss-55c4887b1417"
canonical_url: "https://www.emersonautomationexperts.com/2026/valves-actuators-regulators/understanding-valve-operations-heat-recovery-steam-generators/"
published_at: "2026-06-26T13:00:41+00:00"
first_seen_at: "2026-07-25T01:09:56.979097+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:aa789318830a37a22a122ea5faf7cdc56be62c98b5e0fddd05d6dab238d54112"
---

# Understanding Valve Operations in Heat Recovery Steam Generators

Every valve in a Heat Recovery Steam Generator (HRSG) serves a specific purpose at a specific phase of plant operation, from cold startup through normal load to shutdown.[Selecting the right valve](https://www.youtube.com/watch?v=myuu9DDq2V0) for each service and understanding the conditions it will face is essential to protecting equipment and maintaining reliable power generation.


### Why It Matters


HRSG valve applications span an unusually wide range of operating conditions. During startup, a feedwater valve may see the full discharge pressure of the boiler feed pump against an empty, atmospheric-pressure drum, creating severe cavitation potential. As the plant transitions to normal operation, that same flow path must deliver high feedwater rates at much lower differential pressures, demanding a different set of valve characteristics entirely.


### Key Takeaways


- **Cavitation is the defining challenge at startup.** The feedwater startup valve must be specifically designed to eliminate cavitation damage caused by the high differential pressure between pump discharge (2,000 to 3,500 psig) and an empty drum at atmospheric pressure.
- **Two-valve and single-valve feedwater configurations each have trade-offs.** A two-valve system separates startup and normal operating duties. At the same time, a single-valve retrofit uses a specially characterized cage with anti-cavitation trim at lower travel and larger flow passages at higher travel.
- **Tight shutoff in spray water and block valves protects steam-side equipment.** Leaking spray water into a steam line when cooling is not called for can cause thermal shock or push the operation below the temperature setpoint, reducing efficiency.


- **Stop-check valves and main steam stop valves are boiler code requirements, not optional accessories.** When two or more boilers share a common header, these valves prevent dangerous backflow and ensure bi-directional isolation.


### From Cold Iron to Full Load: How Feedwater Valves Manage the Transition


Before a plant starts, the HRSG is empty. The low-pressure (LP) drum is filled first by the condensate system because it provides suction flow to the boiler feed pump. All drain valves on the economizers and evaporators are closed, and vent valves are opened to purge air from the system. The boiler feed pump is then started with its recirculation valve fully open and the downstream feedwater regulating valves closed. A minimum flow rate, determined by the pump manufacturer, is maintained through the recirculation valve to keep the pump cool. Feed pump suction pressure typically ranges from 60 to 100 psig, while discharge pressure ranges from 2,000 to 3,500 psig. The recirculation valve sees a cavitating pressure drop across its entire operating range.


Once the pump stabilizes, the high-pressure (HP) drum is filled using the feedwater startup valve, which is sized to fill the drum in the time the plant designer specifies. Because the drum starts at atmospheric pressure, the full pump discharge pressure creates a high differential that can cause cavitation, so the startup valve must be engineered to handle it. After the proper drum level is reached, vents are closed, combustion turbines are started, and pressure and temperature begin to build. As the load increases, the startup valve continues to open to maintain the HP drum level. If total feedwater flow remains below 30 percent of pump capacity, the recirculation valve closes proportionally to maintain the required minimum flow through the pump, calculated as the sum of recirculation flow and feedwater flow.


As HP drum pressure rises toward operating pressure, differential pressure across the valve drops to a point where cavitation is no longer a factor. An ideally sized regulating valve system will have the main feedwater regulator valve’s flow coefficient (Cv) set to about 20 percent open, equivalent to the startup valve’s Cv at 80 percent open. As a rule of thumb, the feedwater valve should have an equal percentage trim installed to offset the feed pump’s performance characteristic curve. Once feedwater flow reaches or exceeds 30 percent of the maximum continuous rating, the recirculation valve closes fully, and the plant operates on the main feedwater valve alone.


### Single-Valve Retrofits: Combining Startup and Normal Service


A popular alternative to the two-valve arrangement, particularly for retrofitting existing plants, is a single valve with a specially characterized cage. The lower portion of the cage uses anti-cavitation trim to safely handle startup and low-load conditions. The upper portion uses larger drilled holes or windows to deliver the higher flow rates needed during normal operation.


When a single valve is used, the control system must monitor the differential pressure (DP) between the pump discharge and the drum. An interlock should prevent the valve from opening past the anti-cavitation portion of the trim until a non-cavitating DP is reached. As drum pressure rises, additional valve stages allow increased water flow. Simultaneously, as DP across the valve drops, the valve position increases as permitted by drum pressure. This staged approach prevents cavitation throughout valve operation.


### Beyond Feedwater: Valves That Protect the Steam Path and Enable Safe Shutdown


Several other valve types are critical to HRSG operation. Safety valves, required by boiler code, exist solely to prevent any system from exceeding its preset design pressure. Continuous blowdown valves remove impurities such as rust, scale, and excess chemicals that concentrate in drum water as steam is produced. This is a harsh service because the fluid flashes to steam between the drum and the flash tank.


Steam temperature is controlled by an attemperating system consisting of a desuperheater, a spray water valve, a spray water block valve, and a strainer. The desuperheater can be a ring or insertion style, depending on water flow requirements. Because the spring-loaded nozzles in the desuperheater have relatively small passages, a strainer sized for those nozzles should be installed in the spray water line to prevent plugging. Tight shutoffs on both the spray water and block valves are essential to prevent water from leaking into the steam line when spray is not called for, which could cause thermal shock or efficiency losses.


Start-up vent valves, sometimes called sky vents, regulate the temperature of the HRSG steam system during warm-up. Stop-check valves prevent higher-pressure operating units from backflowing into an offline or tripped HRSG. A stop-check combines isolation and check-valve functions in a single body: its stem is disconnected from the plug, so in forward flow the plug remains open and in reverse flow it slams shut. While required by boiler code when two or more boilers share a common header, a stop-check should not be relied upon as the primary shutoff. That role belongs to the main steam stop valve, which provides a tight, bi-directional shutoff to the main steam line.


During shutdown, steam production is gradually reduced, and feedwater flow decreases to maintain drum level. When feedwater drops below 30 percent, the recirculation valve reopens to maintain cooling flow through the pump. Once the pump is secured, the drums are bottled up for the next start, or, if an outage is planned, vents and drains are opened to remove all water from the HRSG.


**Ready to dig deeper into HRSG valve selection for your plant?** Explore Emerson’s[Heat Recovery Steam Generator](https://www.emerson.com/en/corporate/industries/power/natural-gas-power/heat-recovery-steam-generator) solutions.
