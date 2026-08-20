---
schema_version: "1.0.0"
document_id: "5aa7ee8ee92062fd6787823b4161f8e2e9b0340fecffa9b75012ba410f90586e"
company_key: "mobileye-global-inc-class-a-common-stock"
company: "Mobileye Global Inc."
source_id: "mobileye-global-inc-class-a-common-stock-news-import-c713d153ef4c"
canonical_url: "https://www.mobileye.com/blog/how-mobileye-redefines-camera-design-with-dual-hdr-image-capture/"
published_at: "2025-12-09T06:00:00+00:00"
first_seen_at: "2026-07-24T11:27:24.879644+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:bf92ad66eba5ed7ca7055c19ad810ae18dad2a589b0ef620cbb1308767db638e"
---

# How Mobileye reimagines camera design with dual HDR image capture

Automotive hardware and software constantly intersect, and the need for technology that unites them more effectively has never been greater. Camera design is a strong example of this industry-wide need.


At Mobileye, developing technologies that support this integration is a core focus. By proposing a different type of camera architecture, Mobileye is bringing forth an approach that rethinks how high dynamic range (HDR) scenes can be captured.


Developed by Mobileye for use with select sensor suppliers and intended for vehicle integration, this approach is designed to address one of automotive imaging’s most persistent challenges: balancing exposure time, motion, light and distance.


### **The exposure dilemma in automotive cameras**


There are a lot of factors to consider when it comes to automotive imaging. One of them is ensuring that a camera can capture images clearly, with the right amount of exposure, all while on a fast-moving vehicle, and at various levels of lighting.


At nighttime or in low light conditions, there are a few challenges to navigate. As we know, in limited lighting, longer exposure is needed to brighten distant or poorly lit objects. This, however, increases motion blur.


On the other hand, shorter exposure keeps moving objects sharp but reduces visibility in darker scenes. The result is in both cases is often lost data and lower image quality.


During daylight, exposure challenges can arise too. This often happens with LED traffic lights. In daylight, a camera’s sensor operates at a shorter exposure to reduce motion blur, but that timing doesn’t always align with the LED’s pulsing cycle. Because LEDs pulse rapidly, the human eye sees steady light, but a camera might capture the light when it’s off, resulting in the appearance of LED flicker.


### **The solution – dual HDR image capture**


The key question is how to manage the exposure trade-off without adding unnecessary complexity. Adding a second camera that can capture HDR images at a different exposure to resolve this conflict is possible, but that adds cost and software demands, making it difficult to scale. As an alternative, Mobileye has developed a “dual HDR image capture” technology.


By enabling a single camera to produce two HDR images with only a negligible gap between captures, this method is intended to allow the camera to provide two separate HDR images at different exposures that are read out from the camera, within the same frame time.


This provides algorithms with richer visual data for scene understanding. As a result, the camera can capture both an image with longer HDR exposure and an image with a shorter HDR exposure of the scene almost simultaneously without needing a second camera.


But while this process increases the number of HDR images, it doesn’t inherently require double the processing needs. Rather than transmitting two full 8MP frames, one of the frames can be resized to a lower resolution, helping reduce MIPI bandwidth and manage the overall data load.


The solution is intended to:


- Improve low light images by up to ~2x


- Reduce motion blur at night by up to ~5x


- Reduce LED flicker in daytime scenes with minimal impact on the detection of fast-moving objects while driving


**Such improvements are achievable under optimal operating and driving conditions.*


For sensor suppliers, this can offer


s


a practical path to support improved imaging capabilities. For automakers, it can provide more consistent environment information to support ADAS performance without adding system burden. Overall, this solution is intended to help improve imaging quality and efficiency in automotive systems.


#### *Disclaimer: The dual capture approach was developed by Mobileye for collaboration with select image-sensor suppliers and is intended to support potential integration into automotive systems.*
