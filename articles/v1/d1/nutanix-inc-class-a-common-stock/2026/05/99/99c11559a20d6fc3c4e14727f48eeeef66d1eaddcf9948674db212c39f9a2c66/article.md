---
schema_version: "1.0.0"
document_id: "99c11559a20d6fc3c4e14727f48eeeef66d1eaddcf9948674db212c39f9a2c66"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-rss-12a2d78c04c7"
canonical_url: "https://www.nutanix.dev/2026/05/20/a-platform-engineers-guide-to-federated-networking-with-nutanix-flow-cni/"
published_at: "2026-05-20T14:00:00+00:00"
first_seen_at: "2026-07-20T03:31:13.386524+00:00"
fetched_at: "2026-08-20T00:46:45.571645+00:00"
content_hash: "sha256:a39e9ac982f54d6568eb8230c54232d05d44c97dda79ece460257cce01cbdcbf"
---

# A Platform Engineer’s Guide to Federated Networking with Nutanix Flow CNI

Networking in hybrid cloud and distributed systems has evolved beyond traditional overlays and VLANs. It now demands unified, scalable, and software-centric solutions that seamlessly integrate VMs and cloud-native workloads. Enter[Flow CNI](https://www.nutanix.com/tech-center/blog/introducing-flow-cni) with Nutanix Flow Virtual Networking. In this blog, we’ll explore what is Flow CNI, why it’s crucial in a federated environment, and how it enables consistent connectivity between **VMs and Kubernetes pods** within federated VPCs.


## What Is Nutanix Flow Virtual Networking?


At its core,[Flow Virtual Networking](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-v7_0_0:ear-flow-nw-overview-pc.html) is a software-defined networking (SDN) layer built into the Nutanix platform that enables virtual private cloud (VPC) constructs, multi-tenant isolation, and self-service network provisioning all managed centrally via the **Prism Central** management console. It abstracts the underlying physical network, allowing administrators to define overlay networks, subnets, routing, and policies without reconfiguring hardware.


This modern networking paradigm simplifies connecting distributed workloads across on-premises and cloud sites and also secures multi-tenant environments.


## Understanding CNI and Flow CNI


In Kubernetes®, a **Container Network Interface (CNI)** plugin is what enables pods to communicate over the network. **Flow CNI** is Nutanix’s CNI implementation that allows Kubernetes clusters to participate in Flow Virtual Networking, meaning Kubernetes pods can exist within the same VPC networking context as VMs.


### With **Flow CNI** :


- Kubernetes pods receive network connectivity that’s consistent with other VPC subnets.
- Pod traffic can route seamlessly to VMs and other pods across different clusters managed by Prism Central.
- Standard Kubernetes networking models extend into the Nutanix SDN fabric.


This capability is especially powerful in **federated environments** where different clusters and VPCs interconnect to support distributed applications.


## What Is a Federated Environment?


A[federated environment](https://www.nutanix.com/how-to/deploying-a-federated-network) refers to a network of distributed compute resources such as multiple hypervisor clusters and Kubernetes clusters, that operate under a unified policy and management plane. In Nutanix, federation lets you connect and govern these distributed resources as one logical network domain.


For example:


- Multiple AHV hypervisor clusters (for VMs).
- Multiple Kubernetes clusters (for containers).
- Cloud and edge sites.


All of these can be federated under Prism Central and linked through Flow Virtual Networking.


## Why Use Flow CNI in a Federated Environment?


Here’s where Flow CNI shines:


### Unified Networking Across VMs and Kubernetes Pods


In traditional networking, VMs and pods often live in separate network domains, requiring complex integration (e.g., bridging, NAT, or external routers).
With Flow CNI, Kubernetes pods can smoothly join the VPC network where VMs also reside without special bridging or manual route stitching. This means:


- Pods talk to VMs as if they’re on the same virtual network.
- IP addressing and routing are consistent across the federated environment.


This unified connectivity is crucial for hybrid applications where services span across containers and virtual machines.


### Centralized Control Plane with Prism Central


All Flow Virtual Networking constructs including VPCs, subnets, and policies are controlled centrally through Prism Central. This unified control plane means:


- Networking policies apply uniformly across federated VPCs.
- Kubernetes clusters can be orchestrated alongside VM-based workloads.
- Consistency in security and routing is maintained across distributed nodes.


This significantly simplifies operational complexity, especially in large, multi-site deployments.


### Scalable and Secure Multi-Tenant Networking


Flow’s VPCs provide isolated network namespaces. Each VPC:


- Has its own routing and subnet constructs.
- Can support overlapping IP spaces without conflict.
- Helps maintain tenant isolation while maintaining connectivity where needed.


Deploying Flow CNI within this framework means Kubernetes pods benefit from the same multi-tenant networking and policy controls as VMs. This is ideal for enterprises building **secure, tenant-aware services** in federated cloud environments.


## How do things work at a high level?


In a federated setup:


- **Prism Central** manages clusters and coordinates Flow Virtual Networking across all sites.
- **VPCs** are created to represent isolated network domains for applications or tenants.
- **Flow CNI** integrates Kubernetes clusters into these VPCs so pods receive networking consistent with VPC overlay subnets.
- VMs and pods within these VPCs can communicate as peers, with routing and policy enforcement applied at the virtual SDN layer.


This means your API servers, services, and workloads whether running in VMs or Kubernetes, operate with a unified networking layer.


## Pre-Requisites


- Minimum Required Versions


AOS version 7.5


PC version (X-Large) 7.5


Network Controller version 7.0.0


Flow CNI version 1.0.0


NKP version 2.17.1


- This article assumes that you have an NKP cluster[created with FlowCNI installed](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-v7_0_0:ear-flow-cni-prepare-kubernetes-cluster-nkp-pc-t.html) on the workload cluster.


- Once the workload cluster is up and running with FlowCNI, you should see the CNI Plugin as Nutanix Flow.


### Demo Set-Up


- We create a subnet in the **remote** site (AZ) for the physical server.


- ` phy-vpc-subnet1`


- We create two subnets namely in the **local** site


- ` user-VPC1-subnet1`
- ` federated-vpc-subnet1` ( this subnet is extended towards the physical server)


- We use[Layer2 Subnet extension](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-Flow-Controller-v7_5:ear-flow-nw-l2-virtual-network-extension-c.html) – With Layer 2 Network Extension, you can migrate a set of applications to the remote AZ while retaining their network bindings such as IP address, MAC address, and default gateway. With the subnet extension mechanism, VMs can communicate over the same broadcast domain helping to reduce the need to architect the network topology again, which can otherwise result in downtime.


- In this article we are using[L2 Network extension over VTEP](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-Flow-Controller-v7_5:ear-virtual-subnet-extend-vtep-c.html) .


- In this article, a 3-tier application, composed of frontend, middleware, and backend components, is distributed across these three forms of workloads.


- ***The frontend runs in pods on a Kubernetes cluster (192.168.50.9/24)***
- ***The middleware operates on a virtual machine (192.168.1.160/24)***
- ***The database is hosted on a physical server (192.168.1.150/24)***


- Federation is in place amongst the two VPC’s.


- User VPC ( for VM’s only)
- Federated VPC ( for VM’s and Containers)


## Use-Cases


### Reachability across VM’s, Containers, Pods & Physical Servers.


- Create an[NGINX® web server deployment](https://github.com/amitmavgupta/tech-notes/blob/main/Kubernetes/Manifests/sample-nginx-deployment.yaml) on the workload cluster.


```text
[nutanix@nkp-quickstart ~]$ kubectl get pods -o wide | grep nginx
nginx-deployment-96b9d695-89q9k   1/1     Running   0                78d   192.168.50.9    nkp-cluster1-workload-md-0-mq9x4-c289l-jtcc4   <none>           <none>


[nutanix@nkp-quickstart ~]$ kubectl get svc
NAME            TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
deathstar       ClusterIP      10.2.183.154   <none>        80/TCP         20d
kubernetes      ClusterIP      10.2.0.1       <none>        443/TCP        78d
nginx-service   LoadBalancer   10.2.156.172   10.x.x.x   80:30937/TCP   78d


[nutanix@nkp-quickstart ~]$ kubectl get deployment
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
deathstar          2/2     2            2           20d
flow-ovn-ic        1/1     1            1           33h
nginx-deployment   1/1     1            1           78d
```


- From the physical server we can reach the service IP of the NGINX Pod


Reaching the NGINX Pod via its Service Pod from the Physical Server


- As seen below From the Physical Server we can reach the Pod IP.
- As seen below From the Federated VM we can reach the Pod IP.


### Database access/ editing/ creation across VM’s, Containers, Pods and Physical Servers.


#### Pre-Requisites


- Install[PostgreSQL®](https://www.postgresql.org/download/linux/redhat/) database software on the Physical Server acting as the Database.


```text
#Install the repository RPM:


#sudo dnf install -y #https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm


# Install PostgreSQL:


#sudo dnf install -y postgresql14-server


# Optionally initialize the database and enable automatic start:


#sudo /usr/pgsql-14/bin/postgresql-14-setup initdb
#sudo systemctl enable postgresql-14
#sudo systemctl start postgresql-14
```


- Postgresql is installed on the Federated VMs (Rocky Linux based) trying to access the DB server.


```text
#sudo dnf update -y
#sudo dnf install postgresql -y
```


- Postgresql is installed on the Kubernetes Pod that we created above (Debian based) from which we will try to access the DB server.


```text
# sudo apt update -y
# sudo apt install postgresql-client-y
```


- **Create a new Database on the Kubernetes Pod** that is running on the workload Kubernetes cluster with FlowCNI.


```text
CREATE DATABASE dvdrental;
CREATE DATABASE
template1=# \l
List of databases
Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-----------+----------+----------+-------------+-------------+-----------------------
dvdrental | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
|          |          |             |             | postgres=CTc/postgres
template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
|          |          |             |             | postgres=CTc/postgres
(4 rows)
```


- Access the newly created Database from **Federated VM’s** and restore a database and see if the same would be accessible and available to users who try to access it from the Kubernetes pods.


```text
[root@federated-vpc-vm1 nutanix]# psql --host 192.168.1.150 --username postgres -d dvdrental
Password for user postgres:
psql (13.23, server 14.20)
WARNING: psql major version 13, server major version 14.
Some psql features might not work.
Type "help" for help.


dvdrental=#
[root@federated-vpc-vm1 nutanix]# wget https://x.x.x.x/dvdrental.zip
--2026-01-12 14:08:06--  https://x.x.x.x/dvdrental.zip


[nutanix@federated-vpc-vm1 ~]$ unzip dvdrental.zip
Archive:  dvdrental.zip
inflating: dvdrental.tar
[nutanix@federated-vpc-vm1 ~]$ ls
dvdrental.tar  dvdrental.zip
```


- Restore the database on the **Federated VM** .


```text
[nutanix@federated-vpc-vm1 ~]$ pg_restore --host 192.168.1.150 -U postgres -d dvdrental dvdrental.tar
Password:
[nutanix@federated-vpc-vm1 ~]$
```


- Let’s try to access the newly restored database from the **pod** and see a detailed view of the database.


```text
root@nginx-deployment-96b9d695-89q9k:/# psql --host 192.168.1.150 --username postgres -d dvdrental
Password for user postgres:
psql (17.7 (Debian 17.7-0+deb13u1), server 14.20)
Type "help" for help.


dvdrental=# \dt
List of relations
Schema |     Name      | Type  |  Owner
--------+---------------+-------+----------
public | actor         | table | postgres
public | address       | table | postgres
public | category      | table | postgres
public | city          | table | postgres
public | country       | table | postgres
public | customer      | table | postgres
public | film          | table | postgres
public | film_actor    | table | postgres
public | film_category | table | postgres
public | inventory     | table | postgres
public | language      | table | postgres
public | payment       | table | postgres
public | rental        | table | postgres
public | staff         | table | postgres
public | store         | table | postgres
(15 rows)
```


- To make sure we have a completely rounded view lets see if we can see who all actors are included in the dvdrental database from the **Physical Server** .


```text
[root@physical-server data]# sudo -u postgres psql dvdrental
psql (13.23, server 14.20)
WARNING: psql major version 13, server major version 14.
Some psql features might not work.
Type "help" for help.


dvdrental=# SELECT * FROM actor;
actor_id | first_name  |  last_name   |      last_update
----------+-------------+--------------+------------------------
1 | Penelope    | Guiness      | 2013-05-26 14:47:57.62
2 | Nick        | Wahlberg     | 2013-05-26 14:47:57.62
3 | Ed          | Chase        | 2013-05-26 14:47:57.62
4 | Jennifer    | Davis        | 2013-05-26 14:47:57.62
5 | Johnny      | Lollobrigida | 2013-05-26 14:47:57.62
6 | Bette       | Nicholson    | 2013-05-26 14:47:57.62
7 | Grace       | Mostel       | 2013-05-26 14:47:57.62
8 | Matthew     | Johansson    | 2013-05-26 14:47:57.62
9 | Joe         | Swank        | 2013-05-26 14:47:57.62
10 | Christian   | Gable        | 2013-05-26 14:47:57.62
11 | Zero        | Cage         | 2013-05-26 14:47:57.62
12 | Karl        | Berry        | 2013-05-26 14:47:57.62
13 | Uma         | Wood         | 2013-05-26 14:47:57.62
14 | Vivien      | Bergen       | 2013-05-26 14:47:57.62
15 | Cuba        | Olivier      | 2013-05-26 14:47:57.62
16 | Fred        | Costner      | 2013-05-26 14:47:57.62
17 | Helen       | Voight       | 2013-05-26 14:47:57.62
```


## Try out Flow CNI


- Try out[Flow CNI with the Nutanix Kubernetes Platform](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-v7_0_0:ear-flow-cni-prepare-kubernetes-cluster-nkp-pc-t.html) solution and get first-hand experience of how it solves some real problems and use-cases in your cloud-native or on-prem environments related to Networking.


## References


- Flow Virtual Networking Overview-[https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-v7_0_0:ear-flow-nw-overview-pc.html](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-v7_0_0:ear-flow-nw-overview-pc.html)
- Flow CNI Overview-[https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-v7_0_0:ear-flow-cni-overview-pc-c.html](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-v7_0_0:ear-flow-cni-overview-pc-c.html)
- Deploying a Federated Network-[https://www.nutanix.com/how-to/deploying-a-federated-network](https://www.nutanix.com/how-to/deploying-a-federated-network)
- L2 Network Extension-[https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-Flow-Controller-v7_5:ear-flow-nw-l2-virtual-network-extension-c.html](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-Flow-Controller-v7_5:ear-flow-nw-l2-virtual-network-extension-c.html)
- L2 Network Extension over VTEP-[https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-Flow-Controller-v7_5:ear-virtual-subnet-extend-vtep-c.html](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Flow-Virtual-Networking-Guide-Flow-Controller-v7_5:ear-virtual-subnet-extend-vtep-c.html)


## Conclusion


As enterprise environments grow more distributed with Kubernetes and VMs working side by side, networking solutions need to adapt. Nutanix Flow CNI delivers an elegant way to unify network connectivity across these different workloads, all within the powerful SDN paradigm of Flow Virtual Networking.


Whether you’re architecting a hybrid cloud, a multi-tenant system, or scaling Kubernetes alongside VMs, **Flow CNI in federated environments provides the connectivity, policy control, and operational simplicity that modern infrastructure demands** .
