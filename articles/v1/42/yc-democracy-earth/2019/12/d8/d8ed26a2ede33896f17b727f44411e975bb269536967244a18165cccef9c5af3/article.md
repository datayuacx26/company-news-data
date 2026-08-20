---
schema_version: "1.0.0"
document_id: "d8ed26a2ede33896f17b727f44411e975bb269536967244a18165cccef9c5af3"
company_key: "yc-democracy-earth"
company: "Democracy Earth"
source_id: "yc-democracy-earth-rss-cda4ae0def8b"
canonical_url: "https://medium.com/hacktivism/announcing-sovereign-0-7-moloch-compatible-6f5dc43558d5"
published_at: "2019-12-05T10:17:38+00:00"
first_seen_at: "2026-07-27T01:43:49.601606+00:00"
fetched_at: "2026-07-28T22:26:49.714113+00:00"
content_hash: "sha256:108ecc505980cb5057639c19603352dd2a5716d28800bc26cc11c4e6d277067a"
---

# Announcing Sovereign 0.7 | Moloch-compatible

# Announcing Sovereign 0.7 | Moloch-compatible 👹


[Democracy Earth](https://medium.com/@DemocracyEarth?source=post_page---byline--6f5dc43558d5---------------------------------------)


5 min read


·


Dec 5, 2019


--


> It is important to differentiate the between verifying an identity and verifying the right to use a given technology.
> *Edward Snowden — Web3 Conference, Berlin.*


Press enter or click to view image in full size


👹👹👹moloch.democracy.earth 👹👹👹


**Today we are proud to announce a pivotal release for our governance software:**[the 0.7 Moloch-compatible Sovereign](https://github.com/democracyearth/sovereign) **.**


The platform has been fully adapted to display events from[MolochDAO](https://molochdao.com/) smart contracts. In this new release you can see all listed proposals in a DAO, check the ongoing results, explore the activity of every address interacting with the DAO, and filter proposals by current period. It's Mobile and Desktop compatible, and — importantly — routing URLs are adapted to match the structure of block explorers like[Etherscan](https://etherscan.io/) .


This is a read-only version, but a key first step towards enabling full compatibility between the legacy development of the Sovereign software and MolochDAO forks on EVM-compatible blockchains.


Here are the links to explore it:


- Demo:[moloch.democracy.earth](https://moloch.democracy.earth/)
- Repo:[github.com/DemocracyEarth/sovereign](https://github.com/DemocracyEarth/sovereign)
- Release:[github.com/DemocracyEarth/sovereign/releases/tag/0.7.0](https://github.com/DemocracyEarth/sovereign/releases/tag/0.7.0)


## ***What is a DAO?***


Throughout the year of 2019, the blockchain ecosystem saw the maturation of a specific class of contracts known as DAOs, *Distributed Autonomous Organizations* . In a DAO, a group with a shared interest pools funds, and then the voting procedures on the specific allocation of the resources is done according to the rules encoded in[Smart Contracts](https://github.com/DemocracyEarth/community./blob/master/Academy.md#41-smart-contracts) — that work as a sort of trusted, automated constitution. This is a new institutional modality, which emerged as a possibility due to the creation of Ethereum, a blockchain network that permits Turing-complete computations (able to process computable contracts), leading to the growth of Smart Contract development for decentralized applications.


The first widely known DAO, named *TheDAO* , was infamously hacked in 2016 — which led to experimentation with governance contracts being put on hold within the ecosystem. Three years later, a clear reversal to this trend was spearheaded by[MolochDAO](https://molochdao.com/) , an organization built by stakeholders invested in the Ethereum Network that pooled 6096 ETH (USD $938,269.68) to fund and advance the development of public infrastructure related to Ethereum 2.0. — the next version of the Ethereum protocol. The MolochDAO contract overcomes the[Tragedy of the Commons](https://science.sciencemag.org/content/162/3859/1243) (where individual incentives are misaligned with globally optimal outcomes) with a feature known as the *RageQuit function* : a grace period ensues after each proposal is approved, during which members can freely exit the DAO and liquidate their shares.


Santi's Membership Proposal for MolochDAO


Democracy Earth Foundation’s co-Founder Santiago Siri joined the MolochDAO using[selloutdao.com](https://selloutdao.com/) — a bribing contract written by MakerDAOs[Mariano Conti](https://twitter.com/nanexcool) during the ETHBerlin hackathon. The smart contract entailed that for 0.5 ETH (USD $75.00), a buyer could send a proposal to MolochDAO through Mariano’s address using his pre-existing shares as a member, and then vote on it with his shares (but without his active involvement).


@nanexcool on[selloutdao.com](https://selloutdao.com/)


It was an experiment on transparent, “automated bribery” aimed at testing the boundaries of blockchain governance. Santi obtained 389 votes in favor — and zero against — his proposal to join MolochDAO, and is now an active member of the guild.


Press enter or click to view image in full size


Different Moloch forks


MolochDAO is becoming the most widely adopted smart contract to deploy DAOs over the Ethereum blockchain — perhaps because it only consists of 400 lines of code and specific game theoretic tweaks that reduce the risk of participating on a DAO. Due to its traction and effectiveness, several new groups forked the Moloch contract for their own governance, creating a burgeoning ecosystem of new DAOs. Some of these new organizations are the[YangDAO](https://www.yangdao.org/) , funding the creation of Internet memes for the US Presidential candidate Andrew Yang, and[OrochiDAO](https://medium.com/wearekickback/introducing-orochidao-and-the-year-of-daos-event-fb9c303b6a9e) , enabling event coordination in a decentralized way.


With the new Sovereign release, our vision is to provide visibility and facilitate the creation of DAOs that can incorporate different voting mechanisms.


**To kickstart things we have already worked on a**[Quadratic Voting Moloch](https://github.com/DemocracyEarth/quadratic-moloch) **** 👹 **.**


Press enter or click to view image in full size


In a modified version of quadratic voting, Colorado legislators each got 100 virtual tokens to buy votes on a number of measures. “There was a pretty clear signal on which items, which bills, were the most important for the caucus to fund,” says one state rep. JOE AMON/THE DENVER POST/GETTY IMAGES


Quadratic Voting is a mechanism pioneered by[Glen Weyl](https://twitter.com/glenweyl) (author of[Radical Markets](http://radicalmarkets.com/) ) and the[RadicalxChange](https://twitter.com/radxchange) movement. With QV voters are able to express the intensity of their preference as much as they want to in support of a cause, but zealotry is penalized at an exponential rate: more votes being put in the same option quickly exhaust the voter’s budget. By doing so, it provides a clear output on whether the intense preferences of minorities outweigh the weak preferences of the majority. Constituencies elaborate on their priorities and find a common ground, providing a more nuanced expression of collective intentions that helps prevent polarization. In April this year, Democracy Earth implemented[the first official Quadratic Vote in the U.S. Government for the State of Colorado.](https://www.bloomberg.com/news/articles/2019-05-01/a-new-way-of-voting-that-makes-zealotry-expensive)


Looking into the future, our aim is to transform sovereign into a DAO Browser, that can read and provide information on the many different protocols in the ecosystem. While traditional smart contracts are aimed mostly at delivering automated outcomes as they perform financial functions, DAOs differ by structuring a set of choices that cannot be automated away and thus require some form of conscious attention in their process. Being intended for governance, DAOs imprint human entropy on chain, offering a promising path as a substrate for an anti-Sybil registry.


While these are very early experiments, they can begin to inform different ways of constructing an identity that is both[intersectional](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3375436) , and[intersubjective](https://www.amazon.com/Making-Social-World-Structure-Civilization/dp/0195396170) .


Stay tuned!


Press enter or click to view image in full size


## Let’s keep talking


[Democracy.Earth](https://democracy.earth/) |[Twitter](https://twitter.com/DemocracyEarth) |[Git](http://bit.ly/DEFGit) |[Instagram](https://www.instagram.com/democracy.earth/) |[Telegram](https://t.me/democracyearthico) |[YouTube](http://bit.ly/DEFonYouTube) |[Subscribe](https://earth.us13.list-manage.com/subscribe?u=301becd69e00afa63c21122f3&id=67746cc019)


Democracy Earth Foundation is a 501(c)(3) nonprofit in San Francisco, California.[Democracy.Earth](https://earth.us13.list-manage.com/track/click?u=301becd69e00afa63c21122f3&id=13c3547c37&e=de07766081) .[On twitter](https://earth.us13.list-manage.com/track/click?u=301becd69e00afa63c21122f3&id=40f90d56cd&e=de07766081) .
Donate:[opencollective.com/democracyearth](https://earth.us13.list-manage.com/track/click?u=301becd69e00afa63c21122f3&id=8a6c110f51&e=de07766081)
ETH Support:
