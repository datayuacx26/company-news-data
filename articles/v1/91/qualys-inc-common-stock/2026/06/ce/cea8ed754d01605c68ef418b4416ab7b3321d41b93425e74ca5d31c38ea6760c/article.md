---
schema_version: "1.0.0"
document_id: "cea8ed754d01605c68ef418b4416ab7b3321d41b93425e74ca5d31c38ea6760c"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-b23fdbdd1cee"
canonical_url: "https://blog.qualys.com/product-tech/2026/06/23/windows-11-24h2-eol-version-drift-trurisk-eliminate"
published_at: "2026-06-23T15:00:00+00:00"
first_seen_at: "2026-07-20T03:31:07.801512+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:a82d63e68a08ea65f95ef8e918a25ad0b7a7d47c074692810ef22aada44bf3fa"
---

# 3 Paths to Upgrade Windows 11 before 24H2 End of Servicing (EOL)

#### Table of Contents


- Before You Start: Assess Endpoint Readiness
- Upgrade Path 1: Enablement Package Preferred Approach (Recommended)
- Upgrade Path 2: Feature Update via ISO (Fallback)
- Upgrade Path 3: Direct Upgrade from Windows 10
- Operationalizing Windows Upgrade Strategy
- Frequently Asked Questions


---


#### Key Takeaways


- Windows 11 24H2 reaches the end of servicing on October 13, 2026, making timely enterprise upgrades critical.
- Enterprises often face version drift, with multiple Windows 11 builds across endpoints requiring different upgrade paths.
- Upgrade approaches vary based on system state and may include enablement packages, ISO-based feature updates, or direct Windows 10 upgrades.
- For supported Windows 11 systems, enablement packages provide the fastest, lowest-impact upgrade method.
- Centralized orchestration with TruRisk Eliminate helps standardize upgrades and reduce version drift at scale.


---


In enterprise Windows environments, endpoints often run multiple build versions due to variations in update cadence, system readiness, and upgrade methodologies. As a result, different upgrade approaches may be required, including enablement packages, feature updates, or direct OS transitions. This makes maintaining version consistency a persistent operational challenge.


This challenge becomes more critical as systems approach the end of their service life. According to[Microsoft’s official](https://learn.microsoft.com/en-us/lifecycle/products/windows-11-home-and-pro) lifecycle policy, **Windows 11 version 24H2 (Home and Pro) will reach the end of servicing on October 13, 2026** . This makes it essential to plan upgrades well in advance, particularly in environments where endpoints are distributed across multiple OS baselines.


To address this challenge,[Qualys TruRisk Eliminate](https://www.qualys.com/apps/trurisk-eliminate) provides comprehensive coverage across all upgrade scenarios, enabling consistent and controlled execution across your entire environment.


## Before You Start: Assess Endpoint Readiness


Before initiating any upgrade activity, endpoints should be grouped by eligibility and current OS state:


- Enablement-ready systems running recent Windows 11 builds
- Systems requiring feature updates on older Windows 11 versions
- Windows 10 systems requiring a direct upgrade


## Upgrade Path 1: Enablement Package Preferred Approach (Recommended)


Enablement packages are lightweight updates that activate functionality already present within the operating system. Rather than performing a full OS upgrade, they unlock new capabilities within the existing build.


**Advantages of Enablement Packages**


- Minimal download size
- Faster installation relative to full feature updates
- Reduced reboot requirements
- Lower operational impact and failure rate


For endpoints already running recent Windows 11 builds, feature updates can be deployed using enablement packages. This makes them the most efficient and low-impact upgrade path available.


### Deploying Windows 11 Upgrades with TruRisk Eliminate


With Qualys TruRisk™ Eliminate, IT teams can create patch jobs directly from the patch catalog and deploy enablement packages at scale. This enables rapid, enterprise-wide rollout across endpoints without relying on fragmented tooling or manual intervention.


Example: Use QQL to identify enablement packages:


```text
patch.title:"Enablement"
```


Select the required patch from the results and create a patch job targeting the relevant asset groups.


## Upgrade Path 2: Feature Update via ISO (Fallback)


Enablement packages are the preferred method for Windows feature updates and are typically released by Microsoft for supported baselines.


However, in rare cases where Microsoft does not release an enablement package for a given transition, a feature update using an ISO becomes necessary. Based on recent release patterns, this scenario is uncommon and has been observed only in limited cases, such as the transition from Windows 11 23H2 to 24H2.


### Deploying ISO-Based Feature Updates with TruRisk Eliminate


Use QQL to identify Microsoft feature update patches:


```text
patch.isFeatureUpdate:true AND patch.vendor:`Microsoft`
```


From the results, select the required ISO-based package, enable it in the catalog, and create a patch job targeting the relevant asset groups.


## Upgrade Path 3: Direct Upgrade from Windows 10


For environments still running Windows 10, multi-step upgrades are not required. Systems can be directly upgraded to Windows 11 25H2 without intermediate transitions.


If endpoints need to move from Windows 10 to the latest supported build, Qualys **TruRisk Eliminate** provides the required coverage to execute this transition in a controlled manner.


### Upgrading Windows 10 Systems with TruRisk Eliminate


With Qualys **TruRisk™ Eliminate** , ready-to-use scripts are available in the[Custom Assessment and Remediation](https://www.qualys.com/apps/custom-assessment-remediation) (CAR) library to directly upgrade systems from Windows 10 to the latest Windows 11 build.


Example: Navigate to **Script Library → System Updates and Upgrades → Windows OS Upgrades** . Select the **Windows 10 to Windows 11 upgrade script** , import it in approved mode, and execute it on the required asset groups.


## Operationalizing Windows Upgrade Strategy


Managing Windows upgrades requires aligning the appropriate method with each system’s state. Using enablement packages, ISO-based upgrades where needed, and direct Windows 10 transitions ensures consistent and controlled execution across the environment.


*Planning your upgrade strategy early can keep your systems supported and aligned. By starting ahead, you ensure seamless integration across your environment. Ready to leap?*


---


**Start your free trial today to see how TruRisk Eliminate can streamline your Windows upgrades!**


[Start Your Free 30-Day Trial](https://www.qualys.com/free-trial-new/trurisk-eliminate)


---


## Frequently Asked Questions


**When is Windows 11 24H2 end of support?**
October 13, 2026 for Home and Pro editions. Enterprises should begin planning upgrades well in advance.


**What is Windows 11 version drift?**
It occurs when endpoints run multiple different builds of Windows 11, requiring varied upgrade approaches (enablement packages, feature updates, or full OS transitions).


**What is the recommended upgrade path for recent Windows 11 builds?**
Enablement packages are the preferred, low-impact method. They unlock new features with minimal download size, faster installation, and fewer reboots.


**How can Qualys TruRisk Eliminate help with Windows 11 upgrades?**
It allows you to target systems with QQL, deploy enablement packages or ISO updates via patch jobs, and run direct Windows 10 to Windows 11 upgrade scripts from the CAR library.


**Should I upgrade Windows 10 systems directly to 24H2?**
Yes. Direct upgrades are supported and recommended to avoid unnecessary intermediate steps.
