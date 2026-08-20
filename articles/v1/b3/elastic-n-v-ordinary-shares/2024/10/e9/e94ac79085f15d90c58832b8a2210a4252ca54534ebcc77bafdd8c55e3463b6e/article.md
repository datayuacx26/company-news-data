---
schema_version: "1.0.0"
document_id: "e94ac79085f15d90c58832b8a2210a4252ca54534ebcc77bafdd8c55e3463b6e"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/production-data-pre-production-environments-logstash"
published_at: "2024-10-01T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:9b5e8aaa58cf559abd054401b609c80a1a35c09d60b566699c5a214cf677f66f"
---

# Safely sample production data into pre-production environments with Logstash

# Safely sample production data into pre-production environments with Logstash


Safely route a subset of production data to pre-production clusters by leveraging UDP’s fire-and-forget functionality


By


[Alexander Marquardt](https://www.elastic.co/blog/author/alexander-marquardt)


October 1, 2024


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


In a well-architected system, it is a best practice to completely separate pre-production and production environments. Distinct environments ensure that issues in one environment will not affect the other, especially when it comes to testing new features or configurations in a pre-production environment.


However, for organizations with limited resources or operational constraints, maintaining separate end-to-end environments can sometimes be impractical. In such cases, workarounds are necessary to ensure that data can flow to both production and pre-production clusters without risk of disruption.


This blog post explores a solution that is intended for such constrained environments. Using Logstash in combination with UDP, the solution that is presented allows you to route a


**random subset**


of data to a pre-production cluster without a risk of interfering with the data flow to the production cluster. This is a lightweight and low-risk alternative to more complex patterns such as the


[output isolator](https://www.elastic.co/guide/en/logstash/current/pipeline-to-pipeline.html#output-isolator-pattern) pattern, which requires the use of a


[persistent queue](https://www.elastic.co/guide/en/logstash/8.15/persistent-queues.html) .


## Risks of driving data into multiple destinations


By default, if a Logstash pipeline routes data to multiple outputs, if one of the destinations goes down then the pipeline will be blocked for all destinations. For example, if you send data to both a production and to an pre-production cluster from a single Logstash pipeline with multiple outputs, if the pre-production cluster fails then Logstash will stop sending data to the production cluster as well.


## Overcoming the drawbacks of the output isolator pattern


The


[output isolator](https://www.elastic.co/guide/en/logstash/current/pipeline-to-pipeline.html#output-isolator-pattern) pattern is a common approach that leverages the


[persistent queue](https://www.elastic.co/guide/en/logstash/8.15/persistent-queues.html) feature to solve the aforementioned problem. The persistent queue allows Logstash to buffer data until the issue is resolved, ensuring that data is not lost.


However, the persistent queue can introduce operational risks (e.g., disk getting full) and


[performance overhead](https://www.elastic.co/blog/using-parallel-logstash-pipelines-to-improve-persistent-queue-performance) . For scenarios where only a subset of data needs to be sent to a pre-production environment, this overhead and risk may not be justified.


The solution presented in this article offers a simpler approach: routing a random subset of production events to a pre-production cluster by leveraging UDP for internal pipeline-to-pipeline communications within Logstash. Because UDP operates on a fire-and-forget basis and doesn't require acknowledgement from the receiver, even if the pre-production cluster encounters issues, the production data pipeline remains unaffected.


***Acknowledgment:***


*Thank you to*


**[Honza Král](https://www.linkedin.com/in/honzakral/) ** *for sharing this UDP solution!*


## Caveats: When to use the pipeline-to-pipeline with UDP solution


While this pipeline-to-pipeline with UDP solution is a practical workaround for constrained environments, it’s important to note that production and pre-production environments should be fully separated if possible. Separation would ensure cleaner boundaries between environments, mitigating potential risks. Such a setup would obviate the need for the solution presented in this article.


However, sometimes resources are limited, or separating environments isn’t feasible. In this case, the approach described in this article will allow your organization to populate your pre-production environment with a random subset of live production data, without introducing additional risk to the production data flow.


If using the solution presented in this article, it is also important to keep in mind that UDP does not guarantee delivery. If we are sampling a random subset of production data and sending it into the pre-production environment, we probably don’t care if a few events are lost. However, if you set the sampling rate to 100%, there is still no guarantee that UDP might drop some events — and therefore this solution should always be thought of as sampling rather than duplicating the production data.


## Pipeline-to-pipeline with UDP overview


In the pipeline-to-pipeline with UDP approach, you send a random subset of events from your production Logstash pipeline to the pre-production pipeline using UDP. UDP’s fire-and-forget nature ensures that even if the pre-production pipeline is blocked (e.g., if the pre-production cluster becomes unavailable), it won’t block data flow to the production cluster.


Unlike the output-isolator pattern, this approach does not require the use of a persistent queue, thereby reducing both risks and performance overhead. This solution is lightweight and effective, particularly for scenarios where only a subset of data needs to be routed to pre-production for testing purposes.


## Sampling the production data with the Ruby filter


The method presented in this section allows us to control which events are sent to the pre-production pipeline before they leave the production pipeline, which ensures that only the selected random subset of events is forwarded via UDP.


The code that is demonstrated below makes use of a


[generator input](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-generator.html) and


[stdout output](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-stdout.html) with the


[rubydebug codec](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-rubydebug.html) . This makes it simple to demonstrate the UDP functionality without relying on external data sources or external destinations. In a real-world pipeline, the generator would be replaced by your actual data inputs (such as Kafka or Beats), and the stdout/rubydebug would be replaced by an


[Elasticsearch output](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-elasticsearch.html) or another destination of your choice. Furthermore, this pipeline uses


[metadata](https://www.elastic.co/guide/en/logstash/current/event-dependent-configuration.html) to temporarily store values that will not appear in the output events.


### Pipeline configuration


The following configuration can be stored in your


[pipelines file](https://www.elastic.co/guide/en/logstash/current/multiple-pipelines.html) located in


config/pipelines.yml


:


```text
- pipeline.id: common
path.config: ./config/common.conf
- pipeline.id: pre-production
path.config: ./config/pre-production.conf
```


And store the following code in


config/common.conf


:


```text
input {
generator {
lines => [
'{"message": "message number 1", "@timestamp": "2020-08-18T19:42:42.000Z"}',
'{"message": "message number 2", "@timestamp": "2020-08-18T19:43:43.000Z"}',
'{"message": "message number 3", "@timestamp": "2020-08-18T20:44:40.000Z"}'
]
count => 1
codec => "json"
}
}


filter {
ruby {
code => "
sampling_rate = 0.3 # 30% sampling rate
if rand() <= sampling_rate
event.set('[@metadata][include_this_doc]', true)
else
event.set('[@metadata][include_this_doc]', false)
end
"
}
}


output {
if [@metadata][include_this_doc] {
udp {
id => "my_udp_output"
host => "localhost"
port => 9999
}
}
stdout { codec => "rubydebug" } # Replace with production Elasticsearch output
}
```


And the following code in


config/pre-production.conf


:


```text
input {
udp {
port => 9999
codec => json
}
}


filter {
mutate {
add_field => {"dest" => "PRE-PRODUCTION CLUSTER"} # add a field so we can see which events were hypothetically sent to pre-production - this can be removed without any consequence
}
}


output {
stdout { codec => "rubydebug" } # Replace with pre-production Elasticsearch cluster
}
```


### Running the code


If you are running a Logstash locally, you can run the above example by executing


./bin/logstash


, which will read


pipelines.yml


by default.


## Example output


Below are some examples of what the output might look like for the above configuration:


### Common pipeline output (simulating production data flow)


The output will look as follows, which simulates the documents that would be sent to the production cluster.


```text
{
"@version" => "1",
"event" => {
"original" => "{\"message\": \"message number 2\", \"@timestamp\": \"2020-08-18T19:43:43.000Z\"}",
"sequence" => 0
},
"host" => {
"name" => "Alexs-MBP-2.lan"
},
"message" => "message number 2",
"@timestamp" => 2020-08-18T19:43:43.000Z
}
```


### Pre-production pipeline output (simulating pre-production data flow)


If the document is selected for inclusion based on the sampling rate, it will be sent to the pre-production pipeline and will (also) use the stdout/rubydebug output to simulate the pre-production cluster destination. Keep in mind that due to the random nature of selection of documents to send to the pre-production cluster and the small number of sample documents, you may need to execute more than once to see a document from this pipeline.


These documents include a “dest” field that was added by the pre-production pipeline to indicate that the event is being routed to the pre-production cluster. In a real scenario, this output would be sent to the pre-production Elasticsearch cluster rather than printed to the console, and therefore this field could be removed.


Events that are sent to the stdout/rubydebug of the pre-production pipeline would appear as follows.


```text
{
"@version" => "1",
"event" => {
"original" => "{\"message\": \"message number 1\", \"@timestamp\": \"2020-08-18T19:42:42.000Z\"}",
"sequence" => 0
},
"message" => "message number 1",
"host" => {
"name" => "Alexs-MBP-2.lan",
"ip" => "127.0.0.1"
},
"dest" => "PRE-PRODUCTION CLUSTER",
"@timestamp" => 2020-08-18T19:42:42.000Z
}
```


## Benefits of the pipeline-to-pipeline with UDP solution


1.


**Non-blocking production flow:**


The use of UDP ensures that production data flow is not blocked by issues in the pre-production cluster.


2.


**Efficiency:**


The Production-side Sampling with Ruby filter ensures that only the sampled events are sent to the pre-production pipeline, reducing pipeline-to-pipeline traffic and processing overhead.


3. **Simplified architecture:**


Without the need for a persistent queue, this approach reduces complexity, operational risks, and the performance overhead associated with managing queues.


## Easy debugging


The use of generator input and rubydebug output allows for testing and debugging of Logstash pipelines, without relying on external data sources or external destinations. This makes it easy to demonstrate and validate this solution or any other solution that you may wish to test.


## Alternative approach: Sampling with the Drop filter


An alternative approach is to do the Sampling with


[Drop filter](https://www.elastic.co/guide/en/logstash/current/plugins-filters-drop.html) , where all events are sent to the pre-production pipeline, and a


[percentage](https://www.elastic.co/guide/en/logstash/current/plugins-filters-drop.html#plugins-filters-drop-percentage) of these events are then dropped. This method may be more intuitive for some, but it is less efficient because all events are sent to the second pipeline, increasing inter-pipeline traffic and processing requirements.


## Discover efficient sampling and fire-and-forget pipelines


While it is generally a best practice to separate production and pre-production environments, the solution presented in this article provides a valuable workaround for scenarios where resources are limited or separation is impractical. By leveraging the fire-and-forget nature of UDP and applying efficient sampling using the Ruby filter, this method allows for seamless testing in pre-production with real production data, while minimizing the risk of impacting your production data flow.


For more insights on managing Elasticsearch, Logstash, and other Elastic Stack components, be sure to explore


[Elastic’s official documentation](https://www.elastic.co/guide/index.html) .


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
