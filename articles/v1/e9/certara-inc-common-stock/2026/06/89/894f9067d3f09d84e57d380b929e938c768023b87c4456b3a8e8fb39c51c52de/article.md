---
schema_version: "1.0.0"
document_id: "894f9067d3f09d84e57d380b929e938c768023b87c4456b3a8e8fb39c51c52de"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-news-import-bbe25235e5a2"
canonical_url: "https://www.certara.com/announcement/migrating-to-chemaxon-java-libraries-26-what-changed-and-how-to-adapt/"
published_at: "2026-06-17T16:07:34+00:00"
first_seen_at: "2026-07-22T21:45:04.765085+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:f9f7e156555549243f0549715b189b45b40cd4db284bcb059cb32cb593c53fe2"
---

# Migrating to Chemaxon Java Libraries 26: What Changed and How to Adapt

ShareShareShare


*June 17, 2026*


Chemaxon has released version 26 of its Java libraries, the first major release built on Java 21. This version involves a broad reorganization of the public API — hundreds of classes have moved or been renamed across nearly every functional area. Codebases that depend on Chemaxon JARs will require source-level changes before the upgrade compiles. This post summarizes why the changes were made, what specifically changed, and how to migrate efficiently.


## Why The Breaking Changes


The primary driver is Java version support. Oracle ended public support for Java 17 in September 2025, making a Java 21 upgrade necessary. Because a breaking change was already required, we used the opportunity to address a longer-standing problem: the existing package structure is incompatible with the Java Module System ([Project Jigsaw](https://openjdk.org/projects/jigsaw/) ).


The Java Module System requires that each package lives in exactly one JAR. Chemaxon’s pre-v26 libraries violated this constraint with split packages — the same package name spread across multiple JARs. This made it impossible to adopt the module system without restructuring package boundaries. Native platform capabilities increasingly available only inside the module system and working around them without it requires disabling platform integrity checks globally, which introduces security risk.


Two secondary goals accompanied the restructuring: consolidating the com.chemaxon.* namespace into chemaxon.* for consistency and removing rarely used classes that were not intended for external consumption.


## What Changed


### Maven artifact IDs


All Maven artifact IDs now carry a chemaxon- prefix. This means the version bump alone will not resolve dependencies — the artifact IDs in your pom.xml or Gradle build files must also be updated. Chemaxon now provides three top-level modules to simplify dependency management.


### Class relocations


This is where most of the migration work lies. The full table of moved and renamed classes is documented at[docs.chemaxon.com/latest/general_migration-guide-java-libraries-26.html](https://docs.chemaxon.com/latest/general_migration-guide-java-libraries-26.html#moved-or-renamed-classes) .


### Discontinued classes


A set of classes has been removed entirely. Notable removals include SOAP and XML-RPC service handlers (use RESTful services instead), several concurrent plugin execution wrappers, legacy MRecord I/O classes, and internal GUI plugin panels. Where a replacement exists, it is listed in the migration guide.


### What has not changed


**Public method signatures are unchanged.** If your code calls only public APIs, the class relocations are the primary change. The most widely used APIs — including Standardizer — are among those least likely to require modification. Pre-compiled binaries that previously needed only a JAR swap will need recompilation.


## How to Migrate


Chemaxon provides migration scripts that automate the bulk of import replacements. Two versions are available for download from the documentation page:


- chemaxon_java_migration_script_26.sh for Linux and macOS
- ChemaxonJavaMigrationScript26.ps1 for Windows (PowerShell)


Both scripts perform in-place modifications but create a backup first. Run the script without parameters to see usage information. The recommended sequence is:


- Update Maven or Gradle dependencies to version 26, including the renamed artifact IDs.
- Run the migration script against your source tree to replace relocated imports automatically.
- Address remaining compilation errors manually by consulting the full class mapping table in the migration guide.


## Looking Ahead to Version 27


Version 27 will add module-info.class descriptors to formally encapsulate non-public APIs. No further structural package changes are planned. Codebases that use JARs via the classpath rather than the module path will not be affected by this addition. For teams that cannot migrate immediately, Chemaxon’s Silicon LTS (25.4.x) release remains supported for at least two more years.


## Conclusion


The v26 migration is primarily a mechanical refactoring. Class names and package paths have changed, but the underlying behavior and all public method contracts are stable. The migration script handles the bulk of the work automatically, and the most widely used APIs require little to no modification.


For teams on a longer timeline, the Silicon LTS release (25.4.x) provides a stable, supported baseline for at least two more years, giving adequate runway to plan and execute the migration without disrupting ongoing development.


The full migration guide, class mapping tables, and migration scripts are available at[docs.chemaxon.com/latest/general_migration-guide-java-libraries-26.html](https://docs.chemaxon.com/latest/general_migration-guide-java-libraries-26.html) .


## About Certara


Certara accelerates medicines using biosimulation software, technology, and services to transform traditional drug discovery and development. Its clients include more than 2,600 biopharmaceutical companies, academic institutions, and regulatory agencies across 70 countries. Learn more at certara.com.


### Certara Contact


[Support team](https://chemaxon.freshdesk.com/support/home)


## You May Also Like


AllDiscovery


[Conditional activation of IL-12 through a Fibronectin-EDB dependent switch gate](https://www.certara.com/publication/conditional-activation-of-il-12-through-a-fibronectin-edb-dependent-switch-gate/)


[Conditional activation of IL-12 through a Fibronectin-EDB dependent switch gate](https://www.certara.com/publication/conditional-activation-of-il-12-through-a-fibronectin-edb-dependent-switch-gate/)[Publication](https://www.certara.com/category/publication/)


### [Conditional activation of IL-12 through a Fibronectin-EDB dependent switch gate](https://www.certara.com/publication/conditional-activation-of-il-12-through-a-fibronectin-edb-dependent-switch-gate/)


[A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)


[A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)[Poster](https://www.certara.com/category/poster/)


### [A novel mathematical framework to predict in vivo bispecific GTX-B001 binding in skin, and translation to humans](https://www.certara.com/poster/a-novel-mathematical-framework-to-predict-in-vivo-bispecific-gtx-b001-binding-in-skin-and-translation-to-humans/)


[Certara Brings Chemaxon Cheminformatics Natively to Python with New Python API](https://www.certara.com/announcement/certara-brings-chemaxon-cheminformatics-natively-to-python-with-new-python-api/)


[Certara Brings Chemaxon Cheminformatics Natively to Python with New Python API](https://www.certara.com/announcement/certara-brings-chemaxon-cheminformatics-natively-to-python-with-new-python-api/)[Announcement](https://www.certara.com/category/announcement/)


### [Certara Brings Chemaxon Cheminformatics Natively to Python with New Python API](https://www.certara.com/announcement/certara-brings-chemaxon-cheminformatics-natively-to-python-with-new-python-api/)
