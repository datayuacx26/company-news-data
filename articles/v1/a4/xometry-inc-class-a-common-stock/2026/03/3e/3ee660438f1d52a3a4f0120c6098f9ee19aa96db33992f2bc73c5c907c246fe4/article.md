---
schema_version: "1.0.0"
document_id: "3ee660438f1d52a3a4f0120c6098f9ee19aa96db33992f2bc73c5c907c246fe4"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/machining/cnc-m-codes/"
published_at: "2026-03-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:17:07.377837+00:00"
content_hash: "sha256:f0e9f524b1453583ad4c4077eaf3c3eccaa56253a3cb038d0429ff67cae6f84f"
---

# CNC M-Codes List

### What Machine Operations Are Controlled by Frequently Used M-Codes?


Frequently used M-codes control the machine operations that run throughout any standard machining cycle. Spindle rotation is one of the first operations activated in a program, with M03 starting clockwise rotation before the tool makes contact with the workpiece. Coolant flow begins with M08, which opens the flood coolant valve to protect the cutting tool and remove chips from the cutting zone. Tool changes rely on M06 to trigger the mechanical swap. Safe practice requires stopping the spindle (M05) and moving to a tool-change reference point (G28 or G30) before M06 is executed. Program control depends on M30 to end the cycle and reset the controller for the next run. Stopping the spindle before a tool change or at the end of the program falls to M05, which brings rotation to a complete halt. Each of the aforementioned operations runs at a specific point in the program sequence, and placing M-codes correctly ensures the CNC machining cycle executes without errors.


#### Do CNC Machining Programs Commonly Include Spindle and Coolant Control M-Codes?


Yes, CNC machining programs commonly include spindle and coolant control M-codes. The spindle and coolant ensure the tool operates efficiently and remains properly cooled in almost any cutting cycle. The machining process would lack critical functions (spindle activation and coolant flow) without the aforementioned commands, leading to potential damage to the tool and the workpiece. M03 activates the spindle, ensuring it starts rotating in the correct direction for cutting, while M08 initiates the coolant flow to protect the tool and workpiece from excessive heat. The aforementioned commands are crucial to maintain cutting efficiency, tool longevity, and a clean work area. M05 stops the spindle, and M09 shuts off the coolant at the end of the cutting cycle, preparing the machine for tool changes or program completion. Skipping the aforementioned spindle and coolant M-codes can lead to issues (tool overheating, poor surface finish, or premature wear on machine components). The aforementioned M-codes are vital for ensuring smooth and safe operations within a CNC machining process, safeguarding the tools and the machine's integrity. The aforementioned M-codes are foundational elements in the entire properly structured CNC machining programs.


## Why Do M-Code Lists Differ Between CNC Controllers?


The M-code lists differ between CNC controllers because each manufacturer develops its own command set to match the specific hardware and features of its control system. Fanuc, Siemens, Haas, and Mitsubishi each assign M-code numbers according to internal standards that reflect their machine architectures. While many M-codes are standardized (M03 or M30), others are manufacturer-specific; for example, a code like M12 might control a parts catcher on one machine but a chip conveyor on another. Manufacturers add custom M-codes to support pallet changers, probing systems, rotary axis controls, and other machine-specific features that standard lists do not cover. The absence of a universal M-code standard across the industry means that a program written for one controller requires review and modification before running on a different system. Machine tool builders also modify default M-code assignments during the integration process, adding another layer of variation across the installations. Operators and programmers must consult the specific CNC controller documentation before writing or editing programs to confirm the correct function of each M-code on that machine.


### How Do Control Systems From Different Manufacturers Define M-Code Functions?


The control systems from different manufacturers define M-code functions by assigning command numbers to machine actions through their internal programming architecture. Fanuc controllers follow one set of assignments, while Haas systems use a modified version of that list with additional proprietary codes added for machine-specific functions. Siemens controllers assign M-codes according to a separate internal standard, and the same number may trigger a different machine action compared to a Fanuc system. Manufacturers document their M-code assignments in the controller's programming manual, which acts as the authoritative reference for any machine running that control. Machine builders add custom M-codes during installation to address functions unique to that machine configuration. The same cutting program transferred from one CNC controller brand to another requires a full M-code audit to identify any conflicting assignments before the program runs safely.


#### Can the Same M-Code Perform Different Functions on Different CNC controllers?


Yes, the same M-code can perform different functions on different CNC controllers. The function an M-code performs depends on the specific controller's configuration and the manufacturer's command assignments. For example, M19 may control spindle orientation on Fanuc systems, but it could perform a different task on another controller with reassigned M-code numbers. Machine builders may modify the aforementioned assignments during installation, resulting in variations even within machines using the same controller brand. Programmers transferring programs across machines must verify each M-code against the destination machine’s documentation to ensure the correct function occurs, preventing potential errors and ensuring optimal machine performance. Failing to do so can result in incorrect operations, risking damage to tooling, fixtures, or the CNC controller itself.


