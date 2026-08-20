---
schema_version: "1.0.0"
document_id: "c39ae912d0024117f9c40604704fa154e271a60ffa263728d08e04c5c0ceead4"
company_key: "yc-skyhook"
company: "Skyhook"
source_id: "yc-skyhook-news-import-0a5e972b70e5"
canonical_url: "https://www.skyhook.io/blog/laravel-to-kubernetes-simplified-with-skyhook"
published_at: "2025-11-01T00:00:00+00:00"
first_seen_at: "2026-07-26T00:39:47.906327+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:90a8007108b8fa41efc594809697482801e00feb5fbce9d711a3bb90e788226e"
---

# Laravel to Kubernetes, Simplified with Skyhook

Laravel is a PHP heavyweight, but running it in production usually means stitching together many moving parts: runtime, scaling, security, TLS, observability, and secrets. Kubernetes solves the runtime puzzle but introduces a new learning curve: container images, manifests, deployments, scaling, service discovery, and secrets management.


Below are two ways to ship Laravel to Kubernetes:


- The manual route: containerize the app, write YAML, own every knob.
- The Skyhook route: let Skyhook automate Day 1 bootstrap and handle Day 2 operations for you.


## What you will build


- Web app: Nginx with PHP-FPM serving Laravel
- Queue workers: Horizon deployment
- Scheduler: CronJob running php artisan schedule:run every minute
- Database migrations: pre-deploy Job
- Ingress with TLS via cert-manager
- Autoscaling via HPA, logs to stdout, metrics to Prometheus


## Prerequisites


Requirement Manual path Skyhook path


Kubernetes cluster You create or have one Create or connect from Skyhook


Docker Needed to build the image Needed, Skyhook can assist


Container registry Needed Built in or external (ECR, GCR, ACR, GHCR)


Git repository Needed Needed


## Option 1: Manual deployment


### 1) Containerize Laravel (PHP-FPM behind Nginx)


PHP-FPM is not an HTTP server. Expose Nginx on port 80 and keep PHP-FPM internal.


**Dockerfile (multi-stage)**


```text
FROM   composer:2   AS   vendor
WORKDIR   /app
COPY   composer.json composer.lock ./
RUN   composer install --no-dev --no-interaction --prefer-dist --no-scripts


FROM   node:22-alpine   AS   assets
WORKDIR   /app
COPY   package.json package-lock.json ./
RUN   npm ci
COPY   resources resources
RUN   npm run build


FROM   php:8.3-fpm-alpine   AS   app
RUN   apk add --no-cache nginx curl libzip-dev icu-dev \
&& docker-php-ext-install pdo_mysql zip intl opcache
WORKDIR   /var/www/html


# App files
COPY   . .
COPY   --from=vendor /app/vendor ./vendor
COPY   --from=assets /app/public/build ./public/build


# Cache config and routes for prod
RUN   php artisan config:cache && php artisan route:cache


# Nginx
COPY   ./infra/nginx.conf /etc/nginx/nginx.conf
EXPOSE   80
CMD   [  "sh"  ,   "-c"  ,   "php-fpm -D && nginx -g 'daemon off;'"  ]
```


**Minimal Nginx config**


```text
events   {}
http   {
server   {
listen   80  ;
root   /var/www/html/public;
index   index.php index.html;


location   /health   {   return   200   "ok"  ; }


location   /   {
try_files   $uri $uri/ /index.php?$query_string;
}


location   ~   \.php$   {
fastcgi_pass   127.0.0.1:9000;
include   fastcgi_params;
fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
}
}
}
```


Build and push:


```text
docker   build   -t   registry.example.com/laravel-demo:1.0   .
docker   push   registry.example.com/laravel-demo:1.0
```


### 2) Write Kubernetes manifests (abbreviated)


**Web Deployment and Service**


