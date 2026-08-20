---
schema_version: "1.0.0"
document_id: "ea782691a8e81b2e457ee9a86a00c174b8538b7a19d8897fcf2439d712581c46"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/styleguide"
published_at: "2017-01-18T22:14:32+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T22:27:28.827282+00:00"
content_hash: "sha256:f05a1ce3155b7d88b00a3328b890d6cd3ae518cfc5f8e3259721e6c746569ebf"
---

# Evolving Our Style Guide to Better Unite Designers and Developers

Exactly two years ago Jess Brown wrote[Bridging the Design-Development Divide](http://dresscode.renttherunway.com/blog/design-dev) . Her post summed up Rent the Runway’s efforts to create consistent styling, and then translate that styling into[Sass](http://sass-lang.com/) variables and classes for easy reuse. This ultimately was embodied in a style guide that lived in GitHub Pages and was publicly accessible.


Jess Brown’s GitHub Pages style guide


The style guide that Jess created greatly aided in developer-designer relations by ensuring designer mockups were consistent with the colors and Sass stylings powering the site. However, because it was *separate* from our site and we were rapidly evolving our brand identity, the GitHub Pages style guide quickly fell behind our storefront stylings. There was nothing forcing the guide to be kept up-to-date.


Since Jess’ creation of the first robust style guide for RtR, the company underwent a rebrand as well as a shift towards using[React](https://facebook.github.io/react/) . This created a new opportunity for a more dynamic system of presenting, maintaining, and using our styles. React allows for the creation of reusable components of varying scales such as a button, product card, or even carousel of products. Components can be nested and reused for various purposes, allowing for flexible building blocks to be created for pages. By taking the elements of the styleguide and translating them into React components, we are able to ensure consistent stylings across the site’s elements.


In tandem with the translation of site elements into components, we moved the style guide into the same code base as the storefront. This means that **the style guide is powered by the same Sass files that power the site’s front end** . Change the font on a button in the style guide and it is instantly reflected site-wide!


The new React component based[style guide](http://renttherunway.com/pages/styleguide) .


We then categorize these components for easy discoverability according to Brad Frost’s[Atomic Design](http://bradfrost.com/blog/post/atomic-web-design/) principles. This means that small components such as buttons, links, etc. are labeled as “atoms.” Collections of atoms are categorized as molecules and so on. If you are interested in creating an atomic design style guide for your own use, checkout[patternlab.io](http://patternlab.io/) . It provides templates for creating style guides that are organized by atomic design principles and served as an inspiration for Rent the Runway’s guide.


So this all sounds great, but one of the biggest challenges is getting cross team collaboration and buy-in to ensure that the living style guide becomes an effective manner of generating consistent styles and fast paced development. For the engineering team, I presented to the Architecture Review Board (ARB) for the introduction of a new grid system in tandem with the style guide. At RtR, we use "ARBs" to screen changes to the way we work, such as using new libraries, or accepting new naming conventions. The ARB for the guide was accepted and the new grid and stylings are being used on Rent the Runway’s pages today. For the product and creative teams, I created Sketch and Photoshop templates of the elements and grid system for use by PMs and Designers, ensuring that the designs the engineering team receives can be built by the frameworks and components in the style guide. In theory, this combined toolkit means every design being passed from a UX designer to a front end engineer can be built by laying out React components from the style guide using the new grid system which in turn greatly reduces repetition of code and speeds up development times.


We will continue to advocate the use of this guide for cataloging, onboarding, and accessibility purposes. By centralizing our styles onto the same Sass codebase, we hope to ensure seamless translation of UX/UI designs into pages.
