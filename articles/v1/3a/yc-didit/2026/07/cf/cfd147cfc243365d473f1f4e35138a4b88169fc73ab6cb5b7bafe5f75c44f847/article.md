---
schema_version: "1.0.0"
document_id: "cfd147cfc243365d473f1f4e35138a4b88169fc73ab6cb5b7bafe5f75c44f847"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/w3c-decentralized-identifiers-dids-specification/"
published_at: "2026-07-28T16:31:55.777+00:00"
first_seen_at: "2026-07-29T00:27:47.387739+00:00"
fetched_at: "2026-07-29T00:27:49.458663+00:00"
content_hash: "sha256:61efa54feacee8521e97790ed8b38a7ee750645080bfc671a485aa94ea858fde"
---

# W3C Decentralized Identifiers (DIDs) Specification

[Back to blog](https://didit.me/blog/) Blog · July 28, 2026


# W3C Decentralized Identifiers (DIDs) Specification


A technical explanation of the W3C DID Core specification: identifier and DID URL syntax, subjects, controllers, DID documents, methods, resolution, verification relationships, services, privacy, and Verifiable Credentials.


By Didit


·


July 28, 2026 ·


Updated Jul 28, 2026


The W3C Decentralized Identifiers (DIDs) specification defines a URI syntax, a common data model, DID documents, core properties, representations, method requirements, and abstract interfaces for resolution and DID URL dereferencing. DID Core 1.0 became a W3C Recommendation on 19 July 2022. DID Core 1.1 was published as a Candidate Recommendation Snapshot on 5 March 2026; it is newer work at a different standards stage.


DID Core does not require a blockchain, prove a person’s legal identity, store credentials inside every DID document, or make every resolved endpoint trustworthy. It standardizes the identifier and document architecture. A separate DID method defines how a particular DID is created, read, updated, and deactivated on its chosen infrastructure.


## **Key takeaways**


- **A DID is a URI, not a credential.** Its generic form is` did::` .
- **Subject and controller are different roles.** The subject is what the DID identifies; the controller is authorized by the method to change the DID document.
- **Keys need explicit purposes.** A verification method becomes usable for authentication, assertion, key agreement, capability invocation, or delegation only through the corresponding verification relationship.
- **The method supplies the operational rules.** DID Core is technology-neutral; the method defines registry interaction, authorization, update, deactivation, and method-specific resolution.
- **Resolution does not create trust by itself.** Implementers must authenticate method results, enforce proof purpose, manage keys and history, protect privacy, and apply application policy.


## **What is a decentralized identifier?**


A decentralized identifier is an identifier designed so that control can be established without requiring one central identity provider or certificate authority to issue and maintain every identifier. The word “decentralized” describes the architecture’s ability to separate identifier control from a single central issuer; it does not say that every implementation is anonymous, public, immutable, or stored on a distributed ledger.


A DID can identify:


- a person;
- an organization or group;
- a device or physical object;
- a digital resource;
- a data model;
- an abstract concept.


The entity identified is the **DID subject** . The string alone does not reveal the subject type.


### **DID syntax**


The generic syntax is:


```text
did:<method-name>:<method-specific-id>
```


For example:


```text
did:example:123456789abcdefghi
```


` did` is the URI scheme.` example` is the DID method name. The remaining string is the method-specific identifier. The method specification defines what that value means and how software processes it.


A valid-looking DID is not necessarily usable. The method must exist and the resolver must support it.


## **DID URLs: paths, queries, and fragments**


A DID URL starts with a DID and can add a path, query, or fragment:


```text
did:example:123456789abcdefghi/path?service=messages#key-1
```


These components can identify or help select:


- a verification method inside the DID document;
- a service entry;
- another DID-document fragment;
- a resource reached through a service;
- a version or method-defined option.


The fragment` #key-1` commonly identifies a verification method. It does not mean the private key is in the document. DID documents publish public verification material or references; secret material must remain protected elsewhere.


## **The DID architecture**


The main concepts are related but not interchangeable:


Concept Role


DID Globally unique identifier conforming to DID syntax


DID subject Person, organization, thing, resource, or concept identified


DID controller Entity authorized under the DID method to change the DID document


DID document Data associated with the subject, including permitted verification methods and services


DID method Separate specification for method-specific syntax and operations


Verifiable data registry Infrastructure a method uses to create, read, update, or deactivate DID state


DID resolver Software or hardware that performs DID resolution for supported methods


DID URL dereferencer Software or hardware that obtains a resource identified by a DID URL


### **Subject versus controller**


The subject and controller can be the same entity, but they do not have to be. A parent could control a DID for a child, an organization could control a DID for a device, or several trustees could control a recovery arrangement.


The top-level` controller` identifies one or more DID controllers. A verification method’s required` controller` identifies who controls that method; it is not automatically the top-level DID controller. Confusing them can grant unintended authority.


## **What is a DID document?**


A DID document is the data associated with a DID subject under the DID Core data model. Its root` id` is the DID. Optional core properties can describe controllers, alternative identifiers, verification methods, verification relationships, and services.


This simplified example uses public material from the specification’s example space:


```text
{
"@context": [
"https://www.w3.org/ns/did/v1",
"https://w3id.org/security/suites/jws-2020/v1"
],
"id": "did:example:123",
"verificationMethod": [
{
"id": "did:example:123#key-1",
"type": "JsonWebKey2020",
"controller": "did:example:123",
"publicKeyJwk": {
"kty": "OKP",
"crv": "Ed25519",
"x": "VCpo2LMLhn6iWku8MKvSLg2ZAoC-nlOyPVQaO3FxVeQ"
}
}
],
"authentication": [
"did:example:123#key-1"
],
"assertionMethod": [
"did:example:123#key-1"
]
}
```


` did:example` is reserved for examples; production systems must use an actual method and its current verification-method suite requirements. The JSON above is intentionally valid JSON. Many examples printed inside technical specifications contain comments or ellipses for readability and should not be copied directly into a parser.


### **Core properties**


- **` id` :** the DID for the subject; required at the document root.
- **` controller` :** one or more DIDs authorized to make changes under the method.
- **` alsoKnownAs` :** other URIs asserted to identify the same subject.
- **` verificationMethod` :** public verification mechanisms that may be referenced by explicit relationships.
- **` authentication` :** methods authorized for authentication as the subject.
- **` assertionMethod` :** methods authorized to express claims, such as credential issuance.
- **` keyAgreement` :** methods intended for deriving shared secret material.
- **` capabilityInvocation` :** methods authorized to invoke capabilities.
- **` capabilityDelegation` :** methods authorized to delegate capabilities.
- **` service` :** endpoints or interaction mechanisms associated with the subject.


` alsoKnownAs` is an assertion, not cryptographic proof that two identifiers are equivalent. Applications should independently verify the relationship required by their trust model.


## **Data model and representations**


DID Core defines an abstract data model and rules for producing and consuming representations. The data model is not identical to one JSON serialization.


Representation requirements are version-specific:


- The 2022 DID Core 1.0 Recommendation defines` application/did+json` and` application/did+ld+json` . Its JSON-LD representation begins with the base context` https://www.w3.org/ns/did/v1` .
- The DID Core 1.1 Candidate Recommendation consolidates the core media type to` application/did` . Its JSON-LD representation begins with` https://www.w3.org/ns/did/v1.1` .


Do not mix the media type or base context from one version with claims of conformance to the other. Pin the specification version that producers and consumers implement.


Implementers should negotiate and validate the representation explicitly. Signing arbitrary serialized JSON bytes without a defined canonicalization and securing mechanism is not equivalent to processing the DID data model correctly.


## **Verification methods and verification relationships**


A **verification method** describes how a proof can be checked. It requires:


- an` id` expressed as a DID URL;
- a` type` ;
- a` controller` ;
- verification material appropriate to that type.


Public material can be represented through a defined property such as` publicKeyJwk` or another form allowed by the verification-method suite. A JWK in a DID document must not include private key material. The same verification material must not be duplicated in multiple material properties within one method.


Defining a verification method does not authorize it for every purpose. Authorization comes from the five explicit verification relationships.


Relationship Intended proof purpose


` authentication` Authenticate as the DID subject through challenge-response or another accepted mechanism


` assertionMethod` Express claims, including signing a Verifiable Credential where the chosen securing mechanism uses it


` keyAgreement` Establish shared cryptographic material, often for encryption


` capabilityInvocation` Invoke an object capability


` capabilityDelegation` Delegate an object capability


A verifier must check the relationship required for the proof. A key listed only under` authentication` is not automatically authorized for credential assertions or key agreement.


Relationships can embed a complete verification method or reference one by DID URL. References improve reuse but require correct dereferencing and exact identifier comparison.


## **Services and service endpoints**


The optional` service` property can advertise ways to communicate with or interact with the DID subject. Every service entry has:


- a unique` id` ;
- a` type` ;
- a` serviceEndpoint` .


The endpoint may be a URI or another allowed structure. Service definitions are extensible, so applications must understand the chosen type.


Publishing an endpoint does not prove that its server, operator, transport, content, or destination is trustworthy.


The application must authenticate the resolved DID state through the method, validate the service type, apply URL and network security controls, and use an application protocol with its own security properties.


Public service endpoints can also create correlation. Reusing one endpoint across pairwise DIDs can defeat the privacy benefit of separate identifiers.


## **What a DID method defines**


DID Core supplies the common architecture. A conforming DID method defines the method-specific rules necessary to implement it, including:


- method name and method-specific identifier syntax;
- how a DID and initial DID document are created;
- how current state is read;
- how authorized updates are submitted and verified;
- how a DID is deactivated;
- how resolution communicates with the registry;
- how the authenticity and integrity of results are established;
- how key rotation, recovery, versioning, and history work;
- method-specific security and privacy considerations.


The verifiable data registry could be a distributed ledger, decentralized filesystem, peer-to-peer network, database, or another system. Architecture should be evaluated on governance, availability, authorization, privacy, cost, history, and attack resistance rather than the word “decentralized.”


### **Method selection criteria**


Before selecting a method, test:


- specification maturity and governance;
- resolver and library interoperability;
- update and deactivation authorization;
- key rotation and recovery;
- historical-state support;
- privacy and metadata leakage;
- registry availability and censorship risk;
- transaction or operating costs;
- cryptographic agility;
- migration and method failure.


Changing methods usually means introducing a new identifier and a trusted migration path.


## **DID resolution versus DID URL dereferencing**


**DID resolution** takes a DID and resolution options and returns:


1. DID resolution metadata;
2. a DID document, document stream, or no document;
3. DID document metadata.


The resolver uses the “read” operation defined by the method. DID Core defines abstract interfaces and common result concepts; method-specific communication and authentication remain with the method.


**DID URL dereferencing** takes a complete DID URL and returns:


1. dereferencing metadata;
2. the identified resource, if available;
3. content metadata.


Dereferencing may first resolve the base DID and then select a fragment, service, or external resource. It is not a synonym for resolution.


The separate[W3C DID Resolution specification](https://www.w3.org/TR/did-resolution/) develops detailed resolution and dereferencing algorithms. Its latest publication is a W3C Working Draft dated 24 July 2026, and DID URL dereferencing is marked as a feature at risk. It remains standards-track work rather than part of the 2022 DID Core 1.0 Recommendation, so pin the draft before claiming conformance.


### **Resolver trust and caching**


A resolver is a security and privacy boundary. It sees requested identifiers and may return stale or manipulated state. Evaluate:


- method-result verification;
- transport and resolver authentication;
- cache freshness and invalidation;
- version and time options;
- error handling and downgrade behavior;
- privacy leakage through lookups;
- behavior during registry or network failure.


## **DIDs and Verifiable Credentials**


DIDs and Verifiable Credentials are complementary specifications, not the same object.


A base DID may identify:


- a credential issuer;
- a credential subject;
- a holder.


A verification method used by a securing mechanism is instead identified by a DID URL, commonly a DID followed by a fragment such as` #key-1` .


The[W3C Verifiable Credentials Data Model 2.0](https://www.w3.org/TR/vc-data-model-2.0/) defines credentials, presentations, issuer, holder, subject, validity, status, schemas, and securing mechanisms. It does not require every identifier to be a DID.


When a DID is used for an issuer, a verifier might resolve the issuer’s DID, locate the proof’s verification method, and confirm that the method is authorized under` assertionMethod` . That cryptographic verification still does not establish:


- that every credential claim is true;
- that the issuer is trusted for that claim;
- that the credential is current or acceptable;
- that its status, schema, or evidence meets policy;
- that the credential subject is the person presenting it.


Those checks belong to the securing mechanism, status system, trust framework, presentation binding, and verifier policy.


## **Privacy and security considerations**


### **Avoid personal data in public documents**


DID documents can be widely replicated. Do not publish names, government identifiers, biometrics, credentials, or other personal data merely because the model is extensible. Encryption is not a durable answer for permanently public ciphertext.


### **Prevent correlation**


Pairwise or context-specific DIDs can reduce correlation only if other data is also separated. Reused keys, service endpoints, network identifiers, credential attributes, timing, and registry activity can link supposedly separate DIDs.


### **Rotate and recover keys**


Plan compromise before launch. Define update authorization, recovery controllers, threshold rules, rotation, old-key handling, and deactivation. Recovery authority needs separation and audit.


### **Validate proof purpose**


Check not only that a signature verifies, but that the verification method was authorized for the required relationship at the relevant time. Prevent substitution between authentication, assertion, agreement, and capability uses.


### **Handle history carefully**


A current DID document may no longer contain an old key. Verifying a historical proof can require a method-supported historical version and reliable evidence of the proof time. “Key not present now” and “proof was never valid” are not equivalent conclusions.


## **Common DID implementation mistakes**


### **Calling DID Core a blockchain specification**


DID Core is technology-neutral. Blockchains are one possible registry architecture.


### **Treating the DID as proof of legal identity**


A DID supports identifier control and cryptographic verification. Real-world identity binding needs separate evidence, issuer assertions, or a trust framework.


### **Using any listed key for any purpose**


Enforce the explicit verification relationship and the proof purpose.


### **Trusting service endpoints automatically**


Validate method state and apply application, transport, URL, and content security.


### **Assuming all DIDs are private or anonymous**


Registry activity, resolution, reused material, and services can expose durable correlation.


## **An implementation checklist**


Before production, confirm that:


- the selected DID Core version and DID method specifications are pinned;
- identifier and DID URL parsing uses standards-compliant URI handling;
- accepted representations and media types are explicit;
- every proof enforces the intended verification relationship;
- method results are authenticated rather than trusted from any resolver;
- caching, versioning, historical verification, and deactivation are tested;
- update, rotation, compromise, recovery, and migration have rehearsed procedures;
- public documents contain no unnecessary personal or correlating data;
- service endpoints receive separate application-layer security review;
- Verifiable Credential trust, status, schema, and presentation checks remain separate.


## **Where DIDs meet identity verification**


DIDs can identify subjects and verification material, but they do not perform identity proofing. A reusable identity system still needs reliable evidence and a governed decision before it issues or accepts claims. Didit’s[ID Verification](https://didit.me/products/id-verification) can supply identity evidence for such a decision, while[Reusable KYC](https://didit.me/products/reusable-kyc) supports reusing a prior verification across participating services and is listed as free.


That product adjacency does not imply that every Didit verification is a DID or that DID resolution replaces KYC. Current module rates are available on the[pricing page](https://didit.me/pricing) . Architecture should keep identifier control, identity evidence, credential issuance, presentation, and relying-party policy as separate trust boundaries.


## **Frequently asked questions**


### **What does the W3C DID specification define?**


It defines DID and DID URL syntax, a common data model, core DID-document properties, representations, method requirements, and abstract resolution and dereferencing interfaces.


### **Does every DID use a blockchain?**


No. A DID method can use a ledger, database, peer-to-peer system, decentralized filesystem, or another registry architecture.


### **What is the difference between a DID and a DID document?**


The DID is the identifier. The DID document is associated data that can describe controllers, verification methods and their purposes, services, and other defined properties.


### **What is the difference between resolution and dereferencing?**


Resolution obtains a DID document and metadata for a DID. Dereferencing obtains the resource identified by a complete DID URL, potentially after resolving its base DID.


### **Is a DID the same as a Verifiable Credential?**


No. A DID is an identifier. A Verifiable Credential is a tamper-evident, machine-verifiable set of claims under the VC data model and a securing mechanism. VCs can use DIDs but do not universally require them.


### **Does controlling a DID prove who a person is?**


No. It can prove control of cryptographic or method-specific authority associated with the DID. Binding that control to a legal or real-world identity requires additional evidence or trusted assertions.


### **Can a DID document contain private keys?**


No. DID documents contain public verification material or references. Private key material must not appear and must remain protected by the controller’s key-management system.


## **Primary references**


- [W3C Decentralized Identifiers (DIDs) v1.0](https://www.w3.org/TR/did-core/)
- [W3C Decentralized Identifiers (DIDs) v1.1](https://www.w3.org/TR/did-1.1/)
- [W3C Decentralized Identifier Resolution](https://www.w3.org/TR/did-resolution/)
- [W3C DID Specification Registries](https://www.w3.org/TR/did-spec-registries/)
- [W3C Verifiable Credentials Data Model v2.0](https://www.w3.org/TR/vc-data-model-2.0/)


DID Core is most useful when its claim stays precise. It standardizes identifiers, documents, verification purposes, services, and method interfaces. Trust still comes from the method’s governance, authenticated resolution, protected keys, explicit proof purpose, privacy-aware design, and the application’s decision about what evidence to accept.


Keep reading


## Related articles


- [PEP Screening: Definitions, Scope, and Monitoring](https://didit.me/blog/pep-screening-definitions-scope-monitoring/)
- [Enhanced Due Diligence (EDD): Compliance Guide](https://didit.me/blog/enhanced-due-diligence-edd-compliance-guide/)
- [MRZ Explained: Machine Readable Zone Technical Guide](https://didit.me/blog/mrz-machine-readable-zone-technical-guide/)
- [Age Verification Software: Buyer's Guide](https://didit.me/blog/age-verification-software-buyers-guide/)
- [Flutter SDK: Add Identity Verification to Your App](https://didit.me/blog/flutter-sdk-identity-verification-integration-guide/)
- [Adverse Media Screening: Process, Tuning, and Risks](https://didit.me/blog/adverse-media-screening-process-tuning-guide/)
