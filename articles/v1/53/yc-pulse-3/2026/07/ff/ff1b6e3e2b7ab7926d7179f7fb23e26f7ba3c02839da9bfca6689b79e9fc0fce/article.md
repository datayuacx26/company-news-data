---
schema_version: "1.0.0"
document_id: "ff1b6e3e2b7ab7926d7179f7fb23e26f7ba3c02839da9bfca6689b79e9fc0fce"
company_key: "yc-pulse-3"
company: "Pulse"
source_id: "yc-pulse-3-news-import-f90f167021ce"
canonical_url: "https://www.runpulse.com/blog/introducing-the-pulse-rotation-model"
published_at: null
first_seen_at: "2026-07-23T21:33:19.979240+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:f8feccc1f336cae6f1d2384f86e0aa11d2f2d307fc385c93e155192783fb2cbb"
---

# Introducing the Pulse Rotation Model

## **The Hidden Problem in Enterprise Scans**


When we started working with large enterprises, we expected the core challenges to be complex tables, noisy charts, and multi section financial packets. What we did not expect was how often documents arrive rotated, skewed, flipped, or scanned at unpredictable angles. These are not rare edge cases. They are the default in finance, insurance, and healthcare.


Our early assumption was that state of the art vision language models and modern OCR systems could handle these distortions. After all, these models can generate photorealistic images and debug code. Surely they can handle a PDF that is slightly rotated.


They cannot.


Rotated and skewed documents expose fundamental blind spots in how modern AI systems interpret visual structure. This single failure mode cascades into table corruption, misaligned bounding boxes, incorrect reading order, and in some cases, complete extraction collapse.


This problem forced us to rethink how Pulse handles raw document intake, and it eventually led us to build the Pulse Rotation Model.


## **Why Orientation Breaks Vision Language Models**


Contrary to intuition, rotation is not a simple geometric nuisance. Orientation affects the intermediate representations these models build.


Vision transformers process documents by chopping them into fixed patches and encoding their spatial relationships. When the entire document is rotated:


- Text lines no longer align horizontally, meaning heuristic cues collapse


- Patch boundaries bisect characters in unpredictable ways


- Positional embeddings fail to capture global orientation


- Downstream layers treat tilted tables as irregular textures


- Layout reconstruction loses anchor relationships like column continuity


Even small angles, such as a three degree tilt from a scanner roller, degrade extraction quality. We observed that VLMs interpret rotated tables as abstract visual patterns instead of structured grids. Once this happens, no prompt engineering can recover the original rows and columns.


Traditional OCR engines historically avoided this problem by running orientation classification before any recognition step. LLM based systems generally do not.


The results are catastrophic for structured documents.


The difference after applying Pulse’s rotation model


## **Real Failures from Real Enterprise Workflows**


Across industries, rotation produced failure modes that were subtle, dangerous, and extremely expensive to catch manually.


1. **Financial packet corruption** A balance sheet scanned at an angle caused the model to merge right aligned numeric columns into a single vector of concatenated numbers. The downstream extraction pipeline interpreted these as new entries.


2. **Insurance underwriting breakdowns** Loss run attachments rotated one hundred eighty degrees produced inverted reading order. Policies appeared to start at page twenty four and end at page one.


3. **Healthcare protocol misinterpretation** Rotated lab forms caused dosage fields to drift away from their labels, resulting in misaligned key-value pairs that looked syntactically valid but were semantically wrong.


These errors were not loud failures. They were silent failures, which is far worse.


## **Why This Is a Hard Computer Vision Problem**


Orientation detection seems simple, but the real world is not limited to tidy one hundred eighty degree flips.


Enterprise documents exhibit:


- partial page rotation


- uneven skew from scanner rollers


- mixed orientation across sections


- pages with no text to anchor orientation


- images embedded inside text-heavy pages


- low resolution scans that obscure directional cues


Standard classification models fail because they expect clean, uniformly rotated inputs. Vision language models fail because orientation shifts alter patch topology and attention patterns.


The conclusion we reached: if we wanted deterministic extraction across millions of pages, we needed a dedicated rotation system.


A few samples of rotated document samples, with rotated tables, text, and flips


## **How Pulse Built the Rotation Model**


Pulse’s Rotation Model runs before any schema extraction, table parsing, or layout reconstruction. It was trained on billions of real scans from finance, insurance, healthcare, logistics, and public sector archives.


Technically, the system combines:


- orientation classifiers for coarse rotation


- skew regression networks for continuous angle correction


- geometric line detection for fallback modes


- multimodal confidence aggregation


- distortion-aware pre processing for noisy scans


The model produces a normalized document that is guaranteed to be upright, properly aligned, and compatible with our downstream models.


This pre processing step is now a non-negotiable part of achieving deterministic, enterprise grade accuracy.


Example of how the rotation model heavily improves Markdown outputs for retrieval systems


## **What Rotation Normalization Unlocks**


Once orientation is stabilized, everything downstream becomes simpler and more reliable.


- Table parsers recover full grid structure


- Schema extractors maintain field stability


- Charts become interpretable rather than abstract images


- Cross page linking preserves continuity


Customers see this as fewer manual corrections, fewer RAG failures, and fewer hours wasted debugging input quality.


In our internal benchmarks:


- orientation accuracy exceeded 98.8 percent


- table structure F1 improved by up to 32 percent


- schema field accuracy improved by up to 40 percent


- noisy scan failure rates dropped significantly


This is not about improving a niche use case. Orientation normalization is what makes scaled document AI feasible.


## **Closing**


The industry conversation tends to focus on larger models, deeper transformers, and more complex vision backbones. But in real world enterprise workflows, the smallest details break entire systems.


Rotation is one of those details.


The Pulse team spent the past year building a model that treats orientation not as a cosmetic issue, but as a first class problem in document intelligence. It is now in closed-beta across the Pulse API and Pulse Meridian.


If you want to test the Pulse Rotation Model on your own scanned archives, let us know. Reach out[here.](https://www.runpulse.com/contact-sales)
