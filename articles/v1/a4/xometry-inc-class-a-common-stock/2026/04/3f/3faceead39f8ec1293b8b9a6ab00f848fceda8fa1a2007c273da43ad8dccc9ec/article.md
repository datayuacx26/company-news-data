---
schema_version: "1.0.0"
document_id: "3faceead39f8ec1293b8b9a6ab00f848fceda8fa1a2007c273da43ad8dccc9ec"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/machining/g-code-and-m-code-differences/"
published_at: "2026-04-16T03:55:00+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:8994bd70822371817112ed70ebe55d99321682643caac81850da0a8d0cadd736"
---

# G-Code and M-Code Differences in CNC Programming

## What Are Common G-Codes?


The common G-Codes are listed below.


- **G00 (Rapid Positioning)** : G00 moves the cutting tool to a specified coordinate at the machine's maximum travel speed. Programmers use G00 to reposition the tool between the cuts without engaging the workpiece.
- **G01 (Linear Interpolation)** : G01 moves the tool in a straight line at a defined feed rate for controlled cutting. The command requires an F-word value to set the cutting speed along the programmed axis path.
- **G02 (Clockwise Circular Interpolation)** : G02 guides the cutting tool along a clockwise arc between the start and end coordinate points. The arc radius or center offset values define the curvature of the tool path.
- **G03 (Counterclockwise Circular Interpolation)** : G03 moves the tool along a counterclockwise arc using the same parameter structure as G02. Programmers select G02 or G03 based on the required direction of the arc.
- **G17, G18, G19 (Plane Selection)** : G17 activates the XY plane, G18 activates the XZ plane, and G19 activates the YZ plane. Plane selection determines how circular interpolation and cutter compensation commands are applied during the machining operation.
- **G20 / G21 (Unit Selection)** : G20 sets the controller to read coordinates in inches, while G21 switches to millimeters. The unit selection command must appear before any motion commands in the program.
- **G28 (Return to Home Position)** : G28 sends specified machine axes back to the reference home position via an intermediate coordinate. Programmers use G28 at the start or end of a program to reset the tool position.
- **G40, G41, G42 (Cutter Compensation)** : G40 cancels cutter radius compensation, G41 applies left-side compensation, and G42 applies right-side compensation. The aforementioned commands adjust the tool path to account for the physical diameter of the cutting tool.
- **G90 / G91 (Absolute vs. Incremental Positioning)** : G90 sets the controller to read all coordinates as absolute positions from the program origin. G91 switches to incremental mode, where each coordinate value derives from the tool's current position.
- **G94 / G95 (Feed Rate Mode)** : G94 sets the feed rate in units per minute, while G95 sets it in units per revolution. Feed rate mode selection affects how the controller interprets the F-word value in motion commands.


### Which G-Codes Handle Rapid and Controlled Movements?


G00 handles rapid positioning, and G01 controls linear cutting movement at a programmed feed rate. G00 moves the cutting tool to a target coordinate at the machine's maximum axis speed without engaging the workpiece. G01 moves the tool in a straight line at a feed rate defined by an F-word value in the same program block. Programmers use G00 to reposition the tool across the cutting passes and G01 to perform straight-line cuts at controlled speeds. The distinction between G00 and G01 commands allows the programmer to prevent the tool from cutting at high speed during non-machining repositioning moves. The two commands require target coordinate values for each active axis. G00 prioritizes speed, while G01 prioritizes accuracy and surface quality. Together, the aforementioned commands manage fast positioning and precise machining movements throughout the entire CNC program sequence.


#### Are G00 and G01 Standards in Most CNC Programs?


