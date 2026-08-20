---
schema_version: "1.0.0"
document_id: "f9a6fe7690334e965d010807dae62490b67a508f354dfc019acf397cc57ec60f"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/sheet/how-to-measure-laser-beam-quality/"
published_at: "2023-03-09T05:00:03+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:7a8c21d737fe928859315cc21ef3f85172a65c6285b31837a741d8c5a181bf62"
---

# How To Measure Laser Beam Quality With M2 Measurement

M² measurement is a standardized method for quantifying the quality of a laser beam. It compares the actual beam to an ideal Gaussian beam, where M² = 1 represents a perfect Gaussian profile. ISO 11146 standard defines how to measure the beam width, divergence, and M² beam quality factor of laser beams. According to ISO 11146, the procedure involves preparing the laser beam, focusing it with appropriate optics, and measuring its radius at multiple positions along the propagation axis. These measurements are then fitted to mathematical models to determine the beam waist, Rayleigh length, and ultimately the M² value. From the beam radius (as a function of propagation distance) and the waist location, the M² value can be calculated. This value indicates the relative beam quality and the laser's ability to be focused.


The following steps outline the recommended procedure for performing an M² measurement:


## 1. Start With a Collimated Beam


For an accurate M² measurement, it is essential to begin with a well-collimated beam. Collimation ensures the laser rays are parallel before entering the focusing optics, minimizing errors in subsequent measurements. A collimating device—typically a lens or a collimator assembly designed for the laser’s divergence and wavelength—is used to align the rays into a parallel beam. Fiber-coupled sources often use collimation packages, while free-space lasers may require adjustable collimating lenses.


## 2. Use an Aberration-Free Lens To Focus the Laser Beam


The next step is to focus the collimated beam with a lens of known focal length. The lens should be as close to aberration-free as possible, meaning it should not introduce distortions such as spherical aberration, astigmatism, or surface imperfections. Optical-grade lenses with high-quality coatings are recommended, as any aberrations would distort the beam waist and compromise the accuracy of the M² calculation.


## 3. Measure the Diameter of the Beam at Various Positions Around the Focal Point


The beam waist is the minimum radius of the beam. This will be the radius of the beam at the focal point of the laser. However, due to inaccuracies in field measurements, it is not possible to precisely measure this beam waist at a single point. Instead, according to ISO 11146 for testing laser beams, it is necessary to take multiple measurements around the known focal point and find the minimum of these measurements to establish the beam waist.


At least 10 data points of beam radius should be taken. About half of these points should be taken close to the focal point (within one Rayleigh length of the focal point), and the other half should be taken further away from the focal point (more than two Rayleigh lengths away). These measurements will be of the laser beam width in both the x direction and the y direction (as the beam may not be perfectly circular).


## 4. Use the Regression Equations Detailed in the ISO Norm to Fit a Hyperbola to Your Data Points for Both the X and Y Axis


The reason for taking data points at different distances from the laser beam focal point is to fit a hyperbola closely to the plotted data. The ISO norm specifies the regression equations to be used for fitting this hyperbola. The data points should be plotted showing the beam radius against the distance from the lens. The hyperbola will show the minimum beam radius, and therefore give the beam waist. A hyperbola should be plotted for both the x-direction and the y-direction.


## 5. Extract the Values for, w0, zR, and M2 for Each Axis


Using the hyperbola fitted to your data points, the minimum beam radius is easily shown as the beam waist (w0). The Rayleigh length (zR) can be calculated as the distance at which the laser radius is greater than the beam waist by a factor of the square root of 2. For a circular beam, this is the distance at which the cross-sectional area is twice the minimum at the beam waist. These values can be determined for a laser beam in both the x-direction and the y-direction.


Finally, with the beam waist known, the M2 factor can be determined by calculation. It is calculated by using the following equation:
