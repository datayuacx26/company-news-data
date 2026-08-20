---
schema_version: "1.0.0"
document_id: "7988341f6ce8ffbb4f6ff9e05bb82dc16e86af836d97b500ac8ede14423f195d"
company_key: "yc-ai-tech-packs"
company: "AI Tech Packs"
source_id: "yc-ai-tech-packs-news-import-be47509359c4"
canonical_url: "https://aitechpacks.com/blog/ai-tech-packs-updates-and-dev-journal"
published_at: "2026-07-08T22:31:45.314+00:00"
first_seen_at: "2026-07-21T05:35:53.930845+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:9b1f4e47020872460bf67e695ce2abc30262a423ff9e421fa84746b42b7fb3ef"
---

# AI Tech Packs Changelog — Software Updates & New Features

## What This Changelog Covers


AI Tech Packs is web-based tech pack software for fashion designers, apparel teams, and manufacturers. This changelog tracks updates across tech pack generation, flat sketch creation, BOM workflows, translation, Excel exports, and factory collaboration tools.


If you're evaluating the product, this page shows what shipped, what workflows changed, and how frequently the software is improving.


## Core Workflows Covered


- [AI tech pack generation](https://aitechpacks.com/features/techpack-generation) Build factory-ready tech packs from garment images and prompts.
- [Flat sketch generation](https://aitechpacks.com/flatsketchgenerator) Create reusable flat sketches for early design development and production packs.
- [Tech pack translation](https://aitechpacks.com/translate-techpack) Translate Excel-based tech packs for cross-border factory communication.
- [Excel export workflows](https://aitechpacks.com/features/excel-export) Export spreadsheets that factories can review, revise, and use in production.
- [Factory share links](https://aitechpacks.com/features/factory-sharing) Share live tech pack views, PDFs, and supporting context with production partners.
- [Techpack Mama](https://aitechpacks.com/features/techpack-mama) Help factories review specs, ask questions, and understand changes faster.


We’re building AI tools that make tech packs faster, clearer, and more collaborative.


This dev journal retraces our journey from conception to now!


You can see what changed, why it matters, and what's coming next.
(Current as of July 10, 2026.)


## July 10, 2026 - Embellishment Notes Beta


Embellishment Notes is now available as an opt-in beta when you create a new V4 tech pack. It adds dedicated front and back pages for marking where logos, embroidery, prints, patches, and other artwork should sit on the garment.


Each artwork placement can include editable measurement lines and written production guidance. You can adjust lines directly on the garment image, focus on one placement at a time, search your notes, and hide or restore details before export.


Your changes use the same Save and Version History workflow as the rest of the tech pack, keeping placement guidance together with the specification your team shares with a factory.


- Dedicated front and back artwork-placement pages when notes are generated.
- Editable dimension lines, placement names, and production descriptions.
- Focus, search, hide, and restore controls for cleaner review and handoff.


To try it, turn on Embellishment Notes (Beta) before generating a new tech pack. The extra analysis currently adds about seven minutes, so the option remains off by default while we continue improving speed and results.


## July 10, 2026 - Asset Library Production Push


Asset Library gives teams a dedicated workspace for reusable Images, Flat sketches, and Size charts, so trusted production assets are easier to find and reuse across new tech packs.


The workflow is built around copied snapshots: when a saved asset is applied to a tech pack, that tech pack keeps the exact version used at that moment, so later library updates do not silently change older work.


We are rolling this out carefully across save, apply, and usage-visibility flows so teams can reuse assets with confidence before we mark the release fully complete.


- Images: save and reuse uploaded or generated visual references.
- Flat sketches: browse existing saved sketches and bring them into new tech packs.
- Size charts: save measurement structures with garment preview context for easier reuse.


## July 8, 2026 - V4 Editor Upgrade + Reference Image Guidance


Released a large V4 editor update focused on making reference-image based tech packs easier to review, edit, and export.


- Artwork and BOM items now work more cleanly together, with clearer preview images and stronger naming/category handling.
- Added a construction guide workflow for garment and embellishment details, giving teams more production context inside the tech pack.
- Improved PDF and page navigation around artwork, placement, measurements, and construction-related pages.
- Improved returning to guest-created tech packs, SVG downloads, and generated tech pack stability.


V4 currently works best when each tech pack is created from a reference image focused on one garment type.


If you are building a set, such as a shirt and pants, please separate the reference into two images and create two tech packs: one for the top and one for the bottom.


Official set support is in progress, so future versions will handle multi-garment reference images more smoothly.


- Full outfit reference photo: not recommended in V4
- Cropped top or shirt image: recommended
- Cropped bottom or pant image: recommended


## July 2, 2026 - Measurement Editing + PDF Navigation Improvements


Improved measurement editing, unit handling, and PDF navigation across the tech pack editor.


- Made centimeter/inch conversion more consistent across measurements, PDFs, exports, and assistant-assisted edits.
- Improved table-of-contents and page-sync behavior in the editor and factory view.
- Added clearer validation around measurement edits so changes are easier to review before applying.


## June 26, 2026 - Tech Pack Cost Calculator + Resource Updates


Released new planning resources for teams comparing tech pack workflows and production costs.


- Added the Tech Pack Cost Calculator tool.
- Added author pages and stronger article metadata for clearer resource attribution.
- Improved blog listing behavior and added more FAQ/resource content.


## June 23, 2026 - Flat Sketch to Tech Pack Generation Flow Updates


Improved generation flows for users starting from saved flat sketches, garment images, or mixed reference inputs.


- Improved persistence for pending generation drafts and smoother recovery if a generation is interrupted.
- Improved placement guide and PDF section behavior during generation.
- Added more stability around flat sketch and garment-image based generation paths.


## June 21, 2026 - Saved Tech Pack Version History


Added version history for saved tech packs.


- Teams can preview older versions of a saved tech pack.
- Restoring an older version creates a new latest version while preserving the previous history.
- Added safeguards for stale saves and read-only version previews.


## June 19, 2026 - Brand Kit Defaults + Collection Handoffs


Improved setup defaults and collaboration workflows for teams using Brand Kits and collections.


- Brand Kits can store generation defaults such as sizing, colorways, order information, and contact details.
- Single and batch tech pack generation now use saved brand defaults more consistently.
- Added a collection handoff workflow for sharing production context more clearly.


## June 17, 2026 - Techpack Mama + Illustrator Workflow Improvements


Updated Techpack Mama and Illustrator-connected workflows to make editing and handoff smoother.


- Improved Techpack Mama context and edit support across more sections of the tech pack.
- Improved Illustrator connection reliability.
- Made order sheet color names editable.
- Improved artwork download and review handoff paths.


## June 12, 2026 - AI Tech Packs Agency Services


Launched AI Tech Packs Agency services for teams that want hands-on tech pack support.


- Added a dedicated agency services page and intake flow.
- Added project category scoping, file-upload support, and a clearer intake path for agency requests.
- Updated pricing and navigation touchpoints so agency services are easier to find.


## June 10, 2026 - Tech Pack Review Requests + Unit and Quantity Fixes


Improved review-request, team, and editing workflows around generated tech packs.


- Added support for manual tech pack review requests from the product flow.
- Improved the review request flow so requests stay connected to the correct saved tech pack.
- Fixed quantity-field editing and additional unit-conversion issues in generated PDF and measurement data.
- Expanded team-seat support for Pro accounts.


## **June 8, 2026 — MFA Support + Speed Improvement**


Released multi-factor authentication for all accounts.


Adds an extra layer of security at login. This has been a common request from enterprise and team customers going through IT review, and it's now live for everyone.


We also optimized our internal AI agent performance to increase generation speed. Average techpack generation had crept up to around 7 minutes as we focused on quality — normal generations now average 3 minutes again.


## **June 4, 2026 — New Measurement System + Global Size Chart Support + Kids Sizing**


Major upgrade to how sizing works at the start of generation.


- Users can now choose between numeric or alpha sizing before generating
- Added support for regional size charts including USA, UK, Asia, and more
- Upgraded the prompt system to handle any size chart format you throw at it
- Officially added support for baby, toddler, and children's sizing


This makes AI Tech Packs usable for brands producing across regions and categories — including kids wear, which was previously unofficially unsupported.


## May 27, 2026 - Flat Sketch Save + Measurement Canvas Stability


Improved flat sketch saving and measurement drawing reliability.


- Made flat sketch save/export behavior more reliable.
- Improved measurement drawing so canvas coordinates stay within bounds more consistently.
- Improved SVG download handling for flat sketch outputs.


## **May 21, 2026 — AI Tech Packs Plugin Live on Adobe Exchange**


The AI Tech Packs plugin is now officially available on the Adobe Creative Cloud Exchange.


Install it directly from the marketplace and connect your Illustrator workflow to AI Tech Packs.


(Adobe Exchange listing:[https://exchange.adobe.com/apps/cc/205198/ai-tech-packs](https://exchange.adobe.com/apps/cc/205198/ai-tech-packs) )


## **May 4, 2026 — Color Sheet Page Deprecated**


Deprecated and removed the Color Sheet page from the techpack flow. The Order Sheet now takes priority as the source of truth for color and order information.


This simplifies the techpack structure by eliminating a redundant page and reduces confusion for users who were splitting color information across two places.


## **April 30, 2026 — Style Block System Update + Centralized Brand Library + Team & Seats**


Major update to how Style Blocks work and how brand-level assets are accessed across the app.


- Updated the Style Block system with improved UI/UX for accessing and managing saved Style Blocks
- Centralized the **Brand Library** as the single hub for: saved Style Blocks, Brand Kit, and personal contact info
- Launched a new **Team & Seats page** for Studio and Enterprise customers — managers can now invite teammates and manage seat assignments directly


This is the foundation for true team workflows on AI Tech Packs and unblocks the multi-seat sales motion for Studio and Enterprise plans.


## **April 27, 2026 — Google Auth, Free Trial Enforcement, BOM + Artwork Backend Refactor**


Three significant updates shipped together:


- Released **Google signup / login** for faster, friction-free account creation
- Enforced the new **free trial system**
- Refactored the backend system for **BOM generation and artwork extraction** — accuracy is dramatically improved


A real-world milestone from this update: the team ran a jacket with **37 unique patches** through the system and produced a full tech pack in **7 minutes** . Every patch was extracted, color coded, sized, and positioned — 37 of 37 isolated, with only one partial-view patch needing manual cleanup. Roughly 80% of the work auto-completed.


A year ago this was impossible. Now it runs in 7 minutes.


(LinkedIn post / sample PDF:[https://lnkd.in/gyFhRpKy](https://lnkd.in/gyFhRpKy) )


## **April 15, 2026 — Saved Techpack & Collection Management Fixes**


Fixed delete and rename functionality across the saved-content hierarchy:


- Saved tech packs
- Collection folders
- Styles


Brings management actions into parity with the new three-level organization (Collections → Styles → Tech Packs) introduced on April 9.


## April 14, 2026 — Flat Sketch Accuracy Improvements


We patched a number of issues from the last flat sketch update.


Flat sketch generation is now more accurate overall, which should lead to better outputs for users working on early design development. We also identified that part of the new error-checking system is causing some generations to fail more often than expected.


We’re actively refining that system so we can keep the quality improvements while reducing failed generations.


## April 10, 2026 — Faster PDF Updates, Better Flat Sketches, and Cleaner BOM Output


We upgraded how PDFs are served, making real-time updates smoother and fixing many of the loading issues users were experiencing.


We also moved our flat sketch system to a more agentic workflow to improve generation accuracy.


Other improvements from this update:


- Artwork reference images are no longer generated by default for every BOM item in free generations, reducing unnecessary output and cost
- PDF generations now use Pantone codes only, and no longer include hex codes


These changes help make generated tech packs cleaner, more consistent, and easier to work with.


## April 9, 2026 — New Share Links + Better Tech Pack Organization


We redesigned how factory sharing works and introduced a better way to organize tech packs.


Tech packs can now be organized across three levels:


- **Collections**
- **Styles**
- **Individual tech packs**


A collection can hold multiple styles, and each style can hold multiple tech packs.


You can now create share links at each level, making it much easier to send:


- a full collection
- a single style
- or an individual tech pack


We also simplified the factory share view so it’s easier for partners to use. It now focuses on two main components:


- a PDF viewer and downloader
- **Techpack Mama for Factories** , which helps explain the tech pack, guide factory review, and support questions


This update makes collaboration with factories much easier, especially when sharing multiple tech packs at once.


## April 6, 2026 — Flat Sketch Generator Relaunch


We relaunched the **Flat Sketch Generator Tool** , previously called the Mockup Generator.


The tool is now focused specifically on generating flat sketches. It also saves completed flat sketches to the **Flat Sketch Library** , so they can be reused later when building tech packs.


This makes it easier to create, save, and reuse design assets across your workflow.


## v3.45.2 — Translation Tool Agent Upgrade + Regeneration Bug Fixes + Mockup Tool Refactor (April 2, 2026)


Major upgrade to the **Translation Tech Pack Tool** , now rebuilt on an **agentic system** to significantly improve translation accuracy.


Fixed critical regeneration bugs affecting:


- Flat sketch image regeneration
- Placement guide image regeneration
- Artwork image regeneration


Also upgraded and refactored the **Mockup Tool** into a **Flat Sketch Generator Tool** , making it more aligned with the core tech pack workflow.


## v3.45.1 — Techpack Mama Agentic Rebuild (March 31, 2026)


Refactored **Techpack Mama** from the ground up on an **agentic system** .


This should make Techpack Mama:


- Faster
- More reliable
- Significantly better in output quality


This is a major backend architecture upgrade that improves how edits and generation tasks are handled across the system.


## v3.45.0 — Prompt Enhancement System + NLP Prompt Handling + Prompting Guide Release (March 20, 2026)


Upgraded the prompting system to automatically enhance weak or poorly written prompts before generation.


Also added stronger handling for **generic / NLP-style prompts** , making it easier for users to write naturally and still get strong outputs.


Released a new guide on prompt best practices:
[How to Write Prompts for AI Tech Packs](https://aitechpacks.com/blog/how-to-write-prompts-for-ai-tech-packs)


This helps both:


- Non-technical users write clearer prompts
- More advanced users understand how to structure prompts for better results


## v3.44.1 — Get Artwork Info + Artwork Extraction from Image + Label/Graphic Instructions (March 5, 2026)


- Major update that lets you extract artwork information directly from an image
- Supports label + graphic-specific instruction pickup (placement, text notes, print/style cues) within the image


Input image for artwork


then press this get artwork info button


Output is able to pick up written specifications of pantone code, size requirements etc .. if none are given, AI will determine best answers for you


## v3.44.0 — Prompt Update +Precise Conversions + Brand Kit Defaults (February 28, 2026)


- Major refactor of conversion system to be more precise
- Allow user to set up default sizes + units before generation, and save this to their Brand Kit


- Handle prompts like grade rule + sample size which we can extract an accurate and full size chart from, and upgrade prompt system to support numeric indexes of POMs
- Time of generation may have increased — use Batch Generation for extra speed


## v3.43.3 - Hide BOM Item + Excel Optimization (February 12,2026)


- Hide BOM lines in export, keep artwork visible. BOM and artwork can be linked or independent.


- Excel sheets updated with fitting format + other minor formating optimizations


## v3.43.2 - Measurement Line Generation Upgrade and Bulk Tech Pack Generation (February 10,2026)


- Massive upgrade to custom fine tuning models to allow AI Tech Pack's ai to generate measurement lines with greater accuracy. Look at these before and after pictures!


- Bulk techpack generation flow added. You can now generate up to 5 techpacks at a time if you are on the studio plan.


## v3.43.1 — Translation Tool (Early Beta) (January 29, 2026)


- Upload Excel sheet to Translation Tool and translate your techpack
- **Only Excel is supported**
- **Why Excel?** Excel is the main production format for techpacks, every brand’s business is unique and needs flexible, custom org formats


## v3.43.0 — BOM + POM Locking + Style Blocks (January 26, 2026)


- Added **BOM Locking** and **POM Locking** : tells the AI to stick strictly to the user’s text prompt and only use that info to generate the techpack
- BOM + POM are passed **in the order they’re read by the AI**
- When locking is enabled: **nothing is changed** in BOM/POM — **only missing values are filled**
- Recommendation: **lock POM** , **unlock BOM** (lets AI handle variable BOM items like artwork) but be guided by specific BOM call outs that you request
- Added **Style Blocks** : save prompts so you can reuse the BOM/POM you want to generate


## v3.42.8 — New Grading System + Removed POM Description + Summary Paragraph (January 21, 2026)


- Simplified grading system + implemented proper grading for full size chart
- New formula: **Size = Base Size + total sum of grade increments across sizes**
- Removed **POM Description** + **Product Summary Paragraph** (unnecessary + repetitive)


## v3.42.3 — Flat Sketch + Placement Guide Regeneration Upgrade (January 9, 2025)


- Major regeneration improvements for **Flat Sketch** + **Placement Guide** images
- Added **Image Source / Mode** visibility inside the **Manual Editor** so users can see the exact input source image used for regeneration


## v3.42.0 — SVG Upgrade + NEW BRAND KIT (January 6, 2025)


- Two SVG export types have been added:


- **Default SVG** (easy to colorize)
- **Line Vector SVG** (optimized linework for editing)


- **Brand Kit** : set **default order quantity** so new techpacks use updated order info, allow users to set different brand templates up for their techpacks so techpacks can quickly be assigned to different brands


## v3.41.2 — Search Bar for Techpacks (January 5, 2025)


- Added a search bar to quickly find techpacks


## v3.41.1 — Export Filename Improvements (PDF + Excel) (January 4, 2025)


- PDF + Excel exports now use readable filenames (not random IDs)


## V3.41 — Uncategorized Folder, Landing Page Redesign, Flat Sketch + Garment Extraction Upgrades (January 3,2026)


**Uncategorized Folder (auto-organize new tech packs)**


- Newly generated tech packs now default into an **Uncategorized** folder.
- Makes it easier to keep your workspace clean and quickly move packs into the right collections when you’re ready.


**Website-Wide UI Redesign**


- Rolled out a **major UI refresh across the entire site** .
- Cleaner layouts, tighter spacing, and a more consistent experience throughout the app.


**More Stable Flat Sketch Generation**


- Improved reliability and consistency when generating **flat sketches** .
- Fewer weird outputs and less need to re-run generations to get something usable.


**Garment Extraction Improvements**


- Upgraded the garment extraction pipeline for **better stability and cleaner results** .
- More dependable detection and extraction, especially on tougher reference images.


## V3.4 — Folders, Brand Management, Max Mode, and UX Patches (December 23,2025)


- Launched **Share to Factory** links. These links create a web portal you can send to manufacturers, always showing the latest version of your techpack. The portal includes a basic purchase order cover page, allows factories to comment directly on the techpack, and provides analytics on views and read time.
- Launched a **folder system and status tracking** for techpacks. You can now organize techpacks into collections and track where each style sits in the production pipeline.
- Launched **max mode** which allows images to be generated using Nano Banana Pro
- Added **version history** for AI-generated images, allowing you to revert to previous generations directly in the editor.
- Re-added missing **artwork color edits** that were lost during the V2 to V3 system upgrade.
- Prompt adjustment to initial flat sketch generations in an attempt to remove wrinkles
- Fixed a major **split-screen UI bug** .


## **V3.3 - Placement Guide & Artwork Regeneration (December 14, 2025)**


- You can now generate artwork without a reference image.
- Placement guide background images can now be regenerated using text prompts.
- Tech Pack Mama has expanded coverage, with stronger support for regenerating artwork images, placement guides, and improved stability when updating color sheet images.


## **V3.2 - Measurement Line Hide/Show Feature, Placement Guide Vision Upgrade, PDF Format Patch (December 11,2025)**


- You can now hide individual measurement lines when they aren’t necessary to show.
- On first AI generation, measurement lines that are detected as poorly drawn will be hidden automatically, making them easier to fix.
- Placement guide generation has been improved, with more accurate callout positioning and cleaner layouts.
- A minor PDF formatting issue has also been fixed.


## **V3.12 - Techpack Mama Upgrade (strong edits)(December 10, 2025)**


- implement smarter Techpack Mama logic to allow regenerating flat sketches, color sheets, and BOM artworks **all from a single prompt**
- able to run a multi-action prompt like:


- “i want you to make the hoodie orange and also add a zipper that goes through the entire hoodie kind of like bape”
- the result of this one prompt could be:
flat sketch regenerated, BOM artworks updated, and the color sheet changed — all synced automatically from a single instruction


## **V3.11 — Techpack Mama Upgrade + Color Sheet Patch (December 9, 2025)**


**What’s new**


- Default chat model upgraded to GPT-5.1.
- Regeneration support added:


- Flat sketch regeneration
- Artwork image regeneration
- *(Placement guide image regeneration coming soon)*


- New “clarify when unsure” Q&A flow.
- Override SVG editing in Color Sheet editor; upload any image or recolor existing SVGs.


## **V3.1 — Excel Export + Minor Patches + Illustrator Release (December 4, 2025)**


- Excel export available for all Studio plans; Pro plans get 3 lifetime exports.
- Illustrator plugin released for all Studio plans.
- Added stability + minor save patches.


## **V3 — Multi-Color Workflows, New Pages, More Control (November 25, 2025)**


### Multi-Color Support


- Full multi-color workflows across techpacks + order sheets.


### New Pages & Controls


- **Color Sheet** page introduced.
- **Artwork pages redesigned** by category.
- Ability to **delete pages** from techpacks.


### Intelligence & Backfill


- Techpack Mama now handles multi-color workflows automatically.
- Upload missing back views later and generate all missing pages/callouts.


### Plus


- Minor bug fixes.


## **V2.46 — Backend Updates, Image Support, UI Updates (November 12, 2025)**


- Bonus gift added for one-time purchases.
- PDF + screen width now fully expandable.
- Tweaks to AI chat toggle button.
- Zod validator added for save in/out system.
- Image uploads added to Techpack Mama.
- Live chat system implemented for user support.


## **V2.45 — Bug Fixes on Placement Guide & Flat Sketch Regeneration (November 4, 2025)**


- Fixed major issue where flat sketch regeneration sometimes didn’t actually regenerate.
- Improved placement guide dot rendering.


## **V2.46 — Size Range Editing & Critical UI Fix (October 30, 2025)**


- Edit size range *after generation* .
- Fixed bug where “apply changes” caused the save button to disappear.


## **V2.45 — Smart Flat Sketch Regeneration (October 29, 2025)**


- Natural-language flat sketch regeneration (“add hood,” “remove button,” etc.).


## **V2.44 — Sample Size Control & PDF Viewer Refresh (October 28, 2025)**


- Change sample size after generation.
- PDF viewer polished for cleaner navigation + readability.


## **V2.43 — Advanced Editing, Full Customization, Smarter Prompts (October 23, 2025)**


- Chatbot handles deep, complex editing.
- Edit history + revert system added.
- Replace placement guide + cover visuals.
- Smarter measurement + materials prompt interpretation.
- UI navigation + readability improvements.


## **V2.42 — Placement Regeneration, Units, Performance (October 16, 2025)**


- Regenerate placement guide + garment images.
- Switch between metric/imperial units.
- Faster load times + PDF generation.
- Critical save + duplicate UUID fixes.
- Placement guide now capped at 10 items.


## **V2.41 — Measurement Table Update (October 13, 2025)**


- Black border around M-size column.
- Added note that only M-size base sample is supported (for now).


## **V2.4 — Agentic Chatbot + Custom Flat Sketch Upload (October 12, 2025)**


- Agentic Chatbot launched: understands, edits, and performs actions.
- One-click application of AI-suggested edits.
- Reintroduced custom flat sketch uploads.


## **V2.3 — Regeneration Tools + SVG Export Enhancements (October 8, 2025)**


- Regenerate flat sketches with print/sewing detail control.
- Upgraded SVG export (grouping for Illustrator).
- Regenerate artwork images on demand.


## **V2.2 — Chatbot Upgrade, UI Fixes, Expanded Free Plan (October 7, 2025)**


- Chatbot now fully aware of all tech pack data.
- Ask “Am I missing anything in my BOM?” etc.
- UI polishing.
- Free users get full editor access (saving + uploads still gated).


## **V2.123 — Minor UI Patch (October 2, 2025)**


- Resizable size chart cells.
- BOM table usability improvements.


## **V2.12 — UI Update + Chatbot Upgrade (October 1, 2025)**


- Compact size chart layout.
- Chatbot connected to internal knowledge base.
- Resizable PDF + editor views.


## **V2.1 — AI Tech Pack Chatbot + BOM/Placement Stabilization (Late September 2025)**


- Chatbot launched for contextual Q&A.
- Improved BOM + placement stability.


## **V2 — Simplified Tech Packs + Detailed Image Extraction (September 19, 2025)**


- Cleaner, easier-to-read tech packs.
- Automatic extraction of trims, labels, artwork, fabrics.


## **V1.1 — Smarter Prompts & Context-Aware Measurements (Early September 2025)**


- Paste size charts directly into prompts.
- More accurate measurement tables.


## **August 2025 — Data Table Refactor, One-Shot, Admin Tools**


- Faster, more reliable save + data tables.
- One-Shot idea generator.
- Improved Placement Guide.
- Admin dashboard + SEO/privacy upgrades.


## **July 2025 — Manual Mode, Mobile, Performance**


- Manual mode for measurement lines.
- Mobile UI polishing.
- PDF pagination controls.
- Autosave + feedback enhancements.


## **V1 (June 2025) — AI Flat Sketch Generation**


- AI generation of professional flat sketches from text + images.


## **Q2 2025 (Apr–May) — Editing Tools, BOM/Costing, Placement Guide**


- Vector/manual editing for measurement lines.
- BOM + cost sheet improvements.
- Placement Guide v1.
- Onboarding + UI improvements.
- Blog + support docs launched.


## **Q1 2025 — Viewer, Subscriptions, Revisions**


- In-app PDF viewer.
- Subscriptions + blurred previews.
- Revisions + change logs.
- Front/back image support for flats.
- Improved sign-in + Sentry tracking.


## **Foundations (Nov–Dec 2024) — First Generator, Payments, Sharing**


- First AI Tech Pack generator prototype.
- CSV/XLSX outputs.
- Stripe payments baseline.
- Shareable tech pack links.
- Early SEO groundwork.
- Established the core user loop.
