---
schema_version: "1.0.0"
document_id: "d94a7c1a1541ef89a4b3e544b451fca80ee9e7835bccee93499535259bddf6d4"
company_key: "workday-inc-class-a-common-stock"
company: "Workday Inc."
source_id: "workday-inc-class-a-common-stock-rss-1edd291cea4c"
canonical_url: "https://medium.com/workday-engineering/facing-data-fragmentation-and-high-costs-large-organizations-require-an-universal-data-layer-b984a82decb5"
published_at: "2026-02-03T16:10:02+00:00"
first_seen_at: "2026-07-20T04:35:52.231186+00:00"
fetched_at: "2026-08-20T00:44:57.479297+00:00"
content_hash: "sha256:5d468d274e4f38a93892870359865a0d6fd4402136c0fc9e602501b6a23f7ee0"
---

# Facing data fragmentation and high costs, large organizations require an Universal Data Layer

### The Universal Data Layer: Apache Iceberg as the Foundation for Agentic AI and Interoperability


*Facing data fragmentation and high costs, large organizations require an Universal Data Layer (UDL). This article validates* ***Apache Iceberg*** *as the optimal open-source table format for this foundation. Iceberg provides ACID compliance and seamless schema evolution, ensuring both* ***unparalleled interoperability*** *across diverse compute engines and* ***Reliable State Synchronization*** *for autonomous AI and Multi-Agent Systems (MAS). By enabling MAS orchestration and serving as the persistent knowledge base for AI agents, Iceberg transforms data architecture into a unified, intelligent foundation for competitive advantage.*


ByPhoenix Majumder with contributions byVaibhav Sharma andAchal Anjaria


**Introduction**


The Universal Data Layer as the Foundation for Agentic AI: **** Large organizations utilizing diverse data consumption platforms — ranging from legacy data warehouses to modern machine learning and AI environments — are frequently challenged by data fragmentation. This fragmentation results in inconsistent context sharing, high operational costs associated with maintaining complex Extract, Transform, Load (ETL) pipelines, and unreliable decision-making. The implementation of a Universal Data Layer (UDL) is strategically necessary to resolve this complexity. UDL is not an official, globally recognized industry standard, but it is a widely used and accepted term. It describes a best practice for standardizing how data is collected and managed across various digital properties. It’s a concept not a technology, to solve for data availability needs.


Apache Iceberg is recognized as the premier open-source table format designed to facilitate this shift. Iceberg transforms commodity cloud storage (data lakes) into a reliable, database-like platform by providing database capabilities for massive datasets. Its core value proposition lies in delivering ACID compliance, enabling seamless schema evolution, and offering performance optimizations previously reserved for traditional data warehouses.


This article confirms Iceberg’s viability as the enterprise UDL based on two foundational capabilities: 1) The provision of unparalleled Interoperability via engine-agnostic metadata management; and 2), The establishment of Reliable State Synchronization, which is essential for the robust operation of autonomous Artificial Intelligence (AI) and Multi-Agent Systems (MAS). Iceberg is explicitly recognized as the optimal choice for organizations prioritizing flexibility and multi-engine support in large-scale, cloud-native environments.


**The Strategic Imperative: Architecting Against Data Fragmentation**


Limitations of Traditional Architectures: Traditional data lake architectures, often relying on file-system metadata (e.g., Hive), suffer from severe limitations. Data identity is tied directly to the physical folder structure, leading to inconsistent views and high dependency on specific compute engines. The necessity of conforming and copying data across multiple silos requires complex, expensive, and fragile ETL pipelines, significantly increasing operational overhead.


Defining the Universal Data Layer (UDL): A modern UDL must provide a single, reliable source of truth decoupled from any single compute engine. To achieve this, a UDL must provide five key guarantees: transactional integrity (ACID), high-performance analytics, seamless interoperability across engines, robust governance, and future-proof architectural flexibility.


The Iceberg Advantage in a Vendor Landscape: Apache Iceberg’s strategic position is defined by its commitment to vendor neutrality. While competing formats like Delta Lake excel within specific ecosystems, Iceberg is best suited for environments prioritizing compatibility across diverse compute stacks. This openness is a critical strategic leverage point, ensuring data portability and maintaining competitive pressure on cloud and compute vendors.


