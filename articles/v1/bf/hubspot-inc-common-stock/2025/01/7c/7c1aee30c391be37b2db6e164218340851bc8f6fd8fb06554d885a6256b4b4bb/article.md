---
schema_version: "1.0.0"
document_id: "7c1aee30c391be37b2db6e164218340851bc8f6fd8fb06554d885a6256b4b4bb"
company_key: "hubspot-inc-common-stock"
company: "HubSpot Inc."
source_id: "hubspot-inc-common-stock-news-import-815987d58dd2"
canonical_url: "https://product.hubspot.com/blog/figma-design-prototype-tips-tricks"
published_at: "2025-01-29T00:00:00+00:00"
first_seen_at: "2026-07-21T23:16:14.524102+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:162f3f09133bff58ffbc26f2f60263c07e5fb7e0875ea0783aaec24287189aa6"
---

# Figma Design & Prototype: Tips & Tricks to Increase Speed

Hi everyone! I'm Yves, a Product Designer on the AI team. AI is moving quickly, and my designs and prototypes need to as well. Prototyping is a powerful tool that helps communicate a user interaction, design concept, or longer-term vision. It can feel time-consuming but I made a guide about ways I cut down Figma time and get high-quality designs in front of people faster.


This guide isn’t a Figma 101 overview, so the basics and foundations of Figma will not be covered. Instead, it contains tips and tricks (and lots of shortcuts) that I find important for increasing your design and prototyping speed.


*This also isn’t an official write-up and as with any guide, this can quickly become out of date as Figma updates. Last update: November 2024.*


*-----*


# **Overview**


I’ll be reviewing two sections: *Design* and *Prototype*


**


1.


Y


ou can toggle between **Design** and **Prototype** using **⇧** E


# **Design**


### **Tip: Make local components**


Local components are important because they allow you to update all your instances from one place. I like to create a separate page for them.


- Shortcut for creating a component is ⌥⌘K


After you create your local component make sure it’s used throughout your design. When you want to customize the instances of your local component (or any component), you can mass update properties or copy using the Select Matching Layers feature or Multi-Edit Text feature.


**1. Select matching layers** : Selects similar layers in other artboards. Applies changes to all.


**2. Multi-edit text:** Selects the text within the similar layers in other artboards. Applies changes to all.


- Shortcut for selecting matching layers is ⌥


⌘A


## **Watch it!**


You have 50 screens with tabbed navigation, and you want to change the “State” property from “Idle” to “Default” and the tab name from “Default” to “Gine”. You click into layers so that tab component is selected and press ⌥⌘A. You see all the same components selected and you change the state using properties dropdown in the right panel. You do it once and it reflects everywhere. You then click into the layers so the text layer is selected, press ⌥⌘A and then ENTER. Again, you see all the components selected and when you update the copy in one area it updates everywhere.


### **Tip: Be organized. Naming matters!**


Being able to use some of Figmas powerful features like *Select Matching Layers* or *Smart Animate* requires consistent naming. Get in the habit of naming your layers and components the first time, so it carries through the rest of the design.


- Shortcut for renaming is ⌘R


### **Tip: Utilize auto layout and clip your contents**


I use *Auto Layout* for almost everything. It creates responsive designs that grow to fill, shrink to fit, and reflow contents. You can rely on it for consistent resizing and padding rules. In the same layout container there’s a *Clip Content* option. This feature is important because it helps you achieve scrolling interactions.


**1. Clip content** : When you want contents to scroll within a container, make sure clip content is checked. By enabling "clip content," any part of an element that falls outside the bounds of the frame will be hidden.


**2. Auto layout:** Automatically aligns contents and creates consistent padding and margin rules. Get to know the auto layout features such as *Resizing* , *Direction + Gap* , *Alignment* and *Padding*


- Shortcut for auto layout is **⇧** A


### **Tip: Get to know shortcuts**


Here are handy shortcuts and examples of how to use them


#### **Replacing an old button with a new one**


I want to add my new AIButton component directly where my existing UIButton component is. I *Copy* the AIButton with ⌘C, select the UIButton and *Paste to Replace* ** with **⇧** ⌘R


- Shortcut for copy is


⌘C


- Shortcut for paste to replace is **⇧** ⌘R


#### **Centering a popup**


I want to see if a popup is the correct UX pattern for my design. I create a *Rectangle* by press R, then *Text* by pressing T, and then add in a UIButton. I *Group* them with **⇧** G. I press **⇧** R to *Rename* the group to “popup modal”. I then want to move it to the center of my screen. I *Align Horizontal Centers* with ⌥H and *Align Vertical Centers* with ⌥V. I like how it looks and want to design my popup further I *Ungroup* it with ⌘⇧G to continue designing.


- Shortcut for align horizontal centers is ⌥H


- Shortcut for align vertical centers is ⌥V


- Shortcut for group is **⇧** G


- Shortcut for ungroup is ⌘ **⇧** G


#### **Creating a slideout form with auto layout**


I want to create a slideout form for users. I add in *Text* by pressing T, add a few UITextInputs, and a UIButton. I can make sure the vertical space between the elements is equal with *Distribute Verical Spacing* or pressing ⌃⌥V. Instead, I select all the elements and apply *Auto Layout* with ⇧A and make sure the *Gap* ** is 16 and the *Padding* is 16. I make sure the *Clip Content* checkbox is checked. I press ⇧R to *Rename* the new frame to “slideout”. I want to move one of my UITextInputs to the top of list, so I select it and *Move to Front* using ⌘\].