Yes, G00 and G01 are standard in most CNC programs. Nearly every CNC program includes at least one instance of G00 for rapid tool positioning and G01 for controlled linear cutting. G00 is used to move the tool to the starting position before the cutting pass begins, minimizing cycle time during non-cutting moves. G01 performs straight-line cuts at precise feed rates, making it the primary command for linear material removal. The RS-274 standard defines the commands, and virtually all CNC controller platforms recognize them. A machinist writing or editing[CNC programs](https://www.xometry.com/resources/machining/cnc-programming/) will encounter G00 and G01 in nearly every tool path sequence across milling, turning, and drilling operations. The G00 and G01 commands are essential for controlling tool motion and positioning, ensuring accurate and efficient machining. Whether performing basic operations or complex part fabrication, G00 and G01 are fundamental in all CNC machining environments.


## What Are Common M-Codes?


The common M-Codes are listed below.


- **M00 (Program Stop)** : M00 pauses the CNC program at the point where the command appears in the code. The machine halts all motion and waits for the operator to press the cycle start button before continuing.
- **M01 (Optional Stop)** : M01 pauses the program when the operator activates the optional stop function on the machine control panel. Programmers insert M01 at inspection points, allowing the operator to choose when to activate or bypass it. The operator controls the pause, ensuring flexibility during the process.
- **M02 (End of Program)** : M02 signals the end of the CNC program and stops all machine activity. The controller does not automatically reset to the program start position after executing M02.
- **M03 (Spindle On, Clockwise)** : M03 starts the spindle in the clockwise direction at the speed defined by the S-word command. Programmers place M03 before any cutting commands to ensure the spindle is rotating before the tool contacts the workpiece.
- **M04 (Spindle On, Counterclockwise)** : M04 activates the spindle in the counterclockwise direction, used for left-hand cutting tools or specific threading operations. The S-word sets the spindle speed for the command.
- **M05 (Spindle Stop)** : M05 stops spindle rotation immediately. Programmers use M05 before tool changes, program pauses, or at the end of a cutting sequence.
- **M06 (Tool Change)** : M06 initiates an automatic tool change, commanding the machine to swap the current tool with the one specified by a T-word command. The machine pauses the machining path movement during the tool change cycle.
- **M08 (Coolant On)** : M08 opens the coolant valve and directs cutting fluid onto the machining zone. Activating coolant reduces heat buildup and removes chips from the cutting area.
- **M09 (Coolant Off)** : M09 closes the coolant valve and stops fluid flow. Programmers place M09 before a tool change or at the end of a cutting sequence.
- **M30 (End of Program and Reset)** : M30 ends the CNC program and resets the controller back to the beginning of the program. Programmers use M30 as the final command in a CNC program to prepare the machine for the next cycle.


### Which M-Codes Control the Spindle On or Off and Coolant?


M03 or M04 starts the spindle, M05 stops it, and M08 activates the coolant. M03 starts the spindle in the clockwise direction at the speed set by the S-word command, while M04 activates it counterclockwise for specific tool or threading requirements. M05 stops spindle rotation at any point in the program where the tool must pause or change. M08 opens the coolant valve to direct cutting fluid onto the machining zone, and M09 closes it when cooling is no longer needed. The aforementioned commands prepare and maintain the machine during cutting operations by managing both rotational force and thermal control. The CNC controller executes the aforementioned M-codes at the exact program line where the codes appear, ensuring the spindle and coolant system respond at the correct machining step. The **M-code and G-code** are integral to the precise control of machine functions during each operation. Proper sequencing of M08 and spindle commands prevents tool damage and improves surface finish quality throughout the cutting cycle.


#### Do M03 and M08 Appear Often in CNC Programs?


Yes, M03 and M08 appear often in CNC programs. M03 activates the spindle before any cutting begins, making it one of the first commands in nearly every CNC program that involves a rotating tool. M08 follows closely to activate coolant and protect the tool and workpiece from heat during cutting. The M03 and M08 are commonly used to start the spindle and enable coolant before machining begins, appearing at the start of each new tool path or after a tool change. A CNC program without M03 risks damaging the tool by initiating cuts without spindle rotation. Omitting M08 in high-speed or deep-cut operations increases heat and accelerates tool wear. Both commands function as standard setup instructions that machinists include in each CNC cutting sequence.


## How Do G-Codes and M-Codes Work Together?


G-code controls the movement and path of the cutting tool, while M-code manages auxiliary machine operations, and together, G-code and M-code execute a complete machining process. A CNC program sequences G-codes and M-codes within the same file, with each command placed at the exact line where its action must occur. M03 starts the spindle before G01 moves the tool into the workpiece for a straight-line cut. M08 activates coolant at the same point where cutting begins, keeping the tool temperature within safe limits. At the end of a cutting sequence, the tool retracts before M05 stops the spindle and M09 shuts off the coolant. G00 then repositions the tool rapidly to the next starting point before the next cutting cycle begins. The CNC controller reads both command types sequentially and executes each one at the right moment in the program. Coordinating G-code and M-code commands is what transforms raw program instructions into a complete, functional machining operation.


### How Do M-Codes Operate While G-Codes Move the Tool?


M-codes activate machine actions at specific points in the program while G-codes command the tool to move along programmed paths. A G01 command moves the tool through a straight cut, and an M08 command on a preceding line keeps coolant flowing throughout that movement. M-codes activate spindle rotation or coolant flow at specific program points, directly supporting the cutting action that G-codes perform. The CNC controller processes both command types from the same program file, reading each line in sequence. G-codes consume machine time during axis movement, while M-codes execute their machine actions quickly and return control to the next program line. In specific cases, a single program block contains both a G-code and an M-code, allowing motion and machine action to be commanded in a single block. The G-code and M-code commands run within the same program to synchronize machine operation with cutting motion throughout the entire machining cycle.


#### Do CNC Programs Combine Both G and M Codes?


Yes, CNC programs combine both G-codes and M-codes. Each functional CNC program requires motion commands from G-codes and operational commands from M-codes to complete a machining cycle. G-codes alone cannot start the spindle, activate coolant, execute tool changes, or end the program, and M-codes alone cannot move the cutting tool. A typical program begins with M03 to start the spindle, followed by G00 to position the tool, then G01 to begin cutting, with M08 managing coolant throughout. M30 closes the program at the end and resets the controller for the next cycle. Removing either command type from the program leaves the machine unable to perform a complete machining operation. The integration ensures that tool movement and machine support functions operate together at each step of the cutting process.


## How Do CNC Machines Use G-Codes and M-Codes?


A CNC machine interprets G-codes to move tools and M-codes to control machine features through the onboard controller. The controller reads the program file line by line and executes each command at the moment it appears in the sequence. G-code commands direct the motion control system, sending signals to the servo motors that drive each axis. M-code commands interact with the machine's electrical subsystems, triggering the spindle motor, coolant pump, or tool changer. The controller processes both command types within the same program cycle without requiring separate input files. Modern CNC controllers store the full program in memory and execute it at speeds that match the required feed rates and spindle speeds. The machine responds to G-codes and M-codes in real time, adjusting tool position and machine state in coordinated sequence. Xometry offers detailed overviews of CNC machines and how each type interprets the aforementioned commands during production operations.


### How Do Different CNC Brands Implement Different Code Sets?


CNC manufacturers customize code sets to match their machine architecture, resulting in variations in M-code functions or additional proprietary commands. The core G-codes defined by the RS-274 standard remain consistent across most controllers, covering fundamental motion commands (G00, G01, G02, and G03). M-codes vary more significantly across brands since M-codes interact directly with the machine's unique electrical and mechanical components. Haas Automation assigns specific M-codes to air blast control or probing cycles that do not appear on other platforms. Siemens controllers use a different command structure for specific cycle operations and modal functions. Programmers transferring CNC code across the machines from different manufacturers must review the target machine's manual before running the program. Brand-specific M-code variations are the most common source of programming errors when adapting code from one CNC platform to another.


#### Are Some Codes Different on Haas and Siemens Machines?


Yes, some codes are different on Haas and Siemens machines. Both platforms share the standard G-code set for linear and circular interpolation, but their M-code assignments diverge on optional and machine-specific functions. Haas controllers use M-codes in ranges above M100 for custom macro functions and optional machine features not covered by standard code lists. Siemens controllers use cycle calls and parameter-based commands that replace specific standard G-code structures used on other platforms. Although core commands are similar, specific machine functions or optional codes vary by brand, requiring programmers to consult each machine's reference documentation before writing or transferring programs. A program written for a Haas machine does not run correctly on a Siemens controller without reviewing and adjusting the brand-specific command differences. CNC programmers working across multiple platforms must treat each controller's code set as its own reference standard.


## Where Did G-Codes and M-Codes Come From?


G-codes and M-codes originated from early CNC programming standards developed for automated machining in the mid-20th century. The Electronic Industries Alliance published the RS-274 standard in the 1960s to define a universal set of numeric commands for controlling machine tools. Before that standard, each machine tool manufacturer used proprietary programming methods that were incompatible with other systems. RS-274 established the letter-address format where "G" preceded preparatory commands and "M" preceded miscellaneous machine functions. The standard allowed machines from different manufacturers to interpret numeric commands for motion and machine control using the same structured format. MIT's Servomechanisms Laboratory contributed foundational research in the early 1950s that led to the first numerically controlled machine tools, which used punched tape to input programmed coordinates. The separation of motion control commands from machine function commands was a deliberate design decision that persists in modern CNC programming. The letter designations "G" and "M" reflect the functional categories established in that original RS-274 standard.


### How Are G-Codes Designed for Motion and M-Codes for Machine Functions?


G-codes are designed to describe tool movement paths and coordinate positions within a structured numeric format. The RS-274 standard assigned the "G" prefix to commands that control how and where the tool moves, including positioning, interpolation, feed rate mode, and unit selection. M-codes were created to control operational machine functions that support machining, including spindle activation, coolant control, tool changes, and program termination. The two command types address different hardware systems within the same machine. G-codes communicate with the motion controller, which drives the servo motors on each axis. M-codes communicate with the programmable logic controller (PLC), which manages electrical signals to auxiliary machine components. Dividing the responsibilities of the two command prefixes gave early CNC system designers a clean architecture for separating kinematic control from machine state management. The design has remained largely unchanged because it maps directly to the physical separation of motion systems and auxiliary systems in CNC hardware.


#### How Were G-Codes and M-Codes Standardized in Early CNC Systems?


Early CNC systems standardized G-code commands through the RS-274 specification, which defined a letter-address format for programming motion and machine functions. The RS-274 standard assigned numeric codes to specific machine actions, establishing a common language that multiple manufacturers adopted for their controllers. G-codes received numeric assignments for motion types, with each number corresponding to a defined rapid travel, linear cutting, or arc movement. M-codes received numeric assignments for machine operations, covering spindle control, coolant management, and program flow functions. The standard defined motion commands and laid the foundation for modern CNC programming by creating a format that controllers from different manufacturers could interpret consistently. Industry adoption of RS-274 made it possible to write CNC programs that transferred across the machines with minimal modification. Later versions, including RS-274-D and its ISO equivalents, expanded the original command set to cover additional machine types and operational requirements.


## How Are G-Codes and M-Codes Documented?


G-codes and M-codes are documented in machine programming manuals and controller reference guides provided by CNC manufacturers. Each manual contains a complete list of supported G-codes and M-codes, along with the parameters each command requires and the machine functions each one activates. Manufacturers organize documentation by command prefix, listing all G-codes in numeric order, followed by all M-codes in numeric order. Each entry includes a command description, syntax example, applicable axes, and any machine-specific limitations or variations. Controller reference guides go beyond basic command lists to explain modal groups, command conflicts, and the order in which commands execute within a single program block. Digital versions of the aforementioned documents are indexed and searchable, making it faster to locate specific command behavior during program development. Machine-specific documentation is more reliable than general CNC references because it reflects the exact command set supported by that controller.


### Do CNC Manuals List All Standard and Optional Codes?


Yes, CNC manuals list all standard and optional codes. Standard codes follow the RS-274 specification and appear in each manual since these codes are available for all programs running on that controller. Optional codes cover machine-specific features, including probing cycles, air blast control, pallet changers, and advanced spindle functions available only on specific machine configurations. Manuals separate standard codes from optional ones, noting which hardware or software options need installation for the optional commands to function. The manuals provide detailed explanations of each command and its machine-specific behavior, including parameter requirements, axis applicability, and any interlock conditions. Programmers consulting a manual for an optional M-code must verify that the corresponding machine option is active before using that command in a production program, and relying on a general CNC reference instead of the machine-specific manual risks using commands that are unsupported or behave differently on that particular controller.


#### Should Operators Check Manuals for Proper Usage?


Yes, operators should check the manuals for proper usage. The machine manual contains the exact list of G-codes and M-codes supported by that specific controller, including any commands that differ from the RS-274 standard. Using a command that is unsupported, reassigned, or parameter-dependent without consulting the manual risks machine faults, incorrect tool motion, or equipment damage. Optional codes require hardware or software options that vary across the machine configurations, and the manual specifies which options must be active for each command to function. The manual documents command syntax, modal group behavior, and execution order within a single program block, information unavailable in general CNC references. A programmer transferring code across the machines from different manufacturers must cross-reference both manuals to identify command differences before running the program. Consulting the machine-specific controller manual is the only reliable method for confirming correct command behavior on a given CNC system.


## How Do G-Codes and M-Codes Work in Advanced CNC Operations?


In advanced operations, G-codes control complex tool paths while M-codes manage supporting machine actions to enable high-precision machining. Multi-axis programs use G-codes to guide the tool across simultaneous rotary and linear axes, generating compound curves and undercuts that flat 2-axis programs cannot produce. M-codes in the mentioned programs handle spindle activation and direction, automatic tool changes, and coolant mode switching at precise points in the cutting sequence. High-speed machining cycles rely on G-code modal settings (G64 for path blending and G43 for tool length compensation) to maintain accuracy at elevated feed rates. M-codes activate probe hardware for cycles that measure the workpiece mid-program and feed measurement data back to the controller for compensation. The coordination across the complex G-code paths and targeted M-code actions enables efficient automation in high-precision machining tasks. Advanced CNC operations demand that programmers understand both command types at a level beyond basic motion and spindle control. Xometry's resources cover how G-code and M-code coordination support advanced production requirements across complex part geometries.


### How Are G-Codes and M-Codes Used Together in Multi-Axis Machining?


In multi-axis machining, G-codes guide tool movement across multiple axes simultaneously, while M-codes control supporting machine functions at critical points in the program. G-codes in a 5-axis program control simultaneous linear and rotary axis motion, generating tool paths that follow complex contoured surfaces with continuous angular adjustment. M-codes control spindle speed, coolant, tool changes, and other functions at defined program points without interrupting axis motion. G49 for tool length compensation cancellation, G68 for coordinate rotation, and other G-code modal settings manage orientation changes between operations in multi-axis programs. M-codes in advanced programs trigger automated probing, activate high-pressure coolant, or engage synchronized axis clamping at specific machining steps. The CNC controller manages both command types in real time to maintain synchronization across the axis motion and machine state. Proper use of G-code and M-code commands in multi-axis programs is the foundation of accurate, repeatable, high-complexity part production.


#### Can Complex CNC Programs Run Without Both Codes?


No, complex CNC programs cannot run without both codes. G-codes alone provide tool motion instructions but cannot start the spindle, activate coolant, execute tool changes, or end the program. M-codes alone manage machine functions but have no mechanism to move the cutting tool along any path. Tool motion and machine operation must work together to perform complete machining processes, and the absence of either command type leaves the program functionally incomplete. A complex multi-axis program relies on G-codes for continuous path control across rotary and linear axes and on M-codes for spindle management, coolant switching, and automated tool changes throughout the cycle. Removing M-codes from an advanced program means the spindle never starts and the coolant never activates, making cutting impossible. The interdependence of G-code and M-code commands is the structural basis of each complete CNC machining program.


## Summary


Xometry provides a wide range of manufacturing capabilities and other value-added services for all of your prototyping and production needs. Visit our website to learn more or to request a[free, no-obligation quote](https://www.xometry.com/quoting/home/) .


#### Disclaimer


*The content appearing on this webpage is for informational purposes only. Xometry makes no representation or warranty of any kind, be it expressed or implied, as to the accuracy, completeness, or validity of the information. Any performance parameters, geometric tolerances, specific design features, quality and types of materials, or processes should not be inferred to represent what will be delivered by third-party suppliers or manufacturers through Xometry’s network. Buyers seeking quotes for parts are responsible for defining the specific requirements for those parts. Please refer to our[terms and conditions](https://www.xometry.com/legal/) for more information.*
