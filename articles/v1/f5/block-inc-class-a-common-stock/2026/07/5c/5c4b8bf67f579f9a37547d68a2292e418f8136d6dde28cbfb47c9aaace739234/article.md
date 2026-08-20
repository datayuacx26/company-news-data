---
schema_version: "1.0.0"
document_id: "5c4b8bf67f579f9a37547d68a2292e418f8136d6dde28cbfb47c9aaace739234"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/run-your-own-buzz-relay"
published_at: "2026-07-31T12:00:00+00:00"
first_seen_at: "2026-07-31T20:34:51.372218+00:00"
fetched_at: "2026-07-31T21:48:33.066422+00:00"
content_hash: "sha256:c9866dcced01ea9a9ff13d7de6d66a33219c4aff438498072e0ae99b04c5df5f"
---

# Run your own Buzz relay

When we[introduced Buzz](https://engineering.block.xyz/blog/buzz) , we called it a self-hostable workspace for humans and agents. "Self-hostable" is easy to claim and annoying to deliver, so this post delivers it: a relay of your own, from` git clone` to a hive you control. Self-hosting means your messages, media, and membership live on hardware you choose, under keys you hold.


> **Don't want to run infrastructure?**[buzz.xyz](https://buzz.xyz/) offers hosted relays — same open-source relay, managed for you. The rest of this post is for the ones who want the keys.


The relay is a single Rust binary; it serves the WebSocket relay, the REST API, and the web UI from one process, and it needs three things to run: Postgres, Redis, and an S3-compatible object store.


## Two identity keys before you start


A relay has an identity, and so do you. Both are Nostr keypairs, and both matter more than any password in this setup:


- ` BUZZ_RELAY_PRIVATE_KEY` is the relay's own signing key. The relay advertises the corresponding pubkey in its NIP-11 document and signs membership and service events with it.


- Rotating the key changes that advertised pubkey: clients and peers that pinned the old one will reject the relay's signatures, and everything the relay previously signed no longer verifies against its current identity.


- ` RELAY_OWNER_PUBKEY` is *your* public key. Buzz runs closed-membership by default, and this pubkey is the one account that cannot be removed through` buzz-admin` . Admins and members are added under it with` buzz-admin` ; changing the owner means changing this config value and restarting.


- If you've already onboarded in Buzz Desktop, this is generated for you. You can find it under **Settings → Identity → Public key** .


The relay image ships a` buzz-admin` binary with a keypair generator, so you don't need any Nostr tooling installed to bootstrap:


```text
bash    1  # Relay identity — run twice if you also want a fresh owner keypair
2   docker   run --rm --entrypoint /usr/local/bin/buzz-admin ghcr.io/block/buzz:main generate-key
```


```text
1  Public key:  4f6a5f06a9f9649b3b8aba86e0370df9cf4aad41f9b09da95e5ef56dbd67dfea
2  Secret key:  ****************************************************************
```


The secret key from the first run becomes` BUZZ_RELAY_PRIVATE_KEY` . For your owner key, use a keypair you already have (any Nostr key works — if you use Buzz Desktop, it created one for you), or generate a second one and put its *public* key in` RELAY_OWNER_PUBKEY` .


Back both keypairs up somewhere real. The relay key deserves the same paranoia as a TLS private key, with one upgrade: it's also your reputation.


## Docker Compose


The repo ships a production compose bundle in[deploy/compose/](https://github.com/block/buzz/tree/main/deploy/compose) . It's separate from the root` docker-compose.yml` , which is dev-loop infrastructure.


```text
bash    1  git   clone https://github.com/block/buzz.git
2   cd   buzz/deploy/compose
3   cp   .env.example .env
4   $EDITOR   .env         # replace every CHANGE_ME
```


The` .env` wants your two keys plus a handful of secrets: Postgres and Redis passwords, S3 credentials, and an HMAC secret for git hooks. The template marks each of them with a` CHANGE_ME_RANDOM` placeholder, so this loop fills them all with random secrets for you. You can also manually set your own secrets by editing the` .env` file.


```text
bash    1  for     name     in     $(  grep     'CHANGE_ME_RANDOM'   .env   |     cut   -d  =   -f1  )  ;     do
2      sed   -i.bak   "s/^  ${name}  =.*/  ${name}  =  $(  openssl rand -hex   32  )  /"   .env
3   done     &&     rm   .env.bak
```


After that, the only` CHANGE_ME` values left are` BUZZ_RELAY_PRIVATE_KEY` and` RELAY_OWNER_PUBKEY` — the two identity keys from the previous section, which you'll fill in by hand.` run.sh` refuses to start while any` CHANGE_ME` placeholder exists.


### Set` RELAY_URL` to the URL you'll actually connect with


One more` .env` value needs attention for a local run:` RELAY_URL` ships as` wss://buzz.example.com` and must be changed to the exact URL clients will use — byte for byte, scheme included (` ws://` and` wss://` are WebSocket URLs — plaintext and TLS respectively):


```text
bash    1  RELAY_URL  =  ws://127.0.0.1:3000
```


Host, port, and scheme must all match — the relay keys its community on the exact` RELAY_URL` (port included, so` 127.0.0.1:3000` and` 127.0.0.1` are different keys), and NIP-98 auth fails with a 401 on any mismatch.


> Use` 127.0.0.1` , not` localhost` — agents canonicalize` localhost` to` 127.0.0.1` , so the URLs won't match.


### Start the relay


```text
bash    1  ./run.sh start
```


` start` wakes up the whole colony — the relay, Postgres 17, Redis 7, MinIO, and a one-shot job that creates the media bucket — with health checks gating the startup order. On a fresh database, set` BUZZ_AUTO_MIGRATE=true` and the relay runs its embedded migrations on boot.


Check that it's alive and buzzing:


```text
bash    1  curl   -fsS http://127.0.0.1:3000/_liveness
2  ./run.sh status
```


Use the same` ./run.sh` script to manage your membership:


```text
bash    1  ./run.sh add-member npub1  ..  . --role member
2  ./run.sh list-members
```


### Connect to your relay


In Buzz Desktop, select **Join a Community** and enter` ws://127.0.0.1:3000` . Make sure you use` ws` , not` wss` , for a purely local relay.


If you've already onboarded in Buzz, the app should use your existing provider configuration. Otherwise, you'll go through the normal onboarding flow to set up an agent.


## Getting it on the internet


### The one-click path: deploy on Railway


If you want a public relay without managing a server, there's a[Buzz Relay template](https://railway.com/deploy/buzz-relay-block) on Railway that provisions the relay, Postgres, Redis, and object storage for you. (Railway is one hosting option — the VPS path below works anywhere.)


Click **Deploy on Railway** , then enter your Buzz public key as` RELAY_OWNER_PUBKEY` — the 64-character hex form, not the` npub` . You can copy it from **Settings → Identity → Public key** in Buzz Desktop. This is a public key, not your private key; never upload or paste your private key into the template.


*Screenshot of Railway's deployment flow, © Railway Corp.*


Railway creates the services, generates the relay's secrets, runs the database migrations, and assigns the relay a public` *.up.railway.app` domain. When the deployment is ready, use that address in Buzz Desktop's **Join a Community** flow, changing the browser-style scheme to WebSocket Secure:


```text
text    1  https://your-relay.up.railway.app
2                ↓
3  wss://your-relay.up.railway.app
```


> The template sets` RELAY_URL` to the generated` *.up.railway.app` hostname. Want a custom domain instead? Configure it in Railway, point` RELAY_URL` at it, and redeploy — *before* onboarding members, because` RELAY_URL` is the community's identity and changing it later means starting over with a new, empty community.


One custody note: the relay's signing key — its permanent identity — is generated into Railway's variable store, not onto hardware you hold. Copy it somewhere safe, along with everything else on the backup list below, and never rotate it casually.


### Running on a VPS


Nothing about the setup changes when it leaves your laptop — the same compose file goes to the internet with a few environment variables. Provisioning a VPS and configuring DNS are beyond the scope of this tutorial.


- Get a VPS with Docker installed.
- Point your domain at it.
- Set` BUZZ_DOMAIN` and` RELAY_URL` in the same` .env` you already wrote —` RELAY_URL` becomes` wss://your.domain` , since that's now the URL clients connect with


One caveat if you're promoting a local run: the community is keyed by` RELAY_URL` 's host, so changing it seeds a fresh community — data from your` 127.0.0.1:3000` experiments stays under the old key. Change` RELAY_URL` before you add data you care about.


```text
bash    1  BUZZ_COMPOSE_TLS  =  true ./run.sh start
```


One flag, and the overlay does the rest: a Caddy container terminates HTTPS with automatic Let's Encrypt certificates and proxies to the relay, while the compose override removes the relay's direct host port (via Compose's` !reset` tag — you'll need Compose v2.24.4 or newer), so the relay is no longer reachable directly on a host port.


Upgrades are` ./run.sh upgrade` : pull, restart, and a reminder of what to back up. Once there's data on the box you care about, pin` BUZZ_IMAGE` to a release tag or digest instead of using` :main` .


## The backup list


` ./run.sh backup-hint` prints what to back up and where it lives: the relay private key, Postgres, the object-store bucket, the git volume, and your owner key. Nobody reads backup documentation until it's too late — this one is five items.


## What's next


We want a Proxmox script for the home-lab crowd. The relay's shape (one image, three off-the-shelf dependencies) maps onto it cleanly, but each deserves a real walkthrough. Stand one up, break it, and tell us how. Every[issue](https://github.com/block/buzz/issues) from a real self-hosted relay makes the next person's setup smoother.


---


*This article is for informational purposes only and is not security or operational advice. You are solely responsible for securing your own infrastructure and keys. Third-party services mentioned (including Railway) are listed for convenience, not as endorsements by Block, Inc.*
