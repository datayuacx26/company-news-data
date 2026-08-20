---
schema_version: "1.0.0"
document_id: "d161f853d52ba8cad397d4ece7ce24798f001a2747af6ff1f829b0989242a6fa"
company_key: "uipath-inc-class-a-common-stock"
company: "UiPath Inc."
source_id: "uipath-inc-class-a-common-stock-rss-2f83a748bf9d"
canonical_url: "https://engineering.uipath.com/temporal-multi-cluster-replication-f8fc1c6da230"
published_at: "2026-02-10T04:22:56+00:00"
first_seen_at: "2026-07-20T23:16:59.255384+00:00"
fetched_at: "2026-07-28T22:21:08.932956+00:00"
content_hash: "sha256:63b3114ec8d0ce45715a2f09cb991de99e613a214168e76e5a80cea063c3d13e"
---

# Temporal Multi-Cluster Replication

# Temporal Multi-Cluster Replication


## Part 1: Cluster Connection with mTLS, Kubernetes, and Azure


[Travis Mcchesney](https://medium.com/@travis.mcchesney?source=post_page---byline--f8fc1c6da230---------------------------------------)


6 min read


·


Feb 10, 2026


--


## Overview


This two-part blog post demonstrates how we at UiPath have set up our Temporal service for[multi-cluster replication](https://docs.temporal.io/temporal-service/multi-cluster-replication) , leveraging[mTLS](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/) communication between the clusters.


Part one will focus on ‌Temporal service[cluster communication using mTLS](https://docs.temporal.io/temporal-service/configuration#mtls-encryption) . We will describe our strategy to share certificate authority (CA) certificates between the clusters with a simple, automated solution that involves Kubernetes, cert-manager, and Azure Key Vaults.


## Who Might Care


If you’re setting up a Temporal service and want to enable multi-cluster replication over the open internet, this post can help you get started with secure communication and certificate management.


By the end, you’ll have a strategy for creating certificates in a manageable way so that two Temporal service clusters can communicate securely.


## Architecture


The architecture of our Temporal clusters looks something like the diagram below. Each cluster lives in its own region, and they communicate with each other over the public internet.


Each cluster is standalone, hosting its own persistence (Cassandra) and visibility layers (ElasticSearch).


This is the recommended approach for enabling high availability for a Temporal service. The replication happens at the application layer, rather than relying on the persistence store’s ability to back up or replicate on its own.


Press enter or click to view image in full size


## Connection and Encryption


As you can see in the architecture diagram in the previous section, ‌communication between these two clusters happens over the open internet.


An alternative to using the public internet would be to create a private link between the two clusters, in which case mTLS may not be necessary. In our case, this wasn’t an option due to our virtual network setup. So, in order to secure ‌communication between clusters, we required mTLS.


## Certificates


In order to enable mTLS (mutual TLS) communication, each cluster must have a certificate to present to the other cluster, and both clusters must trust the other’s certificate.


While TLS trust is generally uni-directional (client trusting the server), and based upon well-known certificate signers (DigiCert, Let’s Encrypt, etc.), mTLS trust is bi-directional (client and server trusting each other).


In order for this bi-directional (mutual) trust to be established, both the client and the server need to trust each other’s CA. To accomplish this, the CA certificate from one cluster is provided to the other cluster.


This type of configuration‌ makes self-signed certificates a viable option, even with communication happening over the open internet. And, in fact, Temporal[requires](https://docs.temporal.io/cloud/certificates#ca-certificates) a not-well-known CA, unless an additional certificate filter is included in the configuration.


## Kubernetes


All of these certificates can seem daunting to create, store, share, etc., but we’ve implemented a strategy that makes the process relatively seamless and easy to maintain.


One of the reasons we needed to deploy this sort of process in the first place is that we have several different Temporal services, each with their own peer clusters for replication. We want to ensure that new clusters are fast and easy to spin up, with little manual intervention.


This diagram shows the components involved in the certificate management process.


Press enter or click to view image in full size


To make it all happen, we use:


- cert-manager: certificate management for Kubernetes
- External Secrets: external secret management for Kubernetes
- Azure Key Vaults: cloud secret storage


The main idea is that we:


1. (On the primary cluster) generate a self-signed CA certificate and key
2. Generate a server certificate based on that CA
3. Push the CA cert and key to an Azure Key Vault
4. On the secondary cluster, pull in the CA certificate and key from the Azure Key Vault that was created by the primary cluster
5. Generate a server certificate based on the primary cluster’s CA certificate


> Note that it’s not necessary to use the same CA in both clusters, it’s just a little more convenient to only have to store one per cluster pair.


### Primary Cluster


***cert-manager***


## Get Travis Mcchesney’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


For certificate creation, we first rely on` Issuer` and` Certificate` objects from the` cert-manager` operator.


On the **primary cluster** , we have the following` cert-manager` objects. This cluster does the heavy lifting of creating a CA` Certificate` using a self-signed` Issuer` , then creates its own server` Certificate` using that CA.


*Self-signed Issuer:* Used for self-signing a CA certificate.


```text
apiVersion: cert-manager.io/v1  kind: Issuer  metadata:    name: temporal-selfsigned-issuer  spec:    selfSigned: {}
```


*CA Issuer:* Used for issuing a server certificate based on the self-signed CA.


```text
apiVersion: cert-manager.io/v1  kind: Issuer  metadata:    name: temporal-ca-issuer  spec:    ca:      secretName: temporal-ca-cert
```


*Certificate (CA):* Uses the self-signed issuer for creation. Note the long duration, which allows the CA to be trusted and shared for a longer timeframe. Rotating the CA must be done around the same time on each cluster and can cause downtime in the replication process, so ‌rotation activity is kept to a minimum.


```text
apiVersion: cert-manager.io/v1  kind: Certificate  metadata:    name: temporal-ca-cert  spec:    isCA: true    duration: 87600h # 10 years    commonName: "*.example.com"    secretName: temporal-ca-cert    privateKey:      algorithm: ECDSA      size: 384    issuerRef:      name: temporal-selfsigned-issuer      kind: Issuer      group: cert-manager.io
```


*Certificate (server):* Uses the CA issuer for creation


```text
apiVersion: cert-manager.io/v1  kind: Certificate  metadata:    name: temporal-cert  spec:    secretName: temporal-cert    issuerRef:      name: temporal-ca-issuer      kind: Issuer    dnsNames:      - "temporal-1.example.com"
```


External secrets


We then use a` PushSecret` to push the CA certificate and key over to the secondary cluster’s Key Vault, represented by the` SecretStore` .


*SecretStore:* Sets up the connection to the secondary cluster’s Azure Key Vault


```text
apiVersion: external-secrets.io/v1beta1  kind: SecretStore  metadata:    name: "temporal-sec-secretstore"  spec:    provider:      azurekv:        authType: WorkloadIdentity        vaultUrl: "https://myseckeyvault.vault.azure.net"        serviceAccountRef:          name: my-sa
```


*PushSecret:* Pushes the self-signed CA and key to the Azure Key Vault


```text
apiVersion: external-secrets.io/v1alpha1  kind: PushSecret  metadata:    name: temporal-push-secret  spec:    updatePolicy: Replace # Policy to overwrite existing secrets in the provider on sync    deletionPolicy: None # the provider' secret will be deleted if the PushSecret is deleted    refreshInterval: 1h # Refresh interval for which push secret will reconcile    secretStoreRefs: # A list of secret stores to push secrets to      - name: temporal-sec-secretstore        kind: SecretStore    selector:      secret:        name: temporal-ca-cert # Source Kubernetes secret to be pushed    data:      - conversionStrategy: None        match:          secretKey: ca.crt          remoteRef:            remoteKey: ca-crt # Remote reference (where the secret is going to be pushed)      - conversionStrategy: None        match:          secretKey: tls.key          remoteRef:            remoteKey: ca-key # Remote reference (where the secret is going to be pushed)
```


### Secondary Cluster


External secrets


On the **secondary cluster, a** n` ExternalSecret` is used to pull the CA certificate and key generated by the primary cluster from its Azure Key Vault, represented by the` SecretStore` . This CA is what’s used to generate the server certificate.


*SecretStore:* Sets up the connection to this cluster’s Azure Key Vault


```text
apiVersion: external-secrets.io/v1beta1  kind: SecretStore  metadata:    name: "temporal-secretstore"  spec:    provider:      azurekv:        authType: WorkloadIdentity        vaultUrl: "https://mykeyvault.vault.azure.net"        serviceAccountRef:          name: my-sa
```


*ExternalSecret:* Imports the CA cert and key from the Azure Key Vault to a Kubernetes` secret`


```text
apiVersion: external-secrets.io/v1beta1  kind: ExternalSecret  metadata:    name: temporal-ca-cert  spec:    refreshInterval: '0'    secretStoreRef:      name: temporal-secretstore      kind: SecretStore    target:      name: temporal-ca-cert      creationPolicy: Owner    data:    - secretKey: tls.crt      remoteRef:        key: ca-crt    - secretKey: tls.key      remoteRef:        key: ca-key
```


***cert-manager***


We then have the following` cert-manager` objects. You’ll notice that on this cluster, we’re using an` ExternalSecret` to pull in the CA cert and key that was generated by the primary cluster, then using a CA` Issuer` to generate the server` Certificate` :


*CA Issuer:* Used for issuing a server certificate based on the CA from the Azure Key Vault


```text
```


*Certificate (server):* Uses the CA issuer for creation


```text
apiVersion: cert-manager.io/v1  kind: Certificate  metadata:    name: temporal-cert  spec:    secretName: temporal-cert    issuerRef:      name: temporal-ca-issuer      kind: Issuer    dnsNames:      - "temporal-2.example.com"
```


With these objects in place, it is now possible to connect two Temporal service clusters together using mTLS communication, and set them up for multi-cluster replication.


In part 2, we will show the configuration that is necessary on the Temporal server to enable this mTLS communication and the replication itself.
