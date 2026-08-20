---
schema_version: "1.0.0"
document_id: "0f48ad561870f897f700e72c7f85db92005bad537fe4b9b82668c5237734fe81"
company_key: "wealthfront-corporation-common-stock"
company: "Wealthfront Corporation"
source_id: "wealthfront-corporation-common-stock-rss-9fbe6ef385e3"
canonical_url: "https://eng.wealthfront.com/2025/11/20/sealed-tests/"
published_at: "2025-11-20T16:33:33+00:00"
first_seen_at: "2026-07-20T23:18:22.615485+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:160b2f2d6bee5cd5abb087bd4abed9616d7891dc708c178e85790c9f13f03e34"
---

# Automating Exhaustive Branch Coverage for Sealed Types in Kotlin

We take code correctness and code quality seriously at Wealthfront. One of the most effective ways we ensure that our code is correct is through rigorous unit testing and strong typing.


In Kotlin, **sealed classes** and **sealed interfaces** offer a powerful tool to model restricted class hierarchies. They provide strong guarantees about the types in a hierarchy and enable exhaustive when expressions: a switch statement syntax for enforcing that all types in a sealed hierarchy are handled. However, when unit testing functions which take sealed parameters as inputs, there isn’t an equivalent mechanism in JUnit to enforce that all types in a particular hierarchy have an associated test. Parameterized tests come close. However, the parameterized test author is still responsible for supplying the set of parameters that get passed to the test function, and it does not guarantee that all variants of sealed parameters are included in that set.


Historically we have relied on code review to identify and remedy insufficient test coverage. While it’s easy to miss a particular case when writing tests, we can automate the enforcement of test coverage for functions that take sealed parameters. In this blog, we’ll explore how to build an **Exhaustive Test Runner** that ensures there is at least one test case for each subclass of a Kotlin sealed type.


### **Why Test Sealed Types Exhaustively?**


Kotlin’s sealed types are useful when you want to define a closed set of related types, where only a predefined set of subtypes can exist. For example, in an Android application, you might have a sealed class representing different types of deep links that the app can handle:


```text
sealed    interface    DeepLink     {
data    class    TransferMoney   ( val   destinationAccountId: String) : DeepLink
data    class    AccountOverview   ( val   accountId: String) : DeepLink
data    object   Blogs : DeepLink
}
Code language:    Kotlin    (  kotlin  )
```


This sealed class hierarchy ensures that only TransferMoney, AccountOverview, and Blogs can be used as types of deep links, and the compiler can ensure that all deep links are handled exhaustively in when expressions. However, when writing unit tests for functions that accept a sealed parameter, there are no compiler guarantees that all the branches of the when statement have test coverage.


An **exhaustive test case coverage** strategy for sealed types ensures that each subtype has at least one unit test case. This approach guarantees that all subtypes are considered, and no potential case is left untested. More importantly, this strategy guarantees that all future types added to the sealed hierarchy have sufficient coverage. This is especially useful for sealed hierarchies which we expect to grow over time.


### **Building a Custom JUnit Test Runner**


To enforce exhaustive test coverage, we need a custom JUnit Test Runner. The custom runner will compare the set of tests declared in the test class with the set of sealed subtypes in the associated type hierarchy and ensure that there is at least one test method for each subtype. Test authors will specify the sealed hierarchy that should be enforced using an @Exhaustive annotation.


#### **Extending** **Suite**


We begin by creating a custom Test Runner that extends Suite, a JUnit-provided test runner that supports running groups of tests.


```text
annotation    class    Exhaustive   (
val   sealedType: KClass<*>,
)


@Repeatable
annotation    class    ExhaustiveCase   (
val   sealedType: KClass<*>,
val   test: KClass<*>,
)


class    ExhaustiveTestRunner   (testClass: Class<*>) : Suite(testClass, getTests(testClass)) {


companion    object   {


private    fun    getTests  (testClass:  Class  <*>)   : Array<Class<*>> {
check(testClass.isAnnotationPresent(Exhaustive:: class  . java  ))   {
"ExhaustiveTestRunner's test class must be annotated with @Exhaustive"
}


val   exhaustive = testClass.getAnnotation(Exhaustive:: class  . java  )!!
val   sealedType = exhaustive.sealedType
check(sealedType.isSealed) {
"@Exhaustive class parameter must represent a sealed type"
}


val   cases = testClass.getAnnotationsByType(ExhaustiveCase:: class  . java  )
check(cases.contentEquals(cases.sortedBy { it.sealedType.simpleName }.toTypedArray())) {
"ExhaustiveCase must be in alphabetical order"
}


check(cases.contentEquals(cases.distinctBy { it.sealedType }.toTypedArray())) {
"ExhaustiveCase must be distinct by key"
}


check(cases.contentEquals(cases.distinctBy { it.test }.toTypedArray())) {
"ExhaustiveCase must be distinct by value"
}


val   testLookup = cases.associate { it.sealedType to it.test }
testLookup.keys.forEach { key ->
check(key.isSubclassOf(sealedType)) {
"Expected ExhaustiveCase key to be a subtype of  $sealedType  "
}
}


return   sealedType.sealedSubclasses
.map { sealedSubclass ->
checkNotNull(testLookup[sealedSubclass]) {
"Missing required exhaustive case for type  $sealedSubclass  "
}
}
.map(KClass<*>::java)
.toTypedArray()
}
}
}
Code language:    Kotlin    (  kotlin  )
```


