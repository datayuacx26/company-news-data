---
schema_version: "1.0.0"
document_id: "1ab4b5ddf9499bb91d099bde410a98eb4276afea10adf093addb90e14e09c8b3"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/blog/autodesk-keypad-teardown-design-webinar/"
published_at: "2025-01-17T21:55:46+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:45c5bb9eb3a9efb23aa530a46098ecde91bf6bb90dbeb48b81456a70c5a8491a"
---

# [Webinar Recording] Keypad Teardown and Design Insights with Autodesk

## Breaking It Down: Live Keypad Teardown and Design Insights with Autodesk


#### Summary


In this exclusive webinar hosted by Xometry’s Greg Paulsen and Autodesk Fusion's Jonathan Odom take a deep dive into the second revision of the macro keypad developed for Autodesk University's Factory Experience 2024. This webinar features a live teardown of the keypad, showcasing how the design team addressed challenges and elevated the product with:


→ Enhanced industrial design


→ Improved fitment and tolerance stack management


→ Simplified assembly processes


→ Reduced carbon footprint in manufacturing


→ Optimized DFM (Design for Manufacturing) for each component


Discover the design decisions and engineering strategies that drove these improvements and gain actionable insights to apply to your projects. The session concludes with a live Q&A, where participants asked Greg and Jonathan about the design and manufacturing process.


#### Key Takeaways


✔ Manufacturing process and material considerations for electromechanical design


✔ Solutions for tolerance stacking and fitment challenges in product design


✔ Techniques for reducing manufacturing complexity and carbon footprint


✔ Best practices for optimizing DFM and assembly workflows


✔ Behind-the-scenes look at the collaboration between Xometry and Autodesk


### Questions and Answers from the Webinar


How quickly can Xometry go from part submission to DFM to injection molded parts?


