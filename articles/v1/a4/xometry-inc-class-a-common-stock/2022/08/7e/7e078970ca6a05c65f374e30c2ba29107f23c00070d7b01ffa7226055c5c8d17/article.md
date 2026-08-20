---
schema_version: "1.0.0"
document_id: "7e078970ca6a05c65f374e30c2ba29107f23c00070d7b01ffa7226055c5c8d17"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/3d-printing/3d-printing-stl-files/"
published_at: "2022-08-30T00:47:38+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:26:31.203349+00:00"
content_hash: "sha256:25f18edb7422ffb8cd6436a5f07e06049aa8874da407a1b2b910201eda8b4f0b"
---

# STL to 3D Printer: How to 3D Print STL Files?

###### Step 1 of 6


### 1. Obtain or Create an STL File


**Overview**


Obtain or create a Stereolithography (STL) file, which serves as the digital foundation for 3D printing. The file contains the 3D model of the object represented by triangular facets, allowing the printer to reproduce the object layer by layer. STL files are downloaded from online repositories or custom models created using 3D Computer-Aided Design (CAD) software. The STL file provides surface geometry only, not machine-readable instructions. It must be processed by slicing software to generate G-code, which the printer actually interprets.


**How the step affects print quality**


The step affects print quality by directly influencing the final print outcome. A well-designed STL file with proper wall thickness, accurate mesh resolution, and manifold geometry ensures smooth surfaces, strong structures, and reliable prints. Low-quality STL files with errors (inverted normals or non-manifold edges) lead to printing issues, weak parts, or structural defects. Verifying the STL file for printability with slicing software prevents errors and ensures the model is ready for printing.


###### Step 2 of 6


### 2. Import the STL Into a Slicer


**Overview**


Import the STL file into slicing software, which prepares the 3D model for printing. The slicer converts the 3D mesh into layer-by-layer instructions, generating G-code for FDM/FFF printers


It is important to note that not all 3D printers use G-code. Some use proprietary formats derived from slicing. Slicer software allows for adjusting print settings (layer height, print speed, and support structures). Importing the STL into the slicer is an essential step before moving forward with the printing process.


**How the step affects print quality**


The step affects print quality by ensuring the 3D model is correctly prepared for printing. The slicer software breaks the model into layers, optimizing it for the chosen printer and material. Proper settings ensure a smooth print process, minimize errors, and avoid issues like weak structures or failed prints. Adjusting parameters (support structures, layer height, and infill density) improves the outcome of the printed object.


###### Step 3 of 6


### 3. Adjust Print Settings


**Overview**


Adjust the print settings in the slicer software to tailor the 3D print according to the desired outcome. Settings (layer height, print speed, temperature, and material type) are configured. The slicer software provides options to customize parameters for the specific printer and filament being used. Proper adjustments help optimize print quality, strength, and efficiency, ensuring the final object meets the intended specifications.


**How the step affects print quality**


The step affects print quality by fine-tuning the conditions under which the 3D model is printed. For example, adjusting layer height controls the resolution and surface smoothness, while print speed affects the print time and precision. Temperature settings ensure the filament melts correctly for better adhesion between layers, and material type selection in slicer software typically assists with temperature and speed presets, but the actual material properties ( strength, flexibility) depend on the physical filament used, not just slicer settings. Incorrect settings lead to print failures, weak structures, or poor surface finish.


###### Ste 4 of 6


### 4. Slice the Model


**Overview**


Slice the model in the slicer software to prepare it for 3D printing. The process involves converting the 3D model into horizontal layers, generating a G-code file that contains the instructions for the printer. The slicing software analyzes the 3D mesh (from STL or other formats) and generates toolpaths based on user-defined settings. The software does not determine slicing automatically without user input (layer height, infill). Once the slicing is complete, the G-code file is ready for transfer to the printer.


**How the step affects print quality**


The step affects print quality by determining how the model is printed layer by layer. Proper slicing ensures that each layer is printed accurately, contributing to the structural integrity and surface finish of the final object. The way the slicer handles features such as supports, overhangs, and infill patterns impacts the strength, appearance, and print time. Incorrect slicing results in defects, poor layer adhesion, or failed prints.


###### Step 5 of 6


### 5. Send the G-Code to the 3D Printer


**Overview**


Send the generated G-code file to the 3D printer to begin the printing process. The G-code contains the specific instructions for the printer, detailing how to move the print head, extrude material, and build the object layer by layer. The file is transferred via SD card, USB cable, direct connection, or wireless transfer, depending on the printer's capabilities. The step initiates the communication between the slicer software and the 3D printer, allowing the printer to follow the defined instructions.


**How the step affects print quality**


The step affects print quality by ensuring the 3D printer receives accurate instructions for creating the object. Transferring the G-code correctly ensures the printer executes the slicing plan precisely, leading to high-quality prints. The printing process is disrupted by errors during file transfer or incorrect configurations, resulting in misprints, failed layers, or other print defects. Proper file transfer is critical to maintaining print consistency and quality.


###### Step 6 of 6


### 6. Start the 3D Print


**Overview**


Start the 3D print by selecting the G-code file and initiating the print on the 3D printer. Ensure the printer is calibrated correctly, with the print bed leveled and the extruder prepared. The printer follows the G-code instructions to build the object layer by layer once the print begins. Monitor the initial layers to confirm proper adhesion and print quality before leaving the print to finish.


**How the step affects print quality**


The step affects print quality by ensuring that the 3D printer starts the process under optimal conditions. Proper calibration of the printer (bed leveling and nozzle temperature) is essential for achieving good adhesion and a stable print. The print fails if the first layers do not adhere correctly, leading to defects or warped objects. Starting the print in a controlled environment ensures smooth printing, reducing the risk of print failure and ensuring a high-quality final product.
