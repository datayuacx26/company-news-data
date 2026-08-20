---
schema_version: "1.0.0"
document_id: "6187034bacfc32384811ca61081c4b9696b468cd4d5151d48414bced0848e1ed"
company_key: "yc-codenow"
company: "CodeNow"
source_id: "yc-codenow-news-import-a318b6d3535f"
canonical_url: "https://www.codenow.com/blog/release-notes-codenow-7-8---october-2024"
published_at: "2024-10-01T00:00:00+00:00"
first_seen_at: "2026-08-09T20:50:58.691009+00:00"
fetched_at: "2026-08-09T20:50:59.843294+00:00"
content_hash: "sha256:19470a84753d7719fc3e0446c3c12da0308e50fb8bc793fdfacc63b052ed9ac3"
---

# Release Notes CodeNOW 7.8 – October 2024

Release Notes


October 1, 2024


# Release Notes CodeNOW 7.8 – October 2024


Latest updates and improvements in CodeNOW 7.8 release.


## Create Component Redesign


CodeNOW is overhauling its Create Component interface to deliver "a more streamlined and flexible experience when setting up Git providers and repositories."


### Git Provider Selection


The updated design features an intuitive interface for choosing Git providers like GitLab and GitHub, complete with visual indicators and descriptive text to guide users through selection.


### Monorepository Support


The redesigned component now provides enhanced compatibility for monorepos, enabling teams to manage multiple services within a single repository. Users can configure individual services with dedicated pipelines, branches, and environment variables.


### Repository Options: Link Existing or Create New


**Link Existing Repository:** Users can browse and connect existing repositories from their Git provider, with auto-populated details like branches and permissions.


**Create New Repository:** The component allows direct repository creation through CodeNOW, integrating with the Git provider's API for seamless setup.


### Enhanced User Experience


The workflow prioritizes efficiency by reducing unnecessary steps. Contextual tooltips and explanations accompany each setting, minimizing reliance on external documentation.


## Support for New External Git Providers


CodeNOW now enables connections to both GitLab.com and self-hosted GitLab instances.


### Key Features


- **GitLab.com Integration:** Users can seamlessly connect their GitLab.com accounts to CodeNOW
- **Self-Hosted GitLab Support:** Organizations can link private or on-premises GitLab infrastructure
- **Streamlined Connection Process:** Authentication occurs via personal access tokens
- **Repository Hosting:** Connected repositories can host application components within the platform


## HTTPS Redirects in Custom Domains


A new optional setting enables automatic HTTPS redirection for custom domains, ensuring encrypted data transfer between clients and servers.


### Key Features


- **Optional HTTPS Redirection:** Users can toggle automatic HTTP-to-HTTPS redirects
- **Enhanced Security:** Encryption protects data during transmission
- **Simple Configuration:** Domain settings allow quick enablement or disablement


## Better Handling of Application Component Default Configuration


When default configuration storage fails during release, CodeNOW now marks releases as incomplete and offers a repair option.


### Key Features


- **Automatic Incomplete Release Status:** Errors trigger an incomplete flag for early detection
- **Repair Option:** Users can retry configuration storage without restarting the entire process
- **Improved Error Visibility:** Clear messages guide users through repairs


## Update Scaffolders


Scaffolders now default to Long-Term Support (LTS) technology versions with enhanced container security.


### Key Features


- **LTS Technology Stack:** Improved stability and long-term support for critical components
- **Enhanced Container Security:**


- Non-root user execution by default
- Root user access disabled
- Privilege escalation prevention


Written by CodeNOW
