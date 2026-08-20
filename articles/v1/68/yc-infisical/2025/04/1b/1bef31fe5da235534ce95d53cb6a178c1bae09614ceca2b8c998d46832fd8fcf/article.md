---
schema_version: "1.0.0"
document_id: "1bef31fe5da235534ce95d53cb6a178c1bae09614ceca2b8c998d46832fd8fcf"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/migration-sealed-secrets"
published_at: "2025-04-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:0a1be01eb3eeee869f15651ad26675a81fec94f62a2c8404e056421b140b8676"
---

# Migrating From Sealed Secrets to Infisical: A Step-by-Step Guide

Bitnami's Sealed Secrets is a popular Kubernetes-native solution for managing secrets in GitOps workflows. It allows teams to store encrypted secrets alongside their code in Git, maintaining a declarative, version-controlled source of truth without exposing sensitive data.


While many teams that work with Kubernetes find it very easy to get started with Sealed Secrets, as practices mature and the number of secrets grows, they often need to scale secrets management beyond what it can provide. This is especially true in larger organizations or multi-cluster environments, where the limitations become more apparent.


This article provides a step-by-step guide for migrating from Sealed Secrets to an external secrets management solution, using Infisical as the primary example while ensuring the principles apply broadly to most external secrets platforms. We explore Sealed Secrets' limitations, highlight the advantages of external secrets management, and outline a detailed migration process to ensure a smooth transition.


## What are Sealed Secrets?


When we talk about Sealed Secrets, we are often referencing the way to bring externally encrypted secrets into your Kubernetes cluster using a` SealedSecret` custom resource definition (CRD).


The idea is that developers use the` kubeseal` CLI to encrypt secrets embed in standard Kubernetes Secret manifests with the controller's public key. These SealedSecrets can only be decrypted by the controller running in the target cluster. Not even the author who sealed it (nor anyone with access to the Git repo) can retrieve the plain secret without the cluster's private key​. This means that even if the Git repository is compromised, the secrets remain safe​. Here's an overview of the GitOps workflow with Sealed Secrets:


The key benefits are that developers only need to use one CLI (` kubeseal` ) to encrypt the sensitive values, and then the manifest can be securely committed to Git. While they need access to the cluster to obtain the public key, they do not need to manage any external secrets management system. The controller takes care of decrypting the SealedSecret into a regular Kubernetes Secret when it is applied to the cluster, and this decryption process is transparent to the user.


In an Argo CD or Flux GitOps setup, SealedSecrets can be treated like any other manifest. Argo CD simply applies the SealedSecret manifest, which means the CI/CD pipeline or Argo CD doesn't need access to plaintext secrets – decryption happens in-cluster at deploy time​.


Finally, the controller also handles the rotation of the encryption keys, so there is no need to manage external systems for encryption keys. This is a significant advantage in terms of developer experience when secrets are stored in Git, as it allows developers to focus on writing code rather than worrying about managing secrets.


However, as the number of secrets grows or the complexity of the environment increases, teams often find that Sealed Secrets is no longer sufficient.


### Limitations of Sealed Secrets


Here are some key drawbacks of Sealed Secrets:


-


**Security responsibility** : You bear full responsibility for the Sealed Secrets controller and its private key. If either is compromised, all secrets become vulnerable. One of the most important security practices when using Sealed Secrets is to back up the controller's private key, preferably in an external system like a KMS. This creates a need for out-of-Git secret management, which defeats the "pure GitOps" advantage (where everything, including secrets, is stored in Git).


-


**Limited access control** : Sealed Secrets are cluster-wide resources, potentially allowing all cluster users to view all secrets. For multi-cluster environments (dev/staging/prod), you must either encrypt secrets separately for each cluster or reuse encryption keys across clusters. You cannot have both convenience and security isolation.


-


**Audit limitations** : With Sealed Secrets, Git commits only show that a secret changed, not what changed. This approach lacks comprehensive audit capabilities that naming conventions or commit messages cannot replace.


-


