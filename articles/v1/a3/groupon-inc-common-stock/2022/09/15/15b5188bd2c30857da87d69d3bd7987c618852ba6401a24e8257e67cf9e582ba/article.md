---
schema_version: "1.0.0"
document_id: "15b5188bd2c30857da87d69d3bd7987c618852ba6401a24e8257e67cf9e582ba"
company_key: "groupon-inc-common-stock"
company: "Groupon Inc."
source_id: "groupon-inc-common-stock-rss-99044b78d036"
canonical_url: "https://medium.com/groupon-eng/loadbalancer-services-using-kubernetes-in-docker-kind-694b4207575d"
published_at: "2022-09-20T20:08:29+00:00"
first_seen_at: "2026-07-20T04:36:53.869647+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:9c5e4dc9831ef256d6d15bf0bb8b31b07e66b9cb9e5ce666d5079135d316c755"
---

# LoadBalancer Services using Kubernetes in Docker (kind)

# LoadBalancer Services using Kubernetes in Docker (kind)


[Owain Williams](https://medium.com/@hedfan?source=post_page---byline--694b4207575d---------------------------------------)


11 min read


·


Sep 20, 2022


--


Kubernetes is a very popular open-source container orchestration system for deploying, scaling, and updating your software, as well as automatically recovering from failures. If your code can be packaged into a container, e.g. via Docker, then it can run on Kubernetes, or k8s for short (which is sometimes pronounced ‘kates’ and refers to the 8 letters between the first and last letter). You can read more about it on the[Kubernetes Documentation](https://kubernetes.io/docs/home/) page.


Press enter or click to view image in full size


You can run a Kubernetes cluster in your own data centre as well as on many of the popular cloud providers, for example there’s the[Amazon Elastic Kubernetes Service (EKS)](https://aws.amazon.com/eks/) , the[Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/services/kubernetes-service/) , and the[Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine) to name a few. You can also create clusters that span your data centre and a cloud provider or even span multiple cloud providers. Here at Groupon, we have several Kubernetes clusters around the world hosted on both EKS and GKE.


This is great for production, or even as a staging cluster, or shared development environment. But what if you just want to try it out and see what all the hype is about before you spend money on a new cluster? The good news is there are several options, if you’ve installed[Docker](https://www.docker.com/) you can enable a single-node Kubernetes cluster simply by ticking the option in the Docker settings. However, you will be tied to the version of Kubernetes that ships with your version of Docker. Another option is[minikube](https://minikube.sigs.k8s.io/docs/start/) which allows you to run specific versions of Kubernetes, or even multiple clusters with different versions. However, you’ll still only be able to run single-node clusters.


A single-node cluster is probably fine, and could well be all you ever need. But you won’t be able to experiment with pods running on different nodes, and the various like-live scenarios that enable, for example using[hostPath](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath) volumes with different files on each node. Before you fire up a load of VMs and try installing[Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way) there is one more option,[kind](https://kind.sigs.k8s.io/) , which stands for Kubernetes in Docker and runs each cluster node inside a container, allowing you to create multi-node clusters by starting several containers. Pretty cool eh?


Press enter or click to view image in full size


## Installing kind


If you’re on a Mac and have[Homebrew](https://brew.sh/) , installing kind is as simple as` brew install kind` , and if you're on Windows and have[Chocolatey](https://chocolatey.org/) it's` choco install kind` . You can find other ways to install kind in the[Quick Start](https://kind.sigs.k8s.io/docs/user/quick-start/) guide, including via MacPorts, for Apple silicon, on Linux, in PowerShell, and from source.


Now you can create a cluster using` kind create cluster` (you'll need to have Docker installed and running) and when you're done delete it with` kind delete cluster` . You can specify the version of Kubernetes your kind cluster runs by setting the` --image` flag to one of the tags on the[releases page](https://github.com/kubernetes-sigs/kind/releases) , and run more than one cluster by giving them unique names using the` --name` flag. However, by default, it will only start a single node cluster. " *I thought the whole point of kind was multi-node clusters?* " I hear you ask!


## Multi Node Cluster


You’re right, but for that, we need to specify a configuration file. For example, to create a cluster with three nodes (one control-plane and two workers) create a file called` kind-config.yaml` with the following contents:


```text
# three node (two workers) cluster config  kind: Cluster  apiVersion: kind.x-k8s.io/v1alpha4  nodes:  - role: control-plane  - role: worker  - role: worker
```


Then create your cluster using the` --config` flag:


```text
kind create cluster --config kind-config.yaml
```


Once this command completes you can see the three nodes using the` kubectl get nodes` command. You can see that these nodes are running in containers in Docker using the` docker ps` command.


Press enter or click to view image in full size


## Adding Services


Let’s add some[pods](https://kubernetes.io/docs/concepts/workloads/pods/) and[services](https://kubernetes.io/docs/concepts/services-networking/service/) to check everything’s working. We’ll add two simple pods that use NGINX images, they’ll both use a[ConfigMap](https://kubernetes.io/docs/concepts/configuration/configmap/) that overrides the default` index.html` file, one returns the text` foo` and the other` bar` , along with two services that target these pods. Create a` foo-service.yaml` file with the following contents:


```text
kind: Pod  apiVersion: v1  metadata:    name: foo-app    labels:      app: foo      svc: foo-bar  spec:    containers:    - name: foo-app      image: nginx:1.22.0      volumeMounts:      - name: config        mountPath: /usr/share/nginx/html        readOnly: true    volumes:    - name: config      configMap:        name: foo-config  ---  apiVersion: v1  kind: ConfigMap  metadata:    name: foo-config  data:    index.html: |      foo  ---  kind: Service  apiVersion: v1  metadata:    name: foo-service  spec:    selector:      app: foo    ports:    - port: 80
```


And add this to your cluster:


```text
kubectl apply -f foo-service.yaml
```


Next, create a` bar-service.yaml` file with the following contents:


```text
kind: Pod  apiVersion: v1  metadata:    name: bar-app    labels:      app: bar      svc: foo-bar  spec:    containers:    - name: bar-app      image: nginx:1.22.0      volumeMounts:      - name: config        mountPath: /usr/share/nginx/html        readOnly: true    volumes:    - name: config      configMap:        name: bar-config  ---  apiVersion: v1  kind: ConfigMap  metadata:    name: bar-config  data:    index.html: |      bar  ---  kind: Service  apiVersion: v1  metadata:    name: bar-service  spec:    selector:      app: bar    ports:    - port: 80
```


And add this to your cluster:


```text
kubectl apply -f bar-service.yaml
```


Now let’s check they’ve been created, list your pods with` kubectl get pods` and your services with` kubectl get svc` (with each command you can use the` -o wide` flag to get more details) you should see output similar to the following:


```text
NAME      READY   STATUS    RESTARTS   AGE  bar-app   1/1     Running   0          29s  foo-app   1/1     Running   0          36s  NAME          TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE  bar-service   ClusterIP   10.96.61.222   <none>        80/TCP    41s  foo-service   ClusterIP   10.96.209.228  <none>        80/TCP    48s  kubernetes    ClusterIP   10.96.0.1      <none>        443/TCP   4m
```


You might be surprised to find when you try and send a request to the IP address for the` foo-service` or the` bar-service` it doesn’t work, for example by using` curl` followed by the appropriate IP address:


```text
curl 10.96.209.228
```


We can check the service is working by using` docker exec -it kind-control-plane bash` to run a bash shell inside the control-plane container, and from there` curl` the two IP addresses for our` foo-service` and` bar-service` services:


```text
docker exec -it kind-control-plane bash  root@kind-control-plane:/# curl 10.96.209.228  foo  root@kind-control-plane:/# curl 10.96.61.222  bar  root@kind-control-plane:/# exit  exit
```


Press enter or click to view image in full size


Looks like the services are running, so what’s going on?


## Ingress


You might have encountered the problem above if you’re running kind in Docker for Mac or Windows. This is because Docker is running your containers in a Linux VM and the container networks are not exposed to the host. If you’re running in Linux and didn’t have any problems using your services, skip on to the LoadBalancer section below.


Press enter or click to view image in full size


To get round this you can use the` extraPortMapping` config option along with an ingress controller when creating your cluster. More details on what's involved and the various options are available in the kind[Ingress](https://kind.sigs.k8s.io/docs/user/ingress/) guide. For this walkthrough, you're going to use an NGINX ingress controller. First, update the` kind-config.yaml` file to allow ingress on ports 80 and 443, and set up a custom node label to identify the control plane node as being ingress-ready:


```text
# three node cluster with an ingress-ready control-plane node  # and extra port mappings over 80/443 and 2 workers  kind: Cluster  apiVersion: kind.x-k8s.io/v1alpha4  nodes:  - role: control-plane    kubeadmConfigPatches:    - |      kind: InitConfiguration      nodeRegistration:        kubeletExtraArgs:          node-labels: "ingress-ready=true"    extraPortMappings:    - containerPort: 80      hostPort: 80      protocol: TCP    - containerPort: 443      hostPort: 443      protocol: TCP  - role: worker  - role: worker
```


Create your cluster with` kind create cluster --config kind-config.yaml` (if you're following along you'll need to either delete your cluster first using` kind delete cluster` or give your new cluster a unique name using the` --name` flag).


Now patch kind to forward the hostPorts to an NGINX ingress controller and schedule it to the control-plane custom labelled node:


```text
kubectl apply -f  https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```


You may need to wait a minute or two for the pods in the` ingress-nginx` namespace to be ready, so in the meantime re-add your pods and services using` kubectl apply -f foo-service.yaml`` kubectl apply -f bar-service.yaml` , and now create an ingress controller to handle traffic from your hostPorts and forward it to the relevant service. Create an` nginx-ingress.yaml` file with the following contents:


```text
apiVersion: networking.k8s.io/v1  kind: Ingress  metadata:    name: nginx-ingress  spec:    rules:    - http:        paths:        - pathType: Prefix          path: "/foo"          backend:            service:              name: foo-service              port:                number: 80        - pathType: Prefix          path: "/bar"          backend:            service:              name: bar-service              port:                number: 80
```


And add this to your cluster (if you get an error you may need to wait a minute or two for the pods in the` ingress-nginx` namespace to be ready before retrying):


```text
kubectl apply -f nginx-ingress.yaml
```


This will receive ingress from the host on ports 80 and 443, forward it to the Ingress controller which will use the path to route the request to the appropriate service:


Press enter or click to view image in full size


We can test this with` curl localhost/foo` or` curl localhost/bar` and you should see the following:


```text
<html>  <head><title>404 Not Found</title></head>  <body>  <center><h1>404 Not Found</h1></center>  <hr><center>nginx/1.22.0</center>  </body>  </html>
```


Brilliant! We’ve got a response. But hang on, why does it say` 404 Not Found` ?


## Rewrite Target


This is because the request is being passed on to the appropriate service verbatim, which means it contains` /foo` or` /bar` in the path, and NGINX doesn't have any resources to serve at those paths (it’s only got an` index.html` page at the root). So we need to rewrite the path using a rewrite annotation on the ingress rule so it's passed onto the target service without` /foo` or` /bar` in the path. You can read more about[Rewrite](https://kubernetes.github.io/ingress-nginx/examples/rewrite/) annotations on the Ingress NGINX Controller documentation page. Edit the` nginx-ingress.yaml` file to have the following contents (note that we're adding an` annotation` to the` metadata` as well as updating the two` path` sections to include a regex):


```text
apiVersion: networking.k8s.io/v1  kind: Ingress  metadata:    name: nginx-ingress    annotations:      nginx.ingress.kubernetes.io/rewrite-target: /$2  spec:    rules:    - http:        paths:        - pathType: Prefix          path: "/foo(/|$)(.*)"          backend:            service:              name: foo-service              port:                number: 80        - pathType: Prefix          path: "/bar(/|$)(.*)"          backend:            service:              name: bar-service              port:                number: 80
```


Re-apply this to your cluster:


```text
kubectl apply -f nginx-ingress.yaml
```


And re-test with` curl localhost/foo` or` curl localhost/bar` and you should see` foo` or` bar` as the response respectively. You’ll see the same if you append` index.html` to the end, e.g.` curl localhost/foo/index.html` which shows you how the target is being re-written, the backend service receives a request to` localhost/index.html` as it’s only the second capture group that’s being used in the path, the` (.*)` matching everything after` /foo` or` /bar` in the request.


## LoadBalancer Service


We added two labels to our pods, an` app` label of` foo` or` bar` that the` foo-service` and` bar-service` can use to locate their pods, and a` svc` label of` foo-bar` which we can use in a load balancer service to route traffic to one or other pod type randomly. To do this create a` foo-bar-lb.yaml` file with the following contents:


```text
kind: Service  apiVersion: v1  metadata:    name: foo-bar-service  spec:    type: LoadBalancer    selector:      svc: foo-bar    ports:    - port: 80
```


And add it to your cluster:


```text
kubectl apply -f foo-bar-lb.yaml
```


Check on the status of this new service:


```text
kubectl get svc  NAME              TYPE           CLUSTER-IP      EXTERNAL-IP   ...  bar-service       ClusterIP      10.96.67.95     <none>        ...  foo-bar-service   LoadBalancer   10.96.123.161   <pending>     ...  foo-service       ClusterIP      10.96.109.102   <none>        ...  kubernetes        ClusterIP      10.96.0.1       <none>        ...
```


Our service has got a cluster IP, but not an external IP. This is because LoadBalancer services are designed to use the load-balancer infrastructure your cloud provider offers. We’re not running in the cloud so we need to use the[MetalLB](https://metallb.universe.tf/) load-balancer implementation. You can read more about how to add this to kind in the[LoadBalancer](https://kind.sigs.k8s.io/docs/user/loadbalancer/) user guide. The process is as follows.


First, create the MetalLB namespace:


```text
kubectl apply -f  https://raw.githubusercontent.com/metallb/metallb/v0.12.1/manifests/namespace.yaml
```


Then apply the MetalLB manifest:


```text
kubectl apply -f  https://raw.githubusercontent.com/metallb/metallb/v0.12.1/manifests/metallb.yaml
```


Now wait for the pods to have a status of Running:


```text
kubectl get pods -n metallb-system --watch
```


Finally, we need to configure metallb to use an IP range from the network Docker has created for the` kind` namespace, we can find this using the following command:


```text
docker network inspect -f '{{.IPAM.Config}}' kind
```


The output will include a cidr such as 172.19.0.0/16, so we want our load-balancer services to be assigned an external IP address from this range, for example, to use 172.19.255.200 to 172.19.255.250 create a` metallb-configmap.yaml` file with the following contents (update the IP addresses to be within the range outputted by the previous command):


```text
apiVersion: v1  kind: ConfigMap  metadata:    namespace: metallb-system    name: config  data:    config: |      address-pools:      - name: default        protocol: layer2        addresses:        - 172.19.255.200-172.19.255.250
```


Add this to your cluster:


```text
kubectl apply -f metallb-configmap.yaml
```


Now when you get your services you should see the` foo-bar-service` has an external IP:


```text
kubectl get svc  NAME              TYPE           CLUSTER-IP      EXTERNAL-IP     ...  bar-service       ClusterIP      10.96.67.95     <none>          ...  foo-bar-service   LoadBalancer   10.96.123.161   172.19.255.200  ...  foo-service       ClusterIP      10.96.109.102   <none>          ...  kubernetes        ClusterIP      10.96.0.1       <none>          ...
```


If you’re on Linux, you should be able to` curl` to that external IP address:


```text
# should output foo and bar on separate lines  LB_IP=$(kubectl get svc/foo-bar-service -o=jsonpath='{.status.loadBalancer.ingress[0].ip}')  for _ in {1..10}; do    curl ${LB_IP}  done
```


However, on Mac and Windows we need to use the ingress controller again.


Press enter or click to view image in full size


Let’s add another rule to the` nginx-ingress.yaml` file taking the` /foo-bar` path and forwarding to our` foo-bar-service` :


```text
apiVersion: networking.k8s.io/v1  kind: Ingress  metadata:    name: nginx-ingress    annotations:      nginx.ingress.kubernetes.io/rewrite-target: /$2  spec:    rules:    - http:        paths:        - pathType: Prefix          path: "/foo(/|$)(.*)"          backend:            service:              name: foo-service              port:                number: 80        - pathType: Prefix          path: "/bar(/|$)(.*)"          backend:            service:              name: bar-service              port:                number: 80        - pathType: Prefix          path: "/foo-bar(/|$)(.*)"          backend:            service:              name: foo-bar-service              port:                number: 80
```


Re-apply this to your cluster:


```text
kubectl apply -f nginx-ingress.yaml
```


Now when you` curl localhost/foo-bar` you should see either` foo` or` bar` and the output will change randomly as your requests are load-balanced across your services. In` bash` you can do this with:


```text
for _ in {1..10}; do    curl localhost/foo-bar  done
```


And in PowerShell the same can be achieved with:


```text
for($i=0; $i -lt 10; $i++)  {      curl localhost/foo-bar  }
```


In either case, the output should be a variation on:


```text
bar  foo  foo  bar  bar  bar  foo  foo  foo  bar
```


## Summary


After all that you’ll have a multi-node kind cluster with` extraPortMappings` to forward requests from your host to an NGINX ingress controller which uses the path to send your request to the appropriate service, rewriting the target so it can recognise the request. Give yourself a pat on the back, this is pretty advanced stuff! Well done.


As an exercise to the reader, try forwarding to services listening on different ports (you’ll still need to` curl` to 80 or 443, but you can configure the NGINX ingress controller to send this to your service's port), see what happens when you include additional path segments in your request (for example` curl localhost/foo/bar` will rewrite your request to call the` foo-service` with the path` /bar` ), and explore making secure requests over port 443 (you'll need to create certificates and add these as secrets).
