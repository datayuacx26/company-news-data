---
schema_version: "1.0.0"
document_id: "482d3d8b17b1f02d1c858b06329955365677c513af941549ae08de734bd79bef"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/technology/the-accessibility-imperative-part-2-a-practical-approach-to-inclusive-design-systems"
published_at: "2025-11-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:25:12.063826+00:00"
content_hash: "sha256:ceb3af4cc39b016b2258eb57a884636bc5a0939252449c3d99e85e015208710f"
---

# The Accessibility Imperative (part 2): A Practical Approach to Inclusive Design Systems

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


- The Accessibility Imperative (part 2): A Practical Approach to Inclusive Design Systems


In[part 1](https://www.guidewire.com/resources/blog/technology/the-accessibility-imperative-part-1-building-a-foundation-for-inclusive-design-systems) of the series, we covered the benefits of a well-executed design system and why accessibility in a design system is so important. We also looked at detailing accessibility specifications for new components and gave some resources of other design systems you can explore for inspiration. In part 2, we will give some practical standards and tools you can use to help ensure your design system is truly inclusive. The focus will be on using these to review an existing design system, but if you’re in the process of building a new one, consider how and when these standards and tools can be adapted and integrated throughout your design and development.


## What accessibility standards should I use?


Most companies trying to adhere to digital accessibility standards use the[Web Content Accessibility Guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/) (commonly referred to as WCAG). WCAG are internationally recognized and they provide a framework for creating accessible web content, including websites, web content and mobile applications. There are different versions of WCAG and it is highly recommended you use the latest version, WCAG 2.2. Each version of the guidelines includes “Success Criteria,” which outline the key requirements for achieving digital accessibility. For example, one Success Criterion is ‘[2.1.1 Keyboard](https://www.w3.org/WAI/WCAG21/Understanding/keyboard.html) ,’ which aims to ensure pointer actions have a keyboard equivalent. A person reliant on using a keyboard to navigate should be able to do so without using a mouse. Another Success Criterion is ‘[1.4.3 Contrast (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html) ,’ which covers the minimum contrast ratio text should have against its background so it can be read by more people.


WCAG has different ‘levels,’ A, AA and AAA, that reflect increasing degrees of accessibility. Regardless of the version of WCAG used, most companies strive for level AA and this means covering all Success Criteria at level A and AA. Level AAA contains additional checks to help make your product even more accessible so you may want to consider meeting some of them, even if you are unable to adopt them all. Success Criteria at this level can be more stringent and challenging though, so this is why most companies go for level AA. At WCAG 2.2 level AA, there are 55 Success Criteria. You should review your design system against all of them to ensure compliance. However, if this seems overwhelming, consider that Success Criteria will not always apply to you. For example, if you do not have video or audio content components in your design system, the Success Criteria specifically addressing multimedia content (such as videos and audio) can be skipped.


It is important to note that adherence to WCAG does not guarantee a good user experience. WCAG provides a useful and structured framework to help make web content more accessible for people with disabilities. However, focusing solely on the guidelines can lead to a ‘checklist’ approach. People have varied preferences, needs, and online behaviors which extend well beyond the scope of any accessibility guidelines. Truly inclusive experiences need a holistic approach which goes beyond compliance.


## Accessible design reviews


An accessibility design review is carried out before development to catch potential accessibility issues as early as possible in the design phase. Bolting on accessibility solutions as an afterthought rarely provides a great UX for people needing accessible and inclusive products. It can also be significantly more challenging and resource-heavy trying to fix accessibility defects at a later stage. By annotating potential (or even existing) accessibility issues in your design system early on, accessibility issues can be addressed before anything is coded.


If you already have a design system, a design review is likely to be more useful for reviewing patterns or flows than simpler ‘out of the box’ components and you can also use them to review applications which have been built using your design system. The more complex an interface is and the more ‘building blocks’ (components) it contains, the greater the likelihood of accessibility issues creeping into development. Design reviews are a proactive way of mitigating this risk.


### What can we capture with accessible design reviews and what tools can be used?


There are certain types of visual issues, such as the contrast of text or important icons, which can be determined by simply checking the user interface (UI). However, there are many more potential issues which could occur if markup is done incorrectly, such as the labelling of buttons or the keyboard focus order.


You certainly won’t be able to foresee every potential accessibility problem, but here are some of the things you can consider as a designer:


1. Color and contrast


- Check that text and interactive elements meet **4.5:1** (normal text) or **3:1** (large text and UI components).
- Ensure color **is not the only way** to convey important information (use icons, labels, textures).
- Take advantage of tools like[Color Contrast Analyser by TPGI](https://www.tpgi.com/color-contrast-checker/) and Figma plugins such as[axe for Designers by TPGi](https://www.figma.com/community/plugin/1085612091163821851/axe-for-designers-a-free-accessibility-plugin) and[Contrast and Accessibility Checker by Stark](https://www.figma.com/community/plugin/732603254453395948/stark-contrast-accessibility-checker) . The Figma plugins will check design aspects such as the touch target size of controls and color contrast, and they also have annotation features.


2. Typography


- Ensure that font choices are clear and legible.
- Text should be **resizable up to 200%** without breaking layouts. Refer to[Success Criterion 1.4.4 Resize Text](https://www.w3.org/WAI/WCAG21/Understanding/resize-text.html) .
- Check line spacing and paragraph spacing are sufficient ([Success Criterion 1.4.12 Text Spacing](https://www.w3.org/WAI/WCAG21/Understanding/text-spacing.html) ). You can use a bookmarklet tool such as this[text spacing bookmarklet](https://codepen.io/stevef/full/YLMqbo) .


3. Layout and structure


- Use **clear headings** and hierarchical structure.
- Ensure that there is a logical reading and focus order (e.g. left-to-right and top-to-bottom).
- Design responsive layouts that work at **320px width** without loss of information or functionality, and without requiring scrolling in two dimensions. Refer to[Success Criterion 1.4.10 Reflow](https://www.w3.org/WAI/WCAG21/Understanding/reflow.html) .


4. Images, icons, and media


- Ensure images conveying important information have appropriate **alternative text** . You can use this[alt Decision Tree](https://www.w3.org/WAI/tutorials/images/decision-tree/) .
- Decorative icons and images should be marked as such so they are ignored by assistive technologies.
- Captions/transcripts for audio and video.


5. Interaction states


- All interactive elements must have:


- **Visible focus indicators** .[Refer to Success Criterion 2.4.7 Focus Visible](https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html) ,[Success Criterion 1.4.11 Non-text Contrast](https://www.w3.org/WAI/WCAG21/Understanding/non-text-contrast.html) and[Success Criterion 2.4.11 Focus Not Obscured (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum.html) .
- Clear hover and active states (not just color changes).


- Check that hit targets are **at least 24x24px** . Refer to[Success Criterion 2.5.8 Target Size (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html) .


6. Forms


- Associate text with form labels explicitly wherever possible. Refer to ‘[Labelling Controls](https://www.w3.org/WAI/tutorials/forms/labels/) ’.
- Include[clear form instructions](https://www.w3.org/WAI/tutorials/forms/instructions/) and error messages.
- Show validation inline wherever possible, and ensure errors are conveyed **textually** (not just visually through color). Refer to ‘[User Notification](https://www.w3.org/WAI/tutorials/forms/notifications/) ’.


Annotate any designs clearly with feedback, try to include screenshots and links to resources where necessary, and discuss problems and solutions with other members of your team as much as possible. You might also want to consider giving a suggested priority rating to any accessibility issues to give others an idea of their likely barrier. Typically, many people will categorize the severity of an issue as ‘high’, ‘medium’, or ‘low’.


### Evaluating components, patterns and pages


There are potential accessibility issues you won’t be able to identify in a design system if reviewing just an atomic level component. For example, you might have a checkbox component, and while you can check aspects such as color contrast, focus indication and labels, there are other reviews you will need to make once it’s part of a pattern alongside other controls and content. For example, the helpfulness of error messages are context-dependent and so usually need to be seen as part of a bigger pattern.


If you are only testing components, there are also Success Criteria from WCAG that you will miss. For example, there is a[Success Criterion 2.4.2 Page Titled](https://www.w3.org/WAI/WCAG22/Understanding/page-titled.html) which is to ensure pages have a descriptive and meaningful title. This can’t be tested with components or even patterns as you need to review this in the context of an entire page. You also cannot check aspects such as the responsiveness of a page and whether text can be resized correctly unless looking at bigger blocks of content.


## Semi-automated tools for developers


In order to fully test that a component or pattern in your design system complies with WCAG, you will need to carry out manual testing. Since a design system will be used to help build products, this testing should be very thorough and you should consider using a range of assistive technologies, including a keyboard and screen readers, as well as a range of popular browsers.


Tools which can detect accessibility issues are improving all the time, but they are not foolproof and they cannot check every Success Criteria in WCAG. For example, many good accessibility review tools should be able to identify if you have images missing alternative text for screen reader users. However, they cannot tell you (yet) whether the alternative text given to an image or icon is likely to be meaningful for users. Also, they can also flag false positives, so bugs that are raised will often involve manual review to confirm if they are accurate.


Any semi-automated tools should be used in conjunction with manual testing. If you’ll excuse the cliché, semi-automated tools can speed up testing by capturing the “low-hanging fruit”, but you will need to do the rest. Fortunately, there are some great tools available which we recommend integrating into your auditing process:


- [Axe DevTools by Deque](https://chromewebstore.google.com/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd) : this is a free accessibility browser plugin, available for Chrome, Firefox, and Edge. You can scan a webpage quickly and it will flag certain types of accessibility bugs almost immediately, pin-pointing where they occur in the code. You can also[integrate Axe into your development environment](https://github.com/dequelabs/axe-core) so you can check for accessibility violations in your design system when working on components and patterns.
- [ARC by TPGi](https://www.tpgi.com/arc-platform/arc-toolkit/) : like Axe, there is a free plugin, available for Chrome and Firefox, and it can quickly carry out page-level tests. You can use the tool to look at various aspects of accessibility, including the page structure, color contrast, and misuse of ARIA (Accessible Rich Internet Applications - the attributes used to define roles, properties, and states of HTML elements so they can be better understood by assistive technology users).


## Beyond design reviews, tools, and manual testing


If you’re experienced with accessibility reviews and audits, you should be able to flag any WCAG-related bugs in your design system. However, people with different access needs interface with products differently, and many will be using assistive technologies you’re not reliant on yourself. If possible:


- Include people with disabilities when reviewing and/or auditing your design system. They may uncover usability and accessibility issues and give feedback you hadn’t considered when carrying out your own testing. Getting people to carry out usability testing on individual ‘out-of-the-box’ components may not give you as much useful feedback as when they’re used in a more realistic context. You may want to consider having them reviewed as part of a larger pattern, page, or workflow.
- Continue to monitor your design system and keep up with the latest accessibility standards.
- Listen carefully to feedback from those using your design system. For example, a customer may find the documentation for your design system difficult to follow and they might spot bugs when carrying out their own reviews. By acting on feedback, you can continue to make enhancements.


### Conclusion


By embedding WCAG 2.2 Level AA standards into your design system, conducting early accessibility-focused design reviews, leveraging both visual and semi-automated testing tools, and engaging with users with disabilities, you’re building more than just compliant components - you’re helping cultivate inclusive experiences. Remember that while compliance helps lay the groundwork, inclusive design requires empathy, continuous feedback, and a willingness to iterate. When accessibility becomes a core practice, from the design stage to testing and beyond, your design system not only becomes more robust, but also more adaptable, future-ready, and genuinely user-centric.


Having a robust and accessible design system in place will only take you so far and it won’t be successful without a culture in your organization which fully commits to building accessible digital products. We’ll explore this in more detail in part 3, covering the importance of accessibility buy-in and championing from senior leadership, as well as tips for cultivating an inclusive mindset in your organization.


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
