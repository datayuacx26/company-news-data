---
schema_version: "1.0.0"
document_id: "2d1e980062e6b78b054bf2196aed62a0c7b82a4a46d864a6f8826f8d9088ca75"
company_key: "yc-howdy-com"
company: "Howdy.com"
source_id: "yc-howdy-com-news-import-7de6c48bfb23"
canonical_url: "https://www.howdylatam.com/blog/ruby-sur-meetup-recap-buenos-aires"
published_at: "2026-07-30T21:15:04.720+00:00"
first_seen_at: "2026-07-31T23:44:21.659856+00:00"
fetched_at: "2026-07-31T23:44:22.904459+00:00"
content_hash: "sha256:9e9d5c057e79040dcd2a4399e04df9b8d09523978e71a1e840f46aa6c8de1901"
---

# Ruby Sur Meetup: The Real Cost of Your Primary Key and the AI That Already Codes on Its Own

On July 23rd, we met up again with the Ruby community in Buenos Aires. We sponsored the Ruby Sur Meetup at La Maquinita Innovation Lab, the same space where we had already been with the React community a few weeks earlier. With the food, the screens, and the venue covered, the night was set for two talks that ended up raising more questions than anyone expected.


## **The Real Cost of Choosing the Wrong Primary Key**


Santiago Merlo, Rails Senior Developer at Rootstrap, opened the night with a talk that started as a hallway argument: a colleague of his was working on a legacy project using UUID as a primary key, and the decision struck Santiago as odd. Instead of leaving it at that, he ran the benchmarks himself.


The numbers he showed were hard to argue with. Using UUID version four as a primary key in Postgres, compared to a traditional BIGINT, means losing more than forty percent of write speed and picking up as much as fifty percent fragmentation in the B-tree index. UUID version seven improves that picture quite a bit, but it still takes up twice the space of an integer. The approach that won him over was a hybrid one: use a BIGINT as the internal primary key in the database, and expose a UUID on the outside so you are not leaking information like record volume or creation order.


The talk pulled in the kind of technical questions that make a good Q&A: what happens with ID collisions during a blue-green deployment, whether a dedicated microservice just for generating unique UUIDs is worth it, and even a story about a UK pet registration system that uses four-character strings to write roman numerals, and runs out of combinations before it gets anywhere near forty.


## **When a Client Would Rather Pay for Tokens Than for Salaries**


The second talk came from Patricio Mac Adden, co-founder of Sinaptia, and it opened with a story that quieted the room: a few months ago, a client of his company said it was cutting its human team because it would rather spend that budget on AI tokens than on people. The client wanted to shift development toward specs and agents working on their own, without anyone looking over their shoulder.


That crisis is where the talk came from. Patricio walked through how we went from Copilot autocomplete to agents that can now plan, write code, and even fix their own mistakes, and why that changes the rules of the game: generating code is already cheap, but getting it wrong still costs the same or more. He talked about vibe coding versus spec driven development, about harness engineering, and about testing several tools himself, Claude Code, OpenCode, minimalist agents like Pi, until he found the one that cut the friction without taking away control.


His core piece of advice was simple to say and hard to live by: do not outsource the thinking. You can let the agent write the code, but the planning and the understanding of why something is being built have to stay yours. That same logic carried into the Q&A: use project-local skills instead of global ones, so you do not end up with the same consistency problems you already had with large human teams.


## **What the Community Took Home**


Ruby Sur is a community with more than thirty meetups and several thousand mates behind it, and this edition added another night of talks worth showing up for, plus food and networking that ran late. Before wrapping up, there was a raffle for a JBL speaker among the people who stuck around until the end.


The next day, the Ruby Sur team publicly thanked Howdy for the sponsorship on their own social media, and they are already thinking about the next edition. So are we. When a talk about primary keys ends with the room asking real architecture questions, and a talk about AI ends with the room rethinking how they work every day, the meetup did its job.
