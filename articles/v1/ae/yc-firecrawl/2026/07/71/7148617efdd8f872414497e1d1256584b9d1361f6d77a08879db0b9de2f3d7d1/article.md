---
schema_version: "1.0.0"
document_id: "7148617efdd8f872414497e1d1256584b9d1361f6d77a08879db0b9de2f3d7d1"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/monitor-website-changes-firecrawl"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-24T08:10:35.258497+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:41b18ccf5a559891067cb9ccc61a95f3f110a67ec6f0505e6574db08e42b8d8b"
---

# How to Monitor a Website (or the Entire Web) for Changes with Firecrawl

Keeping up with a fast-moving page, like a changelog, a pricing page, or the news cycle around a specific AI model, usually means refreshing a tab every few minutes. Firecrawl's **monitor** endpoint replaces that: it scrapes a page (or runs a search) on a schedule you set, diffs the result against the last snapshot, and only pings you when something actually changed.


Let's go through how to use it.


## Getting an API key


Unlike **scrape** , **search** , **interact** , and **parse** , which all work **keyless** on a rate-limited free tier (see the[rate limits docs](https://docs.firecrawl.dev/rate-limits#keyless-no-api-key) ), the monitor endpoint always requires an API key. Sign up at[firecrawl.dev](https://www.firecrawl.dev/) to get one, along with free credits and a higher rate limit than the keyless tier offers.


## First usage


This walkthrough uses the **Node SDK** , but monitors can also be created from the[Python SDK](https://docs.firecrawl.dev/sdks/python) , the[CLI](https://docs.firecrawl.dev/sdks/cli) , the[MCP server](https://docs.firecrawl.dev/mcp-server) , or directly in the Firecrawl dashboard.


Install the SDK with npm:


```text
npm   install   firecrawl
```


Then set up the client and create a monitor. The example below watches the[GitHub releases page](https://github.com/Orva-Studio/camkit/releases) for CamKit, an open-source project, so it fires whenever a new version is published:


```text
import   { Firecrawl }   from   "firecrawl"  ;


const   fc   =   new   Firecrawl  ({ apiKey  :   process  .  env  .  FIRECRAWL_API_KEY   });


const   monitor   =   await   fc  .createMonitor  ({
name  :   "Camkit releases"  ,
schedule  :   { text  :   "every 5 minutes"  ,   timezone  :   "UTC"   }  ,
targets  :   [
{
type  :   "scrape"  ,
urls  :   [  "https://github.com/Orva-Studio/camkit/releases"  ]  ,
scrapeOptions  :   {}  ,
}  ,
]  ,
goal  :   "Notify me when a new camkit release is published"  ,
webhook  :   { url  :   process  .  env  .  WEBHOOK_URL   as   string   }  ,
});


console  .log  (  monitor  .id);
```


But I'll break it down so it's easy to understand.


There are three **required** inputs:


- **name** is a label for the monitor so you can identify it later.
- **schedule** accepts either cron syntax or natural-language text like` "every 5 minutes"` ,` "hourly"` , or` "daily at 9am"` , the minimum interval is 5 minutes.
- **targets** is what gets watched; a monitor accepts 1 to 50 targets and can mix types, but it's best to keep **one monitor per concern** rather than bundling unrelated URLs together.


Here the target has` type: "scrape"` and a` urls` array, meaning it watches known pages.` scrapeOptions` passes straight through to the underlying scrape, so the usual **formats** (` markdown` ,` html` ,` summary` , or a` json` format with its own schema) all apply,` scrapeOptions: {}` here just means the defaults are used.


Two fields are optional but do most of the work:


- **goal** tells Firecrawl's **AI judge** exactly what counts as a meaningful change, so it can ignore everything else.
- **webhook** is where Firecrawl posts a notification whenever a meaningful change is detected. Email notifications work the same way and can be configured instead of or alongside a webhook.


## Viewing monitors from the CLI


Running the file above returns the new monitor's ID. If the[Firecrawl CLI](https://docs.firecrawl.dev/sdks/cli) is installed, you can list monitors and pipe the output through[jq](https://jqlang.org/) (if you have it installed) for a readable view:


```text
firecrawl   monitor   list   |   jq   .
```


The output includes the normalized` cron` (` "every 5 minutes"` became` */5 * * * *` ),` status` , and` nextRunAt` , everything needed to confirm the monitor is set up the way you expect without opening the dashboard. If you've signed up for Firecrawl, the same monitor also shows up in the[Firecrawl dashboard](https://www.firecrawl.dev/app/monitoring) , with its schedule, estimated monthly credit cost, and recent checks.


## Testing the webhook


To see the monitor in action, point its` webhook` at[a small local server](https://gist.github.com/RichardBray/6954981879f9a6618fdf9a61f3246fe1) (the demo here uses a[Bun](https://bun.sh/) server running on port 3000, tunneled out with a[Tailscale funnel](https://tailscale.com/kb/1223/funnel) so it doesn't need a public deploy) and send it a test request:


```text
curl   -s   -X   POST   https://richards-macbook-pro.tail45f396.ts.net/   \
-H   'content-type: application/json'   \
-d   '{"test":"hello from funnel"}'
```


The first real check on the target repo comes back showing **no meaningful differences** , since nothing has changed yet, and the Firecrawl dashboard reflects the same result.


## Seeing a meaningful change get flagged


When a new version is published to the watched repo, the monitor has something real to catch. The next check marks the page as **changed** , flags it as **meaningful** , and includes the actual diff, the old version string next to the new one:


This is the AI judge doing its job: it only marked *this* check as meaningful because the change matched the` goal` ("a new release is published"). A run that doesn't match the goal still happens and still costs a scrape credit, but it won't trigger a notification or add a judge credit.


## Using it for something real


A monitor's output isn't just a notification, it's structured data your own code can act on. In the demo, a fake course site for a CLI tool called[CamKit](https://github.com/Orva-Studio/camkit) displays the tool's latest version number, and manually updating that number every release is exactly the kind of busywork a monitor removes:


The webhook server keeps that version in sync with plain string matching, no extra Firecrawl calls needed. For each` monitor.page` payload, it skips pages where` isMeaningful` is false, then scans the added lines in` diff.text` for a GitHub release-tag pattern (` releases/tag/v1.2.3` ), collects every version it finds, and keeps the highest one by semver:


```text
function   extractNewVersion  (payload) {
if   (  payload  ?.type   !==   "monitor.page"  )   return   null  ;


const   versions   =   [];
// Each Firecrawl monitor check carries an isMeaningful flag and a diff to read
for   (  const   check   of   payload  .data   ??   []) {
if   (  !  check  .isMeaningful)   continue  ;


// Look at added lines in the diff for a new release tag
for   (  const   line   of   (  check  .  diff  ?.text   ??   ""  )  .split  (  "\n"  )) {
if   (  !  line  .startsWith  (  "+"  ))   continue  ;
const   match   =   line  .match  (  /releases\/tag\/(v\d  +  \.\d  +  \.\d  +  )/  );
if   (match)   versions  .push  (match[  1  ]);
}
}
if   (  versions  .  length   ===   0  )   return   null  ;


// Pick the highest version found
versions  .sort  ((a  ,   b)   =>   Bun  .  semver  .order  (  a  .slice  (  1  )  ,   b  .slice  (  1  )));
return   versions[  versions  .  length   -   1  ];
}
```


A small[updateCamkit](https://gist.github.com/RichardBray/58c912ebc9d75f2522fd1e833afd096b#file-webhook-server-camkit-ts-L27) function then writes that version straight into the course site's local data file, replacing the old version string, so the number updates itself the moment the monitor's next check runs, without a page refresh or manual edit. The full webhook server, including the file-writing step, is in[this gist](https://gist.github.com/RichardBray/58c912ebc9d75f2522fd1e833afd096b) .


## Monitoring the entire web, not just one page


Page monitoring only works when you already know the URL. For something like AI model news, there's no single page that has everything. Swapping the target's` type` from` "scrape"` to` "search"` turns the monitor into an **always-on web search** instead of a page diff.


For example, the code below sets up a monitor that keeps an eye on the whole web for Claude Fable 5 news, checking every few hours and pinging you only when something genuinely new turns up:


```text
const   monitor   =   await   fc  .createMonitor  ({
name  :   "Claude Fable 5 news"  ,
schedule  :   { text  :   "every 6 hours"  ,   timezone  :   "UTC"   }  ,
targets  :   [
{
type  :   "search"  ,
queries  :   [  "Claude Fable 5 update"  ,   "Claude Fable 5 release"  ]  ,
searchWindow  :   "24h"  ,
maxResults  :   10  ,
}  ,
]  ,
goal  :   `Notify me about news, updates,
or changes to Anthropic's Claude Fable 5 model`  ,
webhook  :   {
url  :   "https://fable-5-news.pages.dev/api/webhook"  ,
events  :   [  "monitor.page"  ]  ,
}  ,
notification  :   {
email  :   {
enabled  :   true  ,
recipients  :   [  "you@example.com"  ]  ,
includeDiffs  :   true  ,
}  ,
}  ,
});
```


A` search` target swaps the page-level` urls` for two search-specific fields:


- **queries** is a list of 1 to 12 search terms the monitor runs on every check.
- **searchWindow** is a recency filter like` "24h"` or` "7d"` that scopes results to what's actually new.


Every check runs those queries, dedupes the results by canonical URL, and the AI judge scores each **new** result against the` goal` .


Notifications behave just like the page monitor, with two things worth calling out:


- **webhook.events** scopes which event types get posted (` monitor.page` fires per matched result,` monitor.check.completed` fires once per full check).
- **email** adds a second channel alongside the webhook, since a monitor can send both at once.


Only new, on-topic results trigger a notification, and that output can feed a site's news section just like the CamKit version number did earlier.


This exact monitor populates a live[Fable 5 news page](https://fable-5-news.pages.dev/) , where each entry was added automatically as the monitor caught it.


One endpoint covers a single known page, a whole website crawled on a schedule, or the entire web, all with the same scheduling, judging, and notification model underneath.


*Check out the[Firecrawl monitoring docs](https://docs.firecrawl.dev/features/monitoring) to go deeper, or see how to apply monitoring to[competitor tracking and feature alerts](https://www.firecrawl.dev/blog/competitor-monitoring-firecrawl) .*
