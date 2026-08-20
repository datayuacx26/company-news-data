---
schema_version: "1.0.0"
document_id: "ee93ff759adb4451567a6378c402793c4493a2a97f8864bd733ff16ba478c7e4"
company_key: "yc-glasskube"
company: "Glasskube"
source_id: "yc-glasskube-news-import-f40b83a58804"
canonical_url: "https://distr.sh/blog/distr-docker-swarm/"
published_at: "2025-05-20T00:00:00+00:00"
first_seen_at: "2026-07-21T21:44:16.826534+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:f3354be1669b18aa94ca29d58eea04123c02c23be8aca41e5a5ee833df46053e"
---

# Distr Launch Week: Distributing Applications to Docker Swarm

I am Jakob, an engineer working at Glasskube, which helps[software and AI companies](https://distr.sh/glossary/isv-meaning/) distribute their applications to[self-hosted environments](https://distr.sh/glossary/self-hosted-software/) . We build an Open Source Software Distribution platform called Distr ([github.com/distr-sh/distr](https://github.com/distr-sh/distr) ).


If you’ve ever thought that **Docker Compose** is too basic for your deployment needs, but a full **[Kubernetes](https://distr.sh/glossary/kubernetes/)** setup feels like overkill, you’re definitely not alone.


---


Luckily, Docker has come up with a solution just for this: Docker Swarm. This tool lets you connect multiple Docker daemons to create a Swarm cluster, giving you access to distributed computing features. With Docker Swarm, you get benefits like scheduling, better availability, and many of the advantages that come with cloud-native technologies like[Kubernetes](https://distr.sh/glossary/kubernetes/) .


On top of that, Docker Stacks make it easy to deploy almost any Compose file to your Swarm cluster. This means you can take advantage of a distributed deployment infrastructure while keeping your application manifest straightforward and easy to manage.


## About Docker Swarm


In a nutshell, a Docker Swarm cluster is a decentralized system of multiple docker daemons. These talk to each other to orchestrate the deployment of containers, while also taking care all the low-level details of such a system. To enable Swarm mode for your local docker installation, simply run` docker swarm init --advertise-addr ...` with your computer’s IP address (or 127.0.0.1 if just testing).


With this you already have a working Swarm cluster, but it only has a single node in it, so you don’t really get to enjoy **all** of the benefits of Docker Swarm. If you want another computer to join your cluster, run` docker swarm join --token ...` with the token shown by the` init` command from above.


Now, when it comes to deploying an application to a Docker Swarm cluster, you just run` docker stack deploy -c docker-compose.yaml my-app` , which usually just works with existing Compose files. If, however, you want more control over how your application is deployed in a Swarm cluster, you can add a` deploy` section to your compose service in order to specify replication mode, instances, resource requirements and more. This is an optional part of the Compose specification and it’s not available in the reference Docker Compose implementation.


Here’s an example of a Compose service featuring some of the supported` deploy` properties:


```text
services  :        backend  :          image  :   ghcr.io/distr-sh/hello-distr/backend:0.2.1          environment  :            DB_URL  :   postgresql://${DB_USER}:${DB_PASSWORD}@postgres/hello?sslmode=disable          depends_on  :            postgres  :              condition  :   service_healthy          restart  :   unless-stopped          deploy  :            mode  :   replicated            replicas  :   5            endpoint_mode  :   vip
```


## Introducing Docker Swarm Support


Given all of the benefits of Docker Swarm, it’s no surprise that support for it was one of our most requested features. Today, we’re excited to announce that **Docker Swarm support has officially landed in Distr!**


To enable Docker Swarm support, the steps are simple. First, you need to be signed in to Distr. If you don’t have an account yet, you can[sign up for free](https://distr.sh/get-started/) . Afterenabling Swarm mode , all you need to do is to check the box labeled “Swarm mode” when adding a deployment, and you’re good to go! Keep in mind that you currently can’t enable or disable Swarm mode for existing deployments due to some limitations in the underlying Docker system.


Once Swarm mode is turned on, the Distr Docker Agent will use the Docker Stack API to deploy your application’s Compose file. It will also use the Docker Service API to monitor your application’s health metrics and log streams (coming soon). If you want to learn more about Docker Swarm mode in Distr, definitely check out our[documentation](https://distr.sh/docs/agents/docker-agent/#docker-swarm) !


Have you tried out the new Docker Swarm mode in Distr yet? We’d love to hear your thoughts, as we’re on a mission to create the best application distribution solution possible!


This enhancement was made possible thanks to collaboration with GitHub user[Asher Abecasiss](https://github.com/asherAbecasiss) , who led this initiative and provided our very first community contribution.


We appreciate the support and feedback from our community, which helps us keep improving!
