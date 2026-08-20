---
schema_version: "1.0.0"
document_id: "8e98deedcfb1951fd86b9b8e5e6d624e7b42ea6cce530c13ec7c68a3ec4da793"
company_key: "yc-biodock"
company: "Biodock"
source_id: "yc-biodock-rss-567b1dbd4991"
canonical_url: "https://blog.biodock.ai/gradient-index-lenses/"
published_at: "2022-09-07T20:27:55+00:00"
first_seen_at: "2026-07-24T19:19:16.186046+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:586057955fbf410acb32ef65ff5b7849837fde8a28d0ccc944d4370debcd712b"
---

# Peering beyond the surface with Gradient Index lenses

Imagine a New Yorker is gazing out a city window onto the street below. Cars are whizzing by, people are strolling past, and skyscrapers scaffold the horizon. With each passing second of observing this landscape, hundreds of millions of photons are reaching your eye of which some are activating rods and cones of the retina. As the New Yorker’s eye jets across the city scene, light from closer or further distances comes into focus seemingly in an instant. How does the eye do this? This process, called the accommodation, entails ciliary muscles surrounding the eye to contract and relax. This adjusts the curvature of the lens of the eye which alters how light diffracts when passing through it. When the curvature is greater, the light is diffracted more which focuses light from closer objects and vice versa. Accommodation constitutes an extraordinary biological mechanism for focusing our vision which leverages the physics of Gradient Index (GRIN) optics. Evolution harnessed GRIN optics in nature long before we learned how it worked. Now that we understand the physics behind GRIN optics, it ironically has become a useful tool to develop lenses of our own. Here, I will discuss how GRIN optics and imaging work, and how GRIN lenses are being used to peer into deep structures of the body and brain.


a diagram of accommodation of the eye where the lens is more or less convex to focus light from closer or farther distances, respectively.


Table of contents


1. How light behaves in gradient index optics and what it means


2. Developing gradient index lenses and coupled imaging technologies for endoscopy


3. What have we found using gradient index imaging?


4. Outlooks and conclusions


***Gradient Index Lenses consist of a radially varying refractive index which focuses entering light onto a point***


