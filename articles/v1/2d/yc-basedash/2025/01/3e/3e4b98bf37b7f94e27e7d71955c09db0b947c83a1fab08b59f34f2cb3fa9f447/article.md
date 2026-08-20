---
schema_version: "1.0.0"
document_id: "3e4b98bf37b7f94e27e7d71955c09db0b947c83a1fab08b59f34f2cb3fa9f447"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-enable-row-level-security-rls-in-postgresql/"
published_at: "2025-01-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:0b7333b8f7ea5ce3f7a8aa2cad5142959aeae0f6ec2a6e6573ec2da9c143aa7d"
---

# How to enable row-level security (RLS) in PostgreSQL

## What is Row-Level Security (RLS)?


Row-level security (RLS) is a feature in PostgreSQL that allows you to define policies to restrict access to individual rows in a table. This ensures that users can only access or modify data that they’re authorized to see.


## Enabling RLS on a table


First, let’s turn on RLS for a specific table. Here, I’ll assume a table named` employees` .


```text
ALTER   TABLE   employees   ENABLE   ROW   LEVEL   SECURITY  ;
```


By default, this will block all access to the table unless a policy is applied. To still allow the table owner access, you can use:


```text
ALTER   TABLE   employees   FORCE   ROW   LEVEL   SECURITY  ;
```


## Create an RLS policy


A policy dictates what data can be accessed by whom. Let’s create a policy to allow users to see only the rows where the` department` matches their role.


```text
CREATE   POLICY   employee_policy
ON   employees
FOR   SELECT
USING   (department   =   current_setting(  'my_app.user_role'  ));
```


## Apply an RLS policy to table


After creating a policy, apply it to the table using` ALTER TABLE` .


```text
ALTER   TABLE   employees   FORCE   ROW   LEVEL   SECURITY  ;
ALTER   TABLE   employees   ALTER   POLICY   employee_policy   USING   (department   =   current_setting(  'my_app.user_role'  ));
```


## Test RLS


You can test whether RLS is working by changing the current setting for the role and trying a SELECT query.


```text
SET   my_app  .  user_role   =   'Engineering'  ;
SELECT   *   FROM   employees;
```


## Using variables for dynamic RLS policies


You can use session variables to make the policies dynamic.


```text
```


Then, whenever a user logs in, set the session variable to their role.


```text
SET   my_app  .  user_role   =   '<user-role>'  ;
```


## When to Use RLS


RLS is best for multi-tenant applications or applications with different user roles that require access to subsets of data. It’s not recommended for systems with extremely high throughput because it might add performance overhead.


For access management workflows,[Basedash](https://www.basedash.com/) complements SQL controls with governed reporting, shared visibility, and AI-assisted analysis so teams can monitor permission changes without losing auditability.


## Alternatives to RLS


If RLS doesn’t fit your needs, consider using other PostgreSQL features like Views or Table Partitioning for achieving similar results.
