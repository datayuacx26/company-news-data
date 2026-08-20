---
schema_version: "1.0.0"
document_id: "7f39ad0d01532f2449be2c2c645e2a5e6956d73afd362bc5204795c2a518bfd7"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/introducing-sails-quest/"
published_at: "2026-01-02T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:54:56.230774+00:00"
content_hash: "sha256:4f2c460e8287e608a50a40eae94f1d6b3f2e22d422b7fb20c922c6d09eec0ab1"
---

# Introducing Quest - Elegant Job Scheduling for Sails.js

I’m thrilled to announce[Sails Quest](https://docs.sailscasts.com/sails-quest) , a job scheduling hook for Sails.js that makes running scheduled tasks feel natural and intuitive.


## Why Quest?


If you’ve ever needed to run background jobs in a Sails application - sending scheduled emails, cleaning up old data, processing queues, or running periodic reports - you know the options have been limited. You either reach for external tools like cron jobs, or cobble together something with` setInterval` .


Quest changes that. It lets you schedule jobs using the same[Sails scripts](https://sailsjs.com/documentation/concepts/shell-scripts) you already know and love, with human-readable scheduling syntax and full access to your Sails application context.


## What Makes Quest Special


### Human-Readable Scheduling


Forget cryptic cron expressions (though Quest supports those too). Schedule jobs the way you think about them:


```text
quest  : {
interval  :   'every hour'
// or '30 seconds', '5 minutes', '2 days'
}
```


### Full Sails Context


Your scheduled jobs have complete access to models, helpers, and configuration - exactly like your regular Sails scripts:


```text
// scripts/cleanup-sessions.js
module  .  exports   =   {
friendlyName:   'Cleanup old sessions'  ,


quest: {
interval:   '1 hour'  ,
withoutOverlapping:   true
},


inputs: {
daysOld: {
type:   'number'  ,
defaultsTo:   30
}
},


fn  :   async   function  (  inputs  ) {
const   deleted   =   await   Session.  destroy  ({
lastActive: {   '<'  :   new   Date  (Date.  now  ()   -   inputs.daysOld   *   24  *  60  *  60  *  1000  ) }
}).  fetch  ()


await   sails.helpers.sendEmail.  with  ({
to:   ' [email protected]  '  ,
subject:   'Cleanup complete'  ,
text:   `Deleted ${  deleted  .  length  } sessions`
})


return   { deletedCount: deleted.  length   }
}
}
```


### Flexible Configuration


Define schedules in your scripts, in config, or both. Quest intelligently merges them with a clear priority system:


- Script’s` quest` config takes priority for scheduling
- Config inputs serve as defaults that scripts can override
- Runtime inputs from` sails.quest.run()` take highest priority


### Built-in Safeguards


Quest prevents jobs from stepping on each other with` withoutOverlapping` , validates that scripts exist before running, and provides helpful error messages when something goes wrong.


## Getting Started


Install Quest:


```text
npm   install   sails-hook-quest
```


Create a scheduled job in` scripts/` :


```text
// scripts/health-check.js
module  .  exports   =   {
friendlyName:   'Health check'  ,


quest: {
interval:   '5 minutes'
},


fn  :   async   function  () {
const   status   =   await   sails.helpers.  checkHealth  ()
console.  log  (  'System healthy:'  , status)
return   status
}
}
```


That’s it. When Sails lifts, Quest automatically discovers your jobs and starts scheduling them.


## Runtime Control


Quest exposes a clean API for controlling jobs programmatically:


```text
// Run a job immediately
await   sails.quest.  run  (  'health-check'  )


// Run with custom inputs
await   sails.quest.  run  (  'cleanup-sessions'  , { daysOld:   7   })


// Start/stop scheduling
sails.quest.  start  (  'weekly-report'  )
sails.quest.  stop  (  'weekly-report'  )


// Pause without removing schedule
sails.quest.  pause  (  'heavy-task'  )
sails.quest.  resume  (  'heavy-task'  )
```


## Event System


Monitor your jobs with Quest’s event system:


```text
// config/bootstrap.js
sails.  on  (  'quest:job:start'  , (  data  )   =>   {
console.  log  (  `Job ${  data  .  name  } started`  )
})


sails.  on  (  'quest:job:complete'  , (  data  )   =>   {
console.  log  (  `Job ${  data  .  name  } completed in ${  data  .  duration  }ms`  )
})


sails.  on  (  'quest:job:error'  , (  data  )   =>   {
// Send alert to Slack, Discord, etc.
console.  error  (  `Job ${  data  .  name  } failed:`  , data.error)
})
```


## Documentation


Check out the[full documentation](https://docs.sailscasts.com/sails-quest) covering:


- [Configuration options](https://docs.sailscasts.com/sails-quest/configuration)
- [Creating jobs](https://docs.sailscasts.com/sails-quest/creating-jobs)
- [Scheduling syntax](https://docs.sailscasts.com/sails-quest/scheduling)
- [Job inputs and priority](https://docs.sailscasts.com/sails-quest/job-inputs)
- [Job management](https://docs.sailscasts.com/sails-quest/job-management)
- [Events](https://docs.sailscasts.com/sails-quest/events)


## Try It Today


```text
npm   install   sails-hook-quest
```


Report issues or contribute at[on GitHub](https://github.com/sailscastshq/sails-hook-quest) .


Happy scheduling!
