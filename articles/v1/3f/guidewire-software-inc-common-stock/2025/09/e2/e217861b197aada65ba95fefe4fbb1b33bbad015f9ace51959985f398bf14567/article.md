---
schema_version: "1.0.0"
document_id: "e217861b197aada65ba95fefe4fbb1b33bbad015f9ace51959985f398bf14567"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/technology/the-accessibility-imperative-part-1-building-a-foundation-for-inclusive-design-systems"
published_at: "2025-09-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:a325f6c235ff02ce1b0f0b9f2e866355a18837a831c00bff75cdeb64535afca6"
---

# The Accessibility Imperative, Part 1: Building a Foundation for Inclusive Design Systems

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Technology](https://www.guidewire.com/resources/blog/technology)


- The Accessibility Imperative, Part 1: Building a Foundation for Inclusive Design Systems


Accessibility is often discussed in the context of user-facing interfaces, but it must be woven into every aspect of digital product creation – from internal tools and workflows to team collaboration platforms and design systems. This is especially important considering that[nearly 3% of the world’s 27 million software professionals, about 810,000 people, report a disability that can impact how they work with digital environments](https://idego-group.com/blog/2021/12/10/52-statistics-about-software-developers/) .


Whether it’s a low-vision designer navigating a complex interface, a QA tester relying on keyboard-only access, or a content editor using assistive technology, accessible systems empower diverse teams to contribute fully. Standards like the Americans with Disabilities Act (ADA) and Web Content Accessibility Guidelines (WCAG) don’t only apply to public websites — they extend to internal applications and tools that support day-to-day operations. By prioritizing accessibility throughout the digital ecosystem, teams foster more inclusive workplaces and create better, more equitable products for everyone.


In the first part of our “Accessibility Imperative” series, we will explore the importance of design systems in shaping inclusive and accessible products. We will also take you through some of the first key steps to consider, whether you are new to design systems or keen to make an existing one more inclusive.


## What is a design system?


A design system is more than just a set of standardised design elements, patterns, and rules; it’s a foundational toolkit that helps teams build consistent, high-quality user interfaces efficiently. Typically, it includes UI components such as buttons, icons, and form controls, along with style guides for colors, typography, and spacing. Good documentation also outlines when and how to use these components effectively.


Although components and guidelines are essential, the most critical aspect of any design system is the framework, which includes the governance, processes, and cultural foundations that hold everything together. Even the most accessible, well-crafted toolkit won’t succeed without structures in place to support adoption, maintenance, and decision-making. In reality, many design systems fail not because of poor assets, but because they lack the operational backbone to make the system sustainable and scalable.


## Why do design systems matter?


A well-executed design system will help:


- Ensure consistency across pages, applications, and platforms.


- This leads to a more predictable and cohesive user experience, reducing confusion and reinforcing trust in your product.


- Speed up design and development.


- Teams can reuse components and patterns, reducing duplicated effort and accelerating speed-to-market.


- Make collaboration between designers and developers more seamless.


- Shared standards and documentation reduce misunderstandings and help teams work more efficiently toward the same goals.


- Maintain brand identity as your product grows.


- Consistent use of visual elements and interaction patterns strengthens your brand and helps ensure quality at scale. Just as importantly, a well-structured design system enables efficient theming for multi-brand products, allowing teams to launch new branded experiences quickly.


## Why is accessibility in a design system so important?


### Inclusivity is the right thing to do


We want to ensure as many people as possible can use products built using our design systems. Designing with accessibility in mind allows people with disabilities (e.g., visual, auditory, motor, cognitive) to access and use your product effectively.


### Consistency across platforms


A design system with built-in accessibility helps ensure that every component, whether it is a button, link, alert, or form, meets accessibility standards by default, making it hard for teams to “forget” accessibility. However, it’s worth noting that accessible components alone won’t guarantee an accessible experience. Designers and developers still need a basic understanding of things like content hierarchy and logical sequencing to apply those components effectively in context.


### Efficiency for teams


Accessible components save time because developers and designers can reuse proven patterns instead of reinventing the wheel each time (and possibly making mistakes).


### Legal and compliance benefits


Many countries have laws (like the **ADA** or the **European Accessibility Act** ) that require digital accessibility. A design system can help your organization stay compliant. There are many examples of companies and organizations who have been sued for having inaccessible products and services, including Walt Disney, Netflix, and Domino’s. Settlements are usually made without disclosing the costs involved but in a 2008 case brought forward by The National Federation of the Blind, Target were sued for $6 million, with legal fees of $3.7 million. Target was sued because its website, Target.com, was inaccessible for visually impaired people, violating the **ADA** and California state laws. Of course, these figures do not include the potential brand damage!


### Better user experience for everyone


Accessible design often improves usability for all. For example, something as simple as higher contrast makes text readable in bright light. Adequate spacing between touch targets such as buttons can improve usability for all people - not just those who lack fine motor control for example.


### Detailing accessibility specifications for new components


When adding a new component to your design system, it is important to consider the accessibility specifications. If we take a component such as an[accordion](https://www.w3.org/WAI/ARIA/apg/patterns/accordion/examples/accordion/) , it will need to support people reliant on a keyboard for navigation, so the expected interactions should be documented. Additionally, an accordion may use structural markup including headings, and since accordions can be expanded and collapsed, we also need to consider how they are supported using assistive technologies. We must therefore also capture any roles, properties, and states, including any aria attributes.


*W3C Web Accessibility Initiative accordion specifications for keyboard*


## What resources can I use and where can I look for inspiration?


A great place to start for accessible pattern examples is with the[Web Accessibility Initiative](https://www.w3.org/WAI/ARIA/apg/patterns/) website. They have a number of example patterns, with detailed interaction specifications and HTML code.


It is also highly recommended that you explore how components and patterns are put together in other reputable design systems. They might not always contain something that matches your exact needs, but they are often a great source of inspiration. Here are some examples:


- **[Adobe Spectrum](https://spectrum.adobe.com/)** by Adobe
- **[Carbon Design System](https://carbondesignsystem.com/)** by IBM
- **[GOV.UK Design System](https://design-system.service.gov.uk/)** by the United Kingdom Government Digital Service


### Conclusion


Creating accessible environments is not just a compliance checkbox - it’s a business and ethical imperative. In this first part of our series, we’ve delved into the critical role that design systems play in creating inclusive and accessible digital products. By embedding accessibility into every facet of the design system, from internal tools to user-facing interfaces, we ensure that our digital environments are usable by everyone, including the 810,000 software professionals with disabilities. This approach not only aligns with legal standards like the ADA and WCAG, but also fosters a more inclusive workplace and enhances the overall user experience.


In Part 2, we’ll dive into standards and tools for testing and overcoming challenges that may creep in during the process. Stay tuned for more insights and actionable advice on building a truly inclusive digital ecosystem.


Thank you for joining us on this journey towards better, more equitable design.


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
