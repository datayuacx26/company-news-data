---
schema_version: "1.0.0"
document_id: "9b36d2d55e10d677d09679193e329bcf01554b6aa1bf370d539d0222b9774634"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/how-to-manage-secrets-in-kubernetes"
published_at: "2023-02-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:027e13f9b8e1da838714672cc93cc51c671c7af4f4855bd8a96bf452a2887c7c"
---

# A better way to manage Kubernetes secrets

**Disclaimer** : We've published a newer version of this article over at[Kubernetes Secrets Management in 2025 - A Complete Guide (with Best Practices)](https://infisical.com/blog/kubernetes-secrets-management-2025) . Check it out instead!


---


Secrets play a critical role in every modern software development lifecycle. Whether you are developing on your local machine, running workflows in CI/CD, or ready to push your code to production, just about all steps need to[consume API keys](https://infisical.com/blog/api-key-management) , certificates, and other configurations to operate. This article will go over how we can put our secrets management on autopilot in Kubernetes in under 10 minutes.


### What Kubernetes supports out of the box


By default, Kubernetes comes with a resource type called Secret. With it, you can store key-value pairs of your sensitive data under a specific namespace within your cluster. Once you have created a Kubernetes secret, you can feed these secrets into your containers as either environment variables or files through Kubernetes volume.


#### Feeding Kubernetes secrets as environment variables:


```text
apiVersion: apps/v1
kind: Deployment
...
spec:
containers:
- name: nginx
image: nginx:1.14.2
envFrom:
- secretRef:
name: kubenetes-secret-name # ← here
ports:
- containerPort: 80


```


This will inject each secret from the Kubernetes secret as an environment variable accessible by the application in the associated container.


#### Mount Kubernetes secrets as a volume:


```text
apiVersion: apps/v1
kind: Deployment
...
spec:
containers:
- name: nginx
image: nginx:1.14.2
volumeMounts:
- name: secrets-volume-name
mountPath: /etc/secrets
readOnly: true
ports:
- containerPort: 80
volumes:
- name: secrets-volume-name
secret:
secretName: kubenetes-secret-name # ← here


```


We can see that Kubernetes out of the box provides a convenient solution for file based secrets by allowing us to assign a single secret and easily distribute its contents to our containers. This eliminates much of the complexity associated with managing secrets in the form of files.


### Is the default secrets solution from Kubernetes enough?


Great, so now our application can consume secrets from Kubernetes secrets. Done, right? Well, not so fast. If you created your Kubernetes secret via a YAML file, what do you do with it? You can’t just push it as is to GitHub since that would expose them to the public. What about when you need to give other team members access to it? Do you just share the YAML file via Slack or email? That doesn’t sound too secure.


One option to solve this issue is to encrypt these YAML files with open-source tools such as Bitnami Sealed Secrets or Mozilla Sops, then upload the encrypted files to your version control. However, these forms of encryption typically rely on configuring a key management system which can become operationally heavy to set up and manage across your team, especially in large and complex environments. This is further exacerbated when you think of moving to a different cluster or changing cloud providers.


### Is there a better way to manage secrets in Kubernetes?


There are several options on the market to help manage secrets centrally in Kubernetes. One popular option is Hashicorp Vault. HashiCorp Vault has a feature called Vault Agent Sidecar Injector which modifies pod specifications during deployment to include containers with Vault Agent. This is done using Vault Agent Templates, which store secrets in a shared memory volume. The injector works as a Kubernetes Mutation Webhook Controller, intercepting pod events and making changes if required annotations are present. Read more about HashiCorp Vault integration. with Kubernetes here.


Using this method requires a deep understanding of both Kubernetes and HashiCorp Vault and may also require an understanding of how the agent injects secrets as a volume.


Simpler[secrets management](https://infisical.com/blog/secrets-management-complete-guide) solutions exist but are typically not open source nor end-to-end encrypted, putting their security at question. One solution comparable to Hashicorp vault is Infisical secrets manager. Infisical drastically eases secrets management within Kubernetes when compared to Hashicorp vault but is also open source and end-to-end encrypted.


### Using Infisical to manage Kubernetes secrets


The Secrets Operator will fetch secrets from Infisical every 1 minute. The fetched secrets will be stored in a Kubernetes secret. When a secret value changes, the associated deployment will auto-reload to consume the most up-to-date version.


Infisical interacts with your Kubernetes cluster via a custom Kubernetes operator. If you are unfamiliar with Kubernetes operators, it is software that extends Kubernetes and uses custom resources to manage applications and their components.


Once the[Infisical Secrets Operator is installed in your cluster](https://infisical.com/docs/integrations/platforms/kubernetes) , you’ll have access to a new resource type called InfisicalSecret. This is similar to resource types you are already familiar with, such as Deployment, Ingress, Service, etc except it is only available after installing the operator. Each InfisicalSecret resource instance you create, allows you to define how you’ll authenticate with Infisical to retrieve secrets and where to store those secrets.


#### Example InfisicalSecret resource:


```text
apiVersion: secrets.infisical.com/v1alpha1
kind: InfisicalSecret
metadata:
# Name of of this InfisicalSecret resource
name: infisicalsecret-sample
spec:
# The host that should be used to pull secrets from. The default value is https://app.infisical.com/api.
hostAPI: https://app.infisical.com/api


# The Kubernetes secret the stores the Infisical token
tokenSecretReference:
# Kubernetes secret name
secretName: service-token-a
# The secret namespace
secretNamespace: prod


# The Kubernetes secret that Infisical Operator will create and populate with secrets from the above project
managedSecretReference:
# The name of managed Kubernetes secret that should be created
secretName: managed-secret-1
# The namespace the managed secret should be installed in
secretNamespace: prod


```


Once the resource is created, the operator will start to fetch secrets every 1 minute and save them to the native Kubernetes secret defined in the **InfisicalSecret** resource. In the above example, secrets will be fetched from Infisical using the service token stored in the Kubenetes secret named service-token-a and saved to a Kubernetes secret named managed-secret-1 in the prod namespace.


Because the managed secret is just a native Kubernetes secret, you can configure your deployment to consume the managed secret just like you would with any other Kubernetes secret.


### Encrypt Kubernetes secrets at rest


By default, native Kubernetes secrets are stored as base64-encoded strings without encryption and can be accessed by anyone with API access or access to the etcd database. Since the Infisical Operator uses native Kubernetes as its secure store, we recommend enabling encryption at rest for all secrets within the cluster. For step-by-step instructions, please refer to the following link.


### Auto reload deployment when secrets are modified


One of the common pain points with managing native secrets in Kubernetes is that deployments do not automatically reload when the consumed secret changes. This means that after making changes to a Kubernetes secret, you will have to trigger a manual reload on the deployment so that it can consume the latest secret update. If you do not want to manully trigger a reload, you can also utilize[Reloader](https://github.com/stakater/Reloader) . However, this requires adding more software in your cluster to make it work.


Infisical addresses this problem by allowing users to enable auto-reload at the deployment level so that any modifications to the managed secret will cause a reload of the deployment. To enable auto-reload, add the single annotation to your deployment.


#### Auto reload deployment example


```text
apiVersion: apps/v1
kind: Deployment
metadata:
name: nginx-deployment
labels:
app: nginx
annotations:
secrets.infisical.com/auto-reload: "true" # < redeployment annotation
spec:
replicas: 1
selector:
...


```


### Summary


Managing secrets in Kubernetes can be difficult, but by centralizing secrets with Infisical and utilizing the provided Kubernetes operator, we can now modify secrets entirely from outside of the cluster while knowing that all updates will propagate to our deployments. Learn more about secrets automation in Kubernetes here.
