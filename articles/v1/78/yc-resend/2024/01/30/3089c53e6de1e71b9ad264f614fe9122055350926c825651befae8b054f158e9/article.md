---
schema_version: "1.0.0"
document_id: "3089c53e6de1e71b9ad264f614fe9122055350926c825651befae8b054f158e9"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/introducing-light-mode"
published_at: "2024-01-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:26:18.654833+00:00"
content_hash: "sha256:be88fb96aeb8f4d062f09f88a9d3f815df858e3aafb1c2e05fba44f6ad7d8c27"
---

# Introducing Light Mode

Today, we're thrilled to introduce something new - not just for you but also for us: **Resend light mode** .


While the dark color palette remains a staple of Resend's identity, the addition of light mode offers a different perspective to your experience.


Whether it's reducing eye strain during working sessions or enhancing visibility in various conditions, we believe in providing a flexible and inclusive experience for everyone.


Powered by[Radix Colors](https://www.radix-ui.com/colors) and[Tailwind](https://tailwindcss.com/) , we're excited to share how we implemented light mode into our product.


## Why now?


As Resend expands to dozens of thousands of users, we heard feedback on dark mode being difficult for some to use.


Our goal is for people across your entire organization, not just technical users, to collaborate on emails using Resend. That's why we want to respect everyone's preferences.


## How did we take our brand from dark to light mode?


Our brand identity hasn't changed; we still predominantly use dark colors in our communication assets. What we did was expand our dashboard to include light mode, primarily for accessibility purposes.


However, all marketing communications, landing pages, and public pages will continue to use our original, dark brand colors exclusively.


## How did we implement this into our product?


This wasn't a one-to-one conversion from black to white. We began by conducting color experiments in Figma to understand what works and what doesn't.


When designing for a light background, many colors had their lightness and saturation adjusted to ensure legibility. We also had to consider the contrast ratio between the background and foreground colors.


The tech stack facilitated this transition a lot. By relying on[Radix Colors](https://www.radix-ui.com/colors) and[Tailwind](https://tailwindcss.com/) , we accommodated approximately 70% of our needs by importing Radix colors as CSS variables and incorporating these values into our Tailwind configuration.


```text
@  import     '@radix-ui/colors/slate-alpha.css'  ;    @  import     '@radix-ui/colors/slate-dark-alpha.css'  ;
```


```text
module  .  exports   =     {        content  :     [  './src/**/*.{js,ts,jsx,tsx}'  ]  ,        theme  :     {          extend  :     {    		  colors  :     {    			  slate  :     {    				    1  :     'var(--slate-a1)'  ,    				    2  :     'var(--slate-a2)'  ,    				    3  :     'var(--slate-a3)'  ,     ...
```


For the animated icons powered by[Lottie](https://airbnb.design/lottie/) , we used a trick to invert the existing icons with the CSS rule` filter:invert(1)` to avoid generating new JSON files for each color.


We then started fine-tuning colors and other elements using Tailwind. For example, in light mode, the sidebar has a background color, but not in the dark mode. To achieve that, we used the` dark:` variant from Tailwind.


## How about the documentation?


Thanks to[Mintlify](https://mintlify.com/) , we could implement light mode for our docs with a few lines of code.


You can find the new theme toggle on the top right corner of the[documentation page](https://resend.com/docs) .


## Get Started


By default, dark mode is on, but now you have the option to switch the theme to light mode when it suits for you.


Visit the[Resend Dashboard](https://resend.com/emails) today and press` M` to enable light mode.
