---
schema_version: "1.0.0"
document_id: "7314fd6d76b13fd2e04c32541a5f3af1b1781584bc7b6967495907926c6bc2a5"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-7cfcee1b8c2a"
canonical_url: "https://www.porter.run/blog/how-to-deploy-a-flask-app-on-aws"
published_at: "2024-06-10T00:00:00+00:00"
first_seen_at: "2026-07-22T09:55:21.208768+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:0abe9d64d91aee4f848be069f48fb3ad65317146f55097bc8e3255b318652216"
---

# How to Deploy a Flask App on AWS

Porter Standard brings the experience of a PaaS to your own cloud account. You get the flexibility of hosting on enterprise-grade AWS infrastructure, without actually worrying about how it's provisioned or managed - you just have to bring your GitHub repository.


While there are many AWS services available to deploy a Python Flask app, from AWS Elastic Beanstalk to serverless offerings like Lambda and ECS Fargate, Porter gives you the benefits of EKS, like scalability and availability, without having to manage DevOps (you don't have to worry about IaC/Infrastructure as Code or use the AWS Cloud Development Kit) or know anything about Kubernetes at all. Porter and the EC2s the platform spins up are a comparable cost to Fargate, without any of the issues associated with serverless, like cold starts.


In this guide, we're going to walk through using Porter to provision infrastructure in an AWS account (after which you don't really need to touch your AWS console to manage your web apps), and then deploy a simple Flask application to AWS EC2 (Elastic Compute Cloud) and have it up and running.


Note that to follow this guide, you'll need an account on Porter Standard along with an Amazon Web Services account. While Porter itself, doesn't have a free tier, if you're a startup that has raised less than 5M in funding, you can apply for the[Porter Startup Deal.](https://porter.run/for-seed-stage-startups) Combined with AWS credits, you can essentially run your infrastructure for free. We also offer a two-week free trial!


## **What We're Deploying**


We're going to deploy a sample Flask web server to an AWS EC2 instance - but that doesn't mean you're restricted to the Flask web framework. You're free to use any and all Python web frameworks (Python frameworks like FastAPI are common among Porter users). This app's a simple Flask application with a single endpoint - / to demonstrate how you can push out a public-facing app on Porter with a public facing domain and TLS. The idea here is to show you how a new web app can be quickly deployed on Porter, allowing you to then use the same flow for deploying your code.


