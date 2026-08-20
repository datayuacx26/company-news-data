---
schema_version: "1.0.0"
document_id: "38d270e5aaec0f8b1456826b47bb9a80200ae30de550d734d4e9c83f9fe55044"
company_key: "yc-signoz"
company: "SigNoz"
source_id: "yc-signoz-rss-564a62b873f8"
canonical_url: "https://signoz.io/blog/winston-logger"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.602972+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:7df8a92c8a2e67e3ff4430893b7f02aa24e4fc75b8970239fa764053da7c1aa7"
---

# Winston Logger: Node.js Logging Guide

# Winston Logger: Node.js Logging Guide


Last Updated: June 11, 2026


13 min read


Winston logger is a simple[Node.js logging](https://signoz.io/guides/nodejs-logging/) library. It was created to give your Node.js applications a flexible and centralized way to manage logs across multiple destinations, formats, and severity levels without tying the application to a single logging output or storage mechanism.


Developers choose the Winston logger when building internal tools, enterprise applications, traditional backend systems, or when they need multiple transports. A transport can be described as a storage device for your logs. Multiple transports can be configured on a single Winston logger instance at different logging levels. For example, you can configure a transport to send error logs to a database, while all logs can be just printed to the console during development.


Winston offers key features such as multiple transports, logging levels, custom formatting, child loggers, profiling, querying and exception handling. Each of these features is explained in detail in this article with the help of an example for each.


## Quick Start - Install and Setup Winston Logger


In this section, we will install Winston and set up a basic logger in a Node.js application.


### Step 1: Install NPM Winston


```text
npm    install   winston


```


### Step 2: Create a Basic Logger


Create a file called` logger.js` and create a basic logger.


logger.js


```text
const   winston  =    require  (  'winston'  )  ;


// Create a new Winston logger instance
const   logger  =   winston .  createLogger  (  {


// Set the minimum log level to record Logs with level "info" and above
level  :    'info'  ,


// Define the log message format
format  :   winston .  format  .  simple  (  )  ,


// Configure where logs should be sent
transports  :    [
new    winston .  transports .  Console  (  )
]
}  )  ;


logger .  info  (  'This is an info message'  )  ;
logger .  warn  (  'This is a warning message'  )  ;
logger .  error  (  'This is an error message'  )  ;


```


### Step 3: Run and Verify


Run the` logger.js` file and view the generated output in the console.


```text
node   logger.js


```


Output


```text
info: This is an info message
warn: This is a warning message
error: This is an error message


```


The basic Winston logger is now configured. You can modify this logger as per your needs. Follow the next sections to understand each feature in detail and how to use them with the logger.


## Transports in Winston


Transports are the simple destination where your logs are sent. The basic example is to send logs to the console during development.


logger.js


```text
const   winston  =    require  (  'winston'  )  ;


const   logger  =   winston .  createLogger  (  {


// Configured transposts to send log to console
transports  :    [
new    winston .  transports .  Console  (  )
]
}  )  ;


// Log will only be printed in the console
logger .  info  (  'Hello from basic transport!'  )  ;


```


You can set up multiple transports simultaneously in a single Winston logger instance. They can be used to route the logs of different severity levels to different destinations or to apply different formats depending on the destination (e.g., colorized text for the console, pure JSON for a file).


This is helpful in production, where you want real-time visibility in the console while also persisting logs to disk for later analysis. Each transport can have its own log level, so you could send only errors to a file but all messages to the console.


logger.js


```text
const   winston  =    require  (  'winston'  )  ;


const   logger  =   winston .  createLogger  (  {
transports  :    [


// Print the logs to console
new    winston .  transports .  Console  (  {    level  :    'info'    }  )  ,


// Send error logs to app.log file
new    winston .  transports .  File  (  {    filename  :    'app.log'  ,    level  :    'error'    }  )
]
}  )  ;


logger .  info  (  'Shown only in console'  )  ;
logger .  error  (  'Shown in console AND saved to app.log'  )  ;


// Output:
// {"level":"info","message":"Shown only in console"}
// {"level":"error","message":"Shown in console AND saved to app.log"}


```


You can also build a custom transport yourself by extending the base Transport class. It gives you full control over where and how logs are handled, such as sending them to a database, Slack, or an external API. You just need to implement a` log()` method that defines exactly what happens when a message arrives. This is the go-to approach when built-in transports don't fit your specific needs.


logger.js


```text
const    Transport    =    require  (  'winston-transport'  )  ;
const   winston  =    require  (  'winston'  )  ;


class    MyCustomTransport    extends    Transport    {
log  (  info ,   callback  )    {
console  .  log  (  `  [CUSTOM]   ${  info .  level  }   :   ${  info .  message  }   `   )  ;
callback  (  )  ;
}
}


const   logger  =   winston .  createLogger  (  {
transports  :    [  new    MyCustomTransport  (  )  ]
}  )  ;


logger .  info  (  'Handled by my custom transport!'  )  ;


//Output: [CUSTOM] info: Handled by my custom transport!


```


## Logging Levels in Winston


The Winston log levels follow the severity ordering specified by[RFC5424](https://tools.ietf.org/html/rfc5424) , which assumes the levels to be numerically ordered from most important to least important. By default, Winston uses NPM style[logging levels](https://signoz.io/blog/logging-levels/) (` 0-highest` to` 6-lowest` ):


```text
{
error  :    0  ,       // Something broke
warn  :    1  ,        // Something might be wrong
info  :    2  ,        // General information
http  :    3  ,        // HTTP request logs
verbose  :    4  ,     // Detailed information
debug  :    5  ,       // Debugging details
silly  :    6        // Log absolutely everything
}


```


The messages can be logged by calling the level name as a method or by passing the level as a string to the` log()` method.


logger.js


```text
...
// Using the level as a method (preferred, cleaner)
logger .  info  (  'User signed up successfully'  )  ;
logger .  error  (  'Failed to connect to database'  )  ;


// Using log() with the level as a string
logger .  log  (  'info'  ,    'User signed up successfully'  )  ;
logger .  log  (  'error'  ,    'Failed to connect to database'  )  ;
...


```


Both approaches output the same result. The method-based style is more common because it is shorter and easier to read.


Winston logger also allows you to add levels on transport so that Winston will only log that level and everything above it (i.e., everything with a lower number). Messages below that threshold are ignored. For example, if you set a transport's level to` warn` , it will log` warn` (1) and` error` (0), but skip` info` (2),` debug` (5), and everything else. This lets you keep your console clean during production while still capturing detailed logs in a file.


Along with the pre-defined` npm` ,` syslog` , and` cli` log levels available in` winston` , you can also customize them. Just pass an object mapping level names to numeric priorities:


logger.js


```text
const   winston  =    require  (  'winston'  )  ;


const   customLevels  =    {
critical  :    0  ,
important  :    1  ,
normal  :    2  ,
trivial  :    3
}  ;


const   logger  =   winston .  createLogger  (  {
levels  :   customLevels ,
level  :    'normal'  ,
transports  :    [
new    winston .  transports .  Console  (  )
]
}  )  ;


// Winston automatically creates methods for each custom level
logger .  critical  (  'System is on fire'  )  ;
logger .  normal  (  'User updated their profile'  )  ;
logger .  trivial  (  'Button hover event tracked'  )  ;


```


Output


```text
{  "level"  :  "critical"  , "message"  :  "System is on fire"  }
{  "level"  :  "normal"  , "message"  :  "User updated their profile"  }


```


## Winston Formats


In Winston logger, formats dictate how log messages are transformed, structured, and serialized before they are output to a specific transports. Every log entry starts as an info object containing properties like level, message, timestamp, and metadata. Formats modify this object step by step before generating the final output.


```text
const   info  =    {
level  :    'info'  ,
message  :    'Server started on port 3000'
}


```


The following are the different types of Winston Formats.


1.


**Built-in Formats**


Winston provides several built-in formats for common logging needs such as JSON logging, timestamps, colorized console output, metadata handling, and custom message formatting. These formats can be accessed by using` winston.format` . Below are some of the most commonly used built-in Winston formats.


` json()` - It converts log entries into structured JSON format.


```text
const   jsonFormat  =   format .  json  (  )  ;


const   info  =   jsonFormat .  transform  (  {
level  :    'info'  ,
message  :    'my message'  ,
}  )  ;


```


` simple()` - It generates a plain-text log output containing the log level and message.


```text
const   simpleFormat  =   format .  simple  (  )  ;


const   info  =   simpleFormat .  transform  (  {
level  :    'info'  ,
message  :    'my message'  ,
number  :    123
}  )  ;


```


` colorize()` - Adds ANSI colour codes to the` level` or` message` for terminal readability.


```text
const   colorizeFormat  =   format .  colorize  (  {    colors  :    {    info  :    'blue'    }  }  )  ;


const   info  =   colorizeFormat .  transform  (  {
[  LEVEL  ]  :    'info'  ,
level  :    'info'  ,
message  :    'my message'
}  ,    {    all  :    true    }  )  ;


```


` timestamp()` - It appends a timestamp property to the` info` object.


```text
const   timestampFormat  =   format .  timestamp  (  )  ;


const   info  =   timestampFormat .  transform  (  {
level  :    'info'  ,
message  :    'my message'
}  )  ;


```


` printf()` - It allows you to define a completely custom log message format.


```text
const   myFormat  =   format .  printf  (  (  info  )    =>    {
return    `  ${  info .  level  }      ${  info .  message  }   `   ;
}  )


const   info  =   myFormat .  transform  (  {
level  :    'info'  ,
message  :    'my message'
}  )  ;


```


` errors({ stack: true })` - Catches standard JavaScript` Error` objects and ensures their stack traces are included in the log output.


```text
const   errorsFormat  =    errors  (  {    stack  :    true    }  )


const   info  =   errorsFormat .  transform  (  new    Error  (  'Oh no!'  )  )  ;


```


2.


**Combined Formats (` format.combine` )**


Winston allows you to chain multiple formats together using` format.combine()` . The formats are then executed in the order they are defined, operating on the` info` object sequentially like a pipeline.


For example:


logger.js


```text
const   winston  =    require  (  'winston'  )  ;
const    {   combine ,   timestamp ,   printf ,   colorize  }    =   winston .  format  ;


// Define a custom string format for the final output
const   myFormat  =    printf  (  (  {   level ,   message ,   timestamp  }   )    =>    {
return    `  ${  timestamp }    [  ${  level }   ]:   ${  message }   `   ;
}  )  ;


const   pipelineLogger  =   winston .  createLogger  (  {
level  :    'debug'  ,
format  :    combine  (
colorize  (  )  ,             // 1. Colorize the level
timestamp  (  )  ,            // 2. Add a timestamp property
myFormat              // 3. Format the final string
)  ,
transports  :    [  new    winston .  transports .  Console  (  )  ]
}  )  ;


pipelineLogger .  debug  (  'Debugging database connection'  )  ;
// Output: 2026-04-21T06:16:41.000Z [\u001b[34mdebug\u001b[39m]: Debugging database connection


```


3.


**Custom Formats**


If the built in Winston formats do not meet your exact requirements, you can create a custom format using` format(transformFn)` . The transform function takes the` info` object and an optional` opts` object, modifies the` info` object, and returns it. If the function returns` false` , the log message is completely ignored or filtered out.


For example:


logger.js


```text
const   winston  =    require  (  'winston'  )  ;


// Create a custom format that redacts sensitive data
const   redactData  =   winston .  format  (  (  info ,   opts  )    =>    {
if    (  info .  password  )    {
info .  password    =    '[REDACTED]'  ;
}
if    (  info .  creditCard  )    {
info .  creditCard    =    '**-**-****-'    +   info .  creditCard  .  slice  (  -  4  )  ;
}
return   info ;    // Must return the modified info object
}  )  ;


const   customLogger  =   winston .  createLogger  (  {
level  :    'info'  ,
format  :   winston .  format  .  combine  (
redactData  (  )  ,
winston .  format  .  json  (  )
)  ,
transports  :    [  new    winston .  transports .  Console  (  )  ]
}  )  ;


customLogger .  info  (  'User registration attempt'  ,    {
user  :    'admin'  ,
password  :    'supersecretpassword123'
}  )  ;


//Output: {"level":"info","message":"User registration attempt","password":"[REDACTED]","user":"admin"}


```


## Exceptions and Rejections in Winston


Winston logger intercepts and logs unhandled errors that would otherwise cause your Node.js process to crash. By default, Winston only logs what you explicitly tell it to. However, these two features allow it to "listen" for global failures.


#### Handling Uncaught Exceptions


An uncaught exception occurs when a synchronous error is thrown and not caught by a` try...catch` block. Normally, Node.js prints the stack trace to the` stderr` stream and exits with code` 1` . When you configure` exceptionHandlers` Winston logger, the logger attaches a listener to the` process.on('uncaughtException')` event.


By default, Winston will exit the process after logging an exception. You can override this by setting` exitOnError: false` , though this is generally discouraged as the process state may be corrupted. You can also direct exceptions to a specific file or remote endpoint separate from your standard application logs.


logger.js


```text
const   winston  =    require  (  'winston'  )  ;


const   logger  =   winston .  createLogger  (  {
transports  :    [
new    winston .  transports .  File  (  {    filename  :    'combined.log'    }  )
]  ,
exceptionHandlers  :    [
// This will catch any uncaught synchronous errors
new    winston .  transports .  File  (  {    filename  :    'exceptions.log'    }  )
]
}  )  ;


// Triggering an uncaught exception
throw    new    Error  (  'This will be caught by the exceptionHandler'  )  ;


```


#### Handling Unhandled Rejections


An unhandled rejection occurs when a Promise is rejected (e.g., an API call fails) and there is no` .catch()` block or` await` inside a` try...catch` to handle it.


For example:


logger.js


```text
const   winston  =    require  (  'winston'  )  ;


const   logger  =   winston .  createLogger  (  {
transports  :    [
new    winston .  transports .  Console  (  )
]  ,
rejectionHandlers  :    [
// This will catch any unhandled promise rejections
new    winston .  transports .  File  (  {    filename  :    'rejections.log'    }  )
]
}  )  ;


// Triggering an unhandled rejection
Promise  .  reject  (  new    Error  (  'Async failure captured by rejectionHandler'  )  )  ;


```


## Profiling in Winston


Profiling in Winston is a lightweight mechanism for measuring the execution time of specific code blocks. It works by capturing a high-resolution timestamp when you "start" a profile, calculating the delta (duration) when you "stop" it, and then automatically emitting a log entry.


**How does it work?**


The profiling system uses a stateful timer keyed by a unique string (the "id"). The profiling can be split into two phases.


Start Phase - When` logger.profile(id)` is called for the first time with a specific ID, Winston stores the current timestamp in an internal map associated with that ID.


End Phase - When` logger.profile(id)` is called a second time with the same ID, Winston logger retrieves the stored timestamp, calculates the difference against the current time, deletes the ID from the internal map to free memory, and finally outputs a log message at the` info` level (by default) containing the duration in milliseconds.


The following example demonstrates how to profile a synchronous or asynchronous operation without using external hooks or abstractions.


logger.js


```text
const   winston  =    require  (  'winston'  )  ;


// 1. Set up a basic logger
const   logger  =   winston .  createLogger  (  {
level  :    'debug'  ,
format  :   winston .  format  .  combine  (
winston .  format  .  timestamp  (  )  ,
winston .  format  .  json  (  )
)  ,
transports  :    [  new    winston .  transports .  Console  (  )  ]
}  )  ;


async    function    heavyTask  (  )    {
// 2. Start the timer for 'database-query'
logger .  profile  (  'database-query'  )  ;


// Simulate an asynchronous operation (e.g., DB call)
await    new    Promise  (  resolve    =>    setTimeout  (  resolve ,    1500  )  )  ;


// 3. End the timer.
// Winston calculates the delta and logs it automatically.
logger .  profile  (  'database-query'  ,    {
message  :    'Completed fetching user records'  ,
level  :    'debug'    // Optional: override default 'info' level
}  )  ;
}


heavyTask  (  )  ;


```


The output will include a` durationMs` field, which is injected automatically by the profiling logic.


Output


```text
{
"level"  :    "debug"  ,
"message"  :    "Completed fetching user records"  ,
"timestamp"  :    "2026-04-21T16:20:00.000Z"  ,
"durationMs"  :    1501
}


```


## Centralizing Logs in a Log Management Tool


A Node.js service usually writes logs to stdout, local files, or both. That works during local development, but it becomes painful in production. If logs stay on individual hosts or short-lived containers, debugging often means SSHing into a server or running` kubectl logs` before the pod is replaced and the output is gone. Once you have more than one instance, that workflow does not scale.


A[log management tool](https://signoz.io/blog/best-open-source-log-management-tools/) centralizes those logs so you can search them, filter by metadata, build dashboards, and create alerts. In this section, we will send Winston logs from a Node.js application to[SigNoz](https://signoz.io/) , an all-in-one observability platform for logs, metrics, and traces. SigNoz supports[OpenTelemetry](https://signoz.io/opentelemetry/) , so the same logging approach can also be adapted for other OTLP-compatible backends, usually by changing exporter configuration rather than rewriting your logging calls.


The main benefit is log-trace correlation. When Winston logs are emitted inside an active OpenTelemetry span, they can include fields such as` trace_id` and` span_id` . That lets you move from a slow or failing trace to the log lines created during the same request. You can also query[structured log](https://signoz.io/blog/structured-logs/) attributes,[create log-based alerts](https://signoz.io/docs/alerts-management/log-based-alerts/) , and build dashboards without changing every existing` logger.info()` or` logger.error()` call site.


For setup instructions, including both the no-code auto-instrumentation path and the code-level SDK setup, head over to the official guide,[Send logs from Node.js Winston logger to SigNoz](https://signoz.io/docs/logs-management/send-logs/nodejs-winston-logs/) .


## Conclusion


Winston logger gives Node.js applications the core pieces needed for application logging. The key features, such as transports for sending logs to different destinations, configurable log levels, flexible formatting, exception and rejection handling, and simple profiling for measuring how long operations take, make it a better choice for logging inside a single Node.js process, especially when you need control over where logs go and how they are structured.


In production, Winston works best as part of a centralized logging setup. Local files and container output are difficult to search across once an application runs on multiple instances. Sending Winston logs to a tool like SigNoz gives teams one place to query logs, create alerts, and connect log records with traces when OpenTelemetry instrumentation is enabled.
