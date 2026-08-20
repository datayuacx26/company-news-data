---
schema_version: "1.0.0"
document_id: "72f464cb160f2b7579b3796d60ea8e5bcf6505b96e666d1630392a7f9dc36d6b"
company_key: "yc-wave"
company: "Wave"
source_id: "yc-wave-rss-404ae0f13af9"
canonical_url: "https://www.wave.com/en/blog/crypto/"
published_at: "2022-03-30T00:00:00+00:00"
first_seen_at: "2026-07-24T06:52:44.556575+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:abf4edffaa9f6f145d917f448d8434e12b56154830b338a94b3bc4a80d9bdd81"
---

# Cryptocurrency doesn’t address the hard parts of financial inclusion

# Cryptocurrency doesn’t address the hard parts of financial inclusion


Ben Kuhn


on


March 30, 2022


engineering


---


People sometimes ask us whether Wave uses cryptocurrency or blockchain technology. An important part of the cryptocurrency narrative is that[it can help financial inclusion](https://www.weforum.org/agenda/2021/06/cryptocurrencies-financial-inclusion-help-shape-it/) —after all, it’s a way to put your money on a computer, with no legacy banks or regulators acting as gatekeepers. Surely that’s what we need, right?


Unfortunately, in reality, cryptocurrency doesn’t address the hard parts of financial inclusion.


To see why, let’s look at the different problems that Wave needs to solve in order to serve our customers. Here are the main ones, ranked in increasing order of difficulty:


1. We need to keep track of our customers’ account balances.
2. We need them to trust that their Wave balance is correct and Wave won’t lose the money.
3. We need to comply with relevant regulations—know-your-customer laws, capitalization requirements, account limits, etc.
4. We need to provide an easy way for users to exchange their balance for cash and vice versa. (This is especially important in low-income countries, where many of our users have little enough cash that keeping extra balance in their Wave account isn’t practical.) For Wave, this is our agent network.


Does cryptocurrency help with any of these?


Cryptocurrency is a fine solution to the first problem—tracking account balances—especially between third parties that don’t trust each other.[At Wave, though, we prefer to use boring technology,](https://www.wave.com/en/blog/simple-architecture/) and a simple relational database like[Postgres](https://www.postgresql.org/) does an equally good job at this. In fact, our Postgres-based accounting ledger supports more transactions per second, with dramatically quicker transaction confirmation times, than most blockchains.


The second problem, trust, is one that cryptocurrency does help with under some circumstances—but not in the context of financial inclusion. For a technically sophisticated early adopter, the cryptocurrency pitch—“these algorithms guarantee that we can’t get your balance wrong, even if we’re malicious”—makes some sense. But a typical underbanked person in Senegal, who may not understand Merkle trees or Byzantine fault tolerance, might justifiably be skeptical. For us, it’s much more important to have more traditional markers of trust, like approval from bank partners, billboards in major cities, TV and radio ads, and so on.


Cryptocurrency proponents often claim that crypto *does* help with the third problem, compliance—because crypto doesn’t technically fall under central bank jurisdiction, since it’s not fiat money. In our experience, though, this is a badly mistaken assumption about how regulators work. Regulators gonna regulate, regardless of your aggressive interpretation of the letter of the law. If you’re facilitating a substantial fraction of economic activity—as Wave is in Senegal—the central bank needs to be comfortable with what you’re doing.


But the hardest problem we have to solve by far is the fourth: providing an “on-ramp” for our customers to convert their balance to and from cash. Cryptocurrency actually makes this *harder.*


The importance of a smooth on-ramp is seriously underrated by cryptocurrency enthusiasts. That’s understandable—the enthusiasts are exactly the people who were most willing and able to go through all the difficult steps of investing in cryptocurrency today, and it’s easy for them to assume that it would be just as doable for other people. But most of our customers aren’t tech-savvy, number-savvy crypto enthusiasts; they’re normal people with better things to do than stress out about exchange rates, transaction fees, confirmation times and wallet passwords. Plus, they don’t have enough savings to be happy keeping some of it locked away in a parallel financial system. It needs to be frictionless to get money in and out whenever it’s needed.


Suppose you’re a typical Wave user—a Senegalese fisherman who gets paid for your fish via Wave. On an average day, you make around 5,000 CFA francs, or about US$10. You don’t have any savings. When someone pays you for fish, you immediately walk to the nearest agent and withdraw the money.


Now imagine the same scenario using cryptocurrency. If someone’s going to pay you for fish, you need to have the current crypto exchange rate memorized, then do mental multiplication to convert your CFA price for the fish to crypto. When you go to the agent to withdraw your balance, you’ll need to know what exchange rate the agent offers to convert your balance back into cash. You’d also need to pay a high (and unpredictable) transaction fee, and wait a long time for the transaction to be confirmed so that the agent could be protected from double-spending.


These problems are solvable in principle, by using a stablecoin on a much more efficient blockchain. But that’s a whole lot of effort just to get the user experience back to the baseline that we’ve already built! And even once you’re there, it doesn’t seem like the blockchain has any meaningful advantages that are worth that cost.


Meanwhile, most people trying to use cryptocurrency to improve financial inclusion aren’t even thinking about on-ramp problems. When I mentioned this problem in a Twitter thread, someone pointed me towards what they considered to be the best project in the “cryptocurrency plus financial inclusion” space, run by a person they described as very strong on product. I read a description of how to use the product, which ended with “\[the money transfer recipient\] can simply go to a Bitcoin ATM or local Bitcoin teller and receive their local fiat currency.” I looked up the number of Bitcoin ATMs in the country in question; the first one had launched the previous week. The description did not mention transaction fees, confirmation times, or any expected growth in the number of Bitcoin ATMs.


Our view is that the narrative on cryptocurrency and financial inclusion is mostly a case of “if all you have is a hammer, everything looks like a nail.” If you want to improve financial inclusion, that’s awesome, but we’d suggest approaching it the way Wave does—by thinking from first principles about what the simplest, most effective solution to the problem would look like—instead of by throwing your favorite vaguely-related shiny technology at it.


That doesn’t mean we think cryptocurrency is useless. It has several features that could make it well-suited to other use cases:


- As a store of value that’s not pegged to a fiat currency, cryptocurrency can be a relatively accessible safe asset in countries experiencing hyperinflation.
- It’s much easier to build software that interoperates with blockchain systems than with, say, bank accounts.
- Cryptocurrency transfers settle in minutes, compared to hours or days for other types of inter-bank transfers in many countries.


The last benefit is especially underrated, in our view. Businesses with high cash turnover (like Wave!) can end up with huge amounts of funds tied up “in transit” between different bank accounts. We spend an enormous amount of time and effort optimizing this “back-end” funds flow; if we were able to use cryptocurrency to settle these transfers instantly, we would need dramatically less working capital, which would allow us to scale faster and charge our users less.


Of course, the above aren’t the main use cases of cryptocurrency today: those are speculation,[theft](https://selfkey.org/list-of-cryptocurrency-exchange-hacks/) ,[extortion](https://www.coindesk.com/ransomware-is-a-crypto-problem) ,[more theft](https://www.bbc.com/news/uk-england-birmingham-57280115) ,[drugs](https://academic.oup.com/rfs/article-abstract/32/5/1798/5427781?redirectedFrom=fulltext) ,[even more theft](https://github.blog/2021-04-22-github-actions-update-helping-maintainers-combat-bad-actors/) , and[speeding up climate change](https://www.visualcapitalist.com/visualizing-the-power-consumption-of-bitcoin-mining/) . It remains to be seen whether legitimate, productive uses of cryptocurrency will ever justify the hype. We hope so!


*If you’re interested in leveraging[boring, untrendy, and mature, technologies](https://www.wave.com/en/blog/simple-architecture/) to[improve the lives of our users](https://www.wave.com/en/blog/world/) , we’re hiring at every level, from director (basically the highest management level we have since we’re not large enough to have VPs, etc.) on down.*


*At the moment, some particular critical hiring needs are[on-prem knowledgeable SRE](https://grnh.se/72f35bff4us) ,[generalist SRE](https://grnh.se/bf82e3da4us) ,[iOS engineer](https://grnh.se/c6b84d2c4us) and[Android engineer](https://grnh.se/780f49864us) . Of course, we’re also hiring for[generalist software engineers](https://grnh.se/bfe537594us) as well as[many other other positions](https://www.wave.com/en/careers/) . We’re fully remote and have people all over the world!*


---


We work on Wave because we think it’s[an extremely effective way to improve the world](https://www.wave.com/en/blog/world/) . If that’s how you want to spend your career too,[come work with us](https://www.wave.com/en/careers/) !


---


If you liked this post, you can subscribe to[our RSS](https://www.wave.com/en/blog/index.xml) or our mailing list:
