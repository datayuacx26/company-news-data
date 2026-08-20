---
schema_version: "1.0.0"
document_id: "46ba4ab22f21654bb8e574dca770922c2b4f178d1119881f6f504b4796ae6bad"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/make-ai-voice-sound-more-human-less-robotic-php"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-23T13:09:03.715655+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:06afa36340b83ea404b43b298d4fda5f33dc78d773e16e3ffa986ffe1019265a"
---

# How to Make Your AI Voice Sound More Human and Less Robotic with PHP

Time to read:


-
-
-
-
-


July 23, 2026


**Written by**[Matthew Setter](https://www.twilio.com/en-us/blog/authors/author.msetter) Twilion


[Amanda Lange](https://www.twilio.com/en-us/blog/authors/author.amanda-lange) Twilion


---


## How to Make Your AI Voice Sound More Human and Less Robotic with PHP


If you've called a customer service helpline recently, chances are good that you've encountered some kind of AI assistant voice helper. But not all AI helpers are created equally. Some AI assistants are truly a pain to deal with, with long latency that creates a frustrating conversation. Others are more human-like, with a pleasant tone and cadence and the ability to handle your requests and interruptions gracefully.


What's the difference? If you want your AI to sound more human and less robotic on the phone, having a great simulated voice really helps. But aside from that, creating a low-latency bot that handles interruptions gracefully can be a big benefit to your users and make them feel more at ease.


In this tutorial, you will build a real-time voice agent using PHP,[Twilio Conversation Relay,](https://www.twilio.com/en-us/products/conversational-ai/conversationrelay) and[OpenAI](https://openai.com/) . In[previous tutorials](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay) , we've created an AI that operated as a concierge to help users with questions they have about our fictional airline, Owl Air. This time, you will program your AI as a flight assistant dealing with a panicked traveler who is running late. This will allow you to demonstrate and test two important principles that help make an AI sound more human: a patient tone, and gracefully handling user interruptions.


## Prerequisites


To complete this tutorial, be sure to have:


- [PHP](https://php.net/) 8.5 with[the Swoole extension](https://www.php.net/manual/en/book.swoole.php)
- A[Twilio Account](https://www.twilio.com/login) (with a voice-enabled phone number)
- An[OpenAI API Key](https://openai.com/index/openai-api/)
- ngrok (to tunnel your local development server to the internet)
- A text editor or IDE of your choice (such as[Visual Studio Code](https://code.visualstudio.com/) or[neovim](https://neovim.io/) )


## Building your application


### Step 1: Create a new PHP project


Open up your terminal and type the following to create a new PHP project.


Copy code


```text
mkdir -p \
VoiceHumanizer-PHP/src/Service \
VoiceHumanizer-PHP/src/WebSocket/Event


cd VoiceHumanizer-PHP
```


If you're using Microsoft Windows, replace the backslashes with carets (^).


Then, install the required dependencies:


Copy code


```text
composer require \
monolog/monolog \
nyholm/psr7 \
openai-php/client \
symfony/http-client \
twilio/sdk \
vlucas/phpdotenv
```


### Step 2: Setting up environment variables


Create a file named *.env* in your project folder. This will safely store your API key and your URL to avoid exposing them if you decide to use git or share your project in some other way. In this file, add the following information:


Copy code


```text
OPENAI_KEY=your_actual_openai_api_key_here
DOMAIN=ABC12345.ngrok.app
```


Replace the placeholder for your OpenAI key with your real OpenAI API key, found on[the OpenAI dashboard](https://platform.openai.com/api-keys) .


For` DOMAIN` , this is where you will add your ngrok tunneling url, when that is available in a future step.


### Step 3: Creating your API


When a customer calls your Twilio number, Twilio needs to fetch configuration instructions. You will write a small Swoole-powered endpoint that returns[TwiML](https://www.twilio.com/docs/voice/twiml) instructing Twilio to pipe the call details directly into our local PHP WebSocket.


Create a file called *index.php* in your project directory. Then, add the following code to the file:


Copy code


```text
<?php


declare(strict_types=1);


require_once("./vendor/autoload.php");


use App\WebSocket\WebSocketServer;
use Monolog\Handler\ErrorLogHandler;
use Monolog\Logger;


$dotenv = Dotenv\Dotenv::createImmutable(__DIR__);
$dotenv->load();
$dotenv->required('DOMAIN')->notEmpty();


$domain = $_ENV['DOMAIN'] ?? getenv('DOMAIN') ?? '';


$logger = new Logger('name');
$logger->pushHandler(new ErrorLogHandler());


$server = new WebSocketServer($domain, "0.0.0.0", 9501, $logger);
```


Then, in *src/WebSocket/WebSocketServer.php* , and in that file, paste the code below.


Copy code


```text
<?php


declare(strict_types=1);


namespace App\WebSocket;


use App\Service\OpenAiServiceFactory;
use App\WebSocket\Event\OnMessage;
use Psr\Log\LoggerInterface;
use Swoole\Http\Request as SwooleHttpRequest;
use Swoole\Http\Response as SwooleHttpResponse;
use Swoole\WebSocket\Server as SwooleWebSocketServer;
use Twilio\TwiML\VoiceResponse;


final class WebSocketServer
{
private SwooleWebSocketServer $server;


public function __construct(
private string $domain,
private string $ipAddress = "0.0.0.0",
private int $port = 9501,
private ?LoggerInterface $logger = null
) {
$this->server = new SwooleWebSocketServer($ipAddress, $port);


$this->server->on(
'message',
new OnMessage(
new OpenAiServiceFactory()->__invoke(),
$this->logger
),
);


$this->server->on('request', function (SwooleHttpRequest $request, SwooleHttpResponse $response) {
$voiceResponse = new VoiceResponse();
$connect       = $voiceResponse->connect();
$connect->conversationRelay(
[
'url'                         => "wss://{$this->domain}",
'welcomeGreeting'             => <<<EOF
Hey there!
Oh no, I see you missed your flight.
Don't worry, we'll get you sorted out.
Where are you trying to head?
EOF,
'ttsProvider'                 => 'ElevenLabs',
'transcriptionProvider'       => 'Deepgram',
'speechModel'                 => 'nova-3-general',
'eotThreshold'                => '0.8',
'ignoreBackchannel'           => 'true',
'interruptible'               => 'any',
'interruptSensitivity'        => 'medium',
'elevenlabsTextNormalization' => 'auto',
]
);
$response->setHeader('Content-Type', 'text/xml');
$response->end($voiceResponse->asXML());
});


$this->server->start();
}
}
```


The line` ttsProvider="ElevenLabs"` uses[ElevenLabs](https://elevenlabs.io/) for your TTS (Text to Speech). This gives you a more human-like voice cadence for conversations right out of the box. In this tutorial, you are using the` nova-3-general` speech model.


If you want more details about how some of these attributes work, check out our tutorial on Making an AI Phone Agent With Twilio Conversation Relay, which goes into the specifics about these attributes.


You might notice that your code is not yet complete, because you need to write the` OnMessage` and` OpenAiService` objects that actually process the incoming transcript. You will create those in the next step.


### Step 4: Core functionality and interruption handling


In this step you will write the core functionality for your AI. To make our AI sound human, this block of code needs to:


- Listen to incoming transcripts sent from Twilio.
- Stream OpenAI answers word-by-word back to Twilio over the WebSocket with minimal latency.
- Listen for the` interrupt` event from Twilio.


Create a new file named *OnMessage.php* in *src/WebSocket/Event* , and paste the code below into the file:


Copy code


```text
<?php


declare(strict_types=1);


namespace App\WebSocket\Event;


use App\Service\OpenAiService;
use Psr\Log\LoggerInterface;
use Swoole\WebSocket\Frame as SwooleWebSocketFrame;
use Swoole\WebSocket\Server as SwooleWebSocketServer;


use function json_encode;
use function sprintf;


final class OnMessage
{
public function __construct(
private readonly OpenAiService $openAi,
private readonly LoggerInterface $logger,
) {
}


public function __invoke(SwooleWebSocketServer $server, SwooleWebSocketFrame $frame): void
{
$data  = json_decode($frame->data, true);
if (! is_array($data) || $data === []) {
return;
}
$msgType = $data['type'] ?? '';


switch ($msgType) {
case 'setup':
$callSid = $data['callSid'] ?? null;
$this->logger->debug("[{$callSid}] Call connected\n");
break;


case 'prompt':
$last = $data['last'] ?? '';
if ($last === '') {
break;
}
$userText = $data['voicePrompt'] ?? '';
$this->logger->debug("[User Spoke]: {$userText}\n");
$responseText = $this->streamResponse($server, $frame->fd, $userText);
$this->logger->debug("Hoot: {$responseText}\n");
break;


case 'interrupt':
$this->logger->debug("Interrupted detected.\n");
$server->push($frame->fd, json_encode(['type' => 'clear']));
break;


case 'error':
$this->logger->debug(sprintf("Session error: %s\n", $data['description'] ?? ''));
break;
}
}
}
```


### Step 5: Structuring the streamed data for human-like conversation


This helper method calls OpenAI's chat completion endpoint, streams the response token-by-token, wraps the output in[SSML markup tags](https://en.wikipedia.org/wiki/Speech_Synthesis_Markup_Language) , and passes it directly to Twilio.


Add this method to the bottom of your *src/WebSocket/Event/OnMessage.php* file.


Copy code


```text
private function streamResponse(
SwooleWebSocketServer $server,
int $fd,
string $userText = ""
): string {
$fullResponse = '';
try {
$fullResponse = $this->openAi->streamResponse(
$userText,
static function (string $token) use ($server, $fd): void {
// Humanizer Trick: Dynamic SSML Formatting!
// If the LLM generates a dramatic ellipsis '...', turn it into an explicit breathing break.
if ($token === "...") {
$token = "<break time=\"200ms\"/>";
}
$server->push(
$fd,
// Format token to Twilio specifications
json_encode(
[
'type'  => 'text',
'token' => $token,
'last'  => false, // Tells Twilio to expect more streaming speech chunks
]
)
);
},
);
} finally {
$server->push(
$fd,
// Send a final token indicating that this conversational turn is complete
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
```


In *src/Service* create two new files named *OpenAiService.php* and *OpenAiServiceFactory.php* . In *src/Service/OpenAiService.php* , paste the following code:


Copy code


```text
<?php


declare(strict_types=1);


namespace App\Service;


use OpenAI\Client;
use function array_merge;


final class OpenAiService
{
private const MODEL = 'gpt-4o';


public const string PROMPT = <<<EOF
You are an empathetic, lightning-fast airport flight concierge.
Keep answers extremely short (under 2 sentences) because this is a phone call.
Insert a comma or ellipsis '...' where you want natural breathing pauses.
Use occasional casual conversational fillers like 'Oh, okay...', 'Uhm...', or 'Let me check...' to sound natural.
Use plain sentences only. No bullet points, no markdown, no emojis. Spell out all numbers (""thirty dollars"", not ""$30"").
EOF;


public function __construct(private readonly Client $client)
{
}


public function streamResponse(string $userInput, callable $onToken): string
{
$openAiMessages = array_merge(
[
[
'content' => self::PROMPT,
'role'    => 'system',
],
[
'content' => $userInput,
'role'    => 'user',
],
],
);


$stream = $this->client->chat()->createStreamed([
'model'      => self::MODEL,
'messages'   => $openAiMessages,
]);
$fullResponse = '';
$finishReason = null;


foreach ($stream as $response) {
$choice       = $response->choices[0];
$delta        = $choice->delta;
$finishReason = $choice->finishReason ?? $finishReason;


if ($delta->content === null || $delta->content === '') {
continue;
}


if ($delta->content === "[DONE]") {
break;
}


$fullResponse .= $delta->content;
($onToken)($delta->content);
}


return $fullResponse;
}
}
```


Focus your attention on the system prompts that we're creating for this AI. You are using system prompts to make the AI voice model mimic human conversational traits:


- Speaking in concise, run-on phrases rather than structured bullet points.
- Using micro-pauses (` <break time="150ms"/>` ) to simulate taking a breath.
- Occasional usage of natural conversational fillers ("Oh...", "I see...", "Give me just a second").
- Making sure that all numbers are articulated clearly, and the voice isn't trying to respond in emojis or a bulleted list.


Now, in *src/Service/OpenAiServiceFactory.php* , paste the following code:


Copy code


```text
<?php


declare(strict_types=1);


namespace App\Service;


use OpenAI;
use RuntimeException;
use function getenv;


final class OpenAiServiceFactory
{
public function __invoke(): OpenAiService
{
$apiKey = $_ENV[OPENAI_KEY] ?? getenv(OPENAI_KEY);
if (empty($apiKey)) {
throw new RuntimeException(OPENAI_KEY environment variable is not set.');
}


return new OpenAiService(OpenAI::client($apiKey));
}
}
```


This is a small factory class that simplifies instantiating an` OpenAiService` object.


## Testing Your Application


With all the code in place, you're ready to test your application.


In a new terminal tab or session expose your local port using ngrok:


Copy code


```text
ngrok http 9501
```


Ngrok will provide you with a secure url to hit with your web endpoint. Update the` DOMAIN` in your *.env* file to point to this new endpoint.


Now, in a separate terminal, start the PHP application:


Copy code


```text
php index.php
```


Point your Twilio Phone Number’s "A Call Comes In" webhook to your ngrok domain. In[the Twilio Console](https://console.twilio.com/) , navigate through **Products and Services > Numbers and Senders >** choose your number. Then go to **Voice and Emergency Calling** and click **Edit Configuration Details** in the upper right hand corner. Choose your appropriate country. Then go to **How do you want to set up your primary method?** And choose "Use Webhooks". Paste the webhook url from your ngrok endpoint, followed by` /voice` (as in`[https://1234abcd.ngrok.app/voice](https://1234abcd.ngrok.app/voice)` ). Set **Method to** "HTTP POST" from the dropdown menu. Then click **Save** .


Call your Twilio number to test your new bot!


Say hello, and observe how quickly the assistant answers you. Because we stream the output token-by-token directly from OpenAI, your assistant should react to your voice in under 500 milliseconds.


Now, try interrupting your agent as it's speaking to test its interruption handling. While the assistant is explaining flight details, talk directly over it saying: *"Wait! Stop, when does that flight leave?"* Because of your background task listener, the AI will immediately cease playing audio, clear its queue, and smoothly begin answering your interruption.


## Next steps


Making an AI sound human isn't about synthesizing a perfect accent: it is an engineering challenge. By ditching slow, request-response pipelines in favor of PHP WebSockets and[Twilio Conversation Relay](https://www.twilio.com/docs/voice/conversationrelay) , you can build voice systems that react in real-time, handle interruptions with grace, and speak with realistic pauses.


Your AI here isn't designed to collect any real information for the user and is just a quick demonstration, but there are a lot of additional features you could add. For example,[adding tool calling to your AI](https://www.twilio.com/en-us/blog/add-function-tool-calling-twilio-voice-openai-integration) could allow you to get real flight numbers as your user needs them. For more information, check out our Conversation Relay templates on GitHub. Happy building!


*Matthew Setter is a PHP and Go Editor in the Twilio Voices team. He’s also the author of[Mezzio Essentials](https://mezzioessentials.com/) and[Deploy with Docker Compose](https://deploywithdockercompose.com/) . You can find him atmsetter@twilio.com . He's also on[LinkedIn](https://www.linkedin.com/in/matthewsetter/) and[GitHub](https://github.com/settermjd) .*
