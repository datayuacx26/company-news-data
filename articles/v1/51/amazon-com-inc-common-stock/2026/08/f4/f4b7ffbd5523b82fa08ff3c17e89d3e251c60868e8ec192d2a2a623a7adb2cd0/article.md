---
schema_version: "1.0.0"
document_id: "f4b7ffbd5523b82fa08ff3c17e89d3e251c60868e8ec192d2a2a623a7adb2cd0"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/secrets-manager-integration-jenkins-sonarqube/"
published_at: "2026-08-11T14:57:00+00:00"
first_seen_at: "2026-08-11T22:31:48.620839+00:00"
fetched_at: "2026-08-11T22:31:51.421235+00:00"
content_hash: "sha256:e5328b160c5119668bf11521885be8ab592d948b3f40cab75a332954a8257606"
---

# AWS Secrets Manager adds managed external secrets support for Jenkins and SonarQube

AWS Secrets Manager now extends its managed external secrets capability to include Jenkins API Tokens and SonarQube Tokens, enabling you to automatically rotate these third-party credentials directly from the AWS console without writing any custom rotation code.


For Jenkins, Secrets Manager mints a new token and revokes the old one only after the replacement is verified active, so your continuous integration and continuous delivery (CI/CD) jobs transition without interruption. Rotation supports both self-rotation, where the token being rotated authenticates its own replacement, and admin-assisted rotation, where a separate admin token performs the generate and revoke operations. For SonarQube, you can rotate three types of tokens — User Tokens, Global Analysis Tokens, and Project Analysis Tokens — via SonarQube's Web API. User Tokens support self-rotation, while analysis tokens are rotated using an admin token.


These integrations join existing managed external secrets support for BigID, Confluent Cloud, Datadog, GitLab, MongoDB Atlas, Okta, Paddle, Salesforce, and Snowflake.


Jenkins and SonarQube managed external secrets are available in all AWS Regions where AWS Secrets Manager managed external secrets is supported. To learn more, visit the[AWS Secrets Manager managed external secrets documentation](https://docs.aws.amazon.com/secretsmanager/latest/userguide/managed-external-secrets.html) .
