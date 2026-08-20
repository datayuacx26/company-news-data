---
schema_version: "1.0.0"
document_id: "509398d3e6373acd8cf5312bb78b167c03a66d057022a3a98d4582fb05f32117"
company_key: "cargurus-inc-class-a-common-stock"
company: "CarGurus Inc."
source_id: "cargurus-inc-class-a-common-stock-rss-ea326c83890f"
canonical_url: "https://cargurus.dev/2026/04/29/vins-the-encoding-stamped-into-steel/"
published_at: "2026-04-29T14:54:49+00:00"
first_seen_at: "2026-07-20T03:33:13.583792+00:00"
fetched_at: "2026-07-28T21:56:02.721351+00:00"
content_hash: "sha256:ce28c573508ee3b13b13d309bbd3004153f20905f22327a34580a055ff9e35ec"
---

# VINs: The Encoding Stamped Into Steel

Do I have a favorite[standard](https://en.wikipedia.org/wiki/Technical_standard) ? Silly question. How could I not? There are so many great ones, each with its own weirdly fascinating story.


In a previous job, I spent a lot of time getting to know ECMA-262 (the JavaScript language specification). Lately, though, I’ve been acquiring a taste for encoding standards. Encodings are fascinating. They’re all about constraints and information density. How much can you pack into a fixed space? What can you safely leave out? and what existing patterns do you need to work around?


Of course, encodings only work if everyone agrees to use them. Check out the later chapters of Jing Tsu’s excellent[Kingdom of Characters](https://en.wikipedia.org/wiki/Kingdom_of_Characters) for a reminder that even a clever, seemingly-dry technical standard like Unicode can turn into a messy game of politics the moment the whole world has to agree.


But in the automotive world, it’s hard to beat[ISO 3779](https://cdn.standards.iteh.ai/samples/52200/7d8a69aee84c4ad28231053f49f4966e/ISO-3779-2009.pdf) : the international standard for the humble Vehicle Identification Number (VIN). If anyone ever writes the tell-all book about how that standard came to be, I’ll be the first to preorder it.


Because a VIN is so much more than a serial number. It’s an encoding: a compact little artifact that carries pieces of a vehicle’s manufacturing story. Why would we want that? Let me tell you a story…


## **Alice and the bicycle shop**


Imagine a small town with a local bicycle shop. The shop owner, Alice, makes custom bikes. Alice is proud of the quality of her work and decides to offer a 1 year warranty on her bikes. To accomplish this she needs to keep track of her bikes to know if they are still under warranty. At first, she only needs to keep track of a few bikes at a time, so she uses a simple numbering system: Bike 1, Bike 2, Bike 3, and so on.


As Alice’s business grows, she starts making different types of bikes: mountain bikes, road bikes, and city bikes. She also begins to source parts from various suppliers. She grows tired of looking up bike numbers from her record to understand what they are and when they were manufactured. So she creates a more sophisticated system to track not just the number of bikes, but also their types, components, and manufacturing dates.


Alice develops a new identification system. Each bike now gets a 10-character code:


- 2 letters for the bike type (MB for mountain bike, RB for road bike, CB for city bike)
- 4 numbers for the date of manufacture (MMYY)
- 4 numbers for the sequential production number


So, a mountain bike made in March 2023, being the 15th bike of that type, would be: MB032300015.


A month later, a customer rolls in with a bike whose basket has come loose. Alice reads the tag: CB062500128. She immediately knows it’s a city bike built in June 2025, still within warranty, and later she can pull the exact build sheet to see which basket it shipped with. Alice still needs her records, but the code gives her enough context to answer the warranty question without flipping through paperwork.


Alice isn’t alone in discovering this pattern. Serial numbers for high-value manufactured products are common across industries, from power tools to medical devices to industrial equipment. They make it easier to trace inventory, manage warranties and service history, fight counterfeits, support theft recovery, and do targeted recalls when something goes wrong.


## **From serial numbers to VINs**


In the automotive world, serial numbers evolved into something more sophisticated: the VIN (vehicle identification number). In the United States, we follow ISO 3779’s structure, but[U.S. regulations](https://www.ecfr.gov/current/title-49/subtitle-B/chapter-V/part-565) are more prescriptive about how some of those fields are used. And that’s where VINs get fun: they’re not just serial numbers, they’re a shared contract, something thousands of independent companies can read, type, validate, and exchange without needing a central database.


A VIN is a 17-character code, broken into sections:


- **Positions 1-3 (World Manufacturer Identifier):** Identifies the manufacturer, and the first character is commonly used as a country/region-of-origin indicator. For example, vehicles with VINs starting with **1/4/5** are associated with the United States, while **J** is Japan.
- **Positions 4-8 (Vehicle Descriptor Section):** Platform, model, body style, and often the engine type when multiple options exist.
- **Position 9:** Check digit (more on this in a moment)
- **Position 10:** Model year
- **Position 11:** Assembly plant code
- **Positions 12-17:** Sequential production number for that model at that plant in that year


### **Designed for the real world**


The letters O (o), I (i), and Q (q) never appear in VINs. VINs get handwritten on insurance forms, read aloud over phone calls to DMVs, transcribed from photos of dashboard plates. By excluding characters that are easily confused with numerals (I/1, O/0, Q/9), the designers eliminated an entire class of transcription errors. The VIN standard prioritizes surviving the messy real world.


### **The check digit**


Position 9 is a check digit, calculated using a weighted sum algorithm (mod 11). Each position in the VIN has an assigned weight, letters using a translation table from the standard, and the remainder modulo 11 becomes the check digit with the value of 10 represented as X.


This helps detect the most common errors when copying vins such as single-character errors or transpositions. If someone mistypes one digit or swaps two adjacent characters, the check digit won’t match. The VIN itself tells you it’s wrong without requiring a database in the loop. This is the same pattern found in credit card numbers (Luhn algorithm) and ISBNs.


Interestingly, ISO 3779 doesn’t require the check digit. However, it’s mandatory in the US, but far less common in European VINs.


### **The model year trade-off**


Position 10 encodes the model year using a single character that cycles through letters and numbers. The “epoch” is 1980 (when the 17-character VIN became standard), so A = 1980, B = 1981, and so on. But if you paid attention in your information theory class you may have noticed a problem. With only 30 usable characters (remember, I, O, Q are excluded), it’s impossible to encode every year. A meant 1980… and then meant 2010… and it will mean 2040.


This is common encoding trade-off: one character keeps the VIN compact and fixed-width, but you need context to disambiguate. In practice, this doesn’t cause too much trouble. The VDS section provides context (a 2010 Camry vs. a 1980 Camry have different model codes), and wear, regulations, and inspection requirements mean that its rare to find a 30+ year-old vehicle on the road.


### **Plants and sequences**


The 11th digit identifies the assembly plant, but there’s no global registry. Manufacturers define their own plant codes, which gives the system flexibility as plants open, close, and get reassigned. This helps the standard avoid becoming a bottleneck, but the trade-off is that you need manufacturer-specific lookups to interpret this position.


The last six positions (12-17) hold the sequential production number. A theoretical max of one million vehicles per model/plant/year. That may sound like plenty, but Ford has the capacity to make over 700,000 F-150s in a year. Fortunately, most high-volume models are built across multiple assembly plants, each with its own plant code and production sequence.


### **What’s NOT in the VIN**


As much as I’d love for the VIN to encode more, there are some things it deliberately leaves out. Trim level, color, options packages, or title status.


The VIN is designed to be immutable. It is literally stamped into the frame, recorded in government databases, referenced in insurance policies for decades. Trim and options aren’t always finalized at VIN assignment time. Color can change at a body shop and a vehicle’s title status (accidents, salvage, theft recovery) changes on a long enough timeline. Despite all that, the VIN stays stable, acting as a globally unique key that other systems build on.


## **How CarGurus Operationalizes VINs**


At CarGurus, the VIN is more than a technical detail, it’s core to how we make vehicle listings comparable and trustworthy. We require a VIN for every listing because it’s the best way to uniquely identify a specific vehicle and connect it to the information shoppers care about.


#### **Filling in what the VIN leaves out: trim**


Trim level isn’t encoded in the VIN, but it matters. A base-model Honda Accord and a fully-loaded Touring trim can differ by $10,000+ in value. Buyers care deeply about this distinction.


To close that gap, we use our historical listing data to infer trim when we’ve seen that VIN before. Trim is typically stable for a given vehicle over its life (unlike things like title status), so it’s a great example of “not in the VIN, but still knowable.”


#### **Vehicle history: lifecycle facts that change over time**


Title and damage history are another deliberate omission. A VIN encodes the vehicle as it left the factory, but accidents, theft recovery, flood damage, and salvage branding happen after manufacturing. These events can slash resale value by 20–50%, but they aren’t part of the VIN because they can change over a vehicle’s lifetime.


When title defects occur, they’re reported to state DMVs, NMVTIS, and other databases, all using the VIN as their primary key. That’s what enables coordination across this fragmented landscape. Third-party aggregators compile the data, and at CarGurus, we partner with these providers to ensure our listings reflect a vehicle’s history as accurately as possible.


Online marketplaces didn’t exist when the first edition of ISO 3779 was published in 1976. But the VIN gives us what standards do best: a stable foundation that enables coordination at scale. We build the rest from there.


### Share this:


- [Share on X (Opens in new window) X](https://cargurus.dev/2026/04/29/vins-the-encoding-stamped-into-steel/?share=twitter)
- [Share on Facebook (Opens in new window) Facebook](https://cargurus.dev/2026/04/29/vins-the-encoding-stamped-into-steel/?share=facebook)
-


### Like this:


Like


Loading…
