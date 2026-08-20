---
schema_version: "1.0.0"
document_id: "ec2d07257f0389e035ec49cc2e9edf5b2ce3cb5a80f99a171351642c0961536c"
company_key: "yc-wasp"
company: "Wasp"
source_id: "yc-wasp-rss-5b1984e54864"
canonical_url: "https://wasp.sh/blog/2026/08/03/wasp-resend-email-provider"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T12:19:20.821251+00:00"
fetched_at: "2026-08-03T13:03:34.932163+00:00"
content_hash: "sha256:f0fd86c133ed7a5c34b0de221eff222ddf097d58b8637e52e04cc0fa13acd690"
---

# Wasp now supports Resend for sending emails

Wasp 0.25 adds[Resend](https://resend.com/) as a first-class email sender provider, right next to Mailgun, SendGrid, and SMTP. Set` provider: "Resend"` , add your API key, and you're sending emails. Big thanks to community contributor[Jamie Davenport](https://github.com/jamiedavenport) , who built the whole thing!


## Why Resend​


Resend has quickly become one of the most loved email APIs among developers, and it's easy to see why: a clean API, a generous free tier, and great DX around domain setup and debugging. It's also the team behind[React Email](https://react.email/) , so it fits naturally into the React + Node.js world Wasp lives in.


It's been one of our most requested integrations for a while. You could already use Resend with Wasp over SMTP, but now it's a first-class provider: no SMTP credentials, no extra packages, just the official Resend SDK wired in for you.


## How to use it​


Set the provider to` Resend` in your` main.wasp.ts` file:


main.wasp.ts


```text
import     {   app   }     from     "@wasp.sh/spec"         export     default     app  (  {       name  :     "myApp"  ,       emailSender  :     {         provider  :     "Resend"  ,         defaultFrom  :     {           name  :     "Example"  ,           email  :     " [email protected]  "  ,           }  ,         }  ,         // ...      }  )
```


Then grab an API key from the[Resend dashboard](https://resend.com/api-keys) and add it to your` .env.server` file:


.env.server


```text
RESEND_API_KEY=
```


That's the whole setup. Now you can send emails from anywhere on the server:


src/actions/sendEmail.ts


```text
import     {   emailSender   }     from     "wasp/server/email"  ;         // In some action handler...      const   info   =     await   emailSender  .  send  (  {       to  :     " [email protected]  "  ,       subject  :     "Saying hello"  ,       text  :     "Hello world"  ,       html  :     "Hello <strong>world</strong>"  ,      }  )  ;
```


And since` emailSender` is what Wasp uses under the hood for everything email-related, this also covers your[email authentication](https://wasp.sh/docs/auth/email) flows - verification emails and password resets now go through Resend too, with zero extra code.


As with all Wasp providers, switching is a one-line change. Started on SMTP and want to move to Resend (or the other way around)? Change the` provider` field, swap the env variables, and everything else keeps working.


## Built by the community​


This feature was contributed by[Jamie Davenport](https://github.com/jamiedavenport) , who took it from idea to merged PR ([#4381](https://github.com/wasp-lang/wasp/pull/4381) ): the provider implementation, env variable validation, and docs. This is exactly the kind of contribution that makes open source fun - thanks, Jamie! 🙏


If you'd like to see your name here next time, check out our[contributing guide](https://github.com/wasp-lang/wasp/blob/main/CONTRIBUTING.md) or come say hi on[Discord](https://discord.gg/rzdnErX) - we're always happy to help you find a good first issue.


## Try it out​


Resend support is live in Wasp 0.25. If you're not on it yet, update your Wasp CLI:


```text
npm   i   -g   @wasp.sh/wasp-cli
```


Then set` provider: "Resend"` , add your API key, and send your first email. Check out the[email sending docs](https://wasp.sh/docs/advanced/email#resend) for the full reference.


We'd love to hear how it works for you - come chat with us on[Discord](https://discord.gg/rzdnErX) !
