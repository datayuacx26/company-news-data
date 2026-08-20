---
schema_version: "1.0.0"
document_id: "9d2a79a7d8af4bf0a1cdec561a9c478904252bd0ecee51101b8c842a7694cb4e"
company_key: "yc-ultralight"
company: "Ultralight"
source_id: "yc-ultralight-news-import-72fedf71612e"
canonical_url: "https://ultralig.ht/blog/ultralight-1-3-now-available"
published_at: null
first_seen_at: "2026-07-22T17:49:47.142672+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:b1664dc8b7bac23b948fc792ca6be66ec3aed0c1f713d29cf06d8e619f01febe"
---

# Ultralight 1.3 Now Available

Jul. 24th, 2023


# [Ultralight 1.3 Now Available](https://ultralig.ht/blog/ultralight-1-3-now-available)


###### Contents


- Download the SDK
- What’s New
- Upcoming Changes (1.4)
- Porting from 1.2
- Full Changelog


- Major Improvements
- Major API Changes
- Major Bugfixes


We are pleased to announce the stable release of **Ultralight 1.3** !


## Download the SDK


You can download the free SDK[here](https://ultralig.ht/download) ;


## What’s New


This major new release improves rendering accuracy, offers new debugging tools, and add experimental support for Xbox, PS4, and PS5.


Improved CPU Rendering The CPU renderer has been rewritten to be faster and more accurate (new parallel engine built on Skia internals). Enhanced Text Rendering Refined text rendering for clearer and sharper display of text. Improved pixel-snapping and font scaling at various DPI scales. Better HTML/CSS Support Extended support for modern HTML and CSS standards, specifically` box-shadow` ,` linear-gradient` ,` radial-gradient` , dashed strokes, and more. Reduced Memory Usage Optimized to consume less memory, making it more suitable for use in resource-constrained environments. Remote Inspection Capabilities New tools for inspecting Views over the network, perfect for debugging web content on game consoles or embedded devices. Multi-Window Support The AppCore framework now offers support for creating and managing multiple windows on all major desktop platforms. Gamepad Input Support HTML5 gamepad input is now natively supported, offering new interaction modalities on game consoles and PC. Experimental Video/Audio Support Introduces experimental support for HTML5 video/audio, (Windows-only, VP9 and h264 codecs supported). Experimental Game Console Support We now offer experimental support for Xbox, PS4, and PS5 platforms, with more platforms on the way!


## Upcoming Changes (1.4)


We’re already hard at work on the next version (1.4), major changes coming include:


- ARM64 and Apple Silicon Support
- Latest WebKit Trunk Changes
- Clean Restart/Shutdown and Memory Reclamation (recreate the Renderer multiple times in a single process)
- Improved CSS and SVG Filter Support
- Multi-Monitor Support
- New Memory and Thread Management API
- New Network / Resource Request API
- Display Link API


## Porting from 1.2


Make sure to check out our[porting guide](https://docs.ultralig.ht/docs/porting-from-12-to-13) for helpful hints on migrating from 1.2 to 1.3.


## Full Changelog


### Major Improvements


- Rewrite CPU renderer to be faster and more accurate (parallel engine built on Skia internals).
- Add support for Xbox, PS4, and PS5 platforms.
- Improve text rendering accuracy with more intelligent snapping and hinting.
- Improve font scaling accuracy at various device scales.
- Improve` box-shadow` rendering accuracy and performance.
- Improve performance of layout and JavaScript execution using mimalloc.
- Improve performance of JavaScript garbage-collector, reduce per-frame stalls.
- Improve consistency of CPU and GPU renderers (both now blend in sRGB).
- Add memory profiler and memory statistics tracking.
- Add support for gamepad input devices.
- Add support for remote web inspector.
- Add support for multiple windows to AppCore.
- Add support for rendering only a subset of Views.
- Add support for dashed strokes to CPU renderer.
- Add support for SVG` onclick` and other events that require path picking.
- Add support for HTML5 Video / Audio (experimental) via GStreamer/FFmpeg (disabled by default).
- Update` user-agent` string to reflect proper Safari / WebKit version.
- Update to latest CA certificate chain.
- Unify build scripts and toolchain.


### Major API Changes


- Unify API to use` RefPtr<>` everywhere (instead of` Ref<>` class)
- Unify API to use` String` everywhere (instead of` String16` ,` String8` , etc.).
- Make` String` class use` String8` natively (utf-8 is now native representation instead of utf-16).
- Make all enums use` enum class`
- Resources now load via` FileSystem` API instead of` fopen()` .
- ` FileSystem` now required to be defined in` Platform` API before creating` Renderer` .
- ` Buffer` class now accomodates memory-mapped files and destruction callbacks.
- ` FileSystem` interface has been reorganized to use new` Buffer` API.
- Portions of` Config` have been moved to` ViewConfig` (now certain options can be set per-View).
- ` App::Quit()` must now be called manually when using AppCore API.
- Add` Bitmap::LockPixelsSafe()` and` LockedPixels<>` utility class to manage lifetimes.
- Add` WebKitVersionString()` to query corresponding WebKit version.
- Extend AppCore API to enable multi-window support.
- Re-organize C API into logical header files.
- Add` Renderer::StartRemoteInspectorServer`
- Add` Renderer::SetGamepadDetails`
- Add` Renderer::FireGamepadEvent`
- Add` Renderer::FireGamepadAxisEvent`
- Add` Renderer::FireGamepadButtonEvent`
- Add` Renderer::RenderOnly`
- Modify` Renderer::CreateView` signature (options moved to` ViewConfig` param)
- Add` View::device_scale`
- Add` View::set_device_scale`
- Add` View::is_accelerated`
- Add` View::is_transparent`
- Add` View::JavaScriptVM`
- Rename` View::inspector` to` View::CreateLocalInspectorView`
- Modify` View::LockJSContext` return value (now returns` RefPtr<>` to manage lifetime)
- Add` ViewListener::OnCreateInspectorView`
- Add` ViewListener::OnRequestClose`
- Add` WindowListener::OnKeyEvent`
- Add` WindowListener::OnMouseEvent`
- Add` WindowListener::OnScrollEvent`


### Major Bugfixes


- Fix issue managing clip in CPU and GPU renderers.
- Fix issue blending alpha in CPU renderer.
- Fix issue stroking paths in CPU renderer.
- Fix issue drawing glyphs with complex transformations.
- Fix issue drawing radial gradients with CPU renderer.
- Fix issue drawing gradients with more than 12 stops.
- Fix issue running library off the main thread on macOS.
- Fix issue rendering paths that begin with an ArcTo command.
- Fix issue where BGRA byte order was not used on all platforms.
- Fix issue painting dropdown menus.
- Fix issue calculating CSS button height.
- Fix issue displaying box-shadows on native inputs.
- Fix issue displaying context-menu in web inspector.
- Fix issue writing LocalStorage database on Windows.
- Fix issue with` KeyboardEvent.key` in JavaScript always returning null.
- Fix issue with` LoadListener::OnBeginLoading` not passing correct URL.
- Fix issue with` LoadListener::OnFailLoading` not being fired on non-200 HTTP status codes.
- Fix issue resizing windows with GPU acceleration enabled (AppCore).
- Fix issue calling` Window::SetTitle` with Unicode strings (AppCore).
- Fix issue with anti-aliasing when GPU acceleration is enabled on Linux (AppCore).
- Fix issue with automatic DPI switching on Windows (AppCore).
- Fix security issue allocating very large render layers.
- Fix major performance bug in cURL network code.
- Fix memory leak loading certain` file:///` URLs.
- Fix memory leak when constructing` JSString` from` JSStringRef` (AppCore).
- Fix various crashes and buffer overruns in Bitmap class.
- Fix crash loading certain Data URLs.
- Fix crash when` Config::bitmap_alignment` is 0.
- Fix crash when encountering certain text-encodings.
- Fix crash when encountering certain JavaScript code.
- Fix crash in SQLite database code.
- Fix crash at process shutdown.


#### [Back to Blog Home](https://ultralig.ht/blog)
