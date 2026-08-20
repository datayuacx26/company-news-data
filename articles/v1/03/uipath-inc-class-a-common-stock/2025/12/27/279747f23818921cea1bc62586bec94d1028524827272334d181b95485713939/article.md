---
schema_version: "1.0.0"
document_id: "279747f23818921cea1bc62586bec94d1028524827272334d181b95485713939"
company_key: "uipath-inc-class-a-common-stock"
company: "UiPath Inc."
source_id: "uipath-inc-class-a-common-stock-rss-2f83a748bf9d"
canonical_url: "https://engineering.uipath.com/building-reliable-real-time-messaging-with-signalr-handling-large-payloads-and-guaranteed-delivery-7178a28458e2"
published_at: "2025-12-10T06:23:12+00:00"
first_seen_at: "2026-07-20T23:16:59.255384+00:00"
fetched_at: "2026-07-28T22:25:03.887977+00:00"
content_hash: "sha256:8a4635a9fd857d8cc931b6de5f17750157506288dcdb42f5ff10d226e51a7eba"
---

# Building reliable real-time messaging with SignalR: Handling large payloads and guaranteed delivery

# Building reliable real-time messaging with SignalR: Handling large payloads and guaranteed delivery


[sandeep rao](https://medium.com/@sandeepmi7?source=post_page---byline--7178a28458e2---------------------------------------)


9 min read


·


Dec 10, 2025


--


## Who should read this


Engineers and architects working with SignalR or real-time messaging systems who need predictable, reliable message delivery.


## Introduction


Building reliable real-time communication is harder than it looks. When your application depends on bidirectional messaging between distributed components, the usual fire-and-forget approach isn’t good enough. This post explores how we built a comprehensive reliability layer on top of SignalR to enable robust communication between UiPath Apps runtime and robots — handling large payloads, guaranteeing delivery, and recovering from failures.


## Context


With our Unified Developer Experience, users can create web apps and trigger workflows with just a single click. Every expression written within UiPath Apps is evaluated and executed by a UiPath Robot.


To make this possible, we needed a reliable communication channel between the app’s runtime and a robot. This channel allows the runtime to send workflow execution requests and receive status updates or results — whether triggered by a button click or other Apps events.


Since SignalR was our available framework for enabling real-time communication, we leveraged it to establish this connection. The communication between the runtime and the robot is scoped by a unique sessionId. When the Apps runtime starts, it generates a sessionId and initiates a robot job. Both the runtime and the robot use this same sessionId to connect to the SignalR hub, enabling seamless, bidirectional communication between the two components.


Press enter or click to view image in full size


## Why SignalR


UiPath Robots are already designed to communicate using SignalR. Choosing another mechanism would have required rearchitecting our robot communication model — adding complexity and delaying feature delivery. SignalR was the pragmatic choice for achieving real-time, low-latency communication within our existing ecosystem.


## Key challenges with large payloads in SignalR


Our initial SignalR implementation had three significant limitations:


### 1. 32kb payload limit → scalability & cost impact


- SignalR hubs enforce a 32kb maximum message size
- Increasing the limit is technically possible but forces the server to allocate larger buffers
- Larger buffers reduce the number of concurrent connections → directly impacts scalability
- Fewer concurrent connections push us into higher Azure SignalR pricing tiers


### 2. No acknowledgment (fire-and-forget) → high message loss risk


- SignalR does not guarantee delivery — messages are sent without acknowledgement.
- When large payloads are split into multiple chunks, losing even one chunk results in incomplete data
- **User-facing risk:** auser might click a button expecting a change, but nothing happens because part of the message never arrived


### 3. No retry or recovery → Total message failure for large payloads


- If a message fails, there is no automatic retry or error recovery
- A large payload (e.g., 10mb split into thousands of chunks) becomes extremely fragile — one drop means the entire message is unusable
- Failed messages simply vanish with no way to detect or resend them


These weren’t just technical limitations — they were user experience problems waiting to happen.


## The solution: a reliability layer


To address these limitations, we designed a reliability layer focused on overcoming size constraints, ensuring message delivery, and enabling recovery from failures


Our reliability layer handles three core responsibilities:


1. **Message acknowledgment & tracking:** every message gets confirmed
2. **Smart payload chunking:** break large messages into manageable pieces
3. **Intelligent retry logic:** recover from failures automatically


### 1. Message acknowledgment & tracking


Acknowledgment ensures no message is lost and helps with retry in case of failure.


- Each outgoing message includes a unique datasetId.
- When the receiver successfully processes a message, it sends back an acknowledgment with that datasetId.
- Unacknowledged messages remain in an in-memory outbox queue.
- If an acknowledgment isn’t received within a configurable timeout (default: 1 minute), the sender marks the message as failed and triggers retries.
- This ensures every message has a tracked lifecycle — from dispatch to confirmed delivery.


### 2. Smart payload chunking


Chunking keeps payloads under 32kb. For messages larger than 32kb, we implemented intelligent chunking.


**Size calculations:** 32kb supports up to 16,384 UTF-16 characters (32,768 bytes ÷ 2 bytes per character). We use 15,000 characters for payload data, reserving approximately 2.7kb for metadata and safety margins.


Messages are split into chunks of ~15,000 characters.


Each chunk carries metadata to allow reassembly:


```text
interface DatasetMessagePacket {    datasetId: string;        // Unique ID for the entire transfer    targetCommand: string;    // Original event name    totalPackets: number;     // How many chunks to expect    dataChunk: string;        // Actual data fragment    packetId: number;         // Sequence index  }
```


At the receiver:


- Chunks are collected in a` DatasetPacketCollector` .
- Once all chunks are received, the message is reconstructed.
- The receiver sends a` LargeDataDeliveryReport` back to confirm success or failure.


## 3. Intelligent retry logic


Retries to recover from temporary failures. Our reliability layer uses an Outbox Pattern for failed or pending messages.


- Every outgoing message is stored in the outbox until confirmed
- When a` LargeDataDeliveryReport` indicates failure, only the missing chunks are retried
- We support up to three retry attempts before marking a message as permanently failed


**Duplicate detection** ensures no message is processed twice. We maintain a record of processed datasetIds for one hour. If a datasetId has already been processed, the incoming message is ignored.


Successful or failed messages are automatically cleaned up after one hour to manage memory usage.


Example retry handler:


```text
connection.signalRConnection.on("LargeDataDeliveryReport", (data: any) => {    if (data.isSuccessful) {      outboxService.markSuccess(data.datasetId);    } else {      outboxService.retry(data);    }  });
```


### The implementation: bidirectional communication


## Message received acknowledgment / confirmation → ensure delivery


We introduced a new eventName (internal to the client) called` LargeDataDeliveryReport` which will be used to send a delivery report of a message whether it failed or succeeded.


## Get sandeep rao’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**Delivery payload for successful transfer:**


```text
{    datasetId: unique_id,    isSuccessful: true,    timestamp: DateTime.UtcNow,  }
```


**Delivery payload for failed transfer:**


```text
{    datasetId: unique_id,    isSuccessful: false,    timestamp: DateTime.UtcNow,    exception: error,    failedChunks: [] // indexes of failed chunks  }
```


With each data packet, we are passing datasetId.


We are sending back` datasetId` for both parties with` LargeDataDeliveryReport` message.


We use this` datasetId` to retry failed messages.


### Handle large payloads (> 32kb)


**Sender:**


1. Check if the payload is greater than 32kb (> 15,000 characters).
2. If yes, break it down into chunks where each chunk is less than 32kb.
3. Send all the chunks as data packets with related metadata. Each packet has an index so that the receiver can assemble it. Receiver will send LargeDataDeliveryReport with status, which will be used to retry if status is failed.


Each packet looks like:


```text
export interface DatasetMessagePacket {    datasetId: string;        // ID for the whole message transfer    targetCommand: string;    // original event name which was sent from client    totalPackets: number;     // number of packets for the dataset    dataChunk: string;        // chunk of original data in the packet    packetId: number;         // sequential index of the packet within dataset  }
```


> *While calculating the size of the payload, the assumption is that strings are UTF-16 encoded. Each character takes two bytes hence 32kb can accommodate around 16,384 characters. We have kept the limit at 15,000 characters for the payload and reserve space for metadata.*


**Sample code, which checks the size of a message and splits the message into chunks:**


```text
// Checks if it should split the payload  public shouldSplit(fullJson: string): boolean {    return fullJson.length > SignalRMessageSplitter.maxMessageSizeInChars;  }   function split(fullJson: string, eventName: string): DatasetMessagePacket[] {      const jsonParts = this._splitJson(fullJson);      const count = jsonParts.length;      const datasetId = uuid();      return jsonParts.map((dataChunk, index) => ({        packetId: index,        dataChunk: dataChunk,        totalPackets: count,        targetCommand: eventName,        datasetId: datasetId,      }));  }   function _splitJson(fullJson: string): string[] {    if (!fullJson) return [];        const chunks: string[] = [];    const chunkSize = SignalRMessageSplitter.maxMessageSizeInChars;        for (let i = 0; i < fullJson.length; i += chunkSize) {      chunks.push(fullJson.substring(i, i + chunkSize));    }        return chunks;  }
```


**Receiver:**


1. Once we receive the first packet, we store it inside a dictionary DatasetPacketCollector
2. If all chunks are received, the receiver can assemble back all the chunks to construct the original payload
3. If it’s successful, send LargeDataDeliveryReport with isSuccessful true that marks the status of the record as Completed
4. If the receiver does not receive all packets within 1 minute, we send a LargeDataDeliveryReport with isSuccessful false


***\[Note: This is for housekeeping / clean-up\]*** *After one hour, we run a check and clear the successful or failed requests from DatasetPacketCollector. Additionally, if we accumulate 1,000+ datasets, we force cleanup to manage memory.*


```text
export interface DatasetPacketCollector {    datasetId: string;    targetCommand: string;    totalPackets: number;    chunks: string[];    status: PacketStatus;    timer: any;    failedAtUtc?: number;    totalPacketReceived: number;  }
```


**Sample code to receive each packet:**


```text
public async receiveMessage<T>(data: string, observer: Subscriber<T>, sendMsgFunc: Function): Promise<string> {    const datasetPacket = JSON.parse(data) as DatasetMessagePacket;    let packetStored = this._datasetsPacketCollection[datasetPacket.datasetId];    if (!packetStored) {      // This timer is used to trigger a timeout event after 1 minute which       // marks a packet collection as failed if all packets are not stored      const timer$ = timer(this._dataTransferTimeoutMS)        .pipe(switchMap(() => this._onDataTransferTimeout(datasetPacket.datasetId, sendMsgFunc)))        .subscribe();      // When we receive the first packet for any dataset, we create the structure to store all packets      const newPacketsStore: DatasetPacketCollector = {        datasetId: datasetPacket.datasetId,        targetCommand: datasetPacket.targetCommand,        totalPackets: datasetPacket.totalPackets,        chunks: new Array(datasetPacket.totalPackets).fill(null), // generates empty fixed size array        status: PacketStatus.Pending,        timer: timer$,        totalPacketReceived: 1,      };      this._datasetsPacketCollection[newPacketsStore.datasetId] = newPacketsStore;      packetStored = this._datasetsPacketCollection[newPacketsStore.datasetId];    }    // If the dataset is already completed or failed, ignore the packet    if (packetStored.status === PacketStatus.Failed || packetStored.status === PacketStatus.Completed) {      return;    }    packetStored.totalPacketReceived++;    // fill the position of dataset     packetStored.chunks[datasetPacket.packetId] = datasetPacket.dataChunk;    // Check if all packets are received    if (packetStored.totalPacketReceived === packetStored.totalPackets) {      const message = packetStored.chunks.join('');      packetStored.status = PacketStatus.Completed;      packetStored.timer.unsubscribe();      // Send an acknowledgment back to the sender      const ackData = {        DatasetId: datasetPacket.datasetId,        IsSuccessful: true,        Timestamp: new Date(),      };      sendMsgFunc('SendCommand', LARGE_DATA_DELIVERY_REPORT, JSON.stringify(ackData));      // remove the chunk array and keep metadata for idempotency      this._datasetsPacketCollection[datasetPacket.datasetId].chunks = [];      observer.next(JSON.parse(message));    }    return;  }
```


## Retry of failed messages


We use an outbox with a retry pattern.


- Each outgoing message is stored in an Outbox while awaiting a LargeDataDeliveryReport.
- When a LargeDataDeliveryReport event is received with isSuccessful set to false, the failed message is retrieved from the Outbox for a retry. The message is then sent. Importantly, the resent message retains its original datasetId.
- To handle the potential issue of duplicate messages, we employ a check based on the datasetId. If a datasetId has already been processed, then the corresponding incoming message is ignored.
- After three retry attempts, if outgoing messages are still unable to be delivered successfully, the message is considered to have failed. These failed messages are not removed from the Outbox, and an error gets thrown.
- On the other hand, when a LargeDataDeliveryReport is received with isSuccessful set to true, the corresponding message is considered as successfully delivered.
- Memory management is essential for maintaining system performance. To this end, all messages in the Outbox, whether they have failed or succeeded, are cleared after an hour to prevent excess memory consumption.


**Sample code:**


```text
connection.signalRConnection.on("LargeDataDeliveryReport", (data: string) => {    if (data.isSuccessful) {      outboxService.markSuccess(data.datasetId);    } else {      outboxService.retry(data);    }  });   public async retry(data: LargeDataDeliveryReport): Promise<boolean> {    static readonly MAX_ATTEMPTS_ALLOWED = 3;    let packetStored = this._datasetsPacketCollection[data.datasetId];    if (!packetStored || packetStored.retriesAttempted > OutboxService.MAX_ATTEMPTS_ALLOWED) {      // skipped as message is either removed from outbox or never processed      return false;    } else {      packetStored.retriesAttempted++;      // filter chunks which have failed      if (!data.failedChunks) {        return false;      }      const chunksNeeded = packetStored.chunks.filter((item, index) =>         data.failedChunks.includes(index)      );      await Promise.all(        chunksNeeded.map(async (packet, index) => {          return connection.signalRConnection.send(            'SendCommand',            CodeBehindEventNames.DATA_TRANSFER_PACKET,            JSON.stringify(packet)          );        })      );      return true;    }  }
```


## Handling the worst case: disconnections


Network connections fail. It’s not a matter of if, but when. Our approach is pragmatic:


When a SignalR connection drops, we treat it as a terminal failure. The system:


1. Marks the current operation as errored
2. Shows a reconnection loader to the user
3. Spawns a fresh connection
4. Starts clean with a new session


This might seem aggressive, but it’s more reliable than trying to resurrect a broken connection state.


**Failure scenarios handled:**


- **Partial failures during retry:** only failed chunks are retransmitted, preserving bandwidth
- **Hub restarts mid-transfer:** timeout mechanism (1 minute) triggers failure and retry
- **Corrupted chunks or invalid JSON:** JSON parsing errors trigger LargeDataDeliveryReport with failure status


**Concurrency:** the system handles multiple simultaneous large transfers by maintaining separate DatasetPacketCollector entries per datasetId. Each transfer operates independently with its own timeout timer and retry logic.


## Performance considerations


Reliability doesn’t come free. We made several optimization decisions:


**Chunking overhead:** while splitting and reassembling messages adds latency, it’s predictable and acceptable for payloads that couldn’t be sent at all before.


**Memory management:** we aggressively clean up completed and failed transfers


- Successful transfers have their chunk arrays cleared immediately after reassembly (metadata retained for one hour for idempotency)
- Failed transfers persist for one hour for debugging
- If we accumulate 1,000+ datasets, we force cleanup


**UTF-16 Encoding:** our 15,000 character limit accounts for JavaScript’s UTF-16 string encoding (2 bytes per character), giving us a safe margin under the 32KB threshold.


## Why not alternative solutions?


You might wonder why we didn’t use:


**SignalR Streaming:** we use Azure SignalR Service for connection management, which does not support streaming \[[ref](https://learn.microsoft.com/en-us/azure/azure-signalr/signalr-resource-faq#are-there-any-feature-differences-in-using-azure-signalr-service-with-asp-net-signalr-) \]


**Larger payload sizes:** SignalR’s own documentation recommends 32kb limits for performance reasons \[[ref](https://learn.microsoft.com/en-us/aspnet/core/signalr/security?view=aspnetcore-9.0#buffer-management) \]


**Other protocols:** SignalR is currently our only option for real-time robot communication in this architecture


## Conclusion


Building reliable real-time communication requires more than just choosing the right framework — it demands a thoughtful reliability layer. Our solution demonstrates that with careful design around acknowledgments, chunking, and retries, you can build production-grade reliability on top of SignalR’s fire-and-forget foundation.
