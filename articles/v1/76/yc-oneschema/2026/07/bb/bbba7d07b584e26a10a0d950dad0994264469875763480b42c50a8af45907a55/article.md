---
schema_version: "1.0.0"
document_id: "bbba7d07b584e26a10a0d950dad0994264469875763480b42c50a8af45907a55"
company_key: "yc-oneschema"
company: "OneSchema"
source_id: "yc-oneschema-news-import-43da02420b1d"
canonical_url: "https://www.oneschema.co/blog/introducing-azure-blob-storage-connector"
published_at: null
first_seen_at: "2026-07-24T07:26:45.232367+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:ebd18e66f4bfe1d81566a5c6ae991da8b391ab0ff295aa66a3d4663dcac7a9f3"
---

# Introducing: Azure Blob Storage Connector

Microsoft's footprint in enterprise data infrastructure is substantial and growing. Azure is the primary cloud for a significant share for consulting practices, particularly in financial services, healthcare, and the public sector, where Microsoft's compliance certifications and existing enterprise agreements make Azure the path of least resistance. For teams running data migrations in those environments, that means source data frequently lives in Azure Blob Storage: staging containers for ERP exports, data lake layers built on ADLS Gen2 (which runs on Azure Blob), or file drops from operational systems that write to Azure-native storage.


Until now, connecting those environments to MultiFileFeeds required a workaround — pulling files out of Azure through an intermediary or running a separate process to land them somewhere OneSchema could reach. Azure Blob Storage is now a native source and destination in MultiFileFeeds, completing cloud object storage support alongside S3 and GCS. Azure-native data stacks have the same direct integration path that AWS and GCP environments have had.


## **As a source**


Workflows can pull blobs from any container and prefix on a scheduled basis (hourly or daily) with an optional regex filter to scope exactly which files get ingested. A watermark and ingested-object ledger ensure each blob is processed once: subsequent runs pick up only new files, so there's no risk of reprocessing the same extract on the next scheduled pull.


For migration engagements where the client's source system drops refreshed extracts to Azure Blob on a regular schedule, this means the ingestion step is fully automated. The workflow runs, picks up what's new, and processes it — without a team member manually identifying and uploading the latest files


## **As a destination**


Workflow outputs can be pushed directly back to a container and prefix. The connector respects the storage account's existing versioning configuration on overwrite, so behavior aligns with whatever policy the client's Azure environment already enforces.


This matters for engagements where validated output needs to land back in the client's Azure environment, feeding a downstream Azure Synapse pipeline, populating a data lake layer, or delivering processed files to a container that another system reads from. The round-trip stays within Azure without requiring an export step.


## **Authentication**


The connector uses Workload Identity Federation exclusively. No connection strings, SAS tokens, or storage account keys are stored anywhere. Setup involves running a provided script to create a Microsoft Entra ID app registration with a federated identity credential that trusts OneSchema's OIDC issuer, granting Storage Blob RBAC at the appropriate level (Data Reader for source, Data Contributor for destination), and passing back the Client ID. OneSchema mints short-lived Azure AD tokens at runtime and refreshes them automatically.


For enterprise security teams which, in consulting engagements, are often the client's security team reviewing the integration — the absence of stored long-lived credentials is a meaningful difference. Workload Identity Federation is the authentication pattern Microsoft recommends for service-to-service access in Azure, and it removes the credential rotation burden that comes with static keys.


{{blog-content-cta}}
