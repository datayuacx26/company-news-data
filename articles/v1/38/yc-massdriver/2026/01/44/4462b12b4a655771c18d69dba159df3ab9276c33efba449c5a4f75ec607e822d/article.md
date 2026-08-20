---
schema_version: "1.0.0"
document_id: "4462b12b4a655771c18d69dba159df3ab9276c33efba449c5a4f75ec607e822d"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/changelog-version-1.2.0"
published_at: "2026-01-06T00:00:00+00:00"
first_seen_at: "2026-07-27T21:32:22.131944+00:00"
fetched_at: "2026-07-28T22:24:12.939989+00:00"
content_hash: "sha256:0845ccda58c4dddc90d092c0ee67841e82021c208281aad75f1db408fd4dfa3d"
---

# Massdriver Platform Update — Version 1.2.0

## 🎉 New Features


### CLI Commands


#### ` mass logs` - Get Deployment Logs


Retrieves and outputs the log stream for a specific deployment. The logs are dumped to stdout in their original format, making it easy to pipe to files or other tools for analysis. View deployment logs from the command line for debugging, monitoring, or audit purposes. Useful for CI/CD pipelines, troubleshooting failed deployments, or analyzing deployment behavior.


```text
mass logs 12345678-1234-1234-1234-123456789012


```


---


#### ` mass artifact get` - Get Artifact Details


Retrieves detailed information about a specific artifact, including ID, name, type, artifact definition details, package information (if provisioned), specs, metadata, and available download formats. Inspect artifact details, verify artifact configuration, check available formats, and understand artifact relationships. Supports both imported artifacts (UUID) and provisioned artifacts (friendly slug).


```text
mass artifact get api-prod-database-connection --output json


```


---


#### ` mass artifact download` - Download Artifact


Downloads an artifact in the specified format. The artifact data is rendered according to the artifact definition's schema and returned in the requested format (JSON, YAML, etc.). Export artifact data for use in external tools, scripts, or configurations. Perfect for integrating artifact data into CI/CD pipelines, Terraform configurations, or other infrastructure-as-code tools.


```text
mass artifact download api-prod-database-connection --format yaml


```


---


#### ` mass definition delete` - Delete Artifact Definition


Permanently deletes an artifact definition from Massdriver. This action cannot be undone and requires administrator permissions. The command includes a safety confirmation prompt. Remove unused or obsolete artifact definitions from your organization. Useful for cleaning up custom definitions that are no longer needed or were created for testing purposes.


```text
mass definition delete my-org/aws-s3 --force


```


---


### Massdriver Catalog Template


A bootstrap catalog template repository for self-hosted Massdriver instances. This template helps platform teams model their infrastructure architecture and developer experience before writing infrastructure code.


