---
schema_version: "1.0.0"
document_id: "ce1f356ccec67b913976711aa5a858cec7aa44737f59100c8f1d5f0b6a9890f5"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/resolve-call-connectivity-issues-zentrunk-debug-logs/"
published_at: "2024-08-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:252e6b64ed7c9d73cf9d3abcc41f79916293f28ab4f9d0673162f59e5847fc19"
---

# Transform Customer Interactions with Plivo’s Real-Time Audio Streaming

In today's fast-paced business environment, customer interactions play a pivotal role in determining success. Imagine a scenario where a call center receives hundreds of calls daily. Each call contains valuable insights about customer preferences, pain points, and overall satisfaction. However, without the right tools, these insights can remain untapped, buried within the raw audio of customer interactions. This is where Plivo’s Audio Stream comes in, transforming how businesses can leverage real-time audio data.


Plivo’s[Audio Stream](https://docs.plivo.com/docs/voice/concepts/audio-streaming/) allows businesses to stream raw audio from active calls to applications or third-party systems over WebSockets. This feature empowers organizations to capture and analyze customer interactions automatically, thereby enhancing the overall customer experience.


## Getting Started with Audio Stream


In this blog, we’ll outline the key highlights of this feature and offer tips for making the most of Audio Stream.


## How Does Audio Stream Work?


Plivo’s Audio Stream, part of Plivo's Voice API, represents the next generation of real-time access to raw audio data. When coupled with AI-based tools, businesses can leverage audio streaming to offer enhanced voice-based services, extract valuable insights, and elevate customer interactions.


To get started with Audio Stream, establish a WebSocket connection to stream raw audio from active calls in real-time to applications or third-party systems. With this connection, you can play audio, interrupt and clear buffered audio, and send a checkpoint event to indicate the completion of playback. Refer to our[API](https://www.plivo.com/docs/voice/api/stream/) and[XML](https://www.plivo.com/docs/voice/xml/the-stream-element/) documentation for detailed instructions on establishing and managing this connection.


The illustration below shows how a call center could use audio streaming to document key details from a customer interaction - data points that can later improve the customer experience.


### Other Real-Life Use Cases:


- **Healthcare Services:** In a healthcare setting, audio streaming can be used to transcribe patient calls in real-time, ensuring accurate record-keeping and immediate access to patient information for better service delivery. Additionally, by utilizing audio streams, healthcare providers can develop AI-based virtual assistants or bots that assist patients in booking appointments, refilling prescriptions, and answering common medical inquiries, thereby reducing the burden on human staff and improving patient satisfaction.
- **Financial Services:** For financial institutions, audio streaming can help monitor and analyze conversations for compliance purposes, ensuring that all regulatory requirements are met while also enhancing customer service. Additionally, AI-based virtual assistants can be integrated to assist customers with routine banking inquiries, and streamline tasks such as loan applications and account management. This not only improves operational efficiency but also elevates the customer experience by offering prompt and accurate assistance.


These are just a few examples of how audio streaming can be used across industries. Audio streaming can be applied in various use cases across different sectors.


## Bidirectional Audio Streaming


Upon establishing the audio stream via WebSocket, Plivo forks and transmits raw audio over the WebSocket in real-time, ensuring high quality. With bidirectional audio streaming, Plivo offers the functionality to transmit audio from your application back to Plivo, enabling real-time conversational use cases. During the call, Plivo will then relay this audio back to the caller or end user.


## What Can I Build with Audio Streaming APIs?


Audio streaming opens up numerous opportunities to enhance customer satisfaction by providing deeper insights into customer interactions. Here are some potential applications:


- **Conversational AI Bots:** Integrate raw audio captured by Audio Stream with AI bots via Amazon Lex or Google Dialogflow to create AI virtual assistants that engage with your customers.
- **Real-Time Transcriptions:** Use services such as Amazon Transcribe or Google Speech-to-Text for real-time transcriptions in multiple languages.
- **Sentiment Analysis:** Monitor conversations between your customers and agents to analyze service quality and improve training by identifying high and low performers.


## Getting started with Audio Stream


It’s easy to get Audio Stream up and running - simply follow the steps below.


1. [Sign up](https://console.plivo.com/accounts/request-trial/) with Plivo.
2. Purchase a number from the[console](https://console.plivo.com/active-phone-numbers/) or via API.
3. Attach the purchased number to the application which returns the audio stream XML instruction.
4. Dial the number.
5. Return the following XML instruction to start getting raw audio from Plivo and enable a conversational AI bot (bidirectional audio stream).


```text


```


1. Send audio back to Plivo via the same Websocket connection with the format highlighted below.


```text


```


1. Plivo relays the audio back to the call.


## Clear Audio


In scenarios where you need to interrupt or halt audio that you've sent to Plivo, use the clear audio command to seamlessly interrupt and clear buffered audio. Send the following instruction via WebSocket:


```text


```


## Pricing


Audio streaming is priced at $0.003 per minute per stream, in addition to the charges for voice minutes associated with a call. Pricing is subject to change, so check our[pricing page](https://www.plivo.com/voice/pricing/us/) for the most up-to-date information.


‍[Sign up](https://console.plivo.com/accounts/request-trial/) with Plivo today to try this powerful new capability for your calls.
