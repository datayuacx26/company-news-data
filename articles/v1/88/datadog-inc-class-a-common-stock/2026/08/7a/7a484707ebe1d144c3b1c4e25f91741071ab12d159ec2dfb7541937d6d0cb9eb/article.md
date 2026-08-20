---
schema_version: "1.0.0"
document_id: "7a484707ebe1d144c3b1c4e25f91741071ab12d159ec2dfb7541937d6d0cb9eb"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/how-to-manage-unfixed-kubernetes-cves/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T07:27:16.136025+00:00"
fetched_at: "2026-08-11T07:27:17.832245+00:00"
content_hash: "sha256:a9b4c6b257276a14f8d10768851a5fdb93babb65c793e66759f28c19e0288453"
---

# How to manage risk from unfixed Kubernetes CVEs

Mallory Mooney


Staff Technical Content Writer


On June 1, 2026, the[Kubernetes Security Response Committee updated the records for four older CVEs](https://kubernetes.io/blog/2026/05/26/reconciling-unfixed-kubernetes-cves/) that remain unfixed. The corrections may cause vulnerability scanners to report these CVEs in clusters where they weren’t previously detected. But an affected version doesn’t necessarily mean that a cluster is exposed. Each unfixed Kubernetes CVE depends on a particular combination of permissions, cluster features, and network access. Since fixing the underlying behavior in code would break existing Kubernetes functionality, managing these findings means confirming each cluster’s exposure conditions, applying preventive controls, and monitoring the API activity tied to each CVE.


Kubernetes audit logs can identify who made relevant configuration changes or sent proxy requests, but they can’t confirm successful exploitation in every case. In this post, we’ll look at the conditions that create exposure and build detection rule queries for the following scenarios:


-


Watch for Service traffic redirection for CVE-2020-8554


-


Find unexpected endpoint changes for CVE-2021-25740


-


Monitor webhook and logging changes for CVE-2020-8561


-


Investigate proxy activity for CVE-2020-8562


For each CVE, we’ll also explain what the audit activity can confirm and when you’ll need additional evidence for investigations.


## Determine which clusters are exposed


When a scanner reports one of these unfixed Kubernetes CVEs, the finding confirms only that the cluster runs an affected version. It doesn’t show whether the permissions, configuration, and network access required for exploitation are present. You can use the following table to connect each CVE to its exposure conditions, primary preventive control, and available audit evidence:


CVE Exposure conditions Primary preventive control What audit logs can confirm


[CVE-2020-8554](https://github.com/kubernetes/kubernetes/issues/97076) Untrusted identities can create or modify Services and Pods or patch services/status Disable spec.externalIPs and restrict status writes Service submissions containing spec.externalIPs and patches to Service status


[CVE-2021-25740](https://github.com/kubernetes/kubernetes/issues/103675) Untrusted identities can modify Endpoints or EndpointSlices used by an Ingress or LoadBalancer Restrict endpoint writes and review shared routing Which identity modified an endpoint resource, but not whether traffic crossed a namespace boundary


[CVE-2020-8561](https://github.com/kubernetes/kubernetes/issues/104720) An actor can control webhook responses, the API server can reach the target, kube-apiserver verbosity is 10 (or the attacker can raise it to 10), and the attacker can read the process logs Disable profiling, keep verbosity below 10, and restrict webhook and log access Webhook configuration and verbosity changes, but not the redirected response or data exposure


[CVE-2020-8562](https://github.com/kubernetes/kubernetes/issues/101493) Untrusted identities have the required Node or StorageClass permissions and can take advantage of inconsistent DNS responses Restrict permissions and enforce consistent DNS responses Proxy requests and destinations rejected by address validation, but not successful DNS rebinding


When evaluating these CVEs, you’ll want to prioritize multi-tenant clusters, where users or workloads share infrastructure without the same level of trust. If you’re a provider of managed clusters, these issues also warrant closer attention when the control plane can reach networks that cluster users can’t access directly.


### Confirm that your audit logs support the queries


The queries can only detect activity that your audit policy records. Before building the rules, you should confirm that[the policy covers the relevant resources, subresources, and non-resource URLs](https://kubernetes.io/docs/tasks/debug/debug-cluster/audit/#audit-policy) . Amazon Elastic Kubernetes Service (EKS), Azure Kubernetes Service (AKS), Google Kubernetes Engine (GKE), and other log pipelines may also rename the fields parsed from Kubernetes audit events. You can test each query against an event from your environment to see whether you need to adjust the field names or account for data that your provider doesn’t expose.


Audit events establish who performed an API action and which resource was involved. Responders may also need evidence from network, DNS, or process logs to reconstruct what happened afterward.


## Watch for Service traffic redirection for CVE-2020-8554


CVE-2020-8554 can enable a man-in-the-middle attack through either of two traffic redirection paths. First, an identity that can create or modify Services and Pods can add an address to spec.externalIPs. Kubernetes may then route traffic for that IP to the Pods selected by the Service. Second, an identity with access to the services/status subresource can instead change status.loadBalancer.ingress.ip, which causes affected kube-proxy implementations to treat an arbitrary IP as a load balancer address. You can reduce exposure to CVE-2020-8554 by restricting who can configure Service traffic routes and monitoring successful changes to spec.externalIPs and services/status.


Kubernetes formally deprecated spec.externalIPs in version 1.36, but clusters are still exposed while the behavior is enabled. For example, an authorized user can introduce the field even if no existing Service uses it.[Kubernetes recommends disabling the field with the DenyServiceExternalIPs admission controller](https://kubernetes.io/blog/2026/05/14/kubernetes-v1-36-deprecation-and-removal-of-service-externalips/) and planning a migration to another approach.[Datadog Security Labs’ analysis of CVE-2020-8554](https://securitylabs.datadoghq.com/articles/unpatchable-kubernetes-vulnerabilities-cve-2020-8554/) also provides recommendations for other ways to block ExternalIP Services. These controls address only one of the two paths, so access to services/status should also remain scoped to trusted identities.


For understanding a cluster’s exposure to CVE-2020-8554, you should start by identifying the users and service accounts that can create or modify Services and Pods or patch the services/status path. These permissions tell you whether an untrusted identity could introduce either traffic redirection scenario. You should also find existing Services that use spec.externalIPs so you have a baseline for recognizing new or unexpected configurations. Kubernetes explains how to[check the permissions of a specific identity](https://kubernetes.io/docs/reference/kubectl/generated/kubectl_auth/kubectl_auth_can-i/) and[review access granted through RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) .


### Review both Service traffic redirection paths


Once you know who can make these changes, you can monitor both traffic redirection paths and see which one produced a matching event. The following query, **external_ip_change** , detects submitted Service objects or patches that contain spec.externalIPs:


```text
1   @apiVersion:audit.k8s.io/v1 @objectRef.resource:services -@objectRef.subresource:* (@verb:create OR @verb:update OR @verb:patch) (@requestObject.spec.externalIPs:* OR @requestObject.path:/spec/externalIPs*) @responseStatus.code:[200 TO 299]
```


Because it checks the submitted Service data, your audit policy must record the request body at the Request or RequestResponse level. If the policy records only event metadata, you can see who changed the Service but not whether the request contained spec.externalIPs.


The second query monitors patches to services/status, which is the path used to change status.loadBalancer.ingress.ip. Audit events may not show which status field a patch modified, so the following **load_balancer_status_patch** query matches every successful patch to the subresource:


```text
1   @apiVersion:audit.k8s.io/v1 @objectRef.resource:services @objectRef.subresource:status @verb:patch @responseStatus.code:[200 TO 299]
```


Access is typically limited to trusted controllers, which makes a match from an unexpected identity worth investigating. Since either path can redirect traffic on its own, the rule should trigger when either query matches the` external_ip_change > 0 || load_balancer_status_patch > 0` condition.


Grouping results by @user.username, @objectRef.namespace, and @objectRef.name shows who changed which Service. Including the namespace keeps identically named Services separate, while the identity and Service name help you create suppression lists for only approved controller and resource combinations.


## Find unexpected endpoint changes for CVE-2021-25740


CVE-2021-25740 affects clusters where a user can modify Endpoints or EndpointSlices for a Service without a selector, which creates conditions that enable a confused deputy attack. For example, the user can point that Service toward a backend in another namespace, where a trusted Ingress or LoadBalancer may then reach the backend on the user’s behalf, potentially bypassing controls that trust traffic from that component.


Clusters upgraded from versions earlier than 1.22 need particular attention because Kubernetes removed endpoint write permissions from the default edit and admin roles for clusters created with version 1.22. As a result, upgraded clusters may retain broader permissions until an administrator[audits and reconciles system:aggregate-to-edit](https://kubernetes.io/blog/2026/05/26/reconciling-unfixed-kubernetes-cves/) . For more options for limiting endpoint access and reconsidering shared ingress designs, you can review[Datadog Security Labs’ deep dive on CVE-2021-25740](https://securitylabs.datadoghq.com/articles/unpatchable-kubernetes-vulnerabilities-cve-2021-25740/) .


Services with selectors typically receive their backends from a controller, but Services without selectors rely on manually managed Endpoints or EndpointSlices. To find these potential entry points,[Kubernetes recommends listing Services and their selectors](https://github.com/kubernetes/kubernetes/issues/103675) by using the following command:


```text
1   kubectl get services --all-namespaces \ -o=custom-columns='NAME:metadata.name,NAMESPACE:metadata.namespace,SELECTOR:spec.selector'
```


For a Service without a selector, its exposure path and typical controller determine whether an endpoint change is expected or could redirect traffic across a namespace boundary. To establish that context, you should check whether an Ingress or LoadBalancer exposes the Service and which controller legitimately manages its backends. You should also review who can write the associated endpoint resources and whether the submitted addresses fall within the expected network range.


### Track endpoint writes from unexpected identities


Inventorying your Services gives you the context to interpret endpoint writes. With that information, you can use a detection rule to surface successful writes from identities other than the controllers that typically manage these resources. The following **endpoint_write** query covers both resource types:


```text
1   @apiVersion:audit.k8s.io/v1 (@objectRef.resource:endpoints OR @objectRef.resource:endpointslices) (@verb:create OR @verb:update OR @verb:patch) @responseStatus.code:[200 TO 299]
```


Because a single endpoint write can redirect traffic, the rule should trigger on any match for the` endpoint_write > 0` condition.


A match identifies who modified the endpoint resource. You can then examine the submitted addresses and the Service’s routing configuration to confirm whether the change could redirect traffic across a namespace boundary.


Kubernetes controllers commonly write to Endpoints and EndpointSlices, so an untuned rule may produce frequent detections. Grouping by @user.username, @objectRef.namespace, and @objectRef.name shows which identities typically manage each resource. You can then suppress verified controller usernames without excluding all service accounts and potentially hiding activity from a compromised account.


## Monitor webhook and logging changes for CVE-2020-8561


For CVE-2020-8561, audit logs can surface changes to admission webhooks and API server verbosity, although confirming exploitation requires kube-apiserver process logs. CVE-2020-8561 combines control over an admission webhook with API server network access and verbose logging. For example, a malicious webhook response can redirect the API server toward an internal destination. If kube-apiserver logging verbosity is set to 10, an actor who can read its logs may be able to see the redirected response and headers.


Kubernetes states that[examining kube-apiserver log responses is the only known way to detect successful exploitation](https://github.com/kubernetes/kubernetes/issues/104720) . Audit logs complement kube-apiserver process logs by surfacing webhook configuration changes and successful writes to` /debug/flags/v.` Either event can help responders identify changes to the conditions required for exploitation. However, an attacker may not need to generate them if they already control the webhook server or if the API server is already logging at verbosity 10.


### Review existing admission webhook configurations


An attacker who can modify an existing admission webhook configuration may be able to redirect the API server toward a destination they control. Datadog provides an[out-of-the-box rule for the creation of a new admission controller](https://docs.datadoghq.com/security/default_rules/wpm-g1s-8yx/) , but you can extend that coverage with a custom rule for updates to existing validating and mutating webhook configurations. A match identifies who modified the configuration and which trusted webhook was affected.


The following **webhook_configuration_change** query covers successful updates and patches to both webhook types in the same workflow:


```text
1   @apiVersion:audit.k8s.io/v1 (@objectRef.resource:validatingwebhookconfigurations OR @objectRef.resource:mutatingwebhookconfigurations) (@verb:update OR @verb:patch) @responseStatus.code:[200 TO 299]
```


A single modification can affect a component that the API server trusts, so you can trigger the rule on the first match for the` webhook_configuration_change > 0` condition.


As with the other rules, grouping results by @user.username and @objectRef.name associates each event with its identity and webhook. You can then suppress approved combinations of controller identity and webhook name without overlooking modifications to other webhooks. For example, if a Datadog Cluster Agent reconciles its own datadog-webhook, suppressing that pair still lets the rule detect the same identity modifying another webhook.


### Track API server verbosity


When profiling is enabled, an authorized user can send a PUT request to` /debug/flags/v` to adjust kube-apiserver logging verbosity at runtime. Any successful write alters the API server’s logging behavior, so a separate high-severity rule can surface the activity for immediate investigation.


[Datadog’s analysis of CVE-2020-8561](https://securitylabs.datadoghq.com/articles/unpatchable-kubernetes-vulnerabilities-cve-2020-8561/) explains how disabling profiling and examining the network trust boundaries around the API server can reduce the risk. For managed clusters, the exposure assessment also depends on which settings the provider controls and whether cluster operators can access the necessary logs.


Failed requests can’t change the API server’s verbosity, so the following **verbosity_change** query limits matches to successful PUT requests:


```text
1   @apiVersion:audit.k8s.io/v1 @requestURI:"/debug/flags/v" @verb:put @responseStatus.code:[200 TO 299]
```


A successful write is rare enough to warrant investigation, so you can use` verbosity_change > 0` as the rule’s condition to trigger it immediately.


Grouping events by @user.username and kube_cluster_name ties each request to the identity and cluster involved. The audit event records the successful request to` /debug/flags/v` , but not the submitted verbosity value. This means that the rule can’t tell whether the user increased the level to 10 or reset it to a lower value.


## Investigate proxy activity for CVE-2020-8562


For CVE-2020-8562, you can use audit logs to investigate blocked Node proxy requests, but they can’t confirm successful DNS rebinding. CVE-2020-8562 can allow an authorized but untrusted Kubernetes user to reach private networks accessible to control plane components. One path requires permission to create or modify Nodes and send requests through the API server proxy. The other requires permission to modify StorageClasses and access kube-controller-manager logs.


### Focus the rule on requests blocked by validation


Although there is no known way to confirm successful exploitation, audit logs can show when address validation blocks a proxy request. To avoid generating detections for every use of the proxy endpoint, the following **blocked_restricted_address** rule looks for the[“address not allowed” response](https://github.com/kubernetes/kubernetes/issues/101493) that the API server returns for a rejected destination:


```text
1   @apiVersion:audit.k8s.io/v1 @objectRef.resource:nodes @objectRef.subresource:proxy @responseStatus.code:400 @responseStatus.message:"address not allowed"
```


Each match represents a request to a blocked address, so the rule triggers on the first event using the` blocked_restricted_address > 0` condition.


Grouping events by @user.username and @objectRef.name shows which identity sent the request and which Node it used. An “address not allowed” response confirms that validation blocked the destination, but the API server can return this response for loopback, link-local, and other addresses rejected by the API server’s validation. When investigating whether the event relates to CVE-2020-8562, you should confirm that the identity can modify Nodes and use the nodes/proxy subresource, then inspect the Node’s addresses and recent modifications.


If DNS query logs are available for the resolver used by kube-apiserver, you can compare consecutive responses around the event. You can also look at consecutive DNS responses to verify whether the hostname resolved differently around the time of the request, which would be consistent with the[CVE-2020-8562 attack path](https://github.com/kubernetes/kubernetes/issues/101493) .


[Datadog’s analysis of CVE-2020-8562](https://securitylabs.datadoghq.com/articles/unpatchable-kubernetes-vulnerabilities-cve-2020-8562/) explains how enforcing a minimum DNS cache lifetime can help prevent different answers between the validation and connection lookups. It also describes how[Konnectivity](https://kubernetes.io/docs/concepts/architecture/control-plane-node-communication/#konnectivity-service) can keep proxy traffic from traversing the control plane network.


### Account for the StorageClass path


The Node proxy rule covers only one path associated with[CVE-2020-8562](https://github.com/kubernetes/kubernetes/issues/101493) . An untrusted user may also be able to reach private networks by modifying StorageClasses and reading kube-controller-manager logs. You can assess whether this path matters in your environment by identifying who can create or modify StorageClasses, which provisioners accept network locations, and who can read the controller-manager logs that may contain the response.


## Document your response to unfixed Kubernetes CVEs


Because scanners will continue to report these CVEs, teams need a repeatable way to evaluate and manage the findings. An exposure assessment connects each finding to the cluster conditions, preventive controls, and detection coverage discussed above. Vulnerability management teams can use that evidence to identify both mitigated clusters and those that still require changes.


For each cluster—or group of clusters that shares the same trust model and security controls—you can capture the following information:


-


Whether the cluster meets the required permission, networking, and trust conditions


-


Which RBAC, admission, network, or provider controls prevent exploitation


-


Which audit sources and detection rules cover the remaining activity


-


Which controller identities and resources are approved and suppressed


-


Who reviews the detections and accepts the remaining risk


Your findings should record what each detection establishes and which additional sources responders may need during an investigation. This information is especially useful for managed clusters where the provider controls the API server network, DNS, or process logs.


Documenting those gaps gives vulnerability management teams a clearer picture of the residual risk. Datadog Cloud Security’s[CIS Kubernetes compliance report](https://app.datadoghq.com/security/compliance/home/cis-kubernetes) can also help you verify baseline controls such as Kubernetes audit logging and API server profiling settings across your clusters.


## Manage risk when patching is not an option


Managing these unfixed Kubernetes CVEs requires teams to account for how Kubernetes components route traffic and trust network destinations across workload and control plane boundaries. Preventive controls can remove some of the conditions required for exploitation, while Kubernetes audit logs help teams monitor the related API activity. Because audit logs can’t confirm successful exploitation in every case, responders may also need network, DNS, or process evidence to continue their investigation. Datadog Cloud SIEM can help[you monitor unexpected changes and blocked requests](https://docs.datadoghq.com/security/cloud_siem/) across your clusters.


The Datadog Security Labs series provides more detail on the attack mechanics and mitigations for[CVE-2020-8554](https://securitylabs.datadoghq.com/articles/unpatchable-kubernetes-vulnerabilities-cve-2020-8554/) ,[CVE-2021-25740](https://securitylabs.datadoghq.com/articles/unpatchable-kubernetes-vulnerabilities-cve-2021-25740/) ,[CVE-2020-8561](https://securitylabs.datadoghq.com/articles/unpatchable-kubernetes-vulnerabilities-cve-2020-8561/) , and[CVE-2020-8562](https://securitylabs.datadoghq.com/articles/unpatchable-kubernetes-vulnerabilities-cve-2020-8562/) . You can also review our guide on[key Kubernetes audit logs for monitoring cluster security](https://www.datadoghq.com/blog/key-kubernetes-audit-logs-for-monitoring-cluster-security/) .


If you don’t already have a Datadog account, you cansign up for a free 14-day trial.


-
-
-
