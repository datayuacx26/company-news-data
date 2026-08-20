---
schema_version: "1.0.0"
document_id: "bbe3ae8870a8c2f4c7571c577d89371b322a5e15de5303c265fa11cb2ff74223"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2025-12-18-browser-native-verification"
published_at: "2025-12-18T17:38:00+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T22:24:52.050374+00:00"
content_hash: "sha256:887acffabcae34383a46284f7e7864256d554aa7461e80b2920df9deb1ecf819"
---

# Building a Browser-Native Verification Stack for Tinfoil

[← Back to Posts](https://tinfoil.sh/blog)


# Building a Browser-Native Verification Stack for Tinfoil


Dec 18, 2025


•


10 min read


Sacha Servan-Schreiber and Giulio Berra


# Introduction


At[Tinfoil](https://tinfoil.sh/) , we've built the open-source infrastructure to run AI models inside secure hardware enclaves powered by NVIDIA GPUs. Using Tinfoil, it becomes possible to run private AI training and inference workloads in the cloud, without anyone but the end user seeing the data going in and out. All prompts and responses are encrypted and only processed inside hardware-isolated trusted execution environments where the model weights live. This means that nobody, not even Tinfoil, can peek into this secure enclave, ensuring that all data is kept private.


## Privacy is only as good as the verification


Any privacy claim requires verification to be meaningful. Without independent proof, all claims would degrade to "pinky-promise" assurances on Tinfoil's part. Specifically, if users had to trust Tinfoil to never look at the data, the state of the world reverts to a "trust us" model that is similar to existing "zero data retention" policies offered by some AI providers. In contrast, with Tinfoil, anyone should be able to independently inspect and verify that the following hold true:


1. The code running in the enclave matches a published open-source code release
2. The enclave hardware is set up correctly and attested by the manufacturer (e.g., NVIDIA)


Importantly, this verification should happen entirely *client-side* in order to avoid placing trust in a third party auditor. Note that Tinfoil still relies on the manufacturer to produce correct chips and GitHub actions when compiling the code, but does not require third-party trust at runtime aside from the chip manufacturer certifying the hardware.


When a Tinfoil client is running in a web browser, like we have for[Tinfoil Chat](https://chat.tinfoil.sh/) , this means that the whole verification process must be performed in the end user's browser environment.


**Figure 1:** Client-side verification center (as seen here on Tinfoil Chat) shows users that their data is encrypted, the code is auditable through Sigstore, and the runtime is isolated in a secure enclave. All verification happens client-side.


## The Tinfoil verification stack


For verifying that the code running in the enclave matches open-source releases, Tinfoil publishes all releases through GitHub actions onto the[Sigstore](https://www.sigstore.dev/) transparency log. Sigstore is an industry-standard open-source project backed by the Linux foundation that provides cryptographic signing and verification for software artifacts through append-only logs.


The Tinfoil verifier uses the Sigstore verification client to automatically match the code running in the secure enclave to a published GitHub release, ensuring supply-chain security via end-to-end transparency mechanism.


For hardware attestation, the Tinfoil verifier checks the AMD SEV-SNP or Intel TDX attestation reports of the host CPU which, in turn, attest the NVIDIA GPU with the loaded model weights. These attestations provide cryptographic proof that the code (and model weights) are running inside of a trusted execution environment powered by genuine hardware, with the correct configuration, and match the code and weights from Sigstore.


In short, the Tinfoil verifier needs to (1) verify the remote attestation report and (2) check that the signed measurements from the attestation report match the open-source code and weights.


# Browser-Based Verification


## Initial WASM Approach


The main Tinfoil verifier is[a Go library](https://github.com/tinfoilsh/verifier) that combines existing software libraries for[Sigstore](https://sigstore.dev/) verification with hardware attestation verification libraries for AMD SEV-SNP and Intel TDX. Go is a natural choice for these tasks given its rich ecosystem of cryptographic libraries and portability to other languages. In particular, using our Go verifier we were able to bring the client-side verifier to the browser via WebAssembly and to mobile platforms via[gomobile](https://pkg.go.dev/golang.org/x/mobile/app) .


Unfortunately, for browser contexts, the resulting WASM blob is around 80MB in size, making it impractical for web deployments. This large blob size comes from several dependencies (including Sigstore) and the heavy reliance on the Go crypto library. Just verifying Sigstore artifacts requires a[TUF](https://theupdateframework.io/) client for secure distribution of trust anchors, X.509 certificate chain validation, and Merkle inclusion proof verification. Moreover, Go compiles its garbage collector and goroutine scheduler into WASM, increasing its size, and none of the aforementioned dependencies were optimized to play well with WASM.


Despite the 80 MB blob, the WASM approach has served us in production for over six months of running Tinfoil Chat. But with initial page load times being significantly impacted by the blob size, sometimes reaching over 15 seconds, it was obvious to us that a native JavaScript verifier had to come sooner than later.


## Browser-Native Verification


To get a native Tinfoil verifier that runs entirely in the browser, without depending on any WASM, was challenging for several reasons. First, there were no native libraries for remote attestation, no browser-compatible Sigstore library, and little to no JavaScript support for many of the required dependencies.


Fortunately for us, however, both Sigstore and TUF did have NodeJS implementations ([sigstore-js](https://github.com/sigstore/sigstore-js) and[tuf-js](https://github.com/theupdateframework/tuf-js) ) which we could use as references when building the browser-compatible libraries. NodeJS and browsers are both JavaScript runtimes, but with different APIs: NodeJS can access the filesystem and OS while browsers are sandboxed and cannot.


The challenge we faced in migrating the code from the NodeJS runtime is the lack of cryptographic libraries that support browser environments. NodeJS has the` crypto` module with access to OpenSSL. In contrast, browsers only have the[Web Cryptography API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API) , which has a more restricted interface and less low-level control over many operations. As such, cryptographic code written to run in a NodeJS environment cannot always be easily converted to work in the browser.


Another problem was storage and network access. The TUF client (a dependency of Sigstore) needs to cache metadata and handle updates. NodeJS reads and writes to the filesystem, while browsers only have localStorage available,[necessitating some adaptation to work in browser contexts](https://github.com/sigstore/sigstore-js/issues/1195) . While less of a technical problem, this still required careful thinking and adaptation.


In examining possible solutions, we came across[Giulio Berra](https://lsd.cat/) from the[Freedom of the Press Foundation](https://freedom.press/) , who had already investigated the problem of running Sigstore verification in the browser as part of his work on[building integrity into browser-based applications](https://eprint.iacr.org/2025/797) .


**Figure 2:** The client-side verifier checks that code served by the application server matches the pinned code on the Sigstore transparency log.


# Building Browser-Compatible Sigstore and TUF Libraries


Working with Giulio, we created browser-compatible versions of both Sigstore and its TUF dependency in TypeScript:


- [sigstore-browser](https://github.com/freedomofpress/sigstore-browser) — Sigstore verification using WebCrypto for ECDSA and Ed25519 signatures, X.509 certificates, and ASN.1 parsing.
- [tuf-browser](https://github.com/freedomofpress/tuf-browser) — TUF client with storage backends for localStorage.


We started with the reference NodeJS implementations and carefully ported over the code to work in a browser context. This meant migrating all the cryptographic operations to work with WebCrypto, using localStorage instead of the filesystem, and adapting many parts of the codebase to avoid NodeJS-specific primitives like` Buffer` .


Both our` sigstore-browser` and` tuf-browser` libraries are focused exclusively on verification. We believe that, similarly to Tinfoil's requirements, the typical browser-based use case of these libraries will involve some sort of verification of already signed artifacts. Beyond the core cryptographic operations, we ported X.509 certificate parsing and ASN.1 handling to work without NodeJS dependencies. These certificate verification functionality also ends up being used for verifying remote attestations in the full Tinfoil client.


Both libraries pass their respective official conformance test suites, have only one external dependency ([noble-curves](https://www.npmjs.com/package/@noble/curves) for low-level elliptic curve operations not available via the Web Crypto API), and are very lightweight (the Sigstore minified client is only 37kB in size).


For Tinfoil, this means our verifier can now perform Sigstore verification natively in the browser without slowing down page load times. Indeed, the final minified Tinfoil verifier is just under 50KB, representing a 1600x reduction in size relative to our original WASM-based verifier. The full browser-native verification implementation is available in the[tinfoil-js](https://github.com/tinfoilsh/tinfoil-js) SDK.


# Other Applications of Browser-Based Verification


Browser-native supply chain verification is useful beyond enclave attestation and we hope that the Sigstore and TUF libraries will prove beneficial for other projects as well.


For instance, many package managers such as npm, PyPI, and Homebrew all support Sigstore attestations, while most software distributed outside those is difficult to verify for end-users, who are less likely to follow command-line procedures to check hashes or signatures. With a browser-based verifier, software download web pages could verify artifact signatures before users download them, without trusting the individual mirror from which this software is being distributed.


Purely browser-based cryptography significantly strengthens the overall security posture of most systems, especially in the event of data breaches or one-off compromises. However, existing browser security mechanisms don't protect against a compromised server serving malicious JavaScript, and the possibility of targeting specific users could make such attacks undetectable. In practice, there are currently no viable defenses against persistent server attackers. Since the server controls what code is served from the domain, web applications like[Element](https://element.io/en) , Whatsapp Web,[Proton](https://proton.me/) , and[Tinfoil Chat](https://chat.tinfoil.sh/) currently require users to either blindly trust the server to provide the right application code, manually inspect the loaded source code, or self-host the entire application (assuming the client code is open source).


## Future Work: WEBCAT + Tinfoil Chat


All web applications run into the same[trust bootstrapping problem](https://security.stackexchange.com/questions/238441/solution-to-the-browser-crypto-chicken-and-egg-problem) : the JavaScript that performs verification is itself served by the server at almost every page load. The Freedom of Press Foundation is actively building[WEBCAT](https://securedrop.org/news/introducing-webcat-web-based-code-assurance-and-transparency/) , a project to verify the integrity of browser applications, such as the[future version of SecureDrop](https://securedrop.org/news/introducing-securedrop-protocol/) . WEBCAT uses a browser extension to check served code against signed manifests before execution. Application manifests contain detailed information about the web application assets and its execution environment, such as a map of path and file hashes, security headers information. Furthermore, it contains pointers to the originating repository and version to allow auditors to verify reproducibility, if the code is open source.


If the manifest doesn't match what's published on the transparency log, or isn't properly signed by its developers, the page doesn't load. As the validation happens transparently, end users do not have to pay attention to new security indicators or information in order to be protected. Information about authorized signers and[their chosen transparency logs](https://www.youtube.com/watch?v=8ZJqV_zog7s) is kept in a public distributed ledger and fetched at browser startup. However, after that, the entire verification process is performed locally, avoiding contacting third parties, which is fundamental both for latency and to avoid privacy leaks.


We're actively working with Freedom of Press Foundation to bring WEBCAT into Tinfoil Chat and elevate the security guarantees for our web-based applications. With WEBCAT, users will be able to verify the application code (which includes the Tinfoil verifier), closing the loop on verifiability for browser-based private AI applications.


# References


- [tinfoil-js](https://github.com/tinfoilsh/tinfoil-js) — Tinfoil SDK with browser-native verification for secure enclaves
- [sigstore-browser](https://github.com/freedomofpress/sigstore-browser) — Browser-native Sigstore verification library
- [tuf-browser](https://github.com/freedomofpress/tuf-browser) — Browser-native TUF client library


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2025-09-24-private-chat-backups-local-first)[Next Post](https://tinfoil.sh/blog/2026-01-22-private-ai-web-search)
