---
schema_version: "1.0.0"
document_id: "575b5025466a2c8672b1ec99225ae4536c935b861a313ecb744bd9a3709b1af3"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/kubectl-remote-clusters/"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-22T15:57:37.609410+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:fc249e59bddb780cb24abdfdb92f5d9db0b51034469a759b20cf2b95ac305de4"
---

# Securing kubectl on Remote Kubernetes Clusters Without Static Credentials or VPNs

Fleets of robots, drones, EV chargers, and sensors now run[K3s](https://k3s.io/) or[MicroK8s](https://microk8s.io/) on the device itself, bringing container orchestration to hardware deployed in warehouses, cell towers, and customer sites around the world. Engineers need[kubectl](https://goteleport.com/blog/what-is-kubectl/) to debug and manage containerized workloads on those devices in the field. But because each cluster requires a kubeconfig file, and every kubeconfig file is a shared static credential, risk grows with each new device added to the fleet.


**Read this guide to learn:**


- Why kubectl and kubeconfig break down when devices are behind NAT and off-network
- How kubeconfig sprawl and static credentials create fleet-wide risk
- Best practices for securing Kubernetes clusters across distributed clusters
- How to eliminate static credentials across entire edge fleets


## Why securing Kubernetes clusters on remote devices is challenging


Cloud-hosted clusters sit inside networks the engineering team controls, with stable IP addresses and reachable API servers. Clusters deployed at the edge or in the field fit inside networks controlled by someone else, behind firewalls and NAT layers that block inbound connections by default.


### Why common workarounds for API server reachability fail at fleet scale


Remote devices (such as field or customer deployed robots, drones, or sensors) running K3s or MicroK8s may sit behind[Network Address Translation (NAT)](https://www.rfc-editor.org/rfc/rfc2663) ,[Carrier-Grade NAT (CGNAT)](https://www.rfc-editor.org/rfc/rfc6888) , or other uncontrolled firewalls configured to block all inbound connections. This means that engineers who need to reach[Kubernetes API servers](https://goteleport.com/blog/kubernetes-api-access-security/) on those devices cannot initiate a connection from outside the network. Instead, another party, like a customer, controls that network perimeter and is unlikely to open inbound ports for outside connections.


A common (but rather risky) workaround to blocked inbound connections is to expose the Kubernetes API server to the public internet, which enables engineers to reach the cluster from any location. However, this also means attackers can also easily reach the exposed cluster, and as more and more devices are added to the fleet, any exposed API server creates massive risk. For example, a fleet of five hundred devices with five hundred publicly reachable API servers presents five hundred entry points into production infrastructure.


VPNs are another common workaround. However, at the scale of modern device fleets, VPNs can introduce massive operational overhead. Each VPN tunnel must be provisioned, maintained, and monitored per device or per site. And because those connections create a bidirectional pathway, any organizations that host devices on their own networks will almost always reject persistent VPN connections from external parties.


### Kubeconfig sprawl across hundreds of clusters


Every K3s or MicroK8s cluster generates its own kubeconfig file containing embedded long-lived credentials. But as the size of a fleet scales, the number of kubeconfig files required grows, too, and can result in engineers accumulating dozens or even hundreds of kubeconfigs. Even for fleets with a few dozen clusters, distributing and revoking these files by hand (and even with specialized kube utilities) becomes a full-time job and is continually complicated each time a new team member joins, leaves, or changes roles.


However, it's the credentials embedded within these kubeconfigs that present the biggest challenge. Whether these credentials are service account tokens or client certificates, they often persist until they are manually revoked, if ever. This means that, like a leaked key, a leaked kubeconfig effectively grants an attacker persistent access to the cluster and the workloads that run on the device. Rotating or revoking credentials across an entire fleet requires touching every kubeconfig on every engineer's machine and every automation pipeline that connects to those clusters.


For infrastructure engineers, managing kubeconfig distribution and credential rotation across hundreds of clusters can cause once-routine tasks, such as onboarding a new team member, to require hundreds of configuration changes across the fleet.


### Fragmented access control and auditing


Native Kubernetes[Role-Based Access Control (RBAC)](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) is configured per cluster, which means policy enforcement varies from device to device as the fleet grows. Keeping policies consistent across hundreds of clusters requires either a dedicated configuration management pipeline or accepting that policies will drift over time. Teams managing remote device fleets of any size simply do not have the bandwidth to maintain either option.


Audit logs present a similar problem. Because each Kubernetes cluster stores its own audit log locally, querying "who accessed which device and when" requires aggregating logs from every cluster in the fleet. To do this, engineers are responsible for collecting, normalizing, and then correlating logs from hundreds of devices and clusters, many of which may have been intermittently connected or outright unreachable.


Compliance standards like[SOC 2](https://goteleport.com/use-cases/soc2-compliance/) ,[ISO 27001](https://goteleport.com/use-cases/iso-27001-compliance/) , and[FedRAMP](https://goteleport.com/use-cases/fedramp-compliance/) require evidence that access controls are applied consistently and proof that audit records are tamper-resistant and complete. As a result, auditors will expect a single, consolidated record of every session event, and per-cluster logs spread across a fleet do not meet that expectation without additional tooling to unify them. Assembling this evidence from hundreds of individual clusters is not only labor-intensive, but is highly prone to human error.


## Best practices for eliminating static credentials on remote and edge clusters


These challenges (API/network exposure, static credential sprawl, and fragmented auditing) can all be attributed to the same root cause. Put simply, Kubernetes was designed for cloud environments where three variables are consistently true:


1. The network is friendly
2. The control plane is reachable
3. A single team manages both the cluster and the network


But remote Kubernetes, K3s, and MicroK8s deployments shatter each of these assumptions. As a result, accepted best practices for secure, consistent, and auditable kubectl sessions on remote Kubernetes clusters have adapted to the constraints of hostile networks, distributed clusters, and limited hardware capacity, in addition to the operational costs required to maintain Kubernetes fleets.


### Outbound reverse tunnels to keep the API server private


The networks that remote devices operate within will not always (and likely, may never) be friendly. This is why outbound reverse tunnels offer a more efficient way to access remote clusters.


Instead of requiring the Kubernetes API server to be exposed publicly, outbound connections are initiated to a proxy sitting in a network the engineering team controls. The proxy accepts and maintains that inbound connection from the device so engineers connect to the proxy without requiring exposed ports or modifications to uncontrolled firewalls.


Reverse tunnels work by inverting the connection direction. A lightweight agent on the edge device opens an outbound TCP connection to the proxy, and that connection stays open as a tunnel through which the proxy can forward kubectl traffic back to the device. And because this connection is outbound from the device, the tunnel survives NAT, CGNAT, and firewall restrictions that would normally block any inbound connection attempt.


Reverse tunnels are also the best option for establishing connections that can tolerate the network instability expected in edge environments. For example, a device roaming between cellular and satellite connectivity, or experiencing intermittent links in a remote location, is able to re-establish the tunnel when the connection recovers without any manual intervention from the engineering team. This keeps the Kubernetes API private while the engineering team connects through the proxy, and without requiring exposure from the network.


*Learn more about reverse tunnels and SSH tunneling concepts:[SSH Tunneling Explained](https://goteleport.com/blog/ssh-tunneling-explained/)*


### Cryptographic identity and short-lived privileges instead of static kubeconfigs


Like any long-lived credential, static kubeconfigs expose a significant attack surface. At the scale of modern edge fleets, this attack surface only becomes more and more dangerous.


The best alternative to static kubeconfigs is to replace them with short-lived privileges that are tied to an authenticated identity through Single Sign-On (SSO). Instead of using a static kubeconfig file, an engineer authenticates through their identity provider, such as Okta or Microsoft Entra ID, and receives a certificate that grants short-lived privileges to a specific cluster. The privileges expire when the defined maintenance window closes, leaving no static credentials on the engineer's workstation or on the device.


This also eliminates the overhead (and human error risk) that credential lifecycle management creates. For example, if a team member leaves the organization, their identity provider account is disabled, and all future certificate issuance stops. The team does not need to hunt down and revoke kubeconfig files distributed across workstations and CI/CD pipelines, because no static credentials exist to revoke.


### Unifying SSH and kubectl through a single gateway


Edge devices running K3s or MicroK8s typically require at least two forms of access. Engineers need both SSH to debug the host operating system and kubectl to manage the Kubernetes workloads running on the device.


However, running separate access paths for each protocol doubles the management surface, requiring two sets of static credentials, two sets of RBAC policies, and two distinct audit trails for what is physically a single device. A single gateway that multiplexes both SSH and Kubernetes traffic through one reverse tunnel can reduce the number of open connections, consolidate the access path, and simplify network configuration on each device. If the agent on each device handles both protocols through the same outbound connection, the proxy then routes traffic to the correct service based on the protocol. Engineers authenticate once through the same flow for both SSH and kubectl.


Maintaining two separate audit trails for the same device (and split across two access paths) ultimately creates audit blind spots. A unified gateway produces a single audit trail for both SSH sessions and kubectl commands for each device in the fleet.


### Fleet-wide audit logging and session recording


Capturing each kubectl exec, SSH session, and API call at the proxy level produces fleet-wide visibility without relying on per-cluster audit log aggregation. With this approach, the proxy sits between the engineer and every device in the fleet, which means every session event passes through a single point where logging and recording happens before traffic reaches the device.


Recording Kubernetes exec sessions also provides a "flight recorder" capability for incident investigation and liability protection. For example, if a containerized workload fails after an engineer executes commands during a maintenance session, a recorded session shows exactly which commands were run and in what order. For organizations deploying hardware that interacts with the physical world, such as autonomous robots or industrial controllers, this ability to prove what happened during a session has direct legal and contractual value. Compliance requirements become much simpler to maintain when every session event appears in a single fleet-wide audit record, regardless of which device or cluster it targets.


## How Teleport eliminates static credentials and secures remote Kubernetes clusters


Teleport implements each of these best practices in a single platform that handles identity, authorization, and audit for Kubernetes clusters and infrastructure deployed across devices in the field, at the edge, or in customer environments.


### Unified SSH and Kubernetes gateway through reverse tunnels


Teleport agents on edge devices running K3s or MicroK8s initiate outbound connections to the[Teleport proxy](https://goteleport.com/docs/reference/architecture/proxy/) , keeping the Kubernetes API private and requiring no inbound ports on the customer network. The agent handles both SSH and Kubernetes traffic through the same reverse tunnel, and engineers connect to the proxy to authenticate to either service on any device in the fleet. The Kubernetes API server on the device never needs to be exposed to the public internet.


Engineers use kubectl through the same secure reverse tunnel used for SSH, with no separate connection path and no kubeconfig distribution required.[Enrolling a Kubernetes cluster](https://goteleport.com/docs/enroll-resources/kubernetes-access/getting-started/) in Teleport registers the cluster with the proxy, and engineers authenticate to Teleport and select the target cluster by name or label rather than managing per-cluster kubeconfig files. The proxy forwards kubectl traffic through the reverse tunnel to the agent on the device, and the agent forwards the request to the Kubernetes API server.


*"With Teleport, we were able to close the Kubernetes API without losing access for our engineers or automation tools. That alone was a huge win."* – Olga Daminova, Infrastructure Security Engineer, Rush Street Interactive.[Read the full case study.](https://goteleport.com/case-study/rush-street-interactive/)


### Cryptographic identity with short-lived privileges


Teleport eliminates static kubeconfigs by issuing[short-lived certificates](https://goteleport.com/how-it-works/) that grant short-lived privileges after engineers authenticate through SSO providers like Okta or Microsoft Entra ID. The certificate is generated at authentication time, restricted to the clusters the engineer is authorized to reach, and set to expire when the access window closes. This means that engineers do not need to manually manage kubeconfig files because the Teleport client handles certificate issuance and renewal during the session.


Privileges expire when the certificate expires which ensures no static credentials remain on the device or on the engineer's workstation after the session ends. When a team member leaves the organization, disabling their SSO account prevents all future certificate issuance without requiring any changes to the devices in the fleet. This also means that teams do not need to rotate credentials, revoke kubeconfigs, or audit which engineers still have standing privileges on which clusters.


### Kernel-level session recording and tamper-resistant audit logs


Traditional session recording captures only the terminal output that the user sees, which means that encoded commands, shell scripts, and suppressed terminal echo can obscure what actually happened during a session.


Teleport’s[Enhanced Session Recording](https://goteleport.com/docs/enroll-resources/server-access/guides/bpf-session-recording/) captures every interactive SSH and Kubernetes exec session at the kernel level using[Berkeley Packet Filter (BPF)](https://goteleport.com/blog/enhanced-session-recording/) , capturing the full command history attributed to the authenticated identity. BPF-based recording captures system calls at the kernel level, including program execution, network connections, and file access, regardless of how the user invoked those actions.


For example, network connections are logged as` session.network` events, similar to the following:


```text
{
"action"  :  0  ,
"cgroup_id"  :  2360  ,
"cluster_name"  :  "purple"  ,
"code"  :  "T4002I"  ,
"dst_addr"  :  "5.9.243.187"  ,
"dst_port"  :  443  ,
"ei"  :  51  ,
"event"  :  "session.network"  ,
"login"  :  "ec2-user"  ,
"namespace"  :  "default"  ,
"operation"  :  0  ,
"pid"  :  5007  ,
"program"  :  "curl"  ,
"server_hostname"  :  "ip-172-31-34-128.us-east-2.compute.internal"  ,
"server_id"  :  "531dc1de-c2c9-49bd-a0e3-4f4f3a523f5f"  ,
"sid"  :  "86aca627-971d-4883-854d-d309ba04c658"  ,
"src_addr"  :  "172.31.34.128"  ,
"time"  :  "2023-07-04T16:42:20.55Z"  ,
"uid"  :  "da151350-bf45-4a04-a62b-7a4fc805e744"  ,
"user"  :  "alice @goteleport  .com"  ,
"version"  :  4
}


```


Every recorded session is attributed to a specific human or non-human identity through the certificate chain established at authentication. The certificate attributes the session to an SSO identity to ensure the[audit record](https://goteleport.com/docs/reference/deployment/monitoring/audit/) answers "who ran what, on which device, and when" with an authenticated identity rather than a shared username or a generic service account. For organizations deploying autonomous hardware that interacts with the physical world, that attribution provides the difference between proving what happened and guessing.


### Cluster and server discovery, enrollment, and label-based management at fleet scale


Teleport can auto-discover and enroll[Kubernetes clusters](https://goteleport.com/docs/enroll-resources/auto-discovery/kubernetes/) (EKS, AKS) and[Linux servers](https://goteleport.com/docs/enroll-resources/auto-discovery/servers/) to reduce installation and management overhead. The discovery service scans for clusters or servers matching a set of filtering labels and registers them with the Teleport proxy.


Labels attached to Teleport agents allow RBAC policies and discovery rules to target clusters by metadata such as environment, region, customer, or device type rather than by individual cluster name or IP address.[Labels can be set](https://goteleport.com/docs/enroll-resources/kubernetes-access/manage-access/) through the Helm chart values file, and Teleport can then reference those labels to enforce privileges for groups of clusters rather than individual ones. At fleet scale with hundreds of enrolled clusters or servers, managing RBAC policies and enrollment through labels (rather than individual cluster names or IP addresses) reduces upgrade timelines and the need for customer-coordinated planned outages.


## Conclusion


Kubernetes clusters deployed on edge devices will continue to multiply as more organizations push containerized workloads onto robots, drones, EV chargers, and sensors in the field. Every cluster added to the fleet where engineers authenticate with static credentials increases identity risk, and retrofitting identity into a fleet of hundreds or thousands of devices after deployment is far more expensive than building it in from the start.


Teleport establishes a unified identity layer across every K3s and MicroK8s cluster in the fleet, issuing short-lived privileges at authentication time based on role and attributing every SSH and kubectl command to the cryptographic identity behind the session. As fleets grow from dozens of devices to thousands, enrollment, policy enforcement, and audit collection all scale at the proxy rather than requiring configuration changes on each individual cluster or device.


#### Secure remote fleets at scale with unified identity


Learn how Teleport establishes cryptographic identity and access control for every robot, device, and resource to[secure autonomous fleet infrastructure at scale](https://goteleport.com/use-cases/autonomous-fleet/) with:


- Outbound-only NAT traversal
- Short-lived, identity-based certificates
- Kernel-level black box recordings for every session
- And more


---


## Steven Martin


Steven Martin is an experienced Solution Engineer at Teleport with over 20 years of enterprise IT experience. He has led multiple large enterprise application implementations at corporate and government sites using on-prem and cloud resources.
