---
schema_version: "1.0.0"
document_id: "00140bc0c5874bf5b0aa0bee2524dbce397d5d8682942a3a088bf0d18f42c71c"
company_key: "yc-freeport-markets"
company: "Freeport Markets"
source_id: "yc-freeport-markets-news-import-46c2f164323d"
canonical_url: "https://www.freeportmarkets.com/research-weekend-perps.html"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-25T05:46:52.249644+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:8c23461aae94ae6c37ba16e92370fc256d5886ecbd3eb305a2a6ff8f531cde25"
---

# Trading Weekend Perp Markets

From time to time we reach out to users whose onchain activity reflects the kind of trading we think everyone can learn from, and ask whether they would walk us through their process. We never publish or reveal anything about a user without their explicit permission. The piece below is drawn from a long conversation with one such user. All names and personally identifying details have been changed, and while we trust our source, we cannot independently verify every claim he makes. All figures are produced from public data for illustrative purposes and do not constitute investment advice.


## Sea Cliff


Sea Cliff at quarter past eight on a Saturday. The fog has pulled down so low over the Pacific that the Golden Gate is more idea than bridge: you can't see it, but you know it's there. The hedges on El Camino del Mar are glistening with damp. A foghorn sounds, twice and then twice again.


Trader 12 had asked me over for coffee and pastries. He opens the door in a linen shirt, barefoot. Chemex on the counter, croissants on a parchment-lined plate. His day job is at a venture-backed AI hedge fund downtown, the kind where the machines do the trading. Saturdays, though, are for the side gig. "On weekends, markets no longer sleep," he says, pouring two cups of coffee. "You just have to figure out what's moving them."


## The inference problem


