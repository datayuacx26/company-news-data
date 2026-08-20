---
schema_version: "1.0.0"
document_id: "071907220f82367190018a3fa0b81a338d5bc27404ee77514201ebe02a0e71e6"
company_key: "yc-mattermost"
company: "Mattermost"
source_id: "yc-mattermost-rss-3d807a20d23e"
canonical_url: "https://mattermost.com/blog/when-the-archive-answers-back/"
published_at: "2026-06-25T14:38:46+00:00"
first_seen_at: "2026-07-20T23:24:03.492829+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:1e5886245b95c2d1424a5c76ee926a7c133cd34742ca1b8ac5605099f7ee8521"
---

# When the Archive Answers Back

## Key Takeaways


- Semantic Search in Mattermost Agents V2 combines meaning-based and keyword-based search into one ranked result set, so relevant posts surface even when a query doesn’t match the original wording.
- The agent only searches channels the requesting user already has access to, so results respect existing permissions in compartmentalized and classified environments.
- Every AI-generated answer includes a sources list with the original channel, timestamp, and a direct link to each supporting post.
- Embeddings and the summarization model can run entirely on-premises or air-gapped, keeping queries and retrieved content inside the security boundary.
- Semantic Search runs on a Mattermost Enterprise license and can search posts created by reporting and collection-feed integrations, not just manually typed messages.


It’s 0730 at an all-source desk. An analyst who’s been on a foreign government’s economic policy account for 14 months opens their console to a tasking from a senior policymaker: how does last week’s announcement line up with the prior 18 months of reporting?


They remember roughly where the answer is. An assessment from around last summer in the regional channel. A technical thread from the spring where the financing figures came up. A coordination thread with the partner-agency desk that produced a disagreement worth flagging.


None of it comes back cleanly in keyword search. You get hundreds of chronological hits, so you open posts one at a time to build the source list before you can write a single paragraph. Ninety minutes go to the citation chase before the response goes out.


This is a retrieval and correlation problem, and it’s the one Semantic Search in[Mattermost Agents V2](https://mattermost.com/blog/introducing-mattermost-agents-v2-the-next-evolution-of-intelligent-workflows/) is built to solve. The chase becomes a sourced answer you can inspect, challenge, and cite.


## **What Is Semantic Search in Mattermost Agents V2?**


Semantic Search in Mattermost Agents V2 lets teams query channel history in plain language instead of exact keywords. It matches meaning as well as wording, then merges those results with a standard keyword pass so literal and conceptual matches surface together in one ranked list.


The agent sits in the Mattermost search bar, one click away. Ask it directly: ‘what have we assessed about this policy shift across the desk and adjacent channels in the last 18 months, including the technical work?’ The answer streams into the Agents panel on the right.


It doesn’t just match words. It matches meaning too, which is what surfaces the spring financing thread even though your tasking didn’t use the financing terminology the original reporting did. A keyword pass runs alongside it, and the two merge into one ranked set, so literal hits still land where meaning-matching misses.


The agent inherits your access. It sees only what you see, and channel access is[enforced end to end](https://mattermost.com/blog/multiplayer-tool-calling-for-secure-operations-who-approves-who-sees-who-runs-the-tool/) . Two analysts in the same cell can legitimately get different results if one is read into an adjacent thread the other isn’t. That’s what makes this usable in a compartmentalized environment instead of turning search into a spillage risk.


What it reads is the message text in your channels: the analysis people typed, plus any summaries that collection-feed and reporting integrations post as channel messages. A reporting feed that drops a formatted summary into ~raw-rep-archive as a post is searchable right alongside the team’s typed analysis.


## **How Does Mattermost Agents Provide Defensible Citations?**


Mattermost Agents V2 lists the exact source posts under every AI-generated answer, complete with channel, timestamp, and a direct link. Inline citations connect each factual claim to the specific post that supports it, so reviewers can verify originator and classification before relying on the summary.


For an intelligence reader, that’s the whole point. An AI summary you can’t source back to specific reporting isn’t consumable by a formal product.


A summary whose citations resolve to live posts on the on-prem instance is a different thing. You verify originator and classification by clicking through, then quote directly, knowing the link is the post you read.


## Cutting RFI Turnaround From Hours to Minutes


Different desk, different rhythm. An RFI analyst on the same enclave, same on-prem Mattermost, same local model, picks up RFI #2417 at 1100 with a 1700 deadline. The question spans three substantive channels. The old path is two hours of pivoting: keyword queries, pasting hits into a working doc, noting dates and originators by hand while the deadline shrinks.


Instead they paste the RFI into a ~rfi-desk thread and ask the agent to pull 12 months of reporting on the topic and call out where producers disagree. The agent searches the channels that analyst can see and comes back with a structured summary: producer A’s line, producer B’s line, the inline comment from a senior analyst flagging the open disagreement. The sources list renders underneath, each entry a one-click link.


When the reviewer challenges a citation 40 minutes later, the analyst clicks through, grabs the originator from the post header, and replies in 60 seconds. Citation defended. The RFI goes out at 1530, and the sourcing trail is durable instead of a working doc that gets deleted next week.


## **How Does Mattermost Agents Search Run On-Premises and Air-Gapped?**


Mattermost Agents V2 runs semantic search inside the same database that stores every channel post, with no data leaving the environment. Embeddings and the summarization model can run on-premises or air-gapped, so query text, retrieved content, and the AI response all stay inside the security boundary.


That architecture extends to the embeddings and the model. Embeddings can come from an on-prem, OpenAI-compatible endpoint, so if you already have a local model in place for the rest of the Agents stack, you can point search at the same boundary.


The summary you read is written by the same LLM Agents already uses. For an SCI enclave or an[air-gapped facility](https://mattermost.com/blog/mattermost-archtis-arqit-and-whitespace-sovereign-ai-collaboration-for-defence/) , the query, the retrieved post content, and the model response all stay inside the boundary by design. Semantic Search runs with a Mattermost Enterprise license.


## **Ninety Minutes of Citation Chase Becomes Ten**


So back to that 0730 tasking. The analyst opens the search bar, clicks into the agent, and asks the question plainly. Meaning-matching surfaces the spring financing thread. The keyword pass catches the two regional assessments they remembered by phrase. The partner-agency disagreement thread comes back with the rest.


The answer streams into the Agents panel with a sources list under it: five posts, each a one-click link to the original, each timestamp tied to the actual post, each originator visible on click-through. They verify the classification markings, pull direct quotes, and deliver a sourced response to the policymaker in 10 minutes.


Ninety minutes of citation chase becomes 10 minutes of source verification and quoting. And the desk file quietly turns into a self-curating record: the next analyst who inherits the account can ask “what does our prior reporting say about X?” and start from a sourced summary instead of three weeks of reading scrollback.


None of that is a clever prompt. It’s that the archive finally answers back.


## **Try it** today


Open the Mattermost search bar and choose the agent. Ask the question the way you’d ask a colleague who’s been on the desk longer than you. Read the answer in the Agents panel, open the sources list, and click into the post that surprises you.


Full reference is in[the Mattermost Agents docs](https://docs.mattermost.com/end-user-guide/agents.html) .


## Read More Platform Articles


## Open source news, right in your inbox


## Thanks for subscribing!
