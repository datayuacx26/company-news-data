---
schema_version: "1.0.0"
document_id: "41dc513e2b88a7df62debc41dce1fccf76aa1aafe3a6c19628b0bcf87c81b529"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/accessibility-in-design-systems-a-comprehensive-approach-through-documentation-and-assets"
published_at: "2024-03-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T22:26:07.231499+00:00"
content_hash: "sha256:32ca470310826f2d6b378c486cf42bfc6b66c16d35744a072a0088dfeb877116"
---

# Accessibility in Design Systems: A Comprehensive Approach Through Documentation and Assets

‍[Cintia Romero](https://www.linkedin.com/in/cintiaaaromero/) *is a Sr. Product Designer at*[Gestalt](https://gestalt.pinterest.systems/home) *, which is Pinterest's design system. In addition to creating components and writing documentation, she dedicates her time to community engagement. She regularly interacts with design system users inside and outside Pinterest, develops educational training, and advocates for accessibility and inclusive design. You can learn more about her journey by checking out*[this article](https://www.pinterestcareers.com/blog/2023/05/people-behind-the-product-cintia-romero/) *or watching*


*at axe-con-2024.*


Design systems play a crucial role in ensuring a consistent and cohesive user experience across all products and platforms. They are not just a collection of reusable components and pattern guidance that can be shared among teams, but they also enable designers, engineers, and cross-functional teams to incorporate inclusive best practices, including accessibility, into their work.


In this article, I’ll detail how Gestalt provides accessibility support to our users through design system documentation and assets. I hope it inspires you! If you want to learn about creating accessible components, I highly recommend reading[Geri Reid's article](https://www.supernova.io/blog/how-to-create-accessible-design-system-components) .


Have you ever considered the impact of your design system documentation and design assets on creating an accessible product?


It's not just about making your project more inclusive; it's also about creating a community that values accessibility and inclusivity. You probably heard the sentence, "When you use accessible components, you get accessibility for free." But taking it a step further, by providing accessibility support in your design system documentation and assets, you are contributing to a more inclusive product by inspiring others to follow in your footsteps and educating them on how to do it.


I want to share how we embedded accessibility in our Gestalt documentation and design assets to support designers and cross-functional partners in creating accessible and inclusive experiences. We know we are making a positive impact when we receive feedback like the ones below collected in our design system last survey.


> "You are all rockstars and it's always a great time getting all of your guidance! Thank you for all you do in making sure our product is accessible to everyone."


> "I appreciate the team's responsiveness, thoughtfulness in every response, and push for accessible design - y'all are the best of us."


— by Gestalt users


I hope our approach can spark some ideas for you!


## Accessibility in our Gestalt's documentation


### Accessibility as part of foundations


We define Gestalt Foundations as a guide to create inclusive interfaces and interaction patterns for all Pinterest products. At Gestalt, we believe that accessibility should be considered right from the design phase, which is the foundation level.


According to a Deque[case study](https://accessibility.deque.com/qa-webinar-matthew-luken-accessibility-us-bank) , 67% of accessibility issues arise due to design flaws. Many people believe that accessibility is the sole responsibility of developers, but it's crucial for designers to ensure accessibility in their products. Developers cannot fix design flaws when the primary focus is on design. To support accessibility in all phases, we have created an accessibility page to educate designers on the importance of building accessible products. This page will also serve as a comprehensive list of all our internal and external resources.[Check it out](https://gestalt.pinterest.systems/foundations/accessibility) !


**Image legend:** *Gestalt*[foundations](https://gestalt.pinterest.systems/foundations/overview) *page*


### Accessibility guidance for all components


We understand that accessibility is not something that everyone is familiar with, and that's okay. As a team, we recognize that it can be challenging to know where to start when it comes to accessbility. We believe that it's our job to support and educate one another on how to approach accessibility with empathy and understanding. By doing so, we can create an environment that is inclusive and accessible to all.


When we create a component documentation, one of our tasks is to include accessibility guidance.


That is why one of our tasks when creating component documentation is to include accessibility guidance and ensure it is simple to digest for our users.


We also consider[international design](https://gestalt.pinterest.systems/foundations/international_design/about_international_design) and[localization](https://gestalt.pinterest.systems/foundations/international_design/about_international_design#Localization-(l10n)) to ensure content is appropriate for specific markets, including language, culture, and other requirements. It's never too early to start talking about what we need to do to globalize a product; the sooner we include localization and internationalization in a product/feature conception, the more successful we'll be at overcoming language barriers and reaching people.


**Image legend:** *Gestalt*[HelpButton](https://gestalt.pinterest.systems/web/helpbutton#Accessibility) *accessibility and localization*


### Component accessibility scorecard


If you visit each Gestalt component page, you'll notice a status bar at the top of each component that includes accessibility as one of its items. By clicking on it, you'll be able to open a scorecard that evaluates the accessibility status of our components. I like to refer to it as the A11y readiness indicator. The scorecard serves to not only reflect the accessibility level of our components, but also inform our users about our criteria for ensuring that a component is accessibility-ready, meaning that it conforms with the[WCAG 2.2](https://www.w3.org/TR/WCAG22/) guidelines. This scorecard was designed to provide a clear understanding of the accessibility status of our components and our commitment to accessibility.


We consider a component as "accessibility-ready" when it passes the following tests:


1. **Visually accessible** : The component has been checked for suitable color contrast, readability, and appropriate color use.
2. **Screen reader compatibility** : The component has appropriate headings, labels, and alternative text, which can be described verbally by a screen reader.
3. **Navigable and operable** : The component includes focus states and a well-structured design that can be navigated using a keyboard or other input modalities.
4. **Understandable** : The component contains content that is understandable by most users, operates in predictable ways, and provides intuitive error handling.


**Image legend:** *Gestalt*[Button](https://gestalt.pinterest.systems/web/button#Accessibility) *component accessibility scorecard*


### Inclusive language


Imagine creating experiences that are not only functional but also inclusive and culturally sensitive. That's the kind of mindset we want to reinforce: A delightful, functional, inclusive design.


At Pinterest, we believe every user deserves to feel seen and heard, regardless of background or life experience. And as a design system team, we want to reinforce it in our documentation. That's why we've partnered with our talented Content Design Team to create an[inclusive language page](https://gestalt.pinterest.systems/foundations/content_standards/inclusive_language) , guiding designers to communicate effectively with a diverse audience. With this intentional and thoughtful approach, you can create experiences that truly resonate with people from all walks of life.


Refinements to come in the future!


**Image legend:** *Gestalt content standards subpages*


## Accessibility as part of Gestalt's design assets


### Accessibility design checklist


We created an accessibility design checklist in Figma to help designers ensure that their work adheres to the Web Content Accessibility Guidelines (WCAG) 2.2, aiming to achieve the AA conformance level. This checklist, which is in the form of checkboxes, is valuable for any designer committed to creating accessible design work. It is super simple to use, and it is part of our Design Handoff Kit library, a collection of assets to support design handoff.


The checklist serves as a reminder for our[accessibility design considerations](https://gestalt.pinterest.systems/foundations/accessibility#Design-considerations) .


**Image legend:** *A11y design checklist based on our*[design considerations](https://gestalt.pinterest.systems/foundations/accessibility#Design-considerations) *content*


We plan to use this checklist in our design internal plugin as some kind of linter, such as a tool that helps to identify potential issues and errors in design so that designers can catch potential accessibility issues early in the design process and make the necessary changes before the design is finalized. I suggest you do this as well if you have the resources to do so! Please let me know if you do.


### Accessibility annotations


We added accessibility annotation assets in our design handoff kit library as they can guide engineers in considering elements like semantic HTML, alternative text, design structure, and much more. Simple annotation assets support designers in ensuring the UI they are about to ship meets global accessibility standards and addresses common failures that are reasonably easy to avoid. Annotations help the engineering team recognize and implement significant accessibility standards in the earliest stages, as well as encouraging good coding practices.


Unfortunately, our Handoff library and accessibility assets aren't public (yet). But I advise you to check out the Axe toolset's[Axe for Designers](https://www.figma.com/community/plugin/1085612091163821851/Axe-for-Designers-(FREE)) . It includes accessibility annotations is free to use. Figma also has many accessibility resources!


**Image legend:** *Gestalt handoff kit, accessibility annotations*


### Accessibility contrast checker built in our internal plugin


We added a feature in our Figma[Pinterest Design plugin](https://gestalt.pinterest.systems/get_started/designers#Private-Figma-plugins) that enables designers to check for accessible color contrast. The credit for this plugin idea goes to the Pinterest Brand team, who developed it as part of the Brand's internal tools. We have incorporated their first iteration into our plugin, which currently addresses color contrast only for AA and AAA levels of compliance to support our designers. However, as I mentioned above, we want to create some kind of linter for accessibility as we evolve our tools.


**Image legend:** *Pinterest Design Plugin cover and color contrast feature*


We built this plugin to support designers to work faster and smarter. Designers can add Pinterest images directly to designs, export icons in production-ready formats, automatically convert UI to dark mode, reference design system documentation, and check for accessible color contrast.


Our Figma plugin is only internal for now, so we recommend using a tool like[aremycolorsaccessible.com](https://www.aremycolorsaccessible.com/) to check the foreground color against the background color. In Figma, you can use[the Able plugin](https://www.figma.com/community/plugin/734693888346260052/Able-%E2%80%93-Friction-free-accessibility) to check color contrast in your designs. Maybe in the future we will be able to share it with the community.


### Accessibility training


When I joined Pinterest, one of my first projects was to create accessibility training for designers. I was fortunate that I had people excited about that as well!


At Gestalt, we believe training is a powerful tool for promoting component adoption and accessible design. So, besides integrating accessibility into our design components, we provide accessibility training for our designers, covering topics such as inclusive design principles, creating accessible design, and using assistive technology. We aim to have all designers take this training and develop an inclusive mindset that prioritizes accessibility.


It goes beyond design. Pinterest also offers an internal accessibility bootcamp for engineers, where they can learn best practices for developing accessible code. Gestalt engineers are instructors for this training.


**Image legend:** *Gestalt training certificate of completion, Accessibility for designers*


### Conclusion


Design system documentation and assets can be powerful tools in promoting accessibility and inclusivity in products. By embedding accessibility information in design system documentation and providing assets as supportive resources, designers can educate themselves and their teams on the significance of creating accessible products.


We are aware that there is a lot of work to be done, not just in terms of the design system, but also across the entire company and product line. Accessibility is an ongoing process, and as guidelines continue to develop, we must keep ourselves informed. It is important to note that a design system is only a small part of the equation; everyone within the company is responsible for ensuring accessibility.


I hope this list helps you. Thanks for reading this!
