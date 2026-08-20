---
schema_version: "1.0.0"
document_id: "72b3077ea80f47d4de2ef091d208c4bce86087d48eac16b1d12dd51ed85f53e3"
company_key: "yc-oncactus"
company: "Cactus"
source_id: "yc-oncactus-rss-da464479d2a9"
canonical_url: "https://blog.oncactus.com/p/dropping-redis-for-rails-slashed"
published_at: "2024-12-09T22:46:00+00:00"
first_seen_at: "2026-07-27T00:17:06.713845+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:106c19ad24fff929324308db96bd6dc3742de3e23fe29735b15e5679636b5060"
---

# Dropping Redis for Rails Slashed Our AWS Bill by 62%

[Engineering](https://blog.oncactus.com/s/engineering/?utm_source=substack&utm_medium=menu)


# Dropping Redis for Rails Slashed Our AWS Bill by 62%


[Avinash Joshi](https://substack.com/@itsavinash)


Dec 09, 2024


Recently, we integrated Redis through AWS ElastiCache to power real-time updates in our


[AI-driven menu recommendation system](https://cravd.com/maya/overview) . Our initial


[cloud setup](https://world.hey.com/avinash/kamal-2-my-upgrade-journey-a1af9920#:~:text=A%20quick%20detour%20with%20my%20%22cloud%22%20setup) was straightforward: a Rails 8 application deployed with Kamal on a t3a.small EC2 instance ($27.07/month) and RDS for the database ($32.47/month), totaling around $70/month across environments.


# **Cost Surge**


What started as an "elegant solution" quickly became a concern. By July 2024, ElastiCache costs had skyrocketed to $120.53, pushing our total AWS expenses to $197.811. The graph clearly shows how ElastiCache dominated our infrastructure costs, accounting for over 60% of monthly expenses.


# **The Solution**


After successfully implementing


[Solid Queue](https://github.com/rails/solid_queue) and following the release of


[Solid Cable](https://github.com/rails/solid_cable) at


[Rails World 2024](https://rubyonrails.org/world/2024) , we pivoted to this database-backed alternative for real-time features. This eliminated our Redis dependency while maintaining full functionality. The data shows remarkable improvements:


-


Monthly AWS costs decreased from $197.81 to $74.55


-


Infrastructure costs reduced by approximately 62%


-


Complete elimination of the $120.53 monthly ElastiCache expense


# **Beyond the Numbers**


The switch to Solid Queue significantly reduced our infrastructure complexity. By eliminating Redis, we've simplified our stack, making it easier to maintain, debug, and scale. This change aligns with Rails' philosophy of convention over configuration, keeping things simple and streamlined. Following this success, we integrated


[Solid Cache](https://github.com/rails/solid_cache) , completing the Solid trifecta.


# **Key Takeaway**


This migration reinforces a valuable lesson: staying aligned with framework defaults often leads to both technical and financial benefits. Our tech stack is now simpler, more maintainable, and more cost-effective while maintaining all the functionality our application requires.


The cost breakdown graph demonstrates this transformation, showing sustained lower costs from October 2024 onward, with stable expenses around $75/month
