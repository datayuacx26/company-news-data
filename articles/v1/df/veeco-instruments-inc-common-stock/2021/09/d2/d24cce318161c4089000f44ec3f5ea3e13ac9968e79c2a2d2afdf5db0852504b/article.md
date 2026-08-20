---
schema_version: "1.0.0"
document_id: "d24cce318161c4089000f44ec3f5ea3e13ac9968e79c2a2d2afdf5db0852504b"
company_key: "veeco-instruments-inc-common-stock"
company: "Veeco Instruments Inc."
source_id: "veeco-instruments-inc-common-stock-rss-ca17b9656fba"
canonical_url: "https://www.veeco.com/company/blogs/challenges-in-gallium-arsenide-etch-and-the-evolution-of-etch-tool-design/"
published_at: "2021-09-29T13:32:15+00:00"
first_seen_at: "2026-07-25T01:53:19.132600+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:6d484a6860b28bf6145add8a57cd7557eae9480c1d0910813387907ae45ad80f"
---

# Challenges in Gallium Arsenide Etch and the Evolution of Etch Tool Design

## Challenges in Gallium Arsenide Etch and the Evolution of Etch Tool Design


Blogs | Sep 29, 2021


**By John Taddei – Veeco’s Director, Process Development Engineer**


Fab wafer starts with Gallium Arsenide (GaAs) substrates have been increasing for many years and is projected to continue. The trend has been driven by a number of applications including: VCSEL, photonics, lidar applications, RF and LED. As indicated by the graph below, this trend is expected to continue. A portfolio of etching processes is required to support various process flows. These processes include wafer thinning, surface texture control, gate recess, mesa etch, UBM etch and selective etching of substrate to films and vice versa. Each process has unique requirements and therefore requires a tailored tool configuration and process.


Traditional wafer thinning is a common process to eliminate damage from grind, control surface texture and relieve stress in the wafer. Wafers (675 micron in thickness) are mounted to carriers to protect the active side of the substrate. In many cases the carriers are not the same material or diameter as the GaAs wafer. The bulk of material removal is performed by the grind/polish step (to 200 um), which determines the final thickness and profile. Any number of etchants are employed with acid-oxidizer combinations yielding several micron per minute etch rates being common. In general, the wet etch profile uniformity and etch depth repeatability have historically been far tighter than those of the grinding process.


**Schematic of Grind Plus Thinning Process**


**Schematic of thinning process**


Wafer thinning etch requirements have been expanded in a number of process flows, which drives the evolution of etch tools. To lower COO the number of capital tools required and number of process steps are desired to be minimized. Enhancements in etch tool design have achieved this goal and are yielding superior results to traditional methods at lower cost. Technologies such as gas seal tooling, enhanced chemical management, on-board metrology and end point detection capabilities are the drivers. Consider the wafer thinning process outlined above from 675 micron to 200 microns. By employing on the wet etch tool gas seal chucks like the one shown below, chemistry contact can be limited to the backside of the wafer and the wafer bonding/debonding sequence can be eliminated.


Enhancements in chemical management have yielded chemistry mixtures that can achieve >20 micron/minute etch rate with outstanding etch rate repeatability. Spiking (replacing the chemical reaction limiting species consumed during the etch process), replenishing (the addition of fresh chemistry) and temperature control to .1C enable the ability to etch to specific depths. This eliminates the need for grind/polish. On board metrology permits measurement of the substrate thickness at multiple points pre and post process. This enables QC to be performed in real time, etch rate and uniformity to be identified and cataloged. This tool- specific data base enables ultimate process performance in terms of minimizing TTV within wafer and wafer to wafer on the final product.


**Bulk Etch Uniformities Verses Temperature**


**Surface Texture Verses Temperature**


**Pre Etch / Post Etch Surface**


Gallium arsenide etch rates are a function of temperature as indicated by the figure above. Per the graph to the above right final surface texture is a function of starting roughness and conditions employed during the process (chemical, mix ratio, temperature, process parameters). Surface texture (rA) can be smooth or rough post process, as desired.


GaAs pattern etches include gate recess and mesa etches. Resist or thin film masks are put in place and uncovered areas of the substrate are to have material removed. In many cases, it has been found that an oxide removal step enhances etch uniformity and repeatability. During the etch, hyperbolic arm motion is tuned to yield uniform etch processes, yielding <1.5% uniformity for the trench formation process shown below.


The data above shows a quality single wafer tool with superior chemical management is well suited for a variety of applications. There remains some applications with tighter specifications where the etch is to a stop layer. GaAs on InGaP would be an example of this. For these applications the enhancements previously outlined are paired with a chemistry that has etch selectivity (typically >100:1) between the material to be removed and the stop layer. For ultimate control the WaferChek® End Point Detection system is also invaluable to determine when an etch has completed.


Tool features are also important for etching films on GaAs substrates. Under Bump Metal (UBM) etch processes are common. Gas seal tooling to protect the substrate from the metal etchants is required. The UBM etch process requires minimum undercut. By definition undercut will occur from isotropic etchants. Selective chemistry between films, superior etch uniformity and WaferCheck® to identify when the etch is complete can result in undercut equivalent to film depth, per the image above.


Additional challenges occur from the extra demands in today’s fabs. Wafer handling requirements have increased. Requirements include: limited exclusion zones on where wafers can be contacted, the need to flip wafer pre and post process within the etch tool and the need to handle multiple substrate types, diameters and thicknesses. Similarly, it is not uncommon for a wet etch tool to handle thinning, patterned substrate etching, film etching and\\or cleaning within a single platform. Some process flows even perform thick resist\\dry film strip immediately prior to Cu/Ti UBM etch in the same tool.


**Summary**
GaAs wafer manufacturing is and will continue to increase to meet the need for increasing device performance of tomorrow’s devices. Artificial intelligence, data centers demand, 5G\\6G communications, Internet of Things (IoT) and autonomous driving are drivers for change in the semiconductor world. The drive for decreased COO and increased performance is resulting in the necessity of advanced single wafer wet etch platforms. Tighter chemical management (concentration, temperature, flow) is required to deliver a capable toolset with low COO. More demanding applications require the etch tool to deliver tighter performance while obsoleting capital equipment (grind/polish) that existed in previous process flows. On board metrology, adaptive process recipe creation and choice of selective chemistry are requirements on these demanding applications. Flexible wafer handling and processing capability combined with multiple process paths through the wet process tool are common requirements. The specification and design of application specific etch tools is the solution to these requirements. Platforms that are flexible and adaptable are the ones capable of incorporating application specific technology to provide today’s solutions.
