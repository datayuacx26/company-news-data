---
schema_version: "1.0.0"
document_id: "f08d6363b52ac09641a4f3b7e6ec91face4726dfb4b8406b79796cb349391b01"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/data-validation-what-to-check-and-where-to-enforce-it/"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T14:25:21.590403+00:00"
fetched_at: "2026-08-03T16:10:47.640296+00:00"
content_hash: "sha256:6c9be84eb4f4f03551d546af59d35f4bd2d14fc12b1cd300a3e996767ba3be16"
---

# Data validation: what to check and where to enforce it

Data validation is the practice of checking that data satisfies a set of rules before you store it, sync it, or report on it. A validation rule is a statement that must be true:` email` is present and looks like an email,` amount` is a positive number,` status` is one of a fixed list, every` order.customer_id` points at a real customer. If a row breaks a rule, you catch it at the point of failure instead of discovering it later as a wrong number on a dashboard.


The key decision in validation is not what to check but where to check it. The same rule can live in a database constraint, in application code, in a pipeline test, or in the BI layer, and each catches problems at a different moment. The general principle: enforce each rule as close to where the data is written as you can, and re-check the handful of rules the source cannot enforce at the point where the data is consumed.


This guide is for analysts, data engineers, and operators who have been told to “make sure the data is clean” and want something more concrete. It covers what validation is, the main types of validation rules, where to enforce each one, how validation differs from reconciliation and quality monitoring, and the mistakes that let bad data through.


## What is data validation?


Data validation is the process of confirming that data conforms to defined rules for type, format, range, completeness, uniqueness, and business logic. It answers one question: does this record satisfy the constraints we require before we are willing to trust it?


Validation is a gate, not a report. A validation check has a pass or fail outcome for each row or each batch. It is best run at the moment data enters a system: a form submission, an API write, a pipeline load, a warehouse insert. The earlier the gate, the cheaper the fix, because a value rejected at write time never propagates into downstream tables, joins, and dashboards.


It helps to separate validation from two adjacent practices it often gets confused with:


