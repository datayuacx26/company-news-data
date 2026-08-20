---
schema_version: "1.0.0"
document_id: "50b265959c8319f1c32a3f8a0bf09c2efbc4257ac1b0bf414f42d4385154e1ec"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/stop-using-dotenv-in-nodejs-v20.6.0+"
published_at: "2025-02-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:6916ce15b6c2b89170018a461f5e2cfbbf3700cb68563cfcc00520cd7109195a"
---

# Should You Still Use dotenv in 2025?

**TL;DR** : Despite Node.js` v20.6.0+` introducing the native` --env-file` flag to natively read and inject environment variables from a file into process.env,` dotenv` remains one of the most popular npm dependencies (45M+ weekly downloads!). Whatever your choice, you should consider using a secrets manager like Infisical for sensitive values that shouldn't be exposed, like API keys and other secrets.


Did you know that[security researchers have found over 1M secrets from over 58K websites' exposed environments files](https://cybernews.com/security/websites-exposing-million-secrets/) ?


We have previously advocated on this blog[for deprecating .env files](https://infisical.com/blog/stop-using-env-files) when storing sensitive information, recommending instead the use of a secrets manager like Infisical.


However, not all environment variable are sensitive and some developers may still prefer using .env files for local development. In this article, we'll explore the best practices for managing environment variables in Node.js applications and how to avoid common pitfalls.


## Quick Answer


- For local development with Node.js v20.6.0+: Use native` .env` support with` --env-file` flag
- For secrets (especially in prod): Use Infisical or similar secret managers
- For older Node.js versions in development: Use dotenv npm package


## Development vs Production: Best Practices


### Local Development Options


1. **Native Node.js Support** (v20.6.0+)


```text
node --env-file=.env app.js


```


1. **dotenv npm package**


```text
require('dotenv').config()


```


### Production-Ready Solutions for Secrets


For production deployments, using .env files to store secrets pose several security risks:


- Unencrypted secrets stored locally
- Difficult secret synchronization across teams
- No access control for different environments
- Risk of accidental commits to version control
- Lack of secret versioning and audit logs


Instead, use Infisical to manage your secrets:


```text
// Install Infisical CLI
npm install -g @infisical/cli


// Run your application with Infisical
infisical run --env=<secret-env> --path=<path/to/secret> -- node app.js


```


Read the[Infisical CLI](https://infisical.com/docs/cli/overview) documentation for more details on setting up and managing secrets.


## Security Considerations


### Why Move Beyond .env Files?


1. .env files are not secure


- They are not encrypted
- They are easily exposed (cf. introduction)
- You can't track who accessed which secrets or when they were used.


1. .env files are not scalable


- They are difficult to sync across team members
- There's no single source of truth for managing environment variables.
- Teams resort to sharing secrets through chat or email (bad practice)


1. .env files are not robust


- Updating secrets across environments is a manual, error-prone process.
- You can't track changes to secrets or who made them.
- Rolling back to previous versions is difficult.


## Migration Guide From .env to Infisical


1. Install Infisical CLI:


```text
npm install -g @infisical/cli


```


1. Initialize Infisical in your project:


```text
infisical init


```


1. Update your application start command: Before:


```text
{
"scripts": {
"start": "node --env-file=.env app.js"
}
}


```


After:


```text
{
"scripts": {
"start": "infisical run --env=<secret-env> --path=<path/to/secret> -- node app.js"
}
}


```


## F.A.Q.


### How do I set process.env in Node.js?


You have three main options:


- Using the --env-file flag (Node.js 20.6.0+)
- Using the dotenv npm package
- Using a secrets manager like Infisical (recommended for production)


### Should I still use dotenv in 2025?


While` dotenv` is fine for local development, it's not recommended for production environments. Instead:


- Use native .env support for non-senstitive values with Node.js 20.6.0+
- Use Infisical for secrets to ensure proper security and team collaboration
- Consider` dotenv` only if you need to support older Node.js versions in development


### How do I know if an environment variable is sensitive?


Passwords, API keys, access tokens, database credentials or any form of authentication credentials are considered sensitive. Additionaly, any variable storing personally identifiable information (PII) or other confidential data should be treated as sensitive as well.


When in doubt, it's better to err on the side of caution and treat all environment variables as sensitive.


### What are the benefits of using Infisical over .env files?


- Your secrets stay encrypted at all times, unlike plain text in .env files.
- Everyone on your team automatically gets the right secrets with proper access controls.
- You can easily switch back to any previous version of your secrets if needed.
- Every secret access and change is tracked, giving you a clear security trail.
- Works smoothly with GitHub Actions, AWS, and other deployment tools.
- Can run on your own servers for complete control over your secrets.


## Best Practices for Production Deployments


- **Never commit secrets to version control**
- Use Infisical's CLI for local development
- Implement secret rotation policies
- Monitor access through audit logs
- Use different secrets for development/staging/production
- Regularly update secrets and access policies


## Resources


- [Why You Should Stop Using .env Files](https://infisical.com/blog/stop-using-env-files)
- [Node.js Native .env Documentation](https://nodejs.org/api/cli.html#--env-fileconfig)
- [How to use Infisical for your Node stack](https://infisical.com/docs/documentation/guides/node)