**Repository:**[https://github.com/massdriver-cloud/massdriver-catalog](https://github.com/massdriver-cloud/massdriver-catalog)


**What's Included:**


- Example artifact definitions (Network, PostgreSQL, MySQL, storage bucket schemas)
- Template bundles with complete` massdriver.yaml` configs, parameter schemas, and UI definitions
- Credential definitions for AWS, Azure, and GCP
- Development workflow with Makefile for building/validating/publishing
- Pre-commit hooks for YAML/JSON/Terraform formatting
- GitHub Actions workflow for automated publishing


**Workflow:**


1. Fork the repository and customize artifact definitions to match your infrastructure patterns
2. Model your architecture in Massdriver using the bundle schemas (no code needed yet)
3. Test the developer experience—add bundles to a canvas, connect them, configure parameters
4. Replace placeholder OpenTofu/Terraform code with your actual implementation when ready
5. Publish and iterate based on developer feedback


**Get Started:**


```text
git  clone   https://github.com/massdriver-cloud/massdriver-catalog
cd   massdriver-catalog
make  help


```


This template enables you to design your platform's self-service interface—what bundles to offer, how they connect, what parameters developers configure—before implementing any infrastructure code.


---


### Operator Guide Interpolation


Operator guides now support dynamic content interpolation using Mustache or Liquid templating engines. This allows you to reference package configuration values, connection specs, and artifact outputs directly in your documentation, creating context-aware, self-updating operational guides that reflect actual deployed configurations.


**Key Features:**


- Reference package parameters, connection specs, and artifact specs in operator guides
- Automatically populate guides with deployment-specific information
- Reduce documentation maintenance by eliminating hardcoded values
- Ensure guides stay synchronized with actual configurations
- Support for both Mustache and Liquid template engines


**Documentation:**[https://docs.massdriver.cloud/guides/operator-guides](https://docs.massdriver.cloud/guides/operator-guides)


---


### Package Configuration Management


#### ` copyPackage` Mutation


Copy package configuration, version, and release strategy from one package to another with optional overrides. This is perfect for promoting configurations between environments (e.g., from production to staging) or duplicating package setups.


**Key Features:**


- Deep merges copied parameters with optional overrides
- Automatically copies version and release strategy
- Optionally includes secrets
- Respects` $md.copyable: false` schema flags to exclude non-copyable fields


```text
mutation   CopyPackage (
$organizationId  : ID !
$srcPackageId  : ID !
$dstPackageId  : ID !
)    {
copyPackage (
organizationId  :    $organizationId
srcPackageId  :    $srcPackageId
dstPackageId  :    $dstPackageId
overrides  :    "{\"size\": \"small\"}"
includeSecrets  :    false
)    {
successful
result  {
id
params
version
releaseStrategy
}
}
}


```


**Use Cases:**


- Promote production configurations to staging environments
- Duplicate package configurations across multiple environments
- Standardize package settings across similar deployments
- Copy configurations while overriding environment-specific values (e.g., instance sizes)


---


### Environment-Level Operations


#### ` deployEnvironment` Mutation


Deploy all packages in an environment in a single operation. Packages are automatically deployed in the correct dependency order, ensuring upstream dependencies are provisioned before downstream services.


**Key Features:**


- Deploys all packages in an environment atomically
- Respects package dependency ordering
- Optional deployment message for audit trails
- Returns deployment status for all packages


```text
mutation   DeployEnvironment (
$organizationId  : ID !
$environmentId  : ID !
$message  : String
)    {
deployEnvironment (
organizationId  :    $organizationId
environmentId  :    $environmentId
message  :    $message
)    {
successful
result  {
id
slug
packages  {
id
slug
deployments  {
id
status
action
}
}
}
}
}


```


**Use Cases:**


- Deploy entire environments after configuration changes
- Promote environments through deployment pipelines
- Re-deploy environments after infrastructure updates
- Initialize new environments with all required packages


---


#### ` decommissionEnvironment` Mutation


Decommission all packages in an environment in a single operation. Packages are automatically decommissioned in reverse dependency order, ensuring downstream services are removed before upstream dependencies.


**Key Features:**


- Decommissions all packages in an environment atomically
- Respects reverse dependency ordering
- Optional decommission message for audit trails
- Returns decommission status for all packages


```text
mutation   DecommissionEnvironment (
$organizationId  : ID !
$environmentId  : ID !
$message  : String
)    {
decommissionEnvironment (
organizationId  :    $organizationId
environmentId  :    $environmentId
message  :    $message
)    {
successful
result  {
id
slug
packages  {
deployments  {
id
status
action
}
}
}
}
}


```


---


#### ` forkEnvironment` Mutation


Fork an environment to create a new environment that tracks its parent. This is useful for creating staging or preview environments based on production configurations, or for experimenting with changes in isolation.


**Key Features:**


- Creates a new environment with a parent relationship
- Optionally copies secrets, environment defaults, and remote references
- Automatically initializes packages for all manifests in the project
- Maintains parent-child relationship for tracking and merging


```text
mutation   ForkEnvironment (
$organizationId  : ID !
$parentId  : ID !
$input  : ForkEnvironmentInput !
)    {
forkEnvironment (
organizationId  :    $organizationId
parentId  :    $parentId
input  :    $input
)    {
successful
result  {
id
name
slug
parent  {
id
name
}
}
}
}


```


**Use Cases:**


- Create staging environments based on production configurations
- Set up preview environments for pull requests
- Experiment with configuration changes in isolation
- Duplicate environments for testing or development


---


#### ` mergeEnvironment` Mutation


Merge a forked environment back into its parent. This allows you to promote changes from a fork (e.g., staging) back to the parent environment (e.g., production) after testing and validation.


**Key Features:**


- Merges package parameters from fork to parent by default
- Optionally copies secrets, environment defaults, and remote references
- Only works on environments that have a parent (forks)
- Preserves the parent environment while updating its configuration


```text
mutation   MergeEnvironment (
$organizationId  : ID !
$id  : ID !
$input  : MergeEnvironmentInput
)    {
mergeEnvironment (  organizationId  :    $organizationId  ,  id  :    $id  ,  input  :    $input  )  {
successful
result  {
id
name
slug
packages  {
id
params
}
}
}
}


```


**Use Cases:**


- Promote tested configurations from staging to production
- Merge validated changes from preview environments
- Consolidate configuration updates across environment hierarchies
- Update parent environments with changes validated in forks


---


#### ` deleteEnvironment` Mutation (Enhanced)


The` deleteEnvironment` mutation now supports an` orphanForks` option to handle environments that have been forked.


**Key Features:**


- Prevents deletion of environments with forks by default
- Optional` orphanForks` flag to set all forks' parent to none before deletion
- Ensures data integrity by requiring explicit handling of child environments


```text
mutation   DeleteEnvironment (
$organizationId  : ID !
$id  : ID !
$orphanForks  : Boolean
)    {
deleteEnvironment (
organizationId  :    $organizationId
id  :    $id
orphanForks  :    $orphanForks
)    {
successful
}
}


```


---


### OIDC/SSO Provider Configuration


Configure custom OIDC (OpenID Connect) providers for single sign-on authentication. Self-hosted instances can configure multiple OIDC providers by setting environment variables.


**Key Features:**


- Support for multiple OIDC providers simultaneously
- Automatic provider detection from environment variables
- Optional auto-join organization feature
- Query configured providers via the` server` query


**Supported Providers:** GitHub, Google, Microsoft (Azure AD / Entra ID)


#### Environment Variable Format


OIDC providers are configured using environment variables following this pattern:


```text
MD_OIDC__{PROVIDER}__{FIELD}


```


**Example:**


```text
MD_OIDC__{PROVIDER}__CLIENT_ID=your_client_id
MD_OIDC__{PROVIDER}__CLIENT_SECRET=your_client_secret
MD_OIDC__{PROVIDER}__AUTHORIZE_URL=https://...
MD_OIDC__{PROVIDER}__TOKEN_URL=https://...
MD_OIDC__{PROVIDER}__AUTOJOIN_ORGANIZATION=myorg   # Optional


```


**Required Fields:**


- ` CLIENT_ID` - OAuth client ID from your OIDC provider
- ` CLIENT_SECRET` - OAuth client secret from your OIDC provider
- ` AUTHORIZE_URL` - OAuth authorization endpoint URL
- ` TOKEN_URL` - OAuth token endpoint URL


**Optional Fields:**


- ` SCOPE` - OAuth scopes (defaults are provided per provider)
- ` AUTOJOIN_ORGANIZATION` - Organization slug or ID to automatically join users upon first login


#### Checking Configured Providers


Use the` server` query to verify your OIDC providers are configured correctly:


```text
query    {
server  {
ssoProviders  {
name
loginUrl
}
}
}


```


**Note:** The` ENTRA` provider name is deprecated. Use` MICROSOFT` instead for Azure AD / Entra ID configurations.


---


### OpenTelemetry (OTEL) Tracing Support


Added comprehensive OpenTelemetry tracing support across the platform and provisioning system for self-hosted installations. This provides rich observability and distributed tracing capabilities, enabling deeper insights into system stability and performance.


**Key Features:**


- Distributed tracing across all platform services
- Provisioning pipeline observability with trace context propagation
- Integration with OTEL-compatible backends (Jaeger, Zipkin, Honeycomb, etc.)
- Performance monitoring and bottleneck identification
- Error tracking and debugging capabilities
- Request flow visualization across microservices


**Configuration:**


Enable OTEL tracing in your self-hosted installation by configuring the OpenTelemetry collector and exporter settings in your Helm values:


```text
opentelemetry:
enabled:    true
collector:
endpoint:    'http://otel-collector:4317'
serviceName:    'massdriver'
tracesSampleRate:    1.0    # Adjust sampling rate as needed


```


**Benefits:**


- Understand end-to-end request flows from API to infrastructure provisioning
- Identify performance bottlenecks in deployment pipelines
- Debug complex issues with complete trace context
- Monitor system health and reliability metrics
- Optimize provisioning workflows based on performance data


**Reference:**[OTEL Implementation Commit](https://github.com/massdriver-cloud/helm-charts/commit/1e1ce7ae8a90f4ebad0475f688d3d018056aa1e6)


---


## 🔧 Technical Details


### Package Copying


- Fields marked with` $md.copyable: false` are automatically excluded
- Secrets are only copied when` includeSecrets: true` is explicitly set
- Version and release strategy are always copied from source


### Environment Operations


- **Deploy** : Packages deployed in dependency order (upstream first)
- **Decommission** : Packages decommissioned in reverse dependency order (downstream first)
- **Fork** : Creates parent-child relationship; only params copied by default
- **Merge** : Only works on environments with a parent relationship
- **Delete** : Environments with forks require` orphanForks: true` to delete


### OpenTelemetry


- Distributed tracing enabled across platform and provisioning services
- Configurable OTEL collector endpoint and sampling rates
- Compatible with standard OTEL backends (Jaeger, Zipkin, Honeycomb, etc.)
- Trace context propagation through deployment pipelines


---


## 📖 Documentation


For more information, see:


- [GraphQL API Documentation](https://docs.massdriver.cloud/api)
- [Package Management Guide](https://docs.massdriver.cloud/guides/packages)
- [Environment Management Guide](https://docs.massdriver.cloud/guides/environments)
- [Operator Guides](https://docs.massdriver.cloud/guides/operator-guides)
- [Massdriver Catalog Template](https://github.com/massdriver-cloud/massdriver-catalog)


**Breaking Changes:** None. All new features are additive.


---


## 🙏 Feedback


We'd love to hear your feedback on these new features! Please reach out via:


- GitHub Issues:[https://github.com/massdriver-cloud/massdriver/issues](https://github.com/massdriver-cloud/massdriver/issues)
- Community Slack:[https://massdriver.cloud/slack](https://massdriver.cloud/slack)
