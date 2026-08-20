---
schema_version: "1.0.0"
document_id: "cf138d07f29a44b08ae0ce23252bf8c4b80b3adfa99fdfd58b32159bddc27663"
company_key: "baker-hughes-company-class-a-common-stock"
company: "Baker Hughes Company"
source_id: "baker-hughes-company-class-a-common-stock-news-import-09e3d3106256"
canonical_url: "https://www.bakerhughes.com/waygate-technologies/blog/real3d-measurement-glossary"
published_at: "2026-05-09T00:00:00+00:00"
first_seen_at: "2026-07-21T09:24:28.311413+00:00"
fetched_at: "2026-08-18T01:01:13.723404+00:00"
content_hash: "sha256:db0d71b701a139db502d71f3a506a1ed8221dc431e058ec1b10c5b78abc37b4d"
---

# Real3D Measurement Glossary

---


Image


Image


Eyebrow


Blog


# Real3D Measurement Glossary


3 min Read


---


---


## A glossary for advanced 3D measurement technologies, techniques and applications for video borescopes used in remote visual inspection applications.


### #


**3D Data Mask** – Shows red and yellow pixel masks. See "Red Mask on Image" and "Yellow Mask on Image" for details.


**3D Perspective Lines** – Dotted blue lines showing the four corners of the tip field of view in the full image point cloud to help the user understand and improve tip orientation relative to a surface. May be shown or hidden via the settings menu.


**3D Phase Measurement** – A measurement technology that projects line patterns on an object via a series of LEDs. Measurements are calculated using the concept of phase shift analysis combined with proprietary processing technology.


**3DPM Save Format** –


- **PMap** – Original projected pattern images are embedded in the saved image file and fully re-processed to generate new 3D data when re-measurement is performed on the recalled image. Generally used for development and troubleshooting purposes.
- **ZMap** – Computed 3D surface data is embedded in the saved image file and used when re-measurement is performed on the recalled image. Gives smaller files than PMap and is recommended for most users.


**3D Point Cloud** – A 3D graphical representation of the surface being inspected on which lines and spheres are added to show measurements made on that surface. The 3D point cloud can be rotated and viewed from multiple angles and perspectives. It allows an inspector to check measurement setup and initiate and edit cursor placement points.


**3D Stereo Measurement** – Uses the same optical principles and tip optics as Stereo Measurement and combines these with the ability to create, manipulate and analyze a 3D Point Cloud representation of measurement data.


**3D Surface Mask** – Colored semi-transparent masking of surface points in the 2D image and point cloud views during 3DPM or 3D Stereo measurement that helps guide proper cursor placement.


### A


**Active Cursor** – The cursor currently being manipulated, indicated by a blue circle.


**Advisory Message** – “For best results the reference cursors should be on the same plane” – In depth profile measurement, indicates the reference cursors are not on the same plane.


**Advisory Message** – “Move closer to target or move cursors for better results” – Indicates that the measurement is too small to be reliable at the current tip to target distance. Can be remedied by moving tip closer to the indication or selecting larger measurement area.


**Auto Add** – An Auto Area tool type where a single cursor is placed within a feature to tell the AI model which feature to mask and measure. Best used with features having clearly-defined perimeters.


**Auto Box** – An Auto Area tool type where two cursors are placed to create a rectangle around a feature to tell the AI model which feature to mask and measure. Choose Auto Box instead of Auto Add for features having less well-defined perimeters.


**Auto-Repeat** – Setting Auto Repeat in the measurement menu to “On” remembers all measurements performed on the current image along with the current measurement view. Upon subsequently pressing the 3DPM Capture (from live) or Stereo (from Freeze Frame) soft keys, these measurements will be automatically performed on the newly captured image. This feature improves efficiency when making repetitive measurements.


### C


**CSV** – Comma separated value, used for point cloud file data export. Can be opened in a CAD package.


### D


**Dark Boost** – Dark Boost is a live video processing function which digitally brightens darker areas in scenes with bright foreground surfaces, or glare, without over-exposing or degrading the contrast of more uniform scenes.


**Depth Assist** – Feature wherein the system searches the surface data in the vicinity of the first three cursors of a Depth measurement and automatically places the fourth cursor at the deepest point, highest point, or a point on a blade tip saving the user the time of locating those points manually.


**Depth Map** – 3D point cloud viewing mode in which either the tip to target distance (full image) or the perpendicular distance from the measurement reference plane (measurement image) is color coded to provide better understanding of 3D data noise levels and surface contours.


**Depth Profile View** – Alternative 2D view of depth profile along a selected depth profile line.


### F


**FOD** – Foreign Object Damage.


**Full Point Cloud** – Shows all measurements and surface data. The depth map indicates the distance from the measurement tip to the surface.


### G


**Guide Lines** – Lines shown in the point cloud views when using a Measurement Plane with Length, Point to Line, Multi Segment, or Area to help the user position cursors to align with out-of-plane surface points.