**Secret rotation** : Although Sealed Secrets can rotate its own encryption keys, your application secrets require manual re-sealing and updating when rotated. This process becomes increasingly complex at scale.


-


**Growing complexity** : As the number of secrets grows, you may end up with many large encrypted files, making it difficult to track changes and manage access.


### Benefits of Migrating to an External Secrets Management Solution


-


**No Secrets in Git** : You no longer need to store even encrypted secrets in your repo. Instead, you store a stub/reference (for example, an ExternalSecret resource). This eliminates the risk of someone committing a plaintext secret by mistake and reduces repository clutter from large encrypted blobs.


-


**Dynamic Updates** : With Sealed Secrets, any secret change is tied to a Git commit (and Argo/Flux CD sync). With an external secrets manager, you deploy a static reference to the secret (like an ExternalSecret CR) and then update the actual secret value in the external store. The External Secrets mechanism will detect the change and update the in-cluster Secret without requiring a new Git commit. This decouples the secret value from the Git commit process.


-


**Central Management and Audit** : A secrets manager provides a central UI/CLI where all secrets across projects and environments can be managed. This comes with audit logs, role-based access control, and usage tracking​. For example, with Infisical, you can see who updated a secret and when, and you can enforce policies like requiring certain approvals or ensuring secrets meet complexity rules. This centralization is easier to manage than having secrets spread across many Git repos encrypted with various keys.


-


**Flexibility** : An external secrets manager is not tied to Kubernetes and can integrate elsewhere in the DevOps cycle, which unifies secrets handling in one platform for both infrastructure and application deployment needs. For example, Infisical can be used to manage secrets for applications running outside of Kubernetes (like VMs or serverless functions) and can also be integrated with CI/CD (GitHub Actions, GitLab CI, etc.) through its CLI or action, allowing you to inject secrets into pipelines securely (via short-lived tokens, OIDC auth, etc.).


-


**More Security Options** : It's advantageous to have more security options for authentication than relying solely on one private key for all operations. Most secrets management solutions support modern practices like short-lived credentials (via OIDC) and dynamic secrets. For instance, you can configure a Kubernetes service account or a CI job to authenticate to Infisical using OIDC, avoiding long-lived API tokens​. This means even the credential that your cluster uses to fetch secrets can be rotated automatically or scoped tightly. Additionally, Infisical's Kubernetes Operator has features for dynamic secrets (automatic generation of secrets that rotate on schedule or expire)​, which go beyond what static SealedSecrets could do.


-


**Bi-Directional Sync and Secret Push** : Uniquely, Infisical's own operator supports not just pulling secrets into Kubernetes, but also pushing them out. For example, if an application generates a new key at runtime and you want to store it back in Infisical, an InfisicalPushSecret CRD can capture that and send it to Infisical​. This bi-directional sync means your external store can remain the single source of truth even if some secrets originate from within the cluster.


### Key Considerations When Migrating to an External Secrets Management Solution


Before making the move, it's important to evaluate several architectural and operational factors that will impact your environment:


-


You are adding an external service to your architecture. **This creates a dependency** that may require establishing reliable connectivity between your clusters and the service, whether cloud-hosted or self-managed. While this adds a component to your architecture, it also introduces a dedicated system specifically designed for secrets management with enhanced security features.


-


With SealedSecrets, everything needed to decrypt secrets was contained in the Git repo and cluster. Now your workflow will depend on an external system being available. This presents an opportunity to implement **robust high-availability practices** for your secrets infrastructure. Most solutions offer caching mechanisms where secrets are stored locally after syncing, ensuring applications maintain access to existing secrets even during temporary connectivity issues. Consider this an opportunity to define clear SLAs for your secrets management infrastructure.


-


As with any third-party service, you will need **appropriate security vetting** . This should encourage regular security reviews and establish formal evaluation criteria for all components in your infrastructure. Using short-lived credentials, implementing tight RBAC policies, and leveraging machine identity tokens can actually enhance your overall security posture compared to managing encryption keys manually.


-


Your team will need to **incorporate the new secrets management tool into existing GitOps processes** . While this represents a change, it typically results in improved workflows with more intuitive interfaces for secret handling compared to command-line encryption tools. This transition provides an ideal opportunity to document and refine your secrets management practices across the organization.


-


External secrets management solutions **require additional resources** , particularly if self-hosted. Cloud-hosted versions typically offer tiered pricing models with free options for smaller teams. When evaluating costs, consider the offsetting benefits of reduced operational overhead, improved security, and enhanced productivity that come with specialized secrets management tools.


To sum things up, moving to an external secrets manager does mean you'll need to consider some new factors, but these aren't really problems—they're opportunities to improve your secrets handling approach. This change helps you create clear rules about how you manage secrets, which makes your systems more secure and your team more efficient in the long run. When you look at the big picture, these tools are designed to make your life easier, not harder.


## Migrating from Sealed Secrets to Infisical


Now that we've covered the considerations, let's dive into how to actually make the switch to an external secrets management solution like Infisical. Good news—the process is surprisingly straightforward, especially for platform engineers already familiar with Kubernetes. The integration fits nicely into existing GitOps workflows with minimal disruption, allowing you to maintain your declarative configuration approach while gaining all the benefits of external secrets management.


Migrating from Sealed Secrets to Infisical involves two high-level moves:


1. **Adopting Infisical for secret storage and distribution** (deploying Infisical itself, and integrating a method for Kubernetes to fetch secrets from it).
2. **Replacing SealedSecrets manifests in your GitOps repo with references to Infisical** (and ensuring all secret data is transferred to Infisical).


### Step 1: Set up Infisical (Cloud or Self-Hosted)


First, decide whether you will use **Infisical Cloud** or **self-host** Infisical. The cloud version is ready to use – you'd sign up and create an organization/project for your secrets. Self-hosting requires deploying Infisical's server (which you can run in a Kubernetes cluster or on a VM – Infisical provides a Helm chart for easier installation).


In either case, you will create a Project and define Environments in Infisical (e.g., dev, staging, prod) to organize your secrets. Ensure you also configure user access control in Infisical (who can view or edit secrets) according to your team needs.


