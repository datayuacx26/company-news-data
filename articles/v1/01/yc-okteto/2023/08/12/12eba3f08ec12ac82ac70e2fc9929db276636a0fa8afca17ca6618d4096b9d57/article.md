---
schema_version: "1.0.0"
document_id: "12eba3f08ec12ac82ac70e2fc9929db276636a0fa8afca17ca6618d4096b9d57"
company_key: "yc-okteto"
company: "Okteto"
source_id: "yc-okteto-rss-a64bce3f80ea"
canonical_url: "https://www.okteto.com/blog/five-challenges-with-developing-locally-using-docker-compose/"
published_at: "2023-08-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.770894+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:dae840c397cfc8db545fe50e3fd545d4d94e94827bbcee72245c32a6910b889a"
---

# Five Challenges with Developing Locally Using Docker Compose

# Five Challenges with Developing Locally Using Docker Compose


After the popularization of containers, a lot of the development workflow started leaning on Docker Compose. Developers would have a Docker Compose file which defined how to build the container images for all their services, what ports to expose, and have volumes attached to their containers. They would then run the containers locally and leverage the attached volumes to develop their application. However, there are a lot of limitations in this flow which not only hamper developer productivity but can also cause security issues and bugs. Let's discuss some of these and see how using Okteto you can fix these problems.


## Running Things Locally Isn't Practical for Modern Development


When dealing with just a couple of containers running simple microservices, developers might not encounter any issues. However, as they scale up the number of microservices or run more demanding workloads, even the most powerful development machine will start experiencing bottlenecks. This is especially true for modern cloud-native applications we develop today. They typically consist of 10 to 50 different microservices, which is impractical to run locally. Not only does it slow down development, but restarting or spinning up everything again severely hampers productivity.


## This is Not How You Run Containers in Production


One significant issue is that running containers with Docker Compose on local machines differs from how it's done in production. Instead of directly running containers, production environments leverage Kubernetes resources like Deployments or StatefulSets. Consequently, developing locally with Docker Compose means working in an environment that deviates from production. This discrepancy increases the likelihood of bugs that are difficult to identify, such as misspelt environment variables or other similar issues. To ensure stability and avoid unexpected surprises upon deployment, it is crucial to develop in a realistic environment.


## Environments Aren't Reproducible


When developers launch their application using \`docker-compose up\`, they cannot launch another instance of the application to experiment or try out a different version or commit. Technically, they could rename things and then run them again, but not only is that inconvenient, it also significantly impacts the performance of the local machine. Consequently, the developer's creativity and freedom become restricted.


## Sharing Work is Tough


After implementing a feature or bug fix for the application, sharing it with someone on the team instantly and effortlessly becomes a challenge. Engineers need to commit and push their changes, and then someone else on the team has to pull them and run \`docker compose up\` to see the updates. If any further changes are requested, they have to repeat the entire process. This inefficient iteration process significantly slows down the velocity at which developers can deliver code. Additionally, this approach excludes team members from other departments such as product, sales, and marketing, who may not have the expertise to run Docker on their local machines. As a result, feedback from these teams is delayed in the software delivery cycle. However, with Okteto (as we'll soon explore), this feedback can be provided much sooner, streamlining the entire development process.


## End to End Tests


While Docker Compose is useful for bringing up all the services of your application locally, it doesn't replicate the realistic environment of production. As a result, running end-to-end tests on your application using Docker Compose may not yield the expected results. Furthermore, only the developer running the application locally can test it, making it difficult to share endpoints for testing with other teams.


## How Okteto Fixes These Problems and Improves the Docker Compose Workflow


Okteto takes your Docker Compose application and translates your application services into Kubernetes resources, deploying them on a cluster. This ensures that developers can begin development in an environment that closely resembles production. Additionally, by not running any containers locally, the local machine's resources are not constrained, regardless of the demands of the application. What's even better is that developers can deploy their application, or different versions of it, to different Okteto Namespaces, allowing for multiple copies. This approach treats each environment as ephemeral, enabling them to quickly spin up new instances as needed.


Okteto solves another pain point by creating endpoints instead of providing access to your application on localhost. These endpoints reflect live changes you make to your code, enabling you to easily share your work and receive feedback from your team. No need for developers to commit their changes or for other team members to run Docker on their machines. Moreover, these endpoints allow you to run realistic E2E tests on your application. If you'd like to learn more, check out[this](https://www.okteto.com/blog/the-modern-way-of-sharing-dev-work-with-your-team/) article.


## Trying Out Okteto!


Running your Docker Compose applications on the cloud with Okteto is a breeze. Sign up for our self-hosted free trial[here](https://www.okteto.com/signup/) . Once you have Okteto installed on your cluster, check out this video to learn how to get your Docker Compose applications up and running with Okteto!


Arsh Sharma


Developer Experience Engineer / Emojiologist 😜


[View all posts](https://www.okteto.com/blog/authors/arsh-sharma/)


[docker-compose](https://www.okteto.com/blog/tags/docker-compose/)


[docker](https://www.okteto.com/blog/tags/docker/)


#### Share this:
