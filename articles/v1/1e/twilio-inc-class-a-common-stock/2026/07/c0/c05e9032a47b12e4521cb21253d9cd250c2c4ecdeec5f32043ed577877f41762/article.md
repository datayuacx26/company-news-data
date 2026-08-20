---
schema_version: "1.0.0"
document_id: "c05e9032a47b12e4521cb21253d9cd250c2c4ecdeec5f32043ed577877f41762"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay-php"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:a0243d4e90a89e8956294914199d1ce2b89c962e17bdfbc4d4f835af4aef3137"
---

# How to Build An AI Phone Agent With Twilio Conversation Relay in PHP

Time to read:


-
-
-
-
-


July 15, 2026


**Written by**[Matthew Setter](https://www.twilio.com/en-us/blog/authors/author.msetter) Twilion


---


## How To Build An AI Phone Agent With Twilio Conversation Relay - PHP


Building a voice AI agent from scratch means wiring together speech recognition, text-to-speech, turn detection, and real-time audio streaming, all at low enough latency to feel natural.[Twilio Conversation Relay](https://www.twilio.com/en-us/products/conversational-ai/conversationrelay) handles the entire voice pipeline for you, so you can focus on the AI logic.


In this tutorial, you will build a fully functional AI phone agent in PHP using[Mezzio](https://docs.mezzio.dev/mezzio/) and[Mezzio-Swoole](https://docs.mezzio.dev/mezzio-swoole/) , the OpenAI API as the language model, and Deepgram Flux for state-of-the-art speech recognition.


## Programming Language Support


This post is for PHP users. You can also find this tutorial in the following programming languages:


- [Javascript](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay-node)
- [C#](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay-csharp)
- [Python](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay)


## Prerequisites


To follow this tutorial, you will need:


- A free[Twilio account](https://www.twilio.com/try-twilio) with a voice-capable phone number
- An[OpenAI API key](https://platform.openai.com/api-keys)
- [PHP 8.4](https://php.net/) or later installed
- [Composer](https://getcomposer.org/) available globally
- [ngrok](https://ngrok.com/) to expose your local server to Twilio


## Build the app


### Step 1: How Conversation Relay works


Before writing any code, it helps to understand how Conversation Relay fits into the call flow. When someone dials your Twilio number, Twilio requests TwiML from a webhook you configure. Your server returns a[<ConversationRelay>](https://www.twilio.com/docs/voice/twiml/connect/conversationrelay) verb inside a[<Connect>](https://www.twilio.com/docs/voice/twiml/connect) block, which tells Twilio to open a[WebSocket](https://websocket.org/guides/road-to-websockets/) connection to your server and hand the call off to Conversation Relay.


From that point on, Conversation Relay acts as the voice pipeline. It transcribes everything the caller says using speech-to-text (STT) and sends the transcript to your WebSocket server as a JSON message. Your server calls an AI model with the transcript, streams the response back over the WebSocket, and Conversation Relay synthesises each token into speech using text-to-speech (TTS), playing it to the caller in real time.


The flow looks like this:


Conversation Relay's median response latency is under 0.5 seconds, so conversations feel natural rather than robotic.


For this tutorial, you will build Hoot, an AI phone support agent for Owl Air, a fictional airline. Hoot can answer questions about flight status, baggage policy, loyalty points, and booking changes.


### Step 2: Setting up your project


Start by creating a new Mezzio project and changing into it.


Copy code


```text
composer create-project mezzio/mezzio-skeleton owl-air-phone-agent
```


You'll be prompted with a series of questions. Answer them as follows:


Copy code


```text
What type of installation would you like?
[1] Minimal (no default middleware, templates, or assets; configuration only)
[2] Flat (flat source code structure; default selection)
[3] Modular (modular source code structure; recommended)
Make your selection (2): 3
Which container do you want to use for dependency injection?
[1] laminas-servicemanager (supported by laminas)
[2] Symfony DI Container
[3] PHP-DI
[4] chubbyphp-container
Make your selection or type a composer package name and version (laminas-servicemanager (supported by laminas)): 1
Which router do you want to use?
[1] FastRoute (supported by laminas)
[2] laminas-router (supported by laminas)
Make your selection or type a composer package name and version (FastRoute (supported by laminas)): 1
Which template engine do you want to use?
[1] Plates (supported by laminas)
[2] Twig (supported by laminas)
[3] laminas-view installs laminas-servicemanager (supported by laminas)
[n] None of the above
Make your selection or type a composer package name and version (n): n
Which error handler do you want to use during development?
[1] Whoops (supported by laminas)
[n] None of the above
Make your selection or type a composer package name and version (Whoops (supported by laminas)): 1
```


Then, change into the new project directory.


Copy code


```text
cd owl-air-phone-agent
```


Then, in *composer.json* , change the minimum version of` laminas-servicemanager` to` "^4.5"` . With that done, you next need to install the required dependencies; there are only a few:


- [mezzio-swoole-websocket](https://github.com/settermjd/mezzio-swoole-websocket) : This adds[Swoole](https://wiki.swoole.com/en/#/) support for Mezzio that supports both HTTP and WebSocket requests.
- [Monolog](https://seldaek.github.io/monolog/) : This provides logging support for the application.
- [OpenAI's PHP client](https://github.com/openai-php/client) : This simplifies working with OpenAI in PHP.
- [PHP Dotenv](https://github.com/vlucas/phpdotenv) : This simplifies setting environment variables during development.
- [Twilio's PHP Helper Library](https://www.twilio.com/docs/libraries/reference/twilio-php/) : This simplifies working with Twilio in PHP.


To install them, run the following command:


Copy code


```text
composer require settermjd/mezzio-swoole-websocket monolog/monolog openai-php/client twilio/sdk vlucas/phpdotenv
```


When prompted with the following question, answer "y".


Copy code


```text
Do you trust "php-http/discovery" to execute code and wish to enable it now? (writes "allow-plugins" to composer.json) [y,n,d,?]
```


When the dependencies are installed, open *config/config.php* add, before the reference to` App\\ConfigProvider::class` , add in a reference to` \\Settermjd\\MezzioSwoole\\WebSocket\\ConfigProvider::class` , as you can see below:


Copy code


```text
class_exists(\Mezzio\Swoole\ConfigProvider::class)
? \Mezzio\Swoole\ConfigProvider::class
: function (): array {
return [];
},
\Settermjd\MezzioSwoole\WebSocket\ConfigProvider::class,
App\ConfigProvider::class,
```


### Step 3: Writing a voice-optimized system prompt


The system prompt is one of the most important parts of a phone agent. Text-to-speech synthesizers read your agent's responses verbatim, so responses that look fine on screen can sound unnatural when spoken aloud. Markdown formatting, bullet points, emoji, and symbols like` $` or` %` all produce awkward output.


Create a new file named *prompt.global.php* in *config/autoload* and add the following to it:


Copy code


```text
<?php
declare(strict_types=1);
return [
'prompt' => <<<'PROMPT'
You are Hoot, the friendly AI phone support agent for Owl Air.
You help callers with:
- Flight status (use the lookup_flight_status tool to check real-time status by flight number)
- Baggage policy (one carry-on and one personal item included; checked bags are thirty dollars each)
- Owl Air loyalty points (earn ten points per dollar spent; redeem at one cent per point)
- Booking changes (changes are free up to twenty-four hours before departure; same-day changes cost fifty dollars)
- Check-in (opens twenty-four hours before departure online or at the airport kiosk)
Speak naturally, as if talking on the phone. Follow these rules:
- Use plain sentences only. No bullet points, no markdown, no emojis
- Spell out all numbers ("thirty dollars", not "$30")
- Keep each response to two or three sentences maximum
- If you cannot help with something, say so briefly and offer to connect them with an agent
- Never make up information not listed above
PROMPT,
];
```


Spelling out numbers in the prompt itself (` thirty dollars` instead of` $30` ) can encourage the model to use the same format in its responses. Capping replies at two or three sentences keeps the conversation from sounding like a wall of text being read aloud. And, explicitly forbidding Markdown prevents the model from adding asterisks, headers, or lists that would be spoken as literal characters.


### Step 4: Building the TwiML endpoint


Now, it's time to build the endpoint that returns the` <ConversationRelay>` verb inside a` <Connect>` block. The official[Twilio PHP Helper Library](https://www.twilio.com/docs/libraries/reference/twilio-php/) includes TwiML helpers that let you build the response programmatically instead of writing raw XML strings. You'll use that to save time generating the required TwiML in a new endpoint of the application: "/twiml".


To create it, in the terminal, run the following command:


Copy code


```text
composer mezzio mezzio:handler:create -- \
--no-register \
"App\Handler\TwimlHandler"
```


If you're using Microsoft Windows, replace the backslashes with carets (` ^` ) or remove them and have the command be all on one line.


This generates two files: *src/App/src/Handler/TwimlHandler.php* and *src/App/src/Handler/TwimlHandlerFactory.php* . Open *src/App/src/Handler/TwimlHandler.php* and update it to match the following


Copy code


```text
<?php


declare(strict_types=1);


namespace App\Handler;


use Laminas\Diactoros\Response;
use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Twilio\TwiML\VoiceResponse;


final class TwimlHandler implements RequestHandlerInterface
{
public function __construct(private readonly string $domain) { }


public function handle(ServerRequestInterface $request): ResponseInterface
{
$voiceResponse = new VoiceResponse();
$connect       = $voiceResponse->connect();
$connect->conversationRelay(
[
'url'                         => "wss://{$this->domain}",
'welcomeGreeting'             => <<<EOF
Thanks for calling Owl Air! I'm Hoot.
I can help with flight status, baggage policy, loyalty points, or booking changes.
Which of those can I help you with?
EOF,
'ttsProvider'                 => 'ElevenLabs',
'transcriptionProvider'       => 'Deepgram',
'speechModel'                 => 'nova-3-general',
'eotThreshold'                => '0.8',
'ignoreBackchannel'           => 'true',
'hints'                       => 'Owl Air, loyalty points, baggage, carry-on, check-in, boarding pass',
'interruptible'               => 'any',
'interruptSensitivity'        => 'medium',
'elevenlabsTextNormalization' => 'auto',
]
);


$response = new Response();
$response->getBody()->write((string) $voiceResponse);


return $response->withHeader('Content-Type', 'text/xml');
}
}
```


When the "/twiml" endpoint is requested, the` handle()` function, above, will be called.


Now, open *src/App/src/Handler/TwimlHandlerFactory.php* and update it to match the following:


Copy code


```text
<?php


declare(strict_types=1);


namespace App\Handler;


use Psr\Container\ContainerInterface;
use RuntimeException;
use function getenv;


final class TwimlHandlerFactory
{
public function __invoke(ContainerInterface $container): TwimlHandler
{
$domain = $_ENV['DOMAIN'] ?? getenv('DOMAIN');
if ($domain === '') {
throw new RuntimeException('DOMAIN environment variable is not set.');
}


return new TwimlHandler($domain);
}
}
```


This class instantiates *TwimlHandler.php* when it's requested, which happens when "/twiml" is requested. As you can see, it retrieves the` DOMAIN` environment variable and instantiates and returns a` TwimlHandler` object with it.


Finally, register the "/twiml" route in the application's routing table, by updating the static function in *config/routes.php* to match the following:


Copy code


```text
return static function (Application $app, MiddlewareFactory $factory, ContainerInterface $container): void {
$app->post('/twiml', App\Handler\TwimlHandler::class, 'twiml');
$app->get('/api/ping', PingHandler::class, 'api.ping');
};
```


### Step 5: Configuring Conversation Relay attributes


The` <ConversationRelay>` verb accepts a rich set of attributes for tuning[STT (Speech To Text)](https://www.twilio.com/en-us/speech-recognition) and TTS (Text To Speech) behaviour. The configuration above uses several that are worth understanding in detail.


` ttsProvider="ElevenLabs"` and` transcriptionProvider="Deepgram"` select the voice and speech recognition providers.[ElevenLabs](https://elevenlabs.io/?utm_source=bing&utm_medium=cpc&utm_campaign=australia_brandsearch_brand_english&utm_id=569730352&utm_term=elevenlabs&utm_content=brand_-_brand&msclkid=ccfd93d388851ef960c8f987e3ef0a22) is the default TTS provider as of the Conversation Relay GA release, and it produces the most natural-sounding voices.[Deepgram](https://deepgram.com/voice-ai-platform) is the default STT provider and supports Deepgram Flux, the latest speech model.


` speechModel="nova-3-general"` enables Deepgram Flux, which combines transcription and end-of-turn detection in a single model. Compared to earlier models, Flux delivers 200 to 600 milliseconds less latency per response turn and approximately thirty percent fewer false interruptions. This means Hoot is less likely to cut a caller off mid-sentence or wait awkwardly after they have finished speaking.


` eotThreshold="0.8"` controls how confident Deepgram Flux must be that a speaker has finished before finalising the transcript. The value ranges from 0.5 (more aggressive, fires sooner) to 0.9 (more conservative, waits longer). A value of 0.8 is a good starting point for most use cases.


` ignoreBackchannel="true"` filters out conversational filler words like "yeah", "uh huh", and "okay" that callers say while listening. Without this setting, those sounds would trigger your WebSocket handler and send unnecessary requests to the model mid-response.


` hints` is a comma-separated list of words and phrases that Deepgram should give extra weight to during transcription. Business-specific terms like product names, place names, and technical vocabulary often trip up general speech recognition models. Adding them as hints improves accuracy for your specific use case.


` interruptible="any"` and` interruptSensitivity="medium"` control how the agent responds when a caller speaks while it is talking. Setting` interruptible` to` any` means both speech and DTMF keypresses can interrupt the agent. The` medium` sensitivity setting reduces false interruptions in noisier environments while still responding quickly to intentional interruptions.


` elevenlabsTextNormalization="auto"` tells ElevenLabs to automatically normalise numbers and abbreviations in the text before synthesising speech. This is useful as a safety net if your LLM occasionally outputs a digit or symbol despite your instructions.


The` url` attribute points to the WebSocket endpoint on your server. It must use` wss://` (the secure WebSocket protocol), not` ws://` . The` welcomeGreeting` is played to the caller immediately after Conversation Relay connects, before your WebSocket handler sends any response. Listing Hoot's four topic areas in the greeting orients callers right away and reduces the chance they ask for something outside the agent's scope.


### Step 6: Build the WebSocket handler


Create a new directory named *src/App/src/WebSocket/Table* . Then, in that directory, create two files, named *ConnectionTable.php* and *MessageTable.php* .


In *ConnectionTable.php* , paste the following code:


Copy code


```text
<?php


declare(strict_types=1);


namespace App\WebSocket\Table;


use Swoole\Table;


final class ConnectionTable extends Table
{
public function __construct()
{
parent::__construct(1024);
$this->column('callSid', self::TYPE_STRING, 34);
$this->create();
}
}
```


Then, in *MessageTable.php* , paste the following code:


Copy code


```text
<?php


declare(strict_types=1);


namespace App\WebSocket\Table;


use Swoole\Table;


final class MessageTable extends Table
{
public function __construct()
{
parent::__construct(1024);
$this->column('role', self::TYPE_STRING, 10);
$this->column('content', self::TYPE_STRING, 10000);
$this->create();
}
}
```


These two classes are implementations of[Swoole\\Table](https://docs.mezzio.dev/mezzio-swoole/v4/table/) , required for sharing structured data between the message workers and having data outlive the websocket request cycle. What's more, they are automatically synchronized across all workers.` ConnectionTable` stores the callSid, so that all input from a given user can be collected together.` MessageTable` stores the input from a given user.


Next, in *src/App/src/WebSocket/Listener/* , create a new file named *ConversationRelayMessageListener.php* . In that new file, paste the code below.


Copy code


```text
<?php


declare(strict_types=1);


namespace App\WebSocket\Listener;


use App\Service\OpenAiService;
use App\WebSocket\Table\ConnectionTable;
use App\WebSocket\Table\MessageTable;
use Psr\Log\LoggerInterface;
use Settermjd\MezzioSwoole\WebSocket\Event\WebSocketMessageEvent;
use Swoole\WebSocket\Server as SwooleWebSocketServer;


use function json_encode;
use function sprintf;


final class ConversationRelayMessageListener
{
public function __construct(
private readonly OpenAiService $openAi,
private ConnectionTable $connectionTable,
private MessageTable $messageTable,
private readonly LoggerInterface $logger,
) {
}


private function writeLogMessage(string $callSid, string $message): void
{
$this->logger->info($callSid, ['message' => $message]);
}


public function __invoke(WebSocketMessageEvent $event): void
{
$frame = $event->getFrame();
$fd    = $frame->fd;
$data  = json_decode($frame->data, true);
if (! is_array($data) || $data === []) {
return;
}


$msgType = $data['type'] ?? '';
switch ($msgType) {
case 'setup':
$callSid = $data['callSid'] ?? null;
$this->connectionTable->set((string) $fd, ['callSid' => $callSid]);
$this->writeLogMessage(
$callSid,
sprintf("[%s] Call connected\n", $callSid)
);
break;
case 'prompt':
$last = $data['last'] ?? '';
if ($last === '') {
break;
}
$userText = $data['voicePrompt'] ?? '';
$this->writeLogMessage(
$this->connectionTable->get((string) $fd, 'callSid'),
sprintf(
"[%s] Caller: {$userText}\n",
$this->connectionTable->get((string) $fd, 'callSid'),
),
);
$this->messageTable->set((string) $fd, [
'role' => 'user',
'content' => $userText
]);
$responseText = $this->streamResponse(
$event->getServer(),
$fd,
$this->connectionTable->get((string) $fd, 'callSid'),
iterator_to_array($this->messageTable),
);
$this->writeLogMessage(
$this->connectionTable->get((string) $fd, 'callSid'),
sprintf(
"[%s] Hoot: {$responseText}\n",
$this->connectionTable->get((string) $fd, 'callSid'),
)
);
$this->messageTable->set((string) $fd, [
'role' => 'assistant',
'content' => $responseText
]);
break;
case 'interrupt':
$this->writeLogMessage(
$this->connectionTable->get((string) $fd, 'callSid'),
sprintf(
"[%s] Interrupted after: '%s'\n",
$this->connectionTable->get((string) $fd, 'callSid'),
$data['utteranceUntilInterrupt'] ?? ''
)
);
if ($this->messageTable->count() !== 0) {
$prev = $this->messageTable->offsetGet($this->messageTable->count() - 1);
if ($prev !== null && $prev['role'] === 'assistant') {
$this->messageTable->offsetUnset($this->messageTable->count());
}
}
break;
case 'error':
$this->writeLogMessage(
$this->connectionTable->get((string) $fd, 'callSid'),
sprintf(
"[%s] Conversation Relay error: %s\n",
$this->connectionTable->get((string) $fd, 'callSid'),
$data['description'] ?? ''
)
);
break;
}
}


private function streamResponse(
SwooleWebSocketServer $server,
int $fd,
?string $callSid,
array $messages = []
): string {
$fullResponse = '';
try {
$fullResponse = $this->openAi->streamResponse(
$messages,
static function (string $token) use ($server, $fd): void {
$server->push(
$fd,
json_encode(
[
'type'  => 'text',
'token' => $token,
'last'  => false,
]
)
);
},
);
} finally {
$server->push(
$fd,
json_encode(
[
'type'  => 'text',
'token' => '',
'last'  => true,
]
)
);
}
return $fullResponse;
}
}
```


[Swoole](https://wiki.swoole.com/en/#/) , the WebSocket technology that the application is built on, listens for a series of events which callbacks can be registered for. The code above responds to[the onMessage event](https://wiki.swoole.com/en/#/websocket_server?id=onmessage) . This event is called when the application receives a data frame from Twilio.


The` setup` message arrives first, immediately after Conversation Relay opens the WebSocket connection. It contains the` callSid` for the call and any custom parameters you passed via` <Parameter>` elements in your TwiML. This is stored in` $connectionTable` , which will be discussed shortly, so that the caller's transcribed speech can be fully collected, as it is received by the websocket workers. The call connection is logged helping with debug later, should it be required.


The` prompt` message carries the caller's transcribed speech in` $data\["voicePrompt"\]` . As it's received, it's stored in` $messageTable` , which will be discussed shortly. The` last` field indicates whether this is the final transcript for a turn. When` partialPrompts` is disabled (the default), you will only receive` prompt` messages with` last: true` , so the` $data\['last'\]` check ensures you only send requests to the model using the` OpenAiService` object` $openAi` (which will be created later) once the caller has finished speaking.


The` interrupt` message fires when the caller speaks while Hoot is talking. The` utteranceUntilInterrupt` field tells you how much of Hoot's response was played before the interruption. Because that response was cut short, it would be misleading to keep it in the conversation history as a complete assistant turn. The handler removes the last assistant message from the` messages` list, so the next request starts from a clean state.


The` error` message carries a numeric error code and a description. Common errors include invalid provider configurations and WebSocket timeout events. Logging them makes debugging significantly easier.


Now, in *src/App/src/WebSocket/Listener* , you need to create a new file name *ConversationRelayMessageListenerFactory.php* . In the new file, paste the code below:


Copy code


```text
<?php


declare(strict_types=1);


namespace App\WebSocket\Listener;


use App\Service\OpenAiService;
use App\WebSocket\Listener\ConversationRelayMessageListener;
use App\WebSocket\Table\ConnectionTable;
use App\WebSocket\Table\MessageTable;
use Psr\Container\ContainerInterface;
use Psr\Log\LoggerInterface;


use function assert;


final class ConversationRelayMessageListenerFactory
{
public function __invoke(ContainerInterface $container): ConversationRelayMessageListener
{
$openAiService = $container->get(OpenAiService::class);
assert($openAiService instanceof OpenAiService);


$messageTable = $container->get(MessageTable::class);
assert($messageTable instanceof MessageTable);


$connectionTable = $container->get(ConnectionTable::class);
assert($connectionTable instanceof ConnectionTable);


$logger = $container->get(LoggerInterface::class);
assert($logger instanceof LoggerInterface);


return new ConversationRelayMessageListener(
openAi: $openAiService,
connectionTable: $connectionTable,
messageTable: $messageTable,
logger: $logger,
);
}
}
```


In the code above, it retrieves all of the required services from the application's DI container, which you'll create shortly, and uses them to initialise and return a new` ConversationRelayMessageListener` object.


Next up, in *src/App/src/WebSocket/Listener* , create a new file named *ConnectionClosedListener.php* , and in that file paste the code below.


Copy code


```text
<?php


declare(strict_types=1);


namespace App\WebSocket\Listener;


use App\WebSocket\Table\ConnectionTable;
use App\WebSocket\Table\MessageTable;
use Settermjd\MezzioSwoole\WebSocket\Event\WebSocketCloseEvent;


final class ConnectionClosedListener
{
public function __construct(
private MessageTable $messageTable,
private ConnectionTable $connectionTable,
) {
}


public function __invoke(WebSocketCloseEvent $event): void
{
$this->messageTable->del($event->getFd());
$this->connectionTable->del($event->getFd());
}
}
```


This class responds to[Swoole's onClose event](https://wiki.swoole.com/en/#/server/events?id=onclose) , after a TCP client connection is closed. It's important to respond to this to delete both of the Swoole Table objects (` $messageTable` and` $connectionTable` ).


Now, in the same directory, create a new file named *ConnectionClosedListenerFactory.php* , and paste the code below into the file.


Copy code


```text
<?php


declare(strict_types=1);


namespace App\WebSocket\Listener;


use App\WebSocket\Table\ConnectionTable;
use App\WebSocket\Table\MessageTable;
use Psr\Container\ContainerInterface;


final class ConnectionClosedListenerFactory
{
public function __invoke(ContainerInterface $container): ConnectionClosedListener
{
$messageTable = $container->get(MessageTable::class);
assert($messageTable instanceof MessageTable);


$connectionTable = $container->get(ConnectionTable::class);
assert($connectionTable instanceof ConnectionTable);


return new ConnectionClosedListener($messageTable, $connectionTable);
}
}
```


This class instantiates the` ConnectionClosedListener` object, when requested from the application's DI container, providing it with the Swoole Table objects.


Now create a new directory named *Factory* in *src/App/src* , and in that directory create a file named *LoggerFactory.php* . In that new file, paste the code below.


Copy code


```text
<?php


declare(strict_types=1);


namespace App\Factory;


use Monolog\Handler\ErrorLogHandler;
use Monolog\Level;
use Monolog\Logger;
use Monolog\Processor\PsrLogMessageProcessor;
use Psr\Container\ContainerInterface;
use Psr\Log\LoggerInterface;


class LoggerFactory
{
public function __invoke(ContainerInterface $container): LoggerInterface
{
$logger = new Logger('swoole-http-server');
$logger->pushHandler(new ErrorLogHandler(
ErrorLogHandler::OPERATING_SYSTEM,
Level::Debug,
true,
true,
));
$logger->pushProcessor(new PsrLogMessageProcessor());


return $logger;
}
}
```


This class will instantiate a[Monolog Logger](https://seldaek.github.io/monolog/doc/01-usage) instance used in` ConnectionOpenedListener` and` ConversationRelayMessageListener` for logging what happens over the course of incoming requests.


### Step 7: Stream responses and calling tools


Right now, Hoot can only answer questions about facts baked into the system prompt. To look up real flight status information, you need to give the model a tool it can call. That's where the` OpenAIService` , which you'll create in a moment, comes in. It lets you define tools as JSON schemas, and the model decides whether to call one based on the caller's request.


First, create a new directory named *src/App/src/Service* , and in that directory create a new file named *OpenAiService.php* . In the file, paste the code below:


Copy code


```text
<?php


declare(strict_types=1);


namespace App\Service;


use OpenAI\Client;


use function array_map;
use function array_merge;
use function array_values;
use function json_decode;
use function strtoupper;


final class OpenAiService
{
private const MODEL = 'gpt-4o-mini';


private const TOOLS = [
[
'type'     => 'function',
'function' => [
'name'        => 'lookup_flight_status',
'description' => 'Look up the current status of an Owl Air flight by flight number.',
'parameters'  => [
'type'       => 'object',
'properties' => [
'flight_number' => [
'type'        => 'string',
'description' => 'The Owl Air flight number, e.g. OA101',
],
],
'required'   => ['flight_number'],
],
],
],
];


private const MOCK_FLIGHT_DATA = [
'OA101' => ['status' => 'on time',                 'gate' => 'B12', 'departure' => 'two thirty PM'],
'OA205' => ['status' => 'delayed by forty minutes', 'gate' => 'C4',  'departure' => 'five fifteen PM'],
'OA318' => ['status' => 'cancelled'],
];


public function __construct(private readonly Client $client, private readonly string $systemPrompt)
{
}


public function streamResponse(array $messages, callable $onToken): string
{
$openAiMessages = array_merge(
[
[
'content' => $this->systemPrompt,
'role'    => 'system',
],
],
$messages,
);
$stream = $this->client->chat()->createStreamed([
'model'      => self::MODEL,
'max_tokens' => 300,
'messages'   => $openAiMessages,
'tools'      => self::TOOLS,
]);
$fullResponse = '';
$finishReason = null;
$toolCallsAcc = [];  // index => ['id' => '', 'name' => '', 'arguments' => '']
foreach ($stream as $response) {
$choice       = $response->choices[0];
$delta        = $choice->delta;
$finishReason = $choice->finishReason ?? $finishReason;
if ($delta->content !== null) {
$fullResponse .= $delta->content;
($onToken)($delta->content);
}
foreach ($delta->toolCalls ?? [] as $tc) {
if (! isset($toolCallsAcc[$tc->index])) {
$toolCallsAcc[$tc->index] = ['id' => '', 'name' => '', 'arguments' => ''];
}
if ($tc->id !== null) {
$toolCallsAcc[$tc->index]['id'] = $tc->id;
}
if ($tc->function?->name !== null) {
$toolCallsAcc[$tc->index]['name'] = $tc->function->name;
}
if ($tc->function?->arguments !== null) {
$toolCallsAcc[$tc->index]['arguments'] .= $tc->function->arguments;
}
}
}


if ($finishReason === 'tool_calls' && ! empty($toolCallsAcc)) {
$toolCalls = array_values($toolCallsAcc);
$openAiMessages[] = [
'role'       => 'assistant',
'tool_calls' => array_map(
static fn (array $tc): array => [
'id'       => $tc['id'],
'type'     => 'function',
'function' => ['name' => $tc['name'], 'arguments' => $tc['arguments']],
],
$toolCalls,
),
];


foreach ($toolCalls as $tc) {
$args   = json_decode($tc['arguments'], true);
$result = $this->executeTool($tc['name'], $args);
$openAiMessages[] = [
'role'         => 'tool',
'tool_call_id' => $tc['id'],
'content'      => $result,
];
}
$fullResponse = '';
$stream2      = $this->client->chat()->createStreamed([
'model'      => self::MODEL,
'max_tokens' => 400,
'messages'   => $openAiMessages,
]);


foreach ($stream2 as $response) {
$delta = $response->choices[0]->delta;
if ($delta->content !== null) {
$fullResponse .= $delta->content;
($onToken)($delta->content);
}
}
}


return $fullResponse;
}


private function executeTool(string $name, array $args): string
{
if ($name !== 'lookup_flight_status') {
return 'Unknown tool.';
}


$flightNumber = strtoupper($args['flight_number'] ?? '');
$flight       = self::MOCK_FLIGHT_DATA[$flightNumber] ?? null;
if ($flight === null) {
return 'No flight found with that number.';
}


if ($flight['status'] === 'cancelled') {
return 'That flight has been cancelled.';
}


return "Flight {$flightNumber} is {$flight['status']}, "
. "departing at {$flight['departure']} from gate {$flight['gate']}.";
}
}
```


Stepping through the code,` MODEL` sets the OpenAI model to use. Next up,` TOOLS` , is the schema you pass to the OpenAI API.` MOCK_FLIGHT_DATA` is a stand in for a real database or API call. In a production agent, you would replace` executeTool()` with a live lookup. When a caller gives a flight number that is not in the data,` executeTool()` returns` "No flight found with that number."` , so Hoot always has a graceful response rather than hallucinating status information. The flight data uses spelled-out times (` "two thirty PM"` ) following the same voice-optimized convention as the system prompt, so ElevenLabs reads them naturally, when they appear in Hoot's response.


If you add more Owl Air–specific flight numbers to your real data, add them to the` hints` attribute in your TwiML as well. Deepgram gives extra weight to hinted terms during transcription, which reduces misheard flight numbers like "OA one oh one" being transcribed as "0-8101".


The` streamResponse()` function runs in two phases. In phase one, the OpenAI stream includes` tools=TOOLS` . If the model decides to answer directly (a baggage or loyalty question, for example), it returns tokens and` $finishReason` is` "stop"` , so the function is done after sending the` last: True` sentinel. If it wants to call a tool,` $finishReason` is` "tool_calls"` and the streamed deltas contain a tool call rather than content.


Phase two only runs when there are tool calls to execute. The function appends the model's tool-call message to the conversation, calls` executeTool()` for each one, appends the results as` "tool"` role messages, and makes a second streaming call. This second stream produces the spoken response, because the model now has the tool result and can answer the question in natural language.


Each` text` message you send to Conversation Relay contains a` token` (a text fragment) and a` last` flag. Sending tokens one by one as they arrive with` last: False` lets Conversation Relay begin synthesising and playing speech before the full response is ready. The final message with` last: True` and an empty token signals the end of the turn. The function returns the full accumulated text so the WebSocket handler can append it to the conversation history.


Now, in *OpenAiServiceFactory.php* , paste the code below:


Copy code


```text
<?php


declare(strict_types=1);


namespace App\Service;


use OpenAI;
use Psr\Container\ContainerInterface;
use RuntimeException;


use function getenv;


final class OpenAiServiceFactory
{
public function __invoke(ContainerInterface $container): OpenAiService
{
$apiKey = $_ENV['OPENAI_API_KEY'] ?? getenv('OPENAI_API_KEY');
if (empty($apiKey)) {
throw new RuntimeException('OPENAI_API_KEY environment variable is not set.');
}
$systemPrompt = $container->get('config')['prompt'];


return new OpenAiService(OpenAI::client($apiKey), $systemPrompt);
}
}
```


This class instantiates the` OpenAiService` service, when it's requested from the application's DI container. It retrieves the` OPENAI_API_KEY` environment variable and the system prompt, and uses them to instantiate and return an` OpenAiService` object.


With all of the OpenAI-related classes created, you now need to update the application's configuration by wiring the new classes into the application's DI container, so that they can be retrieved when required.


To do that, update the` getDependencies()` function in *src/App/src/ConfigProvider.php* to match the following:


Copy code


```text
public function getDependencies(): array
{
return [
'factories'  => [
Handler\TwimlHandler::class => Handler\TwimlHandlerFactory::class,
LoggerInterface::class => LoggerFactory::class,
Service\OpenAiService::class   => Service\OpenAiServiceFactory::class,
WebSocket\Listener\ConnectionClosedListener::class => WebSocket\Listener\ConnectionClosedListenerFactory::class,
WebSocket\Listener\ConversationRelayMessageListener::class => WebSocket\Listener\ConversationRelayMessageListenerFactory::class,
],
'invokables' => [
Handler\PingHandler::class => Handler\PingHandler::class,
],
'services' => [
ConnectionTable::class => new ConnectionTable(),
MessageTable::class => new MessageTable(),
],
];
}
```


Next, add the following namespaces to the top of the file:


Copy code


```text
use App\Factory\LoggerFactory;
use App\WebSocket\Table\ConnectionTable;
use App\WebSocket\Table\MessageTable;
use Psr\Log\LoggerInterface;
```


Then, register the two listeners so that they respond to message events, by adding the following function to *src/App/src/ConfigProvider.php* .


Copy code


```text
public function getSwooleConfig(): array
{
return [
'swoole-http-server' => [
'listeners' => [
\Settermjd\MezzioSwoole\WebSocket\Event\WebSocketCloseEvent::class => [
WebSocket\Listener\ConnectionClosedListener::class,
],
\Settermjd\MezzioSwoole\WebSocket\Event\WebSocketMessageEvent::class => [
WebSocket\Listener\ConversationRelayMessageListener::class,
],
],
],
];
}
```


Then, update the class'` __invoke()` function to call` getSwooleConfig()` and load the Swoole configuration, as below.


Copy code


```text
public function __invoke(): array
{
return [
'dependencies'  => $this->getDependencies(),
'mezzio-swoole' => $this->getSwooleConfig(),
'templates'     => $this->getTemplates(),
];
}
```


Finally, add the following configuration in a new file named *swoole.global.php* in the *config/autoload* directory.


Copy code


```text
<?php


declare(strict_types=1);


return [
'mezzio-swoole' => [
'enable_coroutine' => true,
'swoole-http-server' => [
'host' => '0.0.0.0',
'mode' => SWOOLE_PROCESS,
'port' => 8080,
'options' => [
'worker_num'      => 2,
'task_worker_num' => 2,
'task_enable_coroutine' => true,
],
],
],
];
```


This tells Swoole to listen for requests on all available IP addresses on port 8080 and sets the number of workers to start.


### Step 8: Running your server


You are ready to run the server. Open a second terminal and start ngrok:


Copy code


```text
ngrok http 8080
```


ngrok will display a` Forwarding` line that looks something like this:


Copy code


```text
Forwarding  https://1234abcd.ngrok.app -> http://localhost:8000
```


Copy the hostname (` 1234abcd.ngrok.app` — **without the protocol** , i.e.,` https://` ). Then, in a new terminal session or tab, paste it into the following command, along with your OpenAI API key, and run it to start the application:


Copy code


```text
DOMAIN=<ngrok domain> OPENAI_API_KEY=<your OpenAI api key> vendor/bin/laminas mezzio:swoole:start
```


In your terminal, you'll see the following output:


Copy code


```text
WebSocketServerFactory built: Swoole\WebSocket\Server
Worker started in /opt/owl-air-phone-agent with ID 0
Worker started in /opt/owl-air-phone-agent with ID 2
Worker started in /opt/owl-air-phone-agent with ID 3
Worker started in /opt/owl-air-phone-agent with ID 1
```


### Step 9: Configuring your Twilio phone number


Log in to the[Twilio Console](https://console.twilio.com/) and navigate to **Phone Numbers** in the left sidebar, then **Manage** , then **Active Numbers** . Click your voice-capable phone number to open its configuration page.


Under the **Voice Configuration** section, set **A Call Comes In** to **Webhook** and enter the URL for your TwiML endpoint, e.g.,:` https://1234abcd.ngrok.app/twiml` .


Replace` 1234abcd.ngrok.app` with your actual ngrok hostname — including the protocol. Set the HTTP method to **HTTP POST** , then click **Save Configuration** .


## Testing your agent


Call your Twilio phone number. You should hear Hoot's greeting within a second or two of the call connecting:


> "Thanks for calling Owl Air! I'm Hoot. I can help with flight status, baggage policy, loyalty points, or booking changes. Which of those can I help you with?"


Try a few test questions to verify the full flow is working:


- "What's the baggage policy?" - Hoot should describe carry-on and checked bag rules in natural spoken language.
- "How do loyalty points work?" - Hoot should explain the earn and redemption rates.
- "Can I change my flight?" - Hoot should give the change fee policy, with amounts spelled out in words.
- "When does check-in open?" - Hoot should mention the twenty-four hour window.
- "What's the status of flight OA205?" - Hoot should call the` lookup_flight_status` tool and report that the flight is delayed by forty minutes. You will see the tool call and its result printed in your terminal.
- "What's the status of flight OA318?" - Hoot should report the flight is cancelled.
- "What's the status of flight OA999?" - Hoot should gracefully report that no flight was found with that number.


Watch your terminal as you speak. You will see each caller turn printed as it arrives, Hoot's streamed response tokens accumulating, and the complete response logged once the turn is finished.


If Hoot does not respond, check that your ngrok tunnel is still running and that the` DOMAIN` environment variable matches your current ngrok hostname. ngrok generates a new hostname each time you restart the free tier, so you will need to update your Twilio webhook URL and restart mezzio-swoole whenever you restart ngrok.


If you hear an error message on the call or see an` error` message in your terminal logs, check the error code against the[Conversation Relay error code reference](https://www.twilio.com/docs/voice/conversationrelay/websocket-messages#error-message) in the Twilio documentation.


## What's next


The agent you built here covers the core Conversation Relay pattern, but there are several natural extensions worth exploring.


**Proactive topic re-offering** - If a caller seems uncertain, Hoot will stay silent and wait. You can fix this with a single addition to the system prompt: Instruct Hoot to re-offer the four topic areas if the caller has not asked a clear question after one or two exchanges. No code change required.


**Human handoff** - The system prompt already tells Hoot to offer to connect callers with a live agent. To implement it, detect when the model's response includes that offer and send` {"type": "end-session", "handoffData": "caller needs live support"}` from the WebSocket handler. You will also need to add an` action` attribute to the` <ConversationRelay>` element in your TwiML (e.g.` action="/handoff"` ). Twilio POSTs to that URL when the session ends, passing the` handoffData` string, where you can return new TwiML to route the call to a queue or a direct number using standard` <Dial>` logic.


**Caller personalization** - You can pass data into the` setup` message using` <Parameter>` elements inside` <Connect>` in your TwiML. For example,` <Parameter name="customerName" value="Alex"/>` makes` customParameters.customerName` available in the` setup` message, so you can have Hoot greet the caller by name. In a real deployment you would look up the caller's name from the` From` phone number before generating the TwiML.


## Conclusion


You have built a fully functional AI phone agent using Twilio Conversation Relay, PHP, and the OpenAI API. Conversation Relay handled the speech-to-text and text-to-speech pipeline, while your WebSocket server connected the model to the call and managed the conversation history, including live flight lookups via tool calling. See the[Conversation Relay documentation](https://www.twilio.com/docs/voice/twiml/connect/conversationrelay) for the full list of attributes and message types.


*Matthew Setter is a PHP and Go Editor in the Twilio Voices team. He’s also the author of[Mezzio Essentials](https://mezzioessentials.com/) and[Deploy with Docker Compose](https://deploywithdockercompose.com/) . You can find him atmsetter@twilio.com . He's also on[LinkedIn](https://www.linkedin.com/in/matthewsetter/) and[GitHub](https://github.com/settermjd) .*
