---
schema_version: "1.0.0"
document_id: "6551894cb6a02035f2d4dc7dbc148fdba3066fc94bd650a9953282579af77d72"
company_key: "yc-okteto"
company: "Okteto"
source_id: "yc-okteto-rss-a64bce3f80ea"
canonical_url: "https://www.okteto.com/blog/product-update-may-2023/"
published_at: "2023-06-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.770894+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:d934193598716e12e01943966de6f0ebaaf5810bc8bfb37fca6121f72f1f0351"
---

# Okteto 1.8 Chart Release, CLI 2.16 and Support for Kubernetes 1.25

# Okteto 1.8 Chart Release, CLI 2.16 and Support for Kubernetes 1.25


We're excited to bring you another product update with several great new features and improvements to the Okteto experience.


### May highlights


- Okteto 1.8 Chart Release
- Change to ingress architecture that affects customers using their own ingress-controller
- Okteto CLI 2.16 Release
- Support for Kubernetes 1.25
- Dropped support for Kubernetes 1.22


### Before you upgrade


Okteto 1.8 includes a change in the ingress architecture. If you're using your own ingress controller instead of the ingress-controller embedded with Okteto chart, please checkout the updated section about["How to use your own ingress controller"](https://www.okteto.com/docs/1.9/self-hosted/administration/configuration/#using-your-own-ingress-controller) .


## New features & improvements


### Updated the Okteto CLI to version 2.16


The latest version of the Okteto CLI includes several new features and some bug fixes. You can find a more thorough overview of what's new in versions 2.15 and 2.16 in our documentation link below.


[Documentation](https://github.com/okteto/okteto/releases)


### Added support for Kubernetes 1.25


As part of our goal to maintain N-2 version support for Kubernetes, we've now added support for v1.25 to remain aligned with cloud infrastructure providers and the regular channel releases from the CNCF and Kubernetes community.


### Added an installation guide to the Okteto UI for Self-Hosted customers


We've added an in-app experience to the Okteto Admin UI showing the main steps in the Okteto installation process. You now have a source of truth within the Admin UI to gather necessary access credentials or other information to get Okteto installed within your environment even faster than before.


Each step in the guide is optional and can be skipped if it does not apply to your situation and provides documentation to help complete the step to ensure success with the process.


[Documentation](https://www.okteto.com/docs/1.9/administration/dashboard/#installation)


### Destroy External Resources from the Okteto UI


You can now manually destroy External Resources from the Okteto UI that you provisioned as part of your **deploy** configuration, in the Okteto manifest, to ensure those resources don't remain active and idle when development of your application is complete.


### Added additional icons for External Resources


We've added additional icons to External Resources to improve the developer experience when interpreting the resources that are part of an application in Okteto. New icons include:


- AWS
- GCP
- LaunchDarkly
- MongoDB


### Catalog items now sorted the same in admin and developer views


Previously the two Catalog modals were sorted differently. Now both the admin and developer views should be defaulted sorted in the same way: alphabetically.


### Support added for Bitbucket personal repositories


We've improved the integration with Bitbucket to now support personal repositories. This gives customers an additional option to bring code repositories into Okteto for faster development.


### Persistent namespaces can no longer be put to sleep by admins


We've removed the option for admins to sleep a namespace that had been set to **Keep Awake** previously. This ensures that when a namespace is intended to persist garbage collection it cannot still be put in a sleep state and cause a confusing experience for developers of that namespace.


## Breaking changes


### Changed default value for buildkit's service session affinity


This value has changed to **None** by default and will break your Okteto experience if you do not make a change to your buildkit configuration.


To keep using **ClientIP** as **sessionAffinity** you must set **service.sessionAffinity: Client IP** in your buildkit configuration.


## See a more detailed list


For a more detailed list of improvements, bug fixes, breaking changes, and deprecation notices please see[our documentation](https://www.okteto.com/docs/self-hosted/install/releases/#180) .


Matt Gonzales


Director of Product / Perpetually Underdressed 👕


[View all posts](https://www.okteto.com/blog/authors/matt-gonzales/)


#### Share this:
