---
schema_version: "1.0.0"
document_id: "3ce32fa2e5d6909eb3e29dba6700764ea0c9eac91c4c61b02dfa157c0bf0e72e"
company_key: "yc-alkali"
company: "Alkali"
source_id: "yc-alkali-news-import-7da3f732694e"
canonical_url: "https://www.alkali.engineering/blog/automated-column-detection-beta"
published_at: null
first_seen_at: "2026-07-21T05:51:46.944392+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:f76961ce92adb8513fc9907b46b46ed5efcfd115504b18ea08b1edf9022ea9ce"
---

# Automated Column Detection (Beta)

## Estimating Column Weight


Estimating the steel mass of columns in a project can be tricky and time consuming. There are three key aspects to doing it properly:


1. **Finding the columns** : Identify column locations on the plan. They often start at the foundation, but not always!
2. **Identifying sizes** : W-shapes, HSS, pipes.
3. **Determining heights** : How tall is each column? (This is the hardest part)


What Makes This Hard


Column information is scattered across multiple drawings:


- **Foundation plans:** Where columns are located
- **Foundation details:** Embed depth below finished floor
- **Framing plans:** Top of steel elevations
- **Column schedules:** Sizes and sometimes heights
- **Elevation plans:** Floor-to-floor dimensions (structural and architectural)


At Alkali, we've built an automated way to find and size columns — it's in pilot, so your feedback helps. Results should be verified by an estimator!


[Try Alkali Here](https://www.alkali.engineering/viewer)


---


## Two Approaches - One Button


The location and type of column information varies widely. Some larger projects have detailed column schedules with heights already calculated. Others require piecing together information from multiple sheets, even architectural drawings.


Start Takeoff


Scan for:


Beams


Bracing


Columns


(Pilot)


Base Plates


(Pilot)


Continue


With a single button, Alkali works to put the pieces together, identifying the columns, beam types, and heights, and adding them to your takeoff.


---


## Mode 1: Column Schedule with Heights


When the drawings include a column schedule that lists sizes AND heights, Alkali can extract column data in one step.


(Column schedule with marks, sizes, and heights)


### What Alkali Does


1. **Finds relevant column schedules** : Locates column schedules in the drawings that include both size and height information.
2. **Scans the column schedule** : Identifies column marks and their corresponding heights and sizes directly from the schedule.
3. **Calibrates a scale** : Determines an absolute scale from the drawing to accurately measure column heights.
4. **Extracts steel shapes** : Recognizes the structural section type, such as W-shapes or HSS tubing, from the schedule.
5. **Detects multiplicity** : Finds and double-counts marks that appear in multiple locations (e.g., annotations found at both D-7 and G-7 will both be included).


### What You Get


These new columns appear in your main steel takeoff table alongside beams and bracing, and can be edited or deleted just like any other annotation.


---


## Mode 2: No Column Schedule (or No Heights)


Many projects have column schedules (we'd call them column tables) that only show steel shapes, but not heights. Or they have no schedule at all, with column sizes called out directly on the plan. To handle the variety of cases here, Alkali can dynamically search and scroll around the document to piece the relevant information together.


### What Alkali Does


- **Scan foundation and framing plans** : Alkali checks every plan — foundation, mezzanine, each floor, roof, even pop-up roofs — to spot all your columns. It look across all the drawings to see which columns show up on which levels so nothing gets missed.
- **Find floor elevations** : For each foundation or framing plan, Alkali reads elevation notes and callouts from the drawings themselves, like “foundation = 100'-0” or “roof = 129'-4”.
- **Calculate heights** : Once we know where each column shows up, we can figure out how tall it needs to be by comparing elevations the column exists on.


Where to Find Heights


- **Elevation markers:** TOS (Top of Steel), BOD (Bottom of Deck), FFL (Finished Floor Level)
- **Building sections:** Architectural sections showing floor-to-floor heights
- **Foundation details:** Show embed depth below finished floor (often 6"–12")


## Alkali Platform


Column detection joins our growing suite of automated takeoff tools:


- **Beam Scanner** : Detect and measure beams on framing plans
- **Real-time Collaboration** : Work with your team on the same takeoff
- **AI Chat** : Ask questions, search drawings, understand details


Reach out tofounders@alkali-eng.com to learn more, or try it below:


[Try Alkali Here](https://www.alkali.engineering/viewer)


---
