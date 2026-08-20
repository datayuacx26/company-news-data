---
schema_version: "1.0.0"
document_id: "7a42517a7173133c5ba5c56ffe3d871e6928c9649f59c64f853037f0d21d388c"
company_key: "yc-convoy-2"
company: "Convoy"
source_id: "yc-convoy-2-news-import-6ac8b1bcd379"
canonical_url: "https://www.getconvoy.io/changelog/google-sso-and-rbac"
published_at: null
first_seen_at: "2026-07-25T00:37:26.853065+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:da8cf21dfc848d9fa30fa0de51123dad4dc907952283c8ebe93cd1611f23b4d9"
---

# Google SSO & Role-Based Access Control

We released two features this week: Google SSO and a complete rewrite of how permissions work.


**Google SSO**


Google Workspace users can sign into Convoy using their Google account, eliminating the need for separate passwords.


Click "Sign in with Google" on the login page. The authentication flow redirects through Google and back to Convoy. First-time users will set up their organization name during the process. Existing accounts with the same email are automatically linked.


**Role-Based Access Control**


The old permissions setup worked for smaller teams but started breaking down as teams grew. We rebuilt it with five roles:


- **Instance Admin** — Controls everything across your Convoy instance
- **Organization Admin** — Runs the organization and all its projects
- **Billing Admin** — Handles billing only
- **Project Admin** — One project's settings and resources
- **Project Viewer** — View-only access to project data


This gives you granular control over who can access and modify resources in your Convoy instance.


Both features are available now. See the[Google SSO docs](https://www.getconvoy.io/docs/product-manual/google-sso) and[RBAC guide](https://www.getconvoy.io/docs/product-manual/rbac) for implementation details.
