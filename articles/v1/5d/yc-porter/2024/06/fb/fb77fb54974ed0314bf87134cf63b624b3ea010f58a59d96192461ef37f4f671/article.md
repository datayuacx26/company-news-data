---
schema_version: "1.0.0"
document_id: "fb77fb54974ed0314bf87134cf63b624b3ea010f58a59d96192461ef37f4f671"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-7cfcee1b8c2a"
canonical_url: "https://www.porter.run/blog/how-to-deploy-a-rails-app-on-aws"
published_at: "2024-06-11T00:00:00+00:00"
first_seen_at: "2026-07-22T09:55:21.208768+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:fe1682b11922dcebf1b6278644dfcbc927768f9783f8c1fbd166033ce3f925cc"
---

# How to Deploy a Rails App on AWS

Porter Standard brings the experience of a traditional PaaS hosting service to your own AWS cloud. You get the flexibility of running your own enterprise-grade infrastructure in your own VPC, without actually worrying about how it's provisioned or managed - you just have to bring your GitHub repository.


While there are many AWS services available to deploy a Ruby on Rails application, from AWS Elastic Beanstalk and ECS (Elastic Container Service) to Fargate, Porter gives you the benefits of EKS, like scalability and high availability, without having to manage DevOps or know anything about EKS at all. Porter and every EC2 instance the platform spins up are a comparable cost to Fargate, without any of the issues associated with a serverless hosting service, like cold starts.


In this guide, we're going to walk through using Porter to provision infrastructure in an AWS account (after which you don't really need to touch your AWS management console to manage your web apps), and then deploy a sample Rails app and have it up and running.


Note that to follow this blog post, you'll need an account on[Porter Standard](http://dashboard.getporter.dev/register) along with an AWS account. While Porter itself, doesn't have a free tier, if you're a startup that has raised less than 5M in funding, you can apply for the[Porter Startup Deal.](https://porter.run/for-seed-stage-startups) Combined with AWS credits, you can essentially run your infrastructure for free. We also offer a two-week long free trial!


## **What We're Deploying**


We're going to deploy a sample Rails app running behind Puma - but you’re free to use frameworks like Sinatra. This app's a very basic app with one endpoint - / to demonstrate how you can push out a public-facing app on Porter with a public facing domain and TLS. The idea here is to show you how a basic app can be quickly deployed onto EC2 instances using Porter, allowing you to then use the same flow for deploying your code.