For self-hosted, after deploying, note the API endpoint URL (it might be an internal URL or custom domain you set up). For cloud, the API endpoint is` https://app.infisical.com/api` (default). These endpoints will be used by the K8s integration. Also, create a[Machine Identity](https://infisical.com/blog/introducing-machine-identities) in Infisical – this is a service account for your Kubernetes cluster to authenticate. Infisical supports a["universal" machine identity](https://infisical.com/docs/documentation/platform/identities/universal-auth) which provides a Client ID and Client Secret credential, and it also supports[Kubernetes Auth](https://infisical.com/docs/documentation/platform/identities/kubernetes-auth#kubernetes-auth) , where the cluster can authenticate using its service account token rather than a fixed secret. For simplicity, you might start with the Client ID/Secret method (especially if using External Secrets Operator) and later consider OIDC for higher security.


### Step 2: Deploy Kubernetes integration (Infisical Operator or External Secrets Operator)


After setting up Infisical, you'll need to choose one of the two Kubernetes operators to help pull secrets into your cluster. Each one has distinct advantages depending on your requirements:


- [Infisical's native Kubernetes Operator](https://infisical.com/docs/integrations/platforms/kubernetes/overview#kubernetes-operator) : Purpose-built for Infisical with specialized features like bi-directional sync and dynamic secrets.
- [The generic External Secrets Operator (ESO)](https://external-secrets.io/latest/provider/infisical/) : A multi-provider operator that supports Infisical alongside many other secret backends.


**Which integration to choose?** If you value a fully **declarative GitOps** approach and want to leverage Infisical-specific features like push or dynamic secrets, the Infisical Operator is ideal. For those already using ESO with other secret providers or preferring a standardized multi-backend approach, the External Secrets Operator is the natural choice. Both solutions integrate seamlessly with Argo CD through their CRDs (` InfisicalSecret` or` ExternalSecret` +` SecretStore` ).


*Read more:[Secure GitOps Workflows: A Practical Guide to Secrets Management](https://infisical.com/blog/gitops-secrets-management)*


#### Infisical Operator approach


Install the Infisical Operator in your cluster[via its Helm chart](https://infisical.com/docs/integrations/platforms/kubernetes/overview#cluster-wide-installation) . This operator introduces CRDs like` InfisicalSecret` that you would then add to your GitOps repo for each set of secrets you want to sync. For example, an` InfisicalSecret` CR might specify "pull secrets from Infisical project X, environment Y, possibly filtered by path or labels, and create a Kubernetes Secret named Z with those values."


Here's an example of an InfisicalSecret definition:


```text
apiVersion: secrets.infisical.com/v1alpha1
kind: InfisicalSecret
metadata:
name: infisicalsecret-sample
labels:
label-to-be-passed-to-managed-secret: sample-value
spec:
hostAPI: https://app.infisical.com/api
resyncInterval: 10
authentication:
kubernetesAuth:
identityId: <machine-identity-id>
serviceAccountRef:
name: <service-account-name>
namespace: <service-account-namespace>
managedKubeSecretReferences:
- secretName: managed-secret
secretNamespace: default
creationPolicy: "Orphan"
template:
includeAllSecrets: true
data:
NEW_KEY_NAME: "{{ .KEY.SecretPath }} {{ .KEY.Value }}"


```


This example uses Kubernetes authentication with a service account token and machine identity. The CRD will create a Secret named` managed-secret` in the` default` namespace, including all secrets from the connected Infisical project/environment.


The Infisical Operator can also manage[dynamic secrets](https://infisical.com/docs/documentation/platform/dynamic-secrets/overview#dynamic-secrets) and[push secrets](https://infisical.com/docs/integrations/platforms/kubernetes/infisical-push-secret-crd) back, which might be overkill if you just need read-only sync. The operator will need credentials: in its Helm values you'd configure the Infisical API host (cloud or self-host URL) and the auth method. It supports a[Kubernetes-native auth](https://infisical.com/docs/documentation/platform/identities/kubernetes-auth#kubernetes-auth) where it uses a[service account token](https://infisical.com/docs/documentation/platform/identities/kubernetes-auth) and a pre-configured Machine Identity (OIDC), meaning you don't necessarily have to create a static secret for credentials. If using OIDC, you'd grant the service account permission in Infisical by using the machine identity's ID. Alternatively, you can provide the Client ID/Secret as a Kubernetes Secret which the operator uses to authenticate to Infisical's API.


*Read about[how to create and use identities for your applications in Kubernetes](https://infisical.com/docs/documentation/platform/identities/kubernetes-auth) .*


#### External Secrets Operator approach


ESO is a widely-adopted, multi-backend operator that supports many secret providers including Infisical. To implement this approach:


1. Install ESO in your cluster (version 0.6.0+)
2. Create a` SecretStore` resource that configures Infisical as the provider
3. Create` ExternalSecret` resources that reference specific secrets or entire paths


The` SecretStore` defines your authentication method and which Infisical project/environment to pull from. You then create` ExternalSecret` resources that specify which secrets to sync and where to store them in Kubernetes. ESO can pull individual secrets or entire collections using pattern matching, making it flexible for various migration scenarios.


Refer to the[ESO documentation](https://external-secrets.io/latest/provider/infisical/) for detailed configuration examples and best practices.


### Step 3: Transfer existing secrets from SealedSecrets to Infisical


This is a critical migration step – you need to ensure all the secret values currently stored in Git (encrypted) are moved into Infisical so that your apps continue to function after the switch. Depending on how you've been managing your "source of truth" for secrets, you have a few strategies:


-


If your team has been keeping an offline record of actual secret values (like in a password manager or a secure file), you can manually import those into Infisical through the UI or CLI. Infisical organizes secrets by environment, and each secret has a key and value (plus metadata). Create the appropriate secrets with the exact keys your applications expect (e.g., if your app expects a Kubernetes Secret` my-app-secret` with key` DB_PASSWORD` , ensure in Infisical you have an entry for` DB_PASSWORD` under the project/environment for that app).


-


If you don't have a readily available list of plaintext values, you can retrieve them from your cluster (since Sealed Secrets already created the Kubernetes Secrets). For each SealedSecret, identify the namespace and name of the resulting Secret (it's usually the same metadata name unless overridden). Use` kubectl` : e.g.,` kubectl get secret my-app-secret -n <namespace> -o yaml` to fetch the secret, decode the base64 values to plaintext. With those values, populate Infisical.


-


If you have many secrets, consider using the **Infisical CLI** or API to script the import. Infisical CLI can push secrets to a project (for instance, you could use` infisical secrets set --file="./k8-secrets.yaml"` ). This process can be automated with a script: list all SealedSecrets in Git, parse their data keys and secret name, then fetch from cluster. But be cautious: handle these plaintext values carefully and delete any temporary files after. Read more about[the Infisical secrets core command](https://infisical.com/docs/cli/commands/secrets) .


At the end of this step, all the secret data needed by your apps should reside in Infisical, organized by environment. Double-check things like: are you splitting environments (dev vs prod secrets) properly in Infisical? Does every secret key from the SealedSecrets exist in Infisical with correct values? This might be a good opportunity to eliminate unused secrets or consolidate naming for consistency.


### Step 4: Add Infisical reference manifests to Git and deploy alongside Sealed Secrets


Before removing SealedSecrets, you should add the new Infisical-based resources to your GitOps config and deploy them. This means creating the` InfisicalSecret` manifests as described in Step 2 and committing them. For each application or component that had a SealedSecret, create an InfisicalSecret referencing Infisical. You can either target the **same Kubernetes Secret names** as before, or use new ones.


- **Option A: Use same Secret names:** This approach aims for a seamless cutover. Configure the InfisicalSecret to write to the same` metadata.name` as the existing Secret. Since that Secret is already present (from SealedSecrets), the Infisical Operator will take ownership of the existing secret. The InfisicalSecret will update the Secret's data with values from Infisical. If everything is set up correctly, it will essentially "sync" and possibly overwrite the Secret with the same values (since you took them from cluster, they should match). This means effectively the secret is now managed by the Infisical Operator, but your app sees no difference.


> **Important:** Ensure the InfisicalSecret is in place **before** you remove the SealedSecret, so there's no gap in secret provisioning. The Infisical Operator will create/update the target Secret.


- **Option B: Use new Secret names:** This is a safer approach if you want to avoid any chance of conflict. You would create InfisicalSecrets that write to a new Secret (e.g.,` my-app-secret-infisical` ). You'd then update your Deployment or Pod specs to reference this new secret (update envFrom or volume references). Deploy those application changes via Argo CD, so the app starts using the new secret. Once everything is working, you can delete the old SealedSecret and old Secret. This approach has no risk of the operator fighting with the existing secret, but it does require changing your app manifests to use a different secret name, which might be more involved (and could be done gradually environment by environment).


Given that Sealed Secrets and Infisical Operator can co-exist (they operate independently), a common strategy would be to **first deploy the Infisical integration in parallel** . For a time, you'd have both the SealedSecret and the InfisicalSecret in the cluster, possibly both producing the same Secret (or two Secrets). You would verify that the Infisical-synced Secret has the correct data. You can do this by checking the Secret in Kubernetes after the Infisical Operator syncs:` kubectl describe secret my-app-secret` and see that it has the expected keys. Also check the Infisical Operator logs for any errors. Once confident, update your app to use Infisical-managed secrets if you chose a new name (or if same name, simply proceed).


### Step 5: Remove Sealed Secrets


Once you've verified your Infisical integration is working properly, remove the SealedSecret manifests from your Git repository and uninstall the controller. Watch for potential issues during removal - the controller might delete underlying Kubernetes Secrets when SealedSecret resources are removed due to owner references. Test this process in staging first if your InfisicalSecret targets the same Secret names.


Before uninstalling, export and store the controller's private key as a precaution for decrypting any old SealedSecrets you might discover later.


For enhanced security, consider implementing Infisical's OIDC authentication using[Kubernetes service account tokens](https://infisical.com/docs/documentation/platform/identities/kubernetes-auth#guide) instead of static credentials.


### Additional Considerations


-


**Testing and Rollback:** Test the migration steps in a non-prod cluster first. Verify that an application can read secrets from Infisical. If something goes wrong in production, the rollback is essentially to reapply the SealedSecret (if you removed it) and perhaps redeploy the Sealed Secrets controller. Keep a backup of those manifests until you are confident in the new system. The good news is, reading secrets from Infisical doesn't modify your application pods (unless you use the CSI driver in which case it mounts volumes – but in our described approach, it's normal Secrets). So, switching the secret source shouldn't require application code changes, just config changes.


-


**Performance and Scaling:** Infisical is built to scale, but you should be mindful of how often secrets sync. The Infisical Operator has polling-based sync capabilities that you can configure. You can adjust resync intervals to find the right balance - a very short interval might load your Infisical server unnecessarily, while a very long one might delay secret updates. A moderate sync interval (e.g., every few minutes) often works well. Also, if you have *many* secrets (thousands), consider organizing them into multiple InfisicalSecret resources rather than one giant one, for easier management and to avoid hitting size limits on a single Kubernetes Secret.


-


We should note the third option Infisical supports: **CSI driver integration** . This allows pods to mount secrets directly as volumes (bypassing Kubernetes Secrets objects entirely). While this can improve security (no secret stored in etcd), it doesn't fit as cleanly with a GitOps model (because the presence of secrets isn't represented as a Kubernetes object Argo CD manages – it's more of a runtime configuration). If you have specific apps that would benefit from CSI (for example, needing constantly updated secrets without pod restart, or not wanting any secret in etcd), you could gradually adopt CSI for those after migrating to Infisical. The CSI driver requires deploying Infisical's CSI provider and creating` SecretProviderClass` resources. That's outside the scope of this migration for most cases, but keep it in mind as an advanced option.


## Conclusion


Migrating from Sealed Secrets to Infisical can significantly streamline secret management in GitOps workflows. You'll move from a model of encrypted blobs in Git to a model of externalized secrets with references in Git, gaining flexibility in secret rotation and central oversight of secrets. Both Argo CD and Helm can work smoothly with Infisical through the Kubernetes operator, and comprehensive documentation is available in[Infisical's official documentation](https://infisical.com/docs/integrations/platforms/kubernetes/overview) .


For DevOps and platform engineers, the key takeaways are:


-


**Infisical keeps secrets out of your repos and leverages strong encryption and access control in a centralized service.** This reduces the chances of secret leaks and enables easier audits. With SealedSecrets, security is good, but managing keys and human processes at scale is error-prone.


-


Infisical provides **audit logs and a single place** to review secret changes, which can simplify compliance reporting. In contrast, reviewing Git commit history of encrypted secrets or scattered KMS logs is more laborious.


-


After migration, developers can add or update secrets via Infisical's UI or CLI **without dealing with encryption commands** . This can improve velocity (though it introduces a new tool to learn). Meanwhile, Argo CD continues to manage deployments declaratively – now it just syncs InfisicalSecret resources instead of SealedSecret resources.


-


**Many tasks (secret rotation, syncing to new clusters) become easier** . If you spin up a new Kubernetes cluster, pointing it at Infisical is straightforward – it can pull all needed secrets without you re-encrypting and applying dozens of SealedSecrets. This aligns with GitOps automation principles, letting clusters self-configure their secrets from a known source.


In making this migration, you are aligning with a broader industry trend: leveraging dedicated secret management solutions alongside GitOps. Tools like Infisical are becoming the go-to for[Kubernetes secret management in 2025](https://infisical.com/blog/kubernetes-secrets-management-2025) and beyond. By following the steps and best practices outlined above, organizations can enhance the security and manageability of their GitOps workflows, ensuring that sensitive configuration is handled in a safe, auditable, and cloud-native manner.