### I


**Image Brightness** – As it relates to a video borescope is a combination of several components, not just available light


- For live video, the image brightness control adjusts the brightness set point for the camera’s auto gain and exposure control processing
- For still images, the image brightness control digitally adds or subtracts an offset from the image’s RGB values


**InspectionWorks Connect** – Remote collaboration tool allowing live remote viewing of inspection video, two–way chat and annotation. Can be activated directly on the Mentor iQ handset.


**Inactive Cursor** – Cursors not currently selected, indicated by a green circle. Insight for where data can be pushed and remeasured by SMEs.


**Inspection Manager** – PC–based re-measurement tool for visual inspection images.


### M


**Measurement Point Cloud** – Shows only the active measurement and surface data in its vicinity. The depth map indicates the perpendicular distance of the surface points from the measurement’s reference plane.


**Missing Corner** – The corner of a turbine blade that has been broken off usually by an object passing through the turbine.


**MTD** – Maximum target distance. Identifies the distance of the furthest cursor point in a given measurement from the tip of the probe.


### N


**Noise in the Point Cloud** – Artifacts in the 3D data not representative of true surface geometry. Noise is generally reduced by moving the tip closer to the target surface or changing angle of approach to reduce reflections.


### O


**Orange Outline Around Measurement Results and MTD Value** – Indicates that the measurement is too small to be reliable for the current tip to target distance. Can be remedied by moving tip closer to measurement or selecting larger measurement area.


### P


**Probability of Detection (POD)** – “ *POD is the probability that a flaw of a specific type will be detected by a given technique given the flaw exists in the material.* ” Bill Prosser, NASA
You may ask, “What is Waygate Technologies’ POD on the Mentor Visual iQ (MViQ) VideoProbe™? And, how will it help me during inspections and indication analysis?”
In essence, POD is a combination of technologies which allow for the transformation of a white light image into one which will facilitate assessing the health of the visual surface, provide additional information about a surface’s condition, and if & when an indication, or “flaw”, is detected these tools are available to provide greater precision and accuracy when deploying Real3D™ Phase and Real3D Stereo Measurement - both of which provide an XYZ pivotable point cloud for analysis.


**Profile Slices** – Paths along the surface between the reference profile lines in an Area Depth Profile Measurement that map the perpendicular distance between the surface points and the reference surface.


**Projected Plane Measurement** – The use of a Measurement Plane in combination with another measurement type to mathematically project the plane of a surface over the entire image and measure on that plane.


### R


**Red Filled Cursors on Depth Profile** – Indicates cursor is not on a flat surface or not on the same plane. Cursor must be repositioned for accurate measurement.


**Red Mask on Image** – Indicates surface data in this area is not available for measurement. Moving closer or adjusting viewing orientation may reduce red mask area.


**Reference Line** – In a point to line measurement, the first two cursors, placed on unaltered surface points, define a straight reference line in 3D space from which the distance to the third cursor point is measured.


**Reference Plane** – The plane, defined by three or more points on a reference surface, from which the perpendicular distance to other surface points is computed. A blue square indicates the reference plane position in the measurement image 3D point cloud view.


- *Note* : A reference plane is calculated as flat, even if the surface the plane rests on is not flat. This is a potential source of error.


**Reference Profile Line** – Area Depth Profile lines set by the measurement cursors which follow the surface contours and are used to determine the area and reference planes for the profile slices.


**Reference Surface** – Unaltered part surface to be used as a reference for various measurements.


**Result Slice** – The Area Depth Profile profile slice that includes the highest or lowest point and is shown on the image and in the point cloud.


### S


**Signal Strength Range Guide** – Visual aid displayed when viewing live video with a 3DPM tip attached to indicate the system’s ability to achieve sufficient brightness with the tip LEDs for a high-quality scan. Increasing the number of bars by moving closer to the target will increase 3D data quality and measurement accuracy.


**Surface Mask** – One of several masks coloring pixels on a surface with various color gradients to indicate various surface contours or distances between cursors.


### Y


**Yellow Mask on Image** – With 3D Phase Measurement, indicates surface data may be of low quality for measurement. The potential for reduced accuracy may exist, especially for depth or small measurements. Yellow mask areas are often caused by surface to surface reflections. Changing the probe orientation to make such reflections bounce away from the probe tip may improve data quality and reduce or eliminate yellow mask areas.


---


---


The VideoProbe™ of the Future


The Mentor Visual iQ is changing the way remote visual inspections are being performed. Leveraging artificial intelligence and cloud-based digital tools to help improve productivity, accuracy, and reliability of inspection data.


Complete the form to request an onsite or virtual product demonstration by one of our RVI experts.


[Request a Demo](https://www.bakerhughes.com/wt-remote-visual-inspection-request-demo?utm_source=mviq+prod+page&utm_medium=hero+cta&utm_campaign=mviq%2B)


---