You can find the repository for this sample app here:[https://github.com/porter-dev/rails-getting-started](https://github.com/porter-dev/rails-getting-started) . Feel free to fork/clone it, or bring your own.


## **Getting started**


Deploying an app from a Github repository on Porter using AWS involves - broadly - the following steps:


1. Provisioning infrastructure inside your AWS account using Porter.
2. Creating a new app on Porter where you specify the repository, the branch, any build settings, as well as what you'd like to run.
3. Building your app and deploying it(automatically handled by Porter).


## **Connecting your AWS account**


On the Porter dashboard, head to` Infrastructure` and select` AWS` .


Here you're required to log into your AWS account and provide your AWS account ID to Porter. Clicking on` Grant permissions` opens your AWS account and takes you to AWS CloudFormation, where you need to authorize Porter to provision a CloudFormation stack; this stack's responsible for provisioning all IAM roles and policies needed by Porter to provision and manage your infrastructure. Once the stack has been deployed, it takes a few minutes to complete:


Once the CloudFormation stack's created on AWS, you can switch back to the Porter tab, where you should see a message about your AWS account being accessible by Porter:


## **Provisioning infrastructure**


After connecting your AWS account to Porter, you'll see a screen with a form - the fields are pre-filled, with details about your cluster:


Ordinarily, the only two fields that should be tweaked are the cluster name and the region - feel free to change those. The other fields are usually changed if Porter detects a conflict between the proposed cluster's VPC and other VPCs in your account - these would be flagged during a preflight test, allowing you the option of tweaking those addresses. You can also choose a different instance type in this section for your cluster; while we tend to default to` t3.medium` instances, we support a lot more instance types. Once you're satisfied, click Deploy.


At this stage, Porter will run preflight checks to ensure your AWS account has enough quotas free for components like vCPUs, elastic IPs as well as any potential conflicts with address spaces belonging to other VPCs. If any issues are detected, these will be flagged on the dashboard along with troubleshooting steps.


Deploying your cluster can take up to 20 minutes, and Porter will also have access to your Amazon Elastic Container Registry. After this process, you won't really need to enter the AWS console again to manage your web application.


## **Creating an App and Connecting Your GitHub Repository**


On the Porter dashboard, select` Create a new application` , which opens the following screen:


This is where you select a name for your app and connect a Git Hub repository containing your code. Once you've selected the appropriate repository, select the branch you'd like to deploy to Porter.


If you already have your own CI/CD pipeline and maintain your own artifacts, like a Docker image, then the onus of setting up CI/CD for your web server's deployment is on you - if you’re looking for a more traditional, PaaS-like flow, you can just choose the repository you want to deploy and Porter will take care of CI/CD for you, using Github actions under the hood.


### Note


**‍** If you signed up for Porter using an email address instead of a Github account, you can easily connect your Github account to Porter by clicking on the profile icon on the top right corner of the dashboard, selecting` Account settings` , and adding your Github account.


## **Configuring Build Settings**


Porter has the ability to automatically detect what language your app is written in and select an appropriate buildpack that can be used to package your app for eventual deployment automatically. Once you've selected the branch you wish to use, Porter will display the following screen:


You can further tune your build here. For instance, we're going to use the newer` Heroku/builder:22` buildpack for our app.


## **Configure Services**


At this point, taking a quick look at applications and services is a good idea. An application on Porter is a group of services where each service shares the same build and the same environment variables. If your app consists of a single repository with separate modules for, say, an API a frontend, and a background worker, then you'd deploy a single application on Porter with three separate services. Porter supports three kinds of services:` web` ,` worker` , and` job` services.


Let's add a single` web` service for our app:


## **Configure Your Service**


Now that we've defined a single web service, it's time to tell Porter how it runs. That means specifying what command to run for this service, what CPU/RAM levels to allocate, and how it will be accessed publicly.


You can define what command you'd like Porter to use to run your app in the Main tab. This is required if your app's being built using a buildpack; this *may* be optional if you opt to use a Dockerfile(since Porter will assume you have an` ENTRYPOINT` in your Dockerfile and use that if it exists).


The Resources tab allows you to define how much CPU and RAM your app's allowed to access. Porter imposes a limit on the resources that can be used by a single app; if your app needs more, it might make sense to look at Porter Standard instead, which allows you to bring your own infrastructure and have more flexibility in terms of resource limits.


In this section, you can also define the number of replicas you'd like to run for this app and any autoscaling rules—these allow you to instruct Porter to add more replicas if resource utilization crosses a certain threshold.


The Networking tab is where you specify what port your app listens on. When you deploy a web app on Porter, we automatically generate a public URL for you to use - but you can also opt to bring your own domain by adding an A record to your DNS (Domain Name Service) records, pointing your domain at your cluster's public load balancer, and adding the custom domain in this section. This can be done at any point - either while you're creating the app or later once you've deployed it.


### Note


If your app listens on` localhost` or` 127.0.0.1` , Porter won't be able to forward incoming connections and requests to your app. To that end, please ensure your app is configured to listen on` 0.0.0.0` instead.


## **Review and Merge Porter's PR**


Hitting` Deploy` will show you the contents of a GitHub Action workflow that Porter would use to build and deploy your app:


This Github Action is configured to run every time you push a commit to the branch you specified earlier - when it runs, Porter applies the selected buildpack to your code, builds a final image, and pushes that image to Porter. Selecting` Deploy app` will allow Porter to open a PR in your repo, adding this workflow file:


All you need to do is merge this PR, and your build will commence.


You can also use the` Activity` tab on the Porter dashboard to see a timeline of your build+deployment going through. Once the build succeeds, you'll also be able to see the deployment in action:


## **Accessing Your App**


Your app's now live on Porter. The Porter-generated unique URL is now visible on the dashboard under your app's name. Let's test it:


You can also check logging and monitoring in the form of app logs and resource consumption metrics on the dashboard, to see how your web application’s doing:


## **Exploring Further**


Through this blog post, we've seen how you can go about deploying your Rails app onto an EC2 instance using Porter, without server configuration - not having to choose an Amazon Machine Image or configure instance details, EC2 security groups, or auto scaling group. Porter allows users to use infrastructure with built-in scalability and high availability with no DevOps overhead.


You can also create production data stores on Porter, including a Postgres database like Amazon RDS or Aurora. Although it’s simply the AWS-managed RDS instance being created, Porter will take care of VPC peering and networking conflicts (so IP range and CIDR range conflicts don’t occur) and provide you with an environment group you can inject into your application so your app can talk to that RDS instance. Other than Amazon RDS and Aurora, users can also spin up Elasticache instances using Porter.


Here are a few pointers to help you dive further into configuring/tuning your app:


1. [Adding your own domain](https://docs.porter.run/configure/custom-domains) .
2. [Adding environment variables and groups](https://docs.porter.run/configure/environment-groups) .
3. [Scaling your app](https://docs.porter.run/configure/autoscaling) (Porter takes care of auto scaling).[‍](https://docs.porter.run/configure/health-checks)
4. [Ensuring your app's never offline](https://docs.porter.run/configure/health-checks) (we’ll renew and manage the SSL certificate for you).
