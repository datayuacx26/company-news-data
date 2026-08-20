---
schema_version: "1.0.0"
document_id: "5c3bde4acc766670f0ebcdaa1b9eaf24777eddaf195779bc84995076ea6026da"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/should-you-still-use-python-dotenv"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T17:35:13.078878+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:8b7b2548444e38a71c4558ce8589388034b2775a7363242896b0ed2484e0bafb"
---

# Should You Still Use python-dotenv in 2026?

Every Python project eventually needs to use secrets like database passwords or API keys, which the app needs at runtime. But those credentials should never sit in the source code where anyone with repo access (or a leaked copy of it) could read it.


The fastest way to get something working is to hard-code the value into the file that needs it, and that's exactly the problem: the moment that file is pushed, the key is public, and it stays public in the Git history even after the line is deleted, which creates vulnerabilities.


Most projects solve this by getting the value out of the code entirely. First it goes into an environment variable, a piece of configuration the operating system hands to the running program, read in Python with` os.environ` , the standard library's dictionary-like view of whatever variables are set in the current process.


That works until manually setting half a dozen of these by hand every time gets tedious, which is usually the point where[python-dotenv](https://pypi.org/project/python-dotenv/) shows up: a small library that reads a` .env` file (a plain text file of` KEY=value` lines) and loads it into the environment automatically, so nobody has to type` export` before every session. That progression works fine for a solo project. It starts to strain the moment a second developer, a second environment, or a production deployment enters the picture.


The Python ecosystem has the same habits, and the same breaking point as[any dotenv](https://infisical.com/blog/stop-using-dotenv-in-nodejs-v20.6.0+) solution. so it's worth asking directly: should you still be reaching for` python-dotenv` in 2026?


## How Python projects handle environment variables today


Most Python codebases handle secrets via a few patterns, usually in this order as the project grows:


**` os.environ` and` os.getenv()`** are the standard library baseline. No dependencies, no configuration, just whatever variables are already set in the process environment:


```text
import os


database_url = os.environ["DATABASE_URL"]
debug = os.getenv("DEBUG", "false") == "true"


```


This works well until a developer has to manually set half a dozen variables in their shell every time they start the app locally.


**` python-dotenv`** fixes that friction. A` .env` file sits in the project root, and` load_dotenv()` reads it into` os.environ` before the rest of the app starts:


```text
from dotenv import load_dotenv
import os


load_dotenv()
database_url = os.environ["DATABASE_URL"]


```


This is why` python-dotenv` became close to a default. It’s a convenience layer that turns a text file into environment variables, and the app code still reads those variables exactly the way it would without the file.


Then there’s **` pydantic-settings`** . It’s increasingly common in typed codebases and FastAPI apps. Instead of reading loose environment variables by name, a` Settings` class declares them as typed fields and validates that everything required is actually present at startup:


```text
from pydantic_settings import BaseSettings


class Settings(BaseSettings):
database_url: str
debug: bool = False


settings = Settings()


```


` pydantic-settings` is effectively a wrapper around` python-dotenv` and works on top of the same` .env` file, it just adds validation and typing on the way in.


**` python-decouple`** . A smaller library that does the same job as` python-dotenv` with a different interface, separating configuration from code through a` config()` call that reads from a` .env` file or a` settings.ini` .


**` keyring`** is an entirely different category. It stores credentials in the operating system's native credential store, Keychain on macOS, Credential Locker on Windows, the Secret Service or GNOME Keyring on Linux. That makes it a reasonable fit for a CLI tool running on a developer's own machine. It's a poor fit for anything deployed to a server or a container, since there is usually no OS keychain for a headless environment to talk to.


## Where python-dotenv and .env files break down for a team


None of those tools change what a` .env` file actually is: a plaintext file with your most sensitive credentials, sitting in a project folder. That's fine for a single developer’s hobby project, but gets harder to defend once a team, multiple environments, and a production deployment are all in play, and the problems aren't only about security. Some are just as much about the everyday friction of running a team.


### .env files are leak-prone, in Python, Django, Flask, or anywhere else.


A` .env` file living in the project root is exactly the kind of file that ends up in a commit by accident, especially on a new teammate's first push before` .gitignore` habits are second nature, or if you’re using AI agents without checking their work.


This matters because a leaked` DATABASE_URL` or API key isn't a theoretical risk, it's a direct, usable credential. Public GitHub commits get scanned by automated bots within minutes looking for exactly this pattern, and once a database password is out, whoever has it can connect to the database directly, the same way the app does.


This is a large part of what[secret sprawl](https://infisical.com/blog/what-is-secret-sprawl) actually looks like in practice: not one dramatic breach, but the same credential copy-pasted into a dozen places until nobody can say for certain where it still lives.


### Sharing .env files means Slack, email, or a shared drive, and it doesn't scale.


Once a new developer needs the same` DATABASE_URL` , the` .env` file has to travel. Beyond the security problem of sharing secrets in plaintext over email, Slack, or other communication apps (none of which expire access or control who can see messages), there's also the operational issue. A new teammate has to track down whoever has the current file, hope it's actually up to date, and repeat that hunt every time a value changes. Nobody owns making sure everyone has the right version, so drift is normal, not an edge case.


### .env doesn’t enable automated secret rotation.


If a key in a shared` .env` file is ever compromised, fixing it means finding and updating every copy of that file across every teammate's machine, every CI job, and every server, by hand. In practice, this is why compromised credentials often stay valid for months after an incident exposes them.


Relying on manual secrets management break builds because the new values don’t automatically propagate. If you miss replacing one copy of a secret with the new value in one place, the rotation can cause an outage .


### .env files leave no audit trail.


A` .env` file can't tell you who read` DATABASE_URL` or when. If something goes wrong, whether that's a suspected leak or just a service failing after a value changed, there's no log to check, so answering "who touched this and when" turns into asking around and hoping someone remembers, instead of looking it up.


### Environment separation is a naming convention, not a boundary.


` .env.development` and` .env.production` sitting in the same repository rely on developers remembering which one to load. Nothing enforces the separation, and this is where the operational risk can get serious fast: a script meant to test against a local database that accidentally picks up` .env.production` (a wrong symlink, a typo, a copy-paste mistake) doesn't fail loudly.


It quietly runs against the real production database, and the first sign anything went wrong is often the incident itself, not a warning beforehand.


## A quick note for Django, Flask, and FastAPI


The team-scale problems with` .env` files hold regardless of framework, since they're really about where the value comes from, not how the framework reads it.


Django and Flask apps both read their configuration the same way any other Python process does, through` os.environ` , usually populated by` python-dotenv` in development.[Infisical's Django integration](https://infisical.com/docs/integrations/frameworks/django) and its[Python guide's Flask example](https://infisical.com/docs/documentation/guides/python) both work by injecting real environment variables ahead of the app starting, so neither framework needs any code change to move off a` .env` file.


FastAPI apps are more likely to reach for` pydantic-settings` specifically, but the underlying mechanism doesn't change. A` Settings` class still reads from` os.environ` , so it picks up variables from any source that sets them before the app boots,` .env` file or otherwise.


## What actually replaces a shared .env file


A[secrets manager](https://infisical.com/blog/secrets-management-complete-guide) is the category of tool built to answer the leak risk, the sharing friction, and the missing rotation and audit trail directly.


- Instead of every developer, server, and CI job holding its own copy of a` .env` file, the secrets live in one centrally managed encrypted store.
- Access is granted to a specific identity (a person, a server, a CI pipeline) rather than to whoever happens to have a copy of the file.
- every read gets logged, and rotating a compromised credential means changing it in one place instead of hunting down every file that had a copy.


Infisical is one such platform, and the most direct way to try it against an existing Python app doesn't touch application code at all.


Moving off an existing` .env` file doesn't mean retyping every value by hand either. Infisical can import a` .env` file directly, reading every` KEY=value` line into a chosen environment (development, staging, production) in one step.


After logging in and connecting, the[Infisical CLI](https://infisical.com/docs/cli/overview) can then start any Python process with real secrets already injected as environment variables:


```text
infisical run -- python manage.py runserver
infisical run -- flask run
infisical run -- uvicorn main:app


```


` os.environ\["DATABASE_URL"\]` still works exactly the same way it did with a` .env` file.` pydantic-settings` still validates the same fields.


The only thing that changes is where the value comes from: a centrally managed, access-controlled store instead of a static file, with every environment (development, staging, production) pointing at its own set of secrets instead of its own file.


For cases that need more than injecting a static value, the[Python SDK](https://pypi.org/project/infisicalsdk/) fetches secrets programmatically:


```text
from infisical_sdk import InfisicalSDKClient


client = InfisicalSDKClient(host="https://app.infisical.com")
client.auth.universal_auth.login(client_id, client_secret)


secret = client.secrets.get_secret_by_name(
secret_name="DATABASE_URL",
project_id=project_id,
environment_slug="prod",
secret_path="/",
)


```


That opens the door to things a static` .env` file can never do, like[dynamic secrets](https://infisical.com/blog/secret-rotation-vs-dynamic-secrets) that generate a short-lived database credential on demand instead of reusing the same password indefinitely.


## When dotenv is still the right call


None of this means` python-dotenv` is wrong to reach for. For a single-developer prototype, a script that runs once, or a personal project with no teammates and no production deployment,` os.environ` and a` .env` file are still the simplest option available, and adding a secrets manager would be more infrastructure than the problem calls for.


The moment a second developer, a second environment, or a real deployment shows up, it’s important to move off` .env` files. This doesn’t require rewriting how the app reads its configuration. It just means pointing` infisical run` at the same start command already in use. Infisical's secrets management covers this end to end, from local development to production, and it's the same platform to reach for later if certificates or privileged access become part of the picture too.


[Start a free trial today](https://infisical.com/pricing) or[talk to one of our experts](https://infisical.com/talk-to-us) to see how Infisical fits into a Python codebase already using` os.environ` .
