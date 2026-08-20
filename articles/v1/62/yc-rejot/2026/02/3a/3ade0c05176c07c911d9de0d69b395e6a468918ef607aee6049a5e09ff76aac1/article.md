---
schema_version: "1.0.0"
document_id: "3ade0c05176c07c911d9de0d69b395e6a468918ef607aee6049a5e09ff76aac1"
company_key: "yc-rejot"
company: "ReJot"
source_id: "yc-rejot-news-import-01598ccac029"
canonical_url: "https://rejot.dev/blog/implicit-last-write-wins/"
published_at: "2026-02-18T00:00:00+00:00"
first_seen_at: "2026-07-31T19:47:01.544738+00:00"
fetched_at: "2026-07-31T19:47:01.910446+00:00"
content_hash: "sha256:a3fe345c59511761adedbd1d286029f43e49740508f6c20817d253bb6543433c"
---

# Implicit last-write-wins behaviour in the age of agents

Most CRUD apps do not handle (semi) concurrent writes gracefully. They show information on a page. Possibility to multiple users are the same time. When user A saves, user B does not see the update. When user B saves, the initial change is overwritten.


But who cares? In practice this doesn’t happen often.


Human users naturally divide work in such a way that they don’t overlay. It’s human nature.


## Agents are a lot more concurrent


… and they don’t care about your work. They` apply_patch` whenever. When I was still using Cursor I noticed this happening a lot. Basically, working in the same file as an agent was a no go.


As we’re starting to use agents more for human work, this is becoming a bigger problem.


Last-write-wins is no longer cutting it in highly concurrent environments. So what can we do instead?


## Optimistic concurrency control


Optimistic concurrency control (OCC) is the answer. It’s a simple pattern, really.


```text
UPDATE   users
SET
name   =   :  name  ,
_version   =   _version   +   1
WHERE
id   =   :id
AND   _version   =   :  version  ;
```


Because of the` WHERE` clause, if the version has changed, the update will become a no-op. Most database drivers allow you to detect this.


Now you get the ability to reload the data and retry the operation.


Or put the up-to-date information in the context window.


## Two-phase OCC in Fragno


We built Fragno’s query API around this pattern. The transaction builder enforces a two-phase structure: a read phase (` retrieve` ) followed by a write phase (` mutate` ). In the mutate phase, the author can use` .check()` to mark rows for version checking. If any checked row was modified by another request since it was read, the mutate phase is rejected and can be retried.


```text
function   updateUserName  (  userId  :   string  ,   newName  :   string  ) {
return   this  .  serviceTx  (schema)
.  retrieve  ((  uow  )   =>
uow.  findFirst  (  "users"  , (  b  )   =>   b.  whereIndex  (  "primary"  , (  eb  )   =>   eb  (  "id"  ,   "="  , userId))),
)
.  mutate  (({   uow  ,   retrieveResult  : [  user  ] })   =>   {
if   (  !  user)   return   null  ;


// Note the .check() call here
uow.  update  (  "users"  , user.id, { name: newName }).  check  ();


return   { id: user.id, name: newName };
})
.  build  ();
}
```


When a check operation detects a version conflict, the transaction can be retried. By default, we use an exponential backoff retry policy. But this really depends on the use case.


If the query is executed as part of a HTTP route or agent operation, it might make more sense to just return the retrieval result directly and let the user/agent decide what to do.
