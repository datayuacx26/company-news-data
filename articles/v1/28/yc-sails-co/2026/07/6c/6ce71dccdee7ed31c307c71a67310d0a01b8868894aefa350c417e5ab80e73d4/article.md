---
schema_version: "1.0.0"
document_id: "6ce71dccdee7ed31c307c71a67310d0a01b8868894aefa350c417e5ab80e73d4"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/slipway-0-0-52/"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-08-01T01:27:47.939622+00:00"
fetched_at: "2026-08-01T01:27:50.082269+00:00"
content_hash: "sha256:5a0ec8f812a5cb2111b12ac2ae5e4f428266d0820625d97070c4e02bd42ea879"
---

# Slipway 0.0.52: Making Production Boring Again

Starting a container is easy.


The real engineering begins when two deployments happen at once, the server restarts halfway through a release, the new version is unhealthy, the proxy update fails, a backup is larger than memory, or the dashboard disagrees with what Docker is actually doing.


That is the work behind Slipway 0.0.52.


Since 0.0.51, we merged 56 pull requests across the deployment engine, Bridge, Helm, Dock, Content, Lookout, configuration, ingress, validation, and the test system. It is the largest production-hardening release since Slipway was introduced.


But the number is not the interesting part.


The interesting part is that production is now more *legible* . Source, build, traffic, data, observation, and rollback are less likely to disagree. When a failure does happen, Slipway is much better at saying what failed, preserving the last healthy state, and giving you one bounded way forward.


That is my standard for a platform: not that failure becomes impossible, but that failure becomes boring.


## Raw land needs a control plane


People often describe the return to VPS hosting as a cost story. The economics are obviously attractive, but I think the deeper reason is legibility.


A VPS gives you one machine, one bill, and a failure boundary you can understand. But the VPS itself is only raw land. Production still needs two reliable paths:


```text
Traffic:
DNS → TLS → firewall → proxy → application → database


Change:
source → build → secrets → release → health check → cutover → logs → rollback
```


Modern cloud platforms bundle the computer and the developer experience. Slipway deliberately separates them. You can rent the computer from Hetzner, DigitalOcean, AWS, Vultr, or someone else. Slipway is the thin, Sails-native control plane that makes that computer feel like a platform.


0.0.52 is where that promise becomes much more serious.


## Deployments now have one truthful order


An application can have many historical deployments, but only one deployment should be building and only one should be serving traffic. The interface now promotes the active build and running release above ordinary history without changing the restrained list design.


The visual ordering reflects a deeper change in the engine:


- deployments are serialized per app, so two workers cannot race for the same release;
- work interrupted by a process restart is reconciled instead of remaining permanently “building”;
- a traffic cutover is transactional, with the previous Caddy route restored when verification fails;
- cancellation terminates active build work, cleans candidate artifacts, and preserves bounded partial output;
- Docker auto-restarts are reconciled back into Slipway state;
- CLI-pushed source can be redeployed without pretending it came from Git;
- deployment preflight failures retain a useful error instead of producing an empty failed page; and
- history is ordered by real deployment time, including existing applications.


The practical implication is simple: the dashboard should stop telling a different story from the server.


## Bridge is becoming a Sails-native application backend


Bridge started as Waterline introspection: read the models in a running Sails app and give the operator a useful interface.


In 0.0.52, it becomes a configurable application backend. The inspiration is closer to Laravel Nova than a generic database viewer, but the contract is Sails-native. Your app still owns its models, helpers, authorization, and storage policy. Slipway owns the reusable operational interface.


` config/slipway.js` can now describe:


- resource labels, search, sorting, list, detail, and form fields;
- numeric, string, and UUID identifiers without coercing them into one shape;
- typed text, boolean, enum, money, JSON, date, URL, relationship, Markdown, image, file, and protected fields;
- view, create, update, delete, bulk, field, and custom-action authorization;
- searchable belongs-to relationships and explicit collection attach/detach;
- resource, record, and bulk actions backed by ordinary app helpers;
- dashboards with metrics, trends, partitions, recent records, and quick actions; and
- type-aware filters and reusable saved lenses.


Required fields are validated before create is enabled. Hidden fields cannot be forged back into a request. Sensitive attributes stay hidden unless the app explicitly exposes them. A broken dashboard card fails independently instead of taking down the entire page.


### App-local access without infrastructure access


Bridge can also be opened through the application itself. A person can manage the data they are authorized to manage without becoming a member of the Slipway infrastructure team.


The invitation is matched against a verified account in the host app. Slipway uses short-lived, single-use launch codes and a dedicated Bridge session. A viewer, editor, or administrator role is only a ceiling: authorization inside the target Sails app can always narrow it further.


The default Boring Stack identity conventions work without app code. Apps with a different authentication model can expose one ordinary Sails helper to resolve the signed-in identity.


### Upload the asset; store the URL


Bridge-managed uploads now support R2 and S3-compatible storage. The upload streams through scoped credentials, returns the canonical public URL, and stores only that URL in the Waterline record.


This is useful for the pure case: a` thumbnailUrl` attribute can render as an upload field, accept an image, and persist the resulting URL without changing the model to understand multipart storage.


Reusable path templates can reference authorized fields and relationships. For Sailscasts, that means course, chapter, lesson, thumbnail, and video hierarchies can come from app configuration. Nothing in Slipway is hardcoded for Sailscasts. Existing conventional` R2_` or` S3_` credentials can be reused, with only the public URL added where it cannot be derived safely.


## Helm is now a production workspace


Helm still lets you run Sails code inside the selected application. What changed is the boundary around that power.


The editor, execution engine, and result viewer now agree on what was run:


- project Helm and Bosun share one parser-backed JavaScript runtime;
- you can run the current selection or the whole document;
- declarations, expressions, statements, async work, and console output behave consistently;
- compatible results can render as a table, JSON tree, or raw value;
- console output remains separate, bounded, and collapsible;
- syntax and runtime errors are attached to the exact CodeMirror token;
- an active execution can be stopped without accepting a late result;
- Sails models, attributes, helpers, configuration paths, and Waterline methods complete without sending private values or records to the browser;
- private execution history and reusable snippets are durable and clearable; and
- named scratchpads make repeated operational work less disposable.


The input editor no longer disappears when a result is large. Console output stays above the selected result view, and history keeps source without copying returned production data into another long-lived store.


Likely writes still require deliberate arming. The point is not to make a production REPL harmless—that would be dishonest. The point is to make its authority visible, bounded, auditable, and difficult to trigger accidentally.


## Content stays Markdown without making writers learn Markdown


The old Content workflow behaved like an editor glued to a preview. The new one is a minimal Tiptap writing surface that feels closer to a focused CMS.


Writers can use Markdown shortcuts, select text for contextual formatting, and upload images inline. Markdown remains the canonical stored value, so the content does not become trapped in editor-specific JSON.


Create, update, and delete operations commit through the connected GitHub repository using Conventional Commits and optimistic blob SHAs. “Save & Deploy” builds the exact content commit, and duplicate webhook deployments are suppressed.


This is a small Git-backed CMS, but it keeps the Git part truthful.


## Dock no longer throws results away


A SQL console has to represent what the database actually returned. A script may contain several statements, some returning rows and others returning only command metadata. It may also fail after earlier statements succeeded.


Dock now preserves every PostgreSQL and MySQL statement result, lets you select the result you want to inspect or export, and identifies partial failure without discarding the successful work that came before it. Command-only results use compact summaries instead of occupying the result area with noisy repetition.


## Secrets and release flags close the control loop


Environment variables are convenient until every value is inherited broadly, displayed too casually, or changed without leaving a useful operational record.


Slipway now has app-scoped, write-only deployment secrets. Every deployment records a value-free manifest and a keyed configuration fingerprint. That makes a configuration-only release distinguishable without putting secret values into the UI, logs, or audit history.


Release flags then let you deploy code before releasing its behavior.


The initial contract stays deliberately small: one audited master switch, deterministic percentage rollout, and optional typed allowlists for users, accounts, tenants, or teams.


In a Sails app:


```text
const   useNewCheckout   =   await   sails.helpers.flags.enabled.  with  ({
key:   'new-checkout'  ,
req:   this  .req,
defaultValue:   false
})
```


` flags.enabled` in`[\[email protected\]](https://blog.sailscasts.com/cdn-cgi/l/email-protection)` is a genuine Sails helper machine. The hook follows the same helpers lifecycle used by` sails-hook-mail` : wait for the built-in helpers hook, then furnish the helper definition through Sails. Sails validates the inputs,` .with()` is the real machine contract, and an app-owned helper at the same identity is preserved.


The hook caches flag configuration and refreshes it in the background. A control-plane outage uses the last valid snapshot or the explicit default; it does not put another network dependency in the critical request path.


Lookout completes the loop by comparing flag-on and flag-off request count, latency, 5xx rate, and trace-linked exceptions.


Now a release can move from the team, to a small cohort, to everyone while the same platform shows whether the new path is actually healthier.


