---
schema_version: "1.0.0"
document_id: "46dc09aafdf90d28f004ed596f89b907285a9e6e84bc63388eb74d4b1352f767"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/3d-printing/tear-strength/"
published_at: "2023-03-23T04:00:11+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:632768b2dd8e7bb8ae9dd2ec39a3d54bf54893f296a139a08388955473ec71cc"
---

# Tear Strength: Definition, Relation to 3D Printing, Formula, Unit, and How to Measure

Tear strength is a material’s ability to resist failure perpendicular to the stress being applied. This is usually tested by measuring the force required to begin a tear, while that force is applied to an unrestrained area close to a clamp retaining the material edge.


In 3D printing, tear strength helps define structural strength advantages of particular methods of 3D print construction. This is particularly relevant to anisotropic construction methods such as FDM printing. Anisotropic materials have different properties depending on the mode and orientation of the material. This can be interpreted as a “grain” similar to wood, where forces along the grain (i.e., aligned with the build plane and the primary filament direction) are much better resisted than those which pull filaments apart from each other.


Tear strength values are measured in newtons per mm or pounds force per unit thickness (lbf/in or N/mm) of the material sample. Although, results from different test setups are not directly comparable, because of methodological differences. Test rigs restrain a part of the sample edge in a fixed plane and clamp a “free” edge close to the restraint. This allows a load to be applied perpendicular to the sample to induce a tear. Some tests require samples to have a smooth edge, others use a specified V-cut nick as an initiator, and others use a specified straight cut for this purpose. The measured value is the force required to initiate tearing only.


This article will discuss tear strength definition, its relation to 3D printing, its formula, and how to measure it.


## What Is Tear Strength?


Tear strength is a material’s resistance to a perpendicular failure. Tearing is a critical ability that many materials possess, and it’s caused by a force perpendicular to the plane of the material. An applied force that exceeds the tear strength causes a yield point failure that parts the material along the line of application of the force. Tear strength is generally useful as a measure of the behavior of non-brittle materials. Brittle materials do not tear but fracture when distorted. The measure is used to define the ability of materials that can distort significantly before failure. Paper has a measurable tear strength, whereas glass does not, as it undergoes a brittle failure. In terms of useful measurements, tear testing is usually performed on sheet materials such as fabrics, papers/cards, flexible plastics, and rubbers.


## How Does Measuring Tear Strength Relate to 3D Printing?


Measuring tear strength relates to 3D printing in that understanding the tear resistance properties of the various 3D print technologies and the orientation of the part helps make prototypes that are useful for active force tests rather than simply for fit and shape. The selection of process, material type, and build orientation can be critical factors in determining the suitability of a 3D-printed part for the intended purpose.


