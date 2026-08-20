---
schema_version: "1.0.0"
document_id: "5f0923a758a69862557a484b95c38df766d176e850b86eefcae057895d5a6e4c"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-cognito-password-hash-import/"
published_at: "2026-07-15T17:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:a796873388ad0950e7d52b5a237687de7963bda2552b34fe401c053c2cd01888"
---

# Amazon Cognito now supports importing users with password hashes

Amazon Cognito now supports importing users with password hashes in CSV user imports. Previously, users imported from a CSV file had to reset their passwords on first sign-in. Now, you can include password hashes in your CSV file so that imported users can sign in immediately with their existing credentials.


When creating a CSV import, you specify the password hashing algorithm used by your source system. Amazon Cognito imports these users and verifies their password against the imported hash on first sign-in. Supported algorithms include bcrypt, scrypt, Argon2id, and PBKDF2 with SHA-256. All imported hashes receive an additional layer of cryptographic protection before storage.


Password hash import is available in all AWS Regions where Amazon Cognito is available. To get started, create a user import using the[AWS Management Console](https://console.aws.amazon.com/cognito/home) , AWS Command Line Interface (CLI), or AWS Software Development Kits (SDKs). See the[developer guide](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-using-import-tool.html) for instructions.