Before understanding GRIN lenses, one first should be acquainted with the basic principles of refraction. Refraction refers to the bending of light. When light travels through one medium to another, its speed through the medium changes resulting in a slight bend in the angle of the light travel (Figure 1a). This explains why placing a pencil in water appears to bend it when observed through a clear glass (Figure 1b). This is called the refraction angle and is a function of the[optical density](https://www.photonics.com/EDU/optical_density/d5788?ref=blog.biodock.ai) of the medium that light is passing through. Light travels slower through a medium with higher optical density and vice versa. The *[refractive index](https://www.youtube.com/watch?v=ZW74ZIn-CkE&ref=blog.biodock.ai)* is a measure of the optical density of a medium. Mathematically, this is represented as the ratio between the speed of light in a vacuum and its speed in the medium it is passing through.


**[Figure 1:](https://www.science-sparks.com/what-is-refraction/?ref=blog.biodock.ai) Principles of refraction** (a) light diffracts as it moved from one medium to another where the angle of diffraction is dependent on the optical density of the medium. (b) light diffracts as the optical density of water and air are different causing the light emitted from a pencil to appear distorted.


Thus, a gradient index lens is what its namesake suggests: a lens where its refractive index quadratically varies radially according to a gradient (Figure 2a). When engineered to the appropriate length, this allows light to be focused onto a point (Figure 2b). GRIN lenses can be machined as[small rod-like shapes](https://www.inscopix.com/lenses-viruses?ref=blog.biodock.ai#proview_integrated_lens) which can be coupled with various acquisition devices for endoscopy.


**[Figure 2](https://www.rp-photonics.com/gradient_index_lenses.html?ref=blog.biodock.ai) : Diffraction principles of GRIN Lenses** (a) the refractive index of a GRIN lens is graded such that it quadratically increases from the edge to the center of the lens. (b) Light (blue line) passing through a GRIN lens (gray shaded area) bends light differentially allowing light from multiple source locations to focus onto a single point


***GRIN lenses can be fabricated in small sizes and chronically implanted into intact tissue in live animals***


How do you create a lens where the refractive index varies appropriately? GRIN Lenses are fabricated through multiple different methods although the most common method is[ion exchange](https://www.frontiersin.org/articles/10.3389/fmats.2016.00041/full?ref=blog.biodock.ai) . In this process, the glass is submerged in a liquid solution where ions in the glass can be replaced with ions in the liquid. For example, switching a sodium ion for an ion with the same electrical charge but higher molecular weight such as potassium can increase the optical density of the medium. This process is used beyond the fabrication of GRIN lenses in creating tougher glass (Figure 3). Other methods for GRIN lens fabrication include[partial polymerization](https://www.nature.com/articles/pj19953.pdf?proof=t&ref=blog.biodock.ai) and[direct laser writing](https://www.nature.com/articles/s41377-020-00431-3?ref=blog.biodock.ai) .


[Figure 3:](https://www.semanticscholar.org/paper/Strengthening-of-Borosilicate-Glass-by-Ion-Exchange-Shim-Eom/7739d8becef2ec0d43e418a128dbceeb3f91a5e9?ref=blog.biodock.ai) Ion exchange in glass can be used to strengthen the material and change its optical density which can be used for the fabrication of GRIN lenses.


Now that we have constructed a GRIN lens, how can we begin to look at things? GRIN lenses can be implanted into tissue and coupled with various imaging systems to inspect regions beyond superficial layers. Think of the GRIN lens as a long window. One field in which GRIN lens endoscopy has become incredibly useful is neuroscience.[Imaging activity in the brain](https://www.youtube.com/watch?v=SQaueBgLknI&ref=blog.biodock.ai) through the use of calcium reporters such as GCaMP has become a ubiquitously used method. When imaging on the surface of the brain however, single and even[multiphoton methods](https://www.nature.com/articles/nphoton.an.2010.2?ref=blog.biodock.ai) are limited in the depth of the tissue one can extract meaningful activity from. By implanting a lens into the brain and expressing a calcium reporter in the region of interest, we can begin to assess the activity of previously inaccessible brain regions.


To implant a GRIN lens in the brain, the subject (typically a mouse) is first anesthetized, and their skull is exposed. After using the sutures of the skull to bring it to level, coordinates for the region of interest are used to locate the site of the implant. A small craniotomy approximately the size of the lens is made using a miniature drill. Once the brain is exposed, the GRIN lens is retrieved and slowly lowered into the brain to the desired depth. Finally, the surface of the skull is covered in cement which encases the sides of the GRIN lens fixing it in place while keeping the top of the lens exposed. Typically, prior to GRIN implantation, the brain region of interest is injected with a virus containing the genetic information for a fluorescent reporter such as GCaMP. The virus and vector can vary according to the specificity of the cell type you are interested. If you are interested further in the implantation procedure, I would encourage you to watch[this miniscope workshop!](https://www.youtube.com/watch?v=SZPAQps_uVo&t=1999s&ref=blog.biodock.ai)


***GRIN lenses are coupled with various optical systems to image live tissue in live animals***


An implanted GRIN lens can then be coupled to a miniature microscope (Figure 4a). This is mounted directly on the mouse and can image the activity of deep brain regions at single-cell resolution (Figure 4b). Many companies and labs have developed their own[miniature microscope designs](http://miniscope.org/index.php/Main_Page?ref=blog.biodock.ai) however, in the typical design, an LED (or another source) at the excitation wavelength for the reporter is passed through the objective and GRIN lens. This causes the reporter to fluoresce. That light is passed through the GRIN, objective, and imaging lens where it lands on the imaging sensor which collects the frame to be transmitted and saved. Both transmitted and emitted light encounters a[dichroic mirror](https://www.rp-photonics.com/dichroic_mirrors.html?ref=blog.biodock.ai) which is a special type of mirror that reflects or transmits light at differing wavelengths. This allows the light from the LED to reflect off the mirror and into the sample and the emitted light from the sample to pass through the mirror and ultimately land on the image sensor.


Figure 4: Miniature microscope coupled with GRIN lens for imaging of neurons in live animals (a) A schematic of the optical design of the miniature microscope (b) a miniature microscope coupled with a GRIN lens implanted in a mouse brain (from[Werner et. al 2019](https://www.frontiersin.org/articles/10.3389/fnins.2019.01174/full?ref=blog.biodock.ai) )


In addition to miniature microscopes, GRIN lenses can also be coupled to[fiber bundles](https://www.rp-photonics.com/fiber_bundles.html?ref=blog.biodock.ai) which can transmit the light signal over long distances to an imaging sensor and camera such that the system for acquiring the light need not be mounted on the mouse’s head. This system is organized similarly to the miniature microscope although a fiber bundle connects the objective lens to the GRIN lens (Figure 5). The fiber bundle is composed of many fiber optic cables which transmit light through[total internal reflection](https://www.physicsclassroom.com/class/refrn/Lesson-3/Total-Internal-Reflection?ref=blog.biodock.ai) where light passing through the bundle reflects internally through the bundle and is transmitted to the other end.


Figure 5: A fiber bundle coupled to a GRIN lens implanted in the brain enables high-resolution neuroimaging in a freely behaving animal. (adapted from[Pochechuev et. al 2020](https://doi.org/10.1002/jbio.202000081?ref=blog.biodock.ai) )


***Endoscopy and photoactivation using implanted GRIN lenses is a powerful tool for neuroscience and oncology***


Gradient index imaging has advanced our understanding of the brain and other organs. For example,[work from the Lin lab](http://dx.doi.org/10.1016/j.neuron.2016.08.037?ref=blog.biodock.ai) at Johns Hopkins University used miniature microscopes to record from clusters of neurons in the striatum in a freely behaving animal at the single cell level. They discovered that discrete populations of neurons were activated during specific behaviors and utilizing the activity of these neurons was sufficient to decode the animal’s behavior. They also showed how the activity of these neurons as it relates to locomotion changed under the influence of cocaine.


Outside the brain in the colon, a modified GRIN lens was used to[image the colon from a side view](https://www.nature.com/articles/nmeth.1440?ref=blog.biodock.ai) . The authors accomplished this by coupling the GRIN lens to a prism and scanning beam microscope. Using this system, they were able to image the colorectal vasculature and markers of inflammation. This tool presents potential use for pathologizing colorectal cancer and broadly in the field of oncology.


GRIN lenses have also been used for more than simply imaging of tissue.[Work from the Emiliani lab](https://www.nature.com/articles/s41598-019-43933-w?ref=blog.biodock.ai) at Sorbonne Université in Paris used a spatial light modulator (SLM) to generate precise patterns of illumination. They were able to generate spatially refined light spots through computer-generated holography which can be used for holographic optogenetic activation of neurons. Succinctly, this enables precise activation of deep regions of the brain which opens so many doors for neuroscientists to perform targeted neural circuit interrogation. For more information on holographic optogenetics,[see my article](https://blog.biodock.ai/gradient-index-lenses/blog.biodock.ai/holographic-optogenetics/) on the topic on Biodock blog!


GRIN imaging of neurons or internal vasculature often includes the identification of cellular features and or neurons across 2D image stacks. If you are interested in the automated identification of fluorescent ROIs such as somas, Biodock can help design and implement a custom AI module for this purpose. For further information,[Request a brochure](https://www.biodock.ai/brochure/?ref=blog.biodock.ai) to examine all the services Biodock has to offer, and do not hesitate to contact us for a consultation.


***Conclusions***


In sum, Gradient Index lenses leverages the refractive nature of light to curve and focus light from a source onto a point between one end and the other. These lenses can be miniaturized and implanted into live tissue and in live animals to observe the activity and architecture of organs from the colon to the brain. Future optimization of coupling GRIN lenses with multiphoton applications are already being developed to enhance imaging resolution. The prospect of using computer generated holography to precisely write into deep neural circuits is incredibly exciting. Developing an imaging system compatible with multiple GRIN lenses could prove useful for our understanding of simultaneous activity dynamics across discrete brain regions. Ultimately, it has it’s irony that the same mechanism by which evolution developed a focusing eye, we are developing lenses of our own to look beneath the surface.