Part submission is instant, just upload your file to the[Xometry Instant Quoting Engine](https://www.xometry.com/quoting/home/?) and select Injection molding from the process dropdown! Our team will work with you to understand your project requirements and provide a lead time for T1 samples. During the process, you can access your[Tool Details](https://www.xometry.com/how-xometry-works/tool-details/) page to track milestones and timelines.


What is the industry standard for surface finish on injection molded parts and where can we find examples?


Unless otherwise specified, an injection molded part will typically have an SPI B-2 default finish to enable better part release from the mold and remove tool marks. We recommend reviewing our article on[injection molding finishes](https://www.xometry.com/resources/injection-molding/choosing-the-right-finish-for-your-injection-molded/) to see examples of the most common SPI and VDI finishes.


What is the main difference between Autodesk Inventor and Fusion 360?


There is a lot of overlap between both programs, but there are some key differences. Inventor is PC only, whereas fusion is Mac or PC. Fusion has a full electronics suite and CAM package built-in and is automatically connected to the cloud for enhanced collaboration and revision control. Inventor is better suited for large complex assemblies with tons of parts. You'll also want to use Inventor if your project has restrictions like ITAR control, as Fusion is not approved for such projects since it is entirely cloud-based.


Is it possible to incorporate fool-proof features on your mating components to create room for easy assembly without making mistakes?


Yes! In the industry, we refer to this as the "poka-yoke" technique. Essentially it means designing components so it's difficult or impossible to put things together the wrong way. An example of this would be the post on the hub of a car wheel which makes it so the hub can only be oriented one way. The macro keypad was designed with features like this to help event attendees assemble their keypads correctly.


On the top clear plastic, there are four screw bosses. There are rib features that connect the bosses to the perimeter frame. What is the purpose of the ribs on these thread bosses?


The ribs connecting the screw bosses to the walls are there primarily to brace them. Without the ribs, the boss would be easily bent which could cause it to become damaged or break. Ribs also help the molten plastic flow during the molding process and keep the design uniform, which prevents molding defects.


Is the macro keypad project open source?


Yes! You can find the project files by visiting this link:[https://www.autodesk.com/campaigns/fusion-360/factory-experience](https://www.autodesk.com/campaigns/fusion-360/factory-experience)


Are the keycap models available for download so I can print my own?


Yes, the design is in the fusion file here:[https://www.autodesk.com/campaigns/fusion-360/factory-experience](https://www.autodesk.com/campaigns/fusion-360/factory-experience)


Do you offer robotic assembly?


We are happy to review your project! We offer electromechanical, cable & harness assembly, along with physical and functional testing.


[Design Guide: Injection Molding Read this design guide to help you understand materials, finishes, and important design-for-manufacturability considerations. Read More](https://www.xometry.com/resources/design-guides/design-guide-injection-molding/)


## Full Transcript


Greg Paulsen:


So, yeah, let's get started and welcome to today's live event. I'm Greg Paulsen from Xometry, and this is going to be, like I said, a really fun webinar. We'll be spending the next hour talking about the design and manufacturing of Autodesk University's Factory Experience Keypad. It's an electromechanical device that went through significant changes from its 2023 V1, version one, to its 2024 version two. I'll also be joined with Jonathan from Autodesk. You can see him on camera right now. We'll give you a full introduction in just a moment. But I'm just so excited to be able to share this manufacturing success story and also pick Jonathan's brain about the behind the scenes of looking at where we went from the design decisions because he's really the engineer here, we just make the parts going forward. So I'm going to move to our agenda. Before we get started though, let's lay down some groundwork.


This is recorded webinar, so there's going to be a recording available afterwards as well. We are going to start with some introductions, a brief background of our companies, and we're going to jump into the manufacturing design. That's going to be the beef of this webinar here. And part of that is a teardown. So Jonathan has two cameras. He's actually going to toggle back and forth as we go through showing kind of like a hands-on view of taking a part of V1 and V2 keypads as we work through. This is also a event where we're talking to you live, we want to answer your questions. You have the opportunity to submit questions using the Q&A tab on this platform, and we're going to try to save time at the end to get to as many as possible. So please use that liberally, we want to answer those questions.


And also after this webinar, we typically will post the recording with some of that Q&A answered as well within a blog post. So even if we don't get to your questions, we will answer them for you. So thank you so much for this and let's just talk about who we are. My name's Greg Paulsen, I'm director of applications engineering at Xometry. We're a digital manufacturing marketplace. I work with customers as well as internally on manufacturing projects. My specialty is the help me choose the what and why when it comes to materials and processes, spinning the CAD, giving design for manufacturability feedback, and working across a multitude of processes. And with me, I have Jonathan Odom from Autodesk Fusion. And if you want to give introduction, Jonathan.


Jonathan Odom:


Yeah. So like Greg said, Jonathan Odom from the Autodesk Fusion team. It's my 10th year at the company. I'm a community manager. That means a lot of different things, but my favorite project I do every year is the Fusion factory experience that usually is at Autodesk University.


Greg Paulsen:


Yeah. We put a slogan for his title. When he does a share screen, it's going to say factory boss because that's really what he is. We have a question at a live event, we're walking straight to him. So before we get into the factory experience, I do want to start with some background about our work and our collaboration with Autodesk and a little bit about what Xometry is if we're new to you. Xometry is a digital manufacturing marketplace. This is a two-sided marketplace model where we support both buyers, people who need stuff made, and suppliers, those who produce parts, connecting those using AI and machine learning in the digital technology. We serve over 64,000 buyers. 64,000 companies are using Xometry to procure parts in over 20 different manufacturing processes. We're also connected to 10,000 suppliers, both domestic as well as global through our European and Asian divisions.


And again, we're offering 20 manufacturing processes. Everything from low-volume prototyping work to fully specced serialized production. We're able to meet you where you are. I also just saw my cat is in the background, so that'll be a fun little Easter egg as we're doing this webinar here. And a little bit more about our business and how we support both needs. For you, we are using AI machine learning in the heart of a digital transaction or instant quoting engine. So your experience would be uploading a 3D model to xometry.com's website. You get instant pricing and lead times. So we actually interpret that 3D model. We even guessed, based on your design, what type of process you're probably designing towards. And you see that pricing lead time as you change and configure your parts. So you can change the materials, processes, features, finishes, shop certifications, all that will dynamically update in real time.


And we're able to meet you where your parts need to be. We are not making shapes for you, we're making your parts. And with that we have an extensive list of what we're able to do and how we match on that side. For sellers, it's kind of like the Uber for manufacturing. So if you're a manufacturer, you're able to see work on demand. So we have a vetted supplier list that's using another platform we call WorkCenter. And for them they see real one work that matches what they need. We use AI machine learning to match make there. And it gives a consistent experience from order to order for our customers as well as for these suppliers. They're seeing stuff that they actually want to make without the need for kind of a RFQ or bid war against it. And, Jonathan, if you want to go into Autodesk and Fusion and how we're evolved together as well.


Jonathan Odom:


Yeah. So Autodesk Fusion, if you're not familiar, is our newest product in the design and manufacturing division. Let's see, we started Fusion in 2014, about the same time I joined the company. And it's basically just an integrated cloud-connected full platform for design and manufacturing. So it does CAD, CAM is built in as well, electronics, all of this stuff is together in one data set. And there's also a really broad suite of management tools. It's really easy to collaborate with and one of my favorite things about it is how easy it is to share stuff with customers.


Greg Paulsen:


Yeah. And throughout this project also, we were using Fusion for sharing and collaborating as well.


Jonathan Odom:


Yeah, yeah. So you're seeing it's just kind of an overview of all the things you can do with it here. So drawings is another thing I didn't mention. Obviously it's fully parametric like Inventor. And yeah, it really is an end-to-end solution for design and manufacturing and it's especially great for consumer products.


Greg Paulsen:


That's awesome. And we're going to go into more of the things that you could do in just a moment here. But yeah, like I said, the bulk of this webinar is going to be talking about these devices, the device that we made for the factory experience. I got my Gen 1 and Gen 2 keypad here, and Jonathan's going to show you in just a second what this will look like as we start to tear them down. But, yeah, let's give some context though, Jonathan, to this. What is this experience? What are your goals for these? And you're doing these every year at the Autodesk University flagship event.


Jonathan Odom:


Yeah. So the idea here is that we are all siloed. Everybody that's in this space has some specialty. If you're a design engineer, you may never set foot in a factory, you certainly won't be assembling anything. You won't be running parts on a machine. If you're a machinist, you're not going to be part of the design process necessarily, and again, you wouldn't be assembling stuff. So what we're trying to do is paint a picture of what it is to take a product from concept to production all on the Fusion platform with our partners. So we have a lot of different partners there. Gravity and Asema Pro is an example. They actually work with Xometry doing sustainability analysis, that kind of stuff. We have machine partners there so you actually get to see the machines running, the machines that Xometry provides access to through their platform. Greg, I'm loving your cat in the background, man.


Greg Paulsen:


I know. I can't do anything right now about it. It's just I'm victim to my cat, whatever she's doing.


Jonathan Odom:


Yeah, she's doing her own thing. It's good. Yeah. So then basically you take a tour of a full working factory and then at the end there's an assembly area where you take parts and assemble this device and take it with you. And then this last year it was the version two of the macro keypad. So I'll pass it on to you, Greg.


Greg Paulsen:


Sure thing. And actually before I go into that, tell me about the macro keypad really quick? This device, what does it do?


Jonathan Odom:


Yeah. So basically these are fairly popular with people that are kind of keyboard nerds I guess. It's got obviously 12 keys on it. There's an OLED screen and a dial. So what you can do is program it for basically anything you want that could come from a keystroke. We have for all of the most popular commands on our top five Autodesk products, so Fusion, Inventor, Maya, AutoCAD, and Revit, and you turn the dial to flip through the different apps. It's all programmed in Circuitpython, so it's very easy to customize. We also have a landing page where if you got one, you could go there and customize it however you want. Make it do basically anything you want it to do. For those of you that understand electronics, there's an RP2040 running the whole device, which is a Raspberry Pi chip. It's powerful. It can do a lot more than just what this device can do pre-programmed.


Greg Paulsen:


Sure thing. And one of the things was we did this in 2023 as well and it was a hit. It's a really cool device and it's something that, I always like to use the phrase desk worthy, right? Is it desk worthy and it can be a companion just like when I do CAD, I have my space mouse. You have these tools that just stay handy with you when you're working in a CAD environment.


Jonathan Odom:


Right.


Greg Paulsen:


So yeah, we made two versions of these. We made a Gen 1 and Gen 2. Just a reminder for those. So we've been here before, Jonathan, talking about this before. So deja vu all over again. But we did the V1 factory experience, we actually did a webinar, so this is my promo, my commercial about Xometry within a Xometry webinar. If you have a chance and you haven't seen this as an audience, I do recommend checking out this webinar.


It's really cool. It goes deeper into some of the injection mold topics around this process. Where today we're going to talk about molding, but we're going to go much more into the design thinking as we're thinking about generations of products because it turns out most products do change. And this is a really great topic for today. So speaking of changes, we made V 1. V 1 was assembled by over 500 participants, I believe, in 2023, and we had goals for V 2. And so walk me through a little bit about what you wanted to see in the second version.


Jonathan Odom:


Right. So version one, the idea here with all of these really is that we show as many manufacturing processes as we can. So it needs to be something that's more or less realistic for what a real consumer product would be, but we tend to put in a little bit more effort into it than you normally would with an off the shelf consumer product. So we want to show CNC machining, injection molding, additive manufacturing, and a combination of all those things. And, obviously, electronics. So both of these devices cover all of those categories. But with the redesign, there were a few things we needed to do. So the original device, you'll see here in a minute when I have the top down view, the original device honestly looks a little clunky. My background's architecture, not industrial design. So there are certain design principles in this area that I just don't really know much about.


So having done that myself with a little help from some of my colleagues, it didn't really have a nice sleek look that it could have. So that was one thing, just better aesthetics, maybe better ergonomics. Improved fitment and tolerance is a really important part. The tolerances were a little too tight for this one. I got this idea in my head for some reason that everything needed to have as tight a tolerance as possible. I don't really know where that instinct came from, but obviously it's not the best idea, especially if you're talking about a consumer product. You need to keep dust out of cracks and stuff like that, but you don't need a aircraft level tolerance between these parts. So the tolerances needed to be a little bit more loose. Simplifying the assembly kind of dovetails into that. So the people that are assembling these things are people that are attending the event.


These are not professional assembly line workers. They may have never really held a screwdriver. I actually have met a couple people that this was the first time they'd ever screwed in something, which kind of blows my mind, but no judgment there. So the assembly was a little tough for this first version. I'll show you in the teardown some of the kind of pinch points there. So that needed to happen. Optimized DFM, I mean you're always trying to do that. So that obviously becomes part of version two. And then a big one is that we wanted to reduce the environmental impact and we were able to bring down the embodied CO₂ in the product by 49%. And I'll get into some of the ways we made that happen. But with Xometry, and Gravity, and, Asema Pro, and Makersite, a few other of our partners, we were able to test and track and analyze the product throughout the process to actually bring that down. So we're pretty proud of that.


Greg Paulsen:


It's pretty amazing that accomplishment. And this is also a cool note is that Xometry, as Jonathan mentioned, we have a partnership with Gravity which allows you to calculate manufacturing carbon offsets. Part of our program, we have a program called Go Green on xometry.com. And first off, we offset the carbon footprint of all of our shipping at Xometry. You don't volunteer for that, we're doing it. All right, so that is something that we are already doing and it's been a policy that we have as part of our ESG policy. And within Go Green as well on the instant quoting engine, as you are uploading your files, you can actually select if you want to offset more. So do you want to offset the manufacturing footprint and we have an estimation for you and you could pay a small portion so you could offset 100% or 50%.


And you could also choose where you want that to be. Do you want that to be in forestry, wind, solar, et cetera. So we have some choices there as well as a customer. So that's something important to you, we have that digitized within our platform. So we're about to tear this down. I do want to give you an overview of the mechanical components that were produced by Xometry and then we're going to start from the first parts you're taking apart and we'll move down from there. So I'll tell you a little bit more about the manufacturing side of it and then Jonathan's going to talk to you about the design and requirements. But we did use injection molding, over-molding, SLS, so selective laser sintering 3D printing, and then CNC machining for these components. The think about the things that are cosmetic, those are going to be the ones that are tooled processes as well as the machine components. And then some of the cosmetic trim, but most of the internal things are going to be printed because you have a lot of flexibility in what you could do with design there.


And I want to talk about the enclosure first because this is what Jonathan's going to be tearing down in a second. But we injection molded the enclosure because injection molding can hit a really great per-unit cost and we're talking parts making parts in the low to mid-hundreds for the requirements of this project. But you also had the requirements of something that is aesthetically pleasing, you need consistency on tolerance system fitment, and you also needed texture. You needed a texturing that was going to be programmed the way you wanted it. And that's everything that you could do when you're accomplishing with a mold, and actually these are the injection mold inserts that were used. So these are the actual tools used to produce this part. Because Autodesk University is a public event, part of our education is bringing those tools, which are extremely heavy, bringing those tools down with us too to show them off. And these are tools minus some of the components that you'll usually see like ejector pins and lifters and other features. But just showing what those look like.


When we do injection molding in Xometry, it's not a direct click and buy experience because there are things that we want to know about. What's your estimated annual need? What are your overall goals? And we build a tool strategy with you. Part of that tool strategy also is this iterative approach like we're giving you design for manufacturability feedback along the way. On the left-hand side here, I'm actually showing, I used Fusion for this view here, but if it's Fusion accessibility analysis, other academy call it an undercut analysis. But that's a very good tool for you if you're working on mold project to kind of understand where your parting lines may be and where there may be requirements to add an action like a lifter or a slide to create a feature that wouldn't be directly on either the A or B side of the mold while making that.


That's something where you give design feedback on. So you as a designer, we usually want your responsibility to be on designing the part with uniform wall thicknesses, mitigating undercuts, you don't need to remove them, it just adds complexity to the tool but undercuts are in a lot of molded products. And then drafting, which is getting an appropriate degree of draft on your design so that when I am ejecting that part it's fully releasing from the tool without dragging out. What we're responsible for is this fun stuff on the right here. So when we're designing and talking about tool strategy, we're figuring out how we're going to part this. So the tool design itself, like where the parting line is going to be. And you can clearly see here where that red meets the green, that would be a parting line there. We're also looking at ejection.


So what this view is actually showing is I suppressed all the tool except for the ejector pins and lifters. So these are the things that are pushing the part out gently in the tool as it's ejecting through. And then we also talk about gating. So how are we injecting material. In this part here it actually had a very unique design where I have this kind of C curve going around here. Usually we have a O-shape opening, like a picture frame opening. You are gating in the middle and then running runners out to the sides in order to get an even flow of material. But in order to make sure that the material flowed around that C curve and hit the end here without incomplete features, we also added a sacrificial runner right here to bridge that gap. That also reduces any warping so it doesn't potato chip up or anything as it's cooling, but it allows for better flow of the material.


So it was really unique because you had the design that you wanted, Jonathan, and then we figured out how to tool it. And there's a little video of the tool running. So you can see here the ejectors and lifters are pushing that part off and it's going through and running through another cycle. Injection molding is liquefying plastic, it's filling up those cavities and then it hardens in place. And then after several seconds it'll open up, you'll get your next part. Rinse and repeat. And that's how you can make hundreds of units. So the investment's in the complexity of the tool, but afterwards you're able to go from there. And these runners were typically just using die clippers to cut those off flush afterwards. With that, Jonathan, it's your time to shine man. So we're going to start taking apart the enclosure and, like I said, we're going to go step by step as you run through. So I'm going to stop share and I'm going to give you the spotlight.


Jonathan Odom:


Cool. Yeah. So you're seeing the two versions of the device right here. I'll tear down this one and then show you the changes we made with this one. So you're going to kind of have both as you go through. Like I said, macro keypad, when you turn the dial it changes the app. It's a high contrast black and white OLED screen. The electronics are identical on both of these. That's one of the reasons we did a version two is that we already had this device designed and it was so popular that we didn't think anybody was going to complain about it coming back. Also what you can see here is as you change the app, the color of the LEDs on the PCB change to match that app. So that would be Maya, I think that's Revit, that's AutoCAD, Fusion, and then it goes through the... Oh yeah, and there's a number pad too. Sorry about the refresh rate. It's just-


Greg Paulsen:


I was going to say there's a frame rate-


Jonathan Odom:


...Of the camera.


Greg Paulsen:


... Of the camera versus LLED. Those are actually steady images for Jonathan's eyes.


Jonathan Odom:


And I'm going to unplug this and just kind of show you here. These are etched and you can see through and in a minute Greg's going to explain how we did that. That was kind of a cool feature on this product. Okay, so we'll do a teardown and talk about the injection molding first. Unplug these. And with the first version of the device, I was really interested in showing off the clip feature in our product design extension. So it's kind of new to injection molding and I thought that would be kind of a cool feature to show off. So I designed it so that these 3D printed sort of, what are we calling these, spacers, clip features that the clips would go into these little pockets in here. So there are machine screws that hold the PCB on and then the injection molded enclosure is clipped in. So I'll pop that out with the screwdriver.


This is more difficult, obviously, than having threaded bosses that you unscrew. Especially if you are not used to making things or working with this kind of stuff. So it was easy to put on, I would say for the most part as far as getting these clips in here for the attendees, but tearing it down as you can see is a little tougher. And then another thing we ran into here were just some tolerance issues that again come from my misguided instincts about trying to get everything so tight for reasons I still don't really remember I was going for there. So pulling that off of here, that little trim piece is kind of lifting out really tight in there.


I like this color, it was kind of cool. We did this frosted, grayish, translucent polycarbonate. One of the things I love about injection molding is there's just an endless array of finishes and colors and translucency and all kinds of stuff you can do. So really happy with how these looked. One thing I want to say about Xometry is that when you work with Xometry, it's kind of like what comes with the package is a whole team of engineers that are specialists in different things. So one thing that happened when I put this design in for a quote was the injection mold engineer got back to me and said, "Hey, this part right here where we have a recess that's to accommodate this encoder right here. That recess had to be there for this all to fit." He said, "That might be good," actually it might've been you, Greg, "that would be a good place to put your logo because you're going to get a sink issue there if you don't keep that consistent thickness in that spot."


So the logo ended up there, which was kind of cool. And it's tough and it all worked great. But when you're putting it together it's really tricky because the tolerance is so tight on these Fusion MultiJet parts that we had to get people to push down right there and you got to push here and pinch there and it's not easy to get this thing on just because those tolerances were so tight. Greg and I actually at the trade show ended up having to deburr a bunch of these things because, again, we had that tolerance but so tight that it made things difficult.


Greg Paulsen:


And this goes back also one of the things that we're trying to show off in this factory experience, you're trying to show off all these cool technologies. But one of the things is every single one of these technologies has different tolerances. So for example, an injection mold to CNC, both are very tolerance processes, very repeatable, but in this case we added 3D printed MultiJet Fusion rubber in between. And we'll go into that a little bit more, but what you're doing is you're sandwiching something that has a much looser tolerance in between two very tight tolerance things and that looser tolerance has variability depending on assembly, build, orientation to other things. And even when we running production, there's just the inherent \[inaudible 00:26:15\] shape-ness that comes with additive manufacturing and that was definitely one of those things where we ran into it. So do you want to show the other side? I actually like showing the screw side on the plate. You have a lot of features here and some complexity for pads and as well as those notches for the tabs, and now turn over the new device.


Jonathan Odom:


Yeah, exactly. So you look at this and see these are pockets for rubber feet, adhesive rubber feet. We've got these features here to grab those clips. We've got machine screws holding the PCB down. Kind of a mess, right? So this year we worked with our resident industrial designer and would you look at that? It's clean. The screws are in line with each other. It looks very intentional. And just, again, one of these instincts from our industrial designer to help simplify things and make it look cleaner. And these are actually thread forming screws. So instead of doing machine screws and then another type of fastening feature, what we ended up doing is this injection molded part actually is holding the whole thing together. There's rest features inside here, little shelves that hold everything together and it all stays nice and tight. So get these out of here.


Greg Paulsen:


Yeah. As you're unscrewing this, even though this is a smaller sample, you ended up putting together 100s of these and you had every single one of them was a contestant, a subject on the design for assembly side of this. And so you are basically getting constant feedback in real time as you're doing this and the idea lists started coming through. So when you all decided to move and make a version two of this for next year, I mean I'm sure you had that goal list of this is what I want.


Jonathan Odom:


Yes.


Greg Paulsen:


And working through. And it's so funny because we see the time it takes to take apart the V 1. Then as we start taking apart the components on the V 2, you're just going to see it's like, oh, it just pops off. It just works. It's just amazing when you work on revisions. Oh, there we go.


Jonathan Odom:


Yep, exactly. Yeah. One of the things we do is gather feedback from attendees every year and try to improve things as we go along. So it's definitely something we did there figuring out that this was unnecessarily difficult. What we're always trying to do here is make it interesting without being frustrating. So there needs to be some effort put into putting it together, otherwise it's boring. But if it's frustrating, obviously that's not an optimal outcome. So a few things to notice about this. The new one is thinner by a millimeter. I think it's a full millimeter thinner than the first one was, which reduced a lot of plastic in the long run. So that's one of the ways we got that carbon footprint down. We've got bosses here for thread forming screws this time that go through this base plate. And then there's these little clip features in here that you can see there and those clips grab onto this FFF 3D printed key plate.


So there's a little female feature there that this one clips into and it holds the whole thing down. The reason that's there is if you didn't do that, if there wasn't a clip feature, it'd be really easy to pull this part up because there's-


Greg Paulsen:


Makes sense.


Jonathan Odom:


... Nothing holding it down. The bosses are only here and here, so this would've been easily kind of flipping up all the time. It also just makes it a nice contained sort of pre-assembly right there before you put the screws in. So really happy with how this came out. The interface between the 3D printed parts and the injection molded parts is just a little bit wider, half a mil, something like that, but enough to make a huge difference in actually putting the thing together. Simplifying that part. Another thing to note here is that we didn't use a rubber type material, TPU, like we did for this one. This one's a more rigid plastic, which also makes that injection molded apart easier to fit in.


Greg Paulsen:


Yeah.


Jonathan Odom:


Yeah. I think that's about it for the injection molded stuff.


Greg Paulsen:


I was going to say-


Jonathan Odom:


Color and finish though.


Greg Paulsen:


... I'm going to let you start taking off the keycaps on the V 1. Because I do want to show one of the cool things about injection molding is we're talking about these very specific things, but part of the beauty of injection molding is reusability. So I always say custom manufacturing is dating, injection molding is marriage.


Jonathan Odom:


Good call.


Greg Paulsen:


You have a tool there. And in this case ain't broke, don't fix. So there's design improvements for the outer enclosure, but the keycaps worked for those goals-


Jonathan Odom:


Caps are great.


Greg Paulsen:


... And we reactivated the tool. So this was a reorder from last year. So the same tool that we used for last year experience we made for this experience. And, again, when you're talking about this, so you have one enclosure per assembly. But with keycaps what we have, what is it, 11 or 12 per assembly. So you're making thousands of components for this. So it makes sense to keep this with molding. These keycaps Johnson was showing you that shine through on this. These keycaps were overmolded with a clear subset substrate and then we put a opaque black PBT over top of that with a little indicator dot. So there's kind of like this little dot here that will show light shine through. And when they had a vertical mill that was actually doing custom engraving based on what commands you wanted to make a little bit more of a shallow depth to show some of that light shine through.


But this is the design that you see for this injection mold. So it's a dual cavity insert where you have essentially a changeable gate. So in one direction I can make my runner go and make my substrate when I'm doing the polycarbonate, and then I will do my overmold on the other side and switch that around to actually do the overmold. Actually I think for throughput we did a tool modification on this. We actually made two more overmold cavities. So after we made all the substrates, for the sake of speed we were actually doing two at a time here. So here's a GIF of us overmolding those keycaps. And you could see that we're running a vertical press. So one of the things that you have to think about when you're overmolding is I need to put my piece there and I don't want it to fall to the ground.


Same thing with insert molding. If we're doing inserts, so say we CNC apart and then we're encapsulating in plastic. Most of the time you're going to be running those in a vertical press because that allows you to use gravity on your side. You just place the substrate down there and then the mold will shut, inject that material over top, open up, remove piece. So there's exceptions to that rule, but typically when we're talking about overmolding or insert molding services, we're usually going to be thinking in a vertical press for that.


Jonathan Odom:


Yeah.


Greg Paulsen:


I'm going to give you the camera back in a second here.


Jonathan Odom:


Okay.


Greg Paulsen:


So let just start back here.


Jonathan Odom:


Yeah. It's funny in the camera it doesn't glow through as much somehow, but you can see there that LED, the LEDs are at the top. But the etching is done just through the black layer and there's a clear layer behind it and it was really cool watching this happen on the Datron machine. We sort of put these things in a little fixture. You would tap the screen to get the icons you want, the app you use the most, and then they're all etched in. So that's why we did that overmold like that. Really happy with how that came out. Yeah. What's next, Greg?


Greg Paulsen:


So what's next is I want you to talk, so one of the things I'll say is on V 1 we did laser cut the kind of aligners for the key. So it's clear acrylic, so there is something there, you can't see it on camera. But this year you actually used, you had a little Prusa farm to produce that. But I think this is a good time for you to talk about kind of the electro of the electromechanical systems, because we made the mechanical designs, but just walk us through a little bit about the electronics and what you used here.


Jonathan Odom:


Sure. Yeah. So it's a PCB that we had made by Adafruit in this case. We are going to be working with Avnet next year for our new product, which I'm really excited about. But yeah, basically it's a fairly simple production circuit board. You're looking at the back of it right there. We took one of their stock products and made a couple changes to it. So these are threaded studs and we added this little cutout right here and we put in a horizontal encoder to make the form factor better. Having a little knob that sticks up doesn't look great, not terribly practical, but with this design layout you can hit keys and then you can just move your finger over here. It feels very natural.


The key plate for this one, like Greg said, it's acrylic. We laser cut some acrylic for it. Works fine, but I sort of had the order of operations wrong. So if you're a keyboard nerd you know that key plates are the order of operations pretty much all the time is that goes onto the PCB and then you put the switches in. What that does is it prevents you from putting these in the wrong way. With this design, we were relying on people to get the alignment right in a fixture separately and then put the whole thing on here. Works fine if you get everything right, but not everybody did that. So people would push it down and then they'd realize this one's rotated 90 degrees. These will actually fit in any key plate, they're radially symmetrical. So there's three different ways to get it wrong basically with each one. And if you're not really paying attention, it's easy to put it in the wrong way and you've got bent pins and so on.


So one of my colleagues jumped in on this project on the design side this time, Miguel Higgins Moy, who actually just ran in here off-screen and gave me my tools that I forgot to bring up with me. So if you're out there, thanks, Miguel. But yeah, so anyway, he designed this and then with a Prusa print farm just that we have in the office, we were able to just crank out a bunch of these things. It clips onto the PCB, these little clip features right here. There's that other little rest inside there for the injection mold to clip onto. So it's kind of a multi-purpose part. And this material is also more durable and a little bit more flexible than the acrylic. So just another kind of assembly improvement there.


Greg Paulsen:


Absolutely. And the next thing we're going to go into, actually if you want to take this apart and then I'll jump into this afterwards, is this trim. So kind of the black compartments that are holding together the knob and the OLED screen there and just talk a little bit about that.


Jonathan Odom:


Sure. Yeah. So let's see. I can just show you real quick before we get into that.


Greg Paulsen:


Oh, yeah.


Jonathan Odom:


Miguel just jumped, he must be on the webinar, he just jumped in here and gave me this. Pop that off and see.


Greg Paulsen:


By the way, hi, Miguel. He's awesome.


Jonathan Odom:


Yeah. Thanks, Miguel. But, yeah, you can see how this thing pops off of there. There's a little socket. There's obviously an orientation for this, right? There has to be. You can't just put the thing in any given way with a Cherry MX switch. So that's going on with that. Okay. Now the trim parts. So the trim part does a couple different things. So it kind of encloses the knob over here. That's kind of an anesthetic issue, but it's also helps keep dust out of the product. So it's creating an enclosure around that. And then it's also housing the LCD screen. So there's a OLED on a ribbon cable that fits inside this trim piece and the whole thing rests on top of the PCB right here. So this was all one part with version one and now the tricky thing about it is in order to get this screen in a position that's ergonomical so that you can actually see it when you're using the device, it's as close to the surface as it can be, we had to really max out this ribbon cable that you're about to see.


So what that meant was it's really risky to take this thing off and it can be tricky to put it on. So right now I'm holding my thumb down on this side because this little ribbon cable in here is really maxed out. So I'm going to carefully pop that over this way and then you can see how that goes in. So it's very delicate. It's pretty tough actually, but it's still, you can get it wrong, you can mess it up. And right here there's a little latch that pops it out. So you have to align that, get that thing in there. You got to do that before the screen goes in. Or, no, after the screen goes in. So I'm going to push the screen out now. It's in this little trim piece right here and it's very delicate and it's glass and you could pretty easily cut yourself on this if you're not careful.


Greg Paulsen:


Yeah. One of the things we found with, talking about fitment and stack on that is, again, the OLEDs are very repeatable. They're off the shelf and they're predictable. But sometimes how the user integrates that into the mechanical component can cause issues. So even if it's a little diagonal or a little bit of that lip was out when you pressed it all together, you could crack the screen-


Jonathan Odom:


That's right.


Greg Paulsen:


... On that design.


Jonathan Odom:


You could crack the screen, you could damage the ribbon cable. It's got to fit in this little slot right here. Again, super tight tolerances because I kind of didn't know what I was doing. So you got to get it in there. And also it's pretty easy to misalign this thing. Because we use this rubber material, there's a lot of play in that. I can push this in, I can get it crooked on the corner. You can kind of see through there, you can push it too far and now part of your screen is cut off by this bezel. Just a lot of ways you can get this wrong. We ended up having to do a bunch of pre-assembly on this. So, Greg, I'm pretty sure you put your hand in for that.


Greg Paulsen:


Oh, yeah. We put a lot of those together. The goal was a good customer experience at the end for the factory, so we wanted to kind of pre-assemble to mitigate that.


Jonathan Odom:


That's right.


Greg Paulsen:


Now let's look at V 2. Like I said, most of this teardown is V 1 because let's see what V 2 looks like now.


Jonathan Odom:


Right. So V 2, this part is doing the same thing. It's creating a bezel around the screen. It's encapsulating the knob over here. But we separated this into two parts for this one. So here's the first part and that just pops right off.


Greg Paulsen:


Boom, yeah.


Jonathan Odom:


There's a hole right there and a post right there. My colleague, Miguel, who just ran in here, played a big part in the redesign of this to make it easier to put together, to make it more easy to manufacture really. There's kind of another part of this. But yeah, so that's the bezel. It slides on top with this little post feature and then there's a hole right there. And then this just comes off like this. The screen is just resting in there. There's a little lip holding it in the back, screen rests in. Just pull this out.


Greg Paulsen:


It's disassembled itself right now. The knob decided to volunteer off-


Jonathan Odom:


The knob just, yeah, just came right off of there. Yeah, so that's it. You put this thing in. We have these pre-assembled, we put that in and just have that as part of the PCB. You're not really learning a lot. It's not that satisfying necessarily to put this thing in here. So did we pre-assemble that? I actually don't remember if we had those... I think they were separate, yeah. Yeah, we had people do this part.


Greg Paulsen:


It's more accessible because you don't have to do the weird, put it in the trim first and then do that weird S-curve. So yeah, this one's much more accessible.


Jonathan Odom:


Exactly, yeah. And then the switches would've been off at this point in terms of the sequence. So first thing you do is put the screen in there. But, again, super easy. Slide this thing in from the front, you put that in that little lip right there, push it down. Now it's in place. This goes on. Bob's your uncle.


Greg Paulsen:


Bob's your uncle. And I'm actually going to steal the screen back real quick to talk a little bit about that process. So one of the things you're seeing though is you're able to add that purpose and build a purpose-built design. And that's kind of the power of additive manufacturing in a nutshell is that you can design for what your needs are. And even in that first iteration, you didn't need to think about that typical uniformity undercut strap like we do with molding, or accessibility like you would with CNC machining. You could build that purpose-built design and get away with it. We ended up actually moving, because this didn't need that rubbery component anymore, this was moved to a rigid material for V 2. We made this in nylon 12, which is a very ductile material and very forgiving for geometries and just overall super useful, using a process called selective laser sintering.


Selective laser sintering is a powder bed fusion process. So imagine all these parts are being cured together with a laser in a three-dimensional space, which you kind of see these nested builds here. And because these parts are suspended and \[inaudible 00:44:22\] while being made, they don't need traditional support structures. So we get to utilize not just the X and Y, if you think about a 3D printer with that build plate, we actually get to think about it in the Z direction. And what I have on the right-hand side is how we optimize builds in selective laser sintering. So this is what we call computational nesting. This allows you to get to a very economical price point with this 3D printing process because you're able to optimize this spacing around these builds. And you could see here how these parts, we basically get them in the orientation and we set the rules of what we want and then we press go and it'll move really quick at first and then start doing minor optimizations until we get to a point of nest where it's satisfactory for our build density and our goals here.


But you're looking at 450 units in the build. It's very, very straightforward to do. And something where moving this to molding wouldn't make as much sense. These are not the heroes, they're not the cosmetic proportion of the overall assembly and they have this geometry where you have a bunch of undercuts, a bunch of little features that would add so much cost and complexity where it just made sense to move forward with 3D printing for that. On that note, I'm going to show CNC side and then I'll let you finish this assembly showing that in real life as well here, Jonathan. So we CNCed the base and the knob of these devices here. They were precision machined. And actually we used different suppliers, both domestic and international depending on the needs to hit the price point that Johnson's looking for. So we are able to utilize our global marketplace for this using 6061-T6, probably C651 in this case.


And one of the things about that is that's the bread and butter material. It's got fantastic properties, machines really well, performs really well. So typically if you're at a machining project, this is probably going to be one of your first choices depending on your application of course, but it's just such a good material to use. And the base was clear anodized. And then the other side was with a standard finish. One of the things I want to note is that the knob itself, a lot of times when you look at this knob, there's a little lip, you could barely see it in my view here, but this does have some indexing for it because it is a turning knob and it's on a Iridite switch. Or, Jonathan, correct me. It's on something, right?


Jonathan Odom:


Encoder.


Greg Paulsen:


It's based on something. Encoder, there we go. And if you're doing one-off, this probably would be a live-turning lathe job.


Jonathan Odom:


That's right.


Greg Paulsen:


When we're doing hundreds of these, a lot of times as a machinist we think about how to optimize throughput and that may be different operations, different machines or how can I batch this together? So this is a really cool approach. I love actually showing this where you see a round object, but this is actually vertically machined. And I have a little time-lapse here showing through, hopefully it plays, and we had this cool little video, I just took a clip of this, of running this vertical machine. You're seeing about 15 minutes operations, by the way, in 10 seconds here. But it's running multiple steps and basically doing this arrangement of an array at a time. And then I'll run that because it's so quick. And then they flip it over, mill off the backside and you're done. You can see they have multiple setups, they could run their machine and basically indicate per one are those bars, do all of OP one, inspect OP one, and then move to OP two.


So sometimes the way that you think something may be made is not necessarily the way, it may change from quantity one to five or 1 to 12 versus quantities 100 to 1,000 or beyond on that side. So I like that example a lot on how we did that. On that note, Jonathan, I'm going to give you the screen back really quickly and then we're going to finish off with some resources here. But please, please get your questions ready, continue to ask questions. We're happy to talk about anything around this project. The software suites we use, Xometry, just geek out about manufacturing in general. We're happy to talk. So let me give you the screen back.


Jonathan Odom:


Cool. Yeah, so knobs there, right? We did a 3D printed ceramic for the first option. I really liked it, thought it was really cool, but we wanted to try something new for this one and I was really impressed with that method of manufacturing. I wasn't expecting that to happen. I was really surprised when it turned out to be a, there it is, turned out to be a mill operation. I would've thought for sure it was going to be a lathe thing. So we'll show you the base here and some of the changes we made with that. So, again, these are machine screws holding that PCB down, kind of an unnecessary extra step in fastening this thing together. So happy that we were able to use the bosses next time and simplify it all.


So when this comes out, this PCB is resting in these little 3D printed clip pieces. So they have the dual purpose of creating a spacer for the PCB and also creating a feature for those clips to go into. So kind of tough to pull out, easier to put in, easier to assemble. But, again, tight tolerances, kind of tricky. Some unnecessary extra steps in there for someone who doesn't necessarily make things.


Greg Paulsen:


Talking about tolerances. So the rubber pieces in there. We also ended up pre-populating as the time went on because fitting those rubber pieces on version one could sometimes be a tolerance stack itself.


Jonathan Odom:


Yeah. One of Greg's colleagues came down from his hotel room with some hand soap and we were soaping these things and cramming them in here. So really tricky. It's easier to pop them off now when they're a little worn. But the idea is they pop in there, there's an extra little catch feature. A lot of unnecessary stuff in short. You'll notice this one's a little wider. That's for the form factor. The form factor is sleeker, ironically enough, because it's bigger. Makes the proportions more horizontal and vertical than the Z direction. And then the way this fits on the tray is just that. So that's it. Few pockets, set the thing on there. Again, those rest features hold the enclosure on. Enclosure has another rest feature over here that holds the back of the PCB down. Simple as that. So yeah, big improvement there. Big thanks to Xometry for helping with this. Couldn't do it without you guys.


One thing I want to mention though is there are a little things about a manufacturer that make a big difference. And one thing I can always count on with Xometry is that they're going to think about the little things. So the quality of the finish coming out. What do you want this thing to look like? What's the design intent? They're always looking for that when you're having a conversation with them, getting something to be manufactured. First time we went with clear anodized for this finish. We went with a satin clear anodized for the last one, which I really like. I seeing those tool marks on there. It brings the whole kind of manufacturing story into it. Thought that looked really cool.


But I've worked with other suppliers before where they send me aluminum parts and they're just polished and they didn't even ask me for what the finish quality was supposed to be. And it's a part that sees a lot of abuse. And then over time now you've got something that's scratched and customer's not happy about that, you're not happy about it. But you can always count on Xometry to pay attention to those little things and give you what you need.


Greg Paulsen:


Yeah. Well, thank you so much for that. By the way, it reminds me of wedding rings. Where wedding rings is kind of the other way around where if you get a brush wedding ring, it's brushed for a year, then it kind of self smooths or becomes this midline between polish and brush as it works through. Because, yeah, these are the working objects there. So by the way, this is just so fun. This is our first time being able to do a teardown during these webinars. I just love seeing this because this is what we geek out with. I live vicariously through people like you, Jonathan, through my work. I'm not designing this stuff myself, but I just geek out when you see these success stories and how things are applied. All this starts with our instant quoting engine. So when we're talking about multiple manufacturing technologies, multiple designs, we don't know what's coming to us next, but we do use computational geometry and AI in real time.


So you could upload your 3D file, get pricing and lead times, and then configure from there with over 20 different manufacturing processes, extensive finish options. And even if you don't see what you're looking for, I have 10,000 shops, I can make it. You just put in other, write what you need, it'll go to one of our solutions engineering team members who's going to actually review that and provide a price for you as well. Everything we do is secure. We work with government, defense, aerospace. We also hit the certifications that you need, whether it's the state or top QMS systems like ISO-9001 to medical device, automotive, or flight or space flight parts. We're ready to meet your needs there. And we are talking with Jonathan and the Fusion team, we do have an app for Fusion 360 or Fusion, sorry, I keep on saying Fusion 360. But we have an app for Fusion.


It's free, it's available on the app store. You can link it under Xometry's resources tab. You'll see CAD add-ins there and you can get a link to that as well. But this is pretty cool because in your design environment you can first get pricing, like check pricing of your parts, but also we have things like DFM feedback within the app and it helps you put eyes where eyes may be needed. So say for example, on this trim piece, this gap check, it may be a non-critical feature, may be good enough, so it may not be a worry. But if it is a worry, you get that DFM right there and you could go and change your design while you're still in the CAD environment.


So it helps mitigate downstream issues as well as gives you that price point so you know what you're working with. You can continue to customize when you click view Xometry Instant Quote from the app. But, again, we have this app available for Fusion. We also have it for SolidWorks and OnShape as well. So we have it for a few popular CAD platforms. All free and all available either through Xometry's resources tab or if you go directly to the app stores, you'll be able to find those as well.


Jonathan Odom:


Yeah. Xometry has a fantastic web platform. I mean, I can't say that enough. It's such a joy to work with that platform. You're in Fusion, you can just grab a part and say, "Give me an estimate for this." And it's such a useful sanity check at the beginning of a project. You can get a ballpark figure. Obviously it's harder to estimate things like injection molding because there's always going to be so much updating done to the part most likely, unless it's something really simple. But, yeah, I mean there's a sanity check. There's warnings about the thickness here maybe isn't quite a good idea, you need to keep consistent thickness all around. Here's a problem area. All that stuff. The initial analysis is super helpful.


Greg Paulsen:


It's a nudge, right? It's a good nudge in the right direction for consistent parts and consistent expectations. So like I said, 3D printing is very forgiving, but it doesn't mean that it'll hit exactly your design intent if you're not designing for the process. So you get a more consistent experience from the get-go. And outwards, we do that. And honestly, CNC, the number one thing is sharp internal corners. We see that all the time. And this lights them up like Christmas lights when you take a look at it so you can mitigate that right away. On our website, by the way, we have fantastic resources. We have webinars, recordings just like this. So I have a 3D printing versus molding webinar that we did, or versus machining. We have those options. We have downloadable free design guides on all the processes that we offer. So if you're the best in the world at CNC machining, but you don't know much about metal 3D printing, you could download our DMLS design guide and learn a little bit more. As well as we have online tools under our resources page.


And always I recommend signing up to subscribe to our blog because we will keep you updated with what's new at Xometry. And with that, we have some questions coming in. Please keep on asking more questions. And also I want to give you a little takeaway. So KEYPAD25, all caps 25 there, you could use that on any order over $100 for $25 off. So definitely upload some files, take a look at Xometry platform, give it a spin, and we'd love to make some parts for you as well. So, Jonathan, I'm actually going to ask Megan to join on. So Megan is magically in the background here who could MC some of the questions that we have.


Megan:


Here I am.


Greg Paulsen:


All right.


Megan:


All right. So one of our first questions is, "How quickly can Xometry go from part submission to DFM to injection molded parts?"


Greg Paulsen:


Oh, that's a good question. Yeah. So part submission is instant, right? So I'm able to upload my 3D file to Xometry's website. I could choose injection molding and configure to what you're kind of good enough state, you don't need to have every answer. And believe me, we always have a little bit of consultation with our customers just by the nature of molding, especially if you're in that low-volume production to high-volume production range. That's going to actually start a tool bio for you. So we actually have a whole tool workflow that is new Xometry actually from last year we introduced that. And you'll be able to see the steps going through the tool on the preliminary stage as well as you go through those ordering steps. But we're going to reach out, get some more information needed to complete a quote. We're very quick on providing quotes. And then we're also going to provide DFM feedback.


There's an iterative process, so even when you're doing that 3D prototyping stage, for example using 3D printing for your injection molded product, we recommend telling us that you're looking to mold because we're going to start giving you that molding DFM while you're still prototyping and save you some step. We are very much working in an agile state versus a waterfall state when it comes to manufacturing. And because we offer anything from prototype to production as our services, we want to have these conversations with our specialists in parallel.


Megan:


Great. Next question. "What is the industry standard for surface finish on injection molded parts and where can we find examples?"


Greg Paulsen:


I'm in the US, North America, SPI is a very common standard with injection molding. So for example, SPI A-level is going to be more of a polish where A-1 would be like optical glasses, and then B is kind of more of a brush, and C is more of a stone, and D is more of bead blast finish. So there's a different scale there. And we do have injection mold finish cards that you can, if you search injection mold finish on Xometry's website, you'll see a link there and you can even get a finishing card from a link there and that's going to show you those different finishes.


Another very common international finish is going to be VDI. So VDI, I would be... This is one of our finish cards. So I have my SPI side, you can see going from polish to matte, so A-level to D-level. And then VDI would be more like EDM finishes on that. But this is the language in which you talk to your tool makers. And so we have a great resource. I made the page, so I like to think it's one of the better resources we have on finishes for injection mold. As well as our specialists, we have solutions engineers in molding that could go even deeper in that conversation.


Jonathan Odom:


Yeah. That little card lives on my desk.


Greg Paulsen:


Yeah, it's handy.


Jonathan Odom:


\[inaudible 01:00:40\].


Megan:


Next question is for Jonathan, "What is the main difference between Autodesk Inventor and Fusion 360?"


Jonathan Odom:


Yeah. So there are a lot of overlaps obviously because they're both CAD packages, but there's some significant differences. So first one is that Inventor is PC only, whereas Fusion is Mac or PC. We're also working on full device independence for Fusion. So look for that in the future. Another difference is that Fusion has an electronics package built in, a full electronic suite. You can do anything with it you can with any other competitive app. And that is also within Fusion. It also has a CAM package built in, so basically everything is in one data set and it's all automatically connected to the cloud. So you have basically all the aspects of whatever the product is, whatever your project is, are all there. And it's also automatically got collaboration tools. So you can see the people tab over there on the upper left, that's your project browser. You can click on that and then see who else is in the project.


Greg Paulsen:


It's just me right now.


Jonathan Odom:


Greg's a lonely guy over here. But yeah, that's a big one. You can see who made what revision. All that stuff is tracked. You can revert to different revisions. So if you look at that browser on the left, you can see V 1 in each one of those designs. And you can revert to other versions, you can track versions. All that stuff is also built. Another difference is that Inventor is great for large assemblies. So I think of it as Inventor's great if you're designing a tractor. We have a whole lot of parts, some really complex mechanical assemblies, that kind of stuff. Inventor's really good for that.


If you're doing anything with ITAR, if you have a defense contract or something like that, you can't use Fusion because it's all on the cloud. Our servers aren't ITAR compliant, at least at this point. Yeah, I mean those are some of the salient differences I'd say. Like I said, there's overlap. You can do a lot with both. But generally speaking, Fusion is kind of an end-to-end solution that has everything built in. And Inventor is a little bit more focused on CAD and large assemblies.


Greg Paulsen:


And I was going to say, so for example Inventor, we do work a lot with the aerospace defense and Jonathan was mentioning ITAR, which Xometry works on ITAR projects. In Fusion, you would export essentially your project as a Inventor file, like an IPT file or a step file which is the universal to use Xometry, or we do that conversion for you through the app itself. When you bring it up to xometry.com site, you can upload natively Inventor files, IPT files directly to our site without any issues. The F3D files its own thing.


So we are a little over, I going to answer a couple more questions. But please don't stop asking because like I said, I'll follow up with questions afterwards as well because we want to address all your needs. So, Jonathan, I saw one, and actually, Megan, I saw this one from Charles here, which I think is a good one because we were talking about Poka-yoke the other day. When Jonathan was presenting on how to assemble the macro keypad and kept repeating that you could easily make a mistake in the assemble process. Is it possible to incorporate foolproof features on your components to create room for easy assembly?


Jonathan Odom:


Absolutely, yeah. And we try to always do that. So Poka-yoke, if you're not familiar, is a standard that Toyota came up with, I believe. The idea is that you cannot put two parts together the wrong way. So there are features built in that prevent you from doing that. One example would be there's a little post on your hub for your car wheel and that post goes through a little hole so you can't orient the hub the wrong way. And we did that with all of the parts on both of these products. You really can't put them together the wrong way, it's obvious when it doesn't work. But some of the issues we ran into when it comes to making a mistake is that the tolerances were so tight that it's conspicuous that it's not fitting together properly because it's bowing or things aren't quite fitting together, but technically you could kind of take it away with it working.


Greg Paulsen:


All right. I got one last question. I'm going to see if I could get the visual for you as we're talking through. And then, again, please keep on asking questions and we'll get to you post webinar. But Tommy asks, "On the enclosure, there are four screw bosses and rib features that connect the bosses to the perimeter frame. So these little guys right here, what's their purpose?" And I am happy to talk on that or Jonathan or if you want to talk to that as well.


Jonathan Odom:


I'm sorry, I was reading. Which one was it?


Greg Paulsen:


Okay. So this is on the four screw bosses, this little guy here, this bridge.


Jonathan Odom:


Right. Basically it braces that little boss, right? So if it didn't have that, the boss would just be sticking up and it would be easy to bend or break. And also that it gives the plastic somewhere to go. So you wouldn't need a gate at the end of it, for example. I'm probably starting to talk a little bit here. But, yeah, it wouldn't need a vent because anytime you can connect parts in an injection mold, it's going to make it stronger and it's going to make it easy for that molten plastic to move around in the mold. Which Greg can talk about way more than I can.


Greg Paulsen:


Yeah. But those are correct answers. So this is one of those things where how do I keep a uniform wall thickness and also add strength. There's a reason why you can cut a stake with a plastic knife. It's design. That if you look at a plastic knife, you actually see that it has a T cross section. It has that brace to it that allows the action of cutting to be stiff in that direction. And the same thing, you're not just making a thicker piece of plastic. And what we don't want is for you to brace a boss that could be very fragile by just making this whole area solid plastic because then you'll actually get a big sink mark, which is basically when the plastic cools around your part it'll actually make an impression, a very visible cosmetic impression, on that part.


So this is a great way to add strength while keeping the rules of wall thickness and even wall thickness when designing for injection mold. And by the way, we're all about this. We have some great guides on this, including some cool boss tricks that you can do to mitigate sink within our design guide. On that note, Jonathan, this has been awesome. We did some practice runs. I was like, "This is so cool. I can't wait to show everybody about this." And it really was, was just a really cool event. Thank you so much for your time and teach us a little bit about this and your experience as well.


Jonathan Odom:


Thanks for having me. I love doing this. I love talking about this project, it's one of my favorite things I do. And I hope it was useful, I hope you all learn something. And I also hope you come to AU this year. I don't think we've announced anything yet as far as when and where, but we will have another factory experience there with Xometry once again. And we've got a really cool new product I'm excited about.


Greg Paulsen:


Yeah. You guys should follow Jonathan on LinkedIn. I think he was showing some teasers the other day, but very excited for the project coming up. And with that, thank you all for attending. I know we went a little over, but I think it was some great questions and have a wonderful day.


Jonathan Odom:


Thanks everybody.


Greg Paulsen:


Cheers.


**Start Your Instant Quote**


STEP · STP · SLDPRT · STL · DXF · IPT · X_T · X_B · 3DXML · CATPART · PRT · SAT · 3MF · JT


[Upload Your Design](https://www.xometry.com/quoting/home/) All uploads are secure and confidential
