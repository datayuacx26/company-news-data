---
schema_version: "1.0.0"
document_id: "d13fa1adef6a5dbe499139934e51d5dce7498e150d43e51feeff72f9ebabbc57"
company_key: "yc-common-paper"
company: "Common Paper"
source_id: "yc-common-paper-rss-3b76f5ee637c"
canonical_url: "https://commonpaper.com/blog/speeding-up-common-paper-reducing-image-size-by-99/"
published_at: "2022-11-14T19:40:19+00:00"
first_seen_at: "2026-07-20T23:20:15.555416+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:b146f042fdbc031c027767660edc327d9aea3bde258e32e60099399e84d5be8b"
---

# Speeding up Common Paper: Reducing Image Size by 99%

As the lead designer at Common Paper, I love working with the visual elements that make our brand unique. The paper “swooshes” you see throughout commonpaper.com came out of our our brand design process with[Focus Labs](https://focuslab.agency/) . These images have a wonderful graininess that mimics the pulp of real paper, adding a real-world richness to the site.


Of course, using large background images like this has its downsides: large file sizes translate to slow page loads. We used CSS media queries to serve the smallest possible image for each device size, but even so, sometimes the background image file we needed to load was huge – nearly 6 MB!


## Load time matters


Adding 6 MB to your site’s load time could be rough for a few reasons. Not all users who come to your site have speedy broadband, and a painfully slow page load is a bad user experience. Accordingly,[search engines prioritize sites that load faster](https://developers.google.com/search/blog/2018/01/using-page-speed-in-mobile-search) .


This problem was bugging me. So when Common Paper held its first hackathon, I decided to see if we could “vectorize” our images: turning them into bits of code the browser can render, instead of millions of pixels that needed to load.


Ideally, I want my final image to be an .svg, with each part of the paper created by its own shape, and a transparent background so I could layer the paper over whichever background color I wanted later. I first revisited our old friend Adobe Illustrator and its .svg export options. Creating my “swooshes” as vector shapes was simple enough, and applying gradients to give them a 3D effect was easy, too. This gave me a solid starting point:


But what about the grain effect? You can, in fact, add a noise texture to these shapes and export everything as an .svg with Illustrator. But it converts the *noise* into a .png, embedded inside the .svg. No file size savings to be found there.


## Enter feTurbulence


“feTurbulence” is an .svg filter function. With a little bit of code, the filter tells your browser how to create a pattern. feTurbulence can do some really interesting things:


Examples from[CSS](https://css-tricks.com/creating-patterns-with-svg-filters/)[–](https://css-tricks.com/creating-patterns-with-svg-filters/)[Tricks](https://css-tricks.com/creating-patterns-with-svg-filters/)


In this case, I just needed a simple bit of random, repeating noise. Here’s what my .svg file looked like when I exported from Illustrator, indented to make it easier to read, and renamed some classes for clarity:


```text
<svg width="1728" height="2688" viewBox="0 0 1728 2688" xmlns="http://www.w3.org/2000/svg">


<defs>


<style>
.svg-paper {
fill:url(#gradient-paper);
}


.svg-fold {
fill:url(#gradient-fold);
}
</style>


<linearGradient id="gradient-paper" x1="-217.67" y1="1124.8" x2="1379.06" y2="479.68"
gradientTransform="translate(119.91 329.76) rotate(7)" gradientUnits="userSpaceOnUse">
<stop offset="0" stop-color="#161d23"/>
<stop offset="1" stop-color="#20575e"/>
</linearGradient>


<linearGradient id="gradient-fold" x1="1028.49" y1="1510.89" x2="2002.85" y2="263.76"
gradientTransform="translate(145.97 -96.3) rotate(7)" gradientUnits="userSpaceOnUse">
<stop offset="0" stop-color="#a0e4e2"/>
<stop offset="1" stop-color="#79bebb"/>
</linearGradient>


</defs>


<g>
<path class="svg-paper"
d="M1801.5,2175.95L1314.79,268.77l-.02,.14c-13.15-41.8-68.83-62.64-136.07-82.64C970.78,124.42,
354.73,0,354.73,0L0,2800l1801.5-624.05"/>


<path class="svg-fold"
d="M1801.5,2175.95S737.27,858.32,737.27,858.32c0,0,363.8-275.55,491.69-415.98,71.36-78.36,94.
65-130.07,87.54-166.63"/>
</g>


</svg>
```


You can see we have 2 “path” shapes (the paper background and the fold), 2 gradients, and little bit of header CSS that assigns a gradient to a shape. Nothing too scary so far! Let’s add our filter:


```text


<defs>


<style>
.svg-paper {
fill:url(#gradient-paper);
}


.svg-fold {
fill:url(#gradient-fold);
}
</style>


<stop offset="0" stop-color="#161d23"/>
<stop offset="1" stop-color="#20575e"/>
</linearGradient>


<stop offset="0" stop-color="#a0e4e2"/>
<stop offset="1" stop-color="#79bebb"/>
</linearGradient>


<filter id="noise-filter" filterUnits="objectBoundingBox" x="0" y="0"
width="100%" height="100%">
<feTurbulence
baseFrequency='.75'
numOctaves='1'
stitchTiles='stitch'
result="noisy-result" />
<feColorMatrix type="saturate" values="0"/>
<feBlend in="SourceGraphic" in2="noisy-result" mode="overlay"/>
</filter>


</defs>


<g>
<path class="svg-paper" filter="url(#noise-filter)"
354.73,0,354.73,0L0,2800l1801.5-624.05"/>


<path class="svg-fold" filter="url(#noise-filter)"
65-130.07,87.54-166.63"/>
</g>


</svg>
```


Because turbulence applies to your whole shape, even if it has other special effects like drop shadows, the noise itself extends beyond the shape by default. So we need to “clip” the edges of our noise. Here we’ll define a few clipping paths – using the same bounds as our paper pieces – and apply them to our shapes.


```text


<defs>


<style>
.svg-paper {
fill:url(#gradient-paper);
}


.svg-fold {
fill:url(#gradient-fold);
}
</style>


<clipPath id="paper">
<path d="M1801.5,2175.95L1314.79,268.77l-.02,.14c-13.15-41.8-68.83-62.64-136.07-82.64C970.78,124.42,
354.73,0,354.73,0L0,2800l1801.5-624.05"></path>
</clipPath>


<clipPath id="fold">
<path d="M1801.5,2175.95S737.27,858.32,737.27,858.32c0,0,363.8-275.55,491.69-415.98,71.36-78.36,94.
65-130.07,87.54-166.63"></path>
</clipPath>


<stop offset="0" stop-color="#161d23"/>
<stop offset="1" stop-color="#20575e"/>
</linearGradient>


<stop offset="0" stop-color="#a0e4e2"/>
<stop offset="1" stop-color="#79bebb"/>
</linearGradient>


<filter id="noise-filter" filterUnits="objectBoundingBox" x="0" y="0"
width="100%" height="100%">
<feTurbulence
baseFrequency='.75'
numOctaves='1'
stitchTiles='stitch'
result="noisy-result" />
<feColorMatrix type="saturate" values="0"/>
<feBlend in="SourceGraphic" in2="noisy-result" mode="overlay"/>
</filter>


</defs>


<g>
<path class="svg-paper" filter="url(#noise-filter)" clip-path="url(#paper)"
354.73,0,354.73,0L0,2800l1801.5-624.05"/>


<path class="svg-fold" filter="url(#noise-filter)" clip-path="url(#fold)"
65-130.07,87.54-166.63"/>
</g>


</svg>
```


Does it work?


It does! (mostly)


## Cross-browser acceptance


With any fancy web trick, we need to test whether the major browsers accurately support what we’re trying to do.


### **Blend modes**


Turbulence filters require a “blend mode”, and not all browsers respect the same options. The filter I used on pale colors looked like party confetti on dark colors, but blend modes (like color burn) that could take the contrast down a notch didn’t work consistently.


I ended up using a “light” turbulence filter in overlay mode for pale colors, and added a “dark” filter in multiply mode for our deep green color.


```text
<filter id="noise-filter" filterUnits="objectBoundingBox" x="0" y="0" width="100%" height="100%">
<feTurbulence
baseFrequency='.75'
numOctaves='1'
stitchTiles='stitch'
result="noisy-result" />
<feColorMatrix type="saturate" values="0"/>
<feBlend in="SourceGraphic" in2="noisy-result" mode="overlay"/>
</filter>


<filter id="noise-filter-dark" filterUnits="objectBoundingBox" x="0" y="0" width="100%" height="100%">
<feTurbulence
type='turbulence'
baseFrequency='.5'
numOctaves='1'
stitchTiles='stitch'
result="noisy-result" />
<feColorMatrix type="saturate" values="0"/>
<feBlend in="SourceGraphic" in2="noisy-result" mode="multiply" />
</filter>
```


### **Smooth rendering**


We’re asking the browser to create gradients and patterns for us, but each browser performs that task differently. A gradient + filter combination that looks great on Safari can look stripey on Firefox.


Darker colors seemed more prone to this behavior. For the smoothest look in files with dark colors, I ended up completely separating my gradient from my noise. I made one path with a solid green fill, applied my noise to *that* , and added a second path on top with a shadow gradient that gave me the 3D effect back.


.Svg will render elements in the order that you list them in the code, so you can stack shapes like this just by entering the top shape last.


### **General bugginess**


My .svg files now looked great, but when it came time to use them as background images, a perplexing bug popped up. I sometimes lost one paper piece entirely—but only when the image was very large, only when the feTurbulence filter was applied, and only on Safari.


Full disclosure: I haven’t totally figured this one out. But in the spirit of hackathon, I hacked my way to a solution: adding extra space in the .svg viewport setting seemed to fix it. This doesn’t change my file size because it’s all the same amount of code.


To make this hack a little more robust, I added a few more paths: “backup” shapes that will load just the gradient and *no* turbulence, stacked behind everything else. They’ll be there in case the noisy shape never manages to load.


## Final results


Even with all my filters and gradients and layers, vectorizing the image on the homepage of commonpaper.com took that background file size from 6 MB down to 1 kB. What used to take half a second to load by itself now takes 55 milliseconds.


Not bad results for a hacakthon project! I can now use this part of our branding with confidence, knowing it won’t slow our users down.
