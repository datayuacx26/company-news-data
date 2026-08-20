---
schema_version: "1.0.0"
document_id: "5787088060aca4d6277da5a9a7258cebc20c5367ae12bec8aca91edf4b9d626c"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-engineering-rss"
canonical_url: "https://engineering.gusto.com/building-resilient-mobile-apps-a-layered-testing-strategy-for-long-term-stability-d035c78bad31"
published_at: "2026-04-14T19:06:10+00:00"
first_seen_at: "2026-07-19T22:15:27.842622+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:d7a4a1c3ed20c506c6204190f8cbb9532d793fbfadf7801734ad46a871444039"
---

# Building Resilient Mobile Apps: A Layered Testing Strategy for Long-Term Stability

# **Building Resilient Mobile Apps: A Layered Testing Strategy for Long-Term Stability**


[Prasenjit Sinha](https://medium.com/@prasann_1433?source=post_page---byline--d035c78bad31---------------------------------------)


8 min read


·


Apr 14, 2026


--


Mobile teams shipping on a weekly cadence need automated confidence that changes won’t break what’s already working. At Gusto, our iOS codebase had unit tests, but coverage was uneven, and we hadn’t yet adopted snapshot testing. Major upgrades and component refactors still required significant manual verification. We wanted a more deliberate, layered approach — one that would scale with the team and the product.


The moment that sharpened our focus: a localization string change wasn’t caught by our existing automation and surfaced just one day before release. The fix took minutes — but the late discovery cost a full release cycle.


Manual testing doesn’t scale with growing teams and faster release cycles. We needed automation to own what humans couldn’t reliably catch across every release.


## What We Had Before — And Why It Wasn’t Enough


Our starting point wasn’t zero. We had unit tests, and we had UI tests. But having tests and having *useful* tests are two different things.


Unit test coverage was too low to catch most regressions. The UI tests relied on a third-party SDK for mocking that introduced its own complexity and maintenance burden. Flaky tests — the kind that fail randomly, not because something is actually broken — were adding noise to the suite. Over time, the signal-to-noise ratio made it harder to catch real issues. That’s not where we wanted to be.


The full test suite clocked in at one hour and fifteen minutes — a long wait for feedback on a weekly release cadence. And despite that time investment, regressions occasionally slipped through.


We stepped back and looked at this through the lens of the **testing pyramid** : a large base of fast, isolated unit tests; a middle layer of component-level tests; and a selective set of UI tests at the top for critical user flows. What we had was almost inverted — heavy on slow UI tests, light on the fast unit tests that should be doing the heavy lifting.


Rebuilding around the pyramid gave us a clear framework for what to fix and in what order.


## Our Layered Testing Strategy


## The Testing Pyramid: A Foundation for Mobile App Stability


## Unit Testing with XCTest


Our MVVM + Coordinator architecture turned out to be a genuine asset here. ViewModels and Coordinators are naturally isolated — they hold business logic, they depend on protocols, and they don’t care about the UI layer. That makes them ideal units to test.


We committed to covering every logic path in ViewModels and Coordinators. The key enabler was protocol-based mocking: instead of hitting real dependencies, each test injects a controlled fake that behaves exactly as needed for that scenario.


```text
// Protocol that the ViewModel depends on  protocol BookTicketServiceProtocol {    func fetchBookedTickets() async throws -> BookedTickets  }  // Mock for use in tests  class MockBookTicketService: BookTicketServiceProtocol {    var stubbedBookedTickets: BookedTickets = .mock    func fetchBookedTickets() async throws -> BookedTickets {      return stubbedBookedTickets    }  }   // Test  func testFetchBookedTicketsCorrectly() async throws {    let mockService = MockBookTicketService()    let viewModel = BookedTicketsViewModel(service: mockService)    await viewModel.loadSchedule()    XCTAssertEqual(viewModel.state, .loaded(mockService.stubbedBookedTickets))  }
```


This approach keeps tests deterministic, fast, and focused on what actually matters: does the business logic behave correctly given a specific input? Not whether the network is up, not whether the UI renders — just the logic.


## Snapshot Testing with swift-snapshot-testing


For visual regressions, we adopted[swift-snapshot-testing](https://github.com/pointfreeco/swift-snapshot-testing) by Point-Free. It’s become one of the most reliable tools in our testing stack.


The idea is straightforward: on the first run, the test renders a view and saves a reference image. On every subsequent run, it renders the view again and diffs it against the reference. Any pixel-level change — an unintended color shift, a layout regression after a component update, a font change from an iOS upgrade — fails the test immediately.


We use it for both SwiftUI views and UIViewController-based screens:


```text
import SnapshotTesting  import XCTest  class BookedTicketsViewSnapshotTests: XCTestCase {    func testBookedTicketsViewDefaultState() {      let view = BookedTicketsView(viewModel: .mockDefault)      // Records on first run, diffs on subsequent runs      assertSnapshot(of: view, as: .image(layout: .device(config: .iPhone13)))    }     func testBookedTicketsViewLoadingState() {      let view = BookedTicketsView(viewModel: .mockLoading)      assertSnapshot(of: view, as: .image(layout: .device(config: .iPhone13)))    }  }
```


These tests run on every PR. The first time a component regression slips in — whether from an iOS upgrade, a design token change, or an accidental layout tweak — the snapshot diff catches it before it ever reaches main.


**A note on repository size:** snapshot images add up fast. As our snapshot test count grew, so did the risk of bloating the repository with binary image files. We solved this with **Git LFS (Large File Storage)** . Instead of storing the actual image files in Git history, Git LFS stores them as lightweight pointer files — the images themselves live in a separate storage backend. The developer experience is identical, but our repository stays lean regardless of how many snapshot tests we add. It’s one of those infrastructure decisions that seems small until you realize it removed an entire category of “should we add more snapshot tests?” hesitation.


## UI Testing with XCUITest and API Stubbing


UI tests are expensive — slow to run, sensitive to timing, and prone to flakiness if they depend on real network calls. The key to making ours reliable was eliminating real network dependency entirely.


## Get Prasenjit Sinha’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


We inject a stubbed URLSession via protocol, allowing tests to control exactly what the network returns for any given request. This works for both REST and GraphQL calls:


```text
// URLSession abstraction  protocol URLSessionProtocol {    func data(for request: URLRequest) async throws -> (Data, URLResponse)  }   // Stub that returns controlled responses  class StubbedURLSession: URLSessionProtocol {    var stubbedResponses: [String: Data] = [:]    func data(for request: URLRequest) async throws -> (Data, URLResponse) {      let key = request.url?.path ?? ""      let data = stubbedResponses[key] ?? Data()      let response = HTTPURLResponse(        url: request.url!,         statusCode: 200,        httpVersion: nil,         headerFields: nil      )!      return (data, response)    }  }   // In XCUITest setup  func testLoginFlow() {    let app = XCUIApplication()    app.launchArguments += [" - useStubbedNetwork"]    app.launch()    app.textFields["emailField"].tap()    app.textFields["emailField"].typeText("test@gusto.com")    app.secureTextFields["passwordField"].typeText("password")    app.buttons["loginButton"].tap()    XCTAssertTrue(app.navigationBars["Home"].waitForExistence(timeout: 5))  }
```


With stubbed responses, our UI tests are fully deterministic. The same test run on a CI machine at 2am produces the same result as one run locally at noon. We focused our UI test coverage on the highest-impact flows first: Login, Time Tracking, and core financial operations. The goal is to expand coverage incrementally to the entire app — but starting with flows where a regression causes the most pain was the right call.


## CI Integration on Bitrise


Having good tests is only half the battle. Running them at the right times — and fast enough that people don’t route around them — is what turns a test suite into a genuine safety net.


Our Bitrise setup is built around two tiers with a specific performance strategy behind each.


**PR builds** run unit tests and snapshot tests only. These are the fast feedback loops that developers actually wait on during code review. To keep them snappy, we lean heavily on Bitrise’s derived data caching. By restoring cached build artifacts from previous runs, we avoid recompiling the world on every PR. The result is that developers get signals quickly enough that checking CI before merging feels natural rather than like a chore.


**Nightly builds** do the heavy lifting — the full suite of unit tests, snapshot tests, and UI tests runs every night. What makes this feasible without a painful runtime is parallel test execution. Rather than running test targets sequentially, we split them across concurrent runners. The entire combined suite — everything — completes in approximately **39 minutes** . For context, our old suite took 1 hour 15 minutes and covered far less ground. Parallelization is what makes comprehensive nightly coverage practical rather than aspirational.


The third piece is observability. Fast tests that nobody watches aren’t much better than no tests. We built two Slack-based monitors into the pipeline:


- **Flaky test monitor:** tracks tests that fail intermittently across runs. When the flaky count crosses a defined threshold, an alert fires to the team’s Slack channel. We treat flaky tests as bugs — they get triaged and fixed, not muted. A test that sometimes passes is just noise, and noise erodes trust in the entire suite.
- **Nightly build monitor:** if the nightly run fails, the team knows immediately via Slack — not the next morning when someone happens to check the dashboard. This matters especially for catching regressions that only surface under the full suite, like a UI test that breaks due to a server-side contract change.


Together, these monitors mean the suite is always being watched, and the right people are always in the loop when something breaks.


## What Changed — The Results


A month into this approach, the numbers told a clear story.


Unit test coverage grew by **6%** in the first month, with a clear upward trajectory. More importantly, the overall test suite runtime dropped from **1 hour 15 minutes to 39 minutes** — even with more tests added. Faster CI machines helped, but the bigger driver was the architectural shift: a larger proportion of fast unit tests replacing slow UI tests for scenarios that don’t need a full app launch.


The qualitative change was just as meaningful. Release cycles stopped being interrupted by late-discovered regressions. Engineers stopped dreading major iOS upgrades because snapshot tests would immediately surface anything that changed visually. The manual verification burden on the team shrank noticeably.


## Lessons Learned


The wins were real, but so were the tradeoffs. Snapshot tests require discipline — every intentional UI change means updating reference images, and that’s a workflow teams need to buy into. We chose to prioritize test stability over raw speed for UI tests, which meant investing in faster CI machines to keep total build times from growing. Neither of these was the wrong call, but they were deliberate choices. If we had to distill what we learned into principles:


- **Layer your tests by purpose.** Unit tests own logic. Snapshot tests own visual correctness. UI tests own critical user flows. Don’t ask one layer to do another’s job.
- **Stub your APIs for UI tests.** Real network calls in UI tests are a flakiness factory. Protocol injection isn’t complicated, and the reliability payoff is immediate.
- **Use Git LFS for snapshot images.** Don’t let repository size become a reason to avoid snapshot testing.
- **Treat flaky tests as bugs.** A test that sometimes passes is not a test — it’s noise.
- **Coverage is a direction, not a destination.** We went to increase the coverage by 6%. The target is to cover max possible areas in code. We’ll get there incrementally, and that’s fine.


The codebase we’re maintaining today is more testable, more observable, and more resilient than the one we started with. The work isn’t done — it never is — but the foundation is solid.


*If testing automation and software development are important to you too, we would love to*[hear from you](https://gusto.com/about/careers) *!*
