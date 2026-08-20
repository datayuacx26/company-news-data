---
schema_version: "1.0.0"
document_id: "99b7183ee2b7651a22d7a66f72fcde419543296fff1b9728a76e642cf4754b6d"
company_key: "ww-international-inc-common-stock"
company: "WW International Inc."
source_id: "ww-international-inc-common-stock-rss-f4e3db081dfb"
canonical_url: "https://medium.com/ww-tech-blog/springboot-java-application-observability-with-opentelemetry-newrelic-part-2-ebc5c24fc3b7"
published_at: "2023-01-04T22:42:02+00:00"
first_seen_at: "2026-07-20T23:20:13.695109+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:371c0151ac4bf17b4d52b6dd08d0c096a72002922bd9fa480dfa05daeb6f40da"
---

# Springboot/Java application observability with OpenTelemetry & NewRelic — Part 2

# Springboot/Java application observability with OpenTelemetry & NewRelic — Part 2


[Amit Misra](https://medium.com/@amit.misra?source=post_page---byline--ebc5c24fc3b7---------------------------------------)


5 min read


·


Jan 4, 2023


--


## Logs in JSON format


In order to get the logs in JSON format, we need to let the Log4j2 know about the layout and we will use a template for our logs which will look like this:


### JsonTemplateLayout


Create a new file under the src/main/resources folder by the name — Log4jEventLayout.json.


You can find additional details about the template layout[here](https://logging.apache.org/log4j/2.x/manual/json-template-layout.html) .


Couple of things to note here:


1. You would see we are duplicating trace_id and span_id by adding trace.id and span.id. This is intentional as NewRelic does not understand trace_id and span_ids.
2. The service.name is externalized and will be replaced by the value stored in the environment variable OTEL_SERVICE_NAME variable.


### Letting Log4j2 know about this configuration


This can be done as follows:


> <?xml version=”1.0" encoding=”UTF-8"?>
>
>
> <Configuration status=”WARN” monitorInterval=”30">
>
>
> <Appenders>
>
>
> <Console name=”ConsoleAppender” target=”SYSTEM_OUT” follow=”true”>
>
>
> <JsonTemplateLayout eventTemplateUri=”classpath:Log4j2EventLayout.json”/>
>
>
> </Console>
>
>
> </Appenders>
>
>
> <Loggers>
>
>
> <Root level=”info”>
>
>
> <AppenderRef ref=”ConsoleAppender” />
>
>
> </Root>
>
>
> </Loggers>
>
>
> </Configuration>


At this point, all the logs spit out by our application should be in JSON format.


> {“timestamp”:”2022–08–19T12:55:33.485–0400",”thread.name”:”restartedMain”,”log.level”:”INFO”,”logger.name”:”com.example.otelservice.OtelServiceApplication”,”message”:”No active profile set, falling back to 1 default profile: \\”default\\””,”service.name”:”wcp-otel-service”}


Some samples with the span and trace details.


> {“timestamp”:”2022–08–19T13:14:56.615–0400",”thread.name”:”http-nio-8080-exec-1",”log.level”:”INFO”,”logger.name”:”com.example.otelservice.service.GreetingService”,”message”:”greet service invoked”,”trace_id”:”f05a3a2dfcc8d04d4f18beaa11f17909",”span_id”:”88477fb780ebf427",”trace.id”:”f05a3a2dfcc8d04d4f18beaa11f17909",”span.id”:”88477fb780ebf427",”service.name”:”wcp-otel-service”}


## OpenTelemetry instrumentation


As per the[instructions](https://opentelemetry.io/docs/instrumentation/java/automatic/) on the OpenTelemetry website, we need to add a java agent to your application. This is going to be a two-step process. First download and stage the instrument library at a known location, and then configure the spring boot maven plugin to use the java agent accordingly. This is accomplished using the maven-dependency-plugin. Refer to the pom.xml file for the project.


## Introduction


Before diving into the implementation details, let’s take a look at the high-level architecture of the[collector](https://opentelemetry.io/docs/collector/) . The following diagram is from their website:


Press enter or click to view image in full size


The OpenTelementry collector is based on a pluggable model with three primary components as can be seen above. You have the


1. Receiver
2. Processor
3. Exporter


## Set up otel-collector


For this demo, we are going to use the docker image provided by opentelemetry-collector-contrib. We will start off by creating two files.


- otel-config.yaml
- docker-comopse.yaml


Please refer to the[github](https://github.com/amitmisra-ww/otel-service) project for references to these files.


## Bringing up the docker instances


At this point, we are ready to take our application and OTel Collector for a spin. Before we bring up the Oel Collector, we need to export the NewRelic API key. Export your API key like so — export NEW_RELIC_API_KEY=<YOUR_API_KEY>


Now we can start the docker instances by firing up the following command: ./mvnw clean install && docker compose up –build


The first part of the command outputs the uber-jar and places the opentelemetry-javaagent.jar file under target/agents directory. The second part of the command first builds the docker instance for our service and brings up the collector along with it.


Try to call our service a bunch of times using the following curl command — curl[http://localhost:8080/hello\\?name\\=OpenTelemetry](http://localhost:8080/hello%5C?name%5C=OpenTelemetry)


You should be able to see our new service under **Service — OpenTelemetry**


Press enter or click to view image in full size


Head over to the “Transactions” from the left navigation menu and you should see something like this.


Press enter or click to view image in full size


Click on one of the traces below you will be able to see one trace and a couple of spans.


Press enter or click to view image in full size


## Taking care of logs


Now that we have trace flow working, it would be nice to see the logs adjacent to our traces. This will help triage any issue we are trying to resolve. To get the logs exported from our application to NewRelic we will take a little help from[FluentD](https://www.fluentd.org/) . You can read more about FluentD and the underlying architecture[here](https://www.fluentd.org/architecture) .


## Log flow


At a high level, this is what the flow will look like.


Press enter or click to view image in full size


## Setup the Fluentd instance


The first step is to configure the Fluentd instance to receive incoming logs on port 24224. After receiving these logs, we will want to filter all the logs in JSON format and send it over to our OpenTelemetry forwardreceiver listening on port 8006.


Please refer to the fluentd.conf and the fluentd docker service in the docker-compose.yaml file in the[github](https://github.com/amitmisra-ww/otel-service) repo.


## Final Results


Once we have established the Fluentd instance, two things should happen on the NewRelic portal. Under the wcp-otel-service, when you click on Logs in the left navigation menu, you should see your application logs.


Press enter or click to view image in full size


Click on Open in the logs to get a view.


Press enter or click to view image in full size


This allows you to see all the application logs in one place, but we can do better than that. Earlier we said it will be really helpful to associate the traces and the logs. If we head over to the Transactions menu, you will see the usual transactions.


Press enter or click to view image in full size


Now if you click on one of the traces you should see something like this.


Press enter or click to view image in full size


Interestingly, now the Logs aren’t showing 0, but 2 2. Heading over the logs you should see something like this.


Press enter or click to view image in full size


Now we have all traces and spans. This should help to debug issues easier than before.


## Next Steps


In the subsequent post, I would like to cover the following topics:


- Logging exception traces appropriately
- Exposing critical metrics and sending them to NewRelic via our OpenTelemetry Collector.


**Interested in joining the WW team?** Check out our[careers page](https://www.weightwatchers.com/us/corporate-careers) to view technology job listings as well as open positions on other teams.