For a few dozen hours from Friday close to Monday open, Korea is dark, Japan is dark, Wall Street is dark. The only live tape on names like EWY, EWJ, and a handful of American chips and hyperscalers is[perps](https://freeportmarkets.com/research-perpetual-future.html) onchain. Someone has to price those names to the Monday open.


"The trap most people fall into," he says, "is to look at the S&P and then just multiply by a beta someone fit on a year of data. But that's not how markets work." (A beta is just a multiplier. If Broadcom's beta to the S&P is 1.3, the rough rule says Broadcom moves 1.3% for every 1% move in the S&P.)


He sketches on a napkin. "Three prices are relatively liquid on perps over the weekend: SPX, NDX, and crude oil. But those three numbers aren't the real drivers. They're summaries of something underneath. Behind them sit what we call factors. A factor is a hidden force that moves a lot of things at once. You can't trade it directly, you can only see it through the prices that respond to it. The reason factors are useful is that most of what happens in the world is explained by relatively few of them. Thousands of stocks bounce around every day, but if you take a few well-chosen forces and ask how each stock responds to each one, you can explain most of what you see. The rest is company-specific noise."


## More factors than prices


"First factor is macroeconomic growth, how fast the world economy is actually expanding. When it's down, everything bleeds together. Second is real rates, long-term interest rates after you subtract inflation. The companies that get hit hardest when real rates rise are the ones whose profits sit far in the future. Then there's country and sector, many smaller stories bundled together: Korean memory, Japanese autos, Taiwanese foundries, US hyperscalers, each reacting to the same news differently. An oil spike crushes Asian manufacturers because they import almost all their energy. For US names it's a shrug, since shale made the U.S. an energy exporter. There are a handful more I'm not naming. The point is that there are more factors than there are liquid prices."


"Nonetheless, you can use the liquid tape as a constraint. SPX, NDX and WTI trade hundreds of millions of dollars a day over weekends. Treat those three as efficient. Whatever values the factors take, they have to reproduce the liquid returns. That doesn't pin them to one answer, but it cuts the space from infinite to a finite envelope of combinations consistent with the liquid tape. Then you project that envelope onto the thin names that only trade a few million a day. Each combination inside the envelope implies a price for Samsung, for Hynix, for EWY; across the whole envelope you get a range. The implicit bet is that the thin names are not efficient: Korea is closed, the perps trade thin."


"There seems to be just one problem," I said. "More factors than prices means many combinations of factor values can produce the same SPX, NDX, and WTI returns. And each combination, projected onto Samsung or Hynix, gives a different answer. The same liquid prints can be consistent with Samsung up two or Samsung down five, depending on which factor story is doing the work. How do you decide?"


"News tells you direction. The envelope gives you magnitude. Direction without magnitude is a tweet. Magnitude without direction is noise. Both together is a trade."


"The news being."


"What the headline points at. An OPEC decision points at growth via oil, with importers and exporters reacting in opposite directions. An Nvidia blowout points at country and sector. A Fed surprise points at real rates. The news doesn't narrow the envelope to one answer. It tells you which corner of it the market wakes up believing."


## The trading problem


"In practice, the way I express a view is mechanically simple: I'm saying some less liquid symbol, Samsung, Hynix, EWJ, should move on a higher or lower beta to the liquid tape than the perp is currently pricing. But the claim underneath is really about the factor drivers behind this particular move being different from the ones averaging into its prior. When Hormuz is the story, the relevant driver is energy intensity, and Asian importers should load harder. When earnings are the story, the relevant driver is[hyperscaler capex](https://freeportmarkets.com/research-earnings-field-guide.html) , and memory should decouple from oil entirely. The different beta is just the shadow that a different factor mix casts on the liquid axis. You're never really betting on a coefficient; you're betting on which factors are doing the work. Let me give you some concrete examples."


"First. The weekend the war began. Tehran and US rhetoric had been escalating all week, a strike package widely reported as imminent. The cross-section already told you who was exposed: Korea and Japan import almost all their energy through Hormuz, the U.S. is a net exporter. Saturday the strike landed. Oil was up onchain with SPX and NDX down slightly, but the interesting thing was that Asian tickers like EWY, EWJ, Samsung and Hynix all traded in line with American symbols even though they had higher exposure to energy shocks. The amount those less liquid Asian symbols were down was inconsistent with how much oil was up and how much the US market was down, so I shorted all of them. On Monday, Samsung opened down nearly ten. Hynix down eleven and a half."


"Then earnings season started. Financials went first, the way they always do: strong credit expansion, consumer spending holding up, capital markets activity firming. Financials are the leading indicator for the real economy because they sit at the front of every transaction. And the intraday correlation between the US market, especially the AI-exposed names, and oil started breaking down. Memory and photonics specifically weren't tracking oil intraday anymore. They were trading their own factor, tied to hyperscaler capex. The decorrelation persisted, day after day."


"You'd think a correlation isn't tradable, but it is, because the way weekend perp traders trade doesn't seem to update as fast as the weekday market does. When bad macro news lands on a Saturday, oil rips, and the perp sells chip names down on the old beta-to-SPX, you fade it. Long the chips at the perp lows. Cover Monday open. The weekday market already knows those chips don't load on oil the way they used to. The weekend perp traders get there eventually. You just get there first."


"The factors themselves don't change. Macroeconomic growth, real rates, country and sector. What changes is how each single stock loads on each one, and that drifts faster than most people give it credit for, as the investor narrative shifts. The hard part is picking which factors actually matter. The principle is simple. The construction is almost all of the work."


## Land's End


As we walk, the morning fog clears and the bridge swings into full view. He filled our coffee into red Solo cups before we left the house, the only cups he had; a pretty degenerate look for two young guys at nine in the morning. The Pacific stretches below us. A jogger goes by. A woman with a stroller. Two old men in windbreakers. Dogs chasing each other in the surf.


"All these people," he says. "None of them is predictable on their own. The woman with the stroller, why she's out here and not the other side of town. The runners. The two old guys. Each one is its own thing. Each one would tell you a long story about why they're here and not elsewhere."


"Sure."


"But step back. Watch a year of this trail. Watch ten years. The aggregate is structured."


"Same as the factor model."


"Same deal. Markets are millions of agents trading their own thing for their own reasons, and each trade is unforecastable on its own. But the covariance structure of returns, what moves with what, collapses onto five or six axes that explain most of the variance. You don't need to know what any individual trader is doing. You just need to know how each name loads on those few axes. Same thing with people on the trail. Same thing with most aggregates of independent agents."


"Why does it collapse like that."


"Because the world is shaped that way. The number of underlying drivers is much smaller than the number of things they push around. The geometry is low-dimensional even when the surface looks impossibly high-dimensional. Most people only see the surface. The trade is in the geometry."


The path curves out toward the point. We keep walking.


*This piece is drawn from a long conversation with a Freeport user who agreed to let us write it up. Names and identifying details have been changed. Perpetual futures carry risk including total loss of principal, and nothing here is investment advice.*


Read more from the Freeport research team on the Freeport Logbook.


[Freeport Logbook](https://freeportlogbook.substack.com/)
