---
schema_version: "1.0.0"
document_id: "cf16e73ff809d75c1d3fb517377a31bec7009e3f1543399488d20112d0ca22ab"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/sql-style-guide-conventions-that-keep-team-queries-readable/"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T14:46:37.423505+00:00"
fetched_at: "2026-08-04T15:13:53.558824+00:00"
content_hash: "sha256:8e02776e92f66c8431b1ca523390fc8ce7fbca9e6ef7947db1cfcc969b045e26"
---

# The SQL style guide: conventions that keep team queries readable

A SQL style guide is a short set of rules a team agrees on for how to name, format, and structure queries. It covers things like whether keywords are uppercase or lowercase, how to indent, how to name columns and CTEs, and when to break a query into steps. The point is not that one style is correct. The point is that everyone on the team writes SQL the same way, so any query is fast to read, review, and reuse.


This guide is for analysts, engineers, and operators who write SQL that other people have to read: shared dashboard queries, dbt models, ad hoc analysis that gets pasted into a doc, or a query someone else will edit six months from now. If your SQL only ever runs once and no one else sees it, style matters less. The moment a query is shared, consistency starts saving real time.


## The short version


If you only take away a few rules, take these:


- Pick one keyword case (all lowercase or all uppercase) and never mix.
- Use` snake_case` for every table, column, and alias.
- Put each selected column, each join, and each filter on its own line.
- Build queries out of CTEs (` with` blocks), not nested subqueries.
- Always write` as` when you alias a column or table.
- Write comments that explain why, not what.


Everything below is the longer version, plus where well-known published guides disagree and how to get a team to actually follow the rules.


## Why a shared style guide is worth the effort


The value of a style guide shows up in three places.


**Review speed.** When every query is formatted the same way, a reviewer reads structure instead of decoding formatting. A diff that only changes logic is easy to approve. A diff where someone also re-indented the whole file hides the real change.


**Onboarding.** A new analyst who inherits a consistent codebase can predict where things are: imports at the top, one CTE per logical step, the final` select` at the bottom. Inconsistent SQL forces them to re-learn each author’s personal habits.


**Reuse.** In most teams, queries get copied. A well-named, well-structured query is safe to lift into a new dashboard. A dense, single-block query with aliases like` t1` and` x` gets rewritten from scratch, which is how two slightly different versions of the same metric end up in production.


None of this requires the “best” style. It requires a style everyone shares.


## Naming conventions


Naming is where a style guide earns its keep, because names are what people read first.


Element Convention Example


Tables` snake_case` , collective or plural noun` orders` ,` customers`


Columns` snake_case` , singular, no table prefix` order_total` ,` email`


Booleans` is_` or` has_` prefix` is_active` ,` has_paid`


Dates and timestamps` _date` or` _at` suffix` signup_date` ,` created_at`


CTEs verbose, describe the transformation` paid_orders` ,` revenue_by_month`


Computed columns name it as if it were a real column` sum(amount) as total_revenue`


A few rules that prevent most naming arguments:


