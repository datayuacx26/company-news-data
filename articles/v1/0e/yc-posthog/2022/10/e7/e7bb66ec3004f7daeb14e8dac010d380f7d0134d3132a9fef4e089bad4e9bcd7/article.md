---
schema_version: "1.0.0"
document_id: "e7bb66ec3004f7daeb14e8dac010d380f7d0134d3132a9fef4e089bad4e9bcd7"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/what-is-sso-and-saml"
published_at: "2022-10-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:681bce227c5ba4ca6abf7e9ed4bdb6d4c84daf9c14130f4cfa1025f06717b38c"
---

# What is SSO and why you should enable it for PostHog

# What is SSO and why you should enable it for PostHog


Oct 31, 2022


- [Explainers](https://posthog.com/blog/explainers)


#### Contents


-
-
-
-


Existing at an intersection of convenience and security, single sign-on (SSO) authentication is used and appreciated by both IT teams *and* everyday users — which is why we strongly recommend setting it up on new PostHog instances. In this article we’ll explain what SSO and SAML are, and why you should use it.


> **Ready to get started?** Check[PostHog’s authentication docs](https://posthog.com/manual/sso)
>
>
> for instructions on setting up SSO and SAML.


##


What is SSO?


SSO stands for Single Sign-On. It enables users to login to services without entering a service-specific password — instead, access is validated by their access to a secondary system or domain. Ever logged into a website using your Google or Facebook account? *That’s* SSO.


Administrators and IT teams often encourage SSO for security purposes, as it ensures only authorized users can access an account. A simple username and password often isn't secure enough. We use SSO at PostHog for this reason and offer support for it via authorized email domains and certain third-party providers — namely Google, GitLab, and GitHub.


PostHog also offers[just-in-time provisioning](https://posthog.com/manual/sso#just-in-time-user-provisioning)


, which automatically creates a new account whenever new users access PostHog — provided that they have a valid email address and SSO provider, of course.


##


What is SAML?


SAML, on the other hand, stands for Security Assertion Markup Language. It’s one of the underlying technologies which makes SSO possible and enables team members to use a single set of login credentials across multiple systems. In PostHog, you can use multi-tenant SAML to support many authentication servers at once and integrate Identity Providers, such as OneLogin or Okta.


> Some SSO and SAML features are intended for use in large organizations and are only available on Enterprise or paid PostHog plans. For a comprehensive explanation of which features are available on each plan, refer to the[authentication docs](https://posthog.com/manual/sso)
>
>
> .


##


Why is SSO useful for teams??


SSO and SAML are typically useful for three main reasons…


- They make logging in to services easier for users (fewer passwords to remember)
- They make managing accounts easier for IT teams (fewer passwords to reset)
- They make it harder to engage in bad practices (fewer shared passwords)


Additionally, SSO and SAML may be required in some organizations which need to comply with regulations that govern how users are provisioned and access is tracked. For this reason, some SSO and SAML features are limited to users on paid or Enterprise plans.


##


How to set up SSO and SAML on PostHog


SSO is available for all versions of PostHog, including self-hosted deployments — however some SSO features, such as Google SSO, are limited to paid or Enterprise plans because they are intended for use in large organizations.


Likewise, SAML features are intended for use in large teams and therefore only available on Enterprise plans.


For more information about which SSO and SAML features are available for which PostHog plans, and instructions on how to set up third-party SSO providers, check[the PostHog SSO docs](https://posthog.com/manual/sso)


.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
