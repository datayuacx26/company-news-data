---
schema_version: "1.0.0"
document_id: "c444925010948edd3ab249d87bd43ebd0f780be8bcf19cdaf19971c769090e3c"
company_key: "baker-hughes-company-class-a-common-stock"
company: "Baker Hughes Company"
source_id: "baker-hughes-company-class-a-common-stock-news-import-09e3d3106256"
canonical_url: "https://www.bakerhughes.com/waygate-technologies/blog/which-3d-stitching-options-are-available-mentor-visual-iq-videoprobe"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-07-21T09:24:28.311413+00:00"
fetched_at: "2026-08-18T01:01:13.723404+00:00"
content_hash: "sha256:b570dc29e8d5c1c74dd149ddfa83e21b4a6623f8a88cd4840025db81f72cb247"
---

# 3D stitching options on the Mentor Visual iQ+ VideoProbe

---


## Which 3D stitching options are available on the Mentor Visual iQ+ VideoProbe?


Inspect, interrogate (measure) and report more of the scene. Ability to combine up to ten images (3D Phase or 3D Stereo) to create a composite image is now possible on the device. Reduce measurement uncertainty by capturing higher quality images and performing measurements on the overall composite image.


3DS can be undertaken during a given inspection or from recalled images captured earlier. Images must however be captured from the same probe and tip in order to successfully stitch.


## Example Applications:


- Large defect measurement assessment
- Establishing defect location relative to fixed positions on the component
- Creation of stitched image for reporting purposes


## Disclaimer:


The quality of the 3DS process is dependent on a number of factors. You, the User, are responsible for checking the quality of both individual and composite images to ensure the best possible outcome. Please refer to the General Guidelines to learn more.


The factors that have the greatest effect on 3DS are:


- Distance from target
- Visual features contained within the scene
- Image overlap
- Target material composition


## Best Practices to Improve 3DS Accuracy:


### Technology Choice


- Both 3DPM and 3D Stereo can be used.
- The 105° FOV of 3DPM tips allow a wider scene to be viewed and measured from a given distance than 3D Stereo (50 or 60° FOV dependent on tip used).
- 3D Stereo may provide better results than 3DPM in compressors having shiny blades and liner.


### Setup


- Position the probe tip as close as possible to the target surface.
- Images must have at least 50% overlap. Too little overlap often results in failed or inaccurate stitching.
- Surface must have unique details that look similar from one image to the next.
- Surfaces that have a shiny or reflective appearance that changes as the probe tip moves position can be problematic. Increasing overlap can help.
- Avoid excessive (c.20°) rotation steps between image captures.
- Avoid excessive (c.30%) magnification steps between image captures.
- Clean data helps – minimize MTD. Use Green or Yellow 3DPM tip where possible.
- Always check registration or stitching quality before proceeding and measuring.


### Checking Registration Accuracy


- During the capture and subsequent stitching of images, it is possible to intermittently check the registration or quality of stitch before proceeding and capturing more images or subsequent measurement assessment.
- This is an important process to ensure accuracy of measurements on stitched images.
- Watch surface details while using ‘Next/Previous Image’
- If well registered, surface details will move very littlebetween images
- If poorly registered, details will shift.


### Signs or indicators of poor image registration are:


- Clear gaps between images
- Surfaces offset from one another
- Surface features are not in the same location when switching between images.


## 3D Stitching – Orange Mask


Orange mask is shown when the device is presenting individual 2D images. The mask represents the areas of the image that do not contribute to overall composite point cloud and are not used for measurement.


All three images are used to generate the composite stitched image but some areas (shaded orange) are discarded and not used. The primary image contains the least orange masked area. Selection of an individual 2D image and subsequent measurement may result in displaying an orange measurement cursor. This cursor highlights it being derived from a different image within that stitched image set. Note: normal measurement cursors are coloured green (when inactive). See below.


## 3D Stitching – Occluded Cursors


On surfaces with significant steps, some parts of the surface may be visible in one image but hidden by a closer surface in a different image.


When a cursor is placed on a surface point that is occluded by a closer surface in the current 2D image, the cursor is shown in orange and a message is displayed.


Saving of full screen 2D images is disabled when stitching to reduce risk of misinterpretation of the position of occluded cursors on the 3D surface. See images below.


## Perspective Lines and Tip Model (not exclusive to 3D Stitching)


These are only shown when a measurement is present and ‘full image’ enabled in order to reduce visual intrusiveness. They can be activated/deactivated within Settings > Measurement Annotation.


With 3D Phase images, a tip model can be enabled via ‘Options’ 2nd level soft key. This provides visual reference of the probe tip in relation to the target surface.


---
