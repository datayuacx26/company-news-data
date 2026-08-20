---
schema_version: "1.0.0"
document_id: "318a53eda76a0bfbd63b666bf44a42b59b673cc5cd872f8b2a0fa16f5fd7daa1"
company_key: "yc-emi-labs"
company: "Emi Labs"
source_id: "yc-emi-labs-rss-87232385bc09"
canonical_url: "https://medium.com/@EmiLabsTech/how-to-setup-self-hosted-sentry-in-elastic-beanstalk-aws-f735f42f277a"
published_at: "2023-01-16T21:41:22+00:00"
first_seen_at: "2026-07-27T09:02:34.241036+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:655b6734da9cef52f66350c60f0d2f27d7e7706d9fef6e4073f0a62ee6a52e7d"
---

# How to setup self-hosted Sentry in Elastic Beanstalk (AWS)

# How to setup self-hosted Sentry in Elastic Beanstalk (AWS)


[Emi Labs Tech - Ravens](https://medium.com/@EmiLabsTech?source=post_page---byline--f735f42f277a---------------------------------------)


7 min read


·


Jan 16, 2023


--


by[Marina Huberman](https://www.linkedin.com/in/marinahuberman/)


*Our team at Emi uses Sentry as the main error-monitoring software, which helps us not only to diagnose and fix errors but also, allows triggering anomaly alerts by using warnings along with statistical alert rules.*


## About Elastic Beanstalk


Elastic Beanstalk is a great option for starting with microservices in AWS: it’s kind of a low-effort alternative to Cloudformation since it handles application deployment and services provisioning for you (load balancing, auto-scaling, and health monitoring).


## Sentry Self-hosted vs. Cloud


For our Sentry implementation, we chose to use the self-hosted (previously called on-premise) version, since it’s way cheaper while it offers almost all the features the cloud one has. Also, self-hosted Sentry is open source, meaning that:


- the code is available in GitHub, and that’s rather handy in case the official documentation is not completely clear.
- Sentry’s open-source team will be there to follow up on GitHub issues (in 2021 we reported issues with the Postgres version upgrade and[the experience was great](https://github.com/getsentry/self-hosted/issues/1097#issuecomment-973255231) , working alongside them to fix it together).


## Pre-requisites


In this post, we’ll explain how to set up a self-hosted Sentry application using a single instance Elastic Beanstalk environment with the Amazon Linux 2 Docker platform, so you’ll need at least some basic knowledge of Docker, Elastic Beanstalk, and/or AWS in general.


## Architecture


The proposed final architecture looks like this:


High-level architecture of Sentry self-hosted running in an Elastic Beanstalk environment


## Setup self-hosted Sentry


### Fork Sentry repository


We suggest using` getsentry/onpremise` GitHub repository as upstream and working in your own separate branch, then rebase the latter on new release branches for upgrading.


You can use this` .git/config` snippet as a reference:


```text
[remote "getsentry"]      url = https://github.com/getsentry/onpremise.git      fetch = +refs/heads/*:refs/remotes/getsentry/*  [branch "releases/20.11.1"]      remote = getsentry      merge = refs/heads/releases/20.11.1  [branch "master"]      remote = origin      merge = refs/heads/master
```


### Setup Elastic Beanstalk environment


Create a new single-instance Elastic Beanstalk environment with the Amazon Linux 2 Docker platform. Bare in mind that instance size will depend on the volume, but at least a t2.medium instance type will be needed to allow the deployment. This is what your` Dockerrun.aws.json` should look like:


```text
{      "AWSEBDockerrunVersion": "3",      "Image": {          "Update": "false"      }  }
```


### Postgres setup


Self-hosted Sentry uses an on-premise Postgres database, running in a Docker container along with everything else. This is not at all suitable for reliability and scaling so we decided to move the Postgres instance to an RDS. The following steps are optional but strongly recommended.


docker-compose.yml file:


```text
@@ -11,7 +11,6 @@ x-sentry-defaults: &sentry_defaults        image: sentry-onpremise-local        depends_on:          - redis  -       - postgres          - memcached          - smtp          - snuba-api      @@ -61,13 +62,6 @@ services:            nofile:              soft: 10032              hard: 10032  -       postgres:  -         << : *restart_policy  -         image: 'postgres:9.6'  -         environment:  -           POSTGRES_HOST_AUTH_METHOD: 'trust'  -         volumes:  -           - 'sentry-postgres:/var/lib/postgresql/data'        zookeeper:          << : *restart_policy          image: 'confluentinc/cp-zookeeper:5.5.0'      @@ -233,8 +227,6 @@ services:      volumes:        sentry-data:          external: true  -     sentry-postgres:  -       external: true        sentry-redis:          external: true        sentry-zookeeper:
```


install.sh file:


```text
@@ -140,7 +140,6 @@ fi  echo ""  echo "Creating volumes for persistent storage..."  echo "Created $(docker volume create --name=sentry-data)."  - echo "Created $(docker volume create --name=sentry-postgres)."   @@ -267,27 +271,6 @@ for topic in $NEEDED_KAFKA_TOPICS; do    fi  done   -# Very naively check whether there's an existing sentry-postgres volume and the PG version in it  -if [[ -n "$(docker volume ls -q --filter name=sentry-postgres)" && "$(docker run --rm -v sentry-postgres:/db busybox cat /db/PG_VERSION 2>/dev/null)" == "9.5" ]]; then  -  docker volume rm sentry-postgres-new || true  -  # If this is Postgres 9.5 data, start upgrading it to 9.6 in a new volume  -  docker run --rm \  -  -v sentry-postgres:/var/lib/postgresql/9.5/data \  -  -v sentry-postgres-new:/var/lib/postgresql/9.6/data \  -  tianon/postgres-upgrade:9.5-to-9.6  -  -  # Get rid of the old volume as we'll rename the new one to that  -  docker volume rm sentry-postgres  -  docker volume create --name sentry-postgres  -  # There's no rename volume in Docker so copy the contents from old to new name  -  # Also append the `host all all all trust` line as `tianon/postgres-upgrade:9.5-to-9.6`  -  # doesn't do that automatically.  -  docker run --rm -v sentry-postgres-new:/from -v sentry-postgres:/to alpine ash -c \  -    "cd /from ; cp -av . /to ; echo 'host all all all trust' >> /to/pg_hba.conf"  -  # Finally, remove the new old volume as we are all in sentry-postgres now  -  docker volume rm sentry-postgres-new  -fi@@ -267,27 +271,6 @@ for topic in $NEEDED_KAFKA_TOPICS; do    fi  done
```


### Enable citext extension


Sentry makes use of case-insensitive columns in postgres, which is enabled by the citext extension. Luckily, this extension is available in RDS, so you only have to activate it by querying the database as follows:


```text
CREATE EXTENSION citext;
```


### Clickhouse volume setup


Clickhouse stores event info, so to avoid wiping the data on each deployment or instance replacement, create an external storage volume and then fix the Elastic Beanstalk Availability Zone to match the same AZ as the volume to be able to attach it. Then, add the following .ebextension to manage the volume mounting:


```text
commands:    01clear-if-unmounted:      command: if ! mount | grep /var/lib/docker/volumes/sentry-clickhouse > /dev/null; then rm -rf /var/lib/docker/volumes/sentry-clickhouse; fi    02attach-volume:      command: aws ec2 attach-volume --region us-east-1 --volume-id $VOLUME_ID --instance-id $(curl -s http://169.254.169.254/latest/meta-data/instance-id) --device /dev/sdh      ignoreErrors: true    03wait:      command: sleep 10    04trymount:      command: |        mkdir -p /var/lib/docker/volumes/sentry-clickhouse        mount /dev/sdh /var/lib/docker/volumes/sentry-clickhouse        ignoreErrors: true    05format-if-not-already:      command: if find /var/lib/docker/volumes/sentry-clickhouse/ -maxdepth 0 -empty | read v; then mkfs -t ext3 /dev/sdh; fi
```


Beware of replacing the` $VOLUME_ID` placeholder with the proper volume id (it’ll be something like` vol-XXXXXX` ).


## Web container environment variables


We added Sentry config files to version control and made changes in them to use environment variables for sensitive parameters instead of hardcoded values. The process’s a little tricky but works flawlessly.


### Remove the following entries from .gitignore


```text
-sentry/sentry.conf.py  -sentry/config.yml
```


### Changes to use environment variables


Add os import to get env vars in sentry/sentry.config.py:


```text
from sentry.conf.server import *  # NOQA  + import os
```


And load environment variables for DB connection:


```text
-         "ENGINE": "sentry.db.postgres",  -         "NAME": "postgres",  -         "USER": "postgres",  -         "PASSWORD": "",  -         "HOST": "postgres",  -         "PORT": "",  +         "ENGINE": os.environ.get('DB_ENGINE'),  +         "NAME": os.environ.get('DB_NAME'),  +         "USER": os.environ.get('DB_USER'),  +         "PASSWORD": os.environ.get('DB_PASSWORD'),  +         "HOST": os.environ.get('DB_HOST'),  +         "PORT": os.environ.get('DB_PORT'),
```


Change` SECRET_KEY` auto-generation and hardcoding to use an environment variable instead. In install.sh:


```text
# Comment this to avoid storing secret keys in versioned config   # if grep -xq "system.secret-key: '!!changeme!!'" $SENTRY_CONFIG_YML ; then  #   echo ""  #   echo "Generating secret key..."  #   # This is to escape the secret key to be used in sed below  #   # Note the need to set LC_ALL=C due to BSD tr and sed always trying to decode  #   # whatever is passed to them. Kudos to https://stackoverflow.com/a/23584470/90297  #   SECRET_KEY=$(export LC_ALL=C; head /dev/urandom | tr -dc "a-z0-9@#%^&*(-_=+)" | head -c 50 | sed -e 's/[\/&]/\\&/g')  #   sed -i -e 's/^system.secret-key:.*$/system.secret-key: '"'$SECRET_KEY'"'/' $SENTRY_CONFIG_YML  #   echo "Secret key written to $SENTRY_CONFIG_YML"  # fi   # set secret key from envvar in config.yml  sed -i -e 's,^system.secret-key:.*$,system.secret-key: '"'$SECRET_KEY'"',' sentry/config.yml  echo "Secret key written to $SENTRY_CONFIG_YML"
```


### Add your variables to the Beanstalk environment in AWS


```text
# database connection  DB_HOST  DB_NAME  DB_USER  DB_PASSWORD  DB_PORT  DB_ENGINE   # Sentry secret key  SECRET_KEY   # Sentry URL, needs to start with http:// or https://  ROOT_URL   # For email alerts  MAIL_FROM  MAIL_PORT  MAIL_HOST  MAIL_USERNAME  MAIL_PASSWORD  MAIL_USE_TLS
```


### Elastic Beanstalk hooks


Add the following file in` .platform/confighooks/prebuild/` and` .platform/hooks/prebuild/` , to create a new` .env_eb` file during the deployment. This is needed to copy the Elastic Beanstalk environment variables to the staging dir, where docker-compose can pick it up.


```text
#!/bin/bash  cp /opt/elasticbeanstalk/deployment/env.list .env_eb
```


### Web container env_file


Then, add this as the env file for the web container in the docker-compose.yml file:


```text
environment:      SENTRY_CONF: '/etc/sentry'      SNUBA: 'http://snuba-api:1218'    env_file:      - .env_eb
```


**Note: This is a workaround since Sentry has its own .env file which is used by install.sh, and Elastic Beanstalk won’t generate the .env file automatically when this filename already exists.**


## Set prebuild hook to run the install script


Execute the install script in the previously created hook at` .platform/hooks/prebuild/` :


```text
#!/bin/bash  cp /opt/elasticbeanstalk/deployment/env.list .env_eb  # Run install of Sentry LTS release without user prompt  SENTRY_IMAGE=getsentry/sentry:20.11.1 ./install.sh --no-user-prompt  wait
```


## Elastic Beanstalk health check configuration


Set` /_health/` as the health check path for your Elastic Beanstalk environment in the AWS console and add the following file as a .ebextension to allow delayed health checks:


```text
Resources:    AWSEBAutoScalingGroup:      Type: "AWS::AutoScaling::AutoScalingGroup"      Properties:        HealthCheckType: "ELB"        HealthCheckGracePeriod: "600"
```


## Additional tweak for Kafka


Increase Kafka socket timeout to avoid health check issues:


```text
-     "socket.timeout.ms": 1000,  +     "socket.timeout.ms": 10000,
```


## Deployment


Make sure to have Elastic Beanstalk CLI installed in your machine, then initialize the Beanstalk environment locally using` eb init` , and use` eb deploy` command to deploy the application.


## The end


Press enter or click to view image in full size


Now that you’ve installed self-hosted Sentry it’s time to set up integrations with your favorite apps and link your applications to start reporting errors. We hope to get back to you soon to share our experience and tips for making the best use of it. But for now, we just want to thank you for sticking to the end. If you like this blog, please do show your appreciation by giving thumbs-ups and sharing it!
