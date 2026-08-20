---
schema_version: "1.0.0"
document_id: "2375324d766108d126cac0a2746080134fd0085628670fbae5fea08862a64773"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/changelog-version-1.3.0"
published_at: "2026-02-19T00:00:00+00:00"
first_seen_at: "2026-07-27T21:32:22.131944+00:00"
fetched_at: "2026-07-28T22:19:38.682305+00:00"
content_hash: "sha256:e2609bbfd0051426a77ed8050bcaf721c5909a65dfe12d83c8d214909847dfe6"
---

# Massdriver Platform Update — Version 1.3.0

*February 2026*


## Highlights


- **SCIM 2.0 Identity Provisioning** — Automate user and group sync with your identity provider
- **Integrations Console** — Configure and manage integrations from the UI
- **Claude Code Plugin** — AI-assisted infrastructure bundle development


---


## 🎉 New Features


### SCIM 2.0 Identity Provisioning


Automate user and group synchronization with your identity provider (Okta, Azure AD, OneLogin, etc.).


- **Automatic provisioning** : New hires get access the moment they're added to your IdP
- **Instant deprovisioning** : Revoke access immediately when someone leaves
- **Group sync** : Map IdP groups to Massdriver groups for role-based access control
- **Audit compliance** : Centralized identity management for SOC 2 and compliance


Enable SCIM from **Settings → Integrations** in the console, or via the GraphQL API.


> **Important** : The bearer token is only returned once during activation. Store it securely.


---


### Integrations Console


Manage all integrations from a single page:


- **Visual configuration** : Form-based setup with schema validation
- **Status monitoring** : See active integrations, last run, and next scheduled run
- **Setup instructions** : Copy-paste ready configuration for external services


Navigate to **Settings → Integrations** to get started.


---


### Package Properties View


See deployed resource properties directly in the console—configuration values, resource IDs, endpoints, and connection strings.


---


### CLI Improvements


**New commands:**


```text
# List packages in a project or environment
mass package list  $project  - $env


# List available bundles
mass bundle list


# Reset a package to redeploy from scratch
mass package reset <package-id>


```


**Artifact definition support** : The CLI now validates and publishes custom artifact types defined in` massdriver.yaml` .


---


### Claude Code Plugin


Build infrastructure bundles faster with AI-assisted development. The Massdriver plugin for Claude Code provides guardrails, patterns, and validation rules for bundle development.


**Installation:**


```text
# Add the marketplace
/plugin marketplace add massdriver-cloud/claude-plugins


# Install the plugin
/plugin install massdriver@massdriver-cloud-claude-plugins


```


**Capabilities:**


- Guides bundle development with proper lifecycle scoping
- Enforces critical rules (namespace collisions, artifact matching, generated files)
- Provides patterns for connections, artifacts, alarms, and Checkov compliance
- Includes copy-paste snippets for common bundle files


**Auto-activation:** The skill automatically activates when working in` bundles/` ,` artifact-definitions/` , or` platforms/` directories, editing` massdriver.yaml` files, or asking about bundles, artifacts, connections, or Massdriver patterns.


**Repository:**[https://github.com/massdriver-cloud/claude-plugins](https://github.com/massdriver-cloud/claude-plugins)


---


### Massdriver Catalog Updates


The[Massdriver Catalog](https://github.com/massdriver-cloud/massdriver-catalog) now supports custom artifact definitions directly in` massdriver.yaml` .


**New Features:**


- **Custom artifact definitions** : Define artifact types with schema, specs, and connection configuration directly in your catalog's` massdriver.yaml` files
- **Platform definitions** : Declarative` massdriver.yaml` format for defining cloud platform integrations (AWS IAM Role, Azure Service Principal, GCP Service Account, Kubernetes)
- **Operator guide interpolation** : Reference package parameters, connection specs, and artifact data in operator runbooks using Mustache or Liquid templating


**Workflow improvements:**


- GitHub Actions workflow for automatic publishing on push to` main`
- Pre-commit hooks for YAML/JSON/Terraform formatting and validation
- ` make all` command for building and publishing entire catalog


**Repository:**[https://github.com/massdriver-cloud/massdriver-catalog](https://github.com/massdriver-cloud/massdriver-catalog)


---


## 🔧 Additional Improvements


- Improved error messages for bundle validation failures
- Performance improvements for large environment deployments
- Enhanced audit logging for SCIM operations


---


## Breaking Changes


None. All changes are additive and backward compatible.


---


## 📖 Documentation


For more information, see:


- [Claude Code Plugin](https://github.com/massdriver-cloud/claude-plugins)
- [Massdriver Catalog](https://github.com/massdriver-cloud/massdriver-catalog)
- [CLI Reference](https://docs.massdriver.cloud/cli/overview)
- [GraphQL API Documentation](https://docs.massdriver.cloud/swapi)


---


## 🙏 Feedback


We'd love to hear your feedback on these new features! Please reach out via:


- GitHub Issues:[https://github.com/massdriver-cloud/massdriver/issues](https://github.com/massdriver-cloud/massdriver/issues)
- Community Slack:[https://massdriver.cloud/slack](https://massdriver.cloud/slack)
