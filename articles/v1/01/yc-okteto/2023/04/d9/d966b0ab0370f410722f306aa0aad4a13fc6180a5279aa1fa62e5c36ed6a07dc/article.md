---
schema_version: "1.0.0"
document_id: "d966b0ab0370f410722f306aa0aad4a13fc6180a5279aa1fa62e5c36ed6a07dc"
company_key: "yc-okteto"
company: "Okteto"
source_id: "yc-okteto-rss-a64bce3f80ea"
canonical_url: "https://www.okteto.com/blog/product-update-april-2023/"
published_at: "2023-04-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.770894+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:5890b2e50e20bfc2391a939328d281faf916e6eea5e572225f425842beb2b0e6"
---

# Okteto Chart 1.6, 1.7 & CLI 2.14 released with Kubernetes 1.24 support

# Okteto Chart 1.6, 1.7 & CLI 2.14 released with Kubernetes 1.24 support


We're excited about March's product update because it contains many improvements to the Okteto product experience. We've updated our CLI to version 2.14, added support for dynamic endpoints for external resources, and more!


Below is a detailed list of the features and improvements that shipped in March with links to the documentation to learn more.


## New features


### Added support for Kubernetes 1.24


Okteto is striving for N-2 version support for Kubernetes to ensure we remain aligned with cloud infrastructure providers and the regular channel releases from the CNCF and Kubernetes community.


This change, and our broader effort to stay current with new Kubernetes versions, will ensure a consistent and stable experience on Okteto.


### Updated the Okteto CLI version to 2.14


The latest version of the Okteto CLI includes several new features and some bug fixes. We continue to invest in the CLI to provide a great developer experience to our customers and end users because it is the entry point for most developers and is core to the Okteto product experience.


[Documentation](https://github.com/okteto/okteto/releases/tag/2.14.0)


### Customize endpoints shown in the Okteto UI


You can now use the **dev.okteto.com/endpoints** annotation in your **Ingress** or **VirtualServices** resource to define a list of endpoints (separated by commas) that will replace the default endpoints used by Okteto.


This change also expands the **OKTETO_NAMESPACE** and **OKTETO_DOMAIN** environment variables to make endpoints portable between namespaces.


[Documentation](https://www.okteto.com/docs/core/endpoints/automatic-ssl#customizing-the-endpoints-shown-in-the-okteto-dashboard)


### Support for configuring the buildkit service account


Buildkit is the build service used in conjunction with **okteto build** to build containers directly in the cluster. The service account for buildkit is now configurable to provide you with more flexibility and control over your Okteto implementation.


[Documentation](https://www.okteto.com/docs/self-hosted/administration/configuration/#buildkit)


### Token-based authentication for simpler self-hosted installation


When installing[Okteto Self-Hosted](https://www.okteto.com/docs/self-hosted/) , we now default to token-based authentication. This simplifies the installation process by removing the need to initially implement more complex authentication systems, seeking internal approvals to get access to those systems, and reducing the time to first exploration of Okteto.


Once you've explored Okteto with this initial implementation, you can switch to another authentication method that is more ideal for expanding to multiple users and production environments.


[Documentation](https://www.okteto.com/docs/self-hosted/administration/configuration/#token)


### Added automatic Personal Access Token expiration


Starting in Okteto 1.7, Personal Access Tokens (PAT) will have a default, automatic expiration of **180 days** . We made this change to align ourselves with information security best practices and balancing the developer experience within Okteto. When a PAT expires, it can no longer be used to authenticate a user or for API requests. It is also not possible to restore an expired token; you must create a new token.


**Note:** Existing tokens are not affected by this change unless or until the Okteto instance is upgraded to 1.7 or newer. At that point, the expiration timer begins from the day the instance is upgraded.


[Documentation](https://www.okteto.com/docs/core/credentials/personal-access-tokens#token-expiration)


### Changed self-hosted registry storage default to container filesystem


When installing Okteto self-hosted, you are no longer required to use external storage for the registry. Now the installer defaults to using the **container** **filesystem storage.** This change simplifies the self-hosted installation process further to help you get setup faster.


[Documentation](https://www.okteto.com/docs/self-hosted/administration/configuration/#filesystem)


### Support custom quotas for node port and load balancer service types


Previously customers would specify desired quotas in the **values.yaml** file but Okteto would ignore values that were not defined as configurable. We've now added **maxNodePorts** and **maxLoadBalancers** to the list of configurable options at the namespace level for your Okteto instance.


[Documentation](https://www.okteto.com/docs/self-hosted/administration/configuration/#quotas)


### Public override support for self-signed certificates and bring your own certificate


We now support the definition of a **publicOverride** option in your chart configuration, which enables you to customize the domain your developers use to access your Okteto instance. This does not affect the (sub)domains used for development environment endpoints within Okteto.


For example, if **publicOverride** is set to **okteto.schrutefarms.com** , your developers would access your Okteto instance using **okteto.schrutefarms.com** but your developer endpoints would still remain **movies-dwight.example.com** .


[Documentation](https://www.okteto.com/docs/self-hosted/administration/configuration/#publicoverride)


### Improved the wake operation


The Okteto wake operation running behind the scenes now prioritizes waking dependency services first to ensure your environment is brought back up without any issues. This change aligns the wake operation with the development environment creation process by using the same order of operations.


### Added support for dynamic endpoints for external resources


You can now use dynamic endpoints for external resources you include in your Okteto manifest. This means you can create a complete list of services for your application that exists within Okteto and provides a convenient link to relevant service endpoints along with documentation to help other developers collaborating in the same namespace.


This feature supports:


- Static endpoints (a hardcoded URL in the **external** section)
- Hybrid endpoints (a mix of static + $ENV_VARS)
- Dynamic endpoints (completely dynamic using $ENV_VARS)


[Documentation](https://www.okteto.com/docs/reference/manifest/#use-dynamic-endpoints-for-an-external-url)


## Breaking changes


### Changed the default ingress class for ingresses managed by okteto


In the March release we've changed the default ingress class for ingresses managed by Okteto from **nginx** to **okteto-nginx** . We made this change to prevent collisions with other controllers that may already be installed on a cluster.


There is no action required from our users and all ingresses managed by Okteto will be automatically updated as part of this change.


[Documentation](https://www.okteto.com/docs/self-hosted/install/releases/#breaking-changes)


## See a more detailed list


For a more detailed list of improvements, bug fixes, breaking changes, and deprecation notices please see[our documentation](https://www.okteto.com/docs/self-hosted/install/releases/#170) .


[Release Notes](https://www.okteto.com/docs/self-hosted/install/releases/#170)


Matt Gonzales


Director of Product / Perpetually Underdressed 👕


[View all posts](https://www.okteto.com/blog/authors/matt-gonzales/)


#### Share this:
