---
schema_version: "1.0.0"
document_id: "a1a8ff53986cf74fb34bc9e39da37a913edaa4eb0766b39f5d2f0a045467a9b5"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/open-source-emoji-manager"
published_at: "2024-04-09T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T22:26:01.008185+00:00"
content_hash: "sha256:11e1a84e2b51d13f2e78361f453eeafd459e68c58eac0744779c26e8e47b5296"
---

# Announcing the Release of Emoji Manager as Open Source

Target is excited to announce the release of Emoji Manager as open source under the Apache 2 license. The team made several improvements based on feedback from our highly engaged users in the nearly a year and a half since I published my article about Target’s use of


[Custom Emoji](https://tech.target.com/blog/custom-emoji-management) . We see how emoji can be a key element of modern text expression with our own teams, and how often a well-placed emoji can help communicate tone and intention. Our team members submit emoji that are meaningful to them, and we find the common language of our custom emoji to improve both expression and inclusion of our team members as they communicate with one another. Emoji Manager has worked so well for us to manage our emoji growth that we want to share this system with the world.


Improving Emoji Quality


As the size of our custom emoji community has grown, a common theme in the feedback from our users is that too many low-quality emoji were being approved. These emoji are often large images scaled down so much that they become unrecognizable. One option we considered was changing the voting rules to require more votes for approval. However, we were not confident this would solve the problem, given the large size of our community. We also wanted to avoid creating the perception of a barrier that’s too high for submitting custom emoji.


After conducting numerous surveys and user interviews, we discovered that most users were not viewing the preview image in the message thread. Instead, they were simply voting based on the original image posted by the user. While requiring the posted image be pre-scaled might have worked, it would have introduced additional clutter in the channel, as users don’t always follow those rules. This led to a new idea: having users send the image to the Emoji Manager bot through direct messages. Emoji Manager would then provide the user a preview image, the feedback about the image, and a button to propose the emoji. Upon clicking the button, Emoji Manager would post in the emoji management channel the preview and the original image in the message thread. Through an informal poll, we found that this change improved users’ perception of the quality of custom emoji.


Cutting the Noise


Another theme that arose as the community grew rapidly was the increased noise in the channel. Many users would post a proposal, receive feedback about it, and repost it later. Sometimes, multiple times. This made it challenging for people to see which emoji proposals were still open for voting. The process changes detailed above partly addressed this issue by getting some of the initial attempts in a private DM with Emoji Manager, but a frequent request was to have some sort of digest of the current emoji proposals. To address this, we utilized the Slack App Home feature to build a dashboard displaying the proposals without all the messages and commentary. This dashboard makes it easy for people to see what is being voted on, what was recently added, and even get help on how to use the system.


Centering our Users


By adopting an Agile mindset, we are striving to continuously deliver value to our users by listening to their feedback, iterating on design, and encouraging a culture of learning and growth. A core value to the Emoji Manager product is user centricity. We want to empower our team members to make decisions, weigh in on proposed emoji, and participate in the process in a way that is accessible and fun. Our aim is to engage the community in the process to encourage diversity and representation in our emoji choices, allowing people to use the emoji that are most meaningful to them in their everyday conversations. We believe


[this release](https://github.com/target/emoji_manager) of Emoji Manager brings us closer to that vision and are excited to share this with the broader open source community.


We invite others to bring Emoji Manager into their workplaces, to help bring levity, joy, and fairness to the emoji used by their teams. We also welcome contributions to make Emoji Manager even better.


[Click here](https://github.com/target/emoji_manager) to access the project on GitHub and begin using it today.


## RELATED POSTS


### 👍 Custom Emoji Management: How Target enhances its tech culture with creativity 🎨 and technology 🕹


By Jay Kline, July 28, 2022


When Target HQ first started to use chat systems, those systems allowed simple emoji usage, quickly turning :-) into 😀, and a few other simple faces. As chat technology evolved, Unicode standardized more sophisticated emoji. Eventually, many chat systems allowed administrators and sometimes users to add custom emoji. This gave us some leeway and ability to get creative when it came to what emoji to use when chatting internally at Target.


### Announcing Target’s Open Source Fund


By Brian Muenzenmeyer, March 11, 2024


Sharing details around Target's new Open Source support fund