- Avoid` camelCase` . It is harder to scan than` snake_case` , which is the identifier convention both major public guides recommend ([sqlstyle.guide](https://www.sqlstyle.guide/) ,[dbt Labs](https://docs.getdbt.com/best-practices/how-we-style/2-how-we-style-our-sql) ).
- Skip descriptive prefixes like` tbl_` or` sp_` . The database already knows what is a table.
- Do not give a column the same name as its table, or vice versa.
- Name a CTE after what it does, not what it holds.` events_joined_to_users` describes a step.` user_events` sounds like a table.


## Formatting and layout


Formatting rules are the ones people fight about and the ones a machine can enforce for you. The specific choices matter less than making them once.


A workable default:


- **Keyword case:** lowercase everything (` select` ,` from` ,` left join` ). Pick the opposite if you prefer, but decide.
- **Indentation:** a consistent width, commonly two or four spaces. Never tabs mixed with spaces.
- **Commas:** trailing commas (at the end of the line), one column per line.
- **Line length:** keep lines readable, roughly 80 to 100 characters, so queries do not scroll sideways in a pull request.
- **One thing per line:** each selected column, each` join` , and each condition in a multi-part` where` on its own line.


Here is the same query written two ways. Messy:


```text
select   id,  name  ,  sum  (amt) total   from   Orders o   join   Customers c   on   o  .  cid  =  c  .  id   where   c  .  Country  =  'US'   group by   1  ,  2
```


Clean:


```text
with   us_customers   as   (
select
customer_id,
name
from   customers
where   country   =   'US'
),


customer_orders   as   (
select
customer_id,
sum  (amount)   as   total_revenue
from   orders
group by   customer_id
)


select
us_customers  .  customer_id  ,
us_customers  .  name  ,
customer_orders  .  total_revenue
from   us_customers
left join   customer_orders
on   us_customers  .  customer_id   =   customer_orders  .  customer_id
```


Both return the same rows. Only the second one is safe to hand to a teammate.


## Query structure: prefer CTEs over nested subqueries


The single biggest readability win in SQL is building a query out of named steps. A common-table-expression (` with` block) lets you name each stage of the work and read the query top to bottom.


Two structural conventions from the dbt style guide are worth adopting directly:


- **Import CTEs at the top.** Reference each source table once in a named CTE, select only the columns you need, and filter early. The rest of the query works off those, so the source references live in one place.
- **One CTE, one job.** Each CTE should do a single logical unit of work, and its name should say what that is. If a CTE is doing three things, split it.


Compare this to the alternative, where a` select` sits inside a` from` sits inside another` select` . Nested subqueries force you to read inside-out, and the intermediate results have no names. CTEs read in the order the work happens, and you can comment out the final` select` and swap in` select * from any_cte` to inspect any step while you build.


## Joins and aliases


Joins are where ambiguity creeps in, so a few explicit rules help:


- **Be explicit about join type.** Write` inner join` , not bare` join` .
- **Prefix columns when more than one table is in play.**` orders.customer_id` is unambiguous. A bare` customer_id` in a two-table join makes the reader guess.
- **Always use` as` for aliases.** It is explicit and easy to scan.
- **Move left to right.** Prefer` left join` and reorder your` from` and` join` so the flow reads in one direction. A` right join` is often a sign you should swap which table you select from.


Aliases are one place where good guides genuinely disagree, covered next.


## Comments: explain why, not what


Good SQL is mostly self-documenting if it is named and structured well, so comments should carry the information the code cannot.


- Comment the *why* : a strange filter (` -- exclude internal test accounts, see ticket 4821` ), a business rule, a known data quirk, or an intentional edge case.
- Skip the *what* :` -- select the customer id` adds nothing next to` select customer_id` .
- Keep a one-line note at the top of a saved or shared query stating what it answers and its grain (one row per what).


That last habit pays off most on queries that power a dashboard, where the next person needs to know the grain before they trust the number.


## Where good SQL style guides disagree


It helps to see that even the most cited public guides make opposite choices. This is the strongest argument for the “pick one and be consistent” principle: there is no universal right answer, only a team default.


Decision[sqlstyle.guide](https://www.sqlstyle.guide/) (Simon Holywell)[dbt Labs](https://docs.getdbt.com/best-practices/how-we-style/2-how-we-style-our-sql)


Keyword case UPPERCASE (` SELECT` ) lowercase (` select` )


Indentation right-aligned “river” down the middle 4 spaces, left-aligned


` group by` list the columns explicitly` group by 1, 2` (by position)


Table aliases short correlation from first letters avoid initialisms, prefix the full table name


Identifiers` snake_case`` snake_case`


Explicit` as` required required


They agree on the things that most affect readability (` snake_case` identifiers, explicit` as` ) and diverge on the things that are mostly taste (case, alignment,` group by` style). When your team writes its own guide, spend your debate budget on the rows where they agree and just pick a side on the rows where they do not.


One note on the` group by` row: grouping by position (` group by 1, 2` ) is terse and common in analytics code, but it breaks silently if someone reorders the` select` list. Grouping by explicit column name is more verbose and more robust. Either is fine. Mixing them in the same codebase is not.


## A starter SQL style guide you can copy


Drop this into your team wiki or the top of your dbt repo and edit the choices you disagree with. The value is having a written default, not the specific rows.


**Naming**


- ` snake_case` for all tables, columns, and aliases
- Singular column names, plural or collective table names
- ` is_` /` has_` for booleans,` _at` /` _date` for time columns
- No` tbl_` /` sp_` prefixes, no` camelCase`
- CTE names describe the transformation, not the contents


**Formatting**


- Lowercase keywords
- Four-space indentation, no tabs
- One column, join, or condition per line
- Trailing commas
- Lines under ~100 characters


**Structure**


- Source tables referenced once, in import CTEs at the top
- One logical step per CTE
- CTEs over nested subqueries
- Explicit join types (` inner join` ,` left join` )
- Prefix column names in multi-table queries
- Explicit` as` for every alias


**Comments**


- Comment the why, not the what
- One-line header on shared queries stating the question and grain


## How to actually get a team to follow it


A style guide that lives only in a doc gets ignored. Enforcement should be as automatic as possible so no one spends review time arguing about commas.


- **Use a formatter and a linter.**[SQLFluff](https://sqlfluff.com/) is a configurable SQL linter and auto-formatter that can enforce most of the rules above, and dbt projects can wire it into the workflow. A formatter settles the taste questions mechanically.
- **Lint in CI.** Run the linter on every pull request so style violations fail before a human looks at the diff. This keeps review focused on logic.
- **Keep the guide next to the code.** A markdown file in the repo, or a short section in your dbt project, beats a wiki page nobody opens.
- **Standardize where SQL is written and shared.** When queries live in a shared workspace instead of scattered across laptops, conventions spread by example. Tools like[Basedash](https://www.basedash.com/) let a team write SQL, turn it into charts, and share the underlying query, so a well-styled query becomes the template the next person copies. If you are still choosing where analysts write queries, see our roundup of[the best SQL editors](https://www.basedash.com/blog/best-sql-editors-in-2026) .


The habit that ties it together: review AI-generated SQL against the same guide. Assistants produce working SQL that ignores your conventions, so treat their output like any other contribution. Our[checklist for reviewing AI-generated SQL](https://www.basedash.com/blog/how-to-review-ai-generated-sql-a-checklist-for-analysts-and-operators) covers what to check beyond formatting.


## Common mistakes


- **Writing the guide but not enforcing it.** Without a linter in CI, the guide decays within weeks.
- **Over-specifying.** A 40-rule guide no one remembers is worse than six rules everyone follows. Start small.
- **Re-formatting in logic commits.** Reformatting a file in the same pull request that changes its logic buries the real change. Format in a separate commit.
- **Cryptic aliases.**` t1` ,` t2` , and` x` save keystrokes and cost every future reader time. Alias to something meaningful.
- **One giant query.** A 200-line query with no CTEs is technically correct and practically unreviewable. Break it into named steps.
- **Debating taste for hours.** Case and alignment do not affect correctness. Flip a coin, write it down, move on.


## When to relax the rules


Style is a means, not an end. Loosen it when the cost outweighs the benefit:


- **Throwaway exploration.** A quick` select *` to eyeball a table does not need CTEs and a header comment.
- **Interactive debugging.** While you are actively poking at data, optimize for speed of iteration, then clean up anything you save or share.
- **Generated SQL.** Query builders and BI tools emit SQL that will never be hand-edited. Do not hold machine output to a human-readability standard.
- **Vendor-specific performance work.** Sometimes the readable form and the fast form differ. When they do, favor performance and leave a comment explaining the trade-off.


The test is simple: will another person read or edit this query? If yes, style it. If no, do what is fastest.


## FAQ


**Should SQL keywords be uppercase or lowercase?**


Either works, and the two most-cited public guides split on it: sqlstyle.guide uses uppercase, dbt Labs uses lowercase. What matters is that your team picks one and applies it everywhere. Lowercase has become common in modern analytics codebases because it is faster to type and reads well alongside lowercase identifiers, but uppercase keywords make the SQL skeleton stand out. Choose one, put it in your guide, and let a formatter enforce it.


**What is the difference between a SQL style guide and a linter?**


The style guide is the set of rules your team agrees on. A linter like SQLFluff is the tool that checks and often auto-fixes SQL against those rules. The guide is the decision, the linter is the enforcement. You want both: the guide so people understand the intent, and the linter so the rules hold up without manual policing in every review.


**Should I use CTEs or subqueries?**


Prefer CTEs (` with` blocks) for anything a person will read. CTEs let you name each step and read the query top to bottom, while nested subqueries force inside-out reading and leave intermediate results unnamed. Modern databases plan CTEs and equivalent subqueries similarly in most cases, so the choice is mostly about readability. Reach for a subquery only for a small, obvious, single-use expression.


**How do I get a team to follow a SQL style guide?**


Automate it. Keep the guide in the repo next to the code, run a formatter and linter like SQLFluff in CI so violations fail the pull request before a human reviews it, and standardize where queries are written and shared so well-styled queries become the copy-paste template. Manual enforcement through code review alone rarely lasts, because reviewers get tired of flagging commas.


**Does SQL style matter if a BI tool generates the query?**


For the machine-generated SQL itself, no: query builders and BI tools emit SQL that no one hand-edits, so readability rules do not apply to it. Style matters for the SQL your team writes by hand, including the custom queries you write inside a BI tool to power a chart or dashboard. Those are shared, reused, and reviewed like any other code, so the same conventions apply.
