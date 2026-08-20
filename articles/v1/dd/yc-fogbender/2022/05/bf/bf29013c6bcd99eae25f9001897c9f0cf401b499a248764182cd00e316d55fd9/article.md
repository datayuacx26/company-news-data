---
schema_version: "1.0.0"
document_id: "bf29013c6bcd99eae25f9001897c9f0cf401b499a248764182cd00e316d55fd9"
company_key: "yc-fogbender"
company: "Fogbender"
source_id: "yc-fogbender-news-import-46cf0bf99c36"
canonical_url: "https://fogbender.com/blog/fogbender-jira-integration"
published_at: "2022-05-02T00:00:00+00:00"
first_seen_at: "2026-07-21T20:41:04.212025+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:db42156b6494a66b278e35d3a100e9157965a050a6023479b841ba73865798d9"
---

# How to configure the Fogbender Jira integration

Once configured, the Fogbender Jira integration makes it possible to file Jira tickets directly from a conversations with a customer team.


To configure the integration, go to the Settings section of your Fogbender workspace:


Under Integrations, click the “ADD INTEGRATION” button and select “Jira” from the dropdown.


1. **Jira URL →** For example, if your Jira issue dashboard is located at


```text
https://alan217.atlassian.net/jira/software/c/projects/AT0/issues/?filter=allissues
```


your Jira URL is


```text
https://alan217.atlassian.net
```


2. **Jira user →** We recommend creating a new non-admin Jira account for this integration. One option is to create an email group such asjira@yourcompany.domain , and use this email for the new Jira account.


3. **Project key →** For the Jira URL above, the project key is


```text
AT0
```


4. **API token →** Once the new Jira account is created, sign in with the new account and create an API token here:[https://id.atlassian.com/manage-profile/security/api-tokens](https://id.atlassian.com/manage-profile/security/api-tokens)


5. **Webhook URL →** You need admin privileges to configure webhooks, so sign in with your admin Jira account and follow the URL in step 5.


Add` labels = fogbender` in the JQL query input and select all options under **Issue** and **Comment** .


Then, click “Create” at the bottom of the page.
