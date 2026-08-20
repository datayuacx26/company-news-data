---
schema_version: "1.0.0"
document_id: "c5b9feb606958c9bf8d4449b089090bc1acc857287259dbccd99931a93067626"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/little-orphan-kube-proxy"
published_at: "2025-03-24T00:00:00+00:00"
first_seen_at: "2026-07-27T21:32:22.131944+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:b84f9b417fe965f4cecfcc4ae80cd7f383977a87cbe424c625a2da6df88a622a"
---

# Little Orphan Kube-Proxy

If you have run an Amazon EKS cluster long enough, you may have stumbled upon a subtle operational responsibility that you inherited without knowing. If you haven’t heard of kube-proxy, it’s a vital component in a Kubernetes cluster that facilitates network communication. In EKS, by default, managing kube-proxy is **YOUR** responsibility. While EKS installs kube-proxy as part of cluster setup, it does not manage updates or compatibility with your Kubernetes version. This silent handoff can lead to problems if left unaddressed.


## The Problem: Kube-Proxy Falls Behind


When you upgrade your EKS cluster to a newer version, core control plane components (like the API server) are managed by AWS, but the kube-proxy daemonset is not automatically updated to reflect the newer version. This is especially relevant since[EKS only keeps Kubernetes versions on “standard support” for ~14 months, and extended support for ~26 months](https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-versions.html) . With new releases every ~4 months, your kube-proxy version can become outdated very quickly. If kube-proxy remains outdated while your cluster version advances, you may encounter several issues:


- **API Compatibility Issues** – Older kube-proxy versions may not support new Kubernetes API changes, leading to unexpected networking behavior.
- **Performance Degradation** – Bug fixes and optimizations introduced in newer kube-proxy releases won’t be applied, leading to potential inefficiencies in packet routing.
- **Security Risks** – An outdated kube-proxy version may lack important security patches, increasing exposure to known vulnerabilities.
- **Unexplained Networking Failures** – Kubernetes networking relies on kube-proxy for[handling virtual IPs](https://kubernetes.io/docs/reference/networking/virtual-ips/) . Incompatibility can cause service disruptions, intermittent failures, or poor DNS resolution.


So how can you tell if the kube-proxy daemonset in your EKS cluster is out of date? Use kubectl to check the image that is running:


```text
kubectl get daemonset -n kube-system kube-proxy -o yaml | grep image:


```


Check the image tag, and if it isn’t aligned to the cluster version, then it's time to update.


## The Solution: Keep Kube-Proxy Up to Date


### Manually Update Kube-Proxy with an Updated Image (Not Recommended)


If you’ve found this article because you’re currently experiencing network issues from an out-of-date kube-proxy and need to remediate immediately, this is your best option. First, fetch the currently running image:


```text
kubectl get daemonset -n kube-system kube-proxy -o yaml | grep image:


```


Then check this page to see what the latest kube-proxy version is for your cluster version:[https://docs.aws.amazon.com/eks/latest/userguide/managing-kube-proxy.html#kube-proxy-versions](https://docs.aws.amazon.com/eks/latest/userguide/managing-kube-proxy.html#kube-proxy-versions)


Take the image from the above command, replace the tag with the updated tag from the article, and update the daemonset:


```text
kubectl  set   image daemonset/kube-proxy -n kube-system kube-proxy=<image>


```


### Use an EKS Addon for Kube-Proxy (Recommended)


AWS now provides an EKS addon for kube-proxy, which simplifies version management. Since the best way to manage EKS is with an Infrastructure-as-Code solution (like OpenTofu or Terraform) I’ve provided an HCL snippet to convert an existing kube-proxy daemonset to an EKS addon, and keep it in sync with the cluster version.


```text
data "aws_eks_addon_version" "kube-proxy" {
addon_name         = "kube-proxy"
kubernetes_version = var.cluster_version
most_recent        = true
}


resource "aws_eks_addon" "kube-proxy" {
cluster_name                = var.cluster_name
addon_name                  = "kube-proxy"
addon_version               = data.aws_eks_addon_version.kube-proxy.version
resolve_conflicts_on_create = "OVERWRITE"
resolve_conflicts_on_update = "PRESERVE"
}


```


This snippet will ensure your kube-proxy version is aligned with your EKS version whenever you apply your IaC. This snippet was pulled directly from Massdriver’s EKS bundle, which is free for use in our platform.


## Conclusion


Hopefully this article was helpful in remediating or preventing an issue with networking in your EKS cluster. While you’re looking at EKS addons, also check out the VPC CNI addon, as well as the CoreDNS addon - both of which have the same issue of user responsibility. You can find an[implementation example for all 3 addons](https://github.com/massdriver-cloud/aws-eks-cluster/blob/cf444956677d0a9336d0c706bbc8973efa72fe3f/core-services/addons.tf#L15-L86) in the public Massdriver EKS bundle template. You can also import this bundle and start using it for free with a Massdriver account. Check it out!