In this custom runner, we populate the parent class Suite with test cases given by the @ExhaustiveCase annotation, and validate that there is one @ExhaustiveCase specified for each sealed subtype given by the kotlin.reflect.KClass.sealedSubclasses property.


#### **Using the Custom Test Runner**


To use the custom Test Runner, we simply need to annotate our test class with @RunWith(ExhaustiveTestRunner::class). This ensures that the custom runner is used to execute the tests in that class. The test author is then responsible for specifying the sealed type hierarchy to scan, and the test cases for those sealed subtypes.


```text
@RunWith(ExhaustiveTestRunner::class)
@Exhaustive(DeepLink::class)
@ExhaustiveCase(DeepLink.TransferMoney::class, DeepLinkTest.TransferMoneyTest::class)
@ExhaustiveCase(DeepLink.AccountOverview::class, DeepLinkTest.AccountOverviewTest::class)
class    DeepLinkTest     {


class    TransferMoneyTest     {
@Test
fun    testTransferMoney  ()    {
// ...
}
}


class    AccountOverviewTest     {
@Test
fun    testAccountOverview  ()    {
// ...
}
}


}
Code language:    Kotlin    (  kotlin  )
```


In the example above, we have tests for TransferMoney and AccountOverview, but we’re missing a test for the Blogs object. When the tests are executed, our custom Test Runner will throw an error indicating that no test case was found for the Blogs subtype. An important flexibility of this design is that the per-subtype test classes can use their own, distinct test runner. For example, TransferMoneyTest can use the RobolectricTestRunner if needed.


### **Benefits of the Exhaustive Test Runner**


1. **Enforces Exhaustive Testing** : The custom runner ensures that no subtype of a sealed class is left untested, reducing the risk of overlooked edge cases.
2. **Automated Validation** : With this runner in place, the validation of test coverage for sealed classes is automatic, eliminating the need for certain manual checks by engineers.
3. **Improved Code Quality** : By enforcing tests for all subtypes, we are more likely to catch bugs and edge cases early, improving the quality and reliability of our code.
4. **Prevents Common Pitfalls** : In large codebases with many sealed types, manually tracking which subtypes have been tested can be error-prone. This runner eliminates this issue.
5. **Documentation** : Like most testing, the exhaustive tests provide accurate and up-to-date documentation for how a particular sub-system is working and what inputs and outputs are expected.


### **Saving Time With Automation**


With an automated approach to verifying that all subtypes of sealed types have corresponding test cases, we can save time by reducing the amount of back-and-forth in code review discussions. Code reviewers no longer need to inspect whether all branches of a sealed type are covered; the custom runner enforces this rule automatically.


By leveraging this “meta-testing” infrastructure, we ensure that our tests are not only complete but that this completeness is verified without manual intervention. This speeds up the review process by ensuring a high quality bar prior to review.


### **Conclusion**


Kotlin’s sealed types provide powerful guarantees that can simplify complex domain modeling. However, ensuring that all subtypes of a sealed class are tested can be a challenge. By implementing a custom JUnit Test Runner, we can automate the enforcement of exhaustive test case coverage, ensuring that every sealed type has a corresponding test case.


This approach helps avoid bugs that could result from untested subtypes, providing greater confidence in the correctness of our code. Additionally, the automated enforcement of test coverage reduces the time spent during code reviews, making the process more efficient and less prone to human error. For our business, where precision and reliability are critical, this system has been an invaluable addition to our testing infrastructure.


---


# Disclosures


The information contained in this communication is provided for general informational purposes only, and should not be construed as investment or tax advice. Nothing in this communication should be construed as a solicitation or offer, or recommendation, to buy or sell any security. Any links provided to other server sites are offered as a matter of convenience and are not intended to imply that Wealthfront Corporation or its affiliates endorses, sponsors, promotes and/or is affiliated with the owners of or participants in those sites, or endorses any information contained on those sites, unless expressly stated otherwise.


Investment advisory services are provided by Wealthfront Advisers LLC, an SEC-registered investment adviser. Brokerage services are provided by Wealthfront Brokerage LLC, Member[FINRA](https://www.finra.org/) /[SIPC](https://www.sipc.org/) . Financial planning tools are provided by Wealthfront Software LLC.


All investing involves risk, including the possible loss of money you invest, and past performance does not guarantee success. Please see our[Full Disclosure](https://www.wealthfront.com/legal/disclosure) for important details.


Wealthfront Advisers LLC, Wealthfront Brokerage LLC, and Wealthfront Software LLC are wholly owned subsidiaries of Wealthfront Corporation.


© 2025 Wealthfront Corporation. All rights reserved.