## How Do M-Codes Control Spindle and Coolant in Face Milling?


M-codes activate the spindle and coolant at the correct points in the program to prepare the machine for cutting before the tool contacts the workpiece surface in face milling. M03 starts the spindle in the clockwise direction at the speed specified by the S-word, bringing the face mill to full rotation before the first pass begins. M08 opens the flood coolant valve to manage the heat generated during face milling operations, where high metal removal rates can lead to rapid thermal degradation of the tool inserts. M05 stops the spindle after the final pass is complete, and M09 shuts off the coolant before the tool retracts to the safe position. Placing the aforementioned commands in the correct program sequence ensures the face mill enters the cut at full speed with active cooling, which protects the insert edges and produces a consistent surface finish. Improper placement of spindle and coolant M-codes in a face milling program leads to tool entry without rotation or cutting without fluid, which accelerates tool wear and damages the machined surface. Correct M-code sequencing in[face milling](https://www.xometry.com/resources/machining/face-milling-vs-peripheral-milling/) directly determines tool life, cycle efficiency, and surface quality across any pass.


### Which M Codes Control Spindle and Coolant?


M03 and M08 are the primary commands that control spindle rotation and coolant activation in CNC milling operations. M03 starts the spindle in the clockwise direction at the programmed speed, preparing the cutting tool for contact with the workpiece. M08 opens the flood coolant system, sending a continuous stream of cutting fluid to the tool and workpiece interface to reduce heat and remove chips. M04 handles counter-clockwise spindle rotation for reverse-cutting applications. M07 activates mist coolant as an alternative to flood coolant in operations where full fluid flow is unnecessary. M05 stops the spindle at the end of a cutting sequence, and M09 shuts off the entire active coolant system. Together, spindle and coolant M-codes form the core machine state commands that any milling cycle depends on.


#### Do M03 and M08 Control the Spindle and Coolant in CNC Milling?


Yes, M03 and M08 control the spindle and coolant in CNC milling. The M03 starts the spindle in clockwise rotation, and M08 activates flood coolant in CNC milling operations. M03 requires a speed value set by the S-word in the program to define how fast the spindle rotates during the cutting pass. M08 opens the coolant valve and maintains continuous fluid flow to the cutting zone throughout the active milling sequence. The mentioned commands appear near the beginning of the cutting cycle, before the tool moves into contact with the workpiece. Correct placement of M03 and M08 in the program ensures the tool enters the cut at full speed with active cooling, protecting insert life and maintaining part quality.


## How Do M-Codes Affect Machining Parameters for Different Steel Grades?


M-codes affect machining parameters for different steel grades by controlling the spindle state and coolant conditions that determine how the cutting tool performs against each material. Harder steel grades (4140 or D2) tool steel require lower spindle speeds and higher coolant flow to manage heat and tool pressure, and M-codes set those conditions before each cut. Softer grades (1018) mild steel tolerate higher spindle speeds with standard flood coolant, still relying on M03 and M08 to activate those conditions at the correct program point. M06 executes tool changes as it moves through roughing and finishing operations, using different insert grades suited to the steel undergoing machining. Gear selection codes (M41 and M42) adjust spindle torque output to match the cutting forces required by different steel hardness levels. M05 and M09 stop the spindle and coolant after each operation, allowing the program to transition cleanly across the passes with different parameters. Accurate M-code sequencing across operations on different[steel grades](https://www.xometry.com/resources/materials/steel-types-grades/) keeps cutting conditions consistent and prevents premature tool failure caused by incorrect machine states.


### How Do M-Codes Affect Spindle Speed and Feed for Different Steels?


The M-codes affect spindle speed and feed for different steel types by controlling the machine states that define cutting conditions for each material. M03 activates the spindle at the speed set by the S-word, with the speed value changing across program operations based on the steel grade in use. Stainless steel grades require lower spindle speeds and higher feed forces compared to mild steel, and programmers adjust the S-word value before each M03 command to match those requirements. M41 shifts the spindle into low gear on machines equipped with a geared headstock, providing the mechanical advantage needed for high-torque cutting on hardened steels. M42 switches to high gear for lighter passes on softer steel grades, enabling faster spindle speeds with lower torque demand. M48 enables feed rate override so operators adjust feed rates manually when cutting conditions on a specific steel type require real-time correction. The combination of gear selection, spindle activation, and feedrate control through M-codes provides programmers with precise authority over the cutting parameters, ensuring optimal performance when working with[Steel Metal](https://www.xometry.com/resources/materials/steel/) .


#### Do CNC Programs Use M-Codes to Optimize Machining for Steel Grades?


Yes, CNC programs use M-codes to optimize machining for steel grades. M41 and M42 select the appropriate gear range, adjusting the torque and spindle speed to meet the specific requirements of the steel. It ensures the optimal cutting conditions, improving efficiency and extending tool life during the machining process. M03 activates the spindle at a speed set by the S-word, which adjusts based on the hardness and machinability of the steel grade. M08 provides flood coolant, though for certain milling operations in stainless steel or high-temp alloys, dry machining or M07 mist is often preferred to prevent thermal cracking of carbide inserts. The aforementioned materials tend to generate significant heat, which can affect tool life and surface quality. M48 and M49 control feed rate override functions, enabling operators to adjust the feed rate during the cut if necessary. The flexibility allows for real-time adjustments depending on the cutting conditions, ensuring optimal machining performance while following the required program parameters for the material processed.


## How Are M-Codes Applied in CNC Plasma Cutting Operations?


The M-codes are applied in CNC plasma cutting operations to control the torch, gas flow, and arc ignition at the correct points in the cutting cycle. The plasma cutting process requires the torch to activate, the cutting gas to reach proper pressure, and the arc to ignite before the machine moves along the cut path. M-codes handle each of those activation steps by triggering the corresponding machine function when the program reaches the command line. M-codes initiate the plasma sequence, while specific parameters like pierce delay and torch height are often managed by the controller's internal logic or G04 dwell commands once the M-code arc-on signal is received. Different plasma systems use different M-code assignments for torch-on and torch-off functions, making controller documentation the required reference for correct programming. M-codes that control gas flow ensure the correct gas type and pressure are active before the arc fires, which directly affects cut quality and kerf width on different material thicknesses. Proper M-code sequencing in a[CNC plasma cutting](https://www.xometry.com/resources/machining/what-is-cnc-plasma-cutting/) program prevents misfires, incomplete cuts, and gas pressure errors that would damage the workpiece or shorten consumable life.


### How Do M-Codes Control Plasma Torch and Gas Flow?


The M-codes control plasma torch ignition and gas flow by activating the machine components assigned to those functions at the programmed points in the cutting cycle. The torch-on M-code triggers the ignition sequence. The CNC controller then waits for an Arc OK or Transfer signal from the plasma power supply before initiating any G-code motion. The gas flow command activates pre-flow, which purges the torch and establishes the correct gas pressure before ignition. A pierce delay sequence holds the torch in position until the plasma penetrates the material before the motion path begins. The torch-off command shuts down the arc and gas flow at the end of each cut segment, allowing the machine to reposition without the plasma torch active. Post-flow gas continues briefly after the torch-off command to cool the consumables before the next ignition cycle.


#### Do Plasma CNC Machines Use M Codes to Control Torch and Gas?


Yes, plasma CNC machines use M-codes to control the torch and gas. The aforementioned M-codes play a crucial role in regulating various functions throughout the cutting cycle, ensuring precision and efficiency. The torch-on command is one of the first M-codes used, activating the arc and initiating plasma gas flow at the start of each cut. Following the specific M-codes triggers the gas pre-flow and post-flow functions, which are essential for maintaining cooling and pressure stability. The aforementioned functions protect the consumables from damage and ensure consistent performance during the cutting process. Specific M-codes can enable or disable the Automatic Torch Height Control (ATHC) system during a cut to prevent the torch from diving into the plate during corners or lead-outs. An M-code activates the height sensor, adjusting the torch height for optimal cutting conditions before the cutting path begins. Each stage of the plasma CNC cutting cycle depends on accurately placed M-codes, ensuring that the torch ignites, operates, and shuts off correctly, maintaining cutting quality and reducing errors.


## What Are the Benefits of Using M-Codes?


The benefits of using M-codes are listed below.


- **Automated Machine Control** : M-codes eliminate the need for manual operator intervention during the cutting cycle by activating spindle rotation, coolant flow, and tool changes at programmed points. Automation reduces cycle time and removes the risk of human error during repetitive operations.
- **Precise Function Sequencing** : M-codes place any machine function at the exact point in the program where it is needed, ensuring the spindle is at full speed and coolant is active before the tool contacts the workpiece. Correct sequencing prevents tool damage caused by cutting without proper machine conditions.
- **Extended Tool Life** : Coolant M-codes (M08) help manage thermal energy, though in many high-speed milling operations with carbide, dry machining is preferred to avoid thermal cracking of the tool inserts. Spindle stop commands (M05) prevent unnecessary rotation across the operations, reducing bearing and spindle wear.
- **Flexible Program Structure** : M-codes (M98 and M99) allow programmers to call subprograms and create loops, reducing program length and simplifying complex multi-operation cycles. Modular program structures built with M-codes are easier to edit and reuse across different jobs.
- **Consistent Repeatability** : M-codes execute the same machine function the same way on any program cycle, eliminating variation caused by manual machine control. Consistent machine states across any run produce uniform part quality and predictable cycle times.
- **Improved Operator Safety** : M-codes (M00 and M01) insert controlled program stops that allow operators to inspect the workpiece or tool without manually intervening in an active cutting cycle. Planned stops reduce the risk of unexpected machine behavior during inspection.
