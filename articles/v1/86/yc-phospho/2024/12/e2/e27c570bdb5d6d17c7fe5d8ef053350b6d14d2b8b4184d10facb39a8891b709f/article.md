---
schema_version: "1.0.0"
document_id: "e27c570bdb5d6d17c7fe5d8ef053350b6d14d2b8b4184d10facb39a8891b709f"
company_key: "yc-phospho"
company: "phospho"
source_id: "yc-phospho-rss-5d0953ae6f5a"
canonical_url: "https://blog.phospho.ai/a-simple-guide-to-set-up-your-llm-backoffice-with-docker/"
published_at: "2024-12-11T13:03:15+00:00"
first_seen_at: "2026-07-25T18:58:01.685792+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:75aa08c2ea9a6bc13cbe795365f088f3ba470aba6c38ad0b6b792a8749b60a21"
---

# A Simple Guide to Set Up Your LLM Backoffice with Docker

Exciting news, phospho is now bringing brains to robots!


With phosphobot, you can control robots, collect data, fine-tune robotics AI models, and deploy them in real-time.


Check it out here:[robots.phospho.ai](https://robots.phospho.ai/?ref=blog.phospho.ai) .


## **Simplify Your LLM Management with phospho**


Deploying and managing tools for large language models (LLMs) can feel overwhelming, but with phospho, it’s never been easier. phospho’s Docker-powered setup ensures that you can get started quickly, no matter your level of technical expertise. In this guide, we’ll focus on how you can deploy phospho using Docker Compose in just a few straightforward steps.


---


### **Why Docker Makes phospho Easy to Deploy**


Docker eliminates the need for complicated installations and configurations by bundling everything phospho needs into lightweight containers. With just a few commands, you’ll have a fully operational LLM management backoffice up and running.


Ready to deploy with Docker?[Check out our detailed Docker deployment guide](https://github.com/phospho-app/phospho/blob/dev/DeploymentGuide.md?ref=blog.phospho.ai) for step-by-step instructions on setting up phospho with Docker.


---


### **Quick and Simple Steps to Deploy phospho**


### **1. Prerequisites:**


**1. Create a Propelauth Account**


Propelauth handles authentication and security.


- Sign Up:[Create your Propelauth account](https://propelauth.com/signup?ref=blog.phospho.ai)


**2. Obtain an OpenAI API Key**


You'll need an OpenAI API Key for analytics and clustering features.


- Get API Key:[Register and obtain your OpenAI API Key](https://platform.openai.com/account/api-keys?ref=blog.phospho.ai)


**3. Register for MongoDB**


MongoDB serves as your database. Choose between a local installation or MongoDB Atlas cloud service.


- Sign Up:[Register for MongoDB](https://www.mongodb.com/try?ref=blog.phospho.ai)


**4. Install Docker**


Docker containerizes your application to ensure consistency across environments.


- Install Docker:[official Docker installation guide](https://docs.docker.com/get-docker/?ref=blog.phospho.ai) .


**5. Set up Google Cloud Platform (GCP)**


The GCP Natural Language API detects language and sentiment in user messages.


- Get Started with GCP:[Google Cloud Platform](https://cloud.google.com/natural-language/docs?ref=blog.phospho.ai)


**6. Set up Temporal**


Temporal manages jobs and long-running computations.


- Get Started with Temporal:[Get started guide](https://learn.temporal.io/getting_started/?ref=blog.phospho.ai)


### **2. Clone the Repository**


The first step is to get the source code. Open your terminal and run:


```text
git clone <https://github.com/phospho-app/phospho.git>


```


This clones the phospho repository to your local machine.


---


### **3. Create Your Environment File**


Configuration is streamlined with an` .env.docker` file. Simply create this file in the root directory of the project and paste the following template:


```text
# Application Environment
APP_ENV=local
ENVIRONMENT="test"


# API Configuration
API_VERSION=v0
NEXT_PUBLIC_API_VERSION=v0
NEXT_PUBLIC_API_URL=http://127.0.0.1:8000
LOCAL_API_URL=http://127.0.0.1:8000
NEXT_PUBLIC_LOCAL_API_URL=http://127.0.0.1:8000


# Authentication Settings
NEXT_PUBLIC_AUTH_URL=https://<your_id>.propelauthtest.com
PROPELAUTH_API_KEY=
PROPELAUTH_REDIRECT_URI=http://localhost:3000/api/auth/callback
PROPELAUTH_VERIFIER_KEY=-----BEGIN PUBLIC KEY-----
PROPELAUTH_URL="https://<your_id>.propelauthtest.com"


# GCP Configuration
GCP_PROJECT_ID=""
GCP_SERVICEACCOUNT_EMAIL=
GCP_PRIVATE_KEY=-----BEGIN PRIVATE KEY-----
GCP_JSON_CREDENTIALS_NATURAL_LANGUAGE_PROCESSING=
GCP_JSON_CREDENTIALS_BUCKET=


# OPENAI API Key For Chat and Embedding models
OPENAI_API_KEY=
OPENAI_BASE_URL=
# MongoDB Database Configuration
MONGODB_NAME="test"
MONGODB_URL=""


# Local Development Settings
TEMPORAL_HOST_URL=host.docker.internal:7233
TEMPORAL_NAMESPACE=default
TEMPORAL_MTLS_TLS_CERT_BASE64=
TEMPORAL_MTLS_TLS_KEY_BASE64=


# (OPTIONNAL) Services and Integrations
RESEND_API_KEY=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
SENTRY_DSN=
EXTRACTOR_SENTRY_DSN=
SLACK_URL=
AZURE_OPENAI_KEY=
AZURE_OPENAI_ENDPOINT=
TEST_PROPELAUTH_ORG_ID=
TEST_PROPELAUTH_USER_ID=


```


Fill in your credentials, and copy the file into the` platform` ,` ai-hub` , and` backend` folders.


---


### **4. Run Temporal (Optional for Development)**


If you need Temporal for workflow management, you can start it locally with:


```text
temporal server start-dev --db-filename your_temporal.db --ui-port 7999


```


This step is optional if you are not working with Temporal-dependent features.


---


### **5. Start phospho with Docker Compose**


Now for the magic. Simply navigate to the project directory and run:


```text
docker compose up


```


Docker Compose will handle building and running the entire phospho platform. No need to worry about dependencies or environment mismatches—it just works.


---


### **Why Choose phospho with Docker?**


- **Ease of Setup:** Minimal configuration means you’re ready to go in minutes.
- **Consistency Across Environments:** Whether you’re on your local machine or deploying to a server, Docker ensures everything works seamlessly.
- **Focus on What Matters:** Skip the complicated setups and dive straight into using phospho’s powerful features.


---


0:00


/ 0:10


## Want to take AI to the next level?


At Phospho, **we give brains to robots** . We let you power any robot with advanced AI – control, collect data, fine-tune, and deploy seamlessly.


New to AI robotics? Start with our **dev kit** .


👉 Explore at[robots.phospho.ai](https://robots.phospho.ai/?ref=blog.phospho.ai) .


If you encounter any challenges, reach out to our support team at` contact@phospho.ai` .
