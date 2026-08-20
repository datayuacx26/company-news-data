---
schema_version: "1.0.0"
document_id: "ba86939e109155c0d39387953ab4a0e452396c2de977b8a02a0c9d4c0ee8873f"
company_key: "yc-warpbuild"
company: "WarpBuild"
source_id: "yc-warpbuild-news-import-6421ae0a6624"
canonical_url: "https://warpbuild.com/blog/docker-mirror-setup"
published_at: "2024-04-17T00:00:00+00:00"
first_seen_at: "2026-07-22T19:23:06.881853+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:69c01a8cc17d6b88d60c71bc757bd6930e657cd11f22fe893d203893a11f4232"
---

# Docker registry mirror setup

You might have run into a docker rate limit issue when pulling docker images from DockerHub if you didn't sign in to DockerHub and were pulling the image anonymously. DockerHub has a rate limit of 100 pulls per 6 hours per IP address. For authenticated users, the limit is 200 pulls per 6 hours. However, this goes up to 5000 pulls per day with a paid plan. In many cases, this is not enough.


One way to work around these limits is to send requests to a registry mirror. The docker daemon can be configured to pull images from the mirror instead of going to DockerHub each time for the pull. Docker maintains an[image](https://hub.docker.com/_/registry) which can be used for this exact purpose.


Another way is to use public mirrors such as Google's` https://mirror.gcr.io` . However, this will not contain private registries and the coverage of all public Dockerhub images is not guaranteed either.


We'll focus on the first method, setting this up on AWS, the pros, cons and common pitfalls.


## Infra setup


- A Kubernetes cluster, EKS. This isn't a requirement for deploying registry but we prefer to manage it through a k8s cluster.
- Ingress nginx setup on the k8s cluster.
- Cert-manager setup on the k8s cluster.
- Cluster issuer for cert-manager setup called` letsencrypt-prod` or something else.
- An S3 bucket. This is used to store the images.


Note


You can set this up as just a container on AWS Fargate, AWS ECS, Google Cloud Run, or any other container runner. The data from the` s3` backend does not go through this container but is instead directly transferred from s3 to the docker process. This is beneficial as it avoids egress fees in many scenarios.


## Deploying


We'll deploy the registry using a[helm chart](https://artifacthub.io/packages/helm/twuni/docker-registry) . If you wish to use just the k8s yaml file. You should do a helm template and make the suggested changes on the generated k8s manifest.


```text
image  :
repository  :   mirror.gcr.io/registry
ingress  :
enabled  :   true
className  :   nginx
path  :   /
hosts  :
-   <mirror-url>
annotations  :
ingress.kubernetes.io/ssl-redirect  :   "true"
ingress.kubernetes.io/proxy-body-size  :   "0"
# You might need to change based on your issuer
cert-manager.io/cluster-issuer  :   letsencrypt-prod
kubernetes.io/tls-acme  :   "true"
nginx.ingress.kubernetes.io/proxy-body-size  :   "0"
nginx.ingress.kubernetes.io/ssl-redirect  :   "true"
labels  : {}
tls  :
-   secretName  :   registry-mirror-docker-registry-ingress
hosts  :
-   <mirror-url>
resources  :
limits  :
cpu  :   1
memory  :   1Gi
requests  :
cpu  :   0.5
memory  :   512Mi
storage  :   s3
# Secrets for S3 access and secret keys
# Use a secretRef with keys (accessKey, secretKey) for secrets stored outside
# the chart
s3  :
secretRef  :   "registry-mirror-aws-credentials"
```


Save the above file as values.yaml.


```text
apiVersion  :   v1
kind  :   Secret
metadata  :
name  :   registry-mirror-aws-credentials
namespace  :   mirror
type  :   Opaque
stringData  :
accessKey  :   <aws-access-key-id>
secretKey  :   <aws-secret-access-key>
```


Save the above Kubernetes secret as secret.yaml and replace the accessKey and secretKey. Since we are using` stringData` you don't need to encode your credentials to base64.


Make sure that the DNS mapping is correct, and make sure the URL is routed to your cluster. The processing of configuring the DNS will depend on what you are using to manage the mappings. If you are completely on AWS, it should be in Route53.


Make sure you have configured kubectl with your cluster context. In the case of AWS EKS just run,


```text
aws eks update-kubeconfig --name <cluster-name> --region <cluster-region>
```


This with make your cluster the default context.


We set up the secret first so that the registry has access to the s3 bucket.


```text
kubectl apply -f secret.yaml
```


If you wish to validate that the secret was added to the cluster run the following.


```text
kubectl get secret registry-mirror-aws-credentials -n mirror
```


This should give you info on the k8s secret.


Install the registry on your cluster with the following command


```text
helm repo add twuni https://helm.twun.io
helm install registry-mirror twuni/docker-registry \
--namespace mirror \
--version 2.2.3 \
--values values.yaml \
--create-namespace
```


This will set up a registry mirror deployment in the mirror namespace. If you need to make updates to the values, replace the` helm install` with` helm update` .


## Try out the mirror


Configure your docker daemon to use the registry mirror.


Add the following entry to your` /etc/docker/daemon.json` . If you don't have this file create a new one with only the entry below.


```text
{
"registry-mirrors"  : [  "<mirror-url>"  ]
}
```


Restart the docker daemon.


```text
# stop the docker daemon
sudo systemctl stop docker.service
# verify that the docker daemon is stopped, this would list the service as
# stopped
sudo systemctl status docker.service
# start the docker daemon
sudo systemctl start docker.service
```


Do a pull of an image say` ubuntu:22.04` .


```text
docker pull ubuntu:22.04
```


After this change, the docker daemon will query the mirror service if it has ubuntu:22.04. If it doesn't it will pull from` docker.io` . And the mirror service will silently pull ubuntu:22.04.


Remove the docker image for ubuntu:22.04 and re-pull.


```text
docker image rm ubuntu:22.04
docker pull ubuntu:22.04
```


This should pull the image from s3. You can check the registry mirror logs to verify this. You would see a bunch of 307 with layer sha codes. 307 is sent because of redirection to the pre-signed URL by the registry mirror. You can also verify that the s3 storage has these sha layers cached.


## Pros


- Faster downloads, download from s3 is very fast which means your images are downloaded at a boosted speed.
- Dockerhub rate limits are bypassed.


## Cons


- Infra management overhead. You'll need to pay for compute and S3.
- No direct lifecycle support for images. So no way to clean up stale images. So you end up having old images in your s3 bucket. You can configure the lifecycle at S3 but it's for all objects not just the stale ones.


## Common Pitfalls


### Using docker.io image for the registry mirror.


We had a bad deployment that was causing the Docker mirror to restart. The registry mirror pods became unreachable which means all the traffic was directed to DockerHub now. This quickly led to the Docker mirror container itself being throttled because we were using the[official registry image](https://hub.docker.com/_/registry) . We switched the image to pull from` gcr.io` instead to mitigate this.


### Surprising data transfer charges for S3


S3 is excellent when you want a cheap and simple way to save and restore data that can scale. It is also free if your transfers are within the same region which is the case for us. So it was surprising to see us incurring an unexpected huge amount of data transfer on our AWS bill. Our infra hadn't changed apart from the registry, and the registry sends a pre-signed URL which is so small that it can be neglected. Scouring through the AWS docs, we found that even though the bucket was in the same region it was still being transferred globally because our route tables didn't have a direct gateway to AWS S3 service. To configure it you need to set up a VPC gateway endpoint for S3, making the data transfers free, within the same region.


## Footnote


This is just one of the many optimizations that you'll get out of the box with WarpBuild runners to make your GitHub Actions CI workflows faster. You can try it out at[app.warpbuild.com](https://app.warpbuild.com/)


## References


Docker registry mirror image:[https://hub.docker.com/_/registry](https://hub.docker.com/_/registry)


Docker registry mirror source code:[https://github.com/distribution/distribution/tree/main/registry](https://github.com/distribution/distribution/tree/main/registry)


AWS S3 Pricing:[https://aws.amazon.com/s3/pricing/](https://aws.amazon.com/s3/pricing/)


AWS S3 Gateway Endpoint:[https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-s3.html](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-s3.html)