**Table 1: Comparison of Open Table Formats for UDL Adoption**


**Apache Iceberg: Architectural Validation of the Universal Data Layer**


Core Metadata Architecture and Transactional Guarantees: The technical viability of Iceberg as a UDL rests on its intelligent metadata architecture, which separates data (e.g., Parquet files) from the metadata that defines the table’s identity.


- Metadata File: The starting point, containing the table schema, partition spec, and a pointer to the current Manifest List.
- Manifest List: A list of Manifest Files that are part of a specific snapshot, with summary statistics for efficient pruning.
- Manifest Files: Contains detailed information about each data file (e.g., path, partition data, column-level stats).


This architecture enables efficient file pruning. Query engines read the small metadata files to identify exactly which data files are relevant, avoiding full table scans.


**Figure 1: Iceberg’s Metadata Architecture for ACID Transactions**


Iceberg’s layered metadata architecture ensures ACID transactions by tracking table state through immutable snapshots, each pointing to a manifest list that details the exact set of data files.


**Advanced Features for Enterprise Reliability**


Seamless Schema Evolution: Allows for adding, dropping, or renaming columns without costly data rewrites.


Time Travel and Versioning: Provides the ability to query data as it existed at any point in the past, which is invaluable for auditing and debugging.


**Enabling True Interoperability: Practical Architecture for Multi-Platform Access**


The primary goal of the UDL is to decouple compute from storage, allowing heterogeneous processing engines to access data simultaneously and consistently.


**Figure 2: Universal Data Layer enabling Multi-Platform Interoperability**


The Iceberg UDL acts as a universal interface, enabling diverse compute engines to read from and write to the same set of tables on object storage without data duplication, ensuring consistency and interoperability.


**Table 2: Practical Implementation Tips for Iceberg Interoperability**


**The Integrated Data Catalog: Governing the Universal Data Layer**


The utility of a UDL is maximized only when integrated with a sophisticated data catalog that handles version control, governance, and compliance.


- The Necessity of the Catalog Layer: A REST-based catalog service (such as Project Nessie) promotes a modern, decoupled architecture.
- Git-like Data Versioning: Provides transformative agility, allowing teams to create zero-copy branches for experimentation and safely merge changes via Write-Audit-Publish (WAP) patterns.
- Compliance and Auditing Synergy: The combination of Iceberg’s Time Travel (data history) and Nessie’s catalog versioning (schema history) provides an immutable record for audits, allowing organizations to revert the entire data library to a consistent prior state if needed.


**The UDL as the Persistent State Layer for Agentic AI**


The viability of Iceberg extends beyond traditional analytics; it serves as the essential, reliable knowledge base required to support autonomous AI agents and Multi-Agent Systems (MAS). For agents, Iceberg’s versioned tables act as persistent long-term memory, while embeddings or vector caches provide short-term semantic recall.


Together, they enable agents to maintain situational awareness, retrieve relevant historical context, and reason over evolving system state without exceeding LLM context limits.


The Paradigm Shift: Bringing AI Agents to the Data


The traditional approach of moving data to AI agents is slow, costly, and insecure. The UDL enables the principle of bringing AI agents to the data, where agents run as code within compute engines that are co-located with the data. This minimizes latency, reduces cost, and enhances security.


Orchestrating Multi-Agent Systems through State Change


MAS are best orchestrated using an Event-Driven Architecture (EDA). Iceberg provides the durable, versioned record of the system’s state change that drives Agent-to-Agent (A2A) communication.


- An agent completes an action, committing the resultant state change to an Iceberg table atomically.
- This successful commit triggers an event in an EDA platform (e.g., Kafka).
- Subsequent agents, notified of the event, access the new, consistent Iceberg snapshot to begin their task.


Iceberg’s Snapshot Isolation guarantees that all agents operate on a consistent view of the world, which is critical for accurate coordination.


**Figure 3: Agent Orchestration via the UDL and Event-Driven Architecture**


Agent-to-Agent communication is driven by state changes in the UDL. An agent commits a change (1,2), which triggers an event (3). The event notifies the next agent (4), which then reads the consistent state from the UDL (5) to perform its task (6).


**Table 3: Interlocking Iceberg UDL Properties with MAS Requirements**