- Shortcut to distribute horizontal spacing is ⌃⌥H


- Shortcut to distribute vertical spacing is ⌃⌥V


- Shortcut to move to front is ⌘\]


- Shortcut to move to back is ⌘\[


- Shortcut to bring to front is \]


- Shortcut to send to back is \[


In addition to all the shortcuts listed so far, here are more design shortcuts I use:


- Shortcut to show/hide is ⌘ **⇧** H


- Shortcut to lock is ⌘ **⇧** L


- Shortcut to flip horizontal is **⇧** H


- Shortcut to flip vertical is **⇧** L


- Shortcut to frame is ⌥⌘G


### **Bonus Tip: Get to know variables**


Variables should have their own tutorial (


[read Figmas overview here](https://help.figma.com/hc/en-us/articles/14506821864087-Overview-of-variables-collections-and-modes)


), and utilizing them will have you designing and prototyping faster. For now, here is where they’re located and how to apply them. You can find *Local Variables* on the empty state of your canvas, or by clicking on the canvas itself (so de-select any artboards or components). You can apply *Local Variables* from the appearance panel using the *Apply Variable Mode* button.


**1. Local variables** : Variables are raw values—like color, numbers, and strings—that can change in value depending on the context of a design.


**2. Apply variable mode** : Apply variables and change properties to the layer selected


# **Prototype**


Prototyping consists of *Interaction Noodles*


And *Interaction Details.*


**


[T aken from Figmas Advance Tutorial YouTube](https://www.youtube.com/watch?v=LfldqERs0aw)


When you’re done prototyping, you *Preview* with **⇧** \[SPACE\]. When in *Preview* ** mode, you can *Restart* the flow by pressing R. Be sure to set *Flow Starts* to keep your prototypes organized.


- Shortcut to preview with ⇧\[SPACE\]


- Shortcut to restart flow is R


### **Tip: Use this custom curve as your default**


There are a number of animation types and curves you can use. I give a quick overview below, however, default to this *Custom Curve* ** in *Smart Animate* I learned in a Figma prototyping tutorial. The *Bezier* is 0.8, 0, 0.2, 1.


Here are other curve types, which you can learn more from in this[Figma blog post](https://help.figma.com/hc/en-us/articles/360051748654-Prototype-easing-and-spring-animations)


or this


[cheat sheet](https://www.reddit.com/r/FigmaDesign/comments/1bc5a3v/cheatsheet_for_easing_in_figma_save_for_future/) .


#### ***Linear:*** **Can feel robotic**


- Good for opening dropdowns


#### ***Ease in:*** **Slow start, fast finish**


- Good for transitioning elements out of view


#### ***Ease out:*** **Fast start, slow finish**


- Good for transitioning elements into view


#### ***Ease in and out:*** **Slow, fast, slow**


- Can feel too perfect if applied to everything


#### ***Ease in back:*** **Goes past initial key frame and then speeds up at the end**


- Results in a small bounce in the beginning. Good for transitioning elements out of view


#### ***Ease out back:*** ****** **Goes fast, slows down, ends with a bounce**


- Can feel like something is settling. Good for transitioning into view


#### ***Ease in and out back:*** **Small slow bounce, speed up, small slow bounce at the end.**


****


#### ***Springs***


- *Gentle:* Most neutral option. Great for subtle spring movement when scaling content


- *Quick:* A bit more spring. Great for toasts and notifications


- *Bouncy:* A quirky preset for delightful animations like a heart bounce


- *Slow:* : A steady, natural way to scale up fullscreen content


- Custom: Adjust *Stiffness* ** (number of bounces) *,* *Damping* ** (level of spring) *,* and *Mass* ** (speed)


### **Tip: Utilize microanimations as well**


For me, it’s the little animations that make a big difference. A dashboard sliding in horizontally and vertically is definitely exciting, but having a button change state on hover is what makes the design feel alive. Don’t underestimate the power of the following


#### ***Hover***


Popularly used for tooltips, or hovering over a CTA


- Trigger type: While hovering


#### ***Loading:*** ****


Create a loading animation upon page load, inside a button, or skeleton loading


- Trigger type: After delay


#### ***Pulse:***


Have a notification appear or complete loading with a pulse animation


- Trigger type: While hovering + After delay


#### ***Scroll:*** ****


Move contents through your container, whether that’s a dashboard or a form


- Scroll animation. Position: Fixed, Overflow: Vertical or Horizontal


**1. P osition** : Similar to coding, declare how the selected container should behave in relation to the artboard it’s in


**2. Overflow** : For contents that go past the view, what should it do?


### **Bonus Tip: Get to know variables, conditions, and expressions**


Once you’ve got these tips and tricks down, push yourself to try advanced prototyping. This will programmatically create more complex relationships between your components. For example, when a checkbox is selected, update the counter by 1.


**You can learn about that example + a handful of others**[in this Figma Learn](https://help.figma.com/hc/en-us/articles/17146044893591-Advanced-prototyping-examples)


****
