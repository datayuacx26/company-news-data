---
schema_version: "1.0.0"
document_id: "4b943bdb0942877d934983732952fee6c4c535c613b4261510802d2f74ac1d69"
company_key: "doubleverify-holdings-inc-common-stock"
company: "DoubleVerify Holdings Inc."
source_id: "doubleverify-holdings-inc-common-stock-rss-f6469e95d005"
canonical_url: "https://medium.com/doubleverify-engineering/simplifying-model-serving-with-kubernetes-and-ray-inside-doubleverifys-ml-platform-78b33faa9e91"
published_at: "2026-03-25T14:28:09+00:00"
first_seen_at: "2026-07-20T23:17:33.321656+00:00"
fetched_at: "2026-07-28T22:17:51.657377+00:00"
content_hash: "sha256:4d0b925a5d2c927208de35f4a01f6806a3327293bd7c665ee859d10f395043bb"
---

# Simplifying Model Serving with Kubernetes and Ray: Inside DoubleVerify’s ML Platform

# **Simplifying Model Serving with Kubernetes and Ray: Inside DoubleVerify’s ML Platform**


[DV Engineering](https://medium.com/@dv-engineering?source=post_page---byline--78b33faa9e91---------------------------------------)


9 min read


·


Mar 25, 2026


--


*Written By:*[Aviad Shimoni](https://www.linkedin.com/in/aviad-shimoni-23301314b/)


## **The Vision Behind the Platform**


For more than a decade, DoubleVerify has harnessed AI and machine learning (ML) to power its portfolio of solutions — from fraud detection and context analysis to optimization and performance. Over the years, these technologies have become deeply embedded in how we deliver trust and transparency across digital advertising.


What changed three years ago was not *whether* we used ML, but *how* we scaled and operationalized it. The turning point came with a Slack message from our ML team lead:


*“Can you help us experiment with Ray?”*


As a curious DevOps engineer, I said yes — despite knowing very little about Ray at the time. That moment sparked a breakthrough in how we build, deploy, and scale ML at DV, reshaping the partnership between our DevOps and ML teams.


While DV had long applied ML in production, we were beginning to explore new frontiers such as video classification, object detection, and speech recognition. The challenge we faced wasn’t a lack of innovation from our ML engineers — it was the gap between model development and reliable deployment. Unlike full-stack engineers, ML engineers need stronger platform support to deploy at scale. At the same time, we needed a technology that could serve as a stable, intuitive foundation in a rapidly evolving AI ecosystem — one embraced by both the ML and open-source communities.


I could relate personally: during my college internship in Deep Learning, I had wrestled with exposing an API endpoint for a PyTorch-based video-classification and lip-reading model. I saw firsthand the friction ML engineers face once their models leave the lab.


It became clear that for our ML teams to thrive, we needed a scalable and stable ML platform — one that would empower engineers to deploy, manage, and scale their models easily. That quest led us to Ray, an open-source framework that integrates seamlessly across Keras, TensorFlow, and PyTorch. Ray provides the foundation for training, hyperparameter tuning, and model serving — all in a unified ecosystem.


Today, the platform we’ve built on Ray has already proven its value at scale. Models deployed on it process over 500 million images, videos, and text items every day, supporting a wide range of production use cases. The platform scales seamlessly as demand grows, allowing our engineers to stay focused and productive while continuing DoubleVerify’s long tradition of AI-powered innovation.


## **Why Ray and Kubernetes Are a Powerful Duo**


As I delved into the ecosystem, I realized the power of combining **Ray** and **Kubernetes** :


- **Ray** simplifies distributed ML workloads, making parallel processing and inference approachable even for those without deep systems knowledge. It has Kubernetes support using the KubeRay operator and Ray YAML manifests.
- **Kubernetes** excels at orchestrating containerized applications, offering scalability, resilience, and flexibility.


### **Ray: Simplifying ML Workflows**


According to a 2024[industry survey](https://www.kdnuggets.com/survey-machine-learning-projects-still-routinely-fail-to-deploy) by ML consultancy Rexer Analytics and highlighted by Eric Siegel in *The AI Playbook* , 43% of data scientists stated that 80% or more of their projects fail to reach **production** .


These findings underscore a critical point: ML models often remain stuck in development, and Ray is here to solve that by abstracting and bridging the gap between development and deployment.


Ray is more than just another tool in an ML engineer’s arsenal; it’s a framework designed to handle distributed computing intuitively. I’ve found Ray to be a no-brainer when it comes to deploying ML models — it took me less than five minutes to get a *ResNet16* up and running.


Here’s why it became our go-to choice:


- **Actor Model:** Ray’s actor model makes scaling Python code across clusters intuitive. It handles complex workflows with minimal modifications, enabling developers to build distributed systems with Pythonic simplicity.
- **Integrated Libraries:** Built-in libraries like Ray Serve for scalable model serving and Ray Tune for hyperparameter optimization integrate natively with Python workflows, streamlining the transition from prototype to production.
- **Unmatched Versatility:** Whether training large models, tuning hyperparameters, serving real-time predictions, or running reinforcement learning simulations, Ray does it all — reducing the reliance on fragmented tools.
- **Auto-Scaling:** Ray dynamically allocates resources based on demand, from single laptops to massive clusters. This ensures optimal resource utilization, particularly in cloud environments such as AWS, GCP, and Kubernetes.
- **Community and Support:** Ray’s open-source community is vibrant and active. I’ve reached out to its Slack community for support and even contributed new features alongside their mentors.
- **Unified Development:** The same Ray code runs on your laptop and a 100-node cluster, eliminating the “works-on-my-machine” issue and simplifying testing, debugging, and scaling.


Ray’s holistic approach makes it a cornerstone for building next-generation ML platforms, where scalability, efficiency, and simplicity are paramount.


### **Kubernetes as the Backbone**


Kubernetes has emerged as a cornerstone of modern software deployment, offering unmatched scalability and resilience — qualities essential for our ML platform. It has been the backbone of our platform, enabling us to deploy Ray containerized applications, allowing for efficient scaling and resource management of machine learning tasks.


Kubernetes’s extensibility is further enhanced by its ability to work with custom operators, making complex workloads more manageable.


### **How KubeRay Bridges Ray with Kubernetes**


Integrating Ray with Kubernetes has become significantly easier with KubeRay, a Kubernetes operator specifically designed to manage Ray applications within Kubernetes clusters, which are heavily used at DoubleVerify and are familiar to the engineers.


Press enter or click to view image in full size


Our use case involves managing the lifecycle of Ray Serve applications and their associated Ray Clusters using the RayService Custom Resource Definition (CRD).


Key features of KubeRay include:


- **Automated Deployments** : Seamlessly creates and manages deployments and pods for Ray Clusters, ensuring proper configuration and smooth operation.
- **Dynamic Scaling** : Adjusts the Ray Cluster size based on workload demands, optimizing resource utilization and handling fluctuations.
- **Resilient Application Deployment** : Ensures smooth rollouts and handles deployment failures by recreating Ray Clusters when needed.
- **Proactive Monitoring** : Provides built-in health checks and recovery actions to maintain high availability.


Press enter or click to view image in full size


[KubeRay Operator managing the Ray Cluster lifecycle.](https://medium.com/sage-ai/demystifying-the-process-of-building-a-ray-cluster-110c67914a99)


Thanks to these features, KubeRay simplifies the management of Ray-based applications on Kubernetes. It improves scalability, fault tolerance, and operational continuity while providing the ease of management and self-healing capabilities typical of Kubernetes operators.


Now, let’s shine the spotlight on what KubeRay manages: RayService–the backbone of Ray Serve deployment.


## **Deploying and Managing RayService with KubeRay and Helm**


### **RayService: Simplifying Model Serving**


RayService is a Custom Resource Definition (CRD) that streamlines model inference. Once your model is trained, you can use RayService to create a distributed Ray Cluster with multiple workers, linking it to your application via a service. It simplifies the deployment and management of Ray Serve applications on Kubernetes by combining the Ray Cluster with a deployment graph, ensuring smooth operation for model serving.


**What is Helm?**


Helm is a package manager for Kubernetes manifests that simplifies the deployment and management of applications by providing reusable, versioned configurations.


**Wrapping RayService with Helm**


Ray’s documentation doesn’t mention Helm, but we quickly realized how powerful it could be for simplifying RayService deployments. By packaging the ***RayService*** Custom Resource Definition (CRD) into a Helm chart, we streamlined model serving workflows, eliminated repetitive YAML configurations, and embedded best practices directly into our setup.


This Helm-based approach lets developers deploy models with minimal infrastructure knowledge — just update a ***values.yaml*** file and deploy. We also added customizations to improve resilience, reduce costs, and integrate seamlessly with our internal developer tooling.


**Best Practice 1: Simplified Python Dependency Management with Private PyPI**


Our ML engineers often rely on internal Python packages. To eliminate repetitive setup steps, we built logic into the Helm chart to automatically inject ***pip*** credentials into every Ray pod. This enables seamless access to our private Artifactory-hosted PyPI registry without requiring project-specific configurations.


As a result, ML engineers can import shared utilities and internal packages with no extra effort — removing one more hurdle to productionization.


**Best Practice 2: Fault-Tolerant Ray Clusters via External Redis (GCP Memorystore)**


Ray’s default behavior ties the cluster state to the head node. If that pod crashes, the cluster resets, leading to an expensive and disruptive failure mode, especially when running GPU workloads.


To address this, we integrated **GCP Memorystore** (managed Redis) into our RayService Helm chart. This externalized Ray’s internal state, enabling new head pods to reconnect to the same Redis instance after a failure. With this setup:


- Cluster state (e.g., Serve deployment graphs, actor schedules) survives restarts.
- GPU workloads don’t get re-spun unnecessarily.
- We cut GPU costs by **30%** thanks to fewer cold starts.


**Achieving Resilience with External Redis Integration**


Press enter or click to view image in full size


Ray architecture with head’s fault tolerance achieved by Redis


To automate this configuration securely, the Helm chart creates a Kubernetes Secret for Redis credentials:


```text
apiVersion: v1  kind: Secret  metadata:    name: redis-password-secret  type: Opaque  data:    password: {{ .Values.SECRET.REDIS_AUTH_STRING | b64enc }}
```


These credentials are injected into the head pod as environment variables:


```text
template:          spec:            containers:            - name: ray-head              …              env:                - name: NETRC                  value: "/home/ray/netrcvolume/.netrc"                {{- if (.Values.rayClusterConfig).enableFaultTolerance }}                - name: REDIS_PASSWORD                  valueFrom:                    secretKeyRef:                      name: redis-password-secret                      key: password                - name: RAY_REDIS_HOST                  Value: <GCP_MEMORISTORE_IP>:6379"
```


And we ensure the Redis integration is respected during cluster startup using ***rayStartParams***


```text
rayStartParams:    redis-password: $REDIS_PASSWORD
```


This configuration ensures both **security** and **resilience** , aligning with our platform’s design goals.


**A Turnkey Deployment Experience**


With these improvements baked into our Helm chart, developers can now deploy models by:


- Customizing a ***values.yaml*** file,
- Pointing to our remote Helm chart (hosted in Artifactory),
- Following well-documented examples in our codebase.


This has reduced onboarding time, accelerated the delivery of ML use cases like real-time fraud detection and recommendation engines, and made scalability and fault-tolerance the default — not an afterthought.


By wrapping ***RayService*** in Helm, we have successfully bridged the gap between ML experimentation and robust, production-grade deployment.


## **Real-World Model Deployment and Use Cases**


Deploying models like MobileNet for image classification has become faster and simpler. As part of our proof of concept (POC), we successfully deployed MobileNet using Kubernetes.


### **Step-by-Step Deployment Guide:**


**Prerequisites**


- A functional **Kubernetes cluster**
- **KubeRay** is installed and operational
- **Helm** installed
- A trained ML model (e.g.,[MobileNet](https://github.com/ray-project/serve_config_examples/blob/master/mobilenet/mobilenet.py) )


### **Step 1: Specify Attributes in a Helm Values File**


Create a file named ***values.yaml*** with the following configuration:


```text
#values.yaml  # RayCluster Configuration  rayClusterConfig:    rayVersion: "2.43.0"   # Worker Node Group Configuration  rayWorkersGroup:    - name: worker   replicas: 1   maxReplicas: 5   # Serve Configuration  serveConfig:    applications:   - name: MobileNet       import_path: mobilenet.mobilenet:app       runtime_env:           Working_dir: "https://github.com/ray-project/serve_config_examples/archive/b393e77bbd6aba0881e3d94c05f968f05a387b96.zip"          pip:           - python-multipart==0.0.6       deployments:         - name: ImageClassifier           num_replicas: 2           ray_actor_options:             num_cpus: 1
```


The ***working_dir*** link points to the Ray Python code. For production environments, it’s[recommended](https://docs.ray.io/en/latest/serve/production-guide/docker.html#custom-docker-images) to package these Python files into a Docker image and use that image for the workers.


### **Step 2: Deploy the MobileNet Model Using Helm**


Run the following command to deploy the MobileNet model:


```text
helm upgrade --install dv-mobilenet dv/dv-rayservice -f values.yaml
```


This deploys the RayService setup, complete with specified worker nodes and application configurations.


### **Step 3: Forward the Port for Ray Serve**


Expose the Ray Serve endpoint to interact with the deployed model by running:


```text
kubectl port-forward svc/dv-mobilenet-serve-svc 8000
```


This forwards the service to port 8000, making it accessible locally for testing.


### **Step 4: Send a Request to the ImageClassifier**


**Step 4.1: Prepare an Image File for Classification**


I chose this golden retriever image:


Press enter or click to view image in full size


**Step 4.2: Update *Image Path* in**[mobilenet_req.py](https://github.com/ray-project/serve_config_examples/blob/master/mobilenet/mobilenet_req.py)


Edit the \`mobilenet_req.py\` file to include the path to your prepared image.


**Step 4.3: Send a Request to the *ImageClassifier***


Run the following command to classify the image:


```text
python mobilenet_req.py
```


**Result:**


{ “prediction”: \[“n02099601”, “golden_retriever”, 0.17944198846817017\] }


## **Embracing Innovation in ML Deployment**


At DoubleVerify, we don’t just develop state-of-the-art ML models — we innovate in deployment, scaling, and management.
This journey with Ray has highlighted the importance of sharing knowledge to empower others. Ray’s robust framework, coupled with the power of Kubernetes, has strengthened our platform and enabled faster, more efficient workflows.


As surveys like Eric Siegel’s illustrate, ML projects often face deployment challenges, but tools like Ray can help bridge this gap and drive production success. By embracing Ray and Kubernetes, we’ve turned our vision into reality — building a platform that supports mission-critical use cases and drives innovation.
