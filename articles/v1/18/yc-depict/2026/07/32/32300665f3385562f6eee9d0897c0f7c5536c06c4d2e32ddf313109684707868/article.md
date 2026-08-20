---
schema_version: "1.0.0"
document_id: "32300665f3385562f6eee9d0897c0f7c5536c06c4d2e32ddf313109684707868"
company_key: "yc-depict"
company: "Depict"
source_id: "yc-depict-news-import-c8e412319241"
canonical_url: "https://depict.ai/resources/guides/lean-teams-guide-to-discovery"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-21T16:08:19.972618+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:f1f70b8ac0d9ea785327e9a28b7e449e6dc42b6688812b302de43d9e0a38a3f4"
---

# The Lean Team's Guide to Discovery

## Written for the person doing five jobs at once


If you run ecommerce for a growing brand, you already know the gap between how this work is described and how it actually happens.


In the descriptions, there's a merchandising team. There's a search specialist. There's someone who owns international, someone who owns trading, someone who owns the site. Roles are clean and the org chart has depth.


In reality, for most brands between roughly £30m and £250m, there are three or four people. The head of ecommerce is also the merchandiser. The merchandiser is also the person managing the agency relationship, also the one writing the campaign brief, also the one who notices at 9pm that the new season collection still hasn't gone live. Trading, site, email, paid, discovery: all of it sits across a handful of people who are permanently choosing what not to do this week.


This guide is for that team. Not the enterprise with a hundred merchandisers, not the brand with a dedicated search function, but the lean team that has to do more with less, and has to trust that the tools they're paying for are pulling their weight rather than adding to the pile.


A word on the word. By discovery we mean everything involved in a shopper finding and choosing products on your site: search, collection and category pages, recommendations, and the behind-the-scenes work of keeping all of it stocked, ordered and on brand. It's industry shorthand, and like most shorthand it hides the fact that on a lean team, all of it is usually one person's job.


We build discovery software, and early on we made a deliberate bet: design for the lean team, not the enterprise model the rest of the market is built around. Everything here comes from watching teams like yours run search, merchandising and recommendations with a fraction of the people the tooling quietly assumes.


> “We built Depict for the team of three doing the work of ten. If a merchandising tool needs a full-time person to be good, the tool has failed. The work should get smaller as the software gets better.”


Josep Nolla


, CEO, Depict


The argument running through all of it is simple. Good discovery, the kind that helps shoppers find products and helps you trade the site, should not require a team you don't have. The work should get smaller as the tooling gets better. If it doesn't, the tool is the problem.


## Part one: the problem with how discovery is sold


### Most discovery tools are built for teams that don't exist


Open up most search and merchandising tools and they quietly assume someone.


A person whose job is to maintain synonym lists, so that a search for "jumper" returns the products tagged "knit". Someone to pin products to the top of a category, and then remember to unpin them when they sell through or fall out of season. Someone to tune relevance sliders for every category, in every market, week after week. Someone to spot that a product is getting plenty of traffic but converting badly, and actually do something about it.


That person is a luxury most teams don't have. The tooling market, though, hasn't caught up to that fact. It still designs for an idealised resourcing model, and then quietly hands the gap to whoever's already stretched.


You can see the assumption in the standard advice. When a search tool underperforms, the recommendation is almost always some version of "you'd benefit from having someone full time on this". For an enterprise, fine. For a lean team, that's not advice, it's a second invoice. Either you hire a person, or you hand 40 hours a month to managing the vendor's product, or you spend your development agency's day rate fixing an integration that should have worked out of the box.


We've heard the justification too: "but the big brands do so well with it". Of course they do. The big brands have the merchandising headcount to feed the machine. Pointing a lean team at an enterprise success story is comparing two completely different operating realities and pretending they're the same purchase.


### The hidden cost of "powerful"


Powerful sounds like the safe choice. More controls, more configurability, more levers to pull. The pitch is reassuring: you can do anything.


For a lean team, "you can do anything" quietly means "you have to do everything". Every lever is a lever someone has to pull. Every category is a category someone has to merchandise by hand. Every synonym is a rule someone has to remember to add, and later remember to remove. The configurability that reads as a strength on the sales call becomes a standing obligation the moment the contract is signed.


