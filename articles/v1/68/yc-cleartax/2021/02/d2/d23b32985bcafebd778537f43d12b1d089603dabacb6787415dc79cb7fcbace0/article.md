---
schema_version: "1.0.0"
document_id: "d23b32985bcafebd778537f43d12b1d089603dabacb6787415dc79cb7fcbace0"
company_key: "yc-cleartax"
company: "Clear"
source_id: "yc-cleartax-rss-3da8b6d91e7d"
canonical_url: "https://medium.com/cleartax-engineering/on-bdd-testing-dialects-6e017d639fb9"
published_at: "2021-02-26T10:21:13+00:00"
first_seen_at: "2026-07-27T13:12:04.161420+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:dc3e5d410ed454412424d5eff84f40c8903195bba8beae799e01961fef021142"
---

# On BDD Testing Dialects

# On BDD Testing Dialects


[Ankit Solanki](https://medium.com/@_anks?source=post_page---byline--6e017d639fb9---------------------------------------)


3 min read


·


Feb 26, 2021


--


*Note: This was initially a discussion with a teammate on the pros and cons of using a BDD testing library. It was a substantive enough discussion that I decided to turn it into a blog post.*


Behaviour driven testing ([BDD](https://www.bddtesting.com/) ) is (generally) a way to write test cases in natural language, meant to bridge the gap between technical and business teams.


The most popular BDD tool is[cucumber](https://cucumber.io/) . There are also libraries in this vein for most platforms. My discussion with a teammate was around a JVM library called[JBehave](https://jbehave.org/) .


Here’s an example *scenario* from cucumber’s[getting started guide](https://cucumber.io/docs/guides/10-minute-tutorial/#write-a-scenario) :


```text
Feature: Is it Friday yet?    Everybody wants to know when it's Friday  Scenario: Sunday isn't Friday      Given today is Sunday      When I ask whether it's Friday yet      Then I should be told "Nope"
```


The promise of BDD is:


- Your tests are written in plain English.
- Non-technical members of the team can also collaborate with engineers — either read and verify that the test cases are *OK* , or even write the tests themselves.


This sounds very exciting. But in practice, I’m not so sure it works. Let’s go point-by-point.


## Will BDD really enable cross-team collaboration?


I’ve seen BDD tools be adopted with the promise of ‘allowing Product/QA teams to write their tests’ / ‘having an executable spec’. It rarely works out in practice though.


What ends up happening in most situations is that the development team will translate requirements into BDD syntax themselves, and then end up writing the code to pass these tests.


Even if your collaborators start off excited about ‘being able to write the executable spec’, they would soon run into subtle issues with syntax / limitations of the BDD DSL, etc. It *feels like* English, but you need to follow strict rules which doesn’t come naturally to anyone.


Simple tests would be easy to write by copy-pasting other existing tests, but more complex scenarios need intervention in the test framework. So the more complex scenarios will either go un-tested or the development team has to step-in and write those tests themselves.


**Suggestion** : Before going all-in, why not iterate on this for a few weeks? Teach someone how to write these tests, and collaborate over a 2–3 week period. Can your stakeholders continue to use the tool themselves? Are they willing to continue? Try low-risk experiment before betting big on a framework like this.


## Does BDD add value if developers are writing tests?


If you rule out collaboration with stakeholders, does it still make sense to use a BDD framework? I am pretty sceptical.


Most tools require you to define the *steps* used in your *scenarios* . For the[above example](https://cucumber.io/docs/guides/10-minute-tutorial/#see-scenario-reported-as-pending) , you have to define the implementation like so:


```text
public class Stepdefs {      private String today;      private String actualAnswer;      @Given("today is Sunday")      public void today_is_Sunday() {          today = "Sunday";      }      @When("I ask whether it's Friday yet")      public void i_ask_whether_it_s_Friday_yet() {          actualAnswer = IsItFriday.isItFriday(today);      }      @Then("I should be told {string}")      public void i_should_be_told(String expectedAnswer) {          assertEquals(expectedAnswer, actualAnswer);      }  }
```


Basically, you need to hook into the test framework and glue together the syntax used by the tool and your own business logic. You’re doing a lot of work to get the framework working.


Instead, consider creating[internal builders and helpers](https://medium.com/cleartax-engineering/a-dsl-for-testing-income-tax-calculations-f3a952b05221) to allow expressing your test cases in the language you’re already familiar with?


This actually takes us to the classic ‘Internal DSL’ vs ‘External DSL’ debate.


## Bonus: Internal vs External DSLs


- Internal DSLs work in the language you’re using, External DSLs define an new language.
- External DSLs will need a lexer, parser, interpreter or compiler.
- The **end user experience** for internal DSLs is as good as the language IDE you use — Auto-complete. Syntax highlighting. Highlighting errors. Lint warnings in code.
- You’re lucky if you get *syntax highlighting* for external DSLs.
- **Extending** internal DSLs is basically just refactoring + following good software design principles. You already know how to do this.
- Extending external DSLs need you to think in terms of building a language. Unless you build programming languages for a living, you won’t be familiar with this.
- Should you update the *syntax* of the language?
- Or, should you update the *standard library* of built in functions / operations available?


## Summary


I would usually want to avoid using such frameworks.


There may be some rare situations where BDD frameworks work out for you. I think they’re the wrong tool most of the time though.


Once you accept this fact, you can actually spend time thinking about how you can make unit testing easier for yourself! You can start by going through this (highly recommended!)[classic series of blog posts](http://www.natpryce.com/articles/000714.html) by Nat Pryce.


Thanks for Ashish Goyal for reading a draft of this.
