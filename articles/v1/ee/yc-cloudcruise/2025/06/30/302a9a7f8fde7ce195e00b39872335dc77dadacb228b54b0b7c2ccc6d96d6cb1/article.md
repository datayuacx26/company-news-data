---
schema_version: "1.0.0"
document_id: "302a9a7f8fde7ce195e00b39872335dc77dadacb228b54b0b7c2ccc6d96d6cb1"
company_key: "yc-cloudcruise"
company: "CloudCruise"
source_id: "yc-cloudcruise-news-import-0286802dbbee"
canonical_url: "https://cloudcruise.com/blog/badger"
published_at: "2025-06-26T00:00:00+00:00"
first_seen_at: "2026-07-23T05:51:05.060428+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:832b39fb7307ec2d880523271c644e1c6bb045b609466bc0d23efd351e52be2f"
---

# BADGER: Graph-Based Browser Automation | CloudCruise

#### Reliable Browser Automation ≠ One-Off Scripts


Playwright is fantastic until your quick automation task devolves into hundreds of lines of loops, retries, and messy conditionals.


We hit this exact wall building robust form-filling bots. Instead of piling on more hacks, we asked: **What if automation logic was a graph you could see, not a script you had to trace?**


The result is BADGER (Browser Automation Directed Graph Engine Ruleset) – a workflow DSL built around explicit, maintainable graphs of browser actions. You can find the full types on our[Github](https://github.com/CloudCruise/BADGER) .


#### Why Not Just Use Playwright Directly?


Playwright is powerful and intuitive—perfect for quick scripts. But when automations grow beyond simple use-cases, scripts quickly become:


-


**Hard to maintain:** Logic, retries, loops, and edge cases spiral into tangled, fragile code.


-


**Painful to debug:** Tracing errors through imperative scripts wastes valuable developer time.


-


**Poorly reusable:** Mixing logic directly with implementation details makes reusing code difficult.


BADGER was designed to solve for this.


#### Automation as Graphs, Not Scripts


BADGER addresses Playwright's scalability limitations by modeling automation as explicit, declarative graphs. Instead of burying control flow in imperative code, workflows become visible structures you can inspect, version, and debug node-by-node.


Each workflow becomes a directed graph composed of:


-


**Nodes** : Clearly defined browser actions (e.g., CLICK, INPUT_TEXT, BOOL_CONDITION), each with a single responsibility.


-


**Edges** : Explicit control flow connections that visually and programmatically represent conditions, loops, and execution paths.


#### Key Benefits


**Easy Debugging** : Pinpoint failures within individual nodes and graph paths.


-


**Reusable Components:** Build modular automation libraries with reusable nodes.


-


**Visual Clarity:** Graph structure makes complex workflows easy to understand and maintain.


Here's an example for a browser agent that posts a comment on X.


```text
{
"id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"workspace_id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"version_id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"version_number"  :    1  ,
"created_at"  :    "2025-05-21T00:00:00Z"  ,
"created_by"  :    "user"  ,
"name"  :    "X: Comment"  ,
"description"  :    "Comment on a X tweet"  ,
"input_schema"  :    {
"$TWEET_URL"  :    {
"type"  :    "string"  ,
"pattern"  :    "^https://x.com/[^/]+/status/[^/]+$"
}  ,
"$COMMENT"  :    {
"type"  :    "string"
}  ,
"$USER_NAME"  :    {
"type"  :    "string"
}  ,
"$PASSWORD"  :    {
"type"  :    "string"
}  ,
"required"  :    [  "$TWEET_URL"  ,    "$COMMENT"  ,    "$USER_NAME"  ,    "$PASSWORD"  ]
}  ,
"output_schema"  :    {  }  ,
"nodes"  :    [
{
"id"  :    "41a01974-5d96-403d-bd4c-21f786bacf2b"  ,
"name"  :    "Navigate to X Tweet"  ,
"action"  :    "START"  ,
"parameters"  :    {
"url"  :    "{$TWEET_URL}"
}
}  ,
{
"id"  :    "9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"  ,
"name"  :    "Is logged in?"  ,
"action"  :    "BOOL_CONDITION"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"comparison_value_1"  :    "<<xpath://input[@autocomplete='username']>>"  ,
"comparison_value_2"  :    null  ,
"comparison_operator"  :    "IS_NULL"  ,
"wait_time"  :    5000  ,
"clear_cookies_on_false"  :    true
}
}  ,
{
"id"  :    "c73ffce4-4f02-42ca-bff8-afdc816e3ca6"  ,
"name"  :    "Type username"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//input[@autocomplete='username']"  ,
"text"  :    "{$USER_NAME}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "74052c6d-51be-4f1b-9fd5-304b5632eee3"  ,
"name"  :    "Click Next"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Next']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "75f38414-62c8-453a-87d8-684847db0783"  ,
"name"  :    "Type password"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//input[@type='password']"  ,
"text"  :    "{$PASSWORD}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"  ,
"name"  :    "Click Log In"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Log in']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"  ,
"name"  :    "Enter Comment"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//div[@data-testid='tweetTextarea_0' and @aria-label='Post text']"  ,
"text"  :    "{$COMMENT}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "b0ddd3b4-5197-4389-aedc-be37f96c1d44"  ,
"name"  :    "Hit Reply"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Reply']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "e406fd72-85f9-4a28-bd09-0753cd8528fd"  ,
"name"  :    "End"  ,
"action"  :    "END"  ,
"parameters"  :    {  }
}
]  ,
"edges"  :    {
"41a01974-5d96-403d-bd4c-21f786bacf2b"  :    {
"to"  :    "9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"
}  ,
"9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"  :    {
"true"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"  ,
"false"  :    "c73ffce4-4f02-42ca-bff8-afdc816e3ca6"
}  ,
"c73ffce4-4f02-42ca-bff8-afdc816e3ca6"  :    {
"to"  :    "74052c6d-51be-4f1b-9fd5-304b5632eee3"
}  ,
"74052c6d-51be-4f1b-9fd5-304b5632eee3"  :    {
"to"  :    "75f38414-62c8-453a-87d8-684847db0783"
}  ,
"75f38414-62c8-453a-87d8-684847db0783"  :    {
"to"  :    "c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"
}  ,
"25382829-81ae-43bc-b7c6-1d6be6a09cfb"  :    {
"to"  :    "b0ddd3b4-5197-4389-aedc-be37f96c1d44"
}  ,
"c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"  :    {
"to"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"
}  ,
"b0ddd3b4-5197-4389-aedc-be37f96c1d44"  :    {
"to"  :    "e406fd72-85f9-4a28-bd09-0753cd8528fd"
}
}  ,
"use_native_actions"  :    true  ,
"video_record_session"  :    true  ,
"encrypted_keys"  :    [  ]  ,
"popup_xpaths"  :    [  ]  ,
"auth_urls"  :    [  ]  ,
"error_codes"  :    [  ]
}
```


```text
{
"id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"workspace_id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"version_id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"version_number"  :    1  ,
"created_at"  :    "2025-05-21T00:00:00Z"  ,
"created_by"  :    "user"  ,
"name"  :    "X: Comment"  ,
"description"  :    "Comment on a X tweet"  ,
"input_schema"  :    {
"$TWEET_URL"  :    {
"type"  :    "string"  ,
"pattern"  :    "^https://x.com/[^/]+/status/[^/]+$"
}  ,
"$COMMENT"  :    {
"type"  :    "string"
}  ,
"$USER_NAME"  :    {
"type"  :    "string"
}  ,
"$PASSWORD"  :    {
"type"  :    "string"
}  ,
}  ,
"output_schema"  :    {  }  ,
"nodes"  :    [
{
"id"  :    "41a01974-5d96-403d-bd4c-21f786bacf2b"  ,
"name"  :    "Navigate to X Tweet"  ,
"action"  :    "START"  ,
"parameters"  :    {
"url"  :    "{$TWEET_URL}"
}
}  ,
{
"id"  :    "9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"  ,
"name"  :    "Is logged in?"  ,
"action"  :    "BOOL_CONDITION"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"comparison_value_1"  :    "<<xpath://input[@autocomplete='username']>>"  ,
"comparison_value_2"  :    null  ,
"comparison_operator"  :    "IS_NULL"  ,
"wait_time"  :    5000  ,
"clear_cookies_on_false"  :    true
}
}  ,
{
"id"  :    "c73ffce4-4f02-42ca-bff8-afdc816e3ca6"  ,
"name"  :    "Type username"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//input[@autocomplete='username']"  ,
"text"  :    "{$USER_NAME}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "74052c6d-51be-4f1b-9fd5-304b5632eee3"  ,
"name"  :    "Click Next"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Next']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "75f38414-62c8-453a-87d8-684847db0783"  ,
"name"  :    "Type password"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//input[@type='password']"  ,
"text"  :    "{$PASSWORD}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"  ,
"name"  :    "Click Log In"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Log in']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"  ,
"name"  :    "Enter Comment"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"text"  :    "{$COMMENT}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "b0ddd3b4-5197-4389-aedc-be37f96c1d44"  ,
"name"  :    "Hit Reply"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Reply']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "e406fd72-85f9-4a28-bd09-0753cd8528fd"  ,
"name"  :    "End"  ,
"action"  :    "END"  ,
"parameters"  :    {  }
}
]  ,
"edges"  :    {
"41a01974-5d96-403d-bd4c-21f786bacf2b"  :    {
"to"  :    "9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"
}  ,
"9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"  :    {
"true"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"  ,
"false"  :    "c73ffce4-4f02-42ca-bff8-afdc816e3ca6"
}  ,
"c73ffce4-4f02-42ca-bff8-afdc816e3ca6"  :    {
"to"  :    "74052c6d-51be-4f1b-9fd5-304b5632eee3"
}  ,
"74052c6d-51be-4f1b-9fd5-304b5632eee3"  :    {
"to"  :    "75f38414-62c8-453a-87d8-684847db0783"
}  ,
"75f38414-62c8-453a-87d8-684847db0783"  :    {
"to"  :    "c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"
}  ,
"25382829-81ae-43bc-b7c6-1d6be6a09cfb"  :    {
"to"  :    "b0ddd3b4-5197-4389-aedc-be37f96c1d44"
}  ,
"c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"  :    {
"to"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"
}  ,
"b0ddd3b4-5197-4389-aedc-be37f96c1d44"  :    {
"to"  :    "e406fd72-85f9-4a28-bd09-0753cd8528fd"
}
}  ,
"use_native_actions"  :    true  ,
"video_record_session"  :    true  ,
"encrypted_keys"  :    [  ]  ,
"popup_xpaths"  :    [  ]  ,
"auth_urls"  :    [  ]  ,
"error_codes"  :    [  ]
}
```


```text
{
"id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"workspace_id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"version_id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"version_number"  :    1  ,
"created_at"  :    "2025-05-21T00:00:00Z"  ,
"created_by"  :    "user"  ,
"name"  :    "X: Comment"  ,
"description"  :    "Comment on a X tweet"  ,
"input_schema"  :    {
"$TWEET_URL"  :    {
"type"  :    "string"  ,
"pattern"  :    "^https://x.com/[^/]+/status/[^/]+$"
}  ,
"$COMMENT"  :    {
"type"  :    "string"
}  ,
"$USER_NAME"  :    {
"type"  :    "string"
}  ,
"$PASSWORD"  :    {
"type"  :    "string"
}  ,
}  ,
"output_schema"  :    {  }  ,
"nodes"  :    [
{
"id"  :    "41a01974-5d96-403d-bd4c-21f786bacf2b"  ,
"name"  :    "Navigate to X Tweet"  ,
"action"  :    "START"  ,
"parameters"  :    {
"url"  :    "{$TWEET_URL}"
}
}  ,
{
"id"  :    "9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"  ,
"name"  :    "Is logged in?"  ,
"action"  :    "BOOL_CONDITION"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"comparison_value_1"  :    "<<xpath://input[@autocomplete='username']>>"  ,
"comparison_value_2"  :    null  ,
"comparison_operator"  :    "IS_NULL"  ,
"wait_time"  :    5000  ,
"clear_cookies_on_false"  :    true
}
}  ,
{
"id"  :    "c73ffce4-4f02-42ca-bff8-afdc816e3ca6"  ,
"name"  :    "Type username"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//input[@autocomplete='username']"  ,
"text"  :    "{$USER_NAME}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "74052c6d-51be-4f1b-9fd5-304b5632eee3"  ,
"name"  :    "Click Next"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Next']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "75f38414-62c8-453a-87d8-684847db0783"  ,
"name"  :    "Type password"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//input[@type='password']"  ,
"text"  :    "{$PASSWORD}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"  ,
"name"  :    "Click Log In"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Log in']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"  ,
"name"  :    "Enter Comment"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"text"  :    "{$COMMENT}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "b0ddd3b4-5197-4389-aedc-be37f96c1d44"  ,
"name"  :    "Hit Reply"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Reply']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "e406fd72-85f9-4a28-bd09-0753cd8528fd"  ,
"name"  :    "End"  ,
"action"  :    "END"  ,
"parameters"  :    {  }
}
]  ,
"edges"  :    {
"41a01974-5d96-403d-bd4c-21f786bacf2b"  :    {
"to"  :    "9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"
}  ,
"9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"  :    {
"true"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"  ,
"false"  :    "c73ffce4-4f02-42ca-bff8-afdc816e3ca6"
}  ,
"c73ffce4-4f02-42ca-bff8-afdc816e3ca6"  :    {
"to"  :    "74052c6d-51be-4f1b-9fd5-304b5632eee3"
}  ,
"74052c6d-51be-4f1b-9fd5-304b5632eee3"  :    {
"to"  :    "75f38414-62c8-453a-87d8-684847db0783"
}  ,
"75f38414-62c8-453a-87d8-684847db0783"  :    {
"to"  :    "c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"
}  ,
"25382829-81ae-43bc-b7c6-1d6be6a09cfb"  :    {
"to"  :    "b0ddd3b4-5197-4389-aedc-be37f96c1d44"
}  ,
"c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"  :    {
"to"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"
}  ,
"b0ddd3b4-5197-4389-aedc-be37f96c1d44"  :    {
"to"  :    "e406fd72-85f9-4a28-bd09-0753cd8528fd"
}
}  ,
"use_native_actions"  :    true  ,
"video_record_session"  :    true  ,
"encrypted_keys"  :    [  ]  ,
"popup_xpaths"  :    [  ]  ,
"auth_urls"  :    [  ]  ,
"error_codes"  :    [  ]
}
```


```text
{
"id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"workspace_id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"version_id"  :    "bc2d338f-3d19-4824-bbd9-fdf8b68416c5"  ,
"version_number"  :    1  ,
"created_at"  :    "2025-05-21T00:00:00Z"  ,
"created_by"  :    "user"  ,
"name"  :    "X: Comment"  ,
"description"  :    "Comment on a X tweet"  ,
"input_schema"  :    {
"$TWEET_URL"  :    {
"type"  :    "string"  ,
"pattern"  :    "^https://x.com/[^/]+/status/[^/]+$"
}  ,
"$COMMENT"  :    {
"type"  :    "string"
}  ,
"$USER_NAME"  :    {
"type"  :    "string"
}  ,
"$PASSWORD"  :    {
"type"  :    "string"
}  ,
}  ,
"output_schema"  :    {  }  ,
"nodes"  :    [
{
"id"  :    "41a01974-5d96-403d-bd4c-21f786bacf2b"  ,
"name"  :    "Navigate to X Tweet"  ,
"action"  :    "START"  ,
"parameters"  :    {
"url"  :    "{$TWEET_URL}"
}
}  ,
{
"id"  :    "9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"  ,
"name"  :    "Is logged in?"  ,
"action"  :    "BOOL_CONDITION"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"comparison_value_1"  :    "<<xpath://input[@autocomplete='username']>>"  ,
"comparison_value_2"  :    null  ,
"comparison_operator"  :    "IS_NULL"  ,
"wait_time"  :    5000  ,
"clear_cookies_on_false"  :    true
}
}  ,
{
"id"  :    "c73ffce4-4f02-42ca-bff8-afdc816e3ca6"  ,
"name"  :    "Type username"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//input[@autocomplete='username']"  ,
"text"  :    "{$USER_NAME}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "74052c6d-51be-4f1b-9fd5-304b5632eee3"  ,
"name"  :    "Click Next"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Next']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "75f38414-62c8-453a-87d8-684847db0783"  ,
"name"  :    "Type password"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//input[@type='password']"  ,
"text"  :    "{$PASSWORD}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"  ,
"name"  :    "Click Log In"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Log in']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"  ,
"name"  :    "Enter Comment"  ,
"action"  :    "INPUT_TEXT"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"text"  :    "{$COMMENT}"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "b0ddd3b4-5197-4389-aedc-be37f96c1d44"  ,
"name"  :    "Hit Reply"  ,
"action"  :    "CLICK"  ,
"parameters"  :    {
"execution"  :    "STATIC"  ,
"selector"  :    "//button[normalize-space()='Reply']"  ,
"wait_time"  :    25000
}
}  ,
{
"id"  :    "e406fd72-85f9-4a28-bd09-0753cd8528fd"  ,
"name"  :    "End"  ,
"action"  :    "END"  ,
"parameters"  :    {  }
}
]  ,
"edges"  :    {
"41a01974-5d96-403d-bd4c-21f786bacf2b"  :    {
"to"  :    "9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"
}  ,
"9df3a95e-0dd6-4115-98a3-3abf8b9b7f29"  :    {
"true"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"  ,
"false"  :    "c73ffce4-4f02-42ca-bff8-afdc816e3ca6"
}  ,
"c73ffce4-4f02-42ca-bff8-afdc816e3ca6"  :    {
"to"  :    "74052c6d-51be-4f1b-9fd5-304b5632eee3"
}  ,
"74052c6d-51be-4f1b-9fd5-304b5632eee3"  :    {
"to"  :    "75f38414-62c8-453a-87d8-684847db0783"
}  ,
"75f38414-62c8-453a-87d8-684847db0783"  :    {
"to"  :    "c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"
}  ,
"25382829-81ae-43bc-b7c6-1d6be6a09cfb"  :    {
"to"  :    "b0ddd3b4-5197-4389-aedc-be37f96c1d44"
}  ,
"c3c0e1d5-1d88-4319-a811-35ad9bf32d3f"  :    {
"to"  :    "25382829-81ae-43bc-b7c6-1d6be6a09cfb"
}  ,
"b0ddd3b4-5197-4389-aedc-be37f96c1d44"  :    {
"to"  :    "e406fd72-85f9-4a28-bd09-0753cd8528fd"
}
}  ,
"use_native_actions"  :    true  ,
"video_record_session"  :    true  ,
"encrypted_keys"  :    [  ]  ,
"popup_xpaths"  :    [  ]  ,
"auth_urls"  :    [  ]  ,
"error_codes"  :    [  ]
}
```


One added benefit of the graph structure is that it's super easy to visualize.


####


#### Technical Deep Dive


BADGER offers a lot of fields to customize the behavior of your browser automations. When applicable, we're trying to set good defaults. Here's a deep dive on the most important ones.


#### Workflow Fields


-


use_native_actions: Enables OS-level actions such as clicking and typing via a desktop application integration. This is particularly useful when standard JavaScript methods encounter issues with complex or secured web elements.


-


input_schema: Defines the structure of input data provided by users or systems to the automation workflow. Commonly used for form-filling or dynamic content input, ensuring data provided meets website form validations.


-


output_schema: Specifies the structured data format returned by the browser agent after workflow execution. Essential for standardizing outputs for downstream processes.


#### Node Types


Each node represents a distinct browser action or logical step in the workflow:


###### **Action Nodes**


-


**Navigate** : Opens a specified URL.


-


url: Destination URL.


-


**Click** : Click on an element.


-


selector: DOM selector of the element to click. Used if execution is STATIC


-


prompt: The prompt for the LLM describing the element to interact with. Used if execution is LLM_DOM or LLM_VISION


-


execution: Determines interaction method (STATIC, LLM_DOM, LLM_VISION).


-


human_mode: When true, performs click actions using human-like mouse movements and speed patterns, mimicking natural user interactions.


-


**InputText** : Inputs text into fields.


-


text: Text to input.


-


do_not_clear: Prevents clearing existing text if true.


-


submit_after_input: Automatically submits form after input if true.


-


max_retries_with_reload: In case the element cannot be found, the amount of retries with in-between reloads of the page.


-


**InputSelect** : Chooses an option from a select field.


-


value: Desired selection value.


-


fuzzy_match: Enables approximate matching of selection options using LLMs.


-


**ExtractDatamodel** : Extracts structured data.


-


extract_data_model: JSON schema defining data structure.


-


**Screenshot** : Takes a screenshot of the page. Can be configured to be full-size by setting the max_scrolls parameter.


-


selector: Element selector to capture.


-


max_scrolls: Number of scroll actions.


###### Condition and Loop Nodes


-


**BoolCondition** : Handles logical branching.


-


comparison_operator: Type of comparison (EQUAL, NOT_EQUAL, CONTAINS, etc.).


-


comparison_value_1 and comparison_value_2: Values for logical comparison.


-


execution: Execution method (STATIC, LLM_DOM, LLM_VISION).


-


**Loop** : Executes repeated operations.


-


over: Number or array to iterate over.


-


variable_current_item: Current iteration's item.


-


variable_current_index: Current iteration's index.


###### Specialized Nodes


-


**Tfa (Two-Factor Authentication)** : Handles entering of 2FA codes.


-


selector: DOM selector for input.


-


code: 2FA code value or retrieval method.


-


tfa_type: 2FA method (SMS, EMAIL, AUTHENTICATOR).


-


tfa_url: URL associated with 2FA validation.


-


**FileUpload** : Manages uploading files.


-


signed_file_url: Secure URL that points to the file.


-


**FileDownload** : Facilitates downloading files.


-


continue_on_failed_download: Continues workflow even if download fails.


-


metadata: Any metadata to be sent together with the file URL of the downloaded file.


-


**TabManagement** : Controls browser tabs.


-


tabAction: Action (OPEN, CLOSE, SWITCH).


-


url: URL for opening a tab.


-


tab_index: Tab index for switching tabs.


-


**UserInteraction** : Awaits manual user input or confirmation.


-


expected_datamodel: Data structure expected from user.


-


timeout: Maximum wait duration.


#### Edges


Edges define control flow:


-


true, false: Branching logic


-


loop_done, loop_not_done: Lop control


-


to: Sequential transitions


#### Execution Types


BADGER nodes can leverage different execution strategies:


-


**STATIC** : Uses explicit selectors, fully deterministic.


-


**LLM_DOM** : Uses the DOM to fulfill the action e.g. finding the next element to click on.


-


**LLM_VISION** : Uses the screenshot to fulfill the action.


#### Trade-Offs And Challenges


Building and maintaining a DSL has trade-offs:


-


**Initial Complexity** : A DSL requires upfront investment and design effort.


-


**Performance Overhead** : Abstraction may add minor performance cost, but this is negligible compared to the reliability benefits.


#### Why This Matters for Reliable Browser Agents


LLM-driven runtime prompting looks great in demos but fails in production. BADGER solves this by drawing a firm line between where LLMs help and where they don’t.


-


Deterministic at runtime: Workflows run as explicit, structured graphs. Predictable, debuggable, and fast.


-


LLMs for targeted repair: When things break, only specific DSL nodes are regenerated – keeping fixes local and understandable.


This approach lets you debug, version-control, and reliably scale browser automation workflows. It combines LLM flexibility exactly where it’s needed, with predictable, maintainable execution everywhere else. Check out[this blog](https://www.cloudcruise.com/blog/genesis) article describing our overall system.
