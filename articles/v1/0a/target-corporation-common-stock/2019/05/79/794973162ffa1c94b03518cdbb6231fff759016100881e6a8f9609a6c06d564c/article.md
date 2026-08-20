---
schema_version: "1.0.0"
document_id: "794973162ffa1c94b03518cdbb6231fff759016100881e6a8f9609a6c06d564c"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/using-drone-for-automated-integration-testing"
published_at: "2019-05-20T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:9cd16de1d35807f44c686a7b3a6d46a981b1a9912fcfd1883836054b40968519"
---

# Using Drone for Automated Integration Testing

Our team developed a service that reads from a Kafka topic, interacts with a Postgres database using basic CRUD operations, and calls APIs on an external service. As part of our increased focus on automated testing, getting to a higher level of testing code coverage required us to tie in the external components of our system architecture. Since we don’t control these components in production, we included these components in our CI/CD pipeline to give us a higher level of confidence that our integrated code is working as expected. We also wanted to run automated functional tests against our service, and that too required running these services in the pipeline. We use Drone as our CI/CD tooling and decided to learn how to use services in Drone to solve this problem. This short blog post runs through an example of how we started up a database service and a Kafka service in Drone and ran a test against these service, that might help others in a similar situation.


Database Service


In order to fire up a Postgres database service in Drone, we added this to our


.drone.yml


specification:


```text
<span class="hljs-attribute">services</span>:           <span class="hljs-attribute">database</span>:             <span class="hljs-attribute">image</span>: <span class="hljs-attribute">postgres</span>:<span class="hljs-number">10.6</span>             <span class="hljs-attribute">environment</span>:               <span class="hljs-attribute">POSTGRES_USER</span>: drone_db_user               <span class="hljs-attribute">POSTGRES_PASSWORD</span>: drone_db_pass
```


The environment variables for the container establish an initial user and password when the service starts.


Our service is written in


golang


and uses a package called


pop


for database interactions, which has a command line tool


soda


that creates the database and instantiates the tables in the database. We therefore needed to install these packages on our Drone system and invoke the


soda


commands in our pipeline once the database service was up and running. Our first pass at a pipeline in the


.drone.yml


file put these pieces together and executed a simple test to write to and read from the database:


```text
pipeline:           build-binary-linux:             image: ${CUSTOM_REGISTRY}/${ORG}/alpine-go111-build:latest             environment:               -<span class="ruby"> GO111MODULE=on         </span>      -<span class="ruby"> CGO_ENABLED=<span class="hljs-number">0</span>         </span>      -<span class="ruby"> GOOS=linux         </span>      -<span class="ruby"> GOARCH=amd64         </span>      -<span class="ruby"> DB_ENVIRONMENT=localdocker         </span>      -<span class="ruby"> DB_USER=drone_db_user         </span>      -<span class="ruby"> DB_PASS=drone_db_pass         </span>      -<span class="ruby"> DB_NAME=dronetesting_db         </span>      -<span class="ruby"> DB_HOST=database         </span>    commands:               -<span class="ruby"> go get -v  github.com/gobuffalo/pop         </span>      -<span class="ruby"> go install github.com/gobuffalo/pop/soda         </span>      -<span class="ruby"> cd ./db         </span>      -<span class="ruby"> soda drop -e dronetesting         </span>      -<span class="ruby"> soda create -e dronetesting         </span>      -<span class="ruby"> soda migrate -e dronetesting up         </span>      -<span class="ruby"> cd ..         </span>      -<span class="ruby"> go test -timeout <span class="hljs-number">240</span>s -p <span class="hljs-number">1</span> github.com/${ORG}/service/cmd -run Test_DBConnection -v</span>
```


There are several things to notice in this pipeline. First, the commands to install


pop


and


soda


in the Docker container that executes the test and to create the database and associated tables are exactly the same commands a developer runs on the command line on their system to set up a local database using these packages. Second, the environment variables


DB_*


are used in our program's database configuration to connect to the correct database service, which is why the user and password values match those created when the database service was started. In this example, we used plaintext for the username and password since our tests are not writing any sensitive data to the database and the database only exists for the duration of the execution of the pipeline, but for added security, these could be stored as secrets. Third, and critically,


DB_HOST


must be the same as the name given to the database service. Depending on the language and packages you are using, the details of how to create tables and populate them with data will differ, but we have employed a similar strategy for creating and connecting to a database for testing a Rust application so know this approach adapts to other environments.


Kafka Service


We wanted to test that our program could read an IP from a Kafka topic, do some processing on it, and write the IP to a table in the database. To be able to effectively utilize a Kafka endpoint, we needed to create and configure a small running instance of Kafka and its required software component ZooKeeper within our CI/CD build, as integration testing relies on true functional capabilities of each external software dependency.


[This article](http://cloudurable.com/blog/kafka-architecture/index.html) gives a brief description of the Kafka architecture and explains its components. After some trial-and-error, we found a successful strategy for bringing up the Kafka service, connecting it to our pipeline, and running a test that executes our test scenario.


In the services section of the


.drone.yml


file, we added a Kafka service:


```text
kafka:             image: spotify/kafka:latest             ports:               -<span class="ruby"> <span class="hljs-string">"9092:9092"</span>         </span>      -<span class="ruby"> <span class="hljs-string">"2181:2181"</span>         </span>    environment:               -<span class="ruby"> TOPICS=test</span>         </code></pre>         <p>This container runs an instance of ZooKeeper on port 2181 and Kafka on port 9092.  Writing to and reading from a Kafka service requires a broker -- the connection to the service itself -- and a topic -- the queue the messages are written to and read from.  Adding the environment variable <code>TOPICS</code> causes the ZooKeeper instance running in this service to create a Kafka topic named <strong>test</strong> when the service starts up. This is the topic we will write to and read from. Our application uses the environment variables <code>KAFKA_BROKER</code>, <code>KAFKA_TOPIC</code>, and <code>KAFKA_GROUP</code> to set up the reader, so our pipeline needed to have those variables set up to connect to the service. As with the database Drone setup, the <code>KAFKA_BROKER</code> variable uses the Drone Kafka service name (kafka) to establish the connection with that service. Below is our final pipeline to run the test:</p>         <pre><code class="lang-yaml">pipeline:           build-binary-linux:             image: $${CUSTOM_REGISTRY}/${ORG}/alpine-go111-build:latest             environment:               -<span class="ruby"> GO111MODULE=on         </span>      -<span class="ruby"> CGO_ENABLED=<span class="hljs-number">0</span>         </span>      -<span class="ruby"> GOOS=linux         </span>      -<span class="ruby"> GOARCH=amd64         </span>      -<span class="ruby"> DB_ENVIRONMENT=localdocker         </span>      -<span class="ruby"> DB_USER=drone_db_user         </span>      -<span class="ruby"> DB_PASS=drone_db_pass         </span>      -<span class="ruby"> DB_NAME=dronetesting_db         </span>      -<span class="ruby"> DB_HOST=database         </span>      -<span class="ruby"> KAFKA_BROKER=<span class="hljs-symbol">kafka:</span><span class="hljs-number">9092</span>         </span>      -<span class="ruby"> KAFKA_TOPIC=test         </span>      -<span class="ruby"> KAFKA_GROUP_ID=<span class="hljs-number">1</span>         </span>    commands:               -<span class="ruby"> go get -v github.com/gobuffalo/pop         </span>      -<span class="ruby"> go install github.com/gobuffalo/pop/soda         </span>      -<span class="ruby"> cd ./db         </span>      -<span class="ruby"> soda create -e dronetesting         </span>      -<span class="ruby"> soda migrate -e dronetesting up         </span>      -<span class="ruby"> cd ..         </span>      -<span class="ruby"> go test -timeout <span class="hljs-number">240</span>s -p <span class="hljs-number">1</span> github.com/${ORG}/service/cmd -run Test_KafkaConnection -v</span>         </code></pre>         <h2 id="automated-test">Automated Test</h2>         <p>There were two additional things we learned related to the test itself that were critical to getting the entire automated test working.  First, the code we wanted to test that created the Kafka reader needed to be started in a separate process.  This ensured the reader was not blocking the execution of the test itself.  Second, writing test data to the topic needed to occur after the process running the reader was completely up.  This ensured that the service was up, topic created, and the connection available.</p>         <pre><code class="lang-go"><span class="hljs-function"><span class="hljs-keyword">func</span> <span class="hljs-title">Test_KafkaConnection</span><span class="hljs-params">(t *testing.T)</span></span> {             <span class="hljs-keyword">var</span> brokerList []<span class="hljs-keyword">string</span>                 broker := utils.Config.WHConfig.KafkaBroker             topic := utils.Config.WHConfig.KafkaTopic                 <span class="hljs-comment">// populate the string slice for the broker list</span>             <span class="hljs-keyword">if</span> strings.Contains(broker, <span class="hljs-string">","</span>) {                 brokerList = strings.Split(broker, <span class="hljs-string">","</span>)             } <span class="hljs-keyword">else</span> {                 brokerList = []<span class="hljs-keyword">string</span>{broker}             }                 writer := kafka.NewWriter(kafka.WriterConfig{                 Brokers:  brokerList,                 Topic:    topic,                 Balancer: &amp;kafka.LeastBytes{},             })                 <span class="hljs-keyword">defer</span> writer.Close()                 <span class="hljs-comment">// Start the service in non-blocking fashion, and give it time to come up</span>             <span class="hljs-keyword">go</span> <span class="hljs-function"><span class="hljs-keyword">func</span><span class="hljs-params">()</span></span> {                 startService()             }()             time.Sleep(<span class="hljs-number">30</span> * time.Second)                 <span class="hljs-comment">// Write the message to the topic</span>             ip := <span class="hljs-string">"some IP provided"</span>             writer.WriteMessages(context.Background(),                 kafka.Message{                     Key:   []<span class="hljs-keyword">byte</span>(<span class="hljs-string">"Event"</span>),                     Value: generateMessage(ip),                 })                 <span class="hljs-comment">// Pause to allow time for service to read from topic, process,</span>             <span class="hljs-comment">// and write to database</span>             time.Sleep(<span class="hljs-number">10</span> * time.Second)                 <span class="hljs-comment">// Verify IP ended up in database</span>             dbConn, err := utils.NewDBConnection(<span class="hljs-string">"localdocker"</span>)             <span class="hljs-keyword">if</span> err != <span class="hljs-literal">nil</span> {                 fmt.Println(err.Error())                 os.Exit(<span class="hljs-number">1</span>)             }                 lookupIP, err := utils.LookupIP(dbConn, ip)             assert.Nil(t, err, <span class="hljs-string">"LookupIP error"</span>)             assert.NotNil(t, lookupIP)         }
```


Here is the output of the services when this pipeline is run in drone:


References


- [Drone services documentation: Drone 0.8.0: Example using Postgres](https://0-8-0.docs.drone.io/postgres-example/)
- [Official Postgres Docker container details on Docker Hub](https://hub.docker.com/_/postgres)
- [Gobuffalo-pop](https://github.com/gobuffalo/pop)
- [Kafka docker container repository](https://github.com/spotify/docker-kafka)
