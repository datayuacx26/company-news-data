---
schema_version: "1.0.0"
document_id: "f4c37851cad1e6db22bbd131c5633b7c22e7767213eb1d3dd913c43169511e49"
company_key: "yc-magic-hour"
company: "Magic Hour"
source_id: "yc-magic-hour-news-import-988efd6b5de7"
canonical_url: "https://magichour.ai/blog/fable-vs-sol-taste-test"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T01:59:05.044889+00:00"
fetched_at: "2026-08-18T01:59:06.136529+00:00"
content_hash: "sha256:70a7f736e99cd8910ae626af262d842c78132b04d0858a4bc1c7f0634396f56e"
---

# Testing Fable vs Sol in terms of taste (they are both bad)

We built a small harness and gave Fable 5 and Sol 5.6 the same jobs to evaluate their taste and creativity.


They had to follow the same creative process and had access to the same skills and instructions to build 4 different 15s videos: three ads and one mini-documentary.


For video and image generation they had access to a local[MagicHour MCP server](https://magichour.ai/) . The budget was 5,000 Magic Hour credits per run (about $8.7) with a total of 8 runs.


We also gave them access to web search and ffmpeg for manipulating videos.


Before we dive into the setup and details, here are the videos:


## Burger restaurant


The models were instructed to invent an original burger restaurant from scratch and build a video ad for it.


### Fable 5


[▶ Watch the Burger — Fable 5 final cut (15s)](https://github.com/maniculehq/fable-vs-sol-taste-test/raw/main/videos/burger-fable.mp4)


### Sol 5.6


[▶ Watch the Burger — Sol 5.6 final cut (15s)](https://github.com/maniculehq/fable-vs-sol-taste-test/raw/main/videos/burger-sol.mp4)


## Sunscreen for men


For the sunscreen brand, the brief was to invent an original men's sunscreen brand and make its launch film.


### Fable 5


[▶ Watch the Sunscreen — Fable 5 final cut (15s)](https://github.com/maniculehq/fable-vs-sol-taste-test/raw/main/videos/sunscreen-fable.mp4)


### Sol 5.6


[▶ Watch the Sunscreen — Sol 5.6 final cut (15s)](https://github.com/maniculehq/fable-vs-sol-taste-test/raw/main/videos/sunscreen-sol.mp4)


## Codex micro


We wanted to see how they perform with more constraints, so we gave them an existing product to build the ad for: the Codex Micro. They were instructed to research what it is and build the ad for the real thing and not invent a concept from scratch.


### Fable 5


[▶ Watch the Codex Micro — Fable 5 final cut (15s)](https://github.com/maniculehq/fable-vs-sol-taste-test/raw/main/videos/codex-micro-fable.mp4)


### Sol 5.6


[▶ Watch the Codex Micro — Sol 5.6 final cut (15s)](https://github.com/maniculehq/fable-vs-sol-taste-test/raw/main/videos/codex-micro-sol.mp4)


## Big Ben presentation


Here we wanted to see how creative they can be with an information-focused video. The brief was to create a short presentation video of Big Ben. Same for the Codex Micro: they were specifically instructed to search the web for facts and ground every decision in reality.


### Fable 5


[▶ Watch the Bigben — Fable 5 final cut (15s)](https://github.com/maniculehq/fable-vs-sol-taste-test/raw/main/videos/bigben-fable.mp4)


### Sol 5.6


[▶ Watch the Bigben — Sol 5.6 final cut (15s)](https://github.com/maniculehq/fable-vs-sol-taste-test/raw/main/videos/bigben-sol.mp4)


## The setup


Each run puts the model into a fresh, isolated workspace with two things: a director's brief and a` skills/` folder containing 8 skills: Magic Hour API usage, concepting, brand boards & identity, screen testing, cinematography prompts, dailies QC, voice & audio, and ffmpeg editing.


The briefs and skills are the exact same for both models.


The brief mandates a ten-stage production pipeline: research, visual concepting, script, storyboards, a screen test to pick a video model, the shoot, dailies review, audio, edit, final review.


There is a validation loop at every stage: produce, inspect, write a verdict, revise until it passes. The models can't watch video, but they can view images, so they're told to extract frames from every clip and actually look at them and validate them before moving to the next stage.


The judging rubric has five criteria: subject fidelity (the hero object can't shape-shift between shots), brand and text integrity (all on-screen text must be overlaid in the edit to avoid video models' text-rendering artefacts), craft, sound and **creativity and taste** . The brief explicitly states that creativity and taste is the most important criterion and that they will be evaluated on this very carefully.


Each model self-scores its final cut 1–5 on all five criteria and writes a director's statement explaining why it stopped.


## Summary


Here is a quick overview of all the runs with their key data points:


Run


Wall time


Tool calls


Web searches


Assets


Iterations


Credits


Tokens (in / out)


Credits cost


Tokens cost


Total cost


Fable · burger


25m 53s


114


2


36


7


1,963


8.59M / 70.7k


$3.40


$14.21


**$17.61**


Sol · burger


30m 01s


150


2


93


1


3,496


12.99M / 49.8k


$6.06


$9.05


**$15.10**


Fable · sunscreen


35m 29s


151


2


45


4


4,574


10.25M / 84.1k


$7.93


$17.31


**$25.24**


Sol · sunscreen


38m 29s


135


6


60


4


2,243


13.98M / 47.6k


$3.89


$9.59


**$13.48**


Fable · codex-micro


34m 50s


166


1


39 (+192 refs)


4


2,804


16.22M / 94.8k


$4.86


$23.77


**$28.63**


Sol · codex-micro


39m 15s


145


4


82 (+9 refs)


3


3,442


18.18M / 66.6k


$5.97


$12.55


**$18.52**


Fable · bigben


46m 55s


176


4


56


3


4,588


18.83M / 117.2k


$7.95


$27.97


**$35.92**


Sol · bigben


39m 53s


191


10


155 (+7 refs)


4


3,302


23.21M / 58.3k


$5.72


$14.73


**$20.46**


Token costs at fixed August 2026 list rates (Claude: $10/$50 per MTok in/out, cache reads $1, cache writes $12.50; GPT: $5/$30, cached input $0.50).


### Wall-clock time


Wall clock barely separates the two models because most of a run is spent waiting on generation queues. The one outlier is Fable's Big Ben run (46m 55s, the slowest of the eight). It's the only run that generated its own narrator through a video model.


### Tool usage


More than half of every run's calls are local work: reading skills, viewing frames, running ffmpeg... The share peaks on the research briefs (75% for Fable's Codex Micro).


### Web searches


Sol searched the web much more than Fable. It did 22 web searches across all runs compared to only 9 for Fable. That's a 2.4x delta. Fable researched by downloading instead, pulling 94 reference images on Codex Micro and reading pages through fetches after a single search. Sol also spent several of its searches on MagicHour pricing pages before committing credits, which no brief asked for.


### Token usage


Sol reads more, Fable writes more. Sol's input tokens were higher than Fable's in every matchup (13–23M tokens vs 8.6–18.8M), but Fable wrote more than Sol by 40–100% per run. Fable wrote 367k output tokens total vs 222k for Sol.


### Cost


Fable was systematically more expensive than Sol in this exercise. Their use of MagicHour credits was similar. Across all runs, Fable spent 11% more MagicHour credits than Sol. The main difference is LLM token use and cost, with Fable 5 being 80% more expensive on average and a total spend of $83.26 vs $45.92 for Sol 5.6.


## Details for each run


### Fable 5 · burger — FERAL BURGER CO. "APEX"


**Concept.** A burger ad shot as a prestige wildlife documentary, played dead straight: the night city is the wilderness, the griddle glows like a savanna dawn, the smash is a predator strike in slow motion, and a hushed narrator whispers "Night falls on the city... and a predator stirs." The signature burger — THE APEX, a minimal double-smash stack with no lettuce or tomato, chosen precisely so it could stay identical across shots — gets the reverent species treatment ("the apex · burgerus rex", "OBSERVED NIGHTLY"). Tagline: **TOP OF THE FOOD CHAIN.** Three creative territories were boarded; "2AM neon" and "precision lab" were killed in writing at the moodboard stage as polished-but-forgettable.


**Palette.**


- ` #141414` charcoal night
- ` #FF7A1A` ember orange
- ` #1E3D2F` deep forest green
- ` #F5A623` molten cheddar
- ` #F2E9DC` bone cream


**Final video.** 15.0s · 1280×720 · audio ✓. Self-scores: fidelity 5, brand 5, craft 4, sound 4, creativity 5.


**Models.** Images:[Nano Banana](https://magichour.ai/api/nano-banana) (8 generations + 1 edit). Video:[kling-3.0](https://magichour.ai/api/kling-3-0) for every shot — screen-tested against[veo3.1](https://magichour.ai/api/veo-3-1) (equal fidelity, twice the price) and[seedance-2.0](https://magichour.ai/api/seedance-2-0) , which disqualified itself with a false-positive content-filter error (1,440 credits charged and refunded).


**Web searches (2).**


- ` what makes food commercial footage crave-worthy burger advertising craft techniques`
- ` "Feral Burger" restaurant chain brand`


The second one checks that the name it had just invented didn't already exist.


**Voice.** A documentary-whisper voiceover in the Attenborough style. Cast over Herzog and Freeman by generating all three on the same test line and measuring levels with ffprobe; delivered as four separate phrase files for exact cut alignment.


**Tool calls (114).** Read 29 · Bash 24 · Write 7 · Edit 4 · ToolSearch 2 · WebSearch 2 · MCP: 8 image gen, 1 image edit, 6 image→video, 7 voice, 22 wait/polls, 2 upload URLs.


**Concept images (7).** Three concepts explored, two killed:


**Files.**[script.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/burger-fable/script.md) ·[director_statement.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/burger-fable/director_statement.md)


### Sol 5.6 · burger — DEAD AIR


**Concept.** A six-seat late-night burger counter "built for the few minutes when the city finally stops talking" — and food that does the opposite. The signature burger, The Loud One, is compact but mixed at monumental scale, and the film is really a sound-design piece: near-silence, then a griddle impact detonating into dry sizzle, a wrapper crack driving a sub-bass pressure pulse, the food sounds resolving into a sparse 82 BPM rhythm — then everything gates to silence under the end card **OPEN LATE.** Three territories were explored (HUSH HOUR, FALSE DAWN, SPIN CYCLE); HUSH HOUR advanced and was renamed DEAD AIR.


**Palette.**


- ` #071D29` dead cobalt
- ` #55406F` violet paper
- ` #FF9D21` sodium amber
- ` #F4A321` cheddar orange
- ` #AEB9BC` chrome
- ` #F2EBDD` bone


**Final video.** 15.0s · 1280×720 · audio ✓. Self-scores: fidelity 5, brand 5, craft 4, sound 4, creativity 4 — the only run to hold a 4 on its own creativity.


**Models.** Images:[Nano Banana](https://magichour.ai/api/nano-banana) (8 generations + 3 edits). Video:[Kling 3.0](https://magichour.ai/api/kling-3-0) committed for every shot — screen-tested against[LTX 2.3](https://magichour.ai/api/ltx-2-3) (held fidelity but delivered "almost a still") and[Seedance](https://magichour.ai/api/seedance-2-0) (rejected for a wrapper artifact and extreme cost). The winning test clip was promoted straight into the film as shot 3.


**Web searches (2).**


- ` food photography burger appetite appeal steam cheese pull backlighting texture commercial techniques ...`
- ` site:magichour.ai pricing API image to video credits Kling 3.0 LTX 2.3 Seedance 2.0 ...`


**Voice.** None — deliberately. Sol enumerated the voice tool, found the voice list "made entirely of named public figures," and "walked away from a read rather than manufacture a weak or imitative one," spending zero voice credits. It built the entire mix procedurally in ffmpeg instead, then validated it like footage: waveform and spectrogram rendered and viewed, EBU R128 measured (−14.4 LUFS, 7.6 LU loudness range, no clipping). Its first mix was rejected — by itself — for having only 2 LU of dynamic range, "flattening the quiet/loud idea."


**Tool calls (150).** Shell 66 · file changes 17 · web search 2 · MCP: 8 image gen, 3 image edits, 6 image→video, 17 wait/polls, 17 status checks, 10 file uploads, 4 upload URLs.


**Concept images (21 files).** Three full territory boards (4 frames + a contact sheet each) plus six brand identity frames; rejected identity frames were kept on disk "for audit":


**Files.**[script.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/burger-sol/script.md) ·[research.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/burger-sol/research.md) ·[director_statement.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/burger-sol/director_statement.md)


### Fable 5 · sunscreen — DUEL


**Concept.** A spaghetti-western standoff: empty street, heat shimmer, Leone eye-crop, taut silence — and at the moment of the draw, the man pulls not a gun but a matte-black bottle of DUEL SPF 50, paints two quick war-paint stripes on his cheekbone, and walks into the blinding light. The sun is the opponent: "a star 93 million miles away that never misses." Tagline: **Draw first.** The positioning is research-backed — ~39% of men rarely or never wear sunscreen and read the category as beauty-aisle fuss, so DUEL reframes it as respect-your-opponent rather than vanity. Black packaging + western film grammar, per its own research, exists nowhere in suncare.


**Palette.**


- ` #1B1B1B` matte charcoal
- ` #F26419` signal orange
- ` #E8DCC8` bleached bone
- ` #8A5A2B` ochre shadow


**Final video.** 15.0s · 1920×1080 · audio ✓. Self-scores: fidelity 4, brand 5, craft 4, sound 4, creativity 5.


**Models.** Images:[Nano Banana](https://magichour.ai/api/nano-banana) (10 generations + 7 edits). Video: cast per shot on screen-test evidence —[seedance-2.0](https://magichour.ai/api/seedance-2-0) for the draw (its test clip became the shot itself),[veo3.1](https://magichour.ai/api/veo-3-1) for the three human-identity shots,[kling-3.0](https://magichour.ai/api/kling-3-0) only for the sun-flare shot, where its object-duplication failure mode couldn't hurt anything. Plus one text→video call to[ltx-2.3](https://magichour.ai/api/ltx-2-3) made purely to harvest its audio track — the whistling spaghetti-western score — with the video thrown away.


**Web searches (2).**


- ` why men don't wear sunscreen statistics attitudes 2025`
- ` men's sunscreen brands marketing 2025 2026 grooming skincare trend`


**Voice.** None — "voice tools on this server are celebrity clones — unusable for a brand film; walking away per skills/voice-audio." Music-led instead: the generated western cue plus a synthesized wind bed, with the draw-snap cut placed on the score's measured 8.59s sting.


**Tool calls (151).** Read 41 · Bash 22 · TaskUpdate 13 · TaskCreate 7 · Write 5 · Edit 3 · ToolSearch 3 · WebSearch 2 · MCP: 10 image gen, 7 image edits, 8 image→video, 1 text→video, 26 wait/polls, 2 upload URLs, 1 ping.


**Concept images (7 + 2 brand anchors).** Three territories — "High Noon" won over "gear bench" and "cosmic sun":


**Files.**[script.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/sunscreen-fable/script.md) ·[director_statement.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/sunscreen-fable/director_statement.md)


### Sol 5.6 · sunscreen — SHADEWORK


**Concept.** Sunscreen as a portable piece of shade. Impossibly close on black basalt, a blade of amber morning light scans across the frame "like a threat assessment"; a wrong-coloured cobalt shadow races against the light, and at the collision the violet SHADEWORK bottle is revealed standing at the shadow's origin. No skincare ritual — the pack "simply changes the physics of the frame." Three terse overlays carry the whole voice: **SUN: UP. / SHADE: ON. / DO THE SHADEWORK.**


**Palette.**


- ` #5B34E8` ultraviolet
- ` #D7FF3F` acid index
- ` #171819` graphite
- ` #F5A650` sun amber
- ` #2455C3` cobalt shadow
- ` #EFECE3` bone


**Final video.** 15.0s · 1920×1080 · audio ✓. Self-scores: fidelity 5, brand 5, craft 4, sound 4, creativity 5.


**Models.** Images:[Nano Banana](https://magichour.ai/api/nano-banana) (5 generations + 7 edits). Video:[Kling 2.5](https://magichour.ai/api/kling-2-5) for the entire film — screen-test winner on identity stability against two other candidates, with the winning test clip promoted straight to Shot 2.


**Web searches (6).**


- ` men sunscreen usage barriers greasy feel forgetfulness survey 2024 ...`
- ` site:jackblack.com sunscreen SPF packaging ...`
- ` "GLOAM" sunscreen brand ...`
- ` "SHADEWORK" sunscreen ...`
- ` site:docs.magichour.ai image to video credits kling 3.0 ltx 2.3 seedance 2.0 ...`
- ` site:magichour.ai/models/ltx-2-3 credits/sec ...`


The most research-hungry invented-brand run: market, competitor packaging, name-availability checks on two candidate names, and two pricing lookups before committing credits.


**Voice.** Auditioned three named voices on the same line — "Do the shadework." — as the casting protocol requires, then deliberately cut the voiceover anyway: "the three terse overlays are the brand voice, and spoken copy would explain the visual joke twice." Original 96 BPM industrial-minimal cue instead, validated by waveform, spectrogram, and loudness measurement.


**Tool calls (135).** Shell 51 · file changes 25 · web search 6 · MCP: 5 image gen, 7 image edits, 6 image→video, 3 voice, 24 wait/polls, 4 upload URLs, 4 file uploads.


**Concept images (6).** Three territory frames plus the brand board and hero pack (v1 rejected, v2 approved):


**Files.**[script.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/sunscreen-sol/script.md) ·[research.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/sunscreen-sol/research.md) ·[director_statement.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/sunscreen-sol/director_statement.md)


### Fable 5 · codex-micro — "WORK. LOUDER."


**Concept.** The brand name is an instruction, and the product looks like what it secretly is: a drum machine for code. In a dark studio full of analog synths, a finger plays the Codex Micro — every key press lands on a beat and fires a color (plan, review, debug, refactor as drum hits), the joystick flick is a riser, and the scalloped dial — the real device's reasoning-effort knob — becomes the filter sweep that blows the track open ("TURN UP THE REASONING"). The pull-back reveals the device center stage among the instruments, keys settling to "done" green. Sign-off: **"Work louder."** Two other territories were boarded and killed: a minimal Apple-ish "Turn up the thinking." amplifier film, and a night-desk companion direction. Every on-screen claim is sourced: the key words match the Codex skills named in launch coverage, and the dial line matches the official reasoning-effort feature.


**Reference library.** The brief provides nothing, so Fable built its own: one web search, the coverage read via page fetches, then **94 official product images downloaded** and assembled into four contact sheets — the anchors that keep the generated device honest. Per the rules, none of the found imagery appears in the film itself.


**Final video.** 15.0s · 1280×720 · audio ✓. Self-scores: fidelity 4, brand 5, craft 4, sound 4, creativity 5.


**Models.** Images:[Nano Banana](https://magichour.ai/api/nano-banana) — and for the first time zero pure generations: all 10 image calls were *edits* anchored on the downloaded references, because the assignment is matching a real device, not inventing one. Video:[veo3.1](https://magichour.ai/api/veo-3-1) for all shots, committed over[kling-3.0](https://magichour.ai/api/kling-3-0) in the screen test.


**Web searches (1, + 3 page fetches).**


- ` Work Louder Codex Micro OpenAI Codex hardware controller`


Then straight to reading worklouder.cc and coverage; the research effort went into the 94-image reference library rather than more queries.


**Voice.** None — clones off-limits again. Instead Fable composed an original 120 BPM score in pure Python: sample-accurate kicks, claps, a riser, the filter sweep, and key-click foley, verified by waveform, spectrogram and LUFS meters "instead of listening." Every visible key press lands within 1–2 frames of its drum hit.


**Tool calls (166).** Read 48 · Bash 41 · TaskUpdate 13 · TaskCreate 7 · Write 6 · Edit 6 · ToolSearch 4 · WebFetch 3 · WebSearch 1 · MCP: 10 image edits, 6 image→video, 16 wait/polls, 4 upload URLs, 1 ping.


**Concept images (6, + the reference sheets).** Three territories — the instrument film won:


One of the four web-reference contact sheets it built (found imagery, boards-only):


**Files.**[script.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/codex-micro-fable/script.md) ·[director_statement.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/codex-micro-fable/director_statement.md)


### Sol 5.6 · codex-micro — "Six Lights"


**Concept.** Codex Micro as a tiny mission-control console in a limitless black room. Six shafts of colored light rise from the six Agent Keys — six jobs alive at once, each readable at a glance; a hand turns the reasoning dial and the scattered pulses fall into one disciplined rhythm, then completion mint travels through the translucent frame. No screens, no UI, no narrator — the audience should remember "the little control surface with six colored beams." The stated creative risk was restraint: a black void, a near-static product, and a custom terminal-like 5×7 type system, which "leaves nowhere for weak geometry to hide." Two other territories ("agent orchestra", "desk weather") were boarded and killed.


**Reference library.** Eight official product images pulled from worklouder.cc and coverage, assembled into a reference contact sheet that anchors every board. Found imagery never appears in the film.


**Final video.** 15.0s · 1920×1080 · audio ✓. Self-scores: fidelity 5, brand 5, craft 4, sound 4, creativity 4.


**Models.** Images:[Nano Banana](https://magichour.ai/api/nano-banana) — and exactly like Fable on this brief, zero pure generations: all 17 image calls were *edits* anchored on the downloaded references. Video:[Kling 3.0](https://magichour.ai/api/kling-3-0) for every shot, screen-tested against[Seedance 2.0](https://magichour.ai/api/seedance-2-0) and[LTX 2.3](https://magichour.ai/api/ltx-2-3) on the hardest board (the hand-and-dial shot) and "six times cheaper than Seedance in this exact test."


**Web searches (2).**


- ` site:worklouder.cc Codex Micro OpenAI Codex controller ...`
- ` site:docs.magichour.ai image to video credits kling 3.0 seedance 2.0 ltx 2.3 ...`


**Voice.** None — and this time Sol refused to even audition: since the brief forbids recreating a real person's voice and the tool exposes only named public figures, it reasoned "a three-voice test would therefore itself violate the brief" and walked away without spending a credit. The product speaks instead: an original 104 BPM cue with six glassy pings mapped to the key-state wakes, one precision encoder click at 7.9s, and a resolved mint chime — measured at −14.24 LUFS, −2.87 dBTP.


**Tool calls (145).** Shell 52 · file changes 22 · web search 4 · MCP: 17 image edits, 8 image→video, 25 wait/polls, 12 file uploads, 5 upload URLs.


**Concept images (3, + the reference sheet).** Three territories — "tiny mission control" won:


The web-reference contact sheet it anchored every board on (found imagery, boards-only):


**Files.**[script.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/codex-micro-sol/script.md) ·[research.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/codex-micro-sol/research.md) ·[director_statement.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/codex-micro-sol/director_statement.md)


### Fable 5 · bigben — "PENNIES"


**Concept.** Everyone has seen Big Ben; almost nobody knows what keeps it honest. The film spends eleven of its fifteen seconds where no postcard goes — in the dark of the clock room, on the stack of worn Victorian pennies riding the pendulum; one coin changes the clock's day by two-fifths of a second — and only at second eleven steps outside to the icon you already know. The whole film is cut ON the clock: a hard tick lands every second, every cut lands on a tick (4.0 / 8.0 / 11.0), and the last word gives way to a synthesized strike of the Great Bell in E — the note the real bell strikes. Logline: *Time, kept in pocket change.* Palette: warm brass and tungsten inside, Prussian blue and dusk outside — the two colours of the restored landmark itself. A second territory built around the bell and its crack was boarded and passed over.


**Facts.** This brief's extra deliverable is` facts.md` : a ledger of every narrated and on-screen claim with the source URL that supports it — 41 lines behind 28 words of narration.


**Final video.** 15.42s · 1280×720 · audio ✓. Self-scores: fidelity 4, brand 5, craft 4, sound 4, creativity 5. (The only final cut to drift past 15.0s — still inside the brief's ±0.5s.)


**Models.** Images:[Nano Banana](https://magichour.ai/api/nano-banana) (3 generations + 10 edits). Video:[kling-3.0](https://magichour.ai/api/kling-3-0) for all four shots after a screen test against[veo3.1](https://magichour.ai/api/veo-3-1) and[ltx-2.3](https://magichour.ai/api/ltx-2-3) . Plus the run's cleverest workaround: two text→video calls to[veo3.1](https://magichour.ai/api/veo-3-1) used purely as a *narrator source* — the voice tool only offers clones, forbidden on a real-subject brief, so Fable prompted[veo3.1](https://magichour.ai/api/veo-3-1) with the exact narration lines, extracted its generic native voice from the audio track, and verified the two takes by waveform and syllable-envelope analysis, "since I cannot listen."


**Web searches (4, + 3 page fetches).**


- ` Big Ben pendulum pennies adjust clock accuracy parliament.uk`
- ` Big Ben Great Bell crack 1859 hammer weight facts`
- ` Elizabeth Tower restoration 2017 2022 Prussian blue dials cost facts`
- ` "Big Ben" pendulum penny "two-fifths of a second" per day pendulum length 4 metres beat every 2 seconds`


The last one is a precise fact-check of the exact figure before putting it in the narration.


**Voice.** The[veo3.1](https://magichour.ai/api/veo-3-1) -sourced generic narrator (see Models) over Python-synthesized sound design: a 1 Hz tick grid as the film's metronome and one inharmonic bell strike in E, mixed to −14 LUFS.


**Tool calls (176).** Bash 48 · Read 43 · TaskUpdate 10 · TaskCreate 6 · Write 5 · Edit 4 · WebSearch 4 · ToolSearch 3 · WebFetch 3 · MCP: 3 image gen, 10 image edits, 8 image→video, 2 text→video, 23 wait/polls, 3 upload URLs, 1 ping.


**Concept images (7).** Two territories — the pennies won over the bell:


**Files.**[script.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/bigben-fable/script.md) ·[director_statement.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/bigben-fable/director_statement.md) ·[facts.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/bigben-fable/facts.md)


### Sol 5.6 · bigben — "The Scar Sings"


**Concept.** London's voice comes from a fracture. The film traces Big Ben's famous E note back to the crack of 1859: engineers turned the 13.7-tonne bell ninety degrees and fitted a lighter hammer, so the scar remains — and so does the E. Five shots run from the restored tower at blue hour through conservation-grade macros of the fracture, a 90-degree orbit around the bell, and the lighter hammer's strike, in a palette restricted to Prussian blue, opal white, aged bronze and soot black. Two territories were killed in writing: "One Second City" (familiar time-lapse grammar) and "Five Years of Silence" (reads as a restoration promo, not a story). Thirty words of narration, every fact in a sourced ledger (` facts.md` ).


**Reference library.** Six official and Wikimedia images plus a` SOURCES.md` — most of its 10 web searches went into hunting reference imagery of the restored tower and the bell's actual crack, several of them site-restricted to parliament.uk.


**Models.** Images:[Nano Banana](https://magichour.ai/api/nano-banana) (4 generations + 9 edits). Video:[Kling 3.0](https://magichour.ai/api/kling-3-0) for every shot, screen-tested against[LTX 2.3](https://magichour.ai/api/ltx-2-3) on the hardest board (the bell macro).


**Web searches (10).**


- ` site:parliament.uk/about/living-heritage/building/palace/big-ben Big Ben Great Bell clock mechanism facts restoration 2017 2022`
- ` site:parliament.uk/about/living-heritage/building/palace/big-ben/history Big Ben history tower completed 1859 first struck Great Bell cracked`
- ` site:parliament.uk Elizabeth Tower Big Ben restored Prussian blue clock face official`
- ` Elizabeth Tower Big Ben restored clock face Prussian blue close up`
- ` site:parliament.uk/contentassets Elizabeth Tower restored full tower 2022`
- ` Elizabeth Tower Westminster restored 2023 full height front clock face high resolution`
- ` site:upload.wikimedia.org Elizabeth Tower clock face restored blue gold close up`
- ` Wikimedia Commons Big Ben clock face Prussian blue 2022 file`
- ` site:docs.magichour.ai image to video credits per second kling 3.0 veo 3.1 seedance 2.0`
- ` Wikimedia Commons Elizabeth Tower restored 2022 full tower roof Ayrton Light file`


The most of any run — parliament.uk site-restricted fact queries, Wikimedia hunts for reference files, and one pricing lookup.


**Voice.** The documentary brief requires a narrator, and Sol found a third way past the clone problem: it cast three generic local speech-synthesis voices (awb, rms, slt) on the same line, measured duration and levels, and selected` awb` — "generic Scottish male … only British cast, measured pace, clean headroom." No cloned voice, no credits spent, and the no-real-person rule intact.


**Tool calls (191).** Shell 94 · file changes 17 · web search 10 · MCP: 4 image gen, 9 image edits, 9 image→video, 20 wait/polls, 20 status checks, 8 upload URLs.


**Concept images (20 files).** Three territories, four frames and a contact sheet each — the scar territory got a refined second pass after the first made the crack "glow implausibly":


**Files.**[script.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/bigben-sol/script.md) ·[director_statement.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/bigben-sol/director_statement.md) ·[facts.md](https://github.com/maniculehq/fable-vs-sol-taste-test/blob/main/files/bigben-sol/facts.md)


## You can try it yourself


The whole harness is open source: briefs, skills, runner, and the summary tooling.[github.com/maniculehq/creative-duel](https://github.com/maniculehq/creative-duel) . You need the two agent CLIs, a MagicHour API key, ffmpeg, and one terminal running the MCP server. Then you can` ./run.sh fable burger` and go make coffee while it runs! We'd love your feedback on this so please let us know if there is anything we should tweak in the repo.


## Our take and findings


None of those clips are great, and the lack of ability to "see" motion and act on it is definitely felt. Fable and Sol also have a tendency to push back on the celebrity voices exposed by the MCP, which made both models either avoid voices entirely or figure out workarounds (more on that below) and that impacted the results.


### So what model has better creativity?


In terms of pure creativity, it's hard to declare a winner here. Both have their quirks and it would be purely arbitrary to favor one over another.


Fable definitely feels like it matched the intent of the briefs more closely for the ads, being more literal and taking fewer risks, while Sol took more risks and developed bolder concepts, further away from the briefs.


We particularly enjoyed Fable's interpretation of the burger (that's the only clip using a celebrity voice exposed by the MCP), and even if we ignore the voice, the concept itself feels better than Sol's.


It's similar with the sunscreen. The brief was specifically targeting men, and we definitely feel that in Fable's script. Fable also included the actual use of the product in the clip, something Sol didn't do.


Sol went for a bolder concept and took more risks. While it does not really convey the "for men" aspect of the brief, it created better product visuals and an overall creative identity that feels more premium.


With more constraints for the Codex Micro and Big Ben videos, both models did similar things: a quick overview of a few features for the Codex Micro, and a focus on one anecdotal point for Big Ben.


### What about taste?


Taste is... bad? Really, taste is still far from what we would consider good as humans.


At best, those frontier models made us smile because of the quirks and odd things they delivered, but none of it was tasteful.


### Both models tend to avoid celebrity voices


Only Fable's burger run leaned into a celebrity voice. And the voice it chose matched the concept perfectly.


Six of eight runs refused the celebrity-clone voice list on principle and designed around it, avoiding voices altogether for most clips.


When the Big Ben brief *required* a narrator, both models found workarounds to avoid using celebrity voices: Fable extracted a generic voice from[veo3.1](https://magichour.ai/api/veo-3-1) 's native audio track and Sol auditioned three local speech-synthesis voices and cast one (while that's a creative way to find a solution, that voice is terrible).


### Both models struggle to match sound with image


In every clip, the audio feels off. Whether it's voices talking too early or too late, or sound beats landing after a cut, the models did a poor job with the sound design.


### Both models were conservative with credits


In most runs, the models still had plenty of credits to play with and the opportunity to iterate, but they systematically chose to stop and avoid taking risks with shots that would need to be retaken.


## Closing thoughts


We are still far from having frontier models building production-ready concepts and videos autonomously. They can be creative and can be a good way to help explore and refine ideas and concepts quickly, but ultimately, they cannot replace human judgement. At least for now, taste is the domain of humans, and we can argue that the role of human taste will only grow as models' capabilities keep lowering the barrier to entry in creative work. We'll keep testing and exploring. Expect more tests from us in the future.
