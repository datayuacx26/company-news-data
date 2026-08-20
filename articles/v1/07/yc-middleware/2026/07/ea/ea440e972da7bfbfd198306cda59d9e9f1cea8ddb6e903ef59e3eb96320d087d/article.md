---
schema_version: "1.0.0"
document_id: "ea440e972da7bfbfd198306cda59d9e9f1cea8ddb6e903ef59e3eb96320d087d"
company_key: "yc-middleware"
company: "Middleware"
source_id: "yc-middleware-news-import-fe54c9028845"
canonical_url: "https://middleware.io/blog/docker-health-checks/"
published_at: null
first_seen_at: "2026-07-22T04:20:55.832849+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:277bebe2f52050e8f6bc1185d039cf03715bd3f777110ea325e5a6d9d791be35"
---

# Docker Health Checks: How to Monitor, Configure, and Troubleshoot Container Health

[Docker](https://middleware.io/blog/understanding-the-docker-ecosystem/) health checks are used to determine if an application running inside a container is actually functioning correctly and not just if the container process is running. A container can appear “up” while its application is unresponsive, misconfigured, or unable to reach required dependencies. Health checks solve this gap by actively validating real application behavior.


By setting up health checks in Dockerfiles and Docker Compose, teams can catch issues earlier, debug unhealthy containers more easily, and improve the overall reliability of containerized systems.


In this article, you’ll learn how Docker health checks work, how to configure them using the` HEALTHCHECK` instruction and Docker Compose, how to troubleshoot unhealthy containers, and the best practices for[monitoring container](https://middleware.io/blog/container-monitoring-tools/) health in production environments.


#### TL;DR


- Docker health checks monitor the health of an application running inside a container, not just the container itself.
- Every container sits in one of three states at any given time: starting, healthy, or unhealthy.
- The HEALTHCHECK instruction gives Docker a command to run on a schedule to determine which state applies.
- docker ps and docker inspect are the main tools for checking on health status from the outside.
- In Compose, Healthcheck and depends_on work together to make sure services come up in a safe order.
- Most unhealthy containers trace back to the same handful of issues: slow startup, wrong endpoint, missing dependency, network trouble, or a timeout that’s set too tight.
- When debugging, the usual path is: check the health logs, run the check command manually, then look at the app logs.
- In production, keep checks lightweight, set timeouts realistically, and monitor continuously.


## What is a Docker health check?


A Docker health check is a diagnostic command that monitors the health of an application running inside a container. Docker periodically executes a predefined command and uses the result to determine the container’s health status.


By default, Docker only tracks whether a container’s main process is running. A health check provides additional visibility into the application’s actual state by verifying that it can respond to requests, connect to dependencies, or perform other critical functions.


Health checks can be configured using the` HEALTHCHECK` instruction in a Dockerfile or the healthcheck configuration in Docker Compose.


## Docker health status states


Docker assigns one of three health states to containers with a configured health check:


Status Meaning


Starting Docker is still evaluating the container’s health during startup


Healthy The health check is passing


Unhealthy The health check has failed repeatedly


These health states help teams quickly identify application issues, troubleshoot unhealthy containers, and monitor container health across environments.


## How Docker health checks work


Docker health checks work by executing a command at regular intervals inside a container. Docker evaluates the command’s exit status and uses the result to determine the container’s health status.


When a container starts, Docker begins running the configured health check based on the specified timing settings. Successful checks keep the container in a healthy state, while repeated failures can cause it to be marked as unhealthy.


Docker determines the result of a health check based on the command’s exit code returned by the health check command:


- 0: Health check passed
- 1: Health check failed
- 2: Reserved by Docker (do not use)


When a health check failure triggers a container kill, Kubernetes records exit code 137 (SIGKILL). Understanding exit codes helps you diagnose whether a container was killed by the[OOM killer](https://middleware.io/blog/kubernetes-oomkilled-error/) , a failed health check, or a graceful shutdown. See Middleware’s guides on[exit code 137](https://middleware.io/blog/exit-code-137-in-kubernetes-causes-diagnosis-fixes/) and[exit code 143](https://middleware.io/blog/exit-code-143/) for a full breakdown.


### The HEALTHCHECK instruction


Docker health checks are configured using the HEALTHCHECK instruction in a Dockerfile.


```text
HEALTHCHECK CMD curl -f http://localhost:8080/health || exit 1
```


In this example, Docker periodically sends a request to the application’s health endpoint. If the endpoint returns a successful response, the health check passes. If the request fails, Docker records the failure and updates the container’s health status accordingly.


Docker uses these exit codes to determine whether a container should remain healthy or be marked as unhealthy after repeated failures.


#### HEALTHCHECK command syntax


The basic syntax for a Docker health check is:


```text
HEALTHCHECK [OPTIONS] CMD command
```


Docker provides several options for controlling health check behavior:


Option Description Default


–interval Time between health checks 30s


–timeout Max time allowed per check 30s


–start-period Grace period before failures count 0s


–retries Consecutive failures before unhealthy 3


Example:


```text
HEALTHCHECK --interval=15s --timeout=3s --start-period=20s --retries=3 \ CMD curl -f http://localhost:3000/health || exit 1
```


This configuration checks the /health endpoint every 15 seconds, allows up to 3 seconds for each check, provides a 20-second startup grace period before failures are counted, and marks the container as unhealthy after 3 consecutive failures.


### Overriding base image health checks


Some base images define a default HEALTHCHECK. When building on top of these images, you may want to remove or replace that behavior.


To disable an inherited health check:


```text
HEALTHCHECK NONE
```


This disables any health check inherited from the base image and allows you to define your own health check later in the Dockerfile if needed.


## CMD vs CMD-SHELL: choosing the right test format


Health check test commands can be written in two formats: CMD and CMD-SHELL. The difference affects how the command is executed inside the container.


### CMD format


With CMD, the command is split into separate elements. The first element is the executable; subsequent elements are its arguments. The command runs directly without a shell.


```text
HEALTHCHECK CMD ["curl", "-f", "http://localhost:8080/health"]
```


Use CMD when you want predictable, shell-independent execution. It avoids shell-specific behavior and is recommended for simple commands.


### CMD-SHELL format


With CMD-SHELL, the command runs inside a shell (` /bin/sh -c on Linux` ). This allows shell features like pipes, conditionals, and variable substitution.


```text
HEALTHCHECK CMD ["CMD-SHELL", "curl -f http://localhost:8080/health || exit 1"]
```


Use` CMD-SHELL` when your check requires shell logic, for example chaining commands with || to control exit codes explicitly.


## Health check commands for common services


### Web Servers (Nginx, Apache)


Web servers typically expose an HTTP endpoint that can be used to verify the service is running and responding to requests.


```text
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
CMD curl -f http://localhost:80/ || exit 1
```


If curl is not available in the image, you can use wget instead:


```text
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \ CMD wget --no-verbose --tries=1 --spider http://localhost:80/ || exit 1
```


### TCP port check (no HTTP endpoint)


For services that don’t expose an HTTP endpoint, a TCP connectivity check confirms that the service is accepting connections on the expected port. This is common for message brokers and custom TCP servers.


```text
HEALTHCHECK --interval=15s --timeout=5s --retries=3 \  CMD nc -z localhost 5672 || exit 1
```


The` nc -z command` attempts to open a TCP connection without sending data. An exit code of 0 confirms the port is open; any other exit code marks the check as failed.


### PostgreSQL


PostgreSQL provides the pg_isready utility, which checks whether the database is running and accepting connections without executing a query.


```text
HEALTHCHECK --interval=10s --timeout=5s --retries=5 \
CMD pg_isready -U postgres || exit 1
```


### MySQL


MySQL includes the mysqladmin utility, which can be used to verify that the database server is running and responding to requests.


```text
HEALTHCHECK --interval=10s --timeout=5s --retries=5 \
CMD mysqladmin ping -h localhost || exit 1
```


### Redis


Redis provides the redis-cli command-line tool, which can be used to verify that the server is running and accepting commands. The redis-cli ping command sends a lightweight request and expects a PONG response from the Redis server.


```text
HEALTHCHECK --interval=10s --timeout=3s --retries=3 \ CMD redis-cli ping || exit 1
```


## Docker Compose health checks


Docker Compose supports health checks directly in the compose.yaml file using a healthcheck block. This is the recommended approach for multi-service setups, as it allows you to define health checks alongside service configuration without modifying Dockerfiles.


### Basic health check in Compose


```text
services:  web:    image: nginx    healthcheck:      test: ["CMD", "curl", "-f", "http://localhost"]      interval: 30s      timeout: 10s      retries: 3      start_period: 40s
```


Docker Compose supports all the same timing options as the Dockerfile HEALTHCHECK instruction, plus two additional parameters introduced in Docker 25:


**Option** **Purpose** **Default**


test Command to execute –


interval Time between checks 30s


timeout Max time per check 30s


retries Failures before unhealthy 3


start_period Grace period before failures count 0s


start_interval Check frequency during grace period (Docker 25+) 5s


` start_interval` controls how frequently Docker polls the container during the` start_period` grace window. This is useful for services with variable startup times you can poll more frequently during startup without changing the steady-state interval.


### Managing service dependencies with depends_on


One of the most valuable uses of Docker Compose health checks is enforcing service startup order. The` depends_on` field with condition:` service_healthy` delays a service from starting until its dependency reports a healthy status.


```text
services:  db:    image: postgres:15    environment:      POSTGRES_USER: app      POSTGRES_PASSWORD: secret    healthcheck:      test: ["CMD", "pg_isready", "-U", "app"]      interval: 10s      timeout: 5s      retries: 5      start_period: 20s  api:    image: my-api:latest    depends_on:      db:        condition: service_healthy
```


In this example, the api service will not start until the db service passes its health check. This prevents the common failure where an application tries to connect to a database that hasn’t finished initializing. For more on Kubernetes-level workload startup issues, see Middleware’s guide on[Kubernetes workload troubleshooting](https://middleware.io/blog/kubernetes-workload-troubleshooting/) .


Note that` depends_on` with` service_healthy` only controls startup order. It does not continuously monitor dependencies after startup or restart dependent services if a dependency later becomes unhealthy.


### Disabling a health check in Compose


To disable a health check defined in the base image:


```text
healthcheck:  disable: true
```


## How to Check Docker Container Health Status


Once a health check is configured, Docker exposes the resulting status through standard CLI commands. These are the tools to use when verifying the state of a container.


### Using docker ps


The quickest way to check container health is with docker ps, which displays the current health status directly in the STATUS column for any container with a configured health check.


```text
docker ps
```


Example output:


```text
CONTAINER ID   IMAGE        STATUS
a1b2c3d4e5f6   my-api:latest   Up 5 minutes (healthy)
f6e5d4c3b2a1   my-db:latest    Up 10 minutes (unhealthy)
```


This command provides a quick overview of container health without displaying detailed health check results.


### Using docker inspect


For a complete view of health check configuration, history, and results, docker inspect provides the full health object stored in the container’s metadata.


```text
docker inspect --format='{{json .State.Health}}' <container_name>
```


Example output:


```text
{
"Status": "unhealthy",
"FailingStreak": 3,
"Log": [
{
"Start": "2026-06-15T10:22:01.123456Z",
"End": "2026-06-15T10:22:01.456789Z",
"ExitCode": 1,
"Output": "curl: (7) Failed to connect to localhost port 8080: Connection refused"
}
]
}
```


This JSON text block shows you:


- Status: Confirms if the container is healthy or unhealthy.
- FailingStreak: How many times the check has failed in a row.
- Log: A list of the most recent health tests.


### Viewing Health Check Logs


To view logs generated by the health check command, use:


```text
docker inspect --format='{{range .State.Health.Log}}{{.Output}}{{end}}' <container_name_or_id>
```


Example Output:


```text
curl: (7) Failed to connect to localhost port 8080: Connection refused
```


In this example, curl could not connect to the application on port 8080. This may indicate that the application has not started successfully, is listening on a different port, or has stopped responding inside the container.


### Using docker compose ps


For multi-service setups, docker compose ps shows health status for all services in the stack at once:


docker compose ps


## Docker health checks vs container readiness checks


Docker health checks verify that a containerized application is functioning correctly, while readiness checks determine if an application is ready to receive traffic. A container can be healthy but still not ready to serve requests.


In Kubernetes, these two concepts map to separate probe types: liveness probes (analogous to Docker health checks) determine if a container should be restarted, while readiness probes control whether traffic is routed to the pod. If you’re moving containerized workloads from Docker Compose to Kubernetes, you’ll need to translate your health check logic into the appropriate probe configuration.


For teams running Kubernetes in production,[Middleware’s Kubernetes monitoring](https://middleware.io/solutions/kubernetes-monitoring/) provides real-time visibility into pod health, resource usage, and probe failures.


Here is how they compare directly:


Feature Docker Health Checks Container Readiness Checks


Purpose Verify that an application is functioning correctly Determine if an application is ready to receive traffic


Primary Focus Application health and availability Traffic routing and service readiness


When They Run Continuously throughout the container lifecycle Before a service begins receiving traffic


Failure Impact Container is marked unhealthy Traffic is not routed to the service


Common Usage Monitoring container health Managing traffic flow in orchestrated environments


Typical Platform Docker, Docker Compose, Kubernetes Kubernetes and other orchestration platforms


## Common reasons a Docker container becomes unhealthy


The[Docker container](https://middleware.io/blog/docker-container-logs/) will be considered unhealthy if the health check fails repeatedly as per its configuration. In many cases the application is still running but the health check fails to properly determine its condition. These are the most frequently occurring ones to be aware of before you begin debugging.


### Application startup takes too long


Some applications need time to fully initialize before they can respond to requests. If the health check fires before the application is ready, Docker will mark the container unhealthy even though the app hasn’t had a chance to start. Configure` start_period` to give the application a grace window before failures begin counting toward the retry threshold.


### Wrong health check endpoint


Health check will always return an error if the path being checked doesn’t exist, the port you use for health check is closed for the application, or the endpoint requires authorization.


It’s always better to check in advance whether your endpoint for the health check is reachable inside the container.


```text
docker exec {container_name} curl -f http://localhost:3000/health
```


### curl or wget not available in the image


Minimal base images (Alpine, distroless) often don’t include curl or wget. If the health check tool isn’t present in the image, the check will fail with a “ **command not found** ” error even if the application is running correctly. Either install the tool in your Dockerfile or switch to a tool that’s already available, such as nc for TCP checks or a lightweight binary bundled with the application.


### DNS and networking issues


Health check for a container may fail if there are some problems with DNS or the network. They usually occur when a hostname is used in requests to other services that aren’t available in container network for some reason or the container was started prematurely without all the network connections.


This type of problem usually shows up as “connection timeout” or “name not resolved” errors in the result of a health check. If this occurs, the first thing you should do is to check container’s network with docker network inspect command.


### Database dependencies not ready


A container can appear healthy while still failing requests if its health check only verifies that the application process is running. Include dependency checks in the` /health` endpoint so it only reports success when critical services are available. Pair this with` depends_on` and condition:` service_healthy` in Docker Compose to prevent startup race conditions. If this pattern escalates into a CrashLoopBackOff loop in Kubernetes, see Middleware’s guide on[Kubernetes common errors](https://middleware.io/blog/kubernetes-common-errors-fix/) for diagnosis steps.


### Timeout set too aggressively


If the configured timeout is shorter than the time it actually takes for the health check command to complete under normal load, Docker will record the check as failed even when the application is working correctly. This is especially common for database connectivity checks during high-traffic periods. Increase the timeout or adjust the interval to match realistic response times under production load.


## How to troubleshoot an unhealthy Docker container


When a container is marked as unhealthy, the health check is repeatedly failing. The goal of troubleshooting is to determine whether the issue is in the health check itself, the application, or a dependency.


### Step 1: Run the health check manually


Start by running the health check command directly inside the container.


```text
docker exec {container_name} curl -f http://localhost:3000/health
```


> **Note:** This example assumes curl is installed inside the container. If it is not available, use an equivalent tool such as wget or another command supported by the container image.


This bypasses Docker’s health check schedule and runs the check directly inside the container. If the command succeeds here but fails in the health check logs, the issue is often related to timing the application may not be fully initialized when the health check runs, or the configured timeout may be too short.


If the command also fails here, the problem is likely with the application itself, its dependencies, or the health check configuration.


### Step 2: Inspect application logs


```text
docker logs {container_name}
```


Look for startup errors, unhandled exceptions, failed database connections, or missing environment variables. These are the most common reasons an application fails to reach a healthy state. For teams managing log pipelines at scale,[Middleware’s log monitoring](https://middleware.io/product/log-monitoring/) centralizes container logs alongside health status and infrastructure metrics, making it faster to correlate failures.


### Step 3: Verify network connectivity


If the health check is failing with a connection error or DNS resolution failure, confirm the container can reach the services it depends on:


```text
docker exec {container_name} curl -f http://api:8080/health
```


Check which networks the container is attached to:


```text
docker inspect --format='{{json .NetworkSettings.Networks}}' {container_name}
```


You can also inspect the network itself to verify that all expected containers are connected:


```text
docker network inspect <network_name>
```


A container attached to the wrong network, or a service name that does not resolve correctly, can cause health checks to fail even when the application is functioning normally. These issues present as connection timeouts or DNS resolution errors.


### Step 4: Adjust health check parameters


If the application is running correctly but the container is still being marked unhealthy, the health check configuration itself may need tuning. Review the configured values for:


- interval
- timeout
- retries
- start_period


Applications with longer startup times may require a larger start_period or additional retries before Docker begins evaluating health check failures.


If repeated failures lead to pod restarts in Kubernetes, see Middleware’s guide on[kubectl restart pod](https://middleware.io/blog/kubectl-restart-pod/) and[OOMKilled errors](https://middleware.io/blog/kubernetes-oomkilled-error/) to understand what the exit codes mean.


## Docker health check best practices


### Keep health checks lightweight


A health check runs on a schedule inside the container alongside your application. Resource-intensive checks that perform heavy computations, open multiple connections, or run complex database queries can create unnecessary overhead and impact performance.


Instead, use lightweight checks that verify availability. For databases, a simple connectivity test is usually enough to confirm the service is reachable.


### Configure realistic timeouts


The default timeout value in Docker is 30 seconds. For many applications, this is longer than necessary and can delay the detection of genuine failures.


For most services, a timeout between 3 and 10 seconds is a reasonable starting point. Configure the timeout based on how quickly your application normally responds under production load, while allowing a small buffer for temporary spikes in latency.


### Validate critical dependencies only


A health check that only verifies the application process is running can produce false positives. If the application depends on a database, cache, or message broker to function correctly, consider including lightweight dependency checks in the /health endpoint.


At the same time, avoid checking every downstream service. Health checks should validate only the dependencies required for the application to operate. Checking unnecessary services can increase complexity and cause false failures.


### Use start_period for slow-starting services


Services that run database migrations, warm up caches, or load large models on startup can take significantly longer to become ready than a standard web server. Setting a start_period prevents false unhealthy marks during normal initialization without permanently relaxing your health check timing.


### Continuously monitor health check results


Health check status should not be something you only investigate after a container becomes unhealthy. Tracking health check results over time can reveal recurring issues, such as intermittent failures and slow application startups, before they escalate into incidents. A dedicated[Docker monitoring tool](https://middleware.io/blog/docker-monitoring-tools/) helps correlate health status with CPU, memory, and network metrics across your fleet.


## Monitor Docker health checks with Middleware


Docker health checks tell you when a container is unhealthy, but they don’t explain why. Middleware helps you investigate failures by correlating container health with logs, metrics, and traces in one place.


With Middleware, you can discover running containers, correlate health status with CPU, memory, and network metrics, receive alerts on anomalies and threshold breaches, and trace failures across services to identify root causes. For[APM](https://middleware.io/product/apm/) and[infrastructure monitoring](https://middleware.io/product/infrastructure-monitoring/) use cases, Middleware provides full-stack visibility from the container level up to the application layer.


For faster incident resolution,[OpsAI](https://middleware.io/product/ops-ai/) , Middleware’s AI SRE agent, automatically investigates incidents, correlates telemetry, and identifies likely root causes helping you troubleshoot container failures more efficiently.


OpsAI handles OOMKilled events, CrashLoopBackOff loops, and ImagePullBackOff errors autonomously. See how it compares in Middleware’s[Kubernetes pod crash auto-remediation guide](https://middleware.io/blog/kubernetes-self-healing-opsai-pod-crash-auto-remediation/) . Teams using OpsAI have seen on-call productivity improve by over 80%.


[Start monitoring your Docker containers](https://app.middleware.io/auth/register/) — 14-day free trial, unlimited ingestion, no credit card required.


## FAQs


### What happens when a Docker container becomes unhealthy?


When a Docker container becomes unhealthy, Docker changes its health status after the configured health check fails repeatedly. The container continues running, but failed checks are recorded in the container’s health history. This status can be viewed using docker ps or docker inspect. Becoming unhealthy does not stop the container process or remove the container automatically.


### Does Docker automatically restart unhealthy containers?


No. The HEALTHCHECK instruction only reports application health and does not trigger restart actions by itself. Container restarts are controlled by restart policies such as –restart=always or by external orchestration platforms. Unlike a container crash, a failed health check alone does not cause Docker to restart the container.


### How often does Docker run health checks?


Docker runs health checks according to the –interval parameter. If no interval is specified, Docker uses a default of 30 seconds. The frequency is not determined by container activity or incoming traffic.


### Can Docker Compose wait for healthy services?


Yes. Docker Compose can delay the startup of dependent services until another service reports a healthy status using depends_on with condition: service_healthy. This only controls startup order and does not continuously manage service dependencies after startup.


### How do I check container health status?


Use docker ps to see health status in the STATUS column, or docker inspect –format='{{json .State.Health}}’ <container_name> for detailed results including failure counts and health check output. For multi-service stacks, docker compose ps shows health for all services at once.


### What's the difference between health checks and readiness checks?


A Docker health check determines whether an application is functioning correctly. A readiness check (a Kubernetes concept) determines whether an application is ready to receive traffic. In Kubernetes, liveness probes map most closely to Docker health checks, while readiness probes control traffic routing independently.


### Why is my Docker container marked unhealthy?


A Docker container is marked unhealthy when its health check fails repeatedly and exceeds the allowed retry threshold. Common causes include incorrect endpoints, missing tools like curl in the image, dependency failures, DNS issues, and timeout settings that are too aggressive. Use docker inspect to review health check output and failure history, then run the check command manually inside the container to isolate the problem.


### What is start_interval in Docker Compose?


start_interval is a Docker Compose parameter (available in Docker 25+) that controls how frequently health checks run during the start_period grace window. The default is 5 seconds. This allows you to poll more aggressively during startup, catching readiness faster without changing the steady-state interval once the service is running.