**Iceberg Implementation: The Workday Example**


For organizations leveraging major Enterprise Resource Planning systems, such as Workday, the **Universal Data Layer** becomes critical for democratizing this high-value data. The implementation involves the following approach:


- **Data Pipeline Standardization:** Structured data extracted from source systems (via APIs or exports) is standardized and written directly into the UDL as Apache Iceberg tables on commodity cloud storage. This replaces complex, point-to-point data pipelines with a unified data asset.
- **Governance Integration:** An integrated catalog service is implemented to apply Git-like version control and seamless schema evolution to the Workday tables. This ensures data lineage and compliance are maintained even as data model changes.
- **Compute Decoupling:** The Iceberg tables serve as the single source of truth, enabling any compute engine — from finance BI tools to machine learning environments — to access the data simultaneously and consistently, thus ensuring unparalleled interoperability.Implementing the UDL successfully requires a systematic, three-pronged approach focused on modernization, governance, and decoupling:1. Data Pipeline Standardization: From Complexity to Unified Assets
- **Extraction and Normalization:** Structured data is meticulously extracted from core source systems (like Workday’s Financial Management, HCM) through efficient APIs or bulk export mechanisms. This raw data is then standardized — cleaned, validated, and normalized — to ensure consistency across the organization.


**The Iceberg Standard:** Instead of writing this standardized data into proprietary formats or generic data lakes, it is immediately written into the UDL as open **Apache Iceberg tables** residing on cost-effective, scalable cloud object storage (e.g., S3, ADLS, GCS). This replaces a myriad of customized connectors and transformations with a single, unified, and self-describing data asset. Iceberg’s table format abstracts away the underlying file organization, providing a high-performance, future-proof, and completely open foundation for all subsequent data operations.


**Figure 4: Illustration of of UDL Implementation Phases**


**Governance Integration, Ensuring Trust, Lineage, and Evolution:** A robust UDL is useless without trust. This step integrates enterprise-grade governance directly into the data layer, ensuring compliance, reliability, and auditability.


- **Integrated Catalog Service:** A centralized catalog service (e.g., Apache Nessie or similar) is implemented specifically to manage the Iceberg tables. This catalog provides essential functionality:
- **Git-like Version Control:** Every transaction — from a simple row update to a massive schema change — is tracked and versioned. This “time travel” capability allows users to query data as it existed at any previous point in time, which is invaluable for regulatory reporting, auditing, and debugging data pipelines.
- **Seamless Schema Evolution:** ERP data models, especially in systems like Workday, change over time. Iceberg natively handles non-destructive schema changes (adding, renaming, or reordering columns) without requiring a disruptive migration or rewriting of existing data files. This ensures that downstream applications remain stable even as the source data model evolves.


**Compute Decoupling: Unleashing Interoperability and AI:** The final, critical step unlocks the power of the UDL by separating the data storage from the compute engines that process it.


- **The Single Source of Truth:** The standardized Iceberg tables become the *single source of truth* for all analytical and operational workflows derived from the Workday data.
- **Unparalleled Interoperability:** Because the Iceberg table format is open and API-driven, it enables a heterogeneous ecosystem of compute engines to access the same data simultaneously and consistently. This eliminates data silos and expensive data movement.
- **Financial Reporting & BI:** Tools like Tableau, Power BI, or modern BI platforms can query the tables directly via engines like Trino or Spark.
- **Machine Learning and Data Science:** Data scientists can use Python/Pandas, TensorFlow, or PySpark environments to access the data for training advanced AI models.


**Foundation for Agentic AI:** This high-quality, version-controlled, and universally accessible data layer is the prerequisite for deploying Agentic AI. Agents require consistent, real-time access to the most accurate enterprise data (like transaction history, HR records, or spending patterns) to perform complex reasoning, automate decisions, and interact reliably with business processes. The UDL powered by Iceberg provides the necessary robust, governed, and performant foundation for these intelligent applications.


---


[Facing data fragmentation and high costs, large organizations require an Universal Data Layer](https://medium.com/workday-engineering/facing-data-fragmentation-and-high-costs-large-organizations-require-an-universal-data-layer-b984a82decb5) was originally published in[Workday Technology Blog](https://medium.com/workday-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
