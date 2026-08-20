---
schema_version: "1.0.0"
document_id: "fb5765d2a4d3029928a90566ead6ff9d646e9a55a2ed1dd5686bf35f3f1edde2"
company_key: "yc-fluke"
company: "fluke"
source_id: "yc-fluke-news-import-b5cacff74b18"
canonical_url: "https://www.fluke.com/en-us/learn/blog/temperature-calibration/infrared-temperature-calibration-101"
published_at: "2024-07-01T00:00:00+00:00"
first_seen_at: "2026-07-25T05:28:17.177808+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:f33ed884039ea501a77ae6224f827c192e600df0e857adf4fe6135155e13e616"
---

# Infrared Temperature Calibration 101

# Infrared Temperature Calibration 101


[Temperature calibration](https://www.fluke.com/en-us/learn/blog/temperature-calibration)


## Using the right tool means better work and more productivity


Infrared thermometers let you measure a target’s surface temperature from a distance without physically touching it. To point and shoot at a target to measure its temperature may seem a little like magic. This application note demystifies the process of infrared (or “IR”) thermometry and explains why regular calibration is important for maximizing the value of these useful devices.


## How infrared thermometers measure temperature


Infrared thermometers measure the electromagnetic radiation emitted by an object as a result of the object’s temperature. Until an object becomes very hot, most of this radiation is in a band of wavelengths called the infrared spectrum. Very hot objects emit visible light which is also a form of electromagnetic radiation.


While the human eye is very sensitive to yellow light with wavelengths around 0.555 micrometers, it cannot detect light with wavelengths longer than 0.700 micrometers (red) and shorter than the 0.400 micrometers (violet). Although our eyes can’t detect the energy outside of this narrow band of wavelengths, called the visible spectrum, we still know it is there, because we can detect it with a radiometer.


Note that a micrometer is very small; in fact, it is one one-millionth of a meter. To put that into perspective, around 100 micrometers would equal the thickness of a human hair.


### “Seeing” temperature


We have some experience measuring temperature by detecting electromagnetic radiation with our eyes. We are familiar with things that “glow hot,” such as the red glow of the embers of a fire, the yellow glow of a candle, and the white glow of an incandescent light bulb. The color we perceive is related to the temperature of the heated object. In fact, steel workers claim that they can accurately judge the temperature of molten steel to within 50 °C based on the color alone.


Like the eyes of steel workers, infrared thermometers are also designed to be sensitive around a specific band of wavelengths. The most commonly used spectral band for general purpose infrared thermometers is from 8 µm to 14 µm (8 to 14 micrometers).


Infrared radiation is electromagnetic radiation with wavelengths longer than visible light and shorter than millimeter wave radiation. Terms like wavelength and amplitude are used to describe infrared and other forms of electromagnetic radiation. For example, wave amplitude describes the intensity of electromagnetic radiation and wavelength is used among other things to determine whether it is a microwave, visible light, or infrared radiation.


## Infrared temperature calibration


### **Temperature source**


In spite of our experiences with seeing temperature, confidence in IR temperature measurement usually requires the use of calibrated instruments.[Calibration](https://www.fluke.com/en-us/learn/blog/calibration/about-calibration) can be defined as a set of operations, performed in accordance with a definite, documented procedure, which compares the measurements performed by an instrument to those made by a more accurate instrument or standard, for the purpose of detecting and reporting, or eliminating by adjustment, errors in the instrument tested.


An IR temperature calibration starts with a measurement surface that acts as a temperature source, which may be a flat plate or cavity that functions as the standard or reference. Calibration geometry, which includes the size of the measurement surface and its distance from the thermometer being calibrated plays an important role in the measurement result. Also critical are the temperature stability and uniformity and the physical properties of the emitting surface such as its emissivity.


### **Converting input temperature to radiance**


The measurement surface of an[infrared calibrator](https://www.fluke.com/en-us/products/calibration-tools/temperature-calibration/industrial-calibrators) acts as a transducer by converting thermal energy into thermal radiation. The intensity of a portion of the infrared radiation emitted by the measurement surface is measured by the infrared thermometer to calculate a temperature. The measurement surface is analogous to the sensor of an RTD which transduces thermal energy into a resistance that is measured by a[readout device](https://www.fluke.com/en-us/products/calibration-tools/temperature-calibration/digital-thermometer-readouts) and used to calculate a temperature. It is interesting to note that the sensor is responsible for most of the error in a temperature measurement, which explains the importance of calibrating the temperature sensor. One of these sources of error in the measurement surface and perhaps the largest in an infrared temperature calibration is emissivity.


### **Emissivity**


The radiant energy coming from an opaque target is a combination of emitted radiance caused by the target’s temperature and reflected radiance coming from elsewhere in the environment. Transmission through the object is another source of radiant energy that must be considered when objects are not opaque. The amount of light emitted at a particular temperature is determined by the emissivity of the surface. Emissivity is the ratio of the radiant energy emitted by a surface to that emitted by a blackbody at the same temperature. Emissivity is greatly affected by the type of material and surface finish of an object. Metals with smooth surface finishes tend to have low emissivity and high reflectivity while long narrow holes have relatively high emissivity and very little reflectivity. The sum of emissivity, reflectivity, and transmission is always equal to one.


[Infrared temperature calibrators](https://www.fluke.com/en-us/products/calibration-tools/temperature-calibration/industrial-calibrators) must be designed to have a known emissivity, which must remain constant over the full operating temperature. Unfortunately, emissivity is neglected in the calibration of most IR calibrators. These calibrators are themselves calibrated by inserting a contact thermometer such as a[platinum resistance thermometer (PRT)](https://www.fluke.com/en-us/products/accessories/probes) physically into the target. This technique also neglects temperature losses at the surface of the infrared target. With this type of calibration, the user may not be aware that complicated emissivity-related corrections are required at each temperature in order to achieve the accuracy claimed by the manufacturer.


These corrections are based on the difference between the actual measured emissivity of the target and the emissivity setting of the thermometer. A one percent error in the emissivity at 500 °C would result in a 3.5 °C error in the calibration. On the other hand, emissivity values offered by manufacturers are usually typical values only and are not actually verified by calibration. This can lead to a loss of traceability and inconsistent results over time and inconsistent results among different calibrators.


To correct for temperature errors in the measurement surface, an infrared temperature calibrator should be calibrated with a radiometer, which measures the amount of radiant energy coming from the calibration target at each temperature. A temperature display that has had a radiometric calibration does not need additional emissivity-related temperature, unless the emissivity setting of the thermometer does not match the calibrator. Look for a calibrator that can compensate for the emissivity settings of infrared thermometers.


As shown in the Spectral Radiance and Temperature graph below, when temperature causes an object to emit light, the light comes out in many different wavelengths. This is called spectral radiance (see graph below). If you could line up all the waves from shortest to longest, you would see that the brightest waves are somewhere in the middle. If you then increased the temperature of the object emitting the light, you would notice that the shorter waves were getting brighter than the longer waves. When the object becomes hot enough, even the very short waves from 0.400 µm to 0.700 µm start to be bright enough that the radiation emitted by the object is visible to the human eye and the object appears to glow. The emissivity at each wavelength, called spectral emissivity, determines how bright each wave will be. The thermometer uses the total brightness of a selected band of these waves to determine the temperature.


### **Emissivity, blackbodies and graybodies**


Emissivity can be any value between and including zero and one. Zero emissivity means that no matter what an object’s temperature, no light will be radiated. An emissivity of one means that the surface will radiate perfectly at all wavelengths. Scientists give these special perfectly radiating objects the name “blackbody.” Objects with emissivity very close to one are also usually called blackbodies. A flat plate calibrator with an emissivity around 0.95 is referred to as a graybody if the emissivity is uniform across all wavelengths.


### **Detecting radiance with a thermometer**


The radiated light energy from the calibrator’s surface is transmitted through the air until it is detected by a thermometer. Dust, smoke and glass surfaces can impede the light and affect the results of the measurement. The detector has to collect the light from the precise area of the intended target to avoid measuring unintended objects in the background. The distance-to-spot ratio (D:S) assists the user in finding the correct position of the thermometer.


However, spot size is only a percentage of the thermometer’s entire field of view. Like the human eye, an infrared thermometer has some peripheral vision. Peripheral vision in IR thermometers sometimes called scatter, can account for between 1 % and 35 % of the total measured energy, depending on instrument quality. While the spot size may be adequate for practical field measurements, it is not always adequate for the laboratory accuracy required during a calibration. That is why calibrating most IR thermometers requires a target that is significantly larger than the size suggested by the distance-to-spot ratio of the thermometer.


The infrared energy collected by the thermometer will be filtered by its optical system, which is only sensitive to a particular spectral band such as 8 µm to 14 µm. Included in the collected energy is reflected radiation caused by the ambient temperature in the room. Reflection is related to emissivity in an interesting way. The detectable infrared radiation coming from the calibrator is usually a combination of emitted thermal radiation from the measurement surface and reflected energy from other warm objects in the environment, such as windows, walls, and people. For an opaque object, emissivity plus reflectivity is equal to one.


Consequently, if the emissivity of an opaque target is zero, then none of the radiated energy coming from that target is caused by its own thermodynamic temperature. All of the light detected by the thermometer is radiation reflected from the target that originated elsewhere, striking it from elsewhere in the room. If the target’s emissivity is 0.95, then its reflectivity is 0.05. In other words, the target absorbs 95 % of the energy in the room and reflects the other 5 %. It also means that the energy emitted by the target caused by temperature is 95 % of what would be expected from a perfect blackbody. IR thermometers attempt to compensate for the energy reflected by a target; however, when a target becomes very cold compared to the environment, or if the emissivity is very low, then the reflected energy may make it difficult to read the correct temperature. This is because the reflected energy is a relatively large proportion of the signal received by the thermometer when the target temperature is below ambient temperature. This situation is often referred to as a low signal-to-noise ratio.


IR thermometers measure a group of wavelengths called a spectral band. IR thermometers are spectral band thermometers because they measure spectral radiance, the collective radiance of all of the wavelengths inside a particular spectral band, such as 8 µm to 14 µm.


### Calculating temperature from radiance


Finally, an accurate temperature calculation requires the emissivity setting of the thermometer, to match the real emissivity of the objects being measured. Alternatively, in the case of a target with a radiometric calibration, the thermometer’s emissivity should match the effective calibrated emissivity of the target, so that a direct comparison can be made between the infrared temperature of the calibrator target and the infrared thermometer under test.


## Tech tip


If the target is at a temperature below the dew point, ice may form in crystal patterns that will cause the emissivity of the surface to change, introducing errors into the calibration. Purging with dry gas is one method of preventing the growth of crystals which eventually may form a sheet of ice that actually masks the temperature of the target, creating even larger errors.


## Conclusion


Infrared thermometers are used in a variety of situations where contact measurements are impractical. Applications involving these useful devices are often misunderstood, leading to a reduction in the usefulness of the resulting measurements. But confidence in these measurements increases with calibration. When the stakes are high, or the application is important, this additional confidence is well worth the investment in regular testing or calibration of the IR thermometer. All calibrations are not created equal, and the right equipment choice can make all the difference. When choosing a calibrator, look for one with a large enough target to accommodate the thermometer’s peripheral vision. You probably need the same size target the manufacturer uses for its own calibrations; thermometer manufacturers also recommend using the same calibration distance that they do to achieve laboratory levels of accuracy in a calibration. If your calibrator does not have a radiometric calibration, you need to know the emissivity of its target so you can calculate the appropriate corrections. These calculations are difficult, so getting the right calibration to begin with makes a big difference. Similar calculations must be made if the emissivity settings on the thermometer do not match those of the calibrator, and that is why a well designed calibrator that can make those calculations for you saves worry and time that you can spend being productive.


## Glossary


### **Absorption:**


The process of converting radiation energy that falls on a surface into internal thermal energy.


### **Blackbody:**


A blackbody is an ideal surface that emits and absorbs electromagnetic radiation with the maximum amount of power possible at a given temperature. Such a surface does not allow radiation to reflect or pass through it. In the laboratory a blackbody is approximated by a large cavity with a small opening. Reflection is prevented because any light entering the hole would have to reflect off the walls of the cavity multiple times, causing it to be absorbed before it could escape.


### **Dew point:**


The dew point is the temperature to which air must be cooled, at a given barometric pressure, for water vapor to condense into water.


### **Distance to spot ratio:**


The distance-to-spot ratio (D:S) is the ratio of the distance to the object and the diameter of the area containing a specified percentage of the total energy picked up by the infrared thermometer. The D:S ratio is used as a guide to determine the appropriate distance for making practical infrared temperature measurements.


### **Electromagnetic radiation:**


Energy emitted by a surface that travels through space as a wave with electric and magnetic components. Examples include: radio waves, microwaves, millimeter wave radiation, infrared radiation, visible light, ultraviolet radiation, X-rays and gamma rays.


### **Emissivity:**


The emissivity of a surface indicates how efficiently it emits radiation compared to a blackbody at the same temperature. It is measured by the ratio of energy radiated by the material to the energy radiated by a blackbody at the same temperature.


### **Field of view:**


The field of view (FOV) is the region in space containing a specified amount of the radiant energy collected by the optical system of an infrared thermometer. FOV is usually expressed in angular degrees (e.g., 1 °). The measurement surface must completely fill the thermometer’s field of view to ensure accurate temperature measurements.


### **Graybody:**


A surface that emits radiation with constant emissivity over all wavelengths and temperatures is called a graybody. Although graybodies do not exist in practice, they are a good approximation for many real surfaces.


### **Infrared thermometer:**


A device that calculates the temperature of an object by measuring the infrared radiation emitted by the object. It is sometimes called a non-contact thermometer because of its ability to measure the temperature of a surface without making contact with that surface.


### **Infrared radiation:**


Infrared (IR) radiation is electromagnetic radiation with wavelengths longer than visible light and shorter than millimeter wave radiation. All surfaces with temperatures above absolute zero (-273.15 °C) emit infrared radiation.


### **Opaque:**


An opaque object does not allow electromagnetic radiation to pass through it. A surface may be opaque for some wavelengths and transparent for others. For example, glass is opaque to infrared radiation with wavelengths longer than ~3 microns but is transparent to visible light.


### **Radiance:**


A measure of the intensity of electromagnetic radiation emitted by or passing through a particular area in a specified direction. It is defined as power per unit area per unit solid angle. The SI unit of radiance is watts per steradian per square meter (W·sr--1·m-2).


### **Radiometric calibration:**


A calibration that compares the measurements made by an instrument to the measurements made by a reference radiometer, for the purpose of detecting and reporting, or eliminating by adjustment, errors in the instrument tested.


### **Radiometer:**


A device used to measure the power, also called radiant flux, in electromagnetic radiation. See also infrared thermometer. Reflection: Electromagnetic radiation is reflected when it changes directions at a surface while remaining in the first medium without passing into the second.


### **Reflectivity:**


The fraction of incident radiation reflected by a surface. Scatter: An effect caused by non-uniformities in a medium that forces light to travel in a direction that deviates from a straight path.


### **Spectral band:**


A well-defined, continuous wavelength range electromagnetic energy. The range from 8 to 14 microns used by many infrared thermometers is an example of a spectral band.


### **Spectral radiance:**


A measure of the intensity of electromagnetic radiation at a specific wavelength emitted by or passing through a particular area in a specified direction. It is defined as radiance per unit wavelength. Spectral radiance has SI units W·sr--1·m-3 when measured per unit wavelength.


### **Spot size:**


The diameter of the measurement spot of a radiometer. It is usually defined as the diameter that collects a defined percentage of the total power collected by the radiometer, e.g., 95 %. This spot size will vary with distance from the radiometer.


### **Thermodynamic temperature:**


A temperature value based on a scale that is consistent with all the known laws of thermodynamics.


### **Transmission:**


The process by which some fraction of electromagnetic energy reaching a surface is allowed to pass through the material belonging to that surface. Transmission assumed to be zero for surfaces that are opaque at the measured spectral band.


### **Transmissivity:**


A measure of how much of an electromagnetic wave passes through a surface. It is calculated by taking the ratio of the value that passes through the surface to the value that reaches the surface.


### **Uniformity:**


A uniformity specification is an expression of the maximum allowable difference between two measurement results within the region contained by a measurement medium or surface.


### **Wavelength:**


The distance in the direction of a moving wave that it takes to complete a single cycle. In a wave, a property such as electric potential varies cyclically with position. In a moving wave the position also changes with time.


**Keep learning**


- [How to Calibrate an Infrared Thermometer](https://players.brightcove.net/5229431846001/default_default/index.html?videoId=6212025633001)
- [How to Create an Infrared Thermometer Uncertainty Budget](https://players.brightcove.net/5229431846001/default_default/index.html?videoId=6217880183001)


**Get Help**


- [Customer & technical support](https://www.fluke.com/en-us/support/technical-support)
- [Speak with a calibration product expert](https://forms.fluke.com/get-assistance) about your equipment needs


## You might also be interested in


Article


### [How to choose a heat transfer fluid for your calibration bath](https://www.fluke.com/en-us/learn/blog/temperature-calibration/how-to-select-calibration-bath-fluid)


[Temperature calibration](https://www.fluke.com/en-us/learn/blog/temperature-calibration) ,[Calibration](https://www.fluke.com/en-us/learn/blog/calibration)


Article


### [Temperature calibration equipment: 4 key bath specifications](https://www.fluke.com/en-us/learn/blog/temperature-calibration/buying-the-right-temperature-calibration-bath-for-your-laboratory)


Article


### [Calibration bath selection guide](https://www.fluke.com/en-us/learn/blog/temperature-calibration/calibration-bath-selection-guide)


Article


### [How to choose a temperature bath calibrator](https://www.fluke.com/en-us/learn/blog/temperature-calibration/choose-calibration-bath)


Chat with our


Fluke assistant


Clear Chat


[Contact Us](https://www.fluke.com/en-us/support/about-us/contact-us)


[Get 10% off](https://www.fluke.com/en-us/support/about-us/contact-us)
