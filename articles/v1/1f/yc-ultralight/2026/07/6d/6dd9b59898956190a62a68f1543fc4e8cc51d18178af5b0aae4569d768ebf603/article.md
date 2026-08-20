---
schema_version: "1.0.0"
document_id: "6dd9b59898956190a62a68f1543fc4e8cc51d18178af5b0aae4569d768ebf603"
company_key: "yc-ultralight"
company: "Ultralight"
source_id: "yc-ultralight-news-import-72fedf71612e"
canonical_url: "https://ultralig.ht/blog/ultralight-1-4-out-now"
published_at: null
first_seen_at: "2026-07-22T17:49:47.142672+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:020b5e766ec32b1f1925b90ed7f6c32bd7849e0ff45ade42eade89ac823292a0"
---

# Ultralight 1.4 Out Now

Apr. 21st, 2025


# [Ultralight 1.4 Out Now](https://ultralig.ht/blog/ultralight-1-4-out-now)


###### Contents


- What is Ultralight?
- What’s New in Rendering


- Updated WebKit Core
- CSS Filter Effects
- External Image Sources
- Variable Font Files
- WebP & Animated WebP
- High‑Refresh Animation Loop


- Expanded Platform Coverage


- macOS Apple Silicon and Linux ARM64
- PlayStation and Xbox Support
- Nintendo Switch


- Security Hardening
- What’s Next?
- Get the New Release
- Full Changelog


- Major Improvements
- Major API Changes


- New Classes / Structs:
- New API Functions:
- Changes to Existing API:


- Major Bugfixes


Ultralight 1.4 is now available. This major release brings our core engine up to **WebKit 615.1.18.100.1** , adds **ARM64 support** , introduces powerful **new rendering and security features** , all while keeping its small footprint and easy-to-use C++ API intact.


---


## What is Ultralight?


Ultralight is a lightweight, ultra-portable fork of WebKit designed for embedding in games and native applications. It is low-latency (no multi-process), GPU-accelerated, supports modern web spec (up to ES2022), with availability on PC and consoles.


---


## What’s New in Rendering


### Updated WebKit Core


The biggest change is under the hood: Ultralight has merged in 3 years of updates and now tracks **WebKit 615.1.18.100.1** which is roughly equivalent to **Safari 16.4.1** .


That means new CSS properties (` :has()` ,` subgrid` ,` @container` ,` @layer` , etc.), broader ES2022 JavaScript features (top-level` await` , ES module support, etc.), native support for the` <dialog>` element, and much more.


### CSS Filter Effects


Ultralight also now supports CSS` filter` effects: eg` blur` ,` saturation` ,` hue‑rotate` , and more— allowing you to apply cinematic effects entirely in CSS.


We’ll be adding GPU acceleration and` backdrop-filter:` in the upcoming 1.4.1 release.


### External Image Sources


Games often need to blend 2D interface components with images generated elsewhere in your engine— a RTT minimap, animated character models, or even live video feeds.


