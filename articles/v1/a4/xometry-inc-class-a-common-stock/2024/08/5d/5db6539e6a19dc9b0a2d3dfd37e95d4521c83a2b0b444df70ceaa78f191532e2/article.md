---
schema_version: "1.0.0"
document_id: "5db6539e6a19dc9b0a2d3dfd37e95d4521c83a2b0b444df70ceaa78f191532e2"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/blog/advanced-tips-for-cnc-designs-and-drawings-webinar/"
published_at: "2024-08-08T18:43:26+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:e5fc5a2a6c998a5a858e7ef8f70e7f20363d47d7ea81a79ae9e284b3550897b0"
---

# [Webinar Recording] Advanced Tips for CNC Designs and Drawings

## Advanced Tips for CNC Designs and Drawings


Xometry surveyed machinists and CNC design for manufacturing experts to gather tips and guidance to prevent common mistakes or headache-causing issues during manufacturing. From defining datums to drill depths, this webinar will cover the best practices for CAD design and technical prints in this 1-hour session.


Xometry’s Greg Paulsen, Director of Applications Engineering, and Steve Zimmerman, Machinist, Toolmaker, and Senior Partner Support Lead, bring a practical approach to design for manufacturing on machined components. They explain what to do and why these tips matter when manufacturing. If you’re a designer or engineer seeking to improve design efficiency and leverage the full potential of CNC machining, you don’t want to miss this webinar.


Key Takeaways:


- Understanding what features require specialized tooling like EDM, gun drilling, grinding, porting, and more.
- Learn common design mistakes and how to avoid them using CAD or technical prints.
- Garner an array of practical tips and tricks from our industry experts to help you design more cost-effectively and efficiently for CNC machining.
- Review Xometry’s inspection and certifications and when they should be used on a manufacturing project.


The presentation concludes with a Q&A session which is summarized below


### Questions and Answers from the Webinar


GD&T?