```text
apiVersion  :   apps/v1
kind  :   Deployment
metadata  : {   name  :   laravel-web  ,   labels  : {   app  :   laravel-web   } }
spec  :
replicas  :   2
selector  : {   matchLabels  : {   app  :   laravel-web   } }
template  :
metadata  : {   labels  : {   app  :   laravel-web   } }
spec  :
containers  :
-   name  :   web
image  :   registry.example.com/laravel-demo:1.0
ports  : [{   containerPort  :   80   }]
envFrom  : [{   secretRef  : {   name  :   laravel-env   } }]
readinessProbe  : {   httpGet  : {   path  :   /health  ,   port  :   80   },   periodSeconds  :   5   }
livenessProbe  : {   httpGet  : {   path  :   /health  ,   port  :   80   },   initialDelaySeconds  :   30   }
resources  :
requests  : {   cpu  :   "200m"  ,   memory  :   "256Mi"   }
limits  : {   cpu  :   "1"  ,   memory  :   "512Mi"   }
securityContext  : {   runAsNonRoot  :   true   }
---
apiVersion  :   v1
kind  :   Service
metadata  : {   name  :   laravel-web   }
spec  :
selector  : {   app  :   laravel-web   }
ports  : [{   port  :   80  ,   targetPort  :   80   }]
```


**Ingress with TLS via cert-manager**


```text
apiVersion  :   networking.k8s.io/v1
kind  :   Ingress
metadata  :
name  :   laravel
annotations  :
cert-manager.io/cluster-issuer  :   letsencrypt
spec  :
tls  :
-   hosts  : [  app.example.com  ]
secretName  :   laravel-tls
rules  :
-   host  :   app.example.com
http  :
paths  :
-   path  :   /
pathType  :   Prefix
backend  : {   service  : {   name  :   laravel-web  ,   port  : {   number  :   80   } } }
```


**Migrations as a pre-deploy Job (works well with Argo CD hooks)**


```text
apiVersion  :   batch/v1
kind  :   Job
metadata  :
name  :   laravel-migrate
annotations  :
argocd.argoproj.io/hook  :   PreSync
argocd.argoproj.io/hook-delete-policy  :   BeforeHookCreation
spec  :
template  :
spec  :
restartPolicy  :   Never
containers  :
-   name  :   migrate
image  :   registry.example.com/laravel-demo:1.0
command  : [  "php"  ,   "artisan"  ,   "migrate"  ,   "--force"  ]
envFrom  : [{   secretRef  : {   name  :   laravel-env   } }]
```


**Queue workers (Horizon)**


```text
apiVersion  :   apps/v1
kind  :   Deployment
metadata  : {   name  :   laravel-horizon  ,   labels  : {   app  :   laravel-horizon   } }
spec  :
replicas  :   2
selector  : {   matchLabels  : {   app  :   laravel-horizon   } }
template  :
metadata  : {   labels  : {   app  :   laravel-horizon   } }
spec  :
containers  :
-   name  :   horizon
image  :   registry.example.com/laravel-demo:1.0
command  : [  "php"  ,   "artisan"  ,   "horizon"  ]
envFrom  : [{   secretRef  : {   name  :   laravel-env   } }]
resources  :
requests  : {   cpu  :   "200m"  ,   memory  :   "256Mi"   }
limits  : {   cpu  :   "1"  ,   memory  :   "512Mi"   }
```


**Scheduler**


```text
apiVersion  :   batch/v1
kind  :   CronJob
metadata  : {   name  :   laravel-schedule   }
spec  :
schedule  :   "* * * * *"
jobTemplate  :
spec  :
template  :
spec  :
restartPolicy  :   Never
containers  :
-   name  :   schedule
image  :   registry.example.com/laravel-demo:1.0
command  : [  "php"  ,   "artisan"  ,   "schedule:run"  ]
envFrom  : [{   secretRef  : {   name  :   laravel-env   } }]
```


**External Secrets example**


