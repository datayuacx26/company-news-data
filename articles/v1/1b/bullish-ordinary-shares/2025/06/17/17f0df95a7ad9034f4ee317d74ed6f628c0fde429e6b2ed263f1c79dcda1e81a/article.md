---
schema_version: "1.0.0"
document_id: "17f0df95a7ad9034f4ee317d74ed6f628c0fde429e6b2ed263f1c79dcda1e81a"
company_key: "bullish-ordinary-shares"
company: "Bullish"
source_id: "bullish-ordinary-shares-rss-b348dbb1f0cd"
canonical_url: "https://medium.com/bullish-engineering/24-7-trading-addressing-fix-sequence-number-overflow-9d239799e0ba"
published_at: "2025-06-14T15:20:21+00:00"
first_seen_at: "2026-07-20T04:36:16.773580+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:b51da15d0fae844cd31c9dcda83f3589a7f9e0b864c3b44e462810124d758ac2"
---

# 24/7 Trading: Addressing FIX sequence number overflow

# 24/7 Trading: Addressing FIX sequence number overflow


[Sandeep Rakhra](https://medium.com/@sandeep.rakhra_41496?source=post_page---byline--9d239799e0ba---------------------------------------)


5 min read


·


Jun 14, 2025


--


Press enter or click to view image in full size


Our FIX API has fast become the most popular connectivity option for our institutional clients. As business demand and trade volumes continue to grow we have started to feel some growing pains.


Before we dive into the specifics, if you are new to FIX then check out these primer posts from[FixSpec](https://fixspec.com/) ⁴:


- [How does the FIX protocol work](https://fixspec.medium.com/how-does-the-fix-protocol-work-8503b9dc9a04)
- [History of FIX protocol](https://fixspec.medium.com/history-of-fix-protocol-17103a5e81f4)
- [What’s The Difference Between FIX and REST APIs?](https://fixspec.medium.com/whats-the-difference-between-fix-and-rest-apis-ad956f7a5dd0)


This post is one in a series in which we describe some practical challenges that we have overcome when building and optimizing our FIX API. Note, you can find information about our FIX connectivity offering from the[Bullish API documentation](https://api.exchange.bullish.com/docs/api/rest/trading-api/v2/#overview--fix-api) . This article focuses on two issues related to long session durations — sequence number overflow and resend requests. Let’s get into it.


## Sequence number overflow


In the equities markets FIX sessions are reset at the end of a trading session, typically 9:30am to 4pm, Monday to Friday. In comparison, the concept of a trading session in crypto markets is non-existent given markets operate 24/7. As a result, FIX sessions will be long-lived where the duration of the FIX session is measured by the **MsgSeqNum(34)** also known as sequence number.


Since the sequence numbers continuously increment without a daily reset, there is a risk of the sequence number reaching its maximum value, which can cause overflow issues. Proper handling and resetting mechanisms need to be in place to manage this risk.


We use Artio¹, an open source high performance FIX engine. Artio is maintained by Adaptive³. It is possible to invoke resource contention on the Artio FixLibrary thread if the FIX initiator sends a **ResendRequest\[2\]** message with a sequence number originating from early in the message log. We found an elegant solution to these problems was to introduce a sequence number cap. We send an unsolicited **Logout\[5\]** message should either the inbound or outbound sequence number reach 100 million. To alert the FIX initiator to an approaching threshold, we send an unsolicited **News\[B\]** message to warn them that they are reaching the maximum threshold, when they reach 1,000 messages less than the threshold. This gives the client the chance to cleanly log out (using a **Logout\[5\]** ) and log back in again ( **Logon\[A\]** ) with the **ResetSeqNumFlag(141)** set to **Y** . This approach allows for a controlled reset of sequence numbers, ensuring the stability and performance of the FIX session while minimizing disruptions for the client.


## Resend requests


The longer the FIX session, the larger the message log becomes. This is because the FIX protocol’s **ResendRequest(2)** message allows the FIX initiator to request the retransmission of a specified range of messages identified by their sequence numbers. Over time, the accumulation of messages can lead to substantial log files in both size and length. This can impact the performance of the FIX engine.


We partnered with Adaptive to understand how we can manage the size of the Aeron® message archive. Artio leverages Aeron®² for inter-process communication between the engine and library components. We found we could use the **pruneArchive** function during our release process to prune all messages before the last sequence reset and not part of the latest sequence index. The effect is that the resend requests for the current sequence index can still be processed but we can still reduce the size of the archive. Therefore we can keep the disk backing the Aeron® message archive at a reasonable and consistent size preventing cloud costs ballooning.


```text
public class TriggerArchivePruneHandler {      private final FixGatewayServer fixGatewayServer;       public void trigger(final AdminTriggerArchivePruneDecoder adminTriggerArchivePruneDecoder) {          final int notPrunedRecordingId = adminTriggerArchivePruneDecoder.recordingId();          log.info("Triggering Archive Pruning. notPrunedRecordingId=(}", notPrunedRecordingId);          if (notPrunedRecordingId == -1) {              fixGatewayServer - getEngine() - pruneArchive(null);          } else {              final Long2LongHashMap minimumPosition = new Long2LongHashMap(NULL_VALUE);              minimumPosition.put(notPrunedRecordingId, 0);              fixGatewayServer.getEngine().pruneArchive(minimumPosition);          }      }  }
```


However as business demand and trade volumes continue to grow, pruning the Aeron® archive every release wasn’t the only strategy we needed to employ. Latency benchmarks also started to glow red at the tails (4 nines) as more and more sessions needed to be supported. To keep our clients happy we planned 3 key changes:


- Offloading drop copy sessions — We moved drop copy sessions off the hot path by moving them to separate FIX engine instances.
- Offloading logging — We moved logging off the hot path to a separate service that tails the Aeron® message archive and emits logging and metrics.
- Reducing the sequence number cap — We are seeking to reduce the sequence number cap from 100 million to 10 million. Institutional clients operating in the equities markets typically reset sequence numbers daily. According to our data, a cap of 10 million provides clients a session duration of approximately 7 days for clients with high trading volumes. This adjustment is an acceptable compromise based on direct feedback from our clients.


We also experimented with load balancing sessions across multiple Artio FIX libraries keyed by a client’s **institutionId** and trade volume metric. The goal was to keep round-trip time (RTT) latencies consistent across sessions by evenly distributing the load and ensuring fairness to all clients. You will be hearing more about the results of that experiment in this blog series.


## Summary


In summary, by being pragmatic and data driven we can now support more FIX sessions (trading/drop-copy) as business demand and trade volumes continue to grow.


Feeling inspired by the cutting-edge work we’re doing at Bullish? We’re always on the lookout for talented individuals to join our team.[Explore our open roles](https://bullish.com/careers/) and be Bullish on your career. Want to stay up to date with the latest news from across Bullish? Follow us on[LinkedIn](https://www.linkedin.com/company/bebullish) and[X](https://twitter.com/Bullish) .


\[1\]: Artio is an open-source high performance FIX gateway/engine.


\[2\]: Aeron® is an open-source, low-latency messaging and high-availability clustering technology for electronic trading systems.


\[3\]:[Adaptive Financial Consulting](https://weareadaptive.com/) are experts in custom trading technology solutions. Both Aeron® and Artio are owned and operated by Adaptive. We have a strong partnership with Adaptive and make extensive use of their professional services to work through the fine details of our cloud native deployment on Google Cloud.


\[4\]:[FixSpec](https://fixspec.com/) are specialists in trading connectivity solutions. We have a strong partnership with FixSpec and make extensive use of their professional services.


*Disclaimer: This material, including any material accessed through embedded links (“Information”) is for general informational purposes only and is not intended to constitute investment, tax, accounting, legal or financial advice. This Information is not an offer to buy or sell or a solicitation of an offer or to buy or sell any particular asset or to provide financial services of any kind. You should consider your own personal situation carefully and consult your professional advisors before making any investment decision.*


*Bullish makes no representation as to the accuracy, completeness, timeliness, suitability, or validity of any Information. The products, services, and solutions discussed in the Information are likely to change, so the Information may become outdated, incorrect or incomplete. Bullish is under no obligation to update Information. Bullish will not be liable for any errors or omissions in the Information. Virtual assets and related products are high risk. Access to certain assets or services referred to in the Information may not be available in your jurisdiction or to all types of investors. Information may reference the Bullish Exchange, which is licensed by the Gibraltar Financial Services Commission (DLT license: FSC1038FSA). This Information may not be quoted, deleted, or modified in any way without Bullish’s prior written consent. All rights reserved. Bullish and the Bullish Logo are trademarks of Bullish Global.*
