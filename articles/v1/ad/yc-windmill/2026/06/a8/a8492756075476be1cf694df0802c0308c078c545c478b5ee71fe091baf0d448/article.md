---
schema_version: "1.0.0"
document_id: "a8492756075476be1cf694df0802c0308c078c545c478b5ee71fe091baf0d448"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/k8s-scale-in-pod-deletion-cost"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:55482fe64ba21a15fc2cfe90f6908bc56d5d7decc7372243c32655a6f7cd7180"
---

# Kubernetes autoscaling scale-in prefers idle worker pods

### [Kubernetes autoscaling scale-in prefers idle worker pods](https://www.windmill.dev/changelog/k8s-scale-in-pod-deletion-cost)


Autoscaling


Kubernetes


[Enterprise](https://www.windmill.dev/pricing)


[Docs](https://www.windmill.dev/docs/core_concepts/autoscaling#scale-in-prefers-idle-workers)


The native Kubernetes autoscaling integration now annotates worker pods with pod-deletion-cost before scaling in, so the ReplicaSet controller deletes idle pods first instead of picking victims blindly. Busy pods are protected proportionally to the age of their oldest running job. The annotation pass is best-effort and requires optional list/patch RBAC on pods; without it, scaling proceeds as before with a warning.


#### New features


- Before a scale-in, worker pods are annotated with controller.kubernetes.io/pod-deletion-cost.
- Idle pods get cost 0 and are deleted first; busy pods get 10 + the age of their oldest running job in seconds (capped at 1 day).
- Best-effort: missing RBAC, API or DB errors never fail the scaling operation, it falls back to the previous behavior with a warning.
- Requires optional list and patch permissions on pods; the autoscaling health check now warns (non-fatally) if they are missing.
- Honored on Kubernetes 1.22+ where the PodDeletionCost feature gate is enabled by default.