```text
apiVersion  :   external-secrets.io/v1beta1
kind  :   ExternalSecret
metadata  : {   name  :   laravel-env   }
spec  :
refreshInterval  :   1h
secretStoreRef  : {   name  :   gcp-sm  ,   kind  :   ClusterSecretStore   }
target  : {   name  :   laravel-env   }
data  :
-   secretKey  :   APP_KEY
remoteRef  : {   key  :   projects/123/secrets/laravel-app-key   }
-   secretKey  :   DB_HOST
remoteRef  : {   key  :   projects/123/secrets/laravel-db-host   }
-   secretKey  :   REDIS_HOST
remoteRef  : {   key  :   projects/123/secrets/laravel-redis-host   }
```


**HPA for web**


```text
apiVersion  :   autoscaling/v2
kind  :   HorizontalPodAutoscaler
metadata  : {   name  :   laravel-web   }
spec  :
scaleTargetRef  : {   apiVersion  :   apps/v1  ,   kind  :   Deployment  ,   name  :   laravel-web   }
minReplicas  :   2
maxReplicas  :   10
metrics  :
-   type  :   Resource
resource  : {   name  :   cpu  ,   target  : {   type  :   Utilization  ,   averageUtilization  :   60   } }
```


**Key Laravel env tips**


```text
APP_ENV=production
APP_DEBUG=false
APP_KEY=base64:...     # generate once and store in your cloud secrets manager
SESSION_DRIVER=redis
CACHE_DRIVER=redis
QUEUE_CONNECTION=redis
LOG_CHANNEL=stderr
```


Apply your YAML:


```text
kubectl   apply   -f   k8s/
```


You will still need to install and wire cert-manager, External Secrets, Prometheus, Grafana, and logging, and you will own upgrades, rollouts, and drift.


## Option 2: Ship Laravel with Skyhook (recommended)


Skyhook supports a hybrid workflow. You keep your repos and clusters. Skyhook automates Day 1 bootstrap and Day 2 operations.


### Day 1: Bootstrap with Terraform (one click)


- Click Create Cluster in the Skyhook console and choose cloud and region.
- Skyhook commits a ready made Terraform module to your infra repo and triggers CI.
- Result: VPC, managed Kubernetes, node pools, OIDC, IAM roles, Argo CD, cert-manager, External Secrets, Grafana, Prometheus, and Loki.


You can also bring your own Terraform and clusters.


### Day 2: Deploy Laravel from the Skyhook UI


- Create a Service by pointing Skyhook to your Git repo.
- Build and push using your Dockerfile or Buildpacks. Skyhook handles registry credentials.
- Environment variables and secrets mapped through your cloud secrets manager via External Secrets.
- Add ons with a click such as cert-manager for automatic TLS, External Secrets for rotation, Redis or RabbitMQ charts for queues.
- Deploy which generates an Github Actions flow or an Argo CD Application, commits it to your apps repo, and rolls it out.
- Observe and scale a full open source observability setup with dashboards and autoscaling policies.


You don’t have to deal with YAML and kubectl apply, but they’ll always be available for you if you want to dig in yourself.


## Skyhook value recap


Benefit What it means for Laravel teams


Best practices out of the box Production networking, probes, HPA, log aggregation, and TLS configured for you


No lock in Manifests live in your Git repos and run on any CNCF conformant Kubernetes


Plug and play setup Cluster up and add ons installed in minutes, no scratch Helm or HCL


Empower developers Self service deploy, debug, and rollback without waiting on DevOps


Designed to scale Multi cluster support, RBAC, and policy controls that grow with you


## Conclusion


You can deploy Laravel on Kubernetes by hand, but teams usually rebuild the same pieces: web, workers, scheduler, migrations, TLS, secrets, and safe rollouts. Skyhook gives you those pieces out of the box, commits them to your Git repos, and lets you operate them from a single UI. Spin up a free Skyhook workspace, point it at your repo, and ship web, workers, scheduler, and migrations behind TLS in minutes.
