---
schema_version: "1.0.0"
document_id: "f9e1db65c8e3338c2b2913c68d48dd791811d5f16354214b3765c08588709826"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/what-does-oomkilled-mean/"
published_at: "2025-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:8d75e6519d4956a3333d7f00b5f484405dfa25fc318428a1fa8d4daa2d87b36a"
---

# What does OOMKilled mean and how do I prevent it?

## What is a Kubernetes OOMKilled Error - And Why Does it Matter?


When creating production-level applications, enterprises want to ensure the high availability of services. This often results in a lengthy development process that requires extensive testing for the applications or a new release. This involves testing the behavior of the application under load, measuring the performance metrics, and accounting for the resource consumption. All this is done to ensure that the application does not behave unexpectedly when being used by clients. Applications being deployed to production, like any piece of code, require set resource allocations to work smoothly. These CPU, storage, and memory resources, often referred to as the total memory or total resources, are limited and come at a cost to the company. This is especially true when there are multiple instances of applications running, and the optimal amount of resources like storage and CPU memory should be readily available. This balanced demand on resources requires optimizing memory usage, a balancing act between providing the resources in the form of hardware (such as adding more memory) and keeping the cost to a minimum. There is a need to keep a certain amount of resources available so the application can be scaled as dictated by the usage and load without affecting the availability of the service. When dealing with memory and CPU utilization, this additional buffer of resources is known as *headroom* . For applications deployed with[Kubernetes](https://kubernetes.io/) running as pods, the headroom can be tracked using metrics servers, and these pods can be scaled automatically. If the headroom runs out, the Kubernetes will start killing pods and returning a pod OOMKilled error. This is often referred to as an exit code referred to as “exit code 137”, a critical memory issue code. This error is a red flag that indicates memory requests and limits are being reached within the Kubernetes scheduler and the memory manager. When OOM errors happen, things grind to a halt - very quickly. When this error occurs, you’ll need to provide more resources to get your application working again, which, on a production scale, will take time and money and might result in downtime. To avoid this, it’s important to have an accurate estimate of the headroom that will be needed before you deploy the application to production. In this article, you’ll learn about the importance of headroom and how to accurately estimate how much you need. You will be provided with an example scenario with code samples and configurations that you can run on your local machine using[minikube](https://minikube.sigs.k8s.io/docs) and[kubectl](https://kubernetes.io/docs/reference/kubectl) to get a better look at how it all works.


## Importance of Testing Headroom


Headroom consists mainly of memory and CPU utilization and indicates how much memory and process power you have to utilize before running into memory constraints. In Kubernetes, the CPU utilization is measured in *m* , millicores, and the memory utilization is measured in *MiB* , mebibytes. For Kubernetes clusters running on-premises, there’s a hard CPU and memory limit to how much headroom can be made available. The amount of computing power required by pods for processing tasks is based on the operation being performed and, as such, is dynamic. When a pod is faced with more complex tasks, it will necessarily consume more resources, increasing the CPU and memory utilization. Setting your allocated pod memory amount limit too low can stop processes from doing what they need to do, but overly permissive or misconfigured memory limits can have drastic impacts on the health of your system. To deal with the increasing load of complex tasks and ensure availability, pods can scale horizontally when set thresholds of memory requests are reached, which results in more pods being spun up through a process called horizontal pod autoscaling (HPA). When the hardware limits are exhausted, the pods start throwing an OOMKilled error. This can happen if there’s a spike in traffic load or because of buggy logic involving infinite loops and a memory leak. Getting an OOMKilled error means the pod cannot process requests, as the unbounded resource consumption is requesting excessive memory allocation or processor capacity. In short, you’ve made a memory request that is above the allocated memory limit. If, during peak traffic, multiple pods show this error, multiple services will be affected, which will negatively affect the availability of the service and the customer experience for people using the service. Additionally, when pods can’t process data requests, you’re losing money on two fronts: in addition to the potential loss of service and customer frustration, you’re also still being charged for the infrastructure being used but not generating any profit. The OOMKilled error is often seen when the pod experiences a load or number of requests that it hasn’t previously experienced, so you’re not aware that there aren’t sufficient resources in place. In load testing, the load can be generated to simulate several times the usual amount of traffic, allowing the behavior of the pod under stress to be monitored. This is how headroom is measured in real-life scenarios. During load testing, many unexpected situations, such as buggy logic, memory leaks, dependency-based errors, and concurrency inconsistencies, come to light. Despite the automation provided by Kubernetes, human intervention is sometimes needed due to the dynamic nature of cloud-native applications. Since systems can function on the same node, across different nodes, or in a combination of setups, automatically tracking memory related issues or CPU usage can be a bit difficult, requiring human intervention. To limit this intervention, it’s important to analyze the application’s key metrics and have an informed estimation of the amount of headroom required. The collection of metrics can be a challenge and is often managed with specialized tools like[Prometheus](https://prometheus.io/) ,[Jaeger](https://jaegertracing.io/) , and[Grafana](https://grafana.com/) . These need to be installed inside the running node and will export the metrics in an organized manner to the desired backend. Many enterprises set up a data lake to more easily analyze the metrics and derive conclusions from the data. Some important metrics to monitor include CPU and memory usage (headroom), network traffic, pod status, API server latency, and crash loop backoffs. In addition to the default metrics, custom metrics can also be enabled on the application end to measure the errors and unexpected behaviors. Enterprises want to ensure very high availability, and if a pod goes down due to an OOMKilled status error, availability is affected. If timely action is not taken, this can result in the whole service coming down. It’s less expensive to delay the development process by conducting proper testing and analysis than it is to face a service outage in production. In the following section, you’ll look at an example scenario where a deployed pod will undergo a load test with a mock client and go down due to an OOMKilled error.


## Prevention of Pod OOMKilled


In this section, you’ll see how an OOMKilled scenario can be simulated on your local Kubernetes cluster. This will be followed by looking at a production-ready scenario and setting up monitoring for your application.


### Basic OOMKilled Scenario


To demonstrate the OOMKilled scenario, you’ll use[minikube](https://minikube.sigs.k8s.io/docs/start/) to spin up a local Kubernetes cluster. kubectl has been configured to point to the` default` namespace:


console@bash:~$ minikube start


To follow along with the following section, you’ll need to have your kubectl command configured to point to the Kubernetes cluster and namespace of your choice. To get the example deployment up and running, you need to have the following YAML file. Here, the deployment` test-webapp` is created with one replica and defined memory limits. The resource has only been provisioned with 5 Mi memory and 10 m CPU, and it can only scale to 10 Mi memory and 20 m CPU utilization. This is called under provisioning: not giving the application enough resources to operate. Here’s the code:


apiVersion: apps/v1 kind: Deployment metadata: name: test-webapp spec: replicas: 1 selector: matchLabels: run: test-webapp template: metadata: labels: run: test-webapp spec: containers: - name: test-webapp image: registry.k8s.io/hpa-example ports: - containerPort: 80 resources: limits: memory: “10Mi” cpu: 25m requests: memory: “5Mi” cpu: 10m


Run the command:


kubectl apply -f webapp-deployment.yaml


This command will apply the deployment with the name` test-webapp` . Now, to access this application, a service needs to be set up. To create a service of type` LoadBalancer` , run the following command:


```text
kubectl   expose   deployment   test-webapp   --type=LoadBalancer   --name=test-service
```


To follow along, open a new terminal window and run the following command:


minikube tunnel


This command simulates a load balancer. Load balancers are used to distribute incoming traffic across the instances of your application. Do not close the current terminal. You can check the status of your cluster in the new terminal window:


kubectl get all


NAME READY STATUS RESTARTS AGE pod/test-webapp-5d5494d5b-szzl6 1/1 Running 0 87s NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE service/kubernetes ClusterIP 10.96.0.1 443/TCP 5m57s service/test-service LoadBalancer 10.98.228.181 127.0.0.1 80/TCP 5m20s NAME READY UP-TO-DATE AVAILABLE AGE deployment.apps/test-webapp 1/1 1 1 87s NAME DESIRED CURRENT READY AGE replicaset.apps/test-webapp-5d5494d5b 1 1 1 87s


The application is deployed and can be accessed at` http://127.0.0.1/` . The load balancer exposes the running pod to HTTP requests. To test the` OOMKilled` scenario, the pod has been provisioned with just enough resources to run. However, when an HTTP request is sent to the pod, it has been configured to run a CPU-intensive task. You can simulate a client by making HTTP requests to the service by running the load test, which will show how the pod will behave under heavy traffic. In a new terminal window, run the following command:


```text
while   sleep   0.01  ;   do   wget   -q   -O-   http://127.0.0.1/  ;   done
```


This will make a hundred HTTP requests per second to the localhost. This is more than enough to bring the pod down, as its resource allocation is very limited. Now check the status of the` test-webapp` deployment using the following command:


kubectl get deployment test-webapp


```text
NAME            READY     UP-TO-DATE     AVAILABLE     AGE
test-webapp     0/1       1              0             38m
```


To look at the exact error, run the following command:


```text
kubectl   describe   nodes
```


This returns the following:


```text
Warning   OOMKilling   Memory   cgroup   out   of   memory
```


So, the pod goes down, and the availability of your application is affected. This is why provisioning an appropriate amount of resources is important. In this example scenario, you saw very basic load testing and client mocking.


### Production-Ready Scenario


In the earlier scenario, you made multiple requests to the running application. However, production-ready applications aren’t this simple and have multiple endpoints. Tools are used to record snapshots of the actual traffic to the application to load test these applications, and these are then replayed when a load test is performed. This allows you to track the application’s behavior under historical customer loads before the application sees production load. This robust testing ensures confidence in the application’s abilities and offers assurance that edge-case scenarios can be found and corrected before deployment. The first step of load testing is setting up an environment and Kubernetes cluster with set rules in place. Next, observability tools need to be set up that can have logging and monitoring in place to check if something goes wrong. Some enterprises use a UI like[Argo](https://argoproj.github.io/) to help them keep track of all the containers and pods more easily. Tools that have monitoring, mocking, and insightful dashboards in place help draw more accurate conclusions from load-testing data. After completing the load tests, the final step is the analysis of the data. There are plenty of[load-testing tools](https://speedscale.com/kubernetes-load-testing) out there, but most of them require significant manual setup, monitoring, and analysis to be done on the user’s end. It’s more effective to use an advanced load-testing tool like[Speedscale](https://speedscale.com/) . Speedscale automates the mock servers and backend creation in a matter of seconds, allowing you to run tests quickly and efficiently. It records the actual traffic on your website and can clone this load, then replay it all on demand during load tests, easily generating test loads that are multiple times the volume of your normal peak-load traffic. This is a huge plus for businesses that have unpredictable traffic and need to be able to scale rapidly. Additionally, Speedscale is an integrated testing framework with many more features that will help you gain new insights. Running applications often make external requests to third-party tools, such as databases, APIs, and streaming services, which are expensive and charged on a per-call basis. During load testing, it’s better not to actually make these external calls, as it can both slow things down and add cost. To avoid this, you can use mocking tools, either embedded in your code or externally, that replicate external calls to third-party tools. This avoids the need to provision an entire end-to-end setup with the instances of third-party databases or APIs. Speedscale has built-in mocking, which learns to simulate third-party calls using the traffic from the production environment. Thanks to a built-in metrics server and dashboard, a lot of application data can be captured, and automated reports can be generated, helping you better understand the application’s behavior. This can help you set appropriate memory limits and ensure the prevention of inefficient resource utilization.


### Monitor Memory Usage with Prometheus


You can add external monitoring with external tools by attaching them to your Kubernetes cluster. These advanced tools monitor critical Kubernetes and application metrics in a distributed environment and are highly reliable, giving you both resource usage data as well as helping to ease node memory pressure by ensuring each node’s memory requirements are met effectively. Take a look at deploying Prometheus to the cluster using[Helm](https://helm.sh/) charts. Run the following command to install Helm on your local cluster:


curl -fsSL -o get_helm.sh[https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3](https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3) && chmod 700 get_helm.sh && ./get_helm.sh


This fetches the Helm agent and installs the dependencies. Helm can be seen as a package manager for Kubernetes, similar to` apt-get` in Linux. To start, you need to add the repository you want to the local Helm charts:


```text
helm   repo   add   prometheus-community   https://prometheus-community.github.io/helm-charts
```


Now, with minikube running, you can simply install the community version of Prometheus:


helm install prometheus prometheus-community/prometheus


This adds the required Prometheus resources to your cluster. To access the Prometheus server using the service, you should run the following command:


```text
kubectl   expose   service   prometheus-server   --type=NodePort   --target-port=9090   --name=metric-server
```


Run the following command to open the tab with the Prometheus server:


minikube service metric-server


The example Prometheus targets look like this: Prometheus custom metrics need to be set up in the application using the[Prometheus SDK](https://prometheus.io/docs/guides/go-application) . When these metrics are exposed to an HTTP endpoint, they are scraped by the Prometheus server and displayed on the Prometheus dashboard. Prometheus and Grafana are often[used together](https://grafana.com/docs/grafana/latest/getting-started/getting-started-prometheus) to draw insights from metrics data. Prometheus is a highly reliable distributed metric collection tool, and Grafana is a reliable backend to collect, query, and present the data. These profiling tools can help you ensure sufficient memory for operation while ensuring minimal memory costs. Overall, this will ensure that you don’t have too much memory while ensuring you have more free memory available for actual work - a delicate balancing act that requires higher visibility throughout the application code, the orchestration across multiple containers, and the actual production utilization data and pod logs.


## Analysis and Decision Making


Now that you have the data about the behavior of the pod under load, you have to leverage this data and incorporate it into the actual production environment. When this data is limited, simple graphs help to analyze and create alerts and limits manually. However, when abundant data is available, it can be sent to a backend for collection and recording purposes. Automated analytics can be performed to derive insights about the resource provisioning required. For businesses that can accurately estimate the peak traffic for their applications, the headroom should be at least thirty percent more than the maximum traffic. For newer businesses that can’t estimate these numbers or businesses that have incredibly variable traffic, keeping the headroom at one hundred to two hundred percent of the expected traffic is recommended. The actual resource limits set also depend on the ability of your business to acquire the resources. Overprovisioning for a short period of time can ensure availability, and with time, analysis of production data will allow you to provision more accurately. Speedscale can also help diagnose errors and perform root cause analysis for traffic data generating errors, which makes it the ideal tool for analysis and decision-making.


## Conclusion


In this article, you learned how to prevent OOMKilled errors when provisioning Kubernetes resources. You saw a practical scenario where a lack of resources could bring the service down, allowing significant memory consumption to be a hard limit, introducing memory pressure, and breaking the Kubernetes memory management approach. You also saw the importance of headroom, the available memory in a system, which allows you to sidestep insufficient memory issues and ensure that an OOMKilled Kubernetes error can be proactively managed through effective resource usage and memory management processes. The setting of load testing, monitoring, and mocks was also discussed in the production scenario. Kubernetes testing can be a black box, but developers and operators need to be aware of what’s going on inside it. This is where[Speedscale](https://speedscale.com/) steps in, enabling you to automate the stress test scenarios without writing time-consuming scripts.
