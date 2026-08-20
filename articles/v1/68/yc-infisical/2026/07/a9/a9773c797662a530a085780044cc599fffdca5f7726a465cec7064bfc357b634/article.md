---
schema_version: "1.0.0"
document_id: "a9773c797662a530a085780044cc599fffdca5f7726a465cec7064bfc357b634"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/docker-secrets-management"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T19:10:17.136731+00:00"
fetched_at: "2026-07-31T19:10:18.826077+00:00"
content_hash: "sha256:20c84effc6635d7dba13fffaa1942d09589e774a767f49f31c27422190d9389b"
---

# Docker Secrets Management: A Complete Guide to Builds, Compose, and Swarm

Containerizing an application introduces a shift in how we manage sensitive credentials. As deployments scale from a developer's workstation to multi-node clusters, the security requirements and attack surfaces evolve. To choose the right secrets strategy, it helps to look at the different ways Docker is utilized across the software development lifecycle, the typical organization size using them, and the specific secrets management challenge each environment presents:


Docker Environment Scale & Org Type The Secrets Management Challenge


**Image Building**
Docker BuildKit All teams building custom containers. **Image Leakage** : Passing secrets via build` ARG` or` ENV` permanently bakes credentials into the public image layer history.


**Local Development**
Docker Compose Solo developers to large engineering teams. **Local Sprawl** : Keeping credentials out of plaintext` .env` files that risk being accidentally committed to version control.


**Single-Host Production**
` docker compose` Small startups and single-VM setups. **Host Exposure** : Plaintext environment variables remain visible to debugging commands (like` docker inspect` ), logs, and monitoring agents.


**Multi-Node Production**
Docker Swarm Mid-sized companies and scaling clusters. **Cluster Distribution** : Securely synchronizing database passwords and API keys across multiple nodes without writing them to disk.


No matter the size of an organization, managing secrets inside containerized environments presents unique challenges compared to traditional virtual machine or bare-metal deployments.


- **Image distribution risks:** Docker builds package everything into an image. If a secret is hardcoded or passed via traditional build variables during compilation, it becomes permanently baked into the image history. Anyone with access to pull the image can extract the secret.
- **Ephemeral deployments:** Containers are temporary and scale dynamically. Secrets must be injected securely at runtime without leaving plaintext remnants on the host disk or in the container shell history.
- **Runtime exposure:** Environment variables are often visible to host processes, monitoring agents, and logs. Docker requires specialized mechanisms to mount secrets as files directly in the container memory space.


Fortunately, Docker provides functionality for each stage of the software development lifecycle that can help limit the exposure of secrets. If you pair that functionality with a secret manager like Infisical, you end up with a streamlined and secure development experience.


To see how this works in practice, we can step through an end-to-end example of a containerized Node.js application, highlighting the key considerations and patterns at each stage.


## Prerequisites


When developing software, a secret manager like Infisical makes it easy to securely store and access your secrets. Infisical lets you:


- centrally manage your secrets
- automate secret rotation
- fetch secrets at runtime ... and[much more](https://infisical.com/) .


Crucially for Docker, Infisical's flexible clients make it easy to integrate with native Docker's native secrets management, so we will start our example by spending 10 minutes or less to set up a few things in Infisical. You'll need:


1. [A free Infisical account](https://app.infisical.com/signup)
2. A project in Secrets Management
3. Two secrets:` /build/NPM_TOKEN` and` /deploy/DATABASE_PASSWORD` (values don't matter for this example)
4. A machine identity with[OpenID Connect (OIDC) authentication for GitHub Actions](https://infisical.com/docs/documentation/platform/identities/oidc-auth/github)
5. [Infisical CLI](https://infisical.com/docs/cli/overview) on your dev workstation


With Infisical ready to go, we can get started with our application.


## The core application


We start our Docker container app with a simple Node.js application that connects to a PostgreSQL database using a secret database password. It reads the password from either an **environment variable** (` DATABASE_PASSWORD` ) or a special **Docker secret file** (` /run/secrets/db_password` ).


` app.js`


```text
const express = require('express');
const { Pool } = require('pg');
const fs = require('fs');
const app = express();
const observer = require('observer');


let dbPassword = process.env.DATABASE_PASSWORD;
if (fs.existsSync('/run/secrets/db_password')) {
dbPassword = fs.readFileSync('/run/secrets/db_password', 'utf8').trim();
}


const pool = new Pool({
host: process.env.DB_HOST,
user: 'postgres',
password: dbPassword,
connectionTimeoutMillis: 2000,
});


pool.on('error', (err) => {
console.error('Unexpected error on database pool:', err);
observer.trace(err);
});


app.get('/status', async (req, res) => {
if (!dbPassword) {
return res.status(500).json({ error: "Missing DATABASE_PASSWORD" });
}


try {
const dbRes = await pool.query('SELECT NOW()');
res.json({
status: "ready",
database: "connected",
time: dbRes.rows[0].now
});
} catch (err) {
res.status(500).json({ error: err.message });
observer.trace(err);
}
});


app.listen(3000, () => console.log('Listening on port 3000'));


```


Our application will need to access a database password at runtime, but we don't need to worry about that quite yet. Our first secret management challenge will be building the Docker image that hosts our application.


## Container image build


In this example, our app needs a custom package called` observer` from a private registry. To access this package, our container image will need a private NPM token at build-time.


When you build Docker images, traditional methods of passing variables like using` ARG` or` ENV` are insecure for passing secrets because they leave **permanent traces in the image layers** . Anyone who pulls the image or inspects its history using` docker history` can easily extract your credentials. **Even if you try to clean up or delete the secret in a later step** , the secret remains saved in the intermediate layers of the image.


To avoid this, we must use **BuildKit (Docker's modern build engine) mount-type secrets** . This feature allows us to temporarily mount a secret during the build process (like an` NPM_TOKEN` to access a private package) without baking it into the final image:


` Dockerfile`


```text
FROM node:24-alpine
WORKDIR /app
COPY package*.json ./


# Mount the secret temporarily and verify it is accessible during build
RUN --mount=type=secret,id=npm_token \
NPM_TOKEN=$(cat /run/secrets/npm_token) && \
if [ -n "$NPM_TOKEN" ]; then \
echo "Private NPM registry login..." && npm ci; \
else \
echo "Error: NPM token missing!" && exit 1; \
fi && \
npm install


COPY app.js .
CMD ["node", "app.js"]


```


To get the secret into the build process, we use Infisical CLI to fetch the secret and then pass it to the` docker build` command using the` --secret` flag:


```text
# Initialize the local directory to use our Infisical project (one-time setup)
infisical init


# Fetch secret from Infisical using CLI
export NPM_TOKEN=$(infisical secrets get --path=/build NPM_TOKEN --plain)


# Build the image securely
docker build --secret id=npm_token,env=NPM_TOKEN -t demo-app:latest .


```


Now we have a safe way to inject the NPM token as the container image is being built, and there are no traces of the secret to be found. We are ready to give our container a test run, but first we need to figure out how to give it the database password it needs.


## Standalone container run


If you need to run a standalone container locally, writing secrets to unencrypted` .env` files is a bad habit that often leads to accidental Git commits or compromised secrets in a supply chain attack. Similarly, passing secrets directly in the container start command (e.g., via` -e DATABASE_PASSWORD=myplaintextpassword` ) is also a security risk because it leaks the secret in your terminal command history and makes it visible to any user or system process on the host machine.


Instead of exposing secrets in files or command line args, we can use the Infisical CLI as a wrapper to inject the secrets directly into the container's environment:


```text
infisical run --path=/deploy -- docker run -d --name my-app -p 3000:3000 -e DATABASE_PASSWORD -e DB_HOST=db-host demo-app:latest


```


By using` infisical run` , the secret is dynamically injected into the container's execution environment. No plain text secrets ever touch the disk, and they won't show up in your terminal history. There is a drawback to our current container run approach, though, and it doesn't have to do with Infisical.


The` docker run` command uses the` -e` flag to pass an environment variable through to the container, which can expose secrets in multiple ways:


- The` docker inspect` command shows the value in plain text
- Child processes inherit the secret automatically
- Error logs may dump the environment and expose the secret


Fortunately, Docker provides a secure way to pass secrets to containers, but you have to use a Docker Compose file.


## Test Compose deployment


For local multi-container testing, Docker Compose is the go-to tool. It provides a native` secrets` function that injects sensitive values as files into the container's filesystem rather than using environment variables.


Let's create a compose file for our project and deploy our app image alongside a database container. We will add a native Docker secret for the database password.


` compose.yaml`


```text
services:
web:
build: .
ports:
- "3000:3000"
environment:
- DB_HOST=db
secrets:
- db_password
depends_on:
db:
condition: service_healthy


db:
image: postgres:18-alpine
environment:
- POSTGRES_PASSWORD_FILE=/run/secrets/db_password
ports:
- "5432:5432"
secrets:
- db_password
healthcheck:
test: ["CMD-SHELL", "pg_isready -U postgres"]
interval: 2s
timeout: 2s
retries: 10


secrets:
db_password:
environment: DATABASE_PASSWORD


```


In our example, the` db_password` Docker secret is getting its value from an environment variable on the Docker host, and then passing that into the container images as a file. When running a Compose app, the most common practice is to have a` .env` file in the folder that contains our secrets like the database password, but that exposes our sensitive credentials outside of a secure vault and increases the risk of committing secrets in version control.


To avoid this, we can use the Infisical CLI to inject secrets dynamically. We run a simple wrapper command to feed the Compose app its configuration dynamically:


```text
infisical run --path=/deploy -- docker compose up -d


```


Infisical CLI will inject the database password into an environment variable, Docker Compose will convert that to a secure file within the container, and then our app's process will read that file and use the password to connect to the database.


Once we are done testing our application, we are ready to send it to GitHub for review and publication.


## CI/CD build (GitHub Actions)


To deploy our app to production, we will use an automated pipeline in GitHub Actions to build a production image. A common mistake here is storing long-lived build credentials like our` NPM_TOKEN` directly as GitHub repository secrets. While GitHub secrets are fairly secure, they tend to be difficult to manage at scale and can fall out of sync.


To have a single source of truth for our secrets, we can configure the workflow to pull secrets from Infisical using the official GitHub Action. Best of all, by authenticating via OpenID Connect (OIDC), we don't have to store any long-lived Infisical credentials in GitHub. OIDC uses a temporary token bound to your GitHub repository's workflow:


` .github/workflows/build-push.yaml`


```text
name: Build and Push with Infisical Secrets
on:
push:
branches: [main]


permissions:
id-token: write # Needed for OIDC auth to Infisical
contents: read


jobs:
build:
runs-on: ubuntu-latest
steps:
- name: Checkout repository
uses: actions/checkout@v6


- name: Get Infisical Secrets
uses: Infisical/secrets-action@v1.0.16
with:
method: "oidc"
domain: "https://app.infisical.com"
identity-id: <your_machine_identity_id>
project-slug: "docker-demo"
secret-path: "/build"
env-slug: "prod"


- name: Build and push
uses: docker/build-push-action@v7
with:
push: false
tags: demo-app:latest
secrets: |
npm_token=${{ env.NPM_TOKEN }}


```


Now we have a production-ready image built and published, and we are ready to deploy it. We could use Docker Compose again in production, but Docker Swarm (Docker's native container orchestration tool) offers advantages that make it a better fit for production deployments:


- Multi-server clustering
- Replicas and scaling
- Rolling updates
- Load balancing


## Production Swarm deployment


Since our production container image will get deployed to a Docker Swarm cluster, we have to make a tweak to how we get our database password from Infisical to the application. Because a Swarm cluster typically uses multiple nodes, the platform has a native` Secrets` API that securely distributes secrets to the containers running in the cluster.


To keep things simple and consistent in our application, we will use the exact same` compose.yaml` file from Docker Compose, but this time we will add a small production override file (` compose.prod.yaml` ) that tells Docker to use the` Secrets` API instead of an environment variable.


First, we define the override file to specify that` db_password` is a Swarm-managed external secret:


` compose.prod.yaml`


```text
secrets:
db_password:
external: true


```


Next, we actually load the secret into our Swarm cluster using Infisical CLI:


```text
infisical secrets get --path=/deploy DATABASE_PASSWORD --plain | docker secret create db_password -


```


Now, we can deploy our app with the compose files, and Docker will inject the secret into the container's file system.


```text
# Deploy stack using both compose files
docker stack deploy -c compose.yaml -c compose.prod.yaml swarm-stack


```


Through this approach, our secrets are securely managed and rotated in Infisical, and deploying production containers remains simple and consistent.


## Scaling secrets management across the Docker ecosystem


Secrets management inside containerized environments is different from traditional virtual machines or bare-metal servers. When organizations do not use a centralized secrets manager, they typically fall back on ad-hoc methods like manual configuration files or raw environment variables. This approach introduces three major security risks:


- **Secret sprawl:** Without a single source of truth, credentials end up scattered across developer laptops, version control settings, server configuration files, and hosting platforms. Tracking which systems use which credentials becomes impossible.
- **Friction in credential rotation:** When a secret needs to be changed, operators must manually update every environment file, rebuild the affected Docker images, and restart all dependent containers. This manual overhead leads to configurations drifting out of sync and makes regular rotation impractical.
- **Lack of visibility:** Decentralized setups do not generate central logs. If a database password or API token is leaked, there is no way to audit which container, application, or developer accessed the credential.


Securing credentials in containerized environments starts with using the right Docker API, but you can take it to the next level by establishing a unified lifecycle from local development to production. By combining the native mounting features of Docker with a centralized secrets manager like Infisical, engineering teams can maintain developer speed while keeping their environments secure.