The pattern is consistent. A brand signs up to an enterprise-grade tool. Implementation alone runs to a week or two of development resource, which on agency day rates can quietly approach the annual cost of the software itself. Then the real cost arrives, which is the ongoing maintenance. The tool works, technically. It does everything it promised. But it assumes a level of resourcing the team never had, and so the gap gets filled by people who were already at capacity.


What happens next is predictable. The tool gets configured once, in a rush, during onboarding. Then it's left. Categories drift out of date. Search rules go stale. New products don't get merchandised properly because there's no time. The result that justified the spend never quite materialises, because nobody has the hours to maintain the thing that was supposed to deliver it. The brand ends up paying premium prices for a tool running at a fraction of its value, and slowly starts to resent it.


Powerful is only powerful if you have the team to wield it. For everyone else, it's overhead wearing a feature list.


The second invoice


What your discovery tooling really costs to run, from numbers you already know.


€ ··,···


### The other end of the market: when "good enough" is the competitor


There's a second pressure worth naming, because it's the one most discovery vendors don't like to talk about.


The free, native search that ships with most ecommerce platforms has got dramatically better. A few years ago, platform-native search was genuinely poor and the case for a dedicated tool made itself. Today, a shopper can search "black dress" on a stock store and get a reasonable result. They can search a product name and find the product. It isn't the best experience available, but it works, and it's free.


That matters for a lean team's decision-making. If the alternative to an expensive, maintenance-heavy tool is free and "fine", plenty of teams will quietly conclude that fine is enough, especially if the paid option needs a person they don't have to run it. Some brands have moved off paid search tools and reported that, for their catalogue, the native results felt about as accurate.


The lesson isn't that dedicated discovery is unnecessary. It's that the bar has moved. A discovery tool now has to clear two hurdles at once: be meaningfully better than free, and be lighter to run than the expensive incumbents. Clearing one isn't enough. A tool that's better but heavier loses to free on effort. A tool that's lighter but no better loses to free on price. The teams worth winning are the ones frustrated by the heavy, expensive end of the market but unwilling to settle for the free end, and the only way to win them is to be both better and lighter at the same time.


## Part two: how to run discovery when you're the whole team


The honest starting point is that your discovery work splits into three piles, and most lean teams spend their hours in the wrong one. There's maintenance that keeps the basics working. There's a small set of decisions that need your judgement. And there's planning, the work that would make next month calmer, which almost never happens because the first pile eats the time. Running discovery well as a small team is mostly about shrinking the first pile, protecting the second, and forcing the third to happen. Everything below is how.


### Sort the mechanical from the judgement


List your recurring discovery tasks and mark each one. Out-of-stock products dropping out of view, new-in surfacing, bestsellers rising, end-of-season lines demoting: mechanical, rule-based, and not work you should be doing by hand. Which three collections carry the brand this season, whether a hero product earns the top slot, how a campaign reads on the page: judgement, and only you can do it.


This is also the honest way to think about AI in this work. The right role for automation is to absorb the mechanical pile completely and leave the judgement pile entirely yours. It should amplify what's distinct about your brand, not average it away. If a tool's automation is making calls that belong to you, that's not intelligence, that's a loss of control. If it's asking you to hand-manage things a rule could handle, that's not control, that's chores.


The highest-leverage habit is refusing to spend judgement-hours on mechanical work. If you are pinning and unpinning products from memory, that is mechanical work wearing a trading hat, and it is the first thing to off-load. This is the clearest thing a good system takes off you.


### Watch the few signals that change a decision


You cannot read every report, so pick the handful that tell you to do something, and ignore the rest. Three usually cover it. A product with high visibility and weak conversion is a prompt to check price, imagery or position. A search term returning no results is a near-direct instruction on what to stock or how to tag. A collection with sliding engagement is a sign the merchandising has gone stale. Set a standing fifteen minutes a week to look at those three and act on one. The discipline is not gathering more data, it is acting on the little that matters.


### Run the holiday test on yourself


