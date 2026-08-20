---
schema_version: "1.0.0"
document_id: "009d5d5f64a49028a57a0da27ae7cb57496b3445d9e9fb96237f3d5f01180c8f"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/the-benefit-of-functional-test-suites"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:8157bab0610b91bfa71316c46f1876c64ed85d82e7b2f2b2a58ddc6678de3ee4"
---

# The Benefit of Functional Test Suites

# **The Benefit of Functional Test Suites**


Quantstamp has audited close to 100 projects to date. In the audit reports we provide to our clients, we include a section that describes the quality of the test suite associated with that project and we often suggest improvements. Unfortunately, such recommendations are often overlooked as audits are performed close to the launch deadline, and significant time and effort are required to improve test suites.


A comprehensive test suite covers all use cases and as much of the implemented functionality as possible, and is able to immediately detect changes in functionality and prevent bugs from being introduced into the master branch. On the other hand, a test suite that does not cover all business use cases may lead to the introduction of functionality bugs when making changes to the code.


One such bug was recently introduced during our engagement with the rDai project. Quantstamp provided a non-security related suggestion to improve the maintainability of the code and save gas in the “Best Practices” section of the[audit report](https://certificate.quantstamp.com/view/r-token-ethereum-contracts) , namely: “The statement on lines 645 and 648 can be brought before the if-statement on L640 and the else-clause could be then removed.”


At first glance this seems like a natural optimization[in the code](https://github.com/rtoken-project/rtoken-contracts/commit/015bd105347abf966bbf3e39120bc08af2183226?diff=unified#diff-57d03f6876ac58f8cd24a399a705cb1cR665-R672) (Note that the line numbers in our recommendation do not match those in the screenshot below because, by the time the screenshot was taken, other changes had been made to the code. Just add 26 to each of the line numbers above to obtain the line numbers in the screenshot):


‍


‍


However, this code change introduced a functional bug that allowed some interest to be leaked, because the \`estimateAndRecollectLoans\` function called another function that was dependent on the value of \`account.hatID\`. The rDai team conducted a preliminary analysis of this bug and found its impact to be limited due to the small number of addresses affected and because the total amount of leaked interest is quite small.


As mentioned in the “Code Coverage” section of the[audit report](https://certificate.quantstamp.com/view/r-token-ethereum-contracts) the test suite did not have a high enough coverage, which we recommended be increased. Unfortunately, this recommendation was not addressed immediately, which also meant that this functional bug was not detected by any failing tests. We understand that most clients want to improve their functional test suites; however, they do not always have the capacity to do so. Therefore, we have decided to lend a helping hand.


Quantstamp now offers functional test suite development as a service. If you are developing a blockchain application, Quantstamp can provide you with a customized functional test suite that covers all business use cases for your application.


### **The Process**


In order to develop the tests, clients first need to provide us with documentation and diagrams that clearly depict the appropriate functional behaviors they expect to see in their application. For example, the rDai project provided us with the list below so we would have the necessary information to start developing a tailor made functional test. rDai is a project that allows users to generate interest off of their Dai and give that interest to a specific person or organization.


‍


Test flow for a single functional test in the rDai smart contracts:


- A user creates a Hat and designates the appropriate recipients to collect interest according to specified rates
- Owner mints tokens with the previously created Hat
- Time passes and recipients accumulate interest
- After a while, the recipients balance is checked to see if it generates the expected results
- Owner changes the hat to change the recipients of the interest
- Interest continues to accumulate
- New and old recipients correctly reflect the amount of interest accumulated


Quantstamp then developed a functional test that matched the desired functional behaviors outlined by rDai (PR with the code is here[https://github.com/rtoken-project/rtoken-contracts/pull/24](https://github.com/rtoken-project/rtoken-contracts/pull/24) ). This test is able to detect a functional bug that may be introduced in subsequent code changes, e.g. if a change was made that allowed old hat recipients to continue accumulating interest, even though they were not supposed to, this test would fail indicating that a functional bug was introduced.


The complex functional test described above is just an example and could be part of an entire test suite that Quantstamp is able to develop for your project. Quantstamp has also developed an additional test (see[https://github.com/rtoken-project/rtoken-contracts/pull/29](https://github.com/rtoken-project/rtoken-contracts/pull/29) ) for the rDai project, with the aim of covering functionality that was not covered by any other test in the test suite. If you are interested in learning more about the functional test suite development services offered by Quantstamp, click on the link below.


*This post was written by Quantstamp Senior Research Engineer Sebastian Banescu, Ph.D and Quantstamp Staff Writer Julian Martinez.*


--


For more Quantstamp news or anything QSP crypto or QSP coin related, check out Quantstamp[Reddit](https://www.reddit.com/r/Quantstamp/) and QSP[Twitter](https://twitter.com/quantstamp) .
