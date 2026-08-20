---
schema_version: "1.0.0"
document_id: "ae0302f655a88eccd1d171c74416d0cdba102a685ba9b303c007b482391616cf"
company_key: "yc-1code-21stdev"
company: "1code (21st.dev)"
source_id: "yc-1code-21stdev-news-import-0b053d108858"
canonical_url: "https://21st.dev/blog/introducing-shader-builder"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-24T04:15:31.771016+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:ae2b5c509be5bc9415fe68b1b95d3fd1e8fdb8cd83a570b3d924e79094abb4e4"
---

# Introducing Shader Builder: Animated Backgrounds You Can Ship as a Component

# Introducing Shader Builder: Animated Backgrounds You Can Ship as a Component


Build an animated WebGL shader background in your browser with knobs instead of code. Tune the style, palette, surface, and motion, make it react to the cursor, then export it as an image, video, or a drop-in React component. Remix and publish looks the whole community can build on.


Serafim Korablev


[@serafimcloud](https://x.com/serafimcloud)


## Backgrounds that move


The best backgrounds on the web are shaders: a fragment of GLSL that paints a soft, living gradient of noise, waves, and light behind everything else. They look incredible and they are miserable to make. You either copy a wall of math you cannot read, or you open a node editor built for graphics engineers and give up.


So we built a real editor for it. Shader Builder lets you make an animated shader background right in your browser, with knobs instead of code. Pick a look, tune it, and it renders live on a full WebGL canvas as you work. Nothing to install, no GLSL required.


## A studio, not a snippet


A shader you copy off the internet is frozen: one look, one palette, and you are stuck with it. This gives you a full board of controls, and every one of them updates the canvas instantly:


- **Style** is the raw pattern: flowing noise, waves, rings, stripes, metaballs, halftone, and more. Each preset has its own live thumbnail so you can see the look before you commit.
- **Palette** sets the colors. Start from a preset, mix your own, or pull the exact palette from any published community theme so your background matches your app.
- **Surface** is the finish on top: brightness, saturation, hue, blur, a vignette, and film grain to take the digital edge off.
- **Motion** decides how it drifts and pulses. Speed it up, slow it down, or reverse the flow.


Everything you change is one canvas away, and undo and redo walk you back and forth through everything you tried.


## Make it react to the cursor


Here is the part that makes people stop scrolling. A shader background does not have to sit still while you move over it. Turn on a cursor effect and the pattern answers the pointer:


- **Push** moves the whole field with the cursor.
- **Repel** pushes the pattern away from it.
- **Swirl** twists the pattern around it.
- **Ripple** sends ripples out from it.
- **Spotlight** brightens a soft area under it.


It is the difference between wallpaper and something that feels alive the moment someone lands on your page.


## Export a real component, not a screenshot


When it looks right, take it with you in whatever form you need. Export a still as a PNG, JPEG, or WebP, or export the animation as a video or GIF, rendered frame by frame right on your machine at up to 4K so the quality holds.


But the one that matters for builders: export the code. Grab the raw GLSL fragment shader, or a drop-in **React component** you can paste straight into your app as a live animated background. No canvas wiring, no uniforms to hook up, no dependency to add. It just runs.


## A shader is a recipe you can share


Every look you build is a recipe: the full set of style, palette, surface, motion, and cursor settings, saved as one thing. A recipe is not a flat image. It is the settings, so anyone can open it, remix it, and make it their own.


That means the looks are reusable the same way a good preset is, and the good ones get better as people build on them.


## Browse, remix, and publish


There is now a community gallery of shaders. Browse what people made, open any one to see it full size and moving, and bookmark the ones you want to come back to. Like one? Open it in the editor and it lands in your controls instantly, ready for you to tweak into something of your own.


When you make a shader you are proud of, publish it. Name it, tag it, and it joins the gallery with its own shareable page and a copy-the-code button. You can manage everything you publish from Studio and feature your best shaders on your profile.


[Open the editor →](https://21st.dev/community/shaders/editor)


## Published


Jul 10, 2026


## Read time


4 min


## Tags


Launch


Shaders


Creative


## Share


## Links


[Open the editor](https://21st.dev/community/shaders/editor)[Browse shaders](https://21st.dev/community/shaders)
