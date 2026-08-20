---
schema_version: "1.0.0"
document_id: "74f8b86a71a6cb0e92a47257c49602fa60d1e9ab10de22b004cc6de8173526a6"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/secure-publication-of-datadog-agent-integrations-with-tuf-and-in-toto/"
published_at: "2019-06-03T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:dd29e57c52deaffb91c24c33ec0710724e30b954af409bbd950a4dcd509dd1bd"
---

# Secure publication of Datadog Agent integrations with TUF and in-toto

Trishank Karthik Kuppusamy


Last year, we decided to meet a key challenge: how do we empower our developers to release new and trustworthy Datadog Agent integrations on demand, without completely trusting automation to do so on their behalf? The challenge lies in how to guarantee end-to-end security when using automation to build, sign, and publish integrations.


## Automatically publishing software


The Datadog Agent includes integrations that collect data from potentially hundreds of software packages. These integrations are bundled with the Agent, and are updated all at once whenever a new version of the Agent is released. This couples the release of integrations with that of the Agent, which can delay the release of crucial feature enhancements and improvements. We also want the ability to automatically build and publish new integrations independently of Agent releases. In this way, interested users can try new integrations as soon as they become available.


## Securing automatic publication with TUF and in-toto


Previous security systems, such as TLS encryption or GPG signatures of software packages, protect users from man-in-the-middle (MitM) attacks. However, they are not *compromise-resilient* , because they do not protect against unauthorized modifications in the infrastructure anywhere between developers and end-users. To the best of our knowledge, we are the first in the industry to discuss how to build such a compromise-resilient CI/CD system: that is, we protect the authenticity and integrity of Agent integrations, from the moment that our developers commit source code, to the point that our end-users install them as packages.


To do so, we use two key pieces of technology:[The Update Framework (TUF)](https://theupdateframework.github.io/) and[in-toto](https://in-toto.github.io/) . The CI/CD system uses TUF to sign new integrations, and in-toto guarantees that the CI/CD system packaged exactly the source code that one of our developers signed. It is important to note that neither technology on its own is sufficient to deliver the desired security guarantees. But by tightly integrating both, we are contributing to our industry’s efforts to make the secure publication of software a standard as opposed to a nice-to-have.


### End-to-end verification with in-toto


To set such a standard, we must prevent tampering at any step in the *software supply chain* between the development and the publication of the software. A step may be, for example, a developer writing source code, or a CI/CD job packaging this source code into a zip file. We use in-toto to specify our supply chain as a fixed series of steps, each of which must produce signed metadata about the input it received, and the output it produced. When a client such as the Datadog Agent puts together the signed metadata, it is able to inspect whether a package was produced following this prescribed series of steps, by only the designated parties.


The four steps of the Datadog Agent integrations supply chain works as follows:


1.


Developers write integration code. An integration consists of Python source code and a few YAML configuration files.


2.


The CI/CD system must receive the source code from the previous step, and previously built Python wheels. Each wheel is a ZIP file that contains the Python and YAML files. It is not allowed to modify previous wheels, but it is allowed to create new wheels.


3.


The CI/CD system must receive the same wheels as the previous step. This steps signs for all wheels using TUF as described shortly.


4.


Finally, the Datadog Agent will download and extract files from one of these wheels in order to ensure that they correspond to exactly the same Python source code and YAML configuration files that our developers signed.


The key point is that this supply chain provides end-to-end verification: it ensures the Agent only trusts wheels containing source code that was released by Datadog developers.


### Secure transport with TUF


While in-toto provides end-to-end verification of a software supply chain, it does not solve a crucial problem that arises in practice: how to securely distribute, revoke, and replace the public keys used to verify the supply chain. This mechanism must be compromise-resilient itself.


TUF provides us with precisely this compromise-resilient mechanism. To do so, TUF adds a higher layer of signed metadata to the repository carefully designed to follow several principles. We use TUF to distribute several artifacts in a compromise-resilient manner:


1.


The root of trust for all wheels as well as TUF and in-toto metadata.


2.


The software supply chain defined using in-toto.


3.


The public keys used to verify this supply chain.


TUF also guarantees that MitM attackers cannot tamper with the consistency, authenticity, and integrity of these files, nor rollback or indefinitely replay metadata.


This security model is simplified because it ignores details and considerations that are out of the scope of this blog post. The key point is that this offline bootstrapping of trust with TUF, and protecting developer signing keys with trusted hardware as described shortly, is what gives in-toto meaningful security guarantees.


### How developers sign integrations with Yubikey, GPG, and in-toto


Our developers sign integrations using hardware keys ([Yubikeys](https://github.com/DataDog/yubikey) ), which are trusted, and support the on-card generation and storage of GPG signing keys.


There are three levels of protection on these keys:


1.


Assuming that the Yubikey firmware works correctly, private keys cannot be exported outside of the card.


2.


Each Yubikey requires a secret user PIN to unlock the signing key.


3.


Every developer must touch the Yubikey to authorize any signing operation.


Our developers use a command-line tool which transparently[calls](https://github.com/DataDog/integrations-core/blob/master/datadog_checks_dev/datadog_checks/dev/tooling/signing.py) in-toto, which in turn calls GPG, to sign integrations. Thus, using Yubikeys, we are able to significantly minimize the risk of a key compromise without hampering usability.


### How the Agent transparently downloads and verifies new integrations with TUF and in-toto


Finally, to install or update integrations, our customers use the Datadog agent, which transparently[calls](https://github.com/DataDog/integrations-core/tree/master/datadog_checks_downloader) TUF and in-toto libraries on their behalf. They see no difference in their experience, except if an attack is caught by TUF or in-toto. In that case, installation of the integration will be denied, and users will see an error message indicating why TUF or in-toto failed to verify the installation.


## Want to learn more?


More technical details, such as designing and securely performing the offline key generation and rotation ceremony, will be discussed in subsequent blog posts. Interested readers may also wish to consult our[KubeCon 2018 talk](https://youtu.be/XAlvd4QXngs) and[28th USENIX Security Symposium technical paper](https://www.usenix.org/conference/usenixsecurity19/presentation/torres-arias) for more details:


TUF is hosted by the Linux Foundation as part of the[Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/) , and is[used](https://theupdateframework.github.io/adoptions.html) in production by a growing number of companies, large and small. A variant of TUF called[Uptane](https://uptane.github.io/) is designed to secure over-the-air updates for ground vehicles, and is being[standardized](https://uptane.github.io/uptane-standard/uptane-standard.html) by the[IEEE-ISTO](https://ieee-isto.org/) for use by the North American automobile industry.


Our implementation of TUF and in-toto is now[generally available](https://docs.datadoghq.com/agent/guide/integration-management/) from Datadog Agent 6.10 onward. We look forward to your feedback, and we hope you enjoy our new way to securely update to the latest Datadog Agent integrations!


-
-
-
