---
schema_version: "1.0.0"
document_id: "918666c339a241a4725269a8b476b4572ea1de78bfda57afc19383f9873b622b"
company_key: "yc-hadrius"
company: "Hadrius"
source_id: "yc-hadrius-news-import-9fdd9031b6f2"
canonical_url: "https://www.hadrius.com/insights/closing-the-mnpi-blind-spot-what-ccos-need-to-know-about-prediction-markets"
published_at: "2026-06-18T15:49:34.929+00:00"
first_seen_at: "2026-07-24T10:33:12.242348+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:f6d6fbc08cd926be2c582bef8fa30962f9811444617fb16c7e9cadf4df8fbc2c"
---

# Closing the MNPI Blind Spot: What CCOs Need to Know About Prediction Markets

Here's a central question of our time: If one of your employees traded on a prediction market contract using material non-public information, would you know?


If your answer is “probably not,” you're not alone. If your answer is *“Don’t tell me that! I am not listening!”* with your fingers in your ears, you might want to consider what you’d respond if you got that question in an audit.


The regulatory framework around prediction markets is, to put it charitably, a work in progress. As of mid-2026, it isn't even fully settled which federal agency governs these venues. And yet, trading volumes on platforms like Kalshi and Polymarket are exploding.[Bernstein Research](https://www.cnbc.com/2026/04/14/prediction-markets-will-grow-to-1-trillion-by-2030-bernstein-says.html) estimates 2026 volumes will hit $240 billion, nearly four times last year's figure.


The gap between market growth and regulatory clarity is exactly where compliance risk lives.


To help CCOs navigate it, Hadrius brought together four of the sharpest minds working on this problem: **Professor Daniel Taylor,** Director of the Wharton Forensic Analytics Lab and member of Kalshi’s Market Integrity board; **Lisa Pinheiro,** a managing principal at Analysis Group and also a member of Kalshi's Market Integrity board; **Aaron Brogan,** founder of Brogan Law and one of the most incisive commentators on emerging asset regulation; and **Som Mohapatra,** co-founder of Hadrius — who, incidentally, has been on both sides of this issue, having previously worn the CCO hat for an RIA *and* run a prediction market himself.


Below, you’ll find a full transcript of the conversation. You can also view the session here:


‍


‍


‍


Here’s a summary of what our panelists covered.


## The MNPI question is real, and already here.


The reality is that the concept of material non-public information maps directly onto prediction markets. If you know something that would move a contract’s price and you trade on it, that's MNPI — full stop. The fact that the contract pays out between zero and one doesn't change the underlying analysis.


What does change is the scale: most prediction market contracts are still too thinly traded to enable large-scale insider trading the way equity markets can. For now. As liquidity grows, so will the stakes.


## “Reasonable” is the operative word.


Right now, regulators are going after obvious bad actors, not firms that try in good faith. The standard isn’t perfection; it’s demonstrable effort. The question every CCO should be asking is: what does a reasonable compliance program look like in this environment?


## The reputational risk won’t wait for regulation.


Regardless of what the future holds for prediction market regulation, the reputational risk of an employee trading on MNPI exists now. SEC and FINRA regulation will likely come, especially for contracts that are derivatives on equities activity, but the immediate risk — which all our panelists agreed was prudent to mitigate — is employee trading that leverages MNPI.


‍


**Read on for the full conversation, and**[click here](https://www.hadrius.com/prediction-markets) **to see what Hadrius has built to help CCOs monitor prediction market trades.**


‍


## Session transcript


**Leonard:**


Hello and welcome to “Closing the MNPI Blind Spot: How CCOs Can Act Now on Prediction Market Trading.” I'm your host, Leonard DeFranco, and I am a producer on the marketing team here at Hadrius. I'm very excited to be leading a discussion today with some truly world-class experts on the subject of prediction markets.


So this is the theme of our talk today: Prediction markets are here. What are compliance officers supposed to do about it?


For those of you who don't know, Hadrius is AI-native compliance infrastructure for RIAs, broker-dealers, and dual registrants. Our entire raison d'être is helping compliance officers manage their workload more efficiently and accurately. So why, you might wonder, are we here talking about voluntarily expanding the number of channels compliance is responsible for monitoring? Are we *trying* to give our customers more work?


The answer is no. We're trying to help our customers manage the work they have today, and *will have* in the future. It is our belief that a whole new era of investing and a whole new world of asset classes, led by things like prediction markets, is upon us.


These developments are already here. What a CCO needs to do about it is less clear.


That's what we're here today to discuss. But whatever you decide for your firm, our goal at Hadrius is to help you run that future-proof compliance program however you see fit.


Joining us today are some of the top experts in the field.


- **Professor Daniel Taylor** is the Arthur Anderson Chaired Professor at the Wharton School and Director of the Wharton Forensic Analytics Lab. With deep expertise in insider trading, corporate disclosures, and fraud prediction. His research has directly shaped SEC rulemaking and informed investigations by the DOJ, SEC, and FBI. In February 2026, Professor Taylor was named alongside another of our guests to the prediction market Kalshi's market integrity task force.
- **Aaron Brogan** is the founder and managing attorney of Brogan Law, a boutique law firm focused on cryptocurrency and novel financial products. Aaron is a widely published commentator on the subjects of emerging asset regulation and his Substack, the Brogan Law newsletter, offers a gimlet-eyed analysis of the evolving regulatory landscape.
- **Lisa Pinheiro** is a managing principal at Analysis Group and one of the foremost experts on market manipulation detection and financial market integrity. She's advised the US DOJ, regulatory agencies, and major exchanges on spoofing and manipulation and currently serves also on Kalshi's surveillance advisory committee.
- **Som Mohapatra** is co-founder of Hadrius, the AI-native compliance platform built for RIAs and broker-dealers. Som brings to this conversation two interesting pieces of experience. Prior to Hadrius, he co-founded and served as the compliance officer for Quantbase, an SEC-registered RIA — and, also, he used to run a prediction market. So he's been on the front lines of this issue from both sides.


As a reminder, this webinar is for educational purposes only and does not constitute financial advice. Panelists are independent and not affiliated with or endorsers of Hadrius or its products. The views expressed by these fine people are their own.


Before we get into the discussion, I'm going to spend five minutes laying out the groundwork of this issue as it pertains to compliance officers.


First of all, let's start off with a simple question: What is a prediction market?


A prediction market is a venue where users can purchase and trade derivative contracts whose payouts depend on real world events. On the screen you see screenshots of two markets from the two major players in prediction markets today. One is Kalshi, a company that is based in New York City and was the first prediction market to be a federally recognized designated contract market, regulated (we think) by the Commodity Futures Trading Commission (CFTC) much like the Chicago Mercantile Exchange. The other is Polymarket, a blockchain-based exchange that is legally headquartered in Panama and which also became legal in the US as of December 2025.


Both markets allow users to bet — invest, whatever you prefer — on real world events such as, here, the Peruvian election or the price of West Texas Intermediate crude oil. The price of each event contract moves with trading providing a novel form of public sentiment aggregation through the mechanism of price discovery.


Prediction markets are big business. Bernstein Research estimates that 2026 trading volumes will reach $240 billion, nearly four times the trading volume of 2025. Morning Consult predicts that market volumes will surpass $1 trillion by 2030. Right now, more than 80% of trading on prediction markets is sports betting. But theoretically, any verifiable events outcome, and any material non-public information, can be traded on.


Which brings us to the compliance angle. If you're a compliance officer, you already get attestations from, and monitor the trading activities of, employees in equities and options, and Hadrius also supports crypto. We thought prediction market activity should be visible too.


So last month, we launched a tool to achieve exactly that. As of May 2026, the Hadrius Code of Ethics module supports monitoring your employees' prediction market trades. All they have to do is add their Kalshi or Polymarket accounts and you will see a blotter-style rundown of every contract they're buying, which appears next to their trades in regulated securities and crypto. You'll also be able to monitor for specific contract activities such as this example, where you're able to see if an employee is trading on the price of Bitcoin, for example.


But a funny thing happened when we were building this tool. A lot of CCOs told us, "I don't want to see what my employees are doing on there." Which brings us to today.


We are here because we believe that prediction markets are here to stay. The market signals could not be clearer: People want to trade on these marketplaces. And if you read past the salacious headlines, prediction markets are really a promising way to take a very ancient method of price discovery into interesting new places.


In a theoretically mature prediction market landscape, in which liquidity is much higher than it is now and far less of the trading volume is on sports betting, you could essentially get a "quantification of the zeitgeist" around any event.


Almost any information could be securitized and traded on, which would make the world far more transparent. But this does open a challenge for anyone who needs to monitor for things like the trading of MNPI.


Real world events can be used as hedges, which is essentially a democratization of the hedging that has long been available to institutions. Aaron Brogan, in one of his blog posts, imagines a Kansas City caterer who hires extra help in the expectation that the Chiefs will make it to the Super Bowl. If they don't make it, the caterer loses a lot of money. So she can do what institutional buyers of agricultural products and commodities have been able to do for decades: buy a hedge against the Chiefs losing and soften her losses.


And not just in sports. This can be done on nearly any event imaginable.


In total, these markets form a kind of API between investors and real world events. Imagine a world in which the outcome of the Peruvian election sits in your brokerage account. The opportunities for building unique portfolios — and for malfeasance — take a logarithmic leap forward.


All of which is to say, it's not out of any kind of opposition to prediction markets that we think regulation is very likely to come to this space.


Part of that regulatory attention will simply be clarity. A few weeks ago, Minnesota became the first state to ban prediction markets flat out. The federal CFTC, which currently has a very pro-alternative assets agenda, sued Minnesota, saying, "You, a state, don't have the right to ban prediction markets. Only we can ban prediction markets, and we don't want to." This leaves us, the compliance community, in a gray area.


Consider this hypothetical: If an auditor from the SEC asks a Minnesota-based RIA why they weren't on top of some questionable employee prediction market trades, the CCO might be able to come back with, "Why would I be responsible for monitoring something illegal, which I had no expectation the employee was doing?"


Is that defense crazy? Is that irresponsible? Will an auditor conclude reasonable oversight was exerted? That's what we're here to discuss.


[Here's a quote](https://broganlaw.substack.com/p/whats-going-to-happen-to-prediction) from one of our panelists: “The question of preemption eventually accedes to the Supreme Court. Once it gets there, all that counts is counting to five.” The point is, don't let a lack of rulemaking fool you. As of June 2026, when we're recording this, it is not even clear yet which regulatory agency governs prediction markets.


The fight over controlling these venues is just beginning, but prediction markets themselves are here to stay. This space is going to clear up soon, and when it does, compliance officers will need to be ready to adapt.


With that I want to throw out the first question to Som: As someone who has been on both sides of this issue, how has the prediction market space grown over time and how is this now landing on the desks of compliance officers?


‍


**Som Mohapatra:**


Great question. These markets are relatively new from a commercial perspective, but as folks here probably know, there have been the Iowa Electronic Markets and other markets that have existed academically for a long time. From a commercial standpoint, they're a relatively recent phenomenon.


When I was trading the first prediction markets, the most liquid markets were weather markets, with just a few hundred thousand in notional volume — and this wasn't even that long ago. Cut to this past week: there was $7 billion in notional volume across 600,000 markets. Compare that to some of the most highly traded assets in the world — one major asset had $30 billion in daily volume this past week. Prediction markets are small, but they're no longer a drop in the bucket.


Overall, my message for compliance officers is: the proliferation of prediction markets is a good thing, but we need to balance it with nuance. Proper regulatory compliance is the way to do so. Oversight and surveillance isn't necessarily a bad thing. Later on in this conversation, I'll dig into how some of our early customers are approaching this.


The last point I'll mention: with the industry pushing for looser regulations, the job of a compliance officer, especially at an investment firm or broker-dealer, is more important than ever before. The goal has never changed: protecting retail investors.


‍


**Leonard:**


Professor Taylor, you're an expert in market manipulation in equities, but the idea of what counts as MNPI in equities has decades of case law behind it. When you're thinking about a prediction market — where essentially everything that happens in reality can be a contract — what counts as an "insider" when you're trading on reality? And how applicable is the forensic toolkit you've built to this domain, or are you having to start from scratch?


‍


**Daniel Taylor:**


So there are a couple of things to unpack. I'm Professor Taylor from the Wharton School. I've been there for 16 years, running the Wharton Forensic Analytics Lab, which looks at market manipulation, insider trading, and fraud detection in equity markets. As Leonard mentioned, I joined Kalshi's market integrity board with Lisa earlier this year.


Initially when I joined the board, I thought there was going to be more similarity between prediction markets and equity markets. Now I'm revising that: I think there are nuanced differences that, not only did I not appreciate, but that a lot of people don't appreciate, in terms of challenges with monitoring them. Putting aside the fact that you can "bet on reality" — or "trade on reality," depending on your preferred word choice.


I don't think in terms of who's an insider and who's not. I think about who has access to material non-public information. The concept of MNPI in prediction markets is the same as in equity markets: is it information that would move the market, or that traders would find useful or relevant if it was made public? For example, if it was known that the US was planning offensive operations against Venezuela, that would be material non-public information — certainly for a contract on whether we would conduct a military raid in Venezuela.


The one thing I would say for your audience — broker-dealers and investment advisors — is that Polymarket is not registered in the US. It is technically not legal for US citizens to trade on Polymarket's international arm. Polymarket has a US arm, but it's smaller and mainly sports-focused. The international Polymarket is the major exchange. It’s not currently legal for US customers to use it. Before the change of administration, the Biden DOJ was investigating whether Polymarket was enticing or allowing US customers to actually trade on the exchange.


One of the reasons this matters to RIAs and broker-dealers: Polymarket is listing equity-based swaps and equity-based contracts without registering them with the SEC. I'm looking at my chart right now — there's a contract for whether Meta stock will end the day up or down, currently trading at 74 cents. So there are equity-like instruments listed on Polymarket. So when people say, “why should I care about prediction markets?” If you care about whether your employees are trading stocks, options, or derivatives, you should care about what they're trading on Polymarket. Because there are things that look a lot like equity on Polymarket. That's the immediate reason to care.


Kalshi has to go through the CFTC, so you don't see as many equity-based instruments there, although you occasionally see things like "When will SpaceX go public?" So the concept of MNPI does carry over. I think surveillance is more challenging.


And the final thing I'll say: the depth of these markets is not nearly the depth of equity markets. Many contracts — like the Maduro contract — traded maybe $100,000 to $200,000 in cumulative lifetime volume. That is not large enough to facilitate large-scale insider trading. While it's possible to make a little bit of money across a wide range of things — like "Is Bad Bunny going to be at the Super Bowl?" or "Will Donald Trump show up at the Knicks game?" — the actual dollar value of MNPI gains is still orders of magnitude smaller than if you had MNPI on a merger in equity markets. So equity markets still need to be the primary concern, but as prediction markets grow and liquidity increases, they'll become an increasing compliance concern.


‍


**Leonard:**


Great, thanks. Same question to Ms. Pinheiro — Professor Taylor is talking about how the volume makes this a fundamentally different type of market. Are you finding that your toolkit is as applicable here? And what are the most common types of market manipulation activities you're seeing on Kalshi?


‍


**Lisa Pinheiro:**


Well, let's see how much we can say about that. One thing I would add to Dan is that the price is between zero and one at any point in time, and it has a limited duration — it appears, then it disappears. The liquidity varies during that window. Also, what's a 10% price increase? Is it going from 1 cent to 2 cents? Is it going from 90 to 99? Those are very different things. So the scale is different. This is something to keep in mind when thinking about a toolkit. In a regular market, you tend to think in percentage terms, but here you have a fixed scale. That's definitely another structural difference you have to keep in mind when applying a typical toolkit.


More generally, I like the democratization of trading. It's the part I find exciting. I personally like to go on and try to arbitrage across different exchanges. It's really fun, but it also makes it very easy for people to try to manipulate things, sometimes maybe in ways they don't even realize are illegal because they haven't been trained as traders. They haven't been told, "You can't do this because that's spoofing."


Think about the Jesus example. There was a contract on Polymarket: will Jesus Christ be back in 2027? It was trading at around 4 cents — implicitly a 4% likelihood. Then there was another contract: will this contract go over 5% this year? If you buy a few contracts — because the market isn't that deep — you can bring it over 5% and make money on the other contracts. So there are questions about even how you structure contracts, and if they're dependent on each other, you can create opportunities for manipulation.


There's also the possibility of spoofing — one area where I've done a lot of work. That's where you want to sell, but you pretend you want to buy by putting a lot of orders in the queue so it looks like buying pressure, the price moves up, and you sell at a better price. Someone might come up with that intuitively without thinking that they're manipulating people through their orders. And you can combine trading with communications — within the platforms or outside — to execute pump-and-dump schemes if you manage to spread false information to the group.


‍


**Leonard:**


Is that the type of activity Kalshi is most worried about — the very intentional pump-and-dump stuff, or insider trading?


‍


**Lisa:**


I can't speak for Kalshi, but I would say the news talks a lot about insider trading, and I think that's where most of the examples come from. I was at a recent event where there were representatives of the CFTC and the SEC, and insider trading always comes up first. But they're always clear that all the other types are also part of what they expect markets to surveil. So not off the hook yet — but insider trading is definitely the most prominent right now. That's what keeps the headlines.


‍


**Leonard:**


Yeah, I would love to meet someone who has MNPI about whether Jesus is coming back.


All right, Aaron: Do you see any structural barrier to these markets becoming mature? A lot of times we say "this could become the new CME" instead of the toy it is now. In crypto we've seen something similar — Ripple wants to replace SWIFT, Bitcoin wants to replace gold reserves, and there's no theoretical reason why those couldn't happen. Do you see a theoretical reason why prediction markets could not become the new standard for contract markets?


‍


**Aaron Brogan:**


Yeah, and thank you for having me. I'm Aaron Brogan, founder of Brogan Law, which represents prediction markets, cryptocurrency, and other novel asset classes.


Maybe it's just being a hammer and seeing everything as a nail, but the main barrier I see to the adoption and growth of these markets is simply the degree of legal uncertainty that still hangs over them. These markets as they exist now — maybe less so for Polymarket, but Kalshi, and I think Polymarket has been buoyed by this wave — exist just because Kalshi was successful in litigation in 2024 and was able to offer new markets. So it's very recent that the CFTC has even treated this product as legal.


And while the CFTC now does — and I think Mike Selig, the current CFTC chair, is quite open to these markets — you mentioned a number of states trying to ban them. It is still an open question as to whether and to what extent that is possible. The Commodity Exchange Act, as amended by Dodd-Frank, is pretty clear in my view that both prediction markets are swaps, and that being swaps, the CEA preempts state enforcement. But there are still arguments that states have made to the contrary, and states have won in a number of federal courts on those arguments.


I see this as the main barrier to the maturation and growth of these markets. A lot of the VC and investor interest is because these markets have the potential to cover a really broad swath of the American economy — almost anything could be included. But most of the current interest continues to be retail trading on sports and entertainment. Maybe elections are slightly different, but the locus is still in the retail zone. There's a world where you could imagine more mature commercial applications, but it's harder for large players to build a business if they don't know what the rules are.


So to me, the culmination of this litigation and the eventual rulemaking by the CFTC are the two main structural barriers to these markets really flourishing. They are flourishing now, but there's more room to run if those two hurdles can be cleared first.


‍


**Leonard:**


And the fundamental question we're wrestling with as a compliance vendor is: what is the implication of the delayed rulemaking regarding the CFTC versus the states, and how will that impact what the SEC expects of actual compliance officers? So let me throw that to the group. Does my hypothetical from the slides hold up — if a compliance officer says, "I shouldn't be expected to surveil employees on channels whose regulation isn't even clear yet," does that constitute a valid excuse?


‍


**Aaron:**


I think it's a really good question. I don't know — and none of this is legal advice — whether that is a valid excuse. For my prediction market clients, I tend to take a more aggressive posture, because market share is being determined now. But on the other side of the same coin, when I advise my clients on whether employees should trade on prediction markets, I generally advise against it, even if there isn't a specific CFTC rule yet. It's not mission-critical to your goal of building a competitive product, and the cost in this still-developing regulatory environment could be very high. So for me it's less a matter of what you have to do and more a matter of what is prudent.


‍


**Lisa:**


Maybe I'll just jump in. I would say they're still at the stage, from a regulatory perspective, where they're going to go for the low-hanging fruit — the obvious manipulation cases, situations where people have been colluding to take advantage of prediction markets. I don't get a sense from what I've heard from regulators that they're going to go after people with good intentions who tried their best but maybe didn't catch everything. They're going to go for the obvious cases. And more generally, I think as long as people are doing something reasonable, the expectation is not yet that it has to be perfect — just that you did something reasonable.


‍


**Taylor:**


I do think there's a question of what "reasonable" means, and I think you can answer it counterfactually. If you're monitoring your employees' trading in IBM on US equities, you probably don't want them trading IBM on a foreign exchange either. So if you would want to monitor that, I think you'd also want to monitor their trading of equity-like instruments on Polymarket specifically.


If you're an officer or director under securities laws, you have to file beneficial ownership forms when your economic ownership changes — whether you buy options, calls, anything like that. There are equity-like instruments currently being traded on Polymarket, and I think that's the closest case to an immediate surveillance obligation — or just banning it and saying "you have to comply with US law, and US law says you can't have an account on Polymarket's main exchange."


I do think things will change over time. Three or four years from now, I think it's not just going to be Kalshi — Susquehanna/SIG has already said they're going to have their own exchange, Robinhood is powering up their own exchange, Crypto.com now has event contracts and is CFTC-regulated. People see dollar signs in prediction markets, and so there's going to be a lot more of them going forward.


I also think the SEC is going to come off the sidelines. I wouldn't be surprised if under the current SEC administration, before they leave office, they try to make a play to incorporate as many prediction market contracts as possible that look like equity. So, a contract on SpaceX's valuation? Security-based swap. A contract on whether Meta goes up or down? Security-based swap. And if the SEC makes that play, it immediately triggers compliance requirements for all broker-dealers and their customers.


I don't think it's sustainable where, just because an instrument trades between zero and one, even though it's linked to a publicly traded company, it's not considered an equity-based swap.


My assessment is that eventually the question of what constitutes gambling regulated by states versus regulated by the CFTC will make it to the Supreme Court. If I was a betting man, I'd say the Supreme Court might scope out a lot of the sports-related prediction market contracts as gambling and carve them out, but I find it hard to see how contracts on conference call mentions, tweets, or presidential elections could be characterized as gambling. It wouldn't surprise me if five years from now, sports gambling gets carved out and non-sports prediction markets are largely regulated at the federal level.


‍


**Lisa:**


And we talk a lot about regulatory risk, but there's also reputational risk. As a firm, you just don't want your employees in the news for that kind of activity. Beyond the fact that it may not be clear what's expected of you, it's definitely bad publicity.


‍


**Taylor:**


And there are going to be legal compliance costs. Think about the individual who was indicted for trading on Google search contracts, a Google employee. Google had to bear costs associated with that: DOJ and FBI showing up with subpoenas and warrants, asking for documents. There's reputational risk and legal risk. The SEC hasn't really said "these are security-based swaps" yet, but when they do, that's going to trigger a tidal wave of necessary compliance. I do think that's coming.


‍


**Leonard:**


I'm also wondering if a change in priorities at the agencies would trigger a change. Right now it's taken as given that these markets want the CFTC to regulate them because the CFTC is taking a forgiving attitude. But if that changed, might they prefer state regulation? Aaron — do you think it's possible that a political change could make the markets prefer state-level oversight?


‍


**Aaron:**


I don't know if I could ever see the major DCMs choosing state regulation — although maybe if there was a very unfavorable regime in place, it’d be possible. Crypto firms have always preferred the CFTC as a regulator, even in unfavorable times. The CFTC has been seen as a pragmatic regulator that's rarely politically captured. I'd still prefer that to the Nevada Gaming Commission, because you're just going to end up having a big fee clipped off you forever in that world.


But this is one of the reasons I think it's critical to get to the Supreme Court for these industries, and critical that the CFTC finalizes rulemaking now, is that as these issues become political footballs, you start to get results that swing with elections and could be unfavorable to participants. I do see the Left taking a less favorable view of prediction markets, viewing them as extractive. That's advantageous for some politicians because these issues can become very salient for people who have a moral response to gambling or risk assets. So there's definitely some risk. I don't think we'd ever prefer the states, but you never know.


‍


**Leonard:**


Som, what would you say is the minimum viable level of monitoring a compliance officer should be aiming for now?


‍


**Som:**


Yeah, for sure. Thanks folks for really digging into the what and the why. In terms of the how — we're a tech vendor, so we don't provide guidance on what you should and shouldn't monitor — but what I can share is, when compliance officers come to us asking, "We're thinking about doing X — how should we approach this?”


The way we think about this is actually surprisingly similar to off-channel communications. About four or five years ago, the prevailing approach at a lot of firms was some form of attestation: "My firm doesn't allow WhatsApp or iMessage archiving, so I'll send out a quarterly request for employees to check a box saying they won't use iMessage." What we found in the past year or two is that didn't work — and we found this out through fines. What we've really seen firms move toward is, your employees are going to use these channels. Make sure you have monitoring in place.


There are roughly four kinds of trade data ingestion. First is paper statements — a broker like Schwab sends you a statement and you forward it to your compliance officer. Second is broker feeds via SFTP. Third is the Plaid-style direct login. Fourth is the API key — the vendor gives you a key and you allow a direct connection.


Prediction markets are built by API-first companies. In fact, Kalshi and Polymarket had APIs before they even had many users. So to answer your question, Leonard: the minimum viable approach to prediction market monitoring is the API key. Any user can go to the search bar, find "API key," create one, and submit it to their compliance vendor. That's how we do it. The best solution — which companies like Plaid are moving toward — is direct login, but we're not there yet. The MVP is the API key.


What I'm watching going forward is, what happens when there's more nuance in the markets, or when it's not just Kalshi and Polymarket — what happens when Robinhood or others enter the fray? There's a lot of change happening not only in how markets are treated by regulatory bodies, but also in how that data gets ingested and interpreted.


‍


**Leonard:**


One question from the audience: Is there any way to monitor not just the contract but the other side of the contract? What are the KYC obligations, and are there opportunities to evade sanctions?


‍


**Lisa:**


Via VPN you can bypass a lot of these protections — which is why, as Daniel was saying, you can tell people they shouldn't, but then they probably will. On the KYC side, at least on the Kalshi front, they definitely gather information and compare against sanctions lists. Is it possible to evade those processes? There are always ways to fraudulently try, but they do have systems in place.


‍


**Taylor:**


Kalshi is inside the US financial system with the standard on-ramps — you deposit into Kalshi using ACH, the same way you deposit into Charles Schwab or Robinhood. With Polymarket there is no KYC or AML. It's entirely anonymous, via crypto. So sanctions evasion is a concern, just like it is on Binance. If Binance decides it doesn't want to comply with US law, it doesn't have to. Same for Polymarket.


What surprises me is that there has not been a coordinated government attempt to prevent US individuals from using VPNs to get onto Polymarket. I would have expected that to change; it was originally a priority of the Biden administration. But elections have consequences: Donald Trump Jr. is on the boards of both Kalshi and Polymarket, and coincidentally the DOJ investigation into US users on Polymarket was dropped.


I'm also very skeptical that Polymarket will be able to get CFTC compliance for its main exchange, because doing so would require US financial rails, which would require KYC and AML — and if you wanted that, you could have done it through Kalshi. I think there's a fundamentally different customer base for Polymarket than for Kalshi, just like there's a different customer base for Binance versus Coinbase.


‍


**Leonard:**


Another question from the audience: Are the equity-like instruments on Polymarket currently only on the international platform that US persons cannot legally trade on? I think the answer is yes — there is the ability to trade on US equities internationally through Polymarket.


‍


**Taylor:**


And if someone had a VPN and wasn't worried about strict compliance, they could still access it.


‍


**Leonard:**


All right, lightning round. Lisa, when you study Kalshi activity, what does the data allow you to understand, and where are the blind spots?


‍


**Lisa:**


The main difference is that in regular markets you have a lot of data on a few contracts, whereas here you have a lot of contracts with varying levels of data. The dynamics are different.


Some things we know empirically to be true about how people trade in regular markets may not always apply here. One example: a basis for spoofing detection is that people react to supply and demand signals: when they see a lot of buy orders, they think the price will go up. Does that even work the same way in prediction markets? Are people even looking at the order book with that kind of thinking in mind? All of that is still empirical. We still need to figure it out.


‍


**Leonard:**


Professor Taylor, the vast majority of profits on these platforms are going to a few accounts, many using automation. Philosophically, where's the line between savvy investing and market manipulation?


‍


**Taylor:**


I don't think you can draw that line based purely on the amount of profits someone makes. *The Wall Street Journal* ran a piece saying most people on Kalshi and Polymarket lose. Yes — I think that's probably true. But most people who try to pick individual stocks also lose relative to the index. The issue is when people think there are no institutions present on prediction markets. There are; there are institutional market makers. When you're trading, it might not be the case that you're on the opposite side from another retail user; you might be trading against Susquehanna.


Early on, prediction markets like InTrade, PredictIt, and the Iowa Electronic Market were mainly retail, with $850 position limits. Now that those limits are gone, institutions have shown up. When you increase the sophistication of trading strategies, the person who doesn't keep up is the one who loses — just like in the stock market. I think within a few years, people will understand they're trading against algorithms on prediction markets, just as they've come to understand that in equities.


‍


**Leonard:**


Aaron — not that you get a say, as an attorney — but do you think the SEC *should* hold off on making a rule about reasonable oversight before the preemption question is settled by the Supreme Court?


‍


**Aaron:**


I think the survival of these prediction markets in their current form as retail platforms probably does depend on the availability of entertainment and sports markets. So yes, I would wait if it was me. The SEC has a lot on its plate.


‍


**Leonard:**


All right. Thank you all for honoring the lightning round time limits, and I think that does it for us. I want to thank our panelists: Professor Daniel Taylor, Lisa Pinheiro, Aaron Brogan, and Som Mohapatra of Hadrius. Thank you all for joining us.


‍


*To see what Hadrius built to help CCOs monitor prediction market trades, visit our Prediction Market tracker page*[here](https://www.hadrius.com/prediction-markets) *.*