- **Validation** asks “does this single record follow our rules?” It is rule-based and usually runs at write or load time.
- **[Reconciliation](https://www.basedash.com/blog/data-reconciliation-how-to-find-why-two-numbers-dont-match)** asks “do these two numbers that should agree actually agree?” It compares two sources after the fact.
- **[Quality monitoring](https://www.basedash.com/blog/data-quality-metrics-what-to-measure-and-how-to-track-it)** asks “how trustworthy is this dataset over time?” It measures properties like completeness and freshness on a schedule and trends them.


A record can pass validation and still fail reconciliation, because two clean datasets can measure different things. And a dataset can be individually valid row by row and still drift in aggregate, which is what monitoring catches. You need all three, but they are not interchangeable.


## The main types of validation rules


Almost every validation rule falls into one of six categories. Naming them helps because it turns “check the data” into a concrete list you can implement.


Rule type What it checks Example


Type and format The value is the right data type and shape` signup_date` is a valid date;` email` matches an email pattern


Range and boundary The value falls within allowed limits` age` between 0 and 120;` discount_pct` between 0 and 100


Presence and completeness Required fields are not null or empty` customer_id` is never null;` country` is present on every address


Uniqueness No unintended duplicates` email` is unique per account; one` subscription` per active plan


Referential and consistency Values line up across tables or fields every` order.customer_id` exists in` customers` ;` ship_date` is after` order_date`


Business logic Domain rules the data must obey a refund cannot exceed the original charge;` status` moves only through allowed transitions


The first four are structural and easy to express as constraints. The last two, referential integrity and business logic, are where most real bugs hide, because they depend on relationships and rules that a single column type cannot capture.


A practical way to build your rule set is to walk each critical column and ask the six questions in order: what type is it, what range is legal, can it be missing, must it be unique, what does it reference, and what business rule governs it. Most columns need two or three rules, not all six.


## Where to enforce each rule


This is the part teams get wrong. They pick one layer, usually the pipeline or the BI tool, and try to validate everything there. The better model is a ladder: each rung catches a class of problem, and rules belong on the lowest rung that can enforce them.


Layer When it runs Best for Tradeoff


Database constraints On every write, synchronously Type, not-null, uniqueness, referential integrity, simple range checks Rejects the write outright; needs schema access and migrations


Application code Before the write, in your product Business logic, cross-field rules, user-friendly error messages Only covers writes that go through your app, not direct loads


Ingestion and pipeline tests On each load or transform run Batch checks, accepted values, freshness, referential checks across sources Catches problems after the data has landed, not before


BI and consumption layer At query or dashboard time Sanity checks on the final numbers, alerts on impossible values Last line of defense; the bad data already exists by now


The rule of thumb: **enforce as early as possible, and re-check at the point of consumption only what earlier layers cannot guarantee.**


Databases are the strongest and most underused rung. A` NOT NULL` ,` UNIQUE` ,` CHECK` , or` FOREIGN KEY` constraint is enforced on every write, by every client, with no code to maintain. PostgreSQL, for example, supports check constraints, not-null constraints, unique constraints, and foreign keys directly on the table, and the database refuses any write that violates them ([PostgreSQL constraints](https://www.postgresql.org/docs/current/ddl-constraints.html) ). If a rule can be a database constraint, it usually should be, because nothing gets past it.


The application layer is where cross-field and stateful business rules belong, since that is where you have the full context of a request and can return a helpful error. But application checks only guard data that flows through the application. Anything that lands through a bulk import, a direct SQL insert, or a third-party sync bypasses them, which is why you still want database constraints underneath.


Pipeline tests catch what the source systems cannot, especially when you are combining data from several places. In dbt, generic tests like` not_null` ,` unique` ,` accepted_values` , and` relationships` run as part of the build and fail the run when the data breaks the expectation ([dbt data tests](https://docs.getdbt.com/docs/build/data-tests) ). This rung is essential for a warehouse fed by many sources, because no single upstream system can enforce a rule that spans them.


The BI layer is the last line, not the first. By the time a number is on a dashboard, invalid data has already been stored and joined. Consumption-layer validation is worth having as a backstop, for example an alert when a KPI goes negative or a count drops to zero, but it should be catching the rare escape, not doing the primary work.


## Implementation patterns


A few patterns make validation durable instead of a one-off cleanup.


**Reject at the boundary, quarantine in batch.** For synchronous writes, reject the bad record and return an error so the caller fixes it. For batch loads where you cannot reject a whole file, route failing rows to a quarantine table with the reason attached, load the good rows, and review the quarantine. This keeps one bad row from blocking a million good ones while still surfacing the problem.


**Make the rule a query.** Any validation you cannot express as a check you cannot automate. “Emails should be valid” becomes` select count(*) from users where email !~ '^\[^@\]+@\[^@\]+\\.\[^@\]+$'` . Once a rule is a query, it can run in a constraint, a pipeline test, or a scheduled monitor.


**Validate at the grain you write.** Row-level rules (type, range, presence) belong on individual rows. Set-level rules (uniqueness, referential integrity, “exactly one active subscription per account”) need a check across rows, so express them as aggregates or constraints, not per-row logic.


**Fail loudly, then trend.** A failed validation should page someone or block a deploy, not write a warning to a log nobody reads. Once the check is trustworthy, trend its pass rate so you can see a source degrading before it breaks.


## A data validation checklist


When you are hardening a new table or dataset, walk this list:


- Every required column has a` NOT NULL` constraint or equivalent.
- Every natural key has a` UNIQUE` constraint; every foreign key is declared and enforced.
- Numeric and date columns have range or boundary checks where a nonsensical value is possible.
- Enum-like columns are constrained to their allowed set (accepted values), not free text.
- Cross-field rules (end after start, refund not exceeding charge) are checked where you have both fields.
- Money is a decimal type, not a float, so validation and sums stay exact.
- Bulk loads route failing rows to a quarantine with a reason, not silent drops.
- Each rule that spans sources has a pipeline test, since no single source can enforce it.
- The most important final numbers have a consumption-layer sanity alert as a backstop.
- Every rule is expressible as a query, so it can be automated and trended.


## Common mistakes


- **Validating only in the application.** Any data that arrives through an import or a direct write skips app-layer checks. Put the structural rules in the database so nothing bypasses them.
- **Treating the BI tool as the validator.** By the dashboard, the bad data is already stored. Consumption checks are a backstop, not the primary gate.
- **Storing enums as free text.** “active”, “Active”, and “ACTIVE” are three different values to a database. Constrain the allowed set at write time.
- **Using floats for money.** Floating-point storage drifts totals and breaks equality checks. Use a decimal type so both validation and aggregation stay exact.
- **Silently dropping bad rows.** Discarding failing rows without recording them hides the size and cause of the problem. Quarantine with a reason instead.
- **Over-validating low-stakes data.** Wrapping every exploratory table in strict constraints slows iteration for no benefit. Save heavy validation for data that feeds decisions, billing, or customers.


## When not to over-validate


Validation has a cost: constraints require migrations, pipeline tests take time to run, and rejected writes can block legitimate work if the rules are too strict. Match the rigor to the stakes.


Validate strictly when the data feeds billing, finance, investor reporting, customer-facing dashboards, or anything where a wrong value has real consequences. These deserve database constraints, pipeline tests, and a consumption backstop.


Validate lightly for exploratory analysis, early product metrics still being defined, and internal scratch tables. Over-constraining data that nobody is going to act on is friction without payoff. You can always tighten the rules once a dataset graduates into something people depend on. This is also where a[single source of truth for definitions](https://www.basedash.com/blog/what-is-a-single-source-of-truth-and-how-to-build-one-for-analytics) pays off: when the rules live in one place, you can promote a rule from “nice to have” to “enforced” without hunting for every copy of it.


## How BI tools fit in


Most validation should happen upstream of your BI tool, in the database and pipeline. But the BI layer is where invalid data becomes visible, so it is a useful place to write the sanity checks that confirm your upstream validation is working: a saved query that counts nulls in a key column, a chart that should never go negative, an alert when a daily count falls to zero.


It also helps when the tool sits directly on your source data. If your dashboard runs live SQL against the same database as your production tables, you can write a validation query, see the offending rows, and confirm a constraint is doing its job without exporting anything.[Basedash](https://www.basedash.com/) works this way: it connects directly to your production database or warehouse, so the same place you build a chart is where you can run a` count(*)` on rows that break a rule and inspect them one click later. It is one option among many, and for enforcing rules you will still want database constraints and pipeline tests doing the heavy lifting, but keeping validation queries next to the data makes the backstop easy to maintain.


## FAQ


**What is the difference between data validation and data cleaning?** Validation checks whether data follows the rules and flags or rejects what does not. Cleaning transforms data to fix problems, like trimming whitespace, standardizing casing, or filling defaults. Validation is a gate that says pass or fail; cleaning is a step that changes values. Many pipelines clean first, then validate what remains.


**Where should data validation happen?** As early as possible. Structural rules (type, not-null, uniqueness, referential integrity) belong in database constraints so no write can bypass them. Business and cross-field rules that need request context belong in the application. Rules that span multiple sources belong in pipeline tests. The BI layer should only hold sanity backstops on the final numbers.


**What are the main types of data validation?** Type and format, range and boundary, presence and completeness, uniqueness, referential and consistency, and business logic. The first four are structural and map cleanly to database constraints. Referential and business-logic rules depend on relationships across tables or fields, which is where most real bugs live.


**Is data validation the same as data quality?** No. Validation is a rule-based gate applied at write or load time to individual records. Data quality is a broader, ongoing measure of how trustworthy a dataset is across dimensions like completeness, accuracy, and timeliness. Validation is one of the mechanisms that keeps quality high, but quality is monitored over time while validation happens at the boundary.


**How do I validate data in a warehouse fed by many sources?** Use pipeline tests at the transform layer, because no single upstream source can enforce a rule that spans systems. Tools like dbt provide generic tests for not-null, uniqueness, accepted values, and referential relationships that run on each build and fail the run when the data breaks them. Add a few consumption-layer alerts on the final metrics as a backstop.
