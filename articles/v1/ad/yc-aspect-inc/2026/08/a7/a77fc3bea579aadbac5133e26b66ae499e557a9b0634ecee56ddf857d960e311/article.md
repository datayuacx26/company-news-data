---
schema_version: "1.0.0"
document_id: "a77fc3bea579aadbac5133e26b66ae499e557a9b0634ecee56ddf857d960e311"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/how-to-build-string-outs-and-selects-for-documentary-editing"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T17:42:38.033693+00:00"
fetched_at: "2026-08-05T17:42:39.958443+00:00"
content_hash: "sha256:f634e680a02d98ada7657bf60f66b9948b09c96cf426749bdee5e9de891396c6"
---

# How to Build String-Outs and Selects for Documentary Editing

The first decision is “what kind of search problem are you trying to solve?” Documentary footage gets painful when every good moment lives only in someone’s memory. String-outs and selects are the bridge between raw media and a rough cut: they let the team watch, compare, tag, and reuse material without constantly digging through bins.


A good setup usually has three layers:


- String-outs that show what exists
- Selects that isolate what is promising
- Story or theme timelines that test how ideas connect


Those are different jobs. If you collapse them into one giant “best stuff” timeline, it may feel fast for a day, then turn into a junk drawer.


Separate timelines help keep raw material, chosen moments, and story assemblies from collapsing into one messy sequence. Timeline type Main job Best organized by Do not use it for


String-out Show what exists in context Shoot day, interview, location, event, or full coverage category Only the best moments, because that hides context


Selects Isolate promising material Topic, character, quality, emotional beat, or editorial function Every usable second, because it becomes another string-out


Story or theme timeline Test how ideas and scenes connect Story beat, argument, emotional turn, or sequence idea Polished cutting, unless the structure is already working


Rough cut Shape the audience experience Act, sequence, scene, or[radio cut](https://human.libretexts.org/Bookshelves/Theater_Film_and_Storytelling/Documentary_Culture/15%3A_Editing_the_Documentary/15.03%3A_Building_Assemblies) order Storage for maybes, because it slows decisions


## Decide what each timeline is supposed to do


A string-out is a linear sequence of footage. It gives the editor, director, producer, or story team a way to watch material in context. For documentary, that can mean everything shot on a day, everything from one interview, all b-roll from a location, or all coverage of a recurring subject.


Selects are narrower. They're the usable, interesting, emotional, funny, revealing, or visually strong pieces pulled out of that larger body of footage. They're the shelf of ingredients you actually want near the stove.


The distinction matters because each timeline answers a different question:


- A camera-day string-out answers: “What did we shoot?”
- An interview string-out answers: “What did this person say, and how did it unfold?”
- A b-roll string-out answers: “What visual coverage do we've?”
- A[selects reel](https://www.oreilly.com/library/view/documentary-editing-2nd/9781000854671/xhtml/chapter5.xhtml) answers: “What might actually make the film?”
- A rough cut answers: “What is the audience experiencing, in what order?”


When a team agrees on those definitions early, assistant editors can prep material without guessing how the editor wants to think. It also helps post supervisors estimate the real amount of editorial work. One hundred hours of footage means watching, logging, syncing, transcribing, tagging, trimming, grouping, and revisiting.


## Build the project around retrieval, not neatness


Folders and bins are useful, but they only solve part of the problem. They tell you where something lives, but they don't tell you why it matters.


Start with a[clean ingest and naming system](https://www.avid.com/resource-center/how-to-edit-documentary) before anyone starts[pulling selects](https://masv.io/workflow/post-production) . Your team often can't recreate documentary media, so backups, consistent folder structure, and clear names protect the edit.


Common top-level bins or folders include:


- 00_PROJECT
- 01_MEDIA
- 02_AUDIO
- 03_SYNCED
- 04_STRINGOUTS
- 05_SELECTS
- 06_STORY
- 07_CUTS
- 08_EXPORTS
- 09_GRAPHICS_STILLS_ARCHIVE


Inside media, most teams sort by shoot date, camera, location, event, interview subject, or some combination of those. Write dates consistently, ideally year-month-day, so folders sort naturally. A folder named` 2026-03-09_Factory_Interview_Maria` is more useful than` Factory stuff Monday` .


The real value comes when your team pairs folder structure with metadata. Add information that helps the editor retrieve material by story need.


Metadata makes a clip searchable by context, not just by where it lives in a folder. Useful metadata fields or tags include:


- Interview subject
- Location
- Shoot date
- Scene, event, or topic
- Character names
- Story beat
- Emotional tone
- Visual quality
- Audio quality
- Release or rights status
- Archival source
- Language
- Transcript status


On day one, you need enough structure that a new assistant can search “Maria eviction anger” or “night exterior rain wide” and land close to the right material.


## Separate interview work from b-roll work


Interview footage and b-roll behave differently, so don't force them through the exact same selects process.


Interviews are usually story-first. The editor needs clarity of thought, contradiction, emotion, setup, payoff, and transitions. Your team will usually organize the best interview selects by what the person is saying, not by when it was shot.


B-roll is usually coverage-first. The editor needs geography, action, cutaways, process, texture, and transitions. Your team will usually organize the best b-roll selects by subject, shot size, movement, and usefulness.


For interview-heavy films, create one string-out per interview or per subject session. Keep the full synchronized interview intact somewhere, then create a working string-out or duplicate sequence for pulling bites. If there are multiple cameras, preserve sync and camera angle relationships. If there's separate production audio, don't let selects become detached from the good audio unless the NLE workflow preserves that relationship reliably.


For b-roll, create string-outs by shoot day, location, event, or visual category. A day-based string-out is useful for review because it mirrors the shoot. A subject-based string-out is useful for editing because it mirrors need.


A b-roll organization might include timelines like:


- BROLL_Factory_Workers_Hands
- BROLL_Factory_Machines_Wides
- BROLL_Maria_Home_Life
- BROLL_City_Transitions_Night
- BROLL_Protests_Crowd_Reactions


The day string-out helps you understand coverage. The[themed b-roll string-out](https://www.youtube.com/watch?v=zA1qaQib4G0) helps you cut faster when the story calls for a specific visual idea.


## Transcribe early, but don't stop watching


For interview-heavy documentaries, transcripts are now part of the core workflow. Text search can save days when a producer asks, “Didn’t someone say the policy changed in 2018?” It also lets story producers and directors work outside the NLE by marking quotes, grouping topics, and building early paper edits.


But transcripts aren't a replacement for watching because they miss hesitation, irony, eye movement, tension, silence, and the way one sentence lands after another. A quote that reads flat may be devastating on camera. A quote that reads perfect may be unusable because the subject laughs through it or the audio is bad.


Use transcripts to speed discovery, then confirm in picture and sound. A practical transcript workflow looks like this:


- Generate transcripts for every synced interview
- Correct names, key terms, locations, and recurring phrases
- Add speaker labels where needed
- Mark strong quotes by topic or story beat
- Transfer useful transcript notes into the NLE as markers, comments, subclips, or timeline labels
- Keep transcript timecode aligned with source media


The key is alignment. If a producer marks a quote in a transcript, the assistant editor should be able to find that exact line in the NLE without detective work.


## Create string-outs that someone can actually watch


A string-out shouldn't be a dumping ground with random clips in random order. It should be linear enough that someone can watch it and understand what they're seeing.


For interviews, keep the order close to the original conversation during the first pass. That preserves context. Later, build topic or theme timelines that rearrange material by idea. If you skip the full interview context too early, you risk lifting quotes that sound stronger than they really are.


For b-roll, choose an order that matches the editor’s likely use. Common options include:


- Shoot order, for reviewing what happened on the day
- Location order, for understanding geography
- Shot size order, from wides to mediums to close-ups
- Action order, following a process from start to finish
- Subject order, grouping all coverage of one person or object


There's no single correct structure. The right structure is the one that reduces future searching. If the film follows a bakery owner through a morning routine, process order may beat shoot order. If the film is about a city changing over ten years, location and time period may matter more.


Trim obvious garbage, but don't over-polish string-outs. Remove camera whips, long black gaps, accidental floor shots, and false starts if they slow review. Keep enough handles that an editor can hear the lead-in and tail of a moment. For interviews, handles are especially important because a breath, pause, or reaction may become part of the eventual edit.


## Pull selects with a consistent rating language


Selects work best when everyone knows what “selected” means. If one assistant pulls only perfect final-cut material and another pulls anything vaguely interesting, the editor will stop trusting the timelines.


Create a simple rating language. It can be markers, clip colors, custom metadata, bin names, or stacked tracks. The exact mechanism depends on the NLE, but your team should share the meaning.


A practical rating system might separate material this way:


- Must-consider material has central story value, strong performance, and clean enough quality to use
- Possible material has a useful idea, but weaker delivery, duplicate thought, or technical issue
- Problematic but important material is story-critical, but has bad audio, bad focus, rights concern, or visual limitation
- Color or texture material is useful as visual support, transition, mood, or pacing relief
- Do-not-use-unless-needed material is redundant, weak, technically compromised, or context-dependent


This keeps selects honest: some selected clips are simply the best available version of a necessary idea.


When pulling interview selects, preserve the question context if it helps the editor understand the answer. You may not use the interviewer’s voice in the edit, but the question often explains why the subject answered the way they did. If the subject says “that was the day everything changed,” the editor needs to know what “that” refers to.


For b-roll selects, tag what the shot does, not only what it depicts. “Maria enters apartment” is useful. “Lonely, slow push, dusk, transition out of court scene” is more useful. Documentary b-roll often carries emotional and structural weight, so log function as well as content.


## Use subclips carefully


Subclips can be great for isolating interview bites and clean b-roll moments. They make bins searchable and help producers browse material without loading long source clips, but they can also create problems if the workflow breaks timecode, loses handles, disconnects[synced audio](https://partnerhelp.netflixstudios.com/hc/en-us/articles/4415931246995-Dailies-Best-Practices) , or hides the original context.


Use subclips when they support retrieval, but avoid using them as the only record of your choices.


For a safer approach, maintain all of the following:


- Full source clips and synced interviews
- Timeline-based selects with original timecode and handles
- Subclips or grouped clips for moments the team will reuse often


That way, the editor gets convenience without losing access to the original material. If the edit later needs five seconds before the selected bite, the team isn't stuck rebuilding the source relationship.


Preserved handles let an editor trim earlier or later without losing the connection back to the source clip.


## Build themed string-outs for story development


Once the first selects pass exists, start building themed string-outs. This is where documentary editing becomes story work.


A themed string-out groups material by idea rather than source. It might contain interview bites from five people, verité moments, archival clips, stills, and b-roll that all relate to one beat. The goal is to see whether the material supports a sequence.


A themed string-out gathers interview, b-roll, stills, and audio around an idea instead of around the source bin. Useful documentary themes might include:


- Origin story
- First warning signs
- Institutional failure
- Money trail
- Aftermath


These timelines let the team test arguments and emotional arcs before committing to a rough cut. They also expose gaps. If the “turning point” string-out has twelve people explaining the same fact but no scene where we feel it happening, the team knows the story material is the issue.


Keep themed string-outs rough. Don't spend an afternoon perfecting music under a sequence that may not survive the next story meeting. Early documentary assembly should move quickly enough that the team can discover unexpected connections.


## Start the rough cut as a radio cut


For many documentaries, especially interview-driven films, you should begin the rough cut with sound and structure. Build a radio cut using the strongest interview and narration material first. Ignore most b-roll, music, color, and polish until the spine works.


A radio cut helps answer the questions that matter early:


- Do we understand the situation?
- Do we care about the people?
- Is there a progression of ideas?
- Is there a real turn, conflict, or reveal?
- Where does the audience need breath?


After the radio cut starts to hold together, pull visual selects into it. This is where organized b-roll pays off. You're no longer browsing every shoot day looking for “something of Maria walking.” You're choosing from a curated set of walking, home, work, transit, and reaction shots that have already been tagged.


If the project is vérité-driven rather than interview-driven, you may start the rough cut with scenes instead of radio structure. Even then, selects still matter. Build scene selects around action, reversals, entrances, exits, and character behavior, then assemble scenes in a rough story order.


## Move selects efficiently inside the NLE


Most NLEs support some version of a three-sequence workflow: string-out, selects, and main edit. Some editors use stacked timelines or[pancake editing](https://www.hobbesedits.com/blog/advanced-video-editing-tips-stringout-selects-pancake-editing) , with the source string-out above the selects or edit timeline. Others prefer source monitor loading, match frame, subclips, or text-based editing tools.


The method matters less than the rule: never make the editor hunt twice.


A common timeline setup is:


- STRINGOUT for all reviewed material in a logical order
- SELECTS for trimmed useful moments grouped by topic, subject, or quality
- STORY for themed assemblies and sequence experiments
- CUT for the current rough cut or active edit


When pulling a moment, copy enough handle to allow trimming later. Keep source timecode visible. Use markers or clip names that survive duplication. If your NLE supports color labels, use them consistently across the project, not differently in every bin.


For teams moving between tools, test the handoff early. XML, AAF, and EDL workflows can behave differently with multicam clips, synced audio, speed changes, nested sequences, transcripts, and markers. Don't wait until the story team has built a whole paper edit to learn that the export strips useful metadata or relinks to the wrong audio.


## Watch for failure modes that slow documentaries down


Most selects workflows fail in boring, predictable ways, but the good news is that your team can prevent them.


Common problems include:


- Giant selects timelines with no sections or labels
- Interview bites separated from transcript context
- Synced audio lost during duplication or export
- Multicam clips flattened too early
- Editors cutting from proxies that don't relink cleanly


Your team fixes this with shared meaning. Decide what counts as a string-out, what counts as a select, how themes are named, and where the current version lives.


## Name timelines like someone else will open them


Keep timeline names boring and clear. If the project goes on for months, clever names become liabilities.


A useful naming pattern includes date, type, subject, and version:


- 2026-03-09_STRINGOUT_INT_Maria_v01
- 2026-03-09_SELECTS_INT_Maria_v02
- STORY_EvictionTimeline_v03
- BROLL_SELECTS_FactoryMachines_v01
- CUT_RoughAssembly_2026-04-18_v05


Avoid names like` new selects` ,` old selects` ,` good stuff` , or` use this one final` because those names always become lies.


For active edits, include the date or version number. For selects and string-outs, version only when the content meaningfully changes. If an assistant fixes a typo in a marker, that probably doesn't need a new selects version. If the team removes weak bites and reorganizes the sequence by story beat, it does.


## The practical endpoint


A strong documentary selects workflow produces confidence. The editor knows what exists. The director can compare options. Producers can find story beats. Assistants can support the edit without constant translation. Post supervisors can see where the material is healthy and where the team is still searching.


Build string-outs to reveal the footage. Pull selects to reduce noise. Use transcripts to find language, but watch the footage to judge performance. Organize b-roll by editorial function. Build themed timelines to test story ideas. Then move only the strongest material into the rough cut and keep the path back to source clear.


That's how you turn a mountain of documentary media into something the edit room can actually think with.


## FAQ


A string-out is a broad timeline that shows what footage exists, often organized by interview, shoot day, location, or event. A selects timeline is narrower and contains the moments that seem useful, strong, revealing, emotional, or visually valuable. String-outs help the team review and understand the material. Selects help the editor work faster when building scenes, story sections, or a rough cut.


B-roll should be tagged by both content and editorial function. A basic tag might describe what is visible, such as "Maria enters apartment." A better tag might add how the shot can be used, such as "lonely dusk entrance, slow push, transition after court scene." Useful b-roll tags often include location, subject, action, shot size, movement, mood, visual quality, and possible story use.


Subclips can be useful for isolating strong interview bites or clean b-roll moments, but they shouldn't be the only record of the selection. Keep access to the full source clips, synced interviews, original timecode, and enough handles. If subclips lose audio relationships, timecode, or context, they can slow the edit later instead of helping it.


Transcripts should be created early, especially for interview-heavy projects. They make it easier to search for facts, quotes, names, dates, and story topics. However, transcripts should support viewing, not replace it. Editors still need to watch and listen for performance, emotion, pauses, irony, bad audio, and visual behavior that may not be obvious in text.


The fastest path is usually to build a rough structure from curated material rather than returning to raw footage for every decision. Interview-driven projects often start with a radio cut using the strongest spoken material, then add visual selects once the story spine works. Vérité-driven projects may start with scene selects organized around action, character behavior, entrances, exits, and turning points. In both cases, the edit should preserve a clear path back to the source media.


Use metadata that reflects editorial need, not only where the footage came from. Aspect supports custom metadata fields for subject, topic, rights status, transcript status, and visual use, helping assistants and editors retrieve moments by story function instead of hunting through bins.
