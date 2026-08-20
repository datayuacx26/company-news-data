---
schema_version: "1.0.0"
document_id: "43eec6367c1d2d680553377d4f24305a8f738617efe880addfa937618378f042"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/securing-your-defi-project-starts-with-quality-testing"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:2f3efb664c3ca31f0824960c33c3391bc04a8ba967313d7fd529c9d92fbbc8cd"
---

# Securing Your DeFi Project Starts with Quality Testing

## Securing Your DeFi Project Starts with Quality Testing


Recently, hacks resulting in the loss of over $26 million USD in value rocked several prominent DeFi projects including[bZx](https://cointelegraph.com/news/decentralized-lending-protocol-bzx-hacked-twice-in-a-matter-of-days) ,[Uniswap](https://defirate.com/imbtc-uniswap-hack/) ,[dForce / Lendf.Me](https://quantstamp.com/blog/how-the-dforce-hacker-used-reentrancy-to-steal-25-million) , and[Hegic](https://defirate.com/hegic-options/) . These losses may have been prevented through quality testing.


Tests are undervalued. Quantstamp has audited over 120 projects and secured over 2 billion USD worth of digital assets since 2017. Through our experience securing smart contracts, we noticed that developers highly underestimate the importance of test suites.


In order to promote secure smart contract development, this post will explain:


- The benefits of a quality test suite,
- Best practices for creating a test suite, and
- What a quality test suite looks like.


> Be very serious about tests. Write extensive unit tests and functional tests. Write tests about happy paths and try out all the edge cases that you might think of. Measure test coverage to help the process. Unfortunately, there are a lot of projects that don't even have any test.
>
>
> — Martinet Lee (@martinetlee)[April 20, 2020](https://twitter.com/martinetlee/status/1252039978630164480?ref_src=twsrc%5Etfw)


*Security Research Engineer Martinet Lee discussing how functional tests are an undervalued security practice after the dForce hack.*


### How tests can make your life easier


Currently, some developers experience feelings of dread when they are tasked with creating tests: however, there are many reasons why it is in your self-interest to excel in this skill. Quality testing saves you and your team time when maintaining code and reduces risk when adding new features. Testing is also a highly marketable skill. When you create quality tests, everyone wins.


*Your time is valuable. Save time with functional test suites.*


### Comprehensive technical specification


Before you create a test suite, you must first set the foundation for success by creating clear technical specifications. Writing comprehensive specifications is a best practice that rarely gets the attention it deserves.


*The image above contains*[ETH 2.0 specifications](https://github.com/ethereum/eth2.0-specs) *. Tables and graphs help explain non-obvious concepts to your fellow engineers.*


When writing your technical specification, it should include the functional requirements of your smart contract and UML diagrams that help explain non-obvious things. Never skip details because you assume that “the devs can figure it out.” For instance, make sure to explain in detail data structures and algorithms that do things like compute interest because, when it comes to complex computations, the devil is in the details.


After completing your specification, have it reviewed by at least two external individuals. Reviewer feedback should include anything that is unclear from a technical perspective and their concerns should be addressed before testing and code implementation begins.


Many consider this to be a tedious process, because it’s just cooler to start coding as soon as possible and figure out if the ideas actually work when implemented. However, writing clear technical specifications help you save time in the long run by:


- Providing a clear guide for both internal and external developers,
- Helping you write high quality tests, and
- Helping auditors audit the code more efficiently and effectively.


*Note: It is often overlooked that external auditors also need clear documentation in order to perform a quality audit in a timely fashion.*


### Creating a quality test suite


Now that we learned how to set the foundation for quality tests by understanding how to create quality documentation, we will explore what it takes to create a quality test suite.


#### Unit Tests


A unit test tests a single unit of code, such as a function. Unit tests are valuable because they allow you to test all edge cases on that unit. Unit tests will also catch some bugs that are not possible to catch during integration and functional tests.


When you create a unit test, you select inputs with the intention of verifying that these inputs always produce the expected output. The quality of your unit tests is highly dependent on your selection of these inputs. Selecting expected inputs is pretty straightforward, but the best testers are skilled at selecting unexpected inputs, because these are the inputs that are likely to lead to bugs in your codebase.


Good unexpected inputs are things that people wouldn’t think of trying. For instance, if you have a string input, try:


- The empty string,
- An extremely long string, and
- Including special characters.


Or, if you have an integer input, try negative values, the maximum integer, and 0.


#### Integration Tests


An integration test tests a combination of units. When isolated, units may be bug free, but once they interoperate, they may still produce unexpected results. When creating integration tests, aim to integrate as many units as possible; however, keep in mind that the more units you integrate, the harder it will be to locate the root cause of a failed test.


There is a simple strategy for integration tests: only integrate units that will interact or influence each other in the final system that is being built. For example, if you are building a system that has two main roles, say buyer and supplier, then it doesn’t make sense to integrate a function from the supplier role with a function from the buyer role that will never interact with each other (i.e. are totally independent from each other). An example of two functions that would be independent from each other in theaforementioned system could be` getBuyerName` and` computeSupplierInterest` . It would not make sense to write an integration test where you integrate these two


#### Functional Tests


*Here is an example of a complex functional test that simulates multiple users exploring both “happy and unhappy paths.”*


A functional test tests the whole system. It is sometimes called user-story testing because such tests should be directly translated into code from the user-stories written during the requirements design phase at the beginning of the project. Requirements (or user-stories) are an important part of the technical specification document(s), which we mentioned in an earlier section of this article. Therefore, functional tests aim to verify if the system requirements hold.


Such tests are arguably very important, because even if all the unit and integration tests pass, a failure in functional tests indicates a problem for the business value of the system, since it does not satisfy all requirements. Conversely, if the test suite encodes all functional requirements and all functional tests pass successfully, then having a few failing unit or integration tests is not as severe as having a failing functional test.


At Quantstamp, we like to take things up a notch. Therefore, we develop what we like to call “complex functional tests,” where we don’t just test one user-story in isolation. Instead, we combine and intertwine as many user-stories as possible (ideally all stories) inside one test file. To increase the chances of detecting bugs in real-world scenarios, we also involve multiple user accounts having both the same role and different roles and different goals. Plus the state (e.g. balance, amounts) of these users would involve non-round fund values (e.g. 1.23456789 ETH). Moreover, in such tests it is important to not only test the happy paths, but also the unhappy paths (e.g. where transactions are expected to fail).


### Understand that 100% line and branch coverage is not enough


Remember, 100% line and branch coverage does not guarantee that all edge cases are covered and that all interactions are safe.


The goal of writing tests is to catch bugs, not to reach 100% line and branch coverage. You should still strive to reach 100% test coverage; just understand that this does not eliminate the possibility of bugs within your codebase. In other words, 100% line and branch coverage does not guarantee that all edge cases are covered and that all interactions are safe. The reason being that even if the tests do not contain any assertions and exercise the code, the level of coverage will still be the same. The two JavaScript code snippets from the figure below illustrate this perfectly:


1. The first code snippet represents “Test1” which performs a couple of calls to 2 smart contracts and then asserts whether the effects of those smart contract calls on the balance and liquidity are as expected.
2. The second code snippet represents “Test2” which performs the same 2 smart contract calls as “Test1,” however, it does not contain any assertions.


Both “Test1” and “Test2” lead to the same amount of code coverage. However, “Test2” is clearly not effective at catching bugs, because it does not check that the effects of the executed code are as expected.


```text


it("Test1: should allow a liquidity provider to deposit funds", async function() {
await daiToken.approve(coverageContract.address, LIQUIDITY_AMOUNT1);
await coverageContract.provide(POOL_INDEX, LIQUIDITY_AMOUNT1);
Utils.assertEqBN(
await dataContract.getLiquidityLeftDai(POOL_INDEX), LIQUIDITY_AMOUNT1);
Utils.assertEqBN(
await dataContract.getTotalBalanceDai(POOL_INDEX), LIQUIDITY_AMOUNT1);
});


it("Test2: should allow a liquidity provider to deposit funds", async function() {
await daiToken.approve(coverageContract.address, LIQUIDITY_AMOUNT1);
await coverageContract.provide(POOL_INDEX, LIQUIDITY_AMOUNT1);
});


```


### When a test fails, get to the bottom of it


When a test fails, it is possible that the test failed because there is a bug in the test itself. Developers are sometimes tempted to assume that the bug was in the test, so they may adjust test assertions until the test passes. This is futile and will lead to software that has bugs!


### Make sure tests cover the behavior correctly


Consider the following` createPool(...)` function that reverts in two cases:


1. ` msg.sender` is not an admin, and
2. When a pool with the given name already exists:


```text


function createPool(string poolName) public {
require(msg.sender == adminAddress, "Only admin is allowed");
require(poolNameToPoolIndex[poolName] == 0, "Pool already exists");
...
}


```


The following test intends to verify that the function reverts when a pool already exists:


```text


it("reverts when pool already exists", async function() {
await Utils.assertTxFail(() => coverageContract.createPool("Existing pool", {from: accounts[1]}));
});


```


However, when the test executes,` createPool(...)` , in fact, fails for a different reason: the address at` accounts\[1\]` is not an admin. Therefore, the uniqueness check is not covered by the given test, in spite of its intention to do so. Such a test is lacking quality and does not give developers confidence.


The following, fixed, test follows a better practice: it asserts that the function reverts for the right reason. Also note that` accounts\[0\]` is now used instead of` accounts\[1\]` :


```text


it("reverts when pool already exists", async function() {
await Utils.assertTxFail(() => coverageContract.createPool("Existing pool", {from: accounts[0]}),
"Pool already exists");
})


```


‍


Such a test reassures developers that the pool uniqueness check is indeed covered in tests.


### Don’t write tests just to reach 100% coverage, write tests to find bugs


Having a high-quality test suite--one that includes unit, integration and functional tests--is essential for DeFi projects. Tests should have assertions that check effects of the executed code in case of a successful transaction as well as a revert message in case of a rejected transaction. Writing such assertions would be cumbersome without a clear technical specification that lists all system requirements. Don’t write tests just to reach 100% coverage, write tests to find bugs. *‍*


*This post was written by Quantstamp Senior Research Engineer Sebastian Banescu, Ph.D, Senior Software Engineer Alex Murashkin, and Quantstamp Staff Writer Julian Martinez.*