The new **[ImageSource API](https://ultralig.ht/api/cpp/1_4_0/classultralight_1_1_image_source.html)** lets you hand Ultralight external GPU/CPU data and use it in an` <img>` or` background-image` , enabling a powerful set of new design possibilities.


### Variable Font Files


We now support **variable font files** (fonts that store multiple weights or styles in a single binary). You can ship one` .ttf` or` .woff2` instead of a family of separate files, and Ultralight will choose the correct one based on` font-weight` and` font-style` .


### WebP & Animated WebP


We’ve added support for the WebP image codec, trimming image payloads by up to 30% without sacrificing quality. We’ve also enabled Animated WebP so you can embed short video clips any place you’d normally use an image (think of it as an HD animated GIF).


### High‑Refresh Animation Loop


Animations, scroll, and` requestAnimationFrame()` are now driven by[Renderer::RefreshDisplay()](https://ultralig.ht/api/cpp/1_4_0/classultralight_1_1_renderer.html#a82549aaa0060e0844ecd650ebca81805) , enabling fluid performance on 120Hz+ monitors.


---


## Expanded Platform Coverage


### macOS Apple Silicon and Linux ARM64


Ultralight binaries are now available for **macOS Apple Silicon** and **Linux ARM64** , two of our most‑requested targets. This completes support for all major PC platforms and expands Ultralight into the embedded / low-power hardware space.


### PlayStation and Xbox Support


Ultralight now supports PlayStation and Xbox officially (we’ve had experimental support for a number of years). We’ve also validated Ultralight on current console hardware so that UI code can ship unchanged between PC and living‑room builds.


Our console ports were developed in collaboration with several of our AAA game studio partners and have been shipping in production for the past 3 years.


### Nintendo Switch


We’re also excited to announce we’re expanding support to Nintendo Switch! Please[contact sales](https://ultralight.typeform.com/contact-sales) to get early access to a build once it’s available.


---


## Security Hardening


Running a browser engine inside a game, kiosk, or embedded device requires firm security guarantees. Ultralight 1.4 introduces two new hooks to help:


**[NetworkListener API](https://ultralig.ht/api/cpp/1_4_0/classultralight_1_1_network_listener.html) — Ultralight Pro**
Attach a listener to any` View` and make synchronous decisions about outbound requests. Build a whitelist/blacklist, enforce additional SSL certificate pinning, or disable network traffic entirely for offline deployments.


**Custom Allocator Hook — Ultralight Enterprise**
Plug in your own` malloc` /` free` pair (or a slab allocator) and force all Ultralight memory into a dedicated heap. Fence it with guard pages, hook it into your leak detector, and capture peak usage in existing telemetry— all without patching the engine.


Both features are opt‑in and compile away if you don’t need them.


---


## What’s Next?


We’re already cooking up a new release (1.4.1) that offers some exciting new benefits including GPU-accelerated filter effects,` backdrop-filter` support, accelerated scroll and compositing, and more. Expect a developer build out soon.


We’ll also be announcing official support for Nintendo Switch and experimental support for mobile platforms (Android and iOS) a little later this year.


If there’s any other feature you need, feel free to drop a line in our friendly[Discord](https://chat.ultralig.ht/) chat.


---


## Get the New Release


Ready to get started? Simply download the SDK and follow the README:


- **[Download Ultralight 1.4 Now](https://ultralig.ht/download)**


Helpful support links:


- **[Porting Guide](https://docs.ultralig.ht/docs/porting-from-13-to-14)** : Learn how to migrate from 1.3 to 1.4.
- **[Explore the API](https://ultralig.ht/api/cpp/1_4_0/index.html)** : Explore the C++ API reference.
- **[Discord Chat](https://chat.ultralig.ht/)** : Come chat with us on Discord.


---


## Full Changelog


### Major Improvements


- Synchronize with upstream WebKit (Safari 16.4.1 / WebKit 615.1.18.100.1)
- Add support for Apple Silicon architecture on macOS
- Add support for ARM64 architecture on Linux
- Add support for WebP and animated WebP videos
- Add support for compositing external textures / images (new` ImageSource` API)
- Add support for CSS filter effects (blurs, saturation, sepia, etc.) (note: not yet natively GPU accelerated, incoming in 1.4.1)
- Add support for file downloads (see` View::set_download_listener` )
- Add support network whitelist / blacklist (see` View::set_network_listener` )
- Add support for variable fonts
- Animation and smooth scroll is now driven by` Renderer::RefreshDisplay`
- Improve interaction and rendering of dropdown menus
- Improve support for SVG / Canvas path rendering
- Improve support for SVG / CSS clips and masks
- Improve performance of Windows port (now built via Clang)
- Improve performance of AppCore (now supports high refresh-rate monitors)
- Improve Chinese-Japanese-Korean text rendering on Linux (AppCore)
- Improve scroll animation performance
- Improve performance of the CPU renderer


### Major API Changes


#### New Classes / Structs:


- ` ImageSource`
- ` ImageSourceProvider`
- ` NetworkListener`
- ` NetworkRequest`
- ` DownloadListener`
- ` ConsoleMessage`
- ` ThreadFactory`


#### New API Functions:


- ` Renderer::RefreshDisplay()` (must be called to update animations and scroll)
- ` View::set_display_id()`
- ` View::display_id()`
- ` View::set_network_listener()`
- ` View::set_download_listener()`
- ` View::CancelDownload()`
- ` Platform::set_thread_factory()`
- ` String::Hash()`
- ` String` (various comparison operators)
- ` Monitor::display_id()`
- ` Monitor::refresh_rate()`
- ` Window::EnableFrameStatistics()`
- ` ShowMessageBox()`


#### Changes to Existing API:


- ` ViewListener::OnAddConsoleMessage()` has different parameters
- ` ViewConfig` struct has a new field “display_id”
- ` MessageSource` enum values have changed


### Major Bugfixes


- Fix issue where` Config::user_stylesheet` would not be applied if HTML doctype was not set
- Fix issue with alpha blending and opacity in the CPU renderer
- Fix issue tessellating stroked paths with round and square line-caps in the GPU renderer
- Fix issue drawing radial gradients in the GPU renderer
- Fix issue where CSS dashed borders would only be applied to an element when the border radius was rounded
- Fix issue where files would not be displayed consistently across platforms due to inconsistent mime-type mappings
- Fix issue validating SSL certificates on Linux and macOS
- Fix issue where cookies weren’t applied to subdomains
- Fix issue where hard reloads weren’t sending no-cache headers
- Fix issue drawing CSS box shadows outside of the initial View rectangle
- Fix issue with flickering artifacts on macOS/Metal when a` Window` has multiple` View` overlays assigned to it
- Fix issue compiling on older GCC toolchains
- Fix crash when rendering certain paths
- Fix crash in C API due to improper mappings of` ULMessageSource` and` ULMessageLevel` enums
- Fix crash when system page size is less than 16384 bytes
- Fix crash when switching networks on macOS
- Fix crash when a remote inspector backend was closed
- Fix crash when a` View` was destroyed with dropdown menu open
- Fix crash when loading Data URIs and certain resource types
- Fix memory leaks in the CPU renderer
- Fix memory leak in JavaScriptCore
- Fix various crashes at shutdown
- Fix high CPU usage when a WebSocket connection was active
- Mitigate memory growth when drawing many Canvas elements


#### [Back to Blog Home](https://ultralig.ht/blog)
