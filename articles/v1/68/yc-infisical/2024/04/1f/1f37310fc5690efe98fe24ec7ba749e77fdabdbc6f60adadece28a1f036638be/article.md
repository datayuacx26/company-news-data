---
schema_version: "1.0.0"
document_id: "1f37310fc5690efe98fe24ec7ba749e77fdabdbc6f60adadece28a1f036638be"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/infisical-update-march-2024"
published_at: "2024-04-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:cb6186b659b2242fde20ec495ce4ba8cc2e7c5f1e60307cdb3c09ceea838f561"
---

# Infisical Update – March 2024

March was an exciting month for[Infisical](https://infisical.com/) with many new product developments and announcements made. As always, we’ll be highlighting a few of the major updates in this post.


## Migration from MongoDB to PostgreSQL


Last week, we announced the completion of the PostgreSQL migration initiative which entailed careful planning and execution. The initiative entailed rewiring logic, re-writing queries, and migrating tens of millions of database records from[MongoDB](https://www.mongodb.com/) to[PostgreSQL](https://www.postgresql.org/) .


With the the completion of this milestone, the platform is now easier to self-host, faster, and more performant than ever.


You can read the official announcement[here](https://infisical.com/blog/postgresql-migration) and the fuller story of the decision-making and execution of the initiate[here](https://infisical.com/blog/postgresql-migration-technical) .


## New landing page


Last week, we also released a brand new website for Infisical which is now all-in on light-mode.


With this milestone, we’ve rearchitected the website to emphasize more on the security, versatility, and reliability of the platform.


Check out the website[here](https://infisical.com/) .


## Dynamic secrets


We launched support for[dynamic secrets](https://infisical.com/docs/documentation/platform/dynamic-secrets/overview) , a way to generate ephemeral database credentials on the fly. With dynamic secrets, developers and machines can avoid using static database credentials that can be exploited well after an accidental leak and instead rely on short-lived secrets with TTL.


Doing so means significantly increasing security posture by reducing the risk associated with credential usage in the event of a leak because the credentials will be defunct beyond the configured TTL.


Check out the documentation for dynamic secrets[here](https://infisical.com/docs/documentation/platform/dynamic-secrets/overview) .


## Permission Update


We’ve been working hard to make the permission system more versatile with the release of two new capabilities now implemented for users and machine identities at the project-level:


- Multiple roles: You can now assign multiple roles to a user or machine identity such that it inherits the composite permissions of all the roles that they have been assigned.
- Temporary access: You can now add a temporary role in conjunction with a permanent role to a user or machine identity such that the temporary one can expire on a certain date.


You can read more about Infisical’s role-based access controls[here](https://infisical.com/docs/documentation/platform/access-controls/overview) and specifically about temporary access[here](https://infisical.com/docs/documentation/platform/access-controls/temporary-access) .


## Scoped JWT tokens


One important behind-the-scenes initiative that we executed was the release of scoped JWT tokens.


In the past, when a user successfully logged into Infisical with a personal authentication method, they were issued a JWT token with access to any organization that the user was a member of. While this worked for a while, it inhibited the platform’s ability to allow unique resource slugs at the organization level. For example, you couldn’t have two projects named` project-alpha` across two different organizations, severely limiting the possible names that could be given for projects.


With this update, it is now possible to have two same project names as long as they belong to different organizations in Infisical.


## Air-gapped support for Infisical Self-Hosted EE


With more and more customers requesting support for using enterprise features in air-gapped/offline environments, we have now released the ability to unlock features in such environments using the new` OFFLINE_LICENSE_KEY` .


If you’re interested in obtaining an enterprise license or running a POC for Infisical, please feel free to reach out tosales@infisical.com or book a demo[here](https://infisical.com/schedule-demo) , and we’d be happy to assist.


## Other


Beyond the above updates, we shipped many smaller feature improvements and patches from revamped notifications with[react-toastify](https://github.com/fkhadra/react-toastify#readme) to furthering support for machine identities across our range of clients to improved documentation.


As always, we’re excited for all the upcoming feature releases ahead and can’t wait for you to try everything that we’ve shipped this past month.


Onward and upward!