Geometric Dimensioning and Tolerancing, GD&T, is a system for defining and efficiently communicating design intent and engineering tolerances. You can learn more about GD&T on[Xometry’s blog](https://www.xometry.com/resources/blog/what-is-gdt-and-how-is-it-used/) .


Is it more cost-effective to model in small edge breaks/chamfers or callout break all edges on a drawing?


We recommend not modeling edge breaks as a best practice. You can call out requirements in a drawing.


Does changing tools in CNC cost up?


CNC machined parts are almost always made with a set of different tools, so that is not a unique cost driver. Cost on parts can be driven by specialized tooling needs, surfacing requirements, or features that require multiple or separate operations.


What is the roughness (spec) when completing with a standard end mill?


The standard, as-machined surface finish is 125 Ra or better. Machine tool marks may leave a swirl-like pattern.


What about floor thickness for metals?


We would recommend a .030” minimum wall thickness, including floors.


Does Xometry offer part review for cost optimization? During the quoting process, can you identify cost drivers so we can address those if necessary?


Xometry has a Solutions Engineering team of experts who can review your design and give feedback on what may be driving costs. For more information, contact your sales lead or[Xometry Support](https://www.xometry.com/contact-us/) .


Is 0.015" a standard endmill floor radius?


Yes, endmills can be readily purchased with .015” corner radii. We also recommend .020”, .030”, and .060” or no floor radii at all.


Would you recommend adding a slightly larger diameter for internal corners (i.e. 0.255") instead of 0.25" for an easier tool path to meet tolerances for a 0.25" tool?


Yes, about 30% more is a good rule of thumb. This helps keep the endmill tool evenly engaged when making the radius.


What is the best way to call out an edge break if you are not including a drawing? Do you have a common edge break if we leave it optional?


Xometry’s[Manufacturing Standards](https://www.xometry.com/manufacturing-standards/) include Workmanship, which will assume edge breaking.


Do we need to call out MINOR Diameter for an internal thread? Or Major Diameter for an external thread?


Design the minor diameter, or drill diameter, for internal threads. Design to the major diameter on external threads. Why? Because we need material to cut!


What is preferred, model threads on the uploaded file or just call out the threads on the drawing?


Call out on a technical print. Modeling threads is good for renders but not too useful when machining parts.


What are the most common issues you see with manufacturers’ ability to interpret “design intent” from technical drawings? What are the biggest issues with the technical drawings for high-precision parts (with GD&T)?


Getting the datums called out correctly. Datums are the foundation for tolerancing on prints and affect how the machinist sets up parts and how our inspection inspects features. Try to avoid theoretical datums and focus on feature-driven datums like planes, edges, or cylinders.


What is the standard tolerance that you guarantee if we do not provide tolerance on features?


For features of size (Length, width, height, diameter) and location (position, concentricity, symmetry), +/-.005” (metals) or +/- .010 (plastics and composites) following ISO 2768 unless otherwise specified.


Is Model Based Definition (MBD) needed in the part file when providing a drawing that is not "over toleranced"? If not, what general tolerances are applied to features if they aren't called out on the drawing?


Xometry offers[Manufacturing Standards](https://www.xometry.com/manufacturing-standards/) that outline standard expectations of tolerances and workmanship in the absence of a drawing. Model Based Definition (MBD) or Part Modeling Information (PMI) will need to be relayed in a drawing or 3D PDF because files uploaded to Xometry are converted into STP where information may be lost. As a digital platform we are exploring ways to automate MBD/PMI in our quote and fulfillment process.


How is a general profile tolerance any different than using Cartesian title block tolerances like +/- .005"? Are those not measured under the same inspections?


Profile callouts require a dot matrix of points measured via CMM or vision inspections to build a surface map of the part and check for deviation. Title block tolerances can be measured with manual tools such as calipers, micrometers, pin gauges, etc. So, by calling out a profile tolerance, you are explicitly asking for a CMM inspection.


When you run the CMM, you compare the physical part to our CAD model, correct?


Correct! CMM will have the CAD model of the part and we use that to program a measurement path. We then take physical “pings” of the made part and compare to the CAD and drawing to create a digital, objective report.


When would you prefer to switch to ISO tolerancing (vs nominal)? In your perfect world, would everything be a JS/JS fit and modeled to nominal? Or are the preferred ISO fits okay?


We read all types of drawings, so JS/JS fits or ISO are okay. What’s most important is communicating this information during the quoting stage so we can translate those requirements into the ordered part's manufacture.


Should I be applying datums based on my part’s function or how my part will be inspected?


Function!


Would you rather have the nominal tolerance as the low/high end and the single direction tolerance than a +/+ or -/- tolerance?


Usually we see +/+ or -/- tolerances on fits that may need to be adjusted/redlined after the first part was made. I prefer using unilateral tolerances for fits and designing to the nominal measurement in CAD. This helps mitigate misreads of a print when setting up CAM paths and causing unnecessary scrapping of a part.


If we have a surface .500" thick plastic part where we just need features cut in the part but not surfaced, would we indicate that the surface does not need to be cut, and would that reduce the overall cost of the quote?


Machining from plate stock and leaving the top and bottom untouched is a great way to reduce costs of manufactured goods. In our quoting engine you’ll need to choose Material: Custom/Other and write in the plate stock and its size. Submit that for a quote review, and our team will return better pricing quickly!


Should notes be added for post-anodising sizing? If a part has a toleranced thickness and needs to fit into something else, would the thickness be for pre- or post-anodizing?


Tolerances are assumed pre-anodize unless your technical print calls out that tolerances apply after anodize. Just make sure your print is calling it out, and our machinists will compensate the model for anodize buildup (e.g., for Type III hardcoat).


If I design in metric, should I adjust the design to nominal English dimension?


Distance is distance, no need to convert! We work between units all day every day.


Do you have any tips about external part size vs. stock size? For example, what is the maximum thickness part you could cut out of 0.25" stock? I'm looking to minimize material waste/machine time/cost.


How much material is removed depends on the stock’s and the part’s tolerances. These decisions come into play when ordering raw materials. For example, we may add .25” to each dimension to have enough to face and square the part for the first operation.


What percentage of your manufacturers are US-based?


Xometry started in the USA, and we have thousands of manufacturers in the US. During the quoting phase, users explicitly choose their project’s manufacturing location, domestic or international.


What percentage of work does Xometry control via its manufacturing network?


All work fulfilled through Xometry is done by suppliers vetted on their network using Xometry’s AI-driven manufacturing marketplace.


Are there requirements for only doing work within the USA?


Customers can choose the manufacturing location during quoting. We can also default accounts at their request for domestic-only, ITAR-only, or other supplier restrictions. Either way Xometry’s quoting platform is transparent on the origin of manufacture at the time of quoting.


What is the best way on a drawing to express that a feature isn’t critical and could be left up to the machinist to adjust for a more optimal tool path?


You can loosen tolerances on non-critical surfaces. This is common practice on things like post-machined castings.


In the past, I learned that a part should be entirely reproducible from the drawings, even without the 3D CAD file. Have I understood you correctly today that low-tolerance features need to be dimensioned on the drawings?


In the last decade, we have seen a swing from “print is king, CAD is a reference” to “CAD is king, print is for the details.” This is due to the increased usage of computer-aided design, manufacturing, and quality control. All of this is on the digital thread, so it makes sense to move in the digital direction. Ultimately, we see CAD *embedding* print-level information in the digital file, removing the need for a technical drawing entirely through part modeling information (PMI) or model-based definition (MBD). In this new model, drawings may be generated by themselves based on embedded model information and best practices. Oh what a time to be alive!


Do you demand submission of 3rd angle projection drawings?


3rd angle projection is most common in the US where 1st angle projection is common in Europe. We read all of them just make sure the projection is explicit on your title block.


Is it practical to do a prototype in a cheaper material (i.e. plastic) to verify a fit, then do the final in the actual (say aluminum) material?


Absolutely and we do this all the time using 3D printing methods to prototype a machined, molded, or cast component. If you are prototyping in CNC I’d recommend choosing 6061-T6 aluminum as the most affordable material. Check out my blog post on[Ways to Save Money on CNC Prototypes](https://www.xometry.com/resources/machining/ways-save-money-cnc-prototypes/) .


How do you ensure that high-tolerance parts are sent to manufacturers with appropriate capabilities?


Through our network, Xometry suppliers can expand on the type of work, machinery, inspection options, and finishes they would like to take on. We also use AI to match based on parts and features. All of this helps ensure that we are pairing projects with the right suppliers for successful outcomes. If there are any issues with your parts we take responsibility and will work through RMA processes to make it right.


Can you explain the “build and hold first article inspection” option? Does this allow me to make small design or dimensional changes after receiving the first article? Or is this really just to give me the opportunity to approve the manufacturing quality and methods?


If you are still in the prototyping phase, we recommend buying what you need to evaluate. Build and hold options are for the first article in-hand production approval. Design changes, if needed, may require a cancellation and reorder of the new design. Any individual part fees will be absorbed into that first article if canceled early.


What percentage of your business is 3D printing? What are the cost limits versus the size of the component?


Xometry offers a variety of manufacturing technologies, with 3D printing supporting seven of its processes. We choose not to disclose the percentages but we are a market leader in additive manufacturing. Check out our[3D printing services](https://www.xometry.com/capabilities/3d-printing-service/) page and navigate to our different capabilities to learn more about sizes and tolerances.


Does adding GD&T to a print automatically drive up the cost because it may now require the use of a CMM?


No, but some GD&T callouts may dictate the need for a CMM, particularly ones that require 3-dimensional readings of a surface, such as surface profile or cylindricity. A CMM inspection does cost more than a formal inspection report.


What are the different capabilities between CNC machining and Metal 3D Printing?


Check out our webinar on[When to Choose 3D Printing vs. Machining](https://www.xometry.com/resources/blog/on-demand-webinar-3d-printing-vs-cnc-machining/) for some pointers!


Can Xometry obtain drawings for the major auto manufacturers so that we can produce locally?


Our customers will need to provide the technical manufacturing information, such as a 3D CAD model and technical drawing.


Is 3D printing material shrinkage accounted for on Xometry's end, or should models be adjusted prior to submitting an order?


Shrink is typically compensated by software, but each process has its design nuances. Check out the specific design tips on our process pages under “Our Solutions” on Xometry’s main site.


As someone new to this, are there any CAD systems that you recommend NOT to use?


Use a CAD system that can perform solid modeling. Any mesh based modeling like Tinkercad or Sketchup will not be able to produce manufacturable files for processes like CNC. For new users I like Onshape, Fusion, Solidworks (if you can access), Alibre, and FreeCAD.


How do you feel about drawings that are multiple pages in order to give context to a part?


It’s digital. Pages are free. If you need many pages to make your part’s context legible, please do that rather than cramming a bunch of important things into a small space.


## Full Transcript


Greg Paulsen:


All right. How's everybody today? I'm going to start right at the hour just because of all the content that we're going to be covering today. So just very excited for today's live event. Welcome, everybody. My name is Greg Paulsen, I'm from Xometry, and I'm excited to talk to you today about Advanced Tips for CNC Designs and Drawings. So before we start though, let's lay down a little bit of groundwork. We're going to begin with introductions. We're going to tell you a little bit about Xometry, what we do and how we do it, and then we're also going to go through the nitty-gritty of the CNC, advanced tips, technical drawing tips, and quoting special requirements and other things that may drive cost and may merit that conversation along the way. Our goal today is to walk you through CNC machining design principles. So common features, what drives cost or complexity, some tips on communicating intent with your drawings, making great technical drawings, and I'm bringing on my co-host Steve Zimmerman, who is an expert machinist and toolmaker to help guide us along the way as well.


So our team at Xometry, we talk from our experience, I'm working on thousands of projects from every industry you could imagine. We love answering questions and I always, always, always encourage you to use that Q&A tab in the Zoom webinar box there and ask questions along the way. We're going to save time at the very end. My colleague Megan's going to help us through some of the Q&A session at the end here, but please ask questions along the way and we'll try to get to as many as possible at the end of our webinar event here. And I'm really excited about that because we love context. I think Steve and I are professional question answerers at this point. So that's what we like to do the best. And of course this webinar is also recorded, so we're going to have a recording of this webinar on a blog post. It'll be posted a few days after this event. And along with that, I'll try to answer some of those Q&A and common topics that we see in that blog post as well, so you'll have something to reference to and share with your colleagues.


So let's introduce the team there. So I'll start with myself here. My name is Greg Paulsen, I'm the Director for Applications Engineering here at Xometry. I work on a lot of projects across a lot of disciplines, talking about design, spinning CAD with our customers, looking at technical drawings, helping customers figure out what process to choose and when. My background is over 15 years of advanced manufacturing, started in industrial additive manufacturing. But over the years, I've expanded working with people like Steve in learning things like machining, sheet metal fabrication, injection molding, and I love just being a sponge for manufacturing design principles. And yeah, along with me here is Steve, who is an expert toolmaker, 30 years experienced machinist, and you are a Senior Partner Support Specialist at Xometry. And Steve, tell me a little bit about what that means. What is a senior support specialist?


Steve Zimmerman:


So I help our partner network with any kind of issues they run across in machining or applications engineering, when it comes to CNC world. I also help onboard all our new partners coming into the network.


Greg Paulsen:


And that's something really exciting. So Steve is there working with our suppliers and our partners as they join the Xometry network and are taking and producing work and working them through their first jobs because beyond just making parts, we are a digital manufacturing platform, and guiding the suppliers through gives the best experiences. It's a great experience for you, our customers, who are designing parts and looking at parts to quality and on time. And it's just terrific feedback that we have here.


So like I said, I have a lot to cover today, lots of topics, and this is something that people ask about so much. So we wanted to put a dedicated webinar to this. So let's just jump in and get started. I'm going to start with what we are. So who are these guys talking to you? What do we do? What is Xometry? So we are a digital manufacturing marketplace. We serve nearly 60,000 customers in making custom manufacturer parts on demand, offering over 20 different manufacturing processes, and we do this through a digital platform experience. We are a two-sided marketplace, which means that we are connecting both the buyers, the customers, those who need things made, and these suppliers, or as Steve says, like the partners who are producing those parts, all throughout a digital platform experience that is Xometry. We are publicly listed as XMTR. We offer full manufacturing services and we're global. So Steve and I are reporting from Xometry North America. We also have a European division as well as an Asian division. So we're just slowly conquering the world, is what I'm trying to say there.


And our manufacturing landscape is pretty vast, over 20 different manufacturing technologies, bread and butter, is going to be machining, sheet metal fabrication, sheet cutting and additive manufacturing. And then we have the formative processes to help you scale from where you are in a product development process to where you want to be in sustained production or wherever you are in that product development journey, we're here to meet you. So injection molding can be bridge tooling or full production tooling. We do die casting services, plastic, metal extrusion, metal stamping, in other formative ways. And on the fabrication side, if you're using a bent tube, laser-cut tube, sheet-cut, water jet, laser-cut, all that is within our capabilities as well. So really truly a one-stop shop. Xometry is also related to Thomas. Thomas is a Xometry company and a lot of you are familiar with what Thomas does, or ThomasNet.com. That is a global directory of manufacturers. So if we don't have it directly at Xometry in our coding engine, you're going to find your supplier via Thomas. We want to be the first place you go to get custom parts made.


Of course, today we're talking CNC machining. I keep on mentioning a digital platform. What we call the digital experience for our customers, we call it Xometry Instant Quoting Engine. The Instant Quoting Engine is a secure web-based platform where you can upload your technical file, a digital file, and we accept most CAD file types, and it'll provide, via AI machine learning, instant pricing and lead times on your part. So a few things happen, it'll interpret that part. Say it's a CNC design part, it can actually predict saying, "Hey, we think that part's a CNC." It'll default to CNC machining, provides you pricing for the standard CNC materials like aluminum 6061. Then from there, you could go and click drop-downs, configure your part and go as deep as you need. If you need to add specific tolerances, inserts, threads, tapped holes, part-marking, bag-and-tag finishes, CMM inspection, ITAR, facility requirements, they're checkboxes. It's like the easy button for sourcing custom manufactured parts.


We offer multiple lead times from the standard to expedite to economy and international economy options. So you have choices along the way, and we meet you where you need us to be for your quality of facility plans. So we've gotten certified along the way. In fact, our last certification that we got was IATF 16949 for automotive part production, but we also do medical device, aerospace, space flight parts and everything in between there. Long story short, we're a place to make parts. We love making parts for you and we make it really, really easy. But today we're talking about one of those technology branches, CNC Machining.


So CNC Machining, what is that? It is Computer Numerical Control. That's what CNC stands for, in case you didn't know that, but it's a very broadly used manufacturing process. It is computer-controlled. Someone who's programming a part for CNC Machining is using CAM, Computer-Aided Manufacturing Software, to interpret that design and turn it into code that'll be interpreted by that machine. When I'm thinking about what a CNC machine is, I like thinking about a robot, but the arms aren't on the outside, they're on the inside. They're going to be moving the part body around, twisting and turning it as a milling feature or drilling feature is going and creating those cuts to produce your manufactured part.


It's something that you know from industry. When you look at parts and you see that smooth finish on anything, even your plastic parts, it turns out that tool was CNC-machined. If you look at stamp metal parts, those stamping and dye tools, CNC machine components. This is the backbone of most manufactured goods, whether it's directly making those parts or building the tools to make those parts. And it's what we know, as designers and engineers and manufacturers, as something that is just a very reliable way to hit precision tolerances. You're cutting away from materials so you're able to get smoother finishes versus process like additive manufacturing where you're kind of growing based on base material, and that base material gives you what that surface finish looks like. This is actually a removal process.


We know a lot about materials. I could for example, order 6061 T6 aluminum alloy from about 2,000 vendors and have very high levels of confidence that I'm going to get the material with the properties that I require based on my engineering needs. And it's just got that track record and it also has a lot of redundancy. Xometry's manufacturing network allows us to tap into open capacity both domestic and globally depending on your requirements and start jobs right away. So lead times on machine parts can be as little as a couple business days. And of course when you add complexities, finishing, secondary operations, that adds up, quantities will add up. But we can start with very quick and then build upon what your needs are. So talking about that, a lot of times you'll hear the vernacular of 3, 4, and 5-axis machining, and Steve, love to talk to you a little bit about what that means. If you could explain when we're talking about axis machining.


Steve Zimmerman:


So when it comes to axes, that's the number of axes that you're actually programming in the machine. So 3-axis part, you're programming in X, Y, and Z, to where you can get everything in an X, Y, and Z motion. When you're talking about 4-axis, you're adding a rotary in there, to where you can get to compound angles a little bit easier. Less setups when it comes to fourth axis. And then fifth axis machining, you add a second rotary to that, so now you have five axes that you're completely programming separately. This can cut down on setup time but can also in improve machining cost as well, when you get into 4 and 5-axis.


Greg Paulsen:


And I think you mentioned cutting down setup, so when I'm building a part in machining, right, I'm fixturing in place.


Steve Zimmerman:


Correct.


Greg Paulsen:


And then I'm moving, and whatever my tool can reach and access for what makes sense for that operation, we call that an op. And then you'll change your fixturing around, sometimes you need to make tooling to hold that for a second op or third op or fourth op, et cetera. And if you have features like this design here where these holes are on plane to each other, but they're off plane to the rest of the holes and same on these holes on this opposite side, if you add a twist right, add an axis, then you could actually make those features within the same op as the other features there. And then fifth axis, you could do even more with that. But yeah, they do drive pricing, as far as higher overhead of machines, but to your point, yeah, they definitely can, depending on the geometry, it may just make sense to use a higher access machine for specific features.


Especially if those features have some sort of GD&T relationship. So a dimensional relationship to themselves, as well. But yeah, this one is kind of hard to see on the image here, but each one of these holes is actually annoyingly at a different plane. So a little bit of tilting and moving, which makes this a truth fifth axis component here because you would have to do a specific operation fixture for every single one of those holes if you were just working in a 3-axis kind of top down motion on those parts.


And tools of the trade. So we are spinning tools in CNC machining, so we're using tools to remove material, and each one of these has their different... I guess, each one of these will be able to make different feature sets, and we're going to go over some of the principles of those feature sets in the next few slides here. But Steve, if you want to go through some of these, we could go from left to right, on just end mills going on.


Steve Zimmerman:


So all the way to the left, that's just a standard end mill. It's got a flat, no corner radius at the bottom. This would be used for machining pockets or voids down inside a part. The next one is just a regular twist drill, like a jobber twist drill, you could make through holes or you can make blind holes. The next one would be a tap, so that would be ran down through a hole after the drill to machine the threads in. The next would be a key seat cutter to where you can get undercuts and underneath features and they come in all different shapes and sizes. The next one would be a ball nose to where you're actually using that to surface a feature. So down inside or on the outside of parts, you can use that and do micro-stepovers to actually put features, kind of like flow features into things.


Greg Paulsen:


And by the way, I'll say this is my PowerPoint skills. So keep in mind this would usually be rounded in the real features here. This is just my extent of being able to make a PowerPoint or slide deck to show off these features.


Steve Zimmerman:


And then the next one would just be an edge break tool, a chamfer tool for breaking edges so that there's no sharp edges for somebody to cut themselves on. And then the last tool, that's what we use to hog a lot of material off. So that's a face mill, that would be to take a lot of material off the top of something or the bottom of something to bring features closer to their intended design.


Greg Paulsen:


And operationalize. When we were building this slide deck, I put this at the end, but really this is often the beginning. So when you're doing details, you may be using an end mill to go and create those paths to do detailed cuts and get into nooks and crannies, but if you just have a broad flat face, you're just trying to hog out material, save time in your machine, you could see this kind of swirl pattern looks a lot bigger than the swirl pattern on this side, and this would be like a face mill clearing off that material going back and forth. And what it does is it saves that overhead time on the part manufacturing for that operation. And time is truly money in this case, time is cost on those parts. And that's a great segue.


So we're moving into... I think the next couple slides are very, very important, especially from the design principles of advanced design principles for CNC Machining here where we want to talk about what's common, and this is typically what we recommend in our design guides versus cost-driving features. So it's never a question of can you make it at Xometry? I have 5,000 shops. We could make parts, make parts of any type of geometry shape or size. It's just more about how much cost does this need? Are there special features? Do I need to add secondary operations? So when you're thinking about Xometry, don't think of us as a single shop when you're asking questions like, "Can you do X?" Or, "Can you cut X?" Like 99% of the time, the answer is yes. The real thing is, does this design incorporate risk or additional features that you may not be aware of that's going to drive cost, lead time or risk to the manufacture of those parts?


And yeah, let's start with the first four here. These are some of the common ones that we have. And Steve, I was thinking for this one, I'll kind of talk about the features, the recommended and then you could talk a little bit about the cost drivers and how you see that through your machining side as well. But the first one is a common one we get asked all the time about which is wall thickness. I'm going to give you the recommended minimums. And I wish there were little asterisks that are appearing all over my screen right now as I said the word minimum, because keep in mind, minimums are like you shouldn't make the whole part to a minimum feature side. That would be like going to your auto store and buying brakes at the minimum thickness. You want to use minimums where you need them, but you don't want to increase risk across your parts.


So minimum recommended size for metals is about 30 thousandths of an inch, 0.030 inches. And plastics, typically double that, for that. And that's because of some vibrations and we're quoting needs for plastics, they're a little more wobbly by their nature. But yeah, cost drivers for wall thickness?


Steve Zimmerman:


So anything that's going to be a real thin wall, whether it's in metal or plastics, going to drive up manufacturing costs because we have to slow the machines way down in order to achieve the correct surface finishes. It might even be a different approach of the manufacturing altogether. But at the end of the day, it definitely drives cost because it slows the machines down.


Greg Paulsen:


Yeah, absolutely. And it's interesting though, internal corners here, we have a one to four diameter depth ratio recommendation. Which is again, what can I cleanly cut into with my end mill? And we also have recommendation to not make that diameter the standard size of an end mill or drill, make it a little bit bigger because that allows you to cut it and cut around with an even engagement of the turning tool. So we have this little bonus, give about 30% bonus diameter, on internal corners. But yeah, one of the most common things we see is leaving sharp.


Steve Zimmerman:


Leaving sharp corners. You got to remember our cutting tools are round, so you almost have to leave some type of corner radius in there or you're going to get into an EDM or broaching operation, which is going to be a complete different machine. It's going to be another setup, which is definitely going to drive up cost.


Greg Paulsen:


And if you have a smaller radii in there, it may look kind of rough and chattered because the tool just is overextended.


Steve Zimmerman:


Correct, yeah. When it comes to cost drivers with manufacturing, it's machine time and tool wear. So the more they can keep that tool engaged and taking the metal removal off at the same amount of time, the longer tool life they're going to get out of that.


Greg Paulsen:


Yeah, absolutely. And this is actually, by the way, I liked when we were making this slide together, I even had this question for you Steve, and you helped give me guidelines, so thank you so much for this. Floor radii. So floor radii, I'm going to jump back to this real quick. So this is end mill creating pockets, and you can get end mills with rounds on them. So recommended is either just leave it, don't radii at all, because you can cut flat on a floor in a pocket or 20, 30 or 60 thousandths is common for a tool. And don't over-tolerance that, but that should be, we call that a reference dimension for example, down there, and we would be able to mill that out with a standard tool without having to custom surface that radii.


Steve Zimmerman:


Yep. Yeah, the main thing when it comes to floor radii is just keeping stuff standard. So your standard end mills come with a 10, 20, 30, 60 thousandths radius, so you want to keep it a standard tool radius. Once you get into 50, a hundred thousand, that kind of thing, then we're actually surfacing that which is going to drive machine cost up and machine time.


Greg Paulsen:


Yeah, absolutely. And edge brakes, actually I saw someone had a question on edge brakes as well, and Justin asked had this question, we'll cover edge brakes in like two sections, but this is section one of edge brakes, which is, you don't need to model them. We do edge brakes as workmanship, it's just part of Xometry's manufacturing standards on machine parts. We don't want you to cut yourself when you handle machine goods. So we actually do edge brakes there. If you're designing or calling them out on the drawing, 45 degrees is standard because you could use a standard tool to create those features. The second that you don't have that, you have created a feature, not an edge break. If you created something that needs to be modeled in the design and produced.


Typically, best practice is not to exceed 30 thousandths of an inch for edge break, but you also don't want to tightly-tolerance that if it's just something that's edge break because we don't want the rejection criteria to be like an edge break that's off by a thou or something like that. So cost drivers is, yeah, anything non-standard. I kind of covered that. But yeah, don't tolerance it too tightly there.


I'm going to go to the next one here. So continue the second page here is actually mostly around drilled holes and pockets here. So when I'm using a drill, so versus an end mill can go down, plunge down and also move left and right, up and down in order to produce cut pocket features. A drill is creating that round hole straight down. So I can have a longer aspect ratio to it. We want to keep it less than 1 to 10 diameter to depth ratio as a basic guideline. You can push it a little bit, but let's talk about recommended, but there is specialized requirements. And Steve, actually, what is a gun drill and when do we use that?


Steve Zimmerman:


So gun drills and then long aircraft drills. I mean, if your application needs a hole deeper than the 1 to 10, and obviously there's tools out there to do it, but you're getting into custom tools, custom machines at sometimes, if you're doing really, really deep gun drilling, then it's going to be a custom machine as well. So that's going to add another operation and more cost to it.


Greg Paulsen:


Yeah, absolutely. And if you ever have those concerns, you could talk to our team, we'll spin the CAD and give you some sound advice there. So pocket width to depth, this is actually very similar to the internal radii, that one to four depth of width ratio. Think of it the same way, in this case, if we were talking about the radii back corner on the previous slide, this is just expanding that throughout the part. Just know that if you have a protruding feature, so say I have a little boss, or for example on this design here I have this little inlet. So the machinist chose to use the same end mill that can fit inside this inlet and create those features as well as the rest of the part. At a one-off, that may be something that's completely fine to do where they're not changing out tool sets for the sake of convenience, but that can drive the time to create that pocketed feature because you have to reduce the end mill size. So reduce size of end mill means I just have to cut more, do more paths to actually remove that material.


Thread-depth-wise, we recommend less than 2x the diameter for the threading itself. So mechanically, only a few threads are actually doing work at any given time on a threaded hole with a threaded bolt style. You don't need to fully thread a hole. So we do like through-holes in it, but as far as what you're actually threading about 2x of diameter or less is probably the best guideline. But you do add costs and risk to breaking taps for example, on longer threads.


Steve Zimmerman:


The main thing with threaded holes, if it's a through-hole you can thread completely through, that's totally fine. But your fastener is only going to be engaging about one and a half to two times diameter. Always leave a little bit of extra room below the threaded holes as well for the tool itself. The tool's got about a 30% taper on it, so about the first 30% of the diameter, you're not getting full threading engagement anyway with the tool.


Greg Paulsen:


Yeah, absolutely. So these ones are very closely related there. That additional hole depth will need that. And also, if you're using inserts, so a Helicoil, a Keensert, something of that matter, like brass screw to expand inserts, all of those, you want to have clearance below those inserts so that we can install those. Especially like a Helicoil, I need to actually, excuse me, first pre-tap that hole, then install that Helicoil to it. So I can't have a bottoming thread there. When you have a need for a bottoming thread, maybe it's a geometry requirement, we do need to use a tool like a thread mill to produce that which may drive some cost on that feature itself.


So on that note here, this is a thread mill by the way. You can see that little, imagine that tap, only is one little groove and you're using a computer numeric control to tell it what to do and make your custom thread. But yeah, Steve, this is a lightning round if you want special features. So yeah, go through.


Steve Zimmerman:


Yeah, special features, this would be stuff that's not your normal typical lathe or mill machining. So gun drilling really deep holes. Reaming of holes are just tight-toleranced holes, which is another tool involved, but it does drive cost if you don't need a really tight-toleranced hole. Any kind of grinding is going to drive cost. Porting, which in the side view of the tools, that's a porting tool right there. So you're actually putting in a form, it's a form tool, that's putting in a bunch of different angles and diameters at the same time. But it is a custom tool. Slitting saws, they're very slow, so any kind of fin features and stuff like that. Most machinists are going to use a slitting saw to cut those. Key seat cutters, which is a normal tool, but at the same time, it's getting up underneath of certain features. So it is going to slow the actual machining of it down.


Extended reach tooling. So anything that's super extended, like a long reach, it's going to slow the machining process down. EDM, wire or sinker EDM, is where you have to have a square corner in a hole or in a pocket and you can't have a radius. We'd have to go in there with an EDM machine, which is another whole different process. Same way with broaching and gear-hopping, they're separate processes to achieve the right features.


Greg Paulsen:


And this is something, again, the question isn't can you at Xometry? It's more about, what are the requirements of that print? So for example, grinding is fairly common for us on harder metals, but you may actually be able to activate our team to review this based on how you're configuring the quote in the Instant Quoting Engine. Usually a ground feature may have a surface finish that is beyond our dropdown menu, or more likely, it has a tolerance that sub-one thousandth. So on Xometry's website you could select down on your tolerances. And we want you to choose the tightest tolerances on your drawing.


If you have tolerances that are below 1,000th of an inch, so plus or minus 1,000th, what'll move the quote to from auto quote stance, it'll go to a manual quote review. We want you to attach your technical print, and a lot of times we're going to read between the lines. In order to achieve this feature, we're going to need to build the net shape and then grind to finish that feature off. Or sometimes you have a hardening step in there, so it's net shape, harden, post-grind to spec, ship to customer.


So we make a lot of parts here. Sometimes it'll automate for a manual quote review as you're specifying your technical needs on our Instant Quoting Engine. Sometimes for things like gun drilling, like extra long holes, those type of features we like if you reach out to us, raise your hand, we'll take a look at your quote really quickly and get back to you with some feedback if we need to change anything. So that was kind of like a speed round of design tips, like we're making those parts.


Now, how do I make my drawing? Write that love letter to your manufacturer that you can give to any manufacturer and if they're reading that, along with your CAD file, you know that you'll receive what you're looking for. That's the biggest thing we want to do is we want to make sure that you have a transferable communication between suppliers. And the first thing is just making a good technical print, make a great drawing. Typically, we are CAD-derived, so it used to be tolerantly, every feature was a common thing, because there was no CAD and CAM building up to a CNC program, which is the typical steps now. It used to be machinists looking at the print, seeing every single measurement and making those features using manual tooling.


Well, now you really only need to call out what's special about the part because I already have your 3D file. If you have a title block, you could call out very quickly using two decimal place, three decimal place, four decimal place, a tolerance range for those features. If they're important and you want us to measure them. But a title block, which is going to be this portion of your print right there can help cover a lot of those features that you need to put a plus or minus on called out tolerances. We also recommend, and I have another slide on datums calling out where your datums are if you have any, and you're using GD&T and building your dimensions off of those datums and tolerancing off of those datums, because those are essentially translating into how our machinist is going to set up your part and how our QA team is going to measure your part.


And again, we'll go through GD&T, but the most important thing for me is to know that GD&T is applying a tolerance methodology to parts that go into other stuff. When we talk about making great drawings, making great CAD designs, we want to think about how transferable that is without you having to tap the machines on the shoulder and say, "Oh yeah, it fits in this, I'm going to give it to you right now, and can you make sure they fit together really quickly, and if they don't just adjust the machining setup really quickly for me." We want to move away from that and move towards methodology to actually make things fit together using your technical prints.


As I mentioned, we have your CAD, we have your drawing. We are going to measure with our inspection reports based on what you call out. So calling out everything that is important is what we want to see on your drawing. But if it's something where it could be CAD-derived, it's just, is the feature there or not? You don't need to call that out in a print itself. So over-tolerancing can drive costs because you're essentially putting work in that spot for our inspection team to measure.


And also, I see this a lot, where a print is redlined, but the CAD is not redlined. We don't want to do anything that may cause an accidental mistake on the machining side downstream. So they may be setting up their CAM program with that CNC, CAD design, but if a redline says move this hole over X amount on the print, that may not get caught right away and we may make a feature that's wrong from that print. So just always take the extra time, take the extra five minutes, if you have a red line on your design, update the CAD or vice versa, make sure that those two match going along the way. So yeah, Steve, if you want go, I'd love talking to you about how a machinist thinks about GD&T, but let's just talk about datums and GD&T really quickly here.


Steve Zimmerman:


Yeah, so datums, geometric dimensioning and tolerancing, like Greg said, it's a system to make sure things fit together. When a machinist or a programmer first takes a look at your print, at your model, they're going to note the datums, your A, B, C is your main datums. That's what everything's going to be programmed and machined off of and then measured back to. So make sure that you're designing your datums clearly to the design intent of that part, if that makes sense.


Greg Paulsen:


Absolutely. And they should be at least a physical feature on the part or something that is kind of captured. So for example, an axis can be a datum, but what we don't want is parts datums that are in some theoretical space. So say for example, this has a curvature on this face here and the center of that curvature is three feet away from me, that's very hard for a machinist to theoretically set up and make a parts based on those datums. So you do want something like for example on-axis here that means that you get turned down and produce features using a lathe tool to produce features that are on-axis and true to that datum. And this is just really important, like setups matter when you're setting up those parts, and measuring and building features off those datums makes it very easy because not only are you the engineer designing parts with datums in mind, the machinist is setting up those parts with those datums in mind and the inspectors are inspecting with those datums in mind. So it is a universal tool that gets you all the way to a shippable component.


And this is a little cheat sheet, it will be like an eight-hour webinar if we actually went over every single GD&T call out. And there's lots of services that also will do training on this, but just an example of why you may use a GD&T call out here. And if you see me looking around it's because I'm moving my camera view around because I have so many features out here. Angularity, is going to be for things like just checking the angle of a feature like angled brackets. Circularity, think about a bearing, something that's going to be fitting in something else that needs a circular feature.


Concentricity, is going to be two circles and how they relate to each other. So I have this part beside me here, it has four threaded holes on the bottom. It has four slightly larger holes up top in this array here. These holes probably have a shoulder screw that's coming up through this feature and shouldering up here. So I may want to measure these as a concentric or even cylindricity is a third-dimension of that, so cylinder to cylinder, to make sure that those are lined up with each other as I measure. And that way, I know that when I get this part into my building, I can install my shaft or bearings immediately and get a functional part out of it.


Flatness, is a lot of times datum A's may have flatness. So datum A's are typically like a large planar face on a datum. Parallelism, think about making a rail guide, making sure that each side of those rails are straight and parallel to each other. Perpendicularity, the opposite of that. Is this thing off of a right angle? And this is a way to measure and give how much degree of wiggle you can allow before it's a rejected component. Again, this part is just a famous part for me right now. Positional tolerances, so these four holes, may be installing into something else entirely, which really important about that is not necessarily are these four holes exactly where they are this way, it's more that are these four holes important in relation to each other? So I may put a positional tolerance for these four holes saying the relationship of these holes to each other is important to this degree of freedom. And that'll allow me to design a part that installs with the other thing that you're making. So position's probably one of the most common GD&T tolerance we see.


Profile is going to be looking at a surface and say you have a keyway or a contour feature, airfoils, really good example of that, for making them, there's both the line, the 2D version and a surface version. Run out would be the straightness of an item and how that may wobble or change or move along the theoretical straight of those parts. And then we have, I just said straightness there. Straightness is going to be the linear dimension of that. And then symmetry is for mirrored features going through.


But these are the symbols for GD&T, a little bit of why's behind them. And like I said, we could be here all day talking about GD&T, but the most common I typically see; position, profile, flatness, concentricity are usually down their, parallelism as well and perpendicularity.


Now, that being said, there are consequences too, when you're toleranting your parts. We measure parts at Xometry and we measure to your design. We have lots of selectable options for inspection reports. So if you choose a formal inspection with report, you're going to receive an inspection report with your part in the mail as it's delivered to you. We also have options for more advanced inspections including like CMM inspection. CMM is a Coordinate Measuring Machine, which is kind of like think like a CNC machine where it's CAM-driven, it's driven by that CAD file, but in this case, it has a little red touch probe that is pinging points in spaces. So when it touches an object, it'll record that point in space. And it's very good for measuring a lot of these GD&T callouts like true position, cylindricity, profile features. In fact, surface profile features, you absolutely need a CMM in order to measure those features correctly or a vision-based scanning system. But CMM is more popular there.


It does add cost, but just know that if you do have those callouts, you do need to ask for the inspection equipment that can actually measure those. And the most common thing I see here is the stuff highlighted in yellow here. I've seen a big trend in this happening on drawings where, and you have this one little note, all dimensions not shown are to be considered basic. All surfaces are subject to the following tolerance, and it gives a surface profile of your entire part. That means that we need to put this on a CMM, which is a higher overhead, more costly device, in order to ping out features all over that part and figure out, do any of these features deviate from that 0.010 to the datums of a surface profile? So you've added cost, it's not good or bad, but it does add costs by putting that little note in, because you do have to request for CMM for those.


By the way, I saw a Phil Petticoat saying, "You have to cater for us Europeans who are metric." Good news is on Xometry.com, we have imperial and metric conversions. So sorry about talking in thousandths for you all here. And now I have more thousandths. So getting out of GD&T, I have a really quick slide just talking about bilateral, unilateral and limit tolerances. So sometimes it's just distance, like how far away is that feature to this feature or how wide does that feature? For example. Generally, bilateral tolerance is what you see most of the time. Sometimes you don't even either put bilateral if they fit your title block. So you could just call out the feature size, and if it's three decimal places and your title block says 0.005 or 0.012 millimeters, my friend, or 0.12 millimeters you will... We already know that. We already know that we can refer to your title block.


If you have something differing than that, like a plus or minus two thousandths for example, on a tighter tolerance feature, you could a bilateral tolerance. If you have a fit where it's like, hey, this thing can't get bigger than this because it's about the fit inside something else, then that's when you use something like a unilateral tolerance. So say a +0. It can't go beyond this, it can't go before that. And usually the mating part will have the opposite of that. So if it's a +0 on a pin feature, the slot feature will be a -0 with bonus tolerance to that.


And then limit is kind of another way to write bilateral, but it's usually more inspector-focused because it just helps them quickly check, is it between these two? But either way, I always recommend, regardless of these, all these are using 0.625 as a nominal. In your CAD design, you will reduce error by designing to the nominals for your tolerances. Steve, do you have anything to add there? Anything you've seen?


Steve Zimmerman:


Yeah, actually an example where I've run across is when I'm programming parts and there's plus and minus dimensions. That means I got to use different tool paths for each of those. Where I could use one tool to complete a bunch of different features, now I have to break it down into 2, 3, 4 different tools because I'm holding different cutter comps and different programming paths to hold those. So nominal modeling is definitely preferred by the machinists.


Greg Paulsen:


Absolutely. I have a burnt-hand-on-stove story of a minus minus tolerance that gave me a bad day, where minus minus tolerance sometimes happen with fit items. They're kind of like a way of writing a unilateral tolerance, but the challenge with the minus minus is say it's 0.625, -10, -20, the design that you receive is still at 0.625, and if you mill out that feature, you could always make a hole bigger, but you can't cut a hole smaller. And so the part is rejected. And in this case this was one of those where the print was kind of rough, so we thought it was just a little scanning error that it looked like minus minus, but it was not. And that brings me back to one design for the nominal and going back to also that note, ensure your drawing and CAD match, because sometimes minus minus comes in when someone makes their design, they think it's correct, then they actually fit it in realize, "Oops, I got to change this really quickly," for a shaft fit or something like that.


So all they do is redline the drawing and they just scratch out the tolerances, put in a minus minus or something. But technically we're in the wrong if we don't hit your print as it's called out. But unfortunately, prints can be deceptive if you're not matching your CAD to your print, if you're doing a red line like that, it's very easy to make that mistake. So you're introducing risk by design with those types of features, which is why we're giving these tips today. And some more drawing tips, go through these a little bit. And again, I've seen questions come in, so thank you so much. Keep on asking those questions. But make sure to fully define your holes and threads, so not just I want a 10 32 here, but I want this with this. I want a 3/8-16th hole, UNC-2B, which gives me the inspection requirements for it and this is a through-hole. Or, I have a hole that goes down, for example, this hole here goes only down .038 in this design. That information is valuable for us, it allows us to inspect it properly.


If not, we're going to go back to best practices, but that may not be your intent. So fully defining holes is very important. One thing to note is, on a drill relief, you don't actually need to throw a drill relief in design if your Thread Wizard has it, no problem. But I've seen on some larger features, that could actually be either drilled or pocketed. If you have a drill relief in, we actually may surface that feature. We actually may think it's a feature of your part and surface it in. You can always call out, put a little line to that feature on your print and say, "Okay to have drill relief." That's fine there, but you don't always need to design it if it's not necessary to your part.


We talked about thread milling. So if it's a truly unique type thread, we may want to see that modeled. Otherwise, a callout is sufficient. That hole should be the drill diameter for that threaded feature, because we need meat to cut out of, and we do not recommend modeling threads in the design. Also, this is just a... I feel like I'm just giving you all these warnings. But things that we've seen rejecting prints that are unnecessary over super tight tolerances on counterbore features, head clearance features, that all they need to do is have extra relief because of a screw head for example. And they have a extremely tight tolerance and a whole part's rejected because it's out by a thousandth of an inch or something. Just loosely tolerance where it's non-critical, tightly tolerance where it's necessary. And we already mentioned the edge breaks.


A nice callout I like to see on edge breaks, is I personally prefer machine breaks. So in my notes section I'll say machine break all corners and sharp edges. I also like this, which is a, not to exceed. So that gives freedom for the machinists and the inspectors. Just know, as long as it's below 0.030 inches, you're in the safe on it, because this again is to prevent you from cutting yourself. Machining is going to be something that just makes it more consistent across. But that's what we have here. Steve, you got any notes?


Steve Zimmerman:


Main thing with the edge breaks is don't over-tolerance them. And like Greg said, you don't have to model those in. Just break all sharp edges, machine break all sharp corners and edges. The machines and our CAM systems, that's just a click of a button to add those edge breaks in. Where if you're actually holding tight tolerances, now you're creating a feature and that's a whole different tool path.


Greg Paulsen:


Yeah, that's something newer on CAM, which is really nice is, I know I saw some modeling CAM in Fusion 360. It's literally a checkbox now, which is pretty nice.


I'll go quickly over these because I do want to get some of these Q&A here, and please keep on asking questions as we go through. Something important to notice, we serve every industry at Xometry. And we have parts in space, we have parts on the moon. We are able to make a lot of different features, but just know that if you do have a flight or space flight component, talk to us. This is not an auto-quote behavior. We want to make sure that you're using the quoting engine as much as possible, but this is where you get our solutions engineer on board. Same for medical devices, same for automotive production. We are here to help guide through because typically, it's not just, can you make the shape? But the documentation trail combined with that to make sure that it fits that highly critical application.


And that's where you bring our solutions engineering team. Whether it's reaching out to your sales lead or reaching out to Xometry's support to get in contact. You can still use the quoting engine to set up that quote, get it configured as much as you can. But just make sure that we're in the conversation before you press buy, for again, flight, space flight. It's just very important for us. I have a little case example there. We have a great case study where we actually did large format machining for the ECLSS system on the International Space Station, which is a CO2 scrubbing system.


And they had a challenge where NASA did not have the machining capabilities to actually produce this part. They were thinking about welding it together, but they had a lot of challenges in qualifying that weld. They asked us about it and we said, "Well, why don't we just mill it from billet?" And they're like, "You could do that?" And we said, "Yeah, we can." And so now we have parts on the space station that are part of their life support system. And so there's a lot of capabilities that we have that we can offer that are... Even if you have your own shop, we may be able to do a little bit more and help you out.


And scaling up, I just like to show this graph. I've made this graph, I think it's pretty, but really we're talking about CNC machining, mostly metal CNC in today's webinar. At some point as you're scaling up, we have other options to help you move into higher level productions. And I'd just like to show you how this looks in the continuum of manufacturing. CNC brings you really far, really precise items. As you move up, there may be some cost savings moving to progressive items like die casting for example, stamping, metal extrusion. I don't have it on here, but metal injection molding. But just to let you know, there's a world of production outside of machining and we're here to help you there as well.


And as always, we have resources, webinars like today, we have recordings of those webinars. We have great design guides, Xometry.com/resources. And case studies, like the NASA one we have here. And we also have this very cool poster. We'll send a link to you as well, but this is a downloadable poster that you could print out with some great CNC design tips, including what Steve and I talked about today. And with that though, I see questions. So I'm going to jump into here, and we got Megan Conniff from our team here who's a marketing guru and will help us through some of these questions. And my quick promotion on the Instant Coding Engine is, if you haven't tried us out, upload your files, I got this code right here that you could use to get a discount on your next order. And yeah, we'd love to do some work for you all.


Megan Conniff:


Excellent. Well, thank you all for submitting questions. We've got a lot in here. We'll get to as many as we can. Steve, first question is for you from Chris, "What are the most common issues you see with manufacturers ability to interpret design intent from technical drawings? What are the biggest issues with technical drawings for high-precision parts with GD&T?"


Steve Zimmerman:


So, not clearly defined datums would probably be the biggest. That, or datums that are, as like Greg said, that are out in space. Keeping your datums to actual measurable points is going to help the manufacturer out greatly on that.


Megan Conniff:


Excellent. All right, next question, "Does Xometry offer part review for cost optimization?"


Greg Paulsen:


Yeah, so that's where our solutions engineering team comes in, and that's been part of my role as well as the role of our team members. We have experts in every discipline of our manufacturing continuum that we offer here at Xometry. So usually that comes hand-in-hand with DFM. So we do a lot of Design for Manufacturability review, and the cost drivers are usually one-to-one on that. Sometimes we can also take a look your quote and kind of figure out what's driving cost and lead time. A lot of times, if you have something, a specific need to hit X cost or X lead time, we will be looking at first like, what can I trim on my secondary finishing? So if I have a powder code step or I have something like that, that may add four or five days to my part that we could trim off if it's applicable. But yeah, that's what we're here to do. We're here to help. Beyond the quoting engine, we have humans in the loop that are ready and available to support your needs.


Megan Conniff:


Next question from Josh, "I think you said you prefer drawings without plus plus or minus minus tolerances, meaning you'd rather have the nominal as the low high end and the single direction tolerance?"


Greg Paulsen:


Yeah, so I like to limit tolerances for those, personally. And for your design though, you want to build to the nominal of that whatever's in the middle of those features. The reason why we talk about design toward a nominal is, say I have a 0.625 +0. And it's modeled at 0.625, and I'm machining to 0.625. A slight variation on one direction can scrap your entire part. Now, if you have that 0.625 +0, -10, and you modeled 0.620, so you modeled to that middle of those, a slight variation won't scrap your part. And that's a difference for you. It could be a difference of business days of lost time. We're still going to make your part right, but by modeling to that nominal and making clear and readable drawings, it's going to be much more useful for both you and those who are interpreting that to make your part successfully on the first run around.


Megan Conniff:


Next question is from Chris, "Should I be applying datums based on my part's function or how my part will be inspected?"


Steve Zimmerman:


Function.


Greg Paulsen:


Yes.


Steve Zimmerman:


Definitely function.


Greg Paulsen:


A good way to think about it is, where's your part installed? So say I have a part that's sitting on a table and nooked-up against another thing in its assembly. Table's, datum A, B and C is the thing that it's nooked-up against. Now, say on that table, it has a whole feature that I'm screwing this into. Well, then I have a true position tolerance for that whole position because I know this is relative to the table and then nooked-up position, that hole should be here. And so that's how you have to think about, it's like you start with function first. And it leads into better QA. So it's a foundation, foundation, foundation for making good prints and good design intent towards your manufacturing needs.


Megan Conniff:


Next question is from Ed, "Is it practical to do a prototype in a cheaper material i.e plastic to verify fit? Then do the final in the actual, say, aluminum material?"


Greg Paulsen:


So funny enough, aluminum's cheaper than plastics most of the time because it's easier to work hold and it doesn't dance around when you're cutting. So you may find 6061 T6 is kind of the bread and butter material for running. We do have other ways to prototype though, including additive manufacturing 3D printing. So you may, for example, use a process like FDM 3D printing to build a part that you're eventually going to be machining. I like using FDM because the design rules are basically one-to-one with CNC machining, like the wall thickness guides and everything else for that. Or selective laser sintering as well. But those may give you a low cost way to quickly iterate and change your designs with still a functional part where you can get tested through, and then you can invest in the machining once you're ready.


Megan Conniff:


Excellent. Next question, "Do you have a template for technical drawings?"


Greg Paulsen:


We actually do. So if you search the Xometry site, how to make great technical drawings. We have a really nice guide that goes step-by-step on things that we like to see, including a lot of what we cover today. And linked to that, if you are running SOLIDWORKS, we have a SOLID draw template that you could download and use that as your own and take out the Xometry logo and whatnot. But you could use that as your own if you don't have one standard already.


Megan Conniff:


Next question is from Eric, "Any tips about external part size versus stock size? For example, what is the maximum thickness you could cut out of 0.25 inch stock looking to minimize material waste, machine time and cost?"


Steve Zimmerman:


Yeah, so it depends on your surface finishes. So your materials are going to have a certain tolerance from the mill or the manufacturer of the material. So if you need something that's got to be close to 0.250 thickness, then you would almost have to order something at a 0.375 and machine it down. So it depends on surface finish at the end of it.


Megan Conniff:


Next question is from Fabian, "What are the different capabilities between CNC machining and metal 3D printing?"


Greg Paulsen:


Oh, yeah. So we're talking about additive versus subtractive manufacturing at that point. So it's about access to features, is the first thing you got to have to think of. When I think about subtractive, I've removing from stock. So I'm thinking about line of sight features. I can see every feature that's created on this part, and those would be my operations or setups to create those. So my design mindset is about subtracting and accessing those features. So say for example, this had a small internal channel that made a curve around, I couldn't hit a straight tool and create that curve. Where additive manufacturing is growing parts, I'm building from a base and growing up, so think like your coral reef. Coral doesn't just grow in the middle of the ocean, it's attached to rock or substrate and then building out based on its function. So when I'm moving to additive manufacturing, I have much more freedom to build organic shapes and create those features. So they're both ways to make metal parts, but my design is very different.


The other thing I know is that when I'm removing material in CNC machining, I'm working. Any hogging out material, that's work, that's overhead, that's cost to part. In additive manufacturing, if I leave material on a part and make a brick, I have to make all that brick, and so that's work. So a lot of times when you'll get an additive component, it's going to be on a diet, it's going to have lots of coring features, internal channels, things that are going to reduce that weight and bring it straight to its function. So your design intent kind of flips around versus what you do with machine parts.


Megan Conniff:


Next question is from Kyle, "Does putting GD&T on a print automatically drive up cost because it may now require use of the CMM?"


Greg Paulsen:


So my short answer is no, it won't. But if you putting GD&T with things that have a three-dimensional reference to them, so concentricity for example, where I need to actually create a data set of essentially... Imagine creating ping points around this feature here, and then I have to compute from that what the central cylinder axis is, that type of stuff. Surface profile, where you need to create a computational model of the theoretical center of something or the theoretical surface, that's going to require the CMM requirements. A lot of things can be done manually. I put on that slide deck around positional tolerances. I put true position as a CMM need. It's not necessary. They've been doing true to positions since before CMMs, but the more holes you have in an array, the more I'd recommend it. Steve, I'm not sure if you have any tidbits on that as well.


Steve Zimmerman:


Yeah, mainly profiles, cylindricity, that kind of thing where you're getting into a three-dimensional type of measuring, that's what's going to drive a CMM inspection.


Megan Conniff:


Great. And now we are at two o'clock, so we're coming up on time, but we have another question here.


Greg Paulsen:


Sure.


Megan Conniff:


"Do the same manufacturers make my parts when reordered?"


Greg Paulsen:


Yes. So Xometry, we are a manufacturing marketplace. We have a variety of manufacturers and we use AI machine learning to match-make, to pair the scope of your work with those suppliers who could do that work best. Your parts are tracked with an internal Xometry part ID. And we have revision control. So if you're making the same part but you have a revision, you could revise that part on our site in multiple locations. But when you do the reorder, it becomes an exclusive offer, or there's a prior supplier. It's a win-win. You get the parts that you expect, a supplier gets parts that they're familiar with. So it's a really good feedback loop that we encourage, and we have embedded in our infrastructure.


All right. Well Megan, thanks so much for being our Q&A MC. And Steve, thank you so much for being a co-host. I mean, this is just-


Steve Zimmerman:


Yeah, no problem.


Greg Paulsen:


I love these things. I learn as we're also preparing for these events. So as always, it's always appreciated, and just know these are team members here at Xometry. We have a lot of expertise in-house no matter what process we offer, and we have a lot of convenience with our instant quoting engine. But if you do need to have that extra step, DFM review, talk to your rep, talk to our team members. We're always happy to help. Thanks so much for attending. Have a great day, you all.


Xometry Marketing:


Building modern machines, devices, cars, or even bicycles, requires a complex manufacturing supply chain. You constantly need to vet and maintain your manufacturing network, which takes time, money, and effort. Ultimately, this distracts companies from doing what they do best, building and manufacturing products. This is where Xometry comes in. We have built a leading manufacturing network with over 10,000 vetted manufacturers globally. Xometry connects people with projects to the manufacturers who can bring them to life. With our AI-powered Instant Quoting Engine, you can get quotes for your prototypes and production parts in just a few clicks.


You can choose from over a dozen manufacturing processes and hundreds of material and finishing options. All you need to do is get your quote, place your order, and Xometry takes care of the rest. From selecting the most experienced certified manufacturer to performing quality inspections according to your required industrial standards. And having the parts delivered to you reliably and on time. Xometry makes it easy for engineers and purchasers at both small and large companies to tap into global manufacturing capacity and create resilient supply chains. From prototypes to high-volume production, build your products with Xometry.


**Start Your Instant Quote**


STEP · STP · SLDPRT · STL · DXF · IPT · X_T · X_B · 3DXML · CATPART · PRT · SAT · 3MF · JT


[Upload Your Design](https://www.xometry.com/quoting/home/) All uploads are secure and confidential
