---
schema_version: "1.0.0"
document_id: "3b89e404b01e45046a89bd3b3bcc98378443c8f42d326216ad87192ae098943d"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-education-platform"
published_at: "2026-05-24T12:25:33+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:12dc9e5573a1f51f72096ed3f9f5115e5c862ee73f4c5122cc10ef2ae856b1ee"
---

# How to Build an Online Education Platform (No Transaction Fees, Own Your Students)

## Step 2: Set Up Student Accounts and Enrollment


Auth is built into Blink — students create accounts, enroll, and track progress without a separate auth setup. Students register with email and password. Their profile connects to their enrollments, progress records, and earned certificates.


Free preview lessons load without login. Paid lessons check enrollment status before showing content. Students who haven't purchased see the course sales page with an enrollment CTA.


Enrollment records which courses a student paid for and when. Once enrollment works, you need a way to track exactly what each student has watched and completed.


The complete student journey: from account creation to earned certificate — every step runs automatically on your owned platform


Blink


*The complete student journey: from account creation to earned certificate — every step runs automatically on your owned platform*


## Step 3: Add Video Lessons and Progress Tracking


Each video lesson stores a video URL, a duration, and a completion record per student. When a student finishes a lesson, the system marks it complete and updates their course progress percentage.


Blink includes the database automatically — student progress stores without Supabase or any external database service. Students resume where they left off on their next visit.


Progress bars appear at the course and module level. The instructor dashboard shows every student's completion percentage across all enrolled courses. Progress data alone doesn't verify understanding — which is why quizzes come next.


## Step 4: Build Quizzes and Assessments


Tell Blink: "Add a quiz system where each lesson can have an optional quiz. Quizzes have multiple choice and short answer questions. Students must score 80% or higher to advance to the next lesson."


Each quiz stores a pass threshold. A student who fails sees their score, correct answers, and a retry button. A student who passes advances — the system logs the result to their progress record.


Required quizzes act as completion gates. Students can't mark a lesson complete until they pass. With assessments in place, you need a way to collect payment for the content you've built.


## Step 5: Accept Payments With Stripe


Stripe integration is included in Blink — you sell courses without separate Stripe configuration. Tell Blink: "Add Stripe checkout for course enrollment. Support one-time purchase and monthly subscription plans."


Each course has a price type: one-time or subscription. Checkout creates a Stripe session, redirects the student, and grants enrollment access on payment success. For subscriptions, the platform follows[Stripe's billing documentation](https://stripe.com/docs/billing) to handle renewals and cancellations automatically.


Payment plans split a large course price into three equal monthly charges. Students see the installment breakdown before they buy. That's the same pattern you'd use for a[membership site](https://blink.new/blog/how-to-build-membership-site) — the enrollment logic is identical.


## Step 6: Auto-Issue Completion Certificates


When a student finishes every required lesson and passes every required quiz, the platform auto-generates a PDF certificate. The certificate includes the student name, course title, completion date, and a unique verification code.


Each certificate gets a shareable public URL. Students post them to LinkedIn or email them to employers directly from the platform.


Tell Blink: "Generate a completion certificate when a student finishes all required lessons. Include a public shareable link for each certificate."


The real cost of using Teachable: at $100K/year in sales, the 7.5% transaction fee alone costs $7,500 — more than a full year on a custom platform


Blink


*The real cost of using Teachable: at $100K/year in sales, the 7.5% transaction fee alone costs $7,500 — more than a full year on a custom platform*


## The True Cost: Teachable vs Owning Your Platform


Here's what each option costs at $100,000 in annual course sales:


Platform Monthly fee Transaction fee Annual cost at $100K sales


Teachable Starter $0–$39/mo 7.5% $7,500+ in fees alone


Teachable Builder $89/mo 0% $1,068


Teachable Business $249/mo 0% $2,988


Thinkific Basic $49/mo 0% $588


Kajabi $149/mo 0% $1,788


**Custom Blink build** **$20/mo** **0%** **$240**


A creator doing $100K/year on Teachable Starter pays $7,500 in transaction fees alone. Add the monthly plan at $39/mo and the total exceeds $7,968 per year — 33x the cost of a Blink Pro plan.


The fee math stings. But the ownership issue hurts more. Teachable holds your student email list. Move platforms and you lose access to your own students. On a platform you build, that data belongs entirely to you.


For the subscription billing side, see[how to build a Stripe subscription app](https://blink.new/blog/how-to-build-stripe-subscription-app) . Once your platform runs,[building a SaaS in a weekend](https://blink.new/blog/build-saas-in-a-weekend) covers how to expand it into a full product.


Export your student list from Teachable as a CSV — full email export is available on the Builder plan and above. Import those emails into your new platform and send a migration email with a password reset link. Students who re-register retain their course history if you map enrollment records during import.


Store video URLs pointing to Cloudflare Stream, Mux, or Bunny.net — all charge under $1 per GB of storage per month. Your Blink platform stores the URL and playback metadata; the video CDN handles delivery. Use YouTube unlisted videos for zero hosting cost while you validate your first courses.


Yes. The enrollment system supports three modes: fully free, free preview with paid upgrade (lesson-level gating), and fully paid. Set the mode per course. You can also issue Stripe coupon codes for promotional access — Blink generates the coupon redemption flow automatically.


Add a booking form to your platform. Students who purchase a coaching tier see a "Book a Call" section in their dashboard. Blink builds a simple scheduling form connected to your calendar in minutes — just describe what you need and it generates the flow.
