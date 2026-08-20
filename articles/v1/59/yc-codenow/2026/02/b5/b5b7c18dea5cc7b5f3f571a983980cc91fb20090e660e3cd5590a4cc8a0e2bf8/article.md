---
schema_version: "1.0.0"
document_id: "b5b7c18dea5cc7b5f3f571a983980cc91fb20090e660e3cd5590a4cc8a0e2bf8"
company_key: "yc-codenow"
company: "CodeNow"
source_id: "yc-codenow-news-import-a318b6d3535f"
canonical_url: "https://www.codenow.com/blog/release-notes-codenow-7-17---february-2026"
published_at: "2026-02-17T00:00:00+00:00"
first_seen_at: "2026-08-09T20:50:58.691009+00:00"
fetched_at: "2026-08-09T20:50:59.843294+00:00"
content_hash: "sha256:9136ea40e60891b75daad22ad3b04d4e94123002d3f35b027b80bbefcb495c42"
---

# Release Notes CodeNOW 7.17 – February 2026

Release Notes


February 17, 2026


# Release Notes CodeNOW 7.17 – February 2026


CodeNOW 7.17 introduces Container components for building reusable Docker images and Vibe Coding Sessions for AI-assisted development directly in the cloud.


## Container Component


Containers in CodeNOW let you build and publish reusable Docker images that other components can reference as base images. Unlike application components, containers are not independently deployed — they are built, released, and then consumed through a` FROM` directive in other components' Dockerfiles.


### Key Capabilities:


- **Create and Manage Containers:** Establish new containers with name and Git configuration, browse, search, and filter accessible containers across the account
- **Repository Access:** Open or clone the Git repository for local development
- **Release Management:** Build and publish new container versions with full version control
- **Setup Guidance:** Retrieve instructions for referencing container images in Dockerfiles
- **Merge Request Handling:** Create and review merge requests following standard CodeNOW workflows
- **Permission Control:** Manage viewing, development, and administrative access per container
- **CI Configuration:** Configure providers, build settings, and triggers
- **Lifecycle Management:** Permanently remove containers and associated repositories when no longer needed


### Benefits:


- Reusable, versioned Docker images reduce duplication across application components
- Centralized base image management ensures consistency and simplifies updates
- Faster CI pipelines with purpose-built build images
- Clear separation between infrastructure images and deployed services
- Standard CodeNOW permission model and Git workflows apply to containers


## Vibe Coding Session


Vibe Coding Session is an AI-assisted coding environment that runs in a dedicated container on one of the account's data planes. It eliminates the need for local development setups by allowing developers to work using natural language prompts directly in the cloud.


### Key Capabilities:


- **AI-Assisted Development:** Generate new code from natural language feature descriptions, refactor existing code, and get explanations of complex logic
- **Cloud-Based Environment:** No local setup required — everything runs in a dedicated container on the account's data plane
- **Full Project Context:** The AI assistant has access to all component repositories within the application
- **Bug Identification and Fixes:** Describe issues in natural language and let the AI suggest or implement fixes
- **Test Generation:** Automatically generate tests for existing functionality
- **Secure Access:** Authenticated via SSH keys configured in the user's Keys & Tokens profile


### Benefits:


- Dramatically lower barrier to entry for new team members — no local environment setup needed
- Accelerated development through AI-assisted code generation and refactoring
- Full integration with CodeNOW Git workflows — commits trigger builds and deployments automatically
- Secure, ephemeral containers with persistent Git commits ensure no work is lost
- Application-wide access to all component repositories in a single session


Written by CodeNOW