Parts that must flex or resist flexing, suffer repeat cycles of loading, and undergo complex load scenarios must be made by a process that delivers the required performance. Where flexibility is required, a basic SLA part will tend to fracture immediately. An FDM-printed part, on the other hand, that has been built such that the filament direction lies along the line of bending (rather than across it or through it) will perform better. For more information, see our guide to[3D Printing](https://www.xometry.com/resources/3d-printing/3d-printing-guide/) .


## What Is the Formula for Tear Strength?


The formula for tear strength is:


Tear strength = F/t


Whether the tear is pre-initiated with a V nick or I cut, or the edge is smooth and undamaged, a force will be measured that initiates the tear. That force (in N or lbf) is then divided by the sample thickness (in mm or inches) to provide a standardized measure of force/unit thickness.


### What Is the Unit Used for Tear Strength?


Tear strength is measured in newtons per mm or pound-force per inch (N/mm or lbf/inch).


### How To Test Tear Strength?


Testing tear strength is generally performed in a tensile test rig. A sample is clamped to the upper and lower jaws of the test machine. It is oriented such that the stress is applied as a tearing motion. The most common format uses the aptly named trouser test. The “legs” or a trouser shape are clamped in a plane and pulled as if worn during a splits posture. Failure occurs at the point where the “legs” of the trousers meet. This stretches or deforms the material edge into a displaced line (when loaded to less than the tear point). Force is then increased in steps until tearing commences at the stress concentrator.


Three forms of edge preparation are made: a V nick or I nick to act as initiator, or the edge is smooth and forms an undamaged curve where the “legs” of the sample meet. These tests are not necessarily comparable with each other. The initiator is likely to result in a much lower tear strength than from otherwise identical samples. The stress (force) concentration occurs at much lower strain (extension/displacement) levels.


Tear strength testing is therefore considered a qualitative test to demonstrate failure mode rather than a value comparative test to allow precise comparisons of various materials. It can be used for qualitative *comparison* of the force resilience of identically shaped/tested material samples. However, quantitative measures in test samples are likely to inform only qualitatively about failure risks in real-world conditions, where load applications are rarely as simplified/ideal as those in a test laboratory.


### How To Measure Tear Strength?


In performing a test, either weight or displacement and a force measurement module are applied to the moving clamp. The force at which tearing is initiated is the resulting test measurement. Sample shapes and initiator cuts vary, but all samples are arranged so that a simple tensile loading applies a tear force at a predetermined point in the sample.


### What Are the Standards by ASTM International To Measure the Tear Strength of different materials?


ASTM D264 is the US test standard for tear resilience, and it specifies five sample types: A (crescent, razor nicked), B (Winkelmann), C (Graves), T (Trouser tear)/ASTM D470, and CP (Constrained path trouser tear). For test specimen types A, B, or C, the measured value of tear strength is simply force (to initiate tear) divided by sample thickness. For specimen types T or CP, the measured value is the average or median force applied at the curve divided by the specimen thickness.


ISO 34-1 is directly comparable in principle but differs in a number of key details that make the two test standards very hard to directly compare, even for identical materials.


To learn more, see our full guide on[ASTM International](https://www.xometry.com/resources/certifications/astm-international/) .


### What Are Examples of Tear Strength of Different Materials?


#### 1. Tear Strength of Fabric


Fabrics are materials produced by weaving together materials such as wool, nylon, and cotton. Cotton fabrics vary widely in tear strength depending on the base material and the force applied.


#### 2. Tear Strength of Rubber


The values for various rubber materials are: natural rubber (23.95 +/-1.85 kN/m), nitrile rubber (9.14 +/-1.54 kN/m), styrene-butadiene rubber (4.88 +/-0.47 kN/m), and EPDM rubber (7.27 +/-0.86 kN/m).


#### 3. Tear Strength of Plastic


The tear strength values for plastics vary, depending on the elongation orientation, polymer property variations, and the wide availability of polymer types. Results are usually only of interest in film materials that will be subject to manufacture or use stress. Polymer films also tend to be tested using ASTM D1922, the Elmendorf tear strength test (results in grams). For example, a modal HDPE has an Elmendorf tear strength of 120g MD (machine direction) and 24g TD (transverse direction). LDPE, on the other hand, has an Elmendorf tear strength of 320g MD and 170g TD.


### What Are the Types of Materials With High Tear Resistance?


In fabrics, synthetic fibers have higher tear resistance. Kevlar® and nylon are good examples of extreme tear resilience in flexible fibers. Parachute materials are made of fine woven nylon due to the severe consequences of catastrophic failure. Military body armor and motorcycle armor are generally kevlar®, in which the combination of low elasticity and huge tensile resistance make for more rigid but tough fabrics.


In elastomers, the highest tear resilience comes from natural rubbers and compounds containing them. This is a result of the very high elongation at the break of natural (vulcanized) rubber. However, the balance of properties favors synthetic rubbers in many applications, which are generally stiffer and more durable.


### What Are the Types of Materials With Low Tear Resistance?


Some examples of materials with low tear resistance are: paper, PVC films, thermoplastic rubbers, silicone rubbers, and natural-fiber fabrics.


## What Is Tear-Yield Ratio?


The tear-yield ratio is a measure of the ability a material shows in elastic and plastic distortion prior to tearing. For example, materials that distort elastically under load will recover when the load is removed. If the elastic limit is exceeded, and plastic deformation occurs, there will be some recovery when unloaded. If the material tears soon after the yield point is reached, recovery will not be possible. A high tear-yield ratio indicates a material that is prone to failure when overloaded in a tearing mode.


## What Is the Average Tear Strength Measurement Good for 3D Printing?


The average tear strength measurement that is considered good in 3D printing depends on the application. The absolute tear resilience of the native material used in the print can help the user assess the selected printing technology and material. Many 3D printed materials are both relatively weak and brittle. The tear strength can help in understanding the material behavior under load. The relative tear strengths of models built in a variety of machine orientations can vary significantly. Where the tear threat runs along a primary grain direction, the material will be weak. In thin-section FDM, for example, the filaments are stronger along their length than they are at 90° to that axis. Selecting the optimal build orientation can have a significant effect on functional properties.


### Frequently Asked Questions About Tear Strength


What Device Is Used To Measure Tear Strength?


The device used to measure tear strength is a tensiometer. A tensile test rig is adapted to tear testing by fitting a more sensitive load cell. This reflects the lower total force required to tear most materials, compared with their ultimate tensile strength.


What Is the Difference Between Tear Strength and Tensile Strength?


Tensile strength testing applies a simple linear load to a predetermined sample that has a controlled stress concentration zone (a neck) at its center. All of the force applied in the test is carried by material resisting linear extension, which is commonly the strongest mode of load resistance in engineering materials.


The same machine can be used for tear testing of flexible material samples—but the test sample looks different. Generally, the two “ends” of the test sample sit together at one end of the test piece and they are reorientated to a common axis for the test, like pulling the legs of a pair of trousers as a rope. This forces a deliberate concentration of stress and strain at the junction point in the center, which is also often notched or cut to force the position of tear initiation.


To learn more, see our full article on[Tensile Strength](https://www.xometry.com/resources/3d-printing/tensile-strength/) .


## Summary


This article presented tear strength, explained it, and discussed its how to calculate and how it relates to 3D printing. To learn more about tear strength,[contact a Xometry representative](https://www.xometry.com/contact-us/) .


Xometry provides a wide range of manufacturing capabilities, including 3D printing and other value-added services for all of your prototyping and production needs. Visit our website to learn more or to request a[free, no-obligation quote](https://www.xometry.com/quoting/home/) .


#### Copyright and Trademark Notices


1. Kevlar® is a trademark of E. I. du Pont de Nemours and Company


#### Disclaimer


*The content appearing on this webpage is for informational purposes only. Xometry makes no representation or warranty of any kind, be it expressed or implied, as to the accuracy, completeness, or validity of the information. Any performance parameters, geometric tolerances, specific design features, quality and types of materials, or processes should not be inferred to represent what will be delivered by third-party suppliers or manufacturers through Xometry’s network. Buyers seeking quotes for parts are responsible for defining the specific requirements for those parts. Please refer to our[terms and conditions](https://www.xometry.com/legal/) for more information.*


**Start Your Instant Quote**


STEP · STP · SLDPRT · STL · DXF · IPT · X_T · X_B · 3DXML · CATPART · PRT · SAT · 3MF · JT


[Upload Your Design](https://www.xometry.com/quoting/home/) All uploads are secure and confidential
