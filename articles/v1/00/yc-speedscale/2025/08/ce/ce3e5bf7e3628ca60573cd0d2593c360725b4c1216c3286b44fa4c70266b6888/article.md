---
schema_version: "1.0.0"
document_id: "ce3e5bf7e3628ca60573cd0d2593c360725b4c1216c3286b44fa4c70266b6888"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/back-in-india-for-kubecon-2025-highlights-from-hyderabad-part-2/"
published_at: "2025-08-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:8080aedc4894b4ff071236d6238d42fdfd610ec37a6280dda375ff299abc779b"
---

# Back in India for KubeCon 2025: Highlights from Hyderabad — Part 2

After an eventful week of customer visits and reconnecting with partners, the centerpiece of my trip was KubeCon in Hyderabad. Walking into the venue, the energy was unmistakable, with halls filled with engineers, community leaders, and companies all pushing the boundaries of what’s possible with Kubernetes and cloud-native technologies.


One of the most memorable sessions was led by Pega Systems, which presented on “Containerized Time Travel: Replicating Production Performance” ([link to session](https://kccncind2025.sched.com/event/23Ev9/containerized-time-travel-replicating-production-performance-sravanthi-naga-hari-babu-volli-pegasystems?iframe=yes&w=100%25&sidebar=yes&bg=no) ). The room was packed, with attendees standing along the walls to catch a glimpse.


Pega shared how they use Speedscale to record production traffic and replay it in controlled environments. When they experienced a major incident in one of their systems, they took the actual traffic from that event and replayed it in another environment. Their demo centered on a **banking application where user logins were failing** , caused by **high user volume combined with unique login data** that stressed the system in unexpected ways. By replaying the failing login traffic, they were able to pinpoint the root cause with the help of Grafana dashboards and Jaeger traces. The session resonated strongly with the audience because it showed not just theory but a concrete example of how replaying traffic can accelerate incident analysis and prevent future outages.


The entire Pega team was there to support their colleagues.


The expo floor was another highlight, **packed with lines for demos and filled with people thirsting for best practices** . I found myself in conversations that ranged from advanced load testing strategies to the nitty-gritty of securing APIs in high-traffic systems. What stood out most was the openness of the community, with everyone eager to share lessons learned and collaborate on what’s next.


Seeing the growth of the conference in India, the scale of participation, and the enthusiasm of younger engineers reinforced just how dynamic this ecosystem is. It’s not just about tools or infrastructure; it’s about a shared mission to make software more resilient, scalable, and reliable.


As I boarded my flight back to the U.S., I couldn’t help but reflect on how much has changed since my last trip to India. The pace of innovation here is accelerating, and the hunger to push boundaries is stronger than ever. For me, the week underscored why community matters: because progress happens faster when we learn, build, and grow together.


Until next time, KubeCon India, thanks for the inspiration.