You can find the repository for this sample Python Flask application here:[https://github.com/porter-dev/flask-getting-started](https://github.com/porter-dev/flask-getting-started) . Feel free to fork/clone it, or bring your own.


## **Getting started**


Deploying a Flask web app from a Github repository on Porter using AWS involves - broadly - the following steps:


1. Provisioning infrastructure inside your AWS account using Porter.
2. Creating a new app on Porter where you specify the repository, the branch, any build settings, as well as what you'd like to run.
3. Building your app and deploying it (Porter handles continuous integration and continuous deployment, or CI/CD).


## **Connecting your AWS account**


On the Porter dashboard, head to` Infrastructure` and select` AWS` .


Here you're required to log into your AWS account and provide your AWS account ID to Porter. Clicking on` Grant permissions` opens your AWS account and takes you to AWS CloudFormation, where you need to authorize Porter to provision a CloudFormation stack; this stack's responsible for provisioning all IAM roles and policies needed by Porter to provision and manage your infrastructure. Once the stack has been deployed, it takes a few minutes to complete:


Once the CloudFormation stack's created on AWS, you can switch back to the Porter tab, where you should see a message about your AWS account being accessible by Porter:


## **Provisioning infrastructure**


After connecting your AWS account to Porter, you'll see a screen with a form - the fields are pre-filled, with details about your cluster:


Ordinarily, the only two fields that should be tweaked are the cluster name and the region - feel free to change those. The other fields are usually changed if Porter detects a conflict between the proposed cluster's VPC and other VPCs in your account - these would be flagged during a preflight test, allowing you the option of tweaking those addresses. You can also choose a different instance type in this section for your cluster; while we tend to default to` t3.medium` instances, we support a lot more instance types. Once you're satisfied, click` Deploy` .


At this stage, Porter will run preflight checks to ensure your AWS account has enough quotas free for components like vCPUs, elastic IPs as well as any potential conflicts with address spaces belonging to other VPCs. If any issues are detected, these will be flagged on the dashboard along with troubleshooting steps.


Deploying your cluster can take up to 20 minutes. After this process, you won't really need to enter the AWS Management Console again to manage an AWS EC2 instance or your Flask web application.


## **Creating an App and Connecting Your Git Repository**


On the Porter dashboard, select` Create a new application` , which opens the following screen:


This is where you select a name for your Python web application and connect a Git repository containing your code. Once you've selected the appropriate repository, select the branch you'd like to deploy to Porter.


If you signed up for Porter using an email address instead of a Github account, you can easily connect your Github account to Porter by clicking on the profile icon on the top right corner of the dashboard, selecting` Account settings` , and adding your Github account.


## **Flask App Configuration: Build Settings**


Porter has the ability to automatically detect what language your web applications are written in and select an appropriate buildpack that can be used to package your app for eventual deployment automatically. Once you've selected the branch you wish to use, Porter will display the following screen:


You can further tune your build here. For instance, we're going to use the newer` heroku/builder:22` buildpack for our app.


## **Configuration of Services**


At this point, taking a quick look at applications and services is a good idea. An application on Porter is a group of services where each service shares the same build and the same environment variables. If your Fask project consists of a single repository with separate modules for, say, an API, a frontend, and a background worker, then you'd deploy a single application on Porter with three separate services. Porter supports three kinds of services: web, worker, and job services (allowing you to run cron tasks or one-off tasks which can be triggered via the dashboard, command line interface, or API).


Let's add a single` web service` for our app:


## **Flask App Configuration: Your Service**


Now that we've defined a single web service, it's time to tell Porter how it runs. That means specifying what command to run for this service, what CPU/RAM levels to allocate, and how it will be accessed publicly.


You can define what command you'd like Porter to use to run your app in the` Main` tab. This is required if your app's being built using a buildpack; this may be optional if you opt to use a Docker file(since Porter will assume you have an` ENTRYPOINT` in your Dockerfile and use that if it exists).


The` Resources` tab allows you to define how much CPU and RAM your app is allowed to access.


In this section, you can also define the number of replicas you'd like to run for this app and any autoscaling rules—these allow you to instruct Porter to add more replicas if resource utilization crosses a certain threshold.


The` Networking` tab is where you specify what port your app listens on. When you deploy a web app on Porter, we automatically generate a public URL for you to use - but you can also opt to bring your own domain by adding an A record to your DNS (Domain Name Service) records, pointing your domain at your cluster's public load balancer, and adding the custom domain in this section. This can be done at any point - either while you're creating the app or later once you've deployed it (and you won't have to worry about SSL certs).


If your app listens on` localhost` or` 127.0.0.1` , Porter won't be able to forward incoming connections and requests to your app. To that end, please ensure your app is configured to listen on` 0.0.0.0` instead.


## **Deployment Process: Review and Merge Porter's PR**


Hitting` Deploy` will show you the contents of a GitHub Action workflow that Porter would use to build and deploy your Flask project/app:


‍


This Github Action is configured to run every time you push a commit to the branch you specified earlier - when it runs, Porter applies the selected buildpack to your code, builds a final image, and pushes that image to Porter. Selecting` Deploy` app will allow Porter to open a PR in your repo, adding this workflow file:


All you need to do is merge this PR, and your build will commence.


You can also use the` Activity` tab on the Porter dashboard to see a timeline of your build+deployment going through. Once the build succeeds, you'll also be able to see the deployment in action:


## **Accessing Your App**


Your app's now live on Porter. The Porter-generated unique URL is now visible on the dashboard under your Flask web app's name. Let's test it:


Porter also provides logging and monitoring in the form of app logs and resource consumption metrics on the dashboard, so you can see how your Flask project is faring:


### **Exploring Further**


We've seen how you can go about deploying a Flask web application using Porter on an EC2 instance in your AWS Cloud, without choosing an Amazon Machine Image, without having to configure a security group, and certainly not having to configure NGINX reverse proxy for load balancing - Porter takes care of it for you.


You can also create production data stores on Porter, including a Postgres database like RDS or Aurora. Although it’s simply the AWS-managed service being created, Porter will take care of VPC peering and networking conflicts (so IP range and CIDR range conflicts don’t occur) and provide you with an environment group you can inject into your application so your app can talk to that RDS instance.


Here are a few pointers to help you dive further into configuring/tuning your app:


1. [Adding your own domain](https://docs.porter.run/configure/custom-domains) .
2. [Adding environment variables and groups](https://docs.porter.run/configure/environment-groups) .
3. [Scaling your app](https://docs.porter.run/configure/autoscaling) (Porter takes care of auto scaling).
4. [Ensuring your app's never offline](https://docs.porter.run/configure/health-checks) (we’ll renew and manage the SSL certificate for you).
