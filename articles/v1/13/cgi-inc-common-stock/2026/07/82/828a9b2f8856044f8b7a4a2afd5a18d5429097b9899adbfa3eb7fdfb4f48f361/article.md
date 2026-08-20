---
schema_version: "1.0.0"
document_id: "828a9b2f8856044f8b7a4a2afd5a18d5429097b9899adbfa3eb7fdfb4f48f361"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/utilities/centralized-vs-blockchain-a-head-to-head-comparison-of-storage-requirements"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:b3c15492d21a756205a21dffc3211db7d7e473330e404aa903b73198e9e5ea50"
---

# Centralized vs. blockchain: A head-to-head comparison of storage requirements

In an[earlier blog post, we addressed blockchain energy dependency](https://www.cgi.com/en/blog/utilities/not-all-blockchains-are-equal-when-it-comes-to-energy-consumption) . This time we take a look at blockchain storage requirements. Indeed, blockchain’s distributed data structure results in a significantly higher storage demand compared to traditional centralized databases. However, central systems often suffer from a lack of trust, resulting in “hidden” data silos and often additional labor costs. To explore this further, let’s zoom in on a real-life use case to compare a centrally governed system and a future blockchain-based system.


## The use case


Three producers of residual heat deliver to an energy wholesaler. The wholesaler is the network owner and, as such, the administrator tasked with ensuring that the producers are paid according to how much energy is delivered. To facilitate this, the wholesaler monitors and registers each producer’s energy input. However, as all the parties have a stake in accurate data, each producer also keeps its own production records. As a result of a lack of trust with the wholesaler’s centralized system to track this data, additional silos of data are created by the producers. This then requires more storage space.


## Centralized data governance


The data needed to correctly invoice the network consists of hourly data points with a timestamp and the quantity of produced thermal energy in megawatt hours. The file size of the spreadsheet containing this data is 430 KB. Next to this, each producing party has its own smaller data file, adding three additional files, and leading to a total storage requirement of 1.08 MB.


In case of a discrepancy between one of the producer’s and the wholesaler’s data set, storage requirements will rise. Emails with file attachments will be sent back and forth. The different files will have to be compared and will also be stored. In the end, conflicting data like this will result in multiple email exchanges containing different data files, doubling the original storage requirement with each exchange. This scenario demonstrates that central databases often require more storage than just an initial file. There is also no telling how many errors will occur or how many iterations will be required to resolve them. Still, the resulting storage requirement will be far lower than what is needed for a distributed database, as we will demonstrate below. The point is that these errors will need to be dealt with by people, resulting in additional labor costs and errors. Even without conflicts, each party has to have its own data redundancy strategy, which will require expensive personnel to ensure the strategies are implemented and maintained.


## A blockchain implementation


In our blockchain case, each of the stakeholders ran their own open source blockchain node on their digital premises to partake in the consensus algorithm in which to track the data. Each node runs an identical application with all of the business logic that determines how transactions between nodes are resolved. The data structure which is used to reach an autonomous data consensus consists of several components. The total storage requirement for these components is 42 MB per node. In our use case, this means 42 MB storage capacity per participant, resulting in a total use case storage requirement of 168 MB. This is a staggering increase compared to the requirements of the centralized database.


However, this increase is not completely unjustifiable. In the blockchain case, the resulting data is 100% fault-proof and validated by all participants and therefore not prone to tedious conflict resolution. Moreover, the blockchain implementation under investigation uses advanced encryption to prevent unwanted data leakage to network participants, which increases the total storage requirement significantly. On top of that, there is no longer a single point of failure. As the participants reach a consensus on the data, should one node lose its data, it can automatically retrieve it from the other nodes in the network. This data redundancy is a built-in feature of blockchain and requires no additional resources to be maintained—it is part of the implementation. Finally, a lot of space is taken up by the autonomous process of reaching data consensus. It should be kept in mind that over time, the storage requirement for reaching that consensus will increase even without adding actual data to the blockchain.


## Conclusion


In this case, and probably most others, we can conclude that a blockchain network will require much more storage than when the data is governed centrally—even with the unpredictable pattern of error creation in a system with no trust. Nonetheless, with a steadily decreasing average price per GB, it might be a small price to pay for a *trustless** data source, with autonomous conflict resolution and no single point of failure. For those systems where storage cost does pose a problem, a centralized solution is a more efficient option, but will always require a trusted third party (solution) to implement.


In the end, the blockchain system contained much more than just the raw data; it also contained everything required to reach consensus and keep a valid data layer accessible to all participants. If the system increases in complexity, for instance by adding new market players, so will the costs arising from resolving conflicts in a centralized solution. For the blockchain implementation, however, these costs will stay constant, adding only to the storage requirement. All in all, there is a lot more to implementing blockchain than just the storage requirement aspect. The choice to implement (or not) should always be made after a careful consideration of all the pros and cons, as is the case with all technologies.


CGI believes that blockchain will prove useful in enabling, or even accelerating, the energy transition. Read[our whitepaper on blockchain for utilities](https://www.cgi.com/en/media/white-paper/opportunities-for-blockchain-in-the-energy-sector) or[this thesis on the use of blockchain in decentralized energy systems](https://www.cgi.com/nl/nl/artikelen/electric-vehicles-the-driving-power-for-energy-transition) for more insight into our blockchain vision.


*Blockchain doesn’t necessarily create a trusted system. It reduces the need for trust from the individual participants. For more on this, read the article["What do we mean by "blockchains are trustless?"](https://medium.com/@preethikasireddy/eli5-what-do-we-mean-by-blockchains-are-trustless-aa420635d5f6) .
