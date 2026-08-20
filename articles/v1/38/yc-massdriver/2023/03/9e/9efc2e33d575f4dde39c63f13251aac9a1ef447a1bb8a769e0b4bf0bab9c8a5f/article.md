---
schema_version: "1.0.0"
document_id: "9efc2e33d575f4dde39c63f13251aac9a1ef447a1bb8a769e0b4bf0bab9c8a5f"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/kubeflow-guide-part-1"
published_at: "2023-03-18T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:8b10b2d1fcda0ac32bebd268d3fe232e88e9cdcd8f5fc3c286a365b0fdf720ff"
---

# Kubeflow guide: Part 1

Developing a machine learning model needs end-to-end thinking. From data collection to model deployment, productionizing an ML model is a journey. After the final step - model deployment, ML engineers and data scientists should not stop there but keep measuring the model performance and monitoring model drift. Due to the complexity and wide areas to cover, people working in the ML domain rely on software like Kubeflow that can streamline the ML process. This two-part article introduces Kubeflow and how to install the software on your local machine.


## What is Kubeflow?


Kubeflow is an open-source platform to develop, improve, and maintain ML models on Kubernetes. It allows you to easily integrate and deploy ML workflows regardless of where your infrastructure exists - on-premise, cloud, or hybrid. The software contains tools and libraries for data processing, model training, deployment, and management and helps users to deal with the complexity of building, deploying, and scaling ML workflows.


### ML workflow concept


Kubeflow makes ML workflows simple. What is an ML workflow? It refers to the process of building and deploying an ML model. A workflow has several stages, including data preparation, model training, evaluation, and deployment. The first step involves collecting and cleaning the data, followed by feature engineering and data normalization. In the training stage, the model is trained using a dataset and an optimization algorithm. After training, the model is evaluated using metrics such as accuracy, precision, and recall. Finally, if the model meets the desired performance, it is deployed to a production environment and monitored for performance. The entire process is iterative and requires fine-tuning and improvement over time.


### Where does Kubeflow sit in ML workflow?


Kubeflow sits in the middle of this ML workflow covering data preparation and model deployment. The open-source software provides a platform for the building, deployment, and management of ML workflows, making it easier to manage the entire process.


### How does it work with Kubernetes?


Kubeflow works with Kubernetes accessing the benefits of Kubernetes’ platform to manage the deployment, scaling, and management of ML workflows. Kubeflow is built on top of Kubernetes utilizing its container orchestration capabilities to deploy and manage ML workflows. The platform includes several custom resources that can be used to describe the different components of an ML workflow, such as data processing, model training, and deployment. These components are deployed as containers on a Kubernetes cluster for easy scaling and management of the entire ML workflow. Kubernetes also provides a unified platform for managing and monitoring the health and performance of the ML workflow, making it easier to identify and resolve issues. In this way, Kubeflow leverages the strengths of Kubernetes to provide a robust, scalable, and efficient platform for managing ML workflows.


## Kubeflow components


Kubeflow consists of six main components. Let’s have a look at each of them to understand Kubeflow’s ecosystem.


### Dashboard


The Kubeflow dashboard is a web-based user interface that gives a centralized view of all the components and activities within an ML workflow. The dashboard displays the various stages of an ML workflow and provides real-time monitoring of the progress of each step. The dashboard allows users to manage and interact with the different components of the workflow. Users can access logs and metadata related to each stage in the dashboard. Additionally, the dashboard includes tools for managing the deployment of ML models, including the ability to roll back to previous versions and manage the scaling of deployment resources.


### Notebooks


Kubeflow Notebooks are also a web-based tool for writing and running code. Data scientists and ML engineers can write, run, and debug code for preparing data, training models and deploying ML workflows. The Notebooks run within the Kubeflow cluster and give access to all the resources and data that are part of the ML workflow. Users can create, edit, and run widely-used Jupyter notebooks. In Kubeflow Notebooks, you can use pre-installed libraries and tools, which makes it easier to start with ML development.


### Kubeflow Pipelines


Kubeflow Pipelines is a component that allows users to build scalable and portable end-to-end ML workflows. Pipelines provide a high-level representation of an ML workflow, enabling engineers to model and automate the process of building and deploying ML models. Each step in the pipeline is described as a container, which can include any combination of data processing, model training, and deployment activities. Pipelines can be run on a variety of platforms, including local workstations, cloud-based environments, and on-premises clusters.


### Katib


Katib was started as a Kubernetes-native project for automated machine learning - AutoML. Using Katib, users can perform hyperparameter tuning, early stopping, and neural architecture search - NAS. Katib is agonistic to ML frameworks such as TensorFlow, MXNet, PyTorch, XGBoost, and others. Katib has four major concepts.


- Experiment: a single tuning (or optimization) run. Users can choose an objective, search space, and search algorithm.
- Suggestion: a set of hyperparameter values that are proposed by the hyperparameter tuning process. Katib creates a trial to evaluate the suggested set of values.
- Trial: an iteration of the hyperparameter tuning process. A trial is handled by one worker job instance with a set of parameter assignments. That set corresponds to a suggestion.
- Worker job: evaluates a trial and calculates its objective value.


### Training Operators


Training operators can be used to train a model using a framework. Kubeflow has operators for TensorFlow, PaddlePaddle, PyTorch, MXNet, XGBoost, MPI, and job scheduling.


### Multi-Tenancy


With Kubeflow’s multi-tenancy feature, you can isolate multiple users and control IAM - Identity Access Management. With user isolation, a data team can implement efficient infrastructure and operations.


## Kubeflow use cases


Kubeflow is now widely used in organizations of different sizes for various purposes. By looking at use cases, you can get a clearer idea of when and how we can use Kubeflow.


- **Automated ML workflows** : Kubeflow can be used to create automated ML workflows that are run as pipelines on multiple machines and environments, from local laptops to cloud-based clusters.
- **Model training** : Kubeflow gives a centralized platform for training machine learning models on large datasets, making it easier to manage and monitor the training process. With the ability to run training jobs on multiple nodes in parallel, it helps organizations to scale their training efforts and work with larger datasets.
- **Model deployment** : Kubeflow streamlines the deployment of trained models by providing a platform for releasing models as microservices with automatic scaling and failover capabilities. This makes it easy to deploy models in production, with the ability to manage and monitor the performance of the models in real-time.
- **Hyperparameter tuning** : Kubeflow provides tools for optimizing the hyperparameters of machine learning models. This makes it easier to find the best configuration for a given task. This helps data scientists to get the most out of their models and to improve the accuracy and reliability of their results.
- **Experiment management** : Kubeflow provides an experiment management system that makes it easy to track and compare the performance of different models, which allows users to select the best one for deployment. With the ability to log and store results, it’s easy to track the progress of the models over time and make informed decisions.
- **Collaboration** : Kubeflow enables collaboration among data scientists by providing a platform for sharing and reproducing ML workflows and models. With the ability to share notebooks, pipelines, and experiments, teams can work together on projects, ensuring that everyone has access to the latest results and insights.


## Conclusion


Kubeflow provides a range of benefits to data scientists and ML engineers. Developing and maintaining an ML model requires extensive and repetitive jobs, which makes the whole ML process resource-intensive and expensive. As the article introduced, with Kubeflow, users can rely on a single platform that can cover the end-to-end cycle. In the next part, we will learn how we can install Kubeflow locally.


‍
