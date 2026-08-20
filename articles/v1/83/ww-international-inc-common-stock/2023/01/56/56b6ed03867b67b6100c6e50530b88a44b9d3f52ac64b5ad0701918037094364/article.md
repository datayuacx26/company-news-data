---
schema_version: "1.0.0"
document_id: "56b6ed03867b67b6100c6e50530b88a44b9d3f52ac64b5ad0701918037094364"
company_key: "ww-international-inc-common-stock"
company: "WW International Inc."
source_id: "ww-international-inc-common-stock-rss-f4e3db081dfb"
canonical_url: "https://medium.com/ww-tech-blog/springboot-java-application-observability-with-opentelemetry-newrelic-part-1-1948cc819653"
published_at: "2023-01-04T22:25:27+00:00"
first_seen_at: "2026-07-20T23:20:13.695109+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:afdd5a499cce0c2c9b3e3f17b3aca9fd38e23ee0027f5e6f7557d069cd172f03"
---

# Springboot/Java application observability with OpenTelemetry & NewRelic — Part 1

Opentelemetry


Otel


New Relic


Fluentd


Spring Boot


# Springboot/Java application observability with OpenTelemetry & NewRelic — Part 1


[Amit Misra](https://medium.com/@amit.misra?source=post_page---byline--1948cc819653---------------------------------------)


4 min read


·


Jan 4, 2023


--


Weaving OpenTelemetry, FluentD, and NewRelic for complete observability


## Observability: more is less


Being able to track individual transactions and see application logs in one place is a requirement when we are dealing with a micro-services explosion. You can head over here to read more about[observability](https://opentelemetry.io/docs/concepts/observability-primer/) and[distributed traces](https://opentelemetry.io/docs/concepts/signals/traces/) . There are two parts to this puzzle though. We need the ability to generate traces and spans, usually achieved by instrumenting the method calls and tying application logs to these traces and spans.


We will walk through the entire process and, in the end, you will have a working example that would look something like this on NewRelic. The source code of the working example can be found[here](https://github.com/amitmisra-ww/otel-service) .


## Trace and span view


Press enter or click to view image in full size


## Application logs associated with those traces and spans


Press enter or click to view image in full size


## Trace and span setup


Setting up traces and spans in a Springboot application is relatively simple. We will use


- Free account setup for NewRelic.
- Springboot application with Log4j2 logging in JSON format.
- Wrap up the application with OpenTelemetry instrumentation.
- Use Docker to bring-up OpenTelemetry Collector to talk to NewRelic.


## NewRelic account setup


If you already have an account then simply move to the next step. Otherwise, follow along. You can sign up for a free NewRelic account using the following[link](https://newrelic.com/signup?via=login) .


## API Keys


From Account Settings, head over to the API Keys as shown below.


Create a new **ingest– (or license)** key and provide a friendly name to the key on the following screen.


Press enter or click to view image in full size


Take note of the key being generated. We need to pass on this information to the OpenTelemetry Collector later. In case you already created a key before and you want to reuse the same key you can get a key like so -


Press enter or click to view image in full size


## SpringBoot application with Log4j2


Head over to[start.spring.io](https://start.spring.io/) and create a new springboot project as per your liking or simply use this[configuration](https://start.spring.io/#!type=maven-project&language=java&platformVersion=2.7.3&packaging=jar&jvmVersion=11&groupId=com.example.otelservice&artifactId=otel-service&name=otel-service&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.otelservice.otel-service&dependencies=web,devtools,lombok) of the spring initializr to download the projectused in this demo.


## Additional dependencies


Modify the pom.xml file for the project you just created above by adding the following properties/dependencies:


### Properties


Add the following property to the “properties” section of your pom.xml.


> <opentelemetry.version>1.16.0</opentelemetry.version>


### Dependencies


Head over to the dependencyManagement section and add the following snippet to your pom.xml.


> <dependencyManagement>
>
>
> <dependencies>
>
>
> <dependency>
>
>
> <groupId>io.opentelemetry</groupId>
>
>
> <artifactId>opentelemetry-bom</artifactId>
>
>
> <version>${opentelemetry.version}</version>
>
>
> <type>pom</type>
>
>
> <scope>import</scope>
>
>
> </dependency>
>
>
> </dependencies>
>
>
> </dependencyManagement>


Additionally, add the dependencies for Log4j2 and OpenTelemetry, like so:


> <! — Log4j2 →
>
>
> <dependency>
>
>
> <groupId>org.springframework.boot</groupId>
>
>
> <artifactId>spring-boot-starter-log4j2</artifactId>
>
>
> </dependency>
>
>
> <dependency>
>
>
> <groupId>org.apache.logging.log4j</groupId>
>
>
> <artifactId>log4j-layout-template-json</artifactId>
>
>
> <version>2.17.2</version>
>
>
> </dependency>
>
>
> <! — OpenTelemetry →
>
>
> <dependency>
>
>
> <groupId>io.opentelemetry</groupId>
>
>
> <artifactId>opentelemetry-exporter-otlp</artifactId>
>
>
> </dependency>
>
>
> <dependency>
>
>
> <groupId>io.opentelemetry</groupId>
>
>
> <artifactId>opentelemetry-extension-annotations</artifactId>
>
>
> </dependency>


Furthermore, we need to ensure that we are excluding Log4j version 1 libraries like so:


> <dependency>
>
>
> <groupId>org.springframework.boot</groupId>
>
>
> <artifactId>spring-boot-starter-web</artifactId>
>
>
> <exclusions>
>
>
> <exclusion>
>
>
> <groupId>org.springframework.boot</groupId>
>
>
> <artifactId>spring-boot-starter-logging</artifactId>
>
>
> </exclusion>
>
>
> </exclusions>
>
>
> </dependency>


## New controller and service


This is the most boring controller and service you will ever see, but it is good enough for the purpose of this demonstration.


### GreetingService


Create a new service.


> @Service
>
>
> @Slf4j
>
>
> public class GreetingService {
>
>
> @WithSpan
>
>
> public String greet(@SpanAttribute String name) {
>
>
> Span. *current* ().setAttribute(“param-name”, name);
>
>
> *log* .info(“greet service invoked”);
>
>
> return String. *format* (“Hello %s”, name);
>
>
> }
>
>
> }


### GreetingController


Create a new controller.


> @RestController(“/”)
>
>
> @Slf4j
>
>
> public class GreetingController {
>
>
> private final GreetingService greetingService;
>
>
> @Autowired
>
>
> public GreetingController(GreetingService greetingService) {
>
>
> this.greetingService = greetingService;
>
>
> }
>
>
> @GetMapping(“/hello”)
>
>
> @ResponseBody
>
>
> @WithSpan
>
>
> public String greet(@RequestParam(“name”) String name) {
>
>
> *log* .info(“Greet Controller invoked”);
>
>
> return greetingService.greet(name);
>
>
> }
>
>
> }


At this point, we have our controller ready and it is communicating with our service. We can test this out by running the application either using the IDE or maven. The following curl should respond with “Hello OpenTelemetry” — curl[http://localhost:8080/hello\\?name\\=OpenTelemetry](http://localhost:8080/hello%5C?name%5C=OpenTelemetry)
