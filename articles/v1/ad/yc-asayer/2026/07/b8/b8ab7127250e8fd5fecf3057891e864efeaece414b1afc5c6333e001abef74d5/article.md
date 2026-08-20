---
schema_version: "1.0.0"
document_id: "b8ab7127250e8fd5fecf3057891e864efeaece414b1afc5c6333e001abef74d5"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/jpeg-xl-vs-avif/"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-25T17:38:53.548823+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:989f459a9cd5fb1f80ba172631210cbac184da7b89a498cbbb062611ac69d650"
---

# JPEG XL vs AVIF: Which Format Should You Ship?

For production web delivery in 2026, ship AVIF: it has roughly[93% global browser coverage](https://caniuse.com/avif) as of mid-2026 and mature CDN, CMS, and build-tool support. JPEG XL is technically the stronger format — faster encoding, decisively better lossless, native progressive decoding — but Chrome still ships it behind a flag, leaving JXL with roughly 15% coverage, all via Safari, which[caniuse](https://caniuse.com/jpegxl) classifies as *partial* support. That single fact decides the question for any public-facing site: until Chrome enables JXL by default, a JXL-first delivery stack serves AVIF or JPEG to the large majority of your users anyway.


This article resolves the mid-decision case directly: you already serve WebP or AVIF, you know JPEG’s limits, and you want a defensible call grounded in current browser reality rather than format evangelism. It goes criterion by criterion — compression, browser support, encode cost, progressive rendering — gives you a correct` <picture>` snippet, and tells you the specific signal to watch for that flips the recommendation.


## Key Takeaways


- For public-web production in 2026, AVIF is the format to ship:[~93% global coverage](https://caniuse.com/avif) versus[~15% Safari-only partial coverage](https://caniuse.com/jpegxl) for JPEG XL.
- A JXL-first` <picture>` element today serves AVIF or JPEG to roughly 85% of users, so you encode and store three format variants to deliver JXL to about one in seven visitors — a pipeline cost that only pays off once Chrome enables JXL by default.
- AVIF wins lossy photographic compression at low-to-mid quality; JPEG XL wins at high quality, on non-photographic/flat-graphic content, and decisively on lossless, where AVIF barely beats PNG.
- JPEG XL’s lossless JPEG recompression is unique among modern formats — transcode an existing JPEG to JXL (~20% smaller) and reconstruct the original byte-for-byte — making it the format to reach for when archiving large JPEG libraries.
- The trigger to make JXL your primary format is Chrome shipping` chrome://flags/#enable-jxl-image-format` as default-on; watch the[Chrome Platform Status](https://chromestatus.com/feature/5114042131808256) entry for that change.


## What Each Format Actually Is


AVIF and JPEG XL solve the same problem — smaller images at equal or better quality — but they come from opposite design lineages. AVIF (AV1 Image File Format) was introduced by the[Alliance for Open Media](https://aomedia.org/) in 2019 and derives from the keyframe intra-coding of the AV1 video codec used by services like YouTube and Netflix. JPEG XL (ISO/IEC 18181) was purpose-built as a still-image format by the Joint Photographic Experts Group together with Google and Cloudinary, finalized as a standard in 2022.


That lineage difference — AVIF derived from a video codec, JPEG XL purpose-built for still images — explains most of the tradeoffs below. AVIF inherits AV1’s strong lossy photographic compression and its CPU-heavy encoder. JPEG XL was engineered around image-specific concerns: progressive decoding, high-bit-depth color, lossless modes, and one feature no other modern format offers — lossless JPEG recompression. An existing JPEG can be transcoded to JXL, reducing file size by[roughly 20%](https://github.com/libjxl/libjxl) , and decoded back to the original JPEG byte-for-byte. That makes JXL the only viable format for archiving large JPEG libraries without quality loss — a genuine decision criterion, not a footnote.


## Compression: Who Wins Depends on the Content


AVIF wins lossy photographic compression at low-to-mid quality; JPEG XL wins at high quality, on non-photographic or flat-graphic content, and decisively on lossless. There is no single “X is N% smaller” answer, and any article that gives you one is overstating a content-dependent result.


The practical breakdown:


- **Lossy photographic, mid quality:** AVIF’s loss is softer and more natural at aggressive bitrates — it blurs edges and hides artifacts where JPEG XL’s VarDCT mode can produce ringing or blocking similar to legacy JPEG. This is AVIF’s home turf and the most common web case.
- **High quality and non-photographic:** JPEG XL matches or surpasses AVIF, particularly on illustrations, logos, screenshots, and text-heavy graphics, where its modular encoding mode handles flat color regions cleanly.
- **Lossless:** JPEG XL is decisively better. Lossless AVIF is impractical — it barely improves on PNG — while lossless JXL is competitive with PNG and frequently much smaller.


Compression ratios vary significantly by image content and quality setting; the JXL-versus-AVIF edge in particular is content-dependent and not a settled number. Before committing a content library to a format, test your specific assets in[Squoosh](https://squoosh.app/) , which encodes both formats in-browser and shows the size/quality tradeoff for your images rather than someone else’s benchmark set.


## Browser Support: The Decisive Axis


Browser support is where this decision is actually made, and it is the axis most reference articles get wrong or report stale. As of mid-2026, AVIF has roughly 93% global coverage and is[default-on in Chrome, Edge, Firefox, and Safari](https://caniuse.com/avif) . JPEG XL sits at roughly 15% coverage — all of it via Safari, and classified as *partial* .


The Chrome history matters because two of the three most-cited articles report it incorrectly. The accurate timeline:


Date Event Source


2021 (Chrome 91) JXL added behind a flag[Chrome Platform Status](https://chromestatus.com/feature/5114042131808256)


Feb 2023 (Chrome 110) JXL decoding removed[caniuse.com/jpegxl](https://caniuse.com/jpegxl)


Sept 2023 (Safari 17) JXL shipped default-on (partial), macOS Sonoma / iOS 17[WebKit release notes](https://webkit.org/blog/14445/webkit-features-in-safari-17-0/)


Dec 2025 – Jan 2026 New Rust decoder (jxl-rs) merged into Chromium[github.com/libjxl/jxl-rs](https://github.com/libjxl/jxl-rs)


Feb 2026 (Chrome 145) JXL decoding shipped behind a flag, not default Chrome Platform Status


The widely repeated claim that Chrome “discontinued” JPEG XL is now out of date. Chromium rescinded the format’s obsolete designation and reintegrated decoding using a new Rust-based decoder rather than the C++` libjxl` . But reintegration is not the same as shipping: through the current stable channel, JXL decoding in Chrome remains disabled by default and requires manual activation via` chrome://flags/#enable-jxl-image-format` . Firefox carries JXL code in Nightly behind the` image.jxl.enabled` preference but does not compile it into release builds and has published no shipping timeline.


The translation into a shipping decision is the part every reference implies but none states crisply: a JXL-first` <picture>` element today serves AVIF or JPEG to roughly 85% of your users. You encode and store three format variants to deliver JXL to about one in seven visitors — and even that reach is Safari-only partial support. That is the actual cost of going JXL-first now, and it only pays off once Chrome enables the format by default.


## Encode Cost and Progressive Rendering


JPEG XL encodes faster than AVIF and uniquely supports progressive decoding; AVIF encoding is CPU-heavy and has no true progressive mode. Both differences are operationally relevant — one in your build pipeline, one in your users’ perceived load time.


### AVIF encoding is the slow step in most pipelines


AVIF encoding with the AV1 reference encoder (` libaom-av1` ) is the slowest stage in a typical image pipeline, materially slower per image than JPEG XL at equivalent perceptual quality. For a single hero image converted by hand this is negligible. For a CI job that processes hundreds of product images on every deploy, it is a real constraint: encode time scales with image count and the AV1 encoder is computationally expensive by design.


Practical mitigations for a Node.js pipeline:


- Use[Sharp](https://sharp.pixelplumbing.com/) , which wraps` libvips` , for AVIF and JXL encoding. It exposes` quality` and` effort` controls that trade encode time against output size.
- Tune the effort/speed setting rather than running the encoder at maximum effort by default — high-effort AVIF encoding produces marginally smaller files for a large time cost.
- Parallelize across worker threads or CPU cores, or push conversion to a CDN that encodes on-the-fly so it never blocks your build.


```text
const   sharp   =   require  (  "  sharp  "  );


// AVIF: lower `effort` trades a little size for much faster CI encoding
await   sharp  (  "  hero.png  "  )
.  avif  ({   quality  :   60  ,   effort  :   4   })
.  toFile  (  "  hero.avif  "  );


// JXL encodes faster at comparable quality (requires a libjxl-enabled build)
await   sharp  (  "  hero.png  "  )
.  jxl  ({   quality  :   60   })
.  toFile  (  "  hero.jxl  "  );
```


Check your installed` sharp` build for format availability with` sharp.format` — JXL support depends on the bundled` libvips` /` libjxl` and is not guaranteed in every distribution.


### Progressive decoding affects perceived LCP


JPEG XL supports native progressive decoding: the browser renders a low-quality version of the image immediately and sharpens it as data arrives. AVIF has no true progressive mode, so a large AVIF image either paints fully or not at all. On slow connections this surfaces as visible Largest Contentful Paint delay — the hero image slot stays blank until the complete file arrives.


This is a class of problem we see directly in session replays: on image-heavy pages, a hero-image slot stays blank for the full duration of a large image download on a constrained connection, with the LCP timestamp pinned to the moment the complete file finally paints. JXL-style progressive decoding addresses exactly that failure mode — a low-quality image appearing early and sharpening — which is the perceptual-performance behavior that matters for real users on slow networks, and the reason payload size and progressive rendering are LCP levers, not cosmetic preferences.


## The Decision Framework: Ship X If…


Ship AVIF now for public-web production, with WebP and JPEG fallbacks via` <picture>` . Reach for JPEG XL in specific, bounded cases: lossless or archival workflows, non-photographic image libraries, Safari-concentrated audiences, or large JPEG archives you want to recompress without quality loss.


**Ship AVIF if:**


- You run a public-facing production site that needs broad coverage today.
- Your assets are mostly photographic or mixed.
- You use a CMS or CDN with native support — AVIF has been native in WordPress since[WP 6.5 (April 2024)](https://make.wordpress.org/core/2024/02/23/wordpress-6-5-adds-avif-support/) , and major CDNs encode it on-the-fly.
- You want measurable LCP gains over JPEG/WebP without managing a fourth format variant.


**Reach for JPEG XL if:**


- You maintain a large JPEG archive and want lossless recompression — transcode to JXL (~20% smaller) and reconstruct originals byte-for-byte.
- Your content is predominantly non-photographic: illustrations, logos, diagrams, screenshots.
- Your audience is Safari-heavy and you control the full` <picture>` markup in a headless or custom setup.
- You are building forward-looking delivery for the moment Chrome default-enables JXL.


JPEG XL has no native WordPress upload support as of mid-2026, which reinforces AVIF as the practical CMS choice.


### A correct, current` <picture>` snippet


In a[<picture> element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture) , the browser evaluates` <source>` entries in document order and selects the first` type` it supports, falling back to the` <img>` if none match. Ordering is therefore not cosmetic — it is the selection algorithm. List formats best-to-worst by your priority, and make the` <img>` fallback a universally supported JPEG, not WebP (WebP needs its own` <source>` entry because it is a distinct MIME type).


```text
<  picture  >
<!-- JXL first: served only to Safari 17+ (partial); Chrome flagged-off in mid-2026 -->
<  source   srcset  =  "  hero.jxl  "   type  =  "  image/jxl  "  >
<!-- AVIF: ~93% coverage, the workhorse tier for nearly all traffic -->
<  source   srcset  =  "  hero.avif  "   type  =  "  image/avif  "  >
<!-- WebP: covers older Chrome/Firefox that predate AVIF -->
<  source   srcset  =  "  hero.webp  "   type  =  "  image/webp  "  >
<!-- JPEG: unconditional fallback; the src that always renders -->
<  img   src  =  "  hero.jpg  "   alt  =  "  Description  "   width  =  "  1200  "   height  =  "  630  "
loading  =  "  lazy  "   decoding  =  "  async  "  >
</  picture  >
```


The honest read on this four-tier stack: today it costs you four encoded variants to hand JXL to a Safari-only minority. If your audience is not Safari-heavy and you have no archival reason for JXL, drop the first` <source>` and ship the three-tier AVIF/WebP/JPEG version — it covers nearly all traffic with one less format to build and store. Add the JXL source when, not before, Chrome flips the flag.


## What to Watch for the Recommendation to Change


The practical trigger to make JPEG XL your primary delivery format is Chrome shipping` chrome://flags/#enable-jxl-image-format` as default-on; until that happens, the coverage math — roughly 15% JXL (Safari-only partial) versus ~93% AVIF — keeps AVIF the only defensible primary format for public-web production. Concrete signals worth monitoring:


- The[Chrome Platform Status](https://chromestatus.com/feature/5114042131808256) entry for JPEG XL changing from flagged to shipped/default-on.
- [caniuse.com/jpegxl](https://caniuse.com/jpegxl) full-support coverage crossing a usable threshold and losing its *partial* designation.
- Firefox moving JXL from Nightly into a release build.
- Major CDNs enabling JXL conversion by default in their image pipelines.


When the first of those lands, a JXL-first` <picture>` stack stops being a speculative cost and becomes the default recommendation, and the framework above inverts.


## Conclusion


Ship AVIF today and keep your` <picture>` markup ready for JPEG XL. AVIF is the format that reaches your users now, with the tooling and CMS support to back it; JPEG XL is the better format on the technical merits and the right tool for lossless archives and JPEG recompression, but its public-web case is gated entirely on Chrome enabling it by default. Convert your assets to AVIF, add WebP and JPEG fallbacks, and set a watch on the Chrome Platform Status entry — the day that flag flips on by default is the day to revisit JXL as your lead source.


## FAQs


Does serving AVIF and a JXL source in the same picture element double my storage and bandwidth?


It increases storage but not per-request bandwidth. Each format variant you list adds an encoded file to store, so a four-tier JXL, AVIF, WebP, JPEG stack means four stored files per image. Bandwidth is unaffected because the browser downloads only the first supported source it selects, never multiple variants. The real cost of a JXL-first stack is the extra encode and storage to reach a Safari-only minority, not duplicated downloads.


Can I convert my AVIF images to JPEG XL later without re-encoding from the original?


No, not without quality loss. JPEG XL's lossless transcoding is unique to JPEG sources, not AVIF or WebP. You can wrap an existing JPEG into JXL and reconstruct it byte-for-byte, but AVIF and WebP are lossy formats with no such reversible path. Converting AVIF to JXL means decoding the already-compressed AVIF and re-encoding it, compounding artifacts. Always keep your original lossless masters so you can re-encode to any format cleanly.


Why does my AVIF hero image still hurt Largest Contentful Paint even though the file is smaller than the JPEG?


AVIF has no true progressive decoding, so a large AVIF image paints only once the full file arrives rather than rendering a low-quality preview first. On a slow connection the hero slot stays blank for the entire download, and the LCP timestamp is pinned to the final paint. A smaller total file size does not change this all-or-nothing behavior. JPEG XL's progressive decoding sharpens incrementally, which improves perceived load even at similar byte counts.


Will a browser that supports both AVIF and JPEG XL pick whichever source I list first?


Yes. The picture element selects the first source whose type attribute the browser supports, evaluated in document order, regardless of file size or format capability. If you list JXL before AVIF, a Safari build supporting both serves JXL; reverse the order and it serves AVIF. The browser does not compare quality or weight, so source order is the selection mechanism. Place your preferred format first and end with a universally supported JPEG img fallback.


Digital experience platform


## Truly understand users experience


See every user interaction, feel every frustration and track all hesitations with **OpenReplay** — the open-source digital experience platform. It can be self-hosted in minutes, giving you complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
