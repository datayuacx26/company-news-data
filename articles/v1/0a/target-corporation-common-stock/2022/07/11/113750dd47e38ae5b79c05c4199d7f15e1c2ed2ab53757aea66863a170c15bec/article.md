---
schema_version: "1.0.0"
document_id: "113750dd47e38ae5b79c05c4199d7f15e1c2ed2ab53757aea66863a170c15bec"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/custom-emoji-management"
published_at: "2022-07-28T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:f7ca574102c352eae847e5aa2becc063c07f613fab5d897c4b3ce8123b8ee3a0"
---

# 👍 Custom Emoji Management: How Target enhances its tech culture with creativity 🎨 and technology 🕹

When Target HQ first started to use chat systems, those systems allowed simple emoji usage, quickly turning :-) into 😀, and a few other simple faces. As chat technology evolved,


[Unicode standardized](https://unicode.org/standard/standard.html) more sophisticated emoji. Eventually, many chat systems allowed administrators and sometimes users to add custom emoji. This gave us some leeway and ability to get creative when it came to what emoji to use when chatting internally at Target.


Building the Emoji Management System on Slack


When Target transitioned to using Slack, our engineering enablement team saw an opportunity to improve our custom emoji process and positively impact our culture. Our very first iteration was simple and effective for engineers who code. We created a git repo with all the custom emoji in it, so any time someone wanted to add or change an emoji, the process kicked off with a pull request.


One huge benefit to this new process was we now had community engagement through discussions and approvals before new emoji made it into Slack. Adding them to Slack was still manual and required a Slack admin, but the process allowed a better sense of community ownership of the custom emoji. This, coupled with no limit on the number of emoji in Slack, allowed for an enormous number of new emoji to appear. Some were logos of products we use, funny memes that scaled down to emoji size well, but even more were custom works by team members as well as teams creating their own logos. Emoji were becoming a fun virtual version of the “laptop sticker” culture and were being shared across the organization.


Introducing an Emoji Linter


One challenge that emerged was people submitting images that would not work as emoji in Slack. Sometimes they were simply way too large in file size or dimensions, or the aspect ratio was so far from square they would be unrecognizable. We tackled that problem like engineers: since the emoji were all in GitHub, we implemented an emoji linter. Now, when someone submits a pull request, the linter would run and make sure the image size was appropriate. Eventually, we added a preview feature, where the proposed emoji would be scaled down, placed on both the light and dark background of Slack, and posted as a comment in the pull request for everyone to see.


Introducing #emoji-announcements


We were soon adding enough custom emoji that we created a Slack channel #emoji-announcements to share newly uploaded emoji. When the volume of custom emoji increased again, we wrote a simple Slack bot to automatically post emoji changes to the channel when they happened, instead of a Slack admin posting manually. The introduction of the #emoji-announcements channel brought greater awareness of custom emoji throughout the team and over four years, we saw more than 400 team members create and add 2886 custom emoji. By most measures, this was a success, but we also saw an opportunity to help less technical team members get more involved as Slack usage spread across the enterprise.


Introducing Emoji Manager


As a


[50 Days of Learning](https://corporate.target.com/article/2019/09/50-days-of-learning) project, we created Emoji Manager to bring emoji management into Slack itself and we created a fully automated and community-driven approach to creating custom emoji. The bot itself is simple: a user posts an image to the #emoji-announcements channel, and Emoji Manager takes that image and turns it into a proposal, where any user in the channel can upvote ✅ or downvote ❌. If a proposal gets enough upvotes, Emoji Manager then builds and uploads the new emoji and announces it to the channel 🎉. Emoji Manager has been running for eight months now and, at time of publishing, it’s already been used to add


450


new emoji.


Listening to our team members and how they use it, we made several tweaks along the way to bring us to the system we have today:


The Proposal


Proposing an emoji is as simple as dropping an image into the channel. The image file name is used as the emoji name, and Emoji Manager will post a message explaining the rules, a preview image (with both light and dark backgrounds), and a simple request to the original poster to explain the significance of the emoji. If for any reason the original poster wants to withdraw the proposal, they simply need to react to the post with ⏪ or delete the post. Requesting a new emoji alias, or requesting an emoji be deleted is done using a slash command, but the process works the same after the preproposal is created.


Winning Votes


For an emoji proposal to be accepted, it needs to achieve at least 15 more ✅ than ❌ votes. We started only requiring 5 more, but the number of participants was large enough that bumping it up made the voting more meaningful. This is a configurable setting that we will change over time based on the needs and size of the community.


Voting Period


We didn’t want to have emoji proposals live forever, so a voting period is established. To make sure everyone has a fair chance to see the proposal and respond to it, we give one full day to a comment period before votes are considered. After that, the proposal has five business days to get the winning votes needed, or the proposal is closed. We intentionally selected business days for the voting period to make sure that weekends and holidays don’t limit the usable time someone has to comment or vote on a proposal.


Slack Admin Controls


Slack admins can react to a post with 🟢 to force the proposal to be uploaded immediately. This is useful for situations like a proposal failing at upload and needing a quick redo, or if there is an urgent need to upload an emoji. Slack Admins can also react with ⛔ ️ to block a proposal from being uploaded.


Why This Matters


Anecdotally, our Slack admins have noticed an increase in emoji usage over the last few years. Team members are using emoji more and more to express themselves in more personalized and tailored ways. Having a variety of custom emoji lets them choose exactly the mood that works for them and helps infuse levity and joy into daily work processes and experiences. It also enables their use in Slack bot messages and profile pictures. Here’s how it works:


Our emoji usage has also become functional. We created a Slack bot that responds with pre-selected messages based on the emoji, which has been super helpful in support channels. Here’s what that looks like:


Power of the Community


When a team member proposes an emoji, an active discussion happens out in the open for everyone to participate in. A core value of Target’s


[culture](https://corporate.target.com/careers/culture) is inclusivity – we value diverse voices and approaches and strive to create equitable and respectful experiences for all. Ensuring that we create emoji in line with this value can be challenging. We want as many team members as possible to weigh in on the emoji and participate in the selection process. This community-driven review and voting process helps ensure non-inclusive and problematic emoji will not become a part of our lexicon. In the case where something is proposed and rejected, it becomes a community discussion creating space for growth and understanding.


Giving the community a voice in emoji selection makes the emoji more meaningful to them, and as a result more likely to be used and appreciated. And by making the process transparent and inclusive, we have not had to write out very many specific rules about what can or shouldn’t be an emoji. Self-governing allows people to safely propose an emoji, and safely learn and grow when the community feels it does not meet the standard.


Our community has grown to more than 900 people mere months after its creation, and as it continues to grow our list of custom emoji will grow alongside it, including even more voices and discussion in the mix.
