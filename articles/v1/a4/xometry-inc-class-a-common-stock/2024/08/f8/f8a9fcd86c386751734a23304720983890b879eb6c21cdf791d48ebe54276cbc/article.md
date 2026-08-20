---
schema_version: "1.0.0"
document_id: "f8a9fcd86c386751734a23304720983890b879eb6c21cdf791d48ebe54276cbc"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/tube/designing-coped-cuts-round-tube/"
published_at: "2024-08-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:53b0751ab29f8c1097e8980997f211fc833dbd63417c3cb897aa34a40732fbc5"
---

# Designing Normalized Coped Cuts on Round Tube

Xometry instantly quotes laser tube cutting and tube bending projects through our[Instant Quoting Engine](https://www.xometry.com/quoting/home/?) . Quoted projects will always assume normal to surface cuts which reduces lead time and price. To learn more about what normal to surface cuts are and why they are industry standard,[click here](https://www.xometry.com/resources/tube/normal-tube-cuts/) .


Normalizing your tube cuts may seem daunting, but it only takes a few tools! This article will explain how to normalize cuts in SolidWorks using two methods. These methods will normalize copes, holes, and most other cuts.


Note that these methods will not correctly normalize miter cuts. To learn how to do this, review[Designing Normalized Mitered Cuts on Round Tube](https://www.xometry.com/resources/tube/designing-mitered-cuts-round-tube/) .


## Replace Face Method


This method is a reliable way to normalize cuts on round tubing and only requires three tools. It involves replacing faces with a ruled surface.


## 1. Make a Ruled Surface


Start by clicking the Ruled Surface tool from the Surfaces tab. Before selecting anything, set the “Type” to “Normal to Surface”. This will make the ruled surface extrude normal to the surface of the tube.


Next, select the inside edge of your cut feature. This selection will be recorded as the “Edge Selection“. A yellow preview of your ruled surface should appear.


Then, input a distance greater than the thickness of your tube. The ruled surface should extrude past the outside surface of the tube. You may need to click the “Reverse Direction” icon to change the direction of your ruled surface.


Finally, click OK.


**Pro Tip:** If the ruled surface is not extruding outward, select your edge from the Edge Selection and click “Alternate Face”. This will alternate the face the tool is referencing.


## 2. Use the Replace Face Tool


Now that you have your ruled surface, click the Replace Face tool in the Surfaces tab.


Start by selecting the “Target Faces for Replacement”. If there are multiple target faces, make sure to remember the order you select them in.


Next, select your ruled surface as the “Replacement Surface”. If your ruled surface has multiple faces, be sure to select them in the same order you selected the target faces. Every target face should have a corresponding replacement face located such that the replacement does not create self-intersecting geometry. Otherwise, your feature will receive an error.


The ruled surface may be automatically selected. If the feature results in an error, right click the “Replacement Surfaces” and click “Clear Selections”. Then manually select the “Replacement Surfaces”.


Finally, click OK.
**Pro Tip:** In rare situations, the number of “Target Faces for Replacement” will not equal the number of “Replacement Surfaces”. To fix this, use the Knit Surface tool from the Surfaces tab to knit faces of your ruled surfaces together until every target face has a corresponding replacement surface.


**Design Guide: CNC Machining**


Download our guide for CNC manufacturability recommendations to optimize your designs, reduce machining time, and lower your costs.


[Download for Free](https://www.xometry.com/resources/design-guides/design-guide-cnc-machining/)


## 3. Delete the Surface Bodies


To remove your ruled surface, click on the Surface Bodies dropdown in the FeatureManager and select the surfaces making up the ruled surface.


Next, right click and select the Delete/Keep Body tool. Change the “Type” from “Keep Bodies” to “Delete Bodies” and click OK. Your ruled surface should now be deleted.


Congratulations! You have successfully normalized the surface of your cut.


## Delete Face Method


This method is the quickest way to normalize cuts on round tubing, requiring only two tools. This method can normalize multiple cuts on a round tube at once; however, any mitered cuts will be incorrectly normalized using this method. Be sure to normalize miters separately. Review[Designing Normalized Mitered Cuts on Round Tube](https://www.xometry.com/resources/tube/designing-mitered-cuts-round-tube/) to learn how to do this.


**Take a Tour of Instant Quoting Engine®**


Get a live demo of Xometry's best-in-class Instant Quoting Engine


[Launch Demo](https://www.xometry.com/testdrive/)


## 1. Delete Faces


Start by clicking the Delete Face tool in the Surfaces tab. Next, select all faces on your tube EXCEPT the inside face. You can do this by selecting each face individually, or by selecting the outside face and clicking “All Outer and Inner Loops” from the selection toolbar. Check the Options dropdown and click Delete.


Click OK. You should be left with the outside surface of the tube and your ruled surface.


## 2. Thicken


Now that we have isolated our inside surface, select the Thicken tool in the Surfaces tab. Select the inside tube surface as the Surface to Thicken. Change the thickness direction until the feature is thickening outward. The yellow preview should be outside of your tube.


Enter the wall thickness of your tube and click OK.


Congratulations! You have normalized your tube cuts. This method can normalize multiple cuts at once, saving significant time. It’s important to note that this method works best on straight tubing. Thickening a bent tube will often not result in normal to surface cuts. For visual tips on how to recognize if your cuts are normal to surface, check our gallery[here](https://www.xometry.com/resources/tube/normal-tube-cuts/) .


### FAQ


What is a normal to surface cut?


A normal to surface cut is a cut that is perpendicular to the surface of the tube. These cuts are industry standard and can reduce lead time and price of laser tube cutting projects. To learn more about normal to surface cuts,[click here](https://www.xometry.com/resources/tube/normal-tube-cuts/) .


Do I need to model my cuts normal to surface?


No. But modeling can help you understand how your design will look after fabrication.


Why is my Ruled Surface not normal to surface?


Make sure that the Type is set to Normal to Surface. If the Type is correct, select the problematic edge and click Alternate Face until the ruled surface is extruding in the correct direction. Alternate Face changes the face the tool is referencing.


Why is Replace Face producing an error?


Make sure the replacement surfaces are selected in the same order as the target faces. Every target face should have a corresponding replacement face that is located such that the replacement does not create self intersecting geometry.


What if the number of Target Faces and Replacement Faces aren’t equal?


Use the Knit Surface tool from the Surfaces tab to knit the faces of your ruled surfaces together until the target faces and replacement surfaces are equal.


Do These Methods Normalize Mitered Cuts?


Not correctly. Mitered cuts require special attention, needing an inflection point to be normalized the correct way. Review[Designing Normalized Mitered Cuts on Round Tube](https://www.xometry.com/resources/tube/designing-mitered-cuts-round-tube/) to learn how to do this.


How do I know if my cuts are normalized?


Rotate your model and imagine if a laser could make every cut perpendicular to the outside surface of the tube. If you still aren’t quite sure, take a look at our tube gallery[here](https://www.xometry.com/resources/tube/normal-tube-cuts/) .


## How Xometry Can Help


Normalizing tube cuts doesn’t have to be daunting or time consuming. We hope these methods help you understand how your round tube cuts will look after fabrication. If you want to make sure you’ve followed these steps correctly, review our gallery[here](https://www.xometry.com/resources/tube/normal-tube-cuts/) .


Now that your tube cuts are normalized, upload your laser tube cutting projects to our[Instant Quoting Engine](https://www.xometry.com/quoting/home/) today!


If you have any feedback about the methods discussed or have methods of your own you’d like to share, email us at **support@xometry.com** !


## Xometry's Tube and Fabrication Services


-


### Laser Tube Cutting Services


High-quality tube laser cutting for metal rectangular, square, and round tubes | Free standard shipping on US and international orders | International prototype pricing includes tariffs


[Get an Instant Laser Tube Cutting Quote](https://www.xometry.com/quoting/home/?processInitialisms=TC)[Laser Tube Cutting Services](https://www.xometry.com/capabilities/laser-tube-cutting-services/)


-


### Tube Bending Services


Xometry offers precision tube bending services for round metal tube stock to meet your needs.


[Get an Instant Tube Bending Quote](https://www.xometry.com/quoting/home/?processInitialisms=TB)[Tube Bending Services](https://www.xometry.com/capabilities/tube-bending-services/)


-


### Custom Online Weldment Services by Xometry


High-quality custom weldment services available for a variety of materials, manufacturing processes, finishes, and more.


[Custom Online Weldment Services by Xometry](https://www.xometry.com/capabilities/weldment-services/)


**Start Your Instant Quote**


STEP · STP · SLDPRT · STL · DXF · IPT · X_T · X_B · 3DXML · CATPART · PRT · SAT · 3MF · JT


[Upload Your Design](https://www.xometry.com/quoting/home/) All uploads are secure and confidential
