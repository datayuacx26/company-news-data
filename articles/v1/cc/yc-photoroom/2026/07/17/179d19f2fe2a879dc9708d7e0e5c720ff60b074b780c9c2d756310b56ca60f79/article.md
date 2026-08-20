---
schema_version: "1.0.0"
document_id: "179d19f2fe2a879dc9708d7e0e5c720ff60b074b780c9c2d756310b56ca60f79"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-16e4c55287d6"
canonical_url: "https://www.photoroom.com/customer-stories/openwardrobe"
published_at: null
first_seen_at: "2026-07-23T22:00:21.424777+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:71290373af1efe08078d99783d9acabc8bea1ab1c28c2bc454a57bedc9516137"
---

# How OpenWardrobe replaced its in-house solution for better quality, performance, and 2x savings with Photoroom

**Life before Photoroom**


-


Built in-house background removal with open source models


-


High training overhead to specialize for fashion items


-


Expensive compute costs without sufficient volume


-


Quality issues and slow performance led to user complaints


-


Received support tickets about poor background removal quality


**Life with Photoroom**


-


Costs reduced by 2x compared to in-house solution


-


Improved processing time with no speed complaints


-


Smoother user upload experience across platform


-


Zero infrastructure overhead (no GPU management or model tuning)


-


Support complaints about background removal dropped from regular to rare


-


Cleaner images improve AI styling recommendations


> **"Photoroom delivered the combination of quality, performance, and cost efficiency that we needed. We avoided the engineering overhead of building in-house and can now seamlessly process 100,000+ images monthly with zero maintenance burden on our team.”**
>
>
> **— Julius Dietmar, CTO at OpenWardrobe**