Ask whether your discovery would quietly degrade if you went away for two weeks. If yes, you have built a job you have to keep feeding, and the fix is to find and remove the manual rules creating that drag. Every synonym you hand-maintain and every weighting you set and have to remember is future work you have signed up for. Before you add a manual rule, ask whether you will remember to unwind it. The honest answer is usually no, which is the case for leaning on logic that maintains itself.


The holiday test


Take the holiday test properly. Six questions, sixty seconds, honest answers.


### Do the work in the quiet weeks


Map your discovery work to the trading calendar and pull it forward. Build the Black Friday collections in October while you have the headspace, and hold them. Draft the seasonal change before the season turns. The work is the same size either way; doing it early is what changes the texture of the job, so the peak weeks become the calm ones because the building already happened. Teams that trade on the front foot are not working less, they are working earlier.


### Spend your design effort where it shows


You cannot give every collection the editorial treatment, so decide which ones earn it. A small number of collections carry most of your revenue and most of your brand impression, and those deserve the imagery, the story, the white space. The long tail can run on a clean default. Knowing which is which, and being willing to leave most collections plain, is the practice. On brand is a set of choices about where to spend attention, not a switch you flip everywhere.


> “Since we started using Depict, we've been able to design and launch more visually appealing and dynamic collection pages, thanks to the intuitive content card and item duplication features.”


Nanushka


### Build it in private, publish when it's right


A close cousin of planning ahead: build and refine a collection out of sight, get it right, then publish a finished thing rather than editing live on the storefront while shoppers watch. For a team that cannot afford a visible mistake during peak, drafting privately removes a whole category of risk.


The same thinking applies below the collection level. Stage whole products ahead of launch wherever your platform supports it: a new drop can be loaded, tagged, photographed and sitting in draft weeks early, then switched on the morning it goes live. Launch day becomes flipping a toggle instead of doing the work under pressure.


None of this is about features. It is about deciding what not to do by hand, watching the few signals that matter, refusing to build work you will have to maintain, and getting ahead of the calendar. The right tooling earns its place by making those practices possible without a person to feed it. If a tool adds to these piles instead of shrinking them, it is the wrong tool, whatever the feature list says.


## Part three: search you don't have to babysit


Search is the part of discovery most likely to be quietly costing you money with nobody watching. Here is how to run it without it becoming a second job.


### Read what shoppers search for and don't find


The single most useful search habit is looking at your no-results and low-results queries. Each one is a shopper who wanted to buy and hit a wall, and each is a near-direct instruction: stock this, tag this, or rename that. Pull the list monthly. Most teams have never looked, and the first read is usually the highest-return hour you will spend on search all quarter.


### Know why search misses, so you stop patching it by hand


Traditional search matches words, not meaning. A shopper types "jumper", your catalogue says "knit", and unless someone built that link, they find nothing. The instinct is to add a synonym, then another, then a list you maintain forever in every language you sell in. That is the second job.


It's worth saying plainly that the synonym list was a reasonable answer to what used to be a hard problem, and that problem has since been solved a layer down. Modern search reads meaning rather than matching strings, and the best of it reads the product images too, so a descriptive query like "black leather trainers with white soles" can return exactly that without anyone having taught the system what those words mean. Treat a growing synonym list as a symptom, not a fix: if you are hand-feeding matches, you are maintaining yesterday's workaround as a permanent line item, and the fix is search that understands meaning, not a longer set of rules you tend.


### Read search as demand, not just navigation


What people type is the cheapest market research you have. A shopper searching "wedding guest" rather than "midi dress" is telling you the occasion. "Cold weather running" is telling you the use case. Skim your top queries for the language of intent, not just product names, and feed it back into how you stock, tag and write collections. It is insight a lean team would never have the hours to gather deliberately, sitting in a report you already hold.


### Judge it on your own catalogue before you commit


Not every catalogue needs the same search. A considered, high-attribute range may want richer, guided search; a simple one does not. So don't evaluate a search product on a demo store with a demo catalogue. Ask to see your own products in it, and bring your own evidence: your zero-results report, your top hundred queries, the searches you know your current setup fumbles. Add a few descriptive queries a real shopper would type, the "black leather trainers with white soles" kind, and see whether the results match the sentence or just the keywords. Run all of it and look at the results with your own eyes. A vendor confident in their search will be comfortable being judged on your catalogue rather than theirs.


