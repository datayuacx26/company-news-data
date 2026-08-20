---
schema_version: "1.0.0"
document_id: "0716db270a558501181fe9e1d6cd3c0e1601aa9c862723e6588b6e7875b5718f"
company_key: "palantir-technologies-inc-class-a-common-stock"
company: "Palantir Technologies Inc."
source_id: "palantir-technologies-inc-class-a-common-stock-rss-f87a89a6619a"
canonical_url: "https://blog.palantir.com/frontend-engineering-at-palantir-engineering-multilingual-collaboration-58217e196bed"
published_at: "2026-04-22T15:49:51+00:00"
first_seen_at: "2026-07-20T03:31:10.454667+00:00"
fetched_at: "2026-07-28T20:52:23.948620+00:00"
content_hash: "sha256:76be7da3121ea9a092e05c708604134f338f1a35590672bdfc5fdfcae1b256fd"
---

# Frontend Engineering at Palantir: Engineering Multilingual Collaboration

# Frontend Engineering at Palantir: Building Multilingual Collaboration


[Palantir](https://palantir.medium.com/?source=post_page---byline--58217e196bed---------------------------------------)


8 min read


·


Apr 22, 2026


--


Press enter or click to view image in full size


[About this Series](https://blog.palantir.com/all?topic=palantir-frontend)
*Frontend engineering at Palantir goes far beyond building standard web apps. Our engineers design interfaces for mission-critical decision-making, build operational applications that translate insight to action, and create systems that handle massive datasets — thinking not just about what the user needs, but what they need when the network is unreliable, the stakes are high, and the margin for error is zero.*


*This series pulls back the curtain on what that work really looks like: the technical problems we solve, the impact we have, and the approaches we take. Whether you’re just curious or exploring opportunities to join us, these posts offer an authentic look at life on our Frontend teams. Find all Frontend Engineering blog posts*[here](https://blog.palantir.com/all?topic=palantir-frontend) *.*


*In this blog post, Rahma, a frontend engineer based in CA, shares how building Dossier’s translation mode — a recent project she tackled at Palantir — deepened her appreciation for frontend engineering, while tackling challenges of secure, multilingual collaboration for global teams.*


When people think about Palantir, they often imagine impressive data pipelines, backend wizardry, and compelling ontologies — but there is a lot more to the story, especially if you care about frontend engineering.


Recently, I worked on a project to bring read-only translation mode to Dossier, one of our collaborative text editor applications. This feature is particularly important for our defense and intelligence customers, who often operate in multilingual and highly restrictive environments. We’ve also implemented translation in our presentation tool, our messaging tool, and our commenting library, creating an ecosystem of multilingual collaboration across all our communication apps.


In this post, I’ll share how we built Dossier’s translation mode, the technical challenges we faced, and why it’s a great example of the kind of end-to-end, high-impact frontend that keeps me excited and motivated at work.


## Why Translation? Why Now?


In almost any important news headline you see today, you’ll see how most operations require deep, multi-national collaboration. These different partner nations often speak different languages, but we can’t let that be a barrier to high-signal collaboration and teamwork. Users, analysts, planners, and operators are working with global teammates more than ever. But many of them are on closed or classified networks, where standard, public, consumer translation tools simply aren’t available or allowed.


To make our productivity suite truly collaborative, we needed to bring translation into Dossier, our rich-text document editor, and make it as seamless as possible. We already had rolled out translation features in Chat and Slides, but Dossier posed a new set of challenges. Unlike chat messages or slide decks, Dossier documents can be hundreds of pages long, full of complex formatting, tables, and live collaborative edits. Our goal was to let users toggle into a translation mode, see the entire document in their chosen language, and still interact with the content (copying text and adding comments) without losing context or performance.


## Designing Translation


Our vision was simple: with one click, users could view any Dossier in a translated, read-only mode. Under the hood, this required:


- **Consistent UX:** The translation experience needed to match what users already knew from Chat and Slides.
- **Security:** All translations had to run through our in-house Language Model Service (LMS), never leaving the secure network.
- **Performance:** Even massive documents should translate quickly, without freezing the browser or overloading the backend.
- **Collaboration:** Translation mode had to play nicely with real-time editing, comments, and live user presence.


## Technical Implementation


**1. Shared components**


We’d already built translation UI components for Slides and Chat, so we extracted these into a shared library to ensure a consistent experience across apps. But Dossier’s document model is significantly more complex — we had to extend these components to handle deeply nested rich-text structures, tables, and custom widgets.


Press enter or click to view image in full size


**2. Redux state for translation**


Translation isn’t a feature you can bolt on — it touches the entire application. A user’s language preference, the loading state of every translated block, caching, toggling on and off without losing your place in the document — all of this needs to be coordinated globally. So we were intentional about how we modeled state, adding a dedicated slice to Dossier’s Redux store to manage it cleanly.


```text
export interface TranslationsCacheValue {    originalText: string;    translatedText: IAsyncLoaded<string>;  }   export interface TranslationsCache {    [elementId: string]: TranslationsCacheValue;  }   export interface TranslationState {    isInTranslationMode: boolean;    supportedLanguages: IAsyncLoaded<LanguageMetadata[]>;    translations: TranslationsCache;    detectedLanguage?: string;    sourceLanguage?: string;    targetLanguage?: string;  }
```


This state is also responsible for tracking which blocks are loading, succeeded, or failed, so we can shown detailed feedback to the user.


**3. Chunking: balancing translation quality and speed**


Translation has to feel fast and seamless, even for large multi-page Dossiers. Sending the entire document at once would leave users staring at a loading screen; translating word-by-word or sentence-by-sentence would be faster but degrade quality, since most translation models rely on broader context to produce natural results.


We settled on chunking at the block level — paragraphs, list items, table cells — which strikes the right balance. Users see incremental progress as blocks appear in their translated form as soon as they’re ready, rather than waiting for the whole document.


For the next version of chunking, we’d like to tackle a few interesting edge cases:


- Giga-long paragraphs: If the user writes a single paragraph that is pages long
- Multi-language blocks: If a paragraph contains multiple languages, the model may fail to detect the correct language
- Non-grammatical fragments: Users can create blocks that are just bullet points, acronyms, or fragments that can be hard for the model to interpret without surrounding context


For now, we rolled out our block-level chunking, but there is plenty of of room for future improvement as we get more usage data and feedback.


**4. Lazy, incremental translation**


## Get Palantir’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


In addition to our strategy of chunking, we also had to think about which chunks to prioritize for 100+ page documents. We used the[Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) to “lazy load” translations. Only the blocks currently visible in the user’s viewport are sent to LMS for translation. As users scroll, new blocks are translation on demand.


```text
// IntersectionObserver API hook for "has entered screen" detection  function useHasEnteredScreen(ref) {    const [hasEntered, setHasEntered] = React.useState(false);     useEffect(() => {      if (hasEntered || ref.current == null) return;       const el = ref.current;      const observer = new IntersectionObserver(([entry]) => {        if (entry.isIntersecting) {          setHasEntered(true);          observer.disconnect();        }      });       observer.observe(el);      return () => observer.disconnect();    }, [hasEntered]);     return hasEntered;  }
```


This keeps the UI responsive and avoids hammering the backend with huge requests. We often have many users accessing the same document at the same time, so we implemented batching, debouncing, and caching to avoid re-translating the same block multiple times.


Press enter or click to view image in full size


Each paragraph or block shows a status indicator: loading, failed, or translated. If translations fail due to translation resource constraints or model limitations, users can retry failed translations with a click.


**5. Handling collaboration & live edits**


Dossier is a collaborative editor, multiple users can edit the same document in real time. If someone edits a paragraph while you’re viewing the translation, we detect the change, invalidate the cached translation, and reloads the updated text for translation.


We use a per-block cache (keyed by element ID) to track translation status. When a block is edited, the cache entry is invalidated and translation is re-triggered as long as the block is still on the user’s screen. This ensures that the translated view is always up to date, even as the document evolves.


**6. Stripping formatting for translation quality**


One of the hard tradeoffs: to get the best translation results, we strip all inline formatting (bold, italics, hyperlinks, mentions) before sending text to LMS. This is because translation models work best when given the full, unbroken sentence context.


If we were to split text around formatting, say translating “I **love** frontend engineering” by sending “I” and “frontend engineering” separately, and “love” as a bolded chunk, the translation model loses the sentence structure. This can produce incorrect or nonsensical results, especially in languages where word order or agreement depends on the whole sentence.


### Example:


Consider the English sentence:


> *I* ***love*** *frontend engineering*


Suppose we tried to preserve formatting by splitting:


- “I”
- “ **love** ”
- “frontend engineering”


If we translate these fragments separately into French:


- “I” → “Je”
- “love” → “amour” (noun, not verb)
- “frontend engineering” → “ingénierie frontend”


If we stitch it back together:


> *Je* ***amour*** *ingénierie frontend*


This is not correct and doesn’t make sense, but if we send the full sentence to the translation model:


> *“I love frontend engineering.” → “J’adore l’ingénierie frontend.”*


Here, “J’adore” is the correct verb form and the sentence flows naturally. This is why preserving sentence context is critical for translation quality, and why we currently strip formatting before translation, otherwise users could end up with confusing or misleading results.


That is why, for now, the translated view is plain text, but we’re exploring more ways to preserve more formatting as model and API support improves. For example, future support for HTML or Markdown-aware translations.


## What makes this work interesting?


The project covered many aspects of frontend development:


- **Component design:** Preparing the next text-based app in our ecosystem for success by enabling seamless adoption of the component work completed for Dossier.
- **State management:** Orchestrating translation status across hundreds of blocks, with real-time-updates.
- **Performance engineering:** Using browser APIs (like intersection Observer and LRU caches) to keep things fast for users and being kind to our backend servers.
- **Security & deployment:** Making sure everything works on air-gapped, classified, or otherwise restricted networks.


## The Result: Translation for Every Dossier


Today, users can toggle translation mode in any Dossier, select source and target languages, and instantly see a translated, read-only version of their document. All text content is “lazy” translated as you scroll, and any incoming edits are re-translated in real time.


## Why this Matters for Frontend Engineers


As a frontend engineer who loves building real products that solve real problems, especially under constraints like security, performance and collaboration, projects like Dossier translation keep me motivated and excited to work. I really enjoyed balancing the technical and performance constraints with a delightful experience for users and leveraging modern web APIs to deliver Dossier translation.


*If this sounds like the kind of project and impact you’re interested in, check out our open roles today:*[https://www.palantir.com/careers/open-positions/](https://www.palantir.com/careers/open-positions/) *. Our most applicable frontend postings are the “Web Application Developer” roles. We’re also hiring for these two specific roles right now:*[Software Engineer — Core Interfaces](https://jobs.lever.co/palantir/cf76738e-3030-42fa-92ac-a9446df956fc) *(Palo Alto), and*[Software Engineer — Defense Applications](https://jobs.lever.co/palantir/f7dbfdf1-0bb1-4c11-ac15-6a139cee3410) *(DC).*
