---
schema_version: "1.0.0"
document_id: "f419cfcf8e6a3887cea77f458aab997179cce12225b0e9f9a4c92e3834a50b5a"
company_key: "yc-signoz"
company: "SigNoz"
source_id: "yc-signoz-rss-564a62b873f8"
canonical_url: "https://signoz.io/opentelemetry/logging-in-python"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.602972+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:0cd31bdb3942825a368f92102cfd95b45b623ded1c4ade44dc4446ebcdaf069e"
---

# Configure OpenTelemetry logging SDK in a Python application

# Configure OpenTelemetry logging SDK in a Python application


Published on: June 20, 2024


Last Updated: July 07, 2026


2 min read


This article is part of the **OpenTelemetry Python series** :


- Previous Article:[Create custom metrics in Python Application using OpenTelemetry](https://signoz.io/opentelemetry/python-custom-metrics/)
- **You are here:** Configure OpenTelemetry logging SDK in a Python application
- Next Article:[Customize metrics streams produced by OpenTelemetry SDK using views](https://signoz.io/opentelemetry/customize-metrics-streams-produced-by-opentelemetry-python-sdk/)


Check out the complete series at:[Overview - Implementing OpenTelemetry in Python applications](https://signoz.io/opentelemetry/python-overview/)


In the[previous tutorial](https://signoz.io/opentelemetry/python-custom-metrics/) , we learnt about custom metrics in Python. In this tutorial, we will look at how to configure OTel logging SDK in a Python application.


The OpenTelemetry SDK provides a handler that can be used to transport logs to any OTLP-compatible backend. The following code snippets show how to configure the OTel logging SDK in a Python application.


**


## Code Repo


Here’s the code repo for this tutorial:[GitHub repo link](https://github.com/SigNoz/opentelemetry-python-example/tree/main/lesson-6)


## Configure the logging SDK


```text
import   logging
from   opentelemetry .  _logs  import   set_logger_provider
from   opentelemetry .  exporter .  otlp .  proto .  grpc .  _log_exporter  import    (
OTLPLogExporter ,
)
from   opentelemetry .  sdk .  _logs  import   LoggerProvider ,   LoggingHandler
from   opentelemetry .  sdk .  _logs .  export  import   BatchLogRecordProcessor


logger_provider  =   LoggerProvider (  )
set_logger_provider (  logger_provider )


exporter  =   OTLPLogExporter (  )
logger_provider .  add_log_record_processor (  BatchLogRecordProcessor (  exporter )  )
handler  =   LoggingHandler (  level =  logging .  NOTSET ,   logger_provider =  logger_provider )


# Attach OTLP handler to root logger
logging .  getLogger (  )  .  addHandler (  handler )


```


In the above code snippet, a handler is created using the` LoggingHandler` class. The handler is attached to the root logger using the` addHandler()` method. The handler receives log records from the logger and sends them to the OTLP backend using the` OTLPLogExporter` .


## See your logs in SigNoz


Go to` Logs` tab of SigNoz and apply filter for your Flask application service. You will be able to see the logs coming from your application.


*Logs from your Flask application*


## Next Steps


In this tutorial, we configured the Python application to send logs to[SigNoz](https://signoz.io/docs/introduction/) using the OpenTelemetry logging SDK.


In the next tutorial, we will see how to customize the metrics stream produced by OpenTelemetry SDK using views.


Before we go ahead with that, you can also check out our adjoining write-up on[Python logging best practices](https://signoz.io/guides/python-logging-best-practices/) , to understand logging configuration in detail.


Read Next Article of OpenTelemetry Python series on[Customize metrics streams produced by OpenTelemetry SDK using views](https://signoz.io/opentelemetry/customize-metrics-streams-produced-by-opentelemetry-python-sdk/)