## The public port boundary is finally explicit


Fresh installations now expose Caddy on ports 80 and 443 while binding the Slipway dashboard and allocated application ports to` 127.0.0.1` by default. Domains continue through Caddy; raw` SERVER_IP:13xx` access is private unless the operator explicitly opts back into it.


We rehearsed the migration and rollback on the existing Hetzner server that runs production applications. We captured the public baseline, moved a disposable application port to loopback, verified it locally and externally, rolled it back, moved the live Slipway dashboard, verified every application domain, and restored the final private dashboard binding. Docker reported the dashboard healthy throughout the final state.


Existing application containers deliberately keep their previous bindings until redeployed. Slipway does not silently make a security migration break an operator who still depends on raw IP access.


That balance matters: a safer default, a documented migration, and an explicit rollback.


## Validation should happen before side effects


Setup, authentication, projects, environments, apps, services, settings, integrations, Bridge, and Content now share server-owned precognitive validation.


The browser can ask whether a form is valid without consuming a rate limit, sending an email, writing a database row, touching Docker, changing Caddy, triggering a webhook, or losing local edits. The server still owns the rules; the browser simply gets the answer earlier.


Slipway also replaces the default framework error views with restrained, responsive 404 and 500 pages in system light and dark mode. Production errors stay generic, while Inertia failures fall back safely.


## We tested the system as a Sails system


All unit, functional, Inertia, and browser trials now run through Sounding 0.2. Controller imports, private testing internals, hand-built CSRF sessions, and mixed runners are gone.


The screenshots in this post are not design mockups. They are browser states captured by those Sounding trials against seeded application data.


The final release gate also packages` sails-hook-slipway` , installs it into a temporary app through` node_modules` , lifts a real Sails instance, and proves that Sails itself furnishes and validates the release-flag helper. The package dry run verifies that the helper definition is actually present in the npm tarball.


This is the sort of test I want more of: prove the contract at the layer our users actually touch.


## Before you upgrade


Slipway, its CLI, and` sails-hook-slipway` now require Node.js 22 or newer. The Docker image already contains the supported runtime.


Apps using Lookout, Bridge, or release flags should install the matching hook and deploy once:


```text
npm   install    [email protected]
```


### Existing SQLite installations need one Bosun preflight


Upgrading an existing SQLite-backed Slipway installation to 0.0.52 requires a one-time Bosun migration for the two Bridge columns introduced on the` App` model. Until those columns exist, the candidate release fails its health check with` SQLITE_ERROR: no such column: bridge_enabled` . Slipway leaves the previous container active, so the migration can be run from the currently running interface before the update is retried.


Open **Bosun → Console** , keep **SQL** selected, choose the **app** database, and inspect Slipway’s control-plane` apps` table:


```text
PRAGMA table_info(apps);
```


Do not run this against` observability` ,` cache` , or one of your deployed application databases. Take a VPS snapshot or your normal database backup, arm Bosun writes, and add each column that the inspection reported missing:


```text
ALTER   TABLE   apps
ADD   COLUMN bridge_enabled   BOOLEAN   NOT NULL   DEFAULT   0  ;


ALTER   TABLE   apps
ADD   COLUMN bridge_secret   TEXT  ;
```


Skip the corresponding statement if either column already exists. Verify both with` PRAGMA table_info(apps);` , then retry the update. Bridge creates its remaining access tables after the new release lifts. Later messages about a closed datastore or Lookout are consequences of the failed lift, not separate migrations.


Existing Slipway installations should read the[Ingress and Firewall guide](https://docs.sailscasts.com/slipway/ingress-and-firewall) before changing public bindings. Bridge users should read the complete[Bridge guide](https://docs.sailscasts.com/slipway/bridge) , and release-flag users should read[Release Flags](https://docs.sailscasts.com/slipway/release-flags) .


The complete migration steps, rollback command, and exhaustive changelog are in the[Slipway 0.0.52 GitHub release](https://github.com/sailscastshq/slipway/releases/tag/v0.0.52) .


## The next cloud may not own the servers


We spent the last decade bundling infrastructure and developer experience into enormous clouds.


The return to the VPS is beginning to unbundle them again. The machine can come from anyone. The developer experience can be a thinner, more legible layer you control.


That is the opportunity I see with Slipway: not another company that owns your computers, but a Sails-native control plane that makes the computers you own feel like a cloud.


The next cloud may not own the servers. It may simply make any server feel like a cloud.


Slipway 0.0.52 makes that idea much less theoretical.
