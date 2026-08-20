---
schema_version: "1.0.0"
document_id: "cabea6c669dde89127cbc1e45194b1d836dff0c4ad4eb0970e109427391c54ba"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/building-cross-platform-image-renderer"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:f0ba814113ffe6ff03435ab57b938c89985bf71f9a38b809e79136a284d2215e"
---

# Building a fast cross-platform image renderer

As is the case with many mobile apps, Photoroom started as an iPhone application. We leveraged Apple's[Core Image](https://developer.apple.com/documentation/coreimage) framework, an amazingly fast, high-quality, and color-accurate image processing pipeline that is also highly extensible. In Photoroom's early days, Core Image allowed a tiny[team of 2 developers](https://apps.apple.com/en/story/id1515847268) to quickly create an image editor used by millions.


When we started to build the Android version of Photoroom, we realized that nothing in the green robot ecosystem was remotely comparable. Some people recommended[GPUImage](https://github.com/cats-oss/android-gpuimage) or[GPUEffect](https://github.com/Frntbac/GPUEffect) for hardware-accelerated image processing. We started experimenting with those but quickly found that they were lacking in flexibility (most notably assuming that filters will always be applied as a chain). And so we decided to roll out our own solution, heavily inspired by the features of Core Image we loved.


## What do we need?


### 🤸‍♂️ **Flexibility**


What we loved about working with Core Image, and that was lacking in the various options we had tried, was how easy it was as a developer to create complex non-linear processing chains. It's easy to think of image processing as a series of transformations you are going to apply one after the other to your original image: "I am going to add brightness, change contrast, apply a blur, *etc* ." but real use cases are often more complicated. One intermediate result of your processing chain might be used as the input to several other steps, which will then be re-composited together. A good system allows you to easily express that: instead of a filter chain, we want our pipeline to express a[Directed Acyclic Graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) of image transformations.


Of course, it is also important to us that image transformations ("kernels" as we call them) can be easily expressed and plugged into the system from outside the core library: ideally any of our developers should feel it is not necessary to understand the inner workings of the core system to be able to create new effects.


### 🏎️ Performance


A typical image going though Photoroom will have 10s of transformations applied to it: brightness, contrast, color temperature are very often applied by our users, in addition to shadows (which require a combination of masking, blurring, and various geometric transformations) and outlines. It is important that our system recognizes the transformations that can be batched together for maximum efficiency (like color-only and geometry-only transformations) and when to perform an intermediate render to cache a result that might be accessed multiple times.


In more technical terms: we want to try and pack all kernels as the minimum number of shaders that will be applied to an image, as to limit the number of render-passes necessary to draw our final result. This will limit the stress on memory as we apply them on the GPU, both in terms of total memory usage (less intermediate results = less memory-hungry textures to store them) but also in terms of available bandwidth (less loading / storing of intermediate results).


### 🌍 Portability


Finally, although this reflection started as how to build our Android pipeline, it quickly became clear that down the line we would want to try to bring[Photoroom to the web,](https://app.photoroom.com/create) with the same effects and render quality as our other platforms. As such, we decided that whichever technical solution we would find should not be limited to Android but should be portable to other systems;


## Prototyping


In order to satisfy the requirements above, I had a few ideas I wanted to try out; Mainly, I wanted to try building a system that would generate fragment shaders on-the-fly, concatenating as much kernels as possible, as a library which we would be able to transpile with[emscripten](https://emscripten.org/) . While emscripten is not tied to C-family languages ***in theory*** , support for C & C++ seem lightyears ahead of the rest. Before committing to writing a massive project in such a language, I wanted to build at least 2 proof-of-concepts;


1.


Build a proof-of-concept of the kernel-concatenation principle; Have a small program with a limited set of easy transformations (saturation, contrast, brightness, etc) the user could apply on images that would all boil down to 1 render-pass.


2.


Take a shader built with the first PoC, wrap it around a bare-bone C program setting up the OpenGL render-pass applying it on an image, to then compile to WebAssembly and find a way to pass it an` HTMLImageElement` I would fetch in JavaScript.


After a few days of hacking away, I managed to get both those PoC working; So we went on building 👷.


## Introducing PhotoGraph


PhotoGraph is the system we developed to meet the requirements above; I leveraged the experience I acquired while prototyping to develop, in C, a fully-fledged OpenGL (ES) engine that allows us to express and then render image transformation pipelines. It is designed to run on desktop (it is mainly developed under macOS) and mobile platforms (Android) with compatibility for transpiling to WebAssembly + WebGL 1.


It provides:


-


🤸‍♂️A flexible way to express image transformation units ("kernels")


-


🏎️ An API that allows chaining of such units to build complex effects while minimizing the amount of work needed to render them.


-


🔧 A toolbox of pre-written kernels to be used in the calling applications (various declinations of Photoroom).


-


🌍 All of that in a pure-C library with no external dependency besides OpenGL: allowing us to compile and use on any platform that offers an OpenGL-like interface, including the web via WebGL, or GPU-less machines through[osmesa](https://docs.mesa3d.org/osmesa.html) !


Although PhotoGraph is written in C, it is not particularly meant to be used from C-family languages. So far we have bindings in:


-


Swift (easy given the natural integration of C in Swift)


-


Java / Kotlin (via[JNA](https://github.com/java-native-access/jna) )


-


JavaScript (via[emscripten](https://emscripten.org/) )


-


Python 3 (via[ctypes](https://docs.python.org/3/library/ctypes.html) )


Kernels in PhotoGraph can be of one of 3 kinds:


-


**Color kernels:** defining single-pixel color transformations. This is typically what you would use if the transformation you implement does not depend on a pixel's neighbors (it could still depend on its position, like a gradient).


-


**Warp kernels:** defining a change of geometry in an image that does not depend on nor affect color data.


-


**Generic kernels:** defining a general-purpose change in an image. Generic kernels are given access to a sampling function to operate on their source data; so they can get pixel data from all over the input. But with great power comes great responsibility: while many color or warp kernels can be bundled together in a single render-pass, a generic kernel usually needs its own render-pass and thus is dramatically more expensive than its counterparts.


They are expressed as pure functions written in GLSL; The person defining the kernel does not have to take care of passing arguments as *uniforms* or building a complete` main` function assigning` gl_FragColor` , this will be taken care of by the core library depending on how many of those functions are concatenated.


As an example, here is the entire kernel code for the exposure kernel (a color-kernel) provided by PhotoGraph:


```text
vec4 pg_exposure_kernel(
const vec4 color,    // Input, passed as a `vec4` since this is a color kernel
const vec2 pos,      // Pixel position - optional for color kernels, and unused here
const float exposure // Argument, provided by the user when the kernel is applied and passed as `uniform` by PhotoGraph
)
{
// Return processed pixel color (alpha-premulitplied, linear-space RGBA)
return vec4(color.rgb * pow(2.0, exposure), color.a);
}
```


And for completeness, here is the kernel code for the affine transformation kernel (a warp-kernel) provided by PhotoGraph:


```text
vec2 pg_image_transform_kernel(
const vec2 pos, // Input, passed as `vec2` - pixel position - since this is a warp kernel
const mat3 m    // Argument, will be passed as `uniform` by PhotoGraph
)
{
// Return position at which to sample the input
return (m * vec3(pos, 1.0)).xy;
}
```


In most languages, the bindings to the C API contain syntactic sugar to quickly define images and apply kernels, in the way that is the most convenient for the host language. For example, to mask an image by another and to then apply exposure and transformation kernels, in Swift:


```text
let image: CGImage = /* ... */
let mask: CGImage = /* ... */
let processedImage = PGImage(cgImage: image)         // Create image from CGImage
.applying(PGMaskKernel()) {                        // Mask it with the given mask
$0.maskImage = PGImage(cgImage: mask)
}
.applying(PGExposureKernel()) {                    // Reduce exposure by 1 EV
$0.exposure = -1
}
.transformed(CGAffineTransform(scaleX: 2, y: 0.5)) // Distort it
```


The resulting object is an instance of` PGImage` , which you can continue to build upon by applying additional kernels, inspect or render to a` CGImage` .


```text
print(processedImage.debugDescription)


pg_image_transform_kernel extent=[0.00 0.00 7680.00 1080.00]
└──pg_exposure_kernel extent=[0.00 0.00 3840.00 2160.00]
└──pg_mask_kernel extent=[0.00 0.00 3840.00 2160.00]
├──pg_srgb_to_linear extent=[0.00 0.00 3840.00 2160.00]
│  └──pg_sample_kernel extent=[0.00 0.00 3840.00 2160.00]
└──pg_sample_kernel extent=[0.00 0.00 3840.00 2160.00]
```


(Notice how PhotoGraph inserted a` pg_srgb_to_linear` kernel since it detected the input` image` had a` CGColorSpace.sRGB` colorspace property)


## What’s next?


Developing a multiplatform rendering library allowed us to develop complex effects for the Android app with a fast iteration speed, and enabled us to leverage all that work when starting to build the web app.


Today, the code for it stays mostly untouched. New kernels are added from time to time to enrich the library of pre-built effects but the core library that makes them run hasn't been significantly changed in a while.


One last thing to answer: will we port the library to iOS? It's not in the roadmap. Core Image is simply too powerful for us to compete and we don't think that replacing it would bring value to our users.