[OpenWardrobe](https://www.openwardrobe.co/) is a wardrobe intelligence platform built on a simple idea: help people get value out of their wardrobe and minimize waste in fashion.


Users digitize their entire closet through a mobile or web app, capturing everything from sweaters and jeans to shoes and accessories. Once cataloged, OpenWardrobe's AI-powered tools learn their style preferences and wearing patterns, surfacing forgotten pieces and suggesting fresh outfit combinations that work with what's already hanging in their wardrobe.


"We're promoting sustainability rather than unnecessary consumption," explains Julius Dietmar, CTO at OpenWardrobe. "The goal is to help people get real value out of their wardrobe and help brands understand what people actually wear so they can produce more intentionally."


Today, OpenWardrobe processes roughly 100,000 images monthly (over 1 million annually). The challenge? Finding the right partner that could improve the image upload experience of customers and scale with them during peak periods.


## **When messy backgrounds affect the visual experience**


A floral bedspread. A patterned carpet. A bathroom mirror reflecting half the room. These are the backdrops of OpenWardrobe's image library—because real people don't have photo studios in their homes.


"When you use the application, you will very quickly understand that our users don't have access to professional photography," says Dietmar. "They're taking images from their mobile phone. They throw the garment on the floor or on the bed and take a picture. If you have a bed cover that is in all kinds of colors or a carpet, that has an effect on the visual fidelity."


This matters because **OpenWardrobe's AI stylist feature needs clean, accurate images to work properly.** Visual quality directly impacts how well the system can recognize garments and create styling recommendations. When a blue sweater photographed against a floral bedspread gets paired with black jeans captured on a gray carpet, the competing backgrounds create visual noise that distracts from the outfit itself.


The AI needs to understand what each garment actually looks like, such as its true color, shape, and texture, to make meaningful styling recommendations. Background clutter undermines that entire process.


For OpenWardrobe to deliver on its promise of intelligent styling, **they needed a background removal solution that could handle the full spectrum of messy, real-world user uploads at scale.**


*Examples of difficult user images with messy backgrounds and elements*


### **First, the team built an in-house solution**


Like many technical teams, OpenWardrobe initially built its own solution. With AI already central to their product strategy, background removal seemed like a logical extension of their capabilities.


But the reality proved more complex than expected. Segmentation is one thing; high-quality background removal specifically for fashion items is another entirely.


> **"Building presented us with cost, quality and performance challenges. General segmentation models exist in the open source community, but specializing them for fashion items takes significant effort. Fine-tuning training data for our use case was time-consuming. And to deliver acceptable performance for our users, we needed substantial compute power."**
>
>
> **— Julius Dietmar, CTO at OpenWardrobe**


**The team faced three interconnected challenges:**


1.


**Training overhead was significant.** Getting quality training data and fine-tuning segmentation capabilities for fashion-specific use cases required substantial ongoing work.


2.


**Compute costs escalated quickly.** Processing images at acceptable speeds demanded high infrastructure costs without the volume to justify it.


3.


**Quality suffered without specialization.** Without the focus and volume to continuously improve the model, visual fidelity lagged behind user expectations.


**Julius realized they needed a vendor who specialized exclusively in this problem,** one with enough volume to justify the technology infrastructure and deliver fast, high-quality results at reasonable pricing.


### **Secondly, they tried commercial solutions**


Through trial and error across multiple vendors, including RemoveBG and Microsoft Azure Cognitive Services, the team developed a quality benchmark using their most challenging real-world uploads:


-


**Screenshots with multiple elements:** mobile phone frames, battery indicators, navigation buttons, and price tags competing for foreground attention


-


**Low-contrast scenarios:** clothing on patterned carpets or bedding where foreground and background barely differ


-


**Partial objects:** hangers, mannequins, or other items that might be garment or background depending on context


**Both solutions handled simple images well, but struggled across these benchmark areas.**


"We came across cases regularly where providers would just leave everything in the picture and replace the background color with transparency," Julius explains. "They couldn't identify which element should be in the foreground. Or they'd strip detail from delicate fabrics and elements like straps, lace, and soft textures would disappear."


This limitation moved Julius and his team towards exploring a more accurate solution.


## **Finally, they found balance with a reliable AI photography partner**


Through web search and more vendor evaluation, Julius discovered Photoroom. The initial appeal was straightforward: competitive pricing, strong quality in benchmark tests, and a sandbox API that let the team validate end-to-end performance before committing.


After Microsoft Azure discontinued its preview service, giving Julius and his team only three months to find a replacement, long-term reliability became a critical evaluation factor alongside quality and cost.


Photoroom consistently outperformed alternatives when benchmarked against OpenWardrobe's quality criteria:


Criteria RemoveBG Microsoft Azure Photoroom


Fine edge & fabric detail ✓ ✓ ✓✓


Screenshots with UI chrome ✓ Poor ✓✓


Consistent white/neutral backgrounds ✓ ✓ ✓✓


No distortions on clothing shape ✓ ✓ ✓✓


Shadow handling N/A N/A ✓✓


Time to integrate ✓ ✓ ✓


System reliability/API ✓ ✓ ✓✓


Maintainability ✓ ✓ ✓✓


**✓ = good | ✓✓ = high quality**


> **"We've had great experiences with Photoroom, especially with difficult backgrounds. The background removal tool has a very high rate of success when it comes to identifying the garment in the image and removing everything else."**
>
>
> **— Julius Dietmar, CTO at OpenWardrobe**


What separated Photoroom from competitors wasn't only the technical capability, but consistent performance across the full spectrum of real-world uploads—the kind of reliability you'd expect from a state-of-the-art background removal technology.


### **Integrating the Photoroom API into OpenWardrobe’s platform**


Integration with the Photoroom[Remove Background API](https://www.photoroom.com/api/remove-background) was straightforward.


-


**Testing:** The team validated quality with a development API key, benchmarking against difficult edge cases before switching to production with a simple API key change.


-


**Implementation timeline:** They completed the initial setup in roughly one week, while the full process of testing, validating, and rolling out to users took approximately one month.


-


**Solution focus:** OpenWardrobe wraps Photoroom's REST API into their microservices architecture, making it available to both mobile and web applications.


-


**User workflow:** When users photograph items to add to their wardrobe, background removal happens automatically by default.


Beyond quality results, one thing that has impressed Julius most is Photoroom’s reliability.


> **"We've been using Photoroom for a while now and haven’t had an outage the whole time. This is one less thing for me to worry about, which is very nice."**
>
>
> **— Julius Dietmar, CTO at OpenWardrobe**


*User workflow in OpenWardrobe iOS app showing Photoroom’s Remove Background tool at work*


## **Improved user experience, fewer complaints, sustainable costs**


Since switching to Photoroom, OpenWardrobe has seen measurable improvements across the metrics that matter most to their operation:


-


**Reduced costs by 2x** compared to building and maintaining the solution in-house, making background removal sustainable as a free feature.


-


**Cut processing time** for users digitizing wardrobes with few or hundreds of items, with near zero complaints about slow performance.


-


**Improved image quality** across all user uploads, with cleaner edges, accurate colors, and better garment recognition for AI styling.


-


**Maintained zero infrastructure overhead,** with no GPU management, model tuning, or scaling considerations required.


-


**Reduced support complaints about background removal** from regular feedback to rare occurrences.


> **"Our users are very sensitive toward their visuals and the overall user experience. Both have improved since integrating Photoroom, alongside the cost, which is now roughly half of what it would cost us implementing and operating a background removal solution ourselves."**
>
>
> **— Julius Dietmar, CTO at OpenWardrobe**


## **Looking ahead**


With background removal working reliably, Julius is exploring Photoroom's[advanced fashion features](https://www.photoroom.com/ai-product-photography/fashion) , particularly 3D visualization and[product beautifier](https://www.photoroom.com/tools/product-beautifier) capabilities that could further enhance visual fidelity.


"We're following your publications closely," he shares. "The beautification feature for shoes and accessories that gives items a three-dimensional view is super interesting. Being able to rotate around items and see them in three-dimensional space could significantly improve user experience. Those are things we're looking into adopting once they become generally available."


For other platforms managing large-scale image workflows and facing similar bottlenecks, Julius shares a thoughtful advice:


> **"Don't worry about reinventing the wheel. Have a look at what's out there and use it. It's likely to be better than what you can do yourself, cheaper than if you do it yourself, and it frees engineering resources to do what's important for your business."**
>
>
> **— Julius Dietmar, CTO at OpenWardrobe**
