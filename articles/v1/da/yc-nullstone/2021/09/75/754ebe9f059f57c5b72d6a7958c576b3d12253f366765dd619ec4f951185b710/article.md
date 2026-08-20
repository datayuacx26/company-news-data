---
schema_version: "1.0.0"
document_id: "754ebe9f059f57c5b72d6a7958c576b3d12253f366765dd619ec4f951185b710"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/how-to-dockerized-laravel-that-just-works"
published_at: "2021-09-01T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:aaad22ea0f01aec7510788be87acae52768343f54ba0018e89f624e4b3016752"
---

# How-To: Dockerized Laravel that just works

## Introduction


Unfortunately, many guides explain *just enough* to get your Laravel app working in Docker, but fall short in a few areas.


1. **The resulting image is large.** This makes for slow local setup, especially if you are working at a coffee shop. This large image also makes for a frustrating experience if you are deploying a critical fix to production and it is taking forever to pull the image.
2. **The container needs restarted locally.** Every time you make a change to your Laravel application, you have to rebuild/restart your container to see your changes. When we face this, we end up running Laravel on our local machine and discarding Docker.
3. **The image is configured for production use.** When configured *only* for production use, the docker image is difficult to debug or diagnose. We end up running Laravel locally and use Docker for production. Or we create a separate \`local.Dockerfile\` that we have to maintain separately.


## Laravel that just works, everywhere


This guide gets you started with docker on a default Laravel app that *just works* locally and in production with the following:
- Server is optimized for \[php-fpm\](https://php-fpm.org/).
- Static assets and files are automatically added using \`ONBUILD\` during your docker build process.
- When making code changes, no need to rebuild/restart your container.
- Logs are emitted to stdout/stderr.
- The resulting image is relatively small (~37mb).
- Preconfigured to attach \[nginx\](https://www.nginx.com/) sidecar container. See below.


This guide assumes the default setup uses a mysql database, but also contains details on how to add other dependencies and databases (e.g. postgres, redis, etc.).


## Quickstart


To get started, you will need to add 3 files to our repository.
Create \`Dockerfile\` in your repository root. This is used to build your local and production docker image.
If you're curious how the base image is configured, visit \[https://github.com/nullstone-io/docker-laravel\](https://github.com/nullstone-io/docker-laravel).


\`\`\`docker


# syntax=docker/dockerfile:1
FROM nullstone/laravel


# Copy code, install dependencies
COPY --chown=nobody:nobody . .
RUN composer install --optimize-autoloader --no-dev


\`\`\`


Create \`.dockerignore\` in your repository root. This will prevent docker from adding sensitive files (e.g. \`env\`) or PHP packages that are specific to your local system. Don't worry, your vendor directory inside the docker container will be cached.


\`\`\`docker
.*
vendor/


\`\`\`


Create \`docker-compose.yml\` in your repository root. This stands up a docker container running php containing your laravel app, an nginx container for static assets, and a mysql database container.


\`\`\`docker
version: "3.8"
services:
nginx:
image: nginx:stable-alpine
volumes:
- app-public:/app/public
- nginx-confd:/etc/nginx/conf.d
- nginx-templates:/etc/nginx/templates:ro
ports:
- "3000:80"
environment:
- WEBAPP_ADDR=app:9000
depends_on:
- app
app:
image: nullstone/laravel:local
volumes:
- vendor:/app/vendor
- .:/app
- app-public:/app/public
- nginx-confd:/etc/nginx/conf.d
- nginx-templates:/etc/nginx/templates
environment:
- APP_ENV=local
- APP_KEY=${APP_KEY}
- MYSQL_URL=mysql://acme:acme@db:3306/acme
depends_on:
- db
db:
image: mysql:8.0
ports:
- "3306:3306"
environment:
MYSQL_ROOT_PASSWORD: acme
MYSQL_DATABASE: acme
MYSQL_USER: acme
MYSQL_PASSWORD: acme
healthcheck:
test: \["CMD", "mysqladmin", "ping", "-pacme"\]
retries: 3
timeout: 5s
volumes:
vendor:
app-public:
nginx-confd:
nginx-templates:
\`\`\`


Now, let's launch our Laravel app.


\`\`\`sh
$ docker compose up
\`\`\`


Once your container is launched, the application should be reachable from \[https://localhost:3000\](https://localhost:3000).


## How to extend and configure


If your app did not launch properly, you may need to extend or configure.
Below is a set of common ways to configure and extend Laravel.


### Port 3000 is already in use


By default, this setup uses port 3000, but only by convention.
If you wish to change, refer to line 11 of \`docker-compose.yml\`.
Change \`3000\` in the line \`- "3000:80"\` to an unused port on your system.


### Postgresql instead of mysql


By default, the database is configured for mysql.
If you would like to swap for postgres, you will need swap out the \`db\` service and reconfigure Laravel to use \`pgsql\`.
Swap out the \`db\` service in the \`services:\` section of \`docker-compose.yml\`.


\`\`\`docker
services:
...
db:
image: postgres
ports:
- 5432:5432
environment:
- POSTGRES_USER=acme
- POSTGRES_PASSWORD=acme
- POSTGRES_DB=acme


\`\`\`


Reconfigure Laravel for \`pgsql\`. Make sure to remove the existing \`DATABASE_URL\` environment variable.


\`\`\`docker
services:
...
app:
...
environment:
...
- DB_CONNECTION=pgsql
- DATABASE_URL=postgres://acme:acme@db:5342/acme?sslmode=disable
\`\`\`


### Redis for caching


By default, caching is done using the file system.


If you would like to add a redis cache layer, then you will need to add redis to your docker-compose and configure Laravel to use redis inside the docker-compose. Then, you will need to update your \`Dockerfile\` with the appropriate dependencies.


Add \`redis\` service under the \`services:\` section in \`docker-compose.yml\`.


\`\`\`docker
services:
...
redis:
image: redis:6-alpine
ports:
- "6379:6379"


\`\`\`


Configure Laravel to use redis in the existing \`app\` service under \`services:\` section in \`docker-compose.yml\`.


\`\`\`docker
services:
app:
...
environment:
...
- CACHE_DRIVER=redis
- REDIS_URL=tcp://redis:6379/0


\`\`\`


Install \`php-redis\` extension to your Dockerfile. Warning: This was left out of the base image because it added ~100mb to the image size.


\`\`\`docker
FROM nullstone/laravel
RUN apk add --no-cache pcre-dev $PHPIZE_DEPS \\
&& pecl install redis \\
&& rm -rf /tmp/pear \\
&& docker-php-ext-enable redis.so
\`\`\` **‍**


## What's next


Hopefully you have a fully functioning Laravel app running in docker on your local machine now. If you still need to configure the OS, you will need to use *\`apk\`* (the alpine package manager) instead of *\`apt\`* , *\`apt-get\`* , or *\`yum\`* . As you configure your app in different environments, make sure to follow docker best-practices by using environment variables. This will allow you to easily configure regardless of how your app is launched into production.


If you are interested in launching your newly-dockerized app to a cloud provider, log in at \[https://app.nullstone.io\](https://app.nullstone.io) and we will launch a secure container app on your cloud provider. We have a reference guide for Laravel in our docs at \[https://docs.nullstone.io/frameworks/php/laravel.html\](https://docs.nullstone.io/frameworks/php/laravel.html).## Run Laravel anywhereDocker is rapidly becoming the standard for running server applications. One of the key benefits of Docker is portability: the ability to run applications identically regardless of the underlying platform or system. This is especially helpful when running Laravel applications that require installation and configuration of OS-level packages.However, configuring a docker image is a tedious, and many times, frustrating endeavor. ## Laravel is difficult to configure on Docker
