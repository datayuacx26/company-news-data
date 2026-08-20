---
schema_version: "1.0.0"
document_id: "8b24acbb59f63d8cdbff83d04854a8a0832b94c80b3ee3e3f9fb7233b0de611c"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/motion-vs-gsap/"
published_at: "2026-08-15T00:00:00+00:00"
first_seen_at: "2026-08-15T15:41:58.644603+00:00"
fetched_at: "2026-08-15T15:41:59.466191+00:00"
content_hash: "sha256:26c4e88388e0a34a4617907ea0512de6c09edf8b5cf5dca8fae960da6b7e3a38"
---

# Motion vs GSAP: Do You Need Both?

Most React projects should start with **Motion** for component and state-driven animation, and add **GSAP** only when a concrete need appears — a scrubbed scroll timeline, complex sequencing, or SVG/canvas work.


Anyone who has tried to bolt a pinned, scrubbed scroll section onto a React app knows the moment this question turns up: the animation works, a re-render wipes it out, and you start wondering whether you picked the wrong library. Running both is common, fully supported, and often the right call for content-heavy sites, but it costs you a second runtime and a second mental model, so it isn’t a default. This piece compares the two on the criteria that actually decide the choice.


Two facts invalidate most older comparisons. Motion (formerly Framer Motion) is now[available for React, JavaScript, and Vue](https://www.npmjs.com/package/framer-motion) , and you import it from` motion/react` instead of` framer-motion` . It is no longer React-only. And GSAP is no longer paid: thanks to Webflow,[the entire GSAP toolset is now free](https://gsap.com/blog/3-13/) , including all the bonus plugins like SplitText and MorphSVG that were previously exclusive to Club GSAP members, even for commercial use.


## Key Takeaways


- Motion (formerly Framer Motion) is framework-agnostic (React, JavaScript, Vue), MIT-licensed, imported from` motion/react` , and on the v12 line: v12.42.2 shipped on 30 June 2026.
- GSAP is 100% free including every previously paid plugin, after Webflow acquired GreenSock in 2024; the v3 line is current, with v3.15.0 released in April 2026.
- The split is declarative (Motion: describe the end state) vs imperative (GSAP: direct each step on a timeline).
- Yes, using both is common and officially supported: GSAP’s` useGSAP()` hook from` @gsap/react` makes the two coexist cleanly in React and Next.js.
- Default for a React app: start with Motion alone; add GSAP the first time you hit a scrubbed scroll timeline or SVG/canvas sequencing.


## How do Motion and GSAP differ?


The mental-model split is the whole decision. Motion is declarative (you describe the end state and it interpolates), while GSAP is imperative and timeline-driven, where you direct each step with fine-grained control over timing and easing.


In Motion, you set target values as props and the library figures out the transition:


```text
import   {   motion   }   from   "  motion/react  "  ;


<  motion.div
initial  =  {  {   opacity  :   0  ,   y  :   20   }  }
animate  =  {  {   opacity  :   1  ,   y  :   0   }  }
transition  =  {  {   duration  :   0.4   }  }
/>
```


When the values in` animate` change, Motion automatically transitions between them; physical properties like` x` and` scale`[use spring physics by default](https://motion.dev/docs/react) , while visual properties like` opacity` use tween easing.[GSAP](https://gsap.com/) inverts this: you write imperative calls and chain them on a timeline for staggered, sequenced control. That difference (describe the result vs choreograph each beat) predicts which tool fits a given problem better than any benchmark does.


## Motion vs GSAP compared on the criteria that matter


Here is the head-to-head on the axes a frontend developer weighs when adopting an animation library: mental model, integration, scroll/timeline work, performance, bundle, framework reach, and licensing.


Criterion Motion GSAP


Mental model Declarative, component/state-driven Imperative, timeline-driven


React integration Native` motion` components,` AnimatePresence` ,` layout` prop` useGSAP()` hook from` @gsap/react`


Scroll & sequencing` useScroll` , scroll-linked effects ScrollTrigger, nested timelines, stagger


Reach React, JavaScript, Vue Any JS framework, plus SVG/canvas/WebGL


Rendering Hybrid: Web Animations API + JS fallback Own JS engine


Licensing MIT Free “no-charge” standard license


Latest release v12.42.2 (June 2026) v3.15.0 (April 2026)


On integration, Motion is built for component apps: it animates between any two layouts with a single` layout` prop, and[AnimatePresence](https://motion.dev/docs/react-animate-presence) keeps elements alive so they can animate as they leave the DOM. GSAP earns its place through timelines and[ScrollTrigger](https://gsap.com/docs/v3/Plugins/ScrollTrigger/) , which is the standard for scroll-driven animation.


On performance, both target 60fps in typical use, and neither vendor’s marketing settles it. Motion’s hybrid engine runs animations natively in the browser using the Web Animations API and ScrollTimeline, falling back to JavaScript when it needs spring physics or gesture tracking.[Motion’s homepage](https://motion.dev/) claims its APIs are “up to 90% smaller than their GSAP alternative,” but that is the vendor’s own figure, not a neutral measurement. Treat it as marketing. What matters is behavior on real devices. Session replay of production pages catches the failure class benchmarks miss: dropped frames on scroll-linked animation, or layout shift introduced by an entrance transition on a mid-tier phone.


On bundle, both are modular. Motion is tree-shakable with a small footprint, and GSAP’s core is light with plugins added à la carte. On licensing, Motion is[MIT licensed](https://www.npmjs.com/package/motion) , and GSAP now ships under a free standard license.


## When should you use Motion vs GSAP?


Reach for Motion for state-driven UI: enter/exit transitions with` AnimatePresence` , automatic layout animations, gestures, and page transitions inside a component tree. It maps cleanly onto React’s render cycle, so animations respond to state and props without manual wiring.


Reach for GSAP for scrubbed, sequenced set-pieces: scroll-linked storytelling with ScrollTrigger, staggered timelines, and SVG or canvas animation that Motion doesn’t target. GSAP animates CSS, SVG, canvas, React, Vue, WebGL, colors, strings, and motion paths, the breadth that marketing sites and data-viz work depend on. Both libraries can honor` prefers-reduced-motion` , so accessibility isn’t a tiebreaker; treat it as a shared responsibility either way.


## Do you need both?


Yes: it’s common and fully supported to run both, Motion for component-level micro-interactions and GSAP for scroll and timeline set-pieces. The combination is a first-class path in React.[useGSAP() from the @gsap/react package](https://www.npmjs.com/package/@gsap/react) is a drop-in replacement for` useEffect()` /` useLayoutEffect()` that automatically handles cleanup, and it uses the isomorphic layout-effect technique so it’s safe in server-side rendering environments, including Next.js App Router, provided the component is a client component:


```text
"  use client  "  ;
import   {   useRef   }   from   "  react  "  ;
import   gsap   from   "  gsap  "  ;
import   {   useGSAP   }   from   "  @gsap/react  "  ;


gsap  .  registerPlugin  (  useGSAP  );


function   Hero  () {
const   container   =   useRef  (  null  );
useGSAP  (()   =>   {
gsap  .  from  (  "  .title  "  , {   y  :   40  ,   opacity  :   0  ,   duration  :   0.6   });
}, {   scope  :   container   });


return   (
<  section   ref  =  {container}>
<  h1   className  =  "  title  "  >Hello</  h1  >
</  section  >
);
}
```


The cost of “both” is real: two animation runtimes mean additive bundle weight and two mental models on the team. Add the second library only when a concrete need appears, not by default. In practice that means Motion carries your buttons, modals, and layout transitions, while GSAP handles the one scroll-narrative section that Motion would fight you on.


## The verdict


Default recommendation for a React app: start with Motion alone, and introduce GSAP the first time you hit a scrubbed scroll timeline, complex sequencing, or SVG/canvas work. But if a project is *mostly* scroll-driven narrative, start with GSAP instead and reach for Motion only if component-state animation grows. Both are current, free, and framework-capable, so the decision is about fit, not licensing. Pick the one your dominant animation pattern points to, pin the versions (Motion v12.42.2, GSAP v3.15.0), and add the second only when a real requirement forces your hand.


## FAQs


Is Framer Motion dead, and what happened to it?


Framer Motion is not dead; it was renamed to Motion and became an independent project in 2024, expanding beyond React to also support JavaScript and Vue. You now install the 'motion' package and import from 'motion/react' instead of 'framer-motion'. It remains MIT-licensed and actively maintained on the v12 line. Older tutorials referencing 'framer-motion' imports still work through a compatibility path, but new projects should use the current package name.


Is GSAP still paid, or do I need a Club GreenSock license?


GSAP is now completely free, including every previously paid plugin such as SplitText, MorphSVG, DrawSVG, ScrollTrigger, and ScrollSmoother. After Webflow acquired GreenSock in 2024, the paywall was dropped on April 30, 2025, and the standard license was expanded to cover commercial use. The library is free for everyone, whether or not you use Webflow, so no Club GreenSock membership is required for any feature.


Can I use Motion and GSAP together in a Next.js App Router project?


Yes. Use GSAP's official useGSAP() hook from the '@gsap/react' package, which handles cleanup and React Strict Mode, and mark the component with the 'use client' directive since App Router components are server components by default. useGSAP() uses the isomorphic layout-effect technique, so it is SSR-safe as long as it runs in a client component. Motion components work in the same tree for state and layout animations without conflict.


Does Motion support scroll-triggered animations, or do I still need GSAP's ScrollTrigger?


Motion supports scroll-linked animation through its useScroll hook and native ScrollTimeline, which covers common effects like progress bars and parallax tied to scroll position. For scrubbed, precisely sequenced scroll narratives with pinning, snapping, and complex timelines, GSAP's ScrollTrigger offers more control and is the established standard. If your scroll work is a straightforward tie between scroll progress and a value, Motion is enough; reach for ScrollTrigger when the choreography gets complex.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
