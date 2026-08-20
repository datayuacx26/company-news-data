---
schema_version: "1.0.0"
document_id: "b5355c04924b9e77b936ae6860429a4a9afe137a5f9da32d1bab601ef33e6c1a"
company_key: "yc-operator-labs"
company: "Operator Labs"
source_id: "yc-operator-labs-news-import-4e5219b8eb88"
canonical_url: "https://operator.io/blog/debugging-gog-openclaw-google-cli"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-27T00:56:34.740230+00:00"
fetched_at: "2026-07-28T21:46:32.935029+00:00"
content_hash: "sha256:85dba0669e8c19486ad7f8d558741eb349d5b56cef2db7d97f1dd7fce1cbc1b5"
---

# Debugging gog, the Google CLI behind your OpenClaw agent

If your OpenClaw agent reads your Gmail or manages your calendar, it is almost certainly doing it through[gog](https://github.com/openclaw/gogcli) , the script friendly Google CLI from the OpenClaw project. It is a Go binary that wraps Google Workspace: Gmail, Calendar, Drive, Docs, Sheets, Contacts, Tasks, and more, with predictable` --json` output on stdout and human readable progress on stderr. OpenClaw drives it through a bundled skill documented in the[gog skill reference](https://github.com/openclaw/gogcli/blob/main/.agents/skills/gog/SKILL.md) .


When it works it is excellent. When it breaks, the failures cluster around a few specific places, and most of them have nothing to do with the agent and everything to do with Google's[OAuth 2.0](https://developers.google.com/identity/protocols/oauth2) model and where your tokens live.


This is a debugging guide for the parts that actually go wrong. The through line is simple: gog is a subprocess the gateway spawns. If` gog auth doctor --check` passes in your laptop shell but the agent still fails, you are debugging process environment and token storage, not the model.


## How gog is wired into OpenClaw


The important mental model: gog authenticates against Google on its own, stores your tokens in your operating system keyring (or an encrypted file keyring you choose), and exposes commands like` gog gmail search` and` gog calendar events` . OpenClaw's skill is a thin layer that calls those commands and feeds the JSON back to the model. So when something fails, the question is almost always "can the gog binary, running as the gateway's child process, read a valid token for this account?"


Run the binary's own self check before you blame the agent:


```text
gog auth doctor --check


```


For automation and agent paths, add` --no-input` so keyring prompts fail clearly instead of hanging:


```text
gog auth doctor --check --no-input


```


The[gogcli site](https://gogcli.sh/) , the[install guide](https://gogcli.sh/install.html) , and the[command reference](https://github.com/openclaw/gogcli/blob/main/docs/commands/README.md) are the authority for flags and auth flows. The[install docs in the repo](https://github.com/openclaw/gogcli/blob/main/docs/install.md) cover Docker, systemd, and headless agents in detail.


## The Google Cloud OAuth client


gog does not ship with Google access baked in, and it cannot. Google requires every app touching your mail to authenticate against a Cloud project that you own. So the real setup, before gog does anything useful, is: create a[Google Cloud project](https://console.cloud.google.com/projectcreate) , enable the specific APIs you want (Gmail API, Calendar API, Drive API, each one separately in the[API library](https://console.cloud.google.com/apis/library) ), create a Desktop OAuth client under[Credentials](https://console.cloud.google.com/apis/credentials) , download its` client_secret.json` , and register it:


```text
gog auth credentials ~/Downloads/client_secret_....json
gog auth add you@gmail.com --services gmail,calendar,drive
gog auth doctor --check


```


The most common early error is` accessNotConfigured` . It means the API you are calling is not enabled on the same Cloud project that owns your OAuth client. Enable it in the API library, wait a minute for it to propagate, and retry. Note that consumer` gmail.com` accounts can use the normal user APIs (Gmail, Calendar, Drive, Docs, Sheets, Contacts, Tasks), but Workspace only surfaces like Admin Directory, Cloud Identity Groups, and Keep need a managed domain and a service account, which is a different setup entirely.


If browser consent fails with redirect URI errors, confirm you created a **Desktop** client, not a Web client. gog's OAuth flow expects the desktop redirect pattern documented in the gogcli README, not a localhost callback you configure by hand.


## The 7 day token expiry


This is the failure that frustrates people most, because it works for a week and then silently stops. If your OAuth app is left in "External" plus "Testing" mode on the consent screen, which is the default when you set it up quickly, Google expires the refresh token for user data scopes after seven days. Your agent runs fine, then a week later every Gmail and Calendar call starts failing with an auth error, and nothing in OpenClaw changed. The API error is usually` invalid_grant` with a message that the token has been expired or revoked.


Google documents this behavior for External apps in Testing mode on the[OAuth audience page](https://console.cloud.google.com/auth/audience) . The[Nango guide to invalid_grant](https://nango.dev/blog/google-oauth-invalid-grant-token-has-been-expired-or-revoked/) walks through the same signature: tokens die on a seven day cadence when publishing status stays on Testing. gog's own README calls this out directly, and it is the single highest value thing to check if your Google agent keeps "randomly" losing access.


The fix is to publish the OAuth app rather than leaving it in testing. In Google Cloud Console, open the OAuth consent screen and move the app to "In production." For a personal app that only you use, Google's unverified app warning on the consent screen is expected and you can proceed through it; publishing is what gives you a refresh token that does not die every Monday. **Publishing is separate from full Google verification.** You can run in production unverified for personal use.


After you publish, you still need a fresh refresh token minted under the new publishing status. Run` gog auth add you@gmail.com --services gmail,calendar,drive --force-consent` so Google issues a new refresh token rather than reusing the old seven day one. If you skip re auth, the stored token may still be the Testing era token that is already on its expiry clock.


Other causes of` invalid_grant` exist (user revoked access, six months of inactivity, Gmail scope tokens invalidated after a password reset, hitting Google's per client refresh token limit). Those are rarer on a single user personal agent, but if publishing did not fix it, check whether the account revoked the app in[Google Account permissions](https://myaccount.google.com/permissions) before you chase OpenClaw config.


## No session found


This one sends people in circles, and there is a whole[GitHub issue](https://github.com/openclaw/openclaw/issues/44456) about it. You run` gog gmail search` in your own shell and it works perfectly. The agent runs the same thing and gets` No session found: gog` . The instinct is that gog is broken. It is not. The agent process cannot read your tokens.


gog stores tokens in the OS keyring by default, and a keyring is tied to a logged in desktop session. The OpenClaw gateway usually runs as a background service (launchd, systemd, a container), and that process does not have access to your interactive keyring. So your shell can decrypt the token and the daemon cannot. On a headless Mac mini, the login Keychain can be locked with no GUI session, and OAuth may appear to succeed while writing an empty token ([#11942](https://github.com/openclaw/openclaw/issues/11942) ).


The fix is to switch gog to the encrypted file keyring and give the gateway process the password through its environment:


```text
gog auth keyring file
# then set these on the gateway/service process, not just your shell:
GOG_KEYRING_BACKEND=file
GOG_KEYRING_PASSWORD=your-strong-passphrase


```


On headless Linux over SSH, do not expect the interactive passphrase prompt to work; inject` GOG_KEYRING_PASSWORD` explicitly ([#389](https://github.com/openclaw/gogcli/issues/389) ). The file backend reads that variable, not older aliases like` KEYRING_FILE_PASSPHRASE` .


The subtle part, and gog's docs are explicit about it ([#566](https://github.com/openclaw/gogcli/issues/566) ): a successful check in your shell does not prove the agent inherited the password.` .bashrc` is not read by systemd user services. Verify through the actual agent's environment, not yours:


```text
openclaw agent --agent main --message \
'Run: gog auth doctor --check --no-input && gog gmail search "newer_than:1d" --max 1 --json'


```


For a persistent systemd fix, put the variables on the gateway unit itself:


```text
# ~/.config/systemd/user/openclaw-gateway.service.d/gog-env.conf
[Service]
Environment=GOG_KEYRING_BACKEND=file
Environment=GOG_KEYRING_PASSWORD=replace-with-secret-manager-injection
Environment=HOME=/home/youruser


```


Then reload and restart:


```text
systemctl --user daemon-reload
systemctl --user restart openclaw-gateway.service
systemctl --user show openclaw-gateway.service --property=Environment


```


For a one session fix from a shell that already has the vars exported,` systemctl --user import-environment GOG_KEYRING_BACKEND GOG_KEYRING_PASSWORD` before restart works too. The passphrase in that drop in is the thing protecting your Google tokens, and a plain` Environment=` line sits readable in the unit file and in` systemctl show` output, so feed it from a secret manager or a` LoadCredential=` reference rather than typing it in by hand, and keep the file readable only by your user.


If you run OpenClaw in Docker, mount a persistent config volume plus` GOG_KEYRING_BACKEND=file` and an injected` GOG_KEYRING_PASSWORD` , as shown in the[install docs](https://github.com/openclaw/gogcli/blob/main/docs/install.md) . Otherwise every restart loses the session. A related sharp edge is that the` exec` tool has not always passed` skills.entries.*.env` down to the command it runs ([issue #31583](https://github.com/openclaw/openclaw/issues/31583) ), so if you set the keyring password as skill env and still see` No session found` , set it on the gateway process itself instead.


## Gmail watch and Pub/Sub push


If you went past polling and set up real time Gmail with gog's[watch / Pub/Sub push](https://github.com/openclaw/gogcli/blob/main/docs/watch.md) , you are in the part of the stack that breaks in the most interesting ways. Reported failures include push notifications reaching the Pub/Sub topic but the OpenClaw webhook endpoint never processing them behind Docker plus a Tailscale Funnel ([issue #77093](https://github.com/openclaw/openclaw/issues/77093) ), the watcher dropping` --exclude-labels` after an upgrade ([#56635](https://github.com/openclaw/openclaw/issues/56635) ), and the watcher hammering Gmail instead of honoring a 429` retry-after` ([#86610](https://github.com/openclaw/openclaw/issues/86610) ).


If your agent stopped reacting to new mail, check that the push is actually arriving at the gateway's public URL before you touch gog config, because the break is usually in the network path, not the watch subscription. Confirm the Pub/Sub push subscription points at a URL the gateway can reach, that TLS terminates correctly, and that the OpenClaw hook route returns 2xx to Google's delivery attempts. That route is a public endpoint that triggers your agent on whatever it receives, so have it verify the push really came from Google, through the OIDC token Pub/Sub can attach or a secret path token, rather than acting on any request that lands on it.


## Locking gog down


This part is worth doing regardless of how you run things, because gog can send mail and delete Drive files, and an agent reading untrusted email is a[prompt injection](https://owasp.org/www-project-top-10-for-large-language-model-applications/) surface. gog has guards built for exactly this:


Flag What it does


` --gmail-no-send` Blocks the agent from sending mail


` --enable-commands` Restricts the agent to an explicit allowlist of commands


` --wrap-untrusted` Wraps fetched free text so the model treats it as untrusted


```text
gog --account you@gmail.com \
--enable-commands gmail.search,gmail.get,calendar.events \
--gmail-no-send \
--wrap-untrusted \
--json \
gmail search 'newer_than:7d'


```


For a stricter deployment, gog can build a[baked safety profile binary](https://github.com/openclaw/gogcli/blob/main/docs/safety-profiles.md) whose limits cannot be reconfigured at runtime. If you give an agent Google access, set these before the first run, not after the first incident. The skill reference recommends` --no-input` in automation and` gog schema` to discover flags instead of guessing them.


## Debugging order


When Google commands fail through the agent, walk this sequence. Run` gog auth doctor --check --no-input` from the agent entrypoint, not only your shell. If keyring errors appear, fix` GOG_KEYRING_BACKEND` and` GOG_KEYRING_PASSWORD` on the gateway unit before re authing. If auth errors mention` invalid_grant` on a weekly cadence, publish the OAuth app and re auth with` --force-consent` .


If` accessNotConfigured` appears, enable the specific API on the same Cloud project as your OAuth client. If only Pub/Sub triggered flows broke, trace webhook delivery before you touch gog tokens.


That order saves time because the error strings overlap.` No session found` and an expired refresh token both look like "Google is broken," but only one of them is fixed by publishing the consent screen.


## The managed option


Every problem above is solvable, and gog is the right tool if you want a local, scriptable Google CLI where the tokens live on your own machine and you control every scope.


If you reached for gog only because you wanted your agent to read Gmail, and the Cloud project, the 7 day token expiry, and the keyring plumbing are not how you wanted to spend the afternoon, there is a managed path.[Operator.io](https://operator.io/) runs OpenClaw for you and connects Google through a managed OAuth broker, Composio or Pipedream, so connecting Gmail is a click through Google's normal sign in popup with no Cloud project to create and no refresh token to keep alive by hand. We cover that route in[connecting Gmail to your agent without a Google Cloud project](https://operator.io/blog/connect-gmail-to-your-agent-without-google-cloud) . Pick gog when you want to own the tokens, and the managed path when you want the inbox connected and forgotten about.