## Part four: if you sell in more than one market


This only matters if you sell across markets, but if you do, it is where the hours quietly multiply, so it deserves a deliberate decision rather than drift.


The old model was a separate store per region, which means redoing every piece of merchandising for each one with the same small team. The industry has moved towards consolidated setups where one store serves many markets, and for new expansion that is the easier path. The trap is that the consolidation story gets told as if every brand is starting fresh. If you already run a dozen legacy expansion stores, you cannot wish them away, and migrating is itself a project to plan for, not a box to tick.


> “A game-changer, we're operating at lightning speeds across 6 stores.”


Lukas Jensen, Ecommerce Manager, Ilse Jacobsen


, on running six storefronts with Depict


So the decision to make deliberately, rather than inherit by accident: wherever you can, run discovery so you merchandise once and localise where it matters, instead of rebuilding the same logic store by store. And decide up front who owns what, central versus local, because an unowned market is the one that goes stale. For most small teams the sensible answer is to centralise the structure and localise only the things that really differ by market: language, availability, a few trading priorities.


## Part five: how to choose, if you're a small team


We've watched discovery tooling get bought, implemented, used and abandoned across a lot of brands. The pattern behind what sticks and what gets abandoned is consistent enough to write down. This is the checklist we'd want every brand to run before signing anything, written from the operator's side of the table.


- **Ask what it costs to run, not just to buy.** Implementation is a one-off. Maintenance is forever. If the honest answer to "how much of my team's week does this need" is more than a few hours, price that in as a real, recurring cost.
- **Be suspicious of "hire someone full time".** If a vendor's answer to their own tool underperforming is for you to add headcount, that's a tell about who the tool was really built for.
- **Check the onboarding and support honestly.** A tool is only as good as the help getting it live and keeping it there. Ask specifically who fixes it when the integration breaks: the vendor's team, or your development agency on day rate?
- **Look at the UI you'll actually live in.** Powerful back ends that are miserable to operate get abandoned. For a team that's in the tool daily, the interface is not cosmetic, it's the entire experience.
- **Lean on your agency, and on other operators.** This is the most undervalued step, so treat it as a real part of the process rather than a footnote. Your agency sees discovery tooling work, and fail, across many brands, which is a perspective no single in-house team can have. Ask them not just "is this good" but "where have you seen this struggle, and with what kind of brand". Then go wider: a trusted group of operators at similar-sized brands will tell you things no demo will, what broke six months in, what the support is really like, whether they'd buy it again. The most reliable signal in the whole market is the head of ecommerce two brands over who actually runs the tool every day. Use it deliberately.
- **Demand your own catalogue in the evaluation.** Demos are choreographed. Insist on seeing your own products, your own categories and your own known-difficult queries inside the tool before you commit, and judge the results yourself. A vendor confident in their product will welcome it; a vendor who resists is telling you something.


## Conclusion: discovery shouldn't be a full-time job to be good


The brands getting the most out of search and merchandising aren't the ones with the biggest teams or the most configurable tools. They're the ones whose tooling does the manual work for them, surfaces what's worth their attention, lets them plan ahead instead of reacting, and stays out of the way technically.


For a lean team, that's the whole game. Less operational drag. More of your limited hours on the decisions that genuinely need a human: the trading calls, the brand storytelling, the judgement. Automation that handles the mundane and amplifies what makes the brand distinct, instead of flattening it into everyone else's storefront. A tool that gets lighter to run as it gets better at its job, rather than heavier.


That's what good discovery looks like for a team that does more with less. Not a second job. A team member that does the grunt work, points you at what matters, and lets you get on with trading.


If you'd like these ideas applied to your own store, take the sixty-second holiday test above, or go straight to the short personalised read: what's visibly working on your storefront, what's quietly leaking, and which of the practices above would move the needle first.


Want this run against your actual store?


Leave your email and your store's web address and we'll send you a short personalised read within 48 hours: what's visibly working from the outside, what's quietly leaking, and which of the practices in this guide would move the needle first for you.


About Depict.


Depict builds merchandising, search and recommendations for brand-led ecommerce, designed so a small team can run discovery without it becoming a full-time job.[depict.ai](https://depict.ai/)
