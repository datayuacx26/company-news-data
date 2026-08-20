---
schema_version: "1.0.0"
document_id: "1efd4636f234e6c19112dea530265c41db3ef662f8f0390e1b3a0499090ebe47"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/postgresql-migration"
published_at: "2024-03-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T22:26:05.625622+00:00"
content_hash: "sha256:868cf15376ff3aa52eb6a8a33cd5a0da2e850f4400785c7663bc9b5ef6e3d4d0"
---

# Migration to PostgreSQL

When we started[Infisical](https://infisical.com/) , our goal was to build the platform with a stack that aligned with our strategy and felt most familiar with the team. As part of that stack, we chose[MongoDB](https://www.mongodb.com/) +[Mongoose ORM](https://mongoosejs.com/) because the combination presented least overhead and allowed us to ship quality features quickly. We were also more focused on building[Infisical Cloud](https://app.infisical.com/) , the managed SaaS offering. As time passed, however, we found that many organizations, especially ones operating sophisticated environments at the intersection of compliance and security, preferred self-hosting the solution to keep data on their own infrastructure.


In the past year, we found ourselves shipping many features catered to reducing the learning curve needed to self-host Infisical such as compacting Infisical into a single Docker image, reducing the required environment variable count, and more. In our search to radically improve self-hosting, we ran into constraints around the capabilities and usability of MongoDB like the lack of support for transactions, clean-up, inconsistent versioning across managed offerings by cloud providers, not to mention issues associated with schema-less database design structure. All in all, we came to the realization that a full database migration to something more universal was the ultimate feature needed to make Infisical more accessible to teams and organizations around the world.


With that, we're announcing the deprecation of the MongoDB storage backend and migration to[PostgreSQL](https://www.postgresql.org/) for Infisical.


## Why PostgreSQL?


In our search for a new database, we found PostgreSQL to be the most favorable choice. Beyond having a vibrant community, extensive documentation, and a myriad of solutions and extensions available, we appreciated most its open source nature and how the vast majority of cloud providers offered managed services of PostgreSQL.


Above all, this meant that users of Infisical could more easily self-host our platform on any cloud provider and pair it with its corresponding managed PostgreSQL service. Moreover, given how widely-adopted the database has become, we were confident that users would have less trouble operating it when using Infisical.


## How can I use Infisical with PostgreSQL and what are the deprecation details?


Firstly, if you’re using Infisical Cloud, then no action is needed on your behalf since we’ve migrated the underlying database from MongoDB to PostgreSQL. That said, if you’ve been self-hosting Infisical with MongoDB, then we’ve put together a set of migration instructions spanning preparation, migration, and post-migration steps that you can easily follow along[here](https://infisical.com/docs/self-hosting/guides/mongo-to-postgres) . If you’re otherwise new to self-hosting Infisical, then you can check out the documentation that’s been updated to reflect PostgreSQL[here](https://infisical.com/docs/self-hosting/overview) . Overall, we highly recommend switching to PostgreSQL because new features to Infisical will be pushed there.


In terms of the deprecation timeline, we’re giving a window of 2 months starting hereon until May 25th 2024. During this grace period, we’ll be available to assist with any related migration questions, prioritizing paid and enterprise customers first; we’ll also push out minor patches where applicable. After this period, we will halt support for Infisical with MongoDB and consider this version deprecated.


As always, you can get in touch at the Slack community[here](https://infisical.com/slack) or via dedicated channels if on enterprise.


Toward a better future!
