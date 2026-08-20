---
schema_version: "1.0.0"
document_id: "5927dc71cf85e69f3847cc4eb4441bd41260fda6fc0333e155d42599f4a3fb01"
company_key: "tower-semiconductor-ltd-ordinary-shares"
company: "Tower Semiconductor Ltd."
source_id: "tower-semiconductor-ltd-ordinary-shares-rss-873c538285d0"
canonical_url: "https://towersemi.com/2020/01/30/light-camera-very-fast-action/"
published_at: "2020-01-30T10:17:53+00:00"
first_seen_at: "2026-07-20T04:36:11.406925+00:00"
fetched_at: "2026-07-28T22:26:48.304506+00:00"
content_hash: "sha256:f5a9b5574064859c812d9d0721fc3f5e1e949d531ad183cec3790628fb1e10ce"
---

# Light, Camera, (very fast) Action!

These well-known limitations of RS based sensors and cameras led image sensors engineers worldwide to design sensors with a more robust architecture, in which the image is captured simultaneously (or globally) over all the pixels of the sensor. This sensor architecture is known as Global Shutter (GS). The main drivers for developing GS sensors technology are:


**(1)** the growing industrial market where cameras are used to monitor or inspect fast moving objects in production lines (e.g. reading bar code labels accurately and fast);


**(2)** the automotive market where GS sensors are needed for creating better images from moving cars, automatic registration of cars license plates in highways toll stations, parking lots, real time monitoring of roads, and automatic speed registration and ticketing; and


**(3)** the last and biggest volume driver – the commercial applications like sports cameras, drone cameras and face recognition for edge devices, where a laser flash is used to project a grid of dots on one’s face and the camera needs to be fully synchronized with the projected pattern.


In a standard RS camera, each pixel in the sensor has a Photo Diode (PD) acting dually as a photo sensitive element and also as an information storage element for readout element. The pixel designer’s task is to maximize the number of photons that hit the photosensitive area while the readout is coupled (almost) directly to the information which is also stored in the PD. The GS pixel on the other hand is considered best-in-class when the PD is fully de-coupled from the read-out element. This often requires an additional element, called a Memory Node (MN), in each pixel which can store the relevant signal information until it is read out. Unlike the PD, which needs to be very sensitive to light and translate as many photons into electrons, the MN needs to be isolated as much as possible from incoming light in order to avoid any ‘mixing’ between information coming from two different frames. The stored image is the information from the previous frame which is now being read out, still line after line, while the new incoming photons should contribute to the signal of the current frame.
