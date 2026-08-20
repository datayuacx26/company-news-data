---
schema_version: "1.0.0"
document_id: "5472ec3108d363c17b9fb69cd1bda8a8c31e5e5c605a6775891d9bdac384d5ba"
company_key: "yc-okteto"
company: "Okteto"
source_id: "yc-okteto-rss-a64bce3f80ea"
canonical_url: "https://www.okteto.com/blog/using-argocd-with-okteto-for-a-unified-kubernetes-development-experience/"
published_at: "2023-07-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.770894+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:1291a82cdd4bd793f773c5d2aaa5bbb005af1db9ce2a9012ad6173ef0bbdcb21"
---

# Using ArgoCD With Okteto for a Unified Kubernetes Development Experience

# Using ArgoCD With Okteto for a Unified Kubernetes Development Experience


## Integrations With Okteto (8 Part Series)


1. [Cloud Native Development Made Easy With Sprkl and Okteto](https://www.okteto.com/blog/cloud-native-development-made-easy-with-sprkl-and-okteto/)
2. [Using LaunchDarkly and Okteto To Automate Modern Feature Flag Management](https://www.okteto.com/blog/using-launchdarkly-and-okteto-to-automate-modern-feature-flag-management/)
3. [Developing Cloud-Native Apps With MongoDB Atlas and Kubernetes](https://www.okteto.com/blog/developing-cloud-native-apps-with-mongodb-atlas-and-kubernetes/)
4. Using ArgoCD With Okteto for a Unified Kubernetes Development Experience


5. [Making Your Helm-Packaged Applications Ready for Cloud Native Development with Okteto](https://www.okteto.com/blog/making-your-helm-packaged-applications-ready-for-cloud-native-development-with-okteto/)
6. [Automate Provisioning Any Dev Resource on Any Cloud Provider With Pulumi and Okteto](https://www.okteto.com/blog/automate-provisioning-any-dev-resource-on-any-cloud-provider-with-pulumi-and-okteto/)
7. [Automating Development Environments and Infrastructure with Terraform and Okteto](https://www.okteto.com/blog/automating-development-environments-and-infrastructure-with-terraform-and-okteto/)
8. [Enhance CI Pipelines with Dagger and Okteto Preview Environments for a Better Developer Experience](https://www.okteto.com/blog/ci-pipelines-dagger-okteto/)


ArgoCD is a powerful tool for continuous deployment that leverages Git repositories as the ultimate source of truth for managing Kubernetes deployments. As a CNCF project, it has garnered tremendous popularity in recent years due to its user-friendly interface, adherence to GitOps principles, and enhancements to the software delivery lifecycle. In this article, let's explore the capabilities of ArgoCD, the benefits of incorporating it into your dev lifecycle, and understand how it can help create a unified development experience with Okteto!


## **What Does ArgoCD Actually Do?**


GitOps principles advocate for the use of Git repositories as the sole source of truth for application infrastructure and configuration. The concept revolves around storing all application-related files and manifests in a Git repository and using it as the central management hub for deployments. ArgoCD aligns with these principles by treating the configuration stored in a Git repository as the desired state for the application deployment, effortlessly deploying it to the specified target environment. What sets it apart is the inclusion of a Controller that continuously monitors the deployed application, ensuring synchronization with the manifests in the repository. In case of inconsistencies, ArgoCD empowers you to reconcile the differences through a single command or the click of a button.


Using ArgoCD for application deployment offers several benefits:


1. It maintains an up-to-date documentation of your application's deployment configuration in a version control system.
2. Rollbacks are as simple as reverting a commit, making it easy to undo changes.
3. It enables effortless modifications to deployments simply by merging pull requests.
4. It monitors the cluster to ensure that the deployed application aligns with the manifests in the git repositories, maintaining the desired state accurately.


As you delve into the rising popularity of ArgoCD, it becomes evident that its appeal is no surprise. However, it prompts an important question: if you're already leveraging ArgoCD for production deployments, why not extend its usage to manage your development deployments as well?


## **Why Integrate ArgoCD with Okteto**


In the industry, it has been widely believed that exactly replicating production environments during development is impractical, mainly due to cost, resource, and technological constraints. This notion became even more daunting with the shift towards distributed architectures and cloud services, as complexity continued to rise. Microservices and containerization have heightened the significance of bridging the gap between development and production. The complexity of microservice-based applications amplifies the likelihood of issues arising in production if not caught early on. But fear not, there is a solution: Okteto.


Okteto empowers platform engineers to automate the modern development experience by provisioning self-serviced production-like dev environments. These environments are for all stages of the dev lifecycle: from coding, until deployment. Okteto comes with ready-to-use Manifest and platform automation to make environments easy to define, control, and govern.


These environments deploy applications on Kubernetes and provision any additional necessary resources. If you're already using ArgoCD to manage your production deployments, it can seamlessly integrate with Okteto. This means you don't have to put in extra effort to recreate your production configuration. This approach ensures a single source of truth for both production and development configuration. And the benefits of ArgoCD, such as easy deployment changes through PR merges and seamless rollback options, extend to the development phase as well.


By leveraging Okteto's capabilities, you gain a faster inner loop in your development workflow. With Okteto, you can make changes to your code and test them quickly, without the need for committing to the repository right away. This allows you to iterate and validate your changes faster, ensuring a smoother development experience. Once you're satisfied with your changes, you can commit them, triggering the ArgoCD deployment logic, and exercise your deployment workflow as intended. This combination of Okteto and ArgoCD provides the best of both worlds: the speed and flexibility of Okteto's fast inner loop development, and the reliability and consistency of ArgoCD's GitOps model tied to commits and deployment logic.


By embracing the Okteto Experience and aligning your development and production environments, you can bridge the gap and create a unified, modern development experience for your team.


Interested?


[Get a demo](https://www.okteto.com/get-demo/)


## **How to Use ArgoCD with Okteto**


Now that you recognize the value of using ArgoCD for your development environments as well, let's explore how you can achieve this with Okteto. When utilizing Okteto, all the configuration for your development environment resides in a single manifest known as` okteto.yaml` . A typical` okteto.yaml` consists of three sections:


- ` build` : This section specifies how to build images for different services as part of your application.
- ` deploy` : Here, you define how Okteto deploys your application to set up your development environment.
- ` dev` : This section contains the configuration for development containers for each of your microservices.


As the name implies, the deploy section is where we leverage ArgoCD to deploy our application to the development environment, mirroring the production setup. If you have prior experience using ArgoCD for production deployments, you are familiar with the typical flow:


1. Running` argocd app create` to create your application.
2. Running` argocd app sync` to fetch manifests from the specified repository and deploy them to your Kubernetes cluster.


We will follow this exact process in the deploy section of our Okteto manifest. To demonstrate, we have created a "Hello, World!" Go server. You can find the code and detailed instructions in the repository[here](https://github.com/RinkiyaKeDad/simple-go-server) . Upon examining the deploy section in the Okteto manifest, you will notice the following commands:


```text
deploy  :
-   argocd app create simple  -  go  -  server   -  -  repo https  :  //github.com/rinkiyakedad/simple  -  go  -  server.git   -  -  path k8s   -  -  dest  -  server https  :  //kubernetes.default.svc   -  -  dest  -  namespace $OKTETO_NAMESPACE
-   argocd app sync simple  -  go  -  server
```


For the` argocd app create` we specify the location of our manifests by providing the` --repo` and` --path` flags. You should replace these with the location of your application manifests.


As the name suggests, the` --dest-namespace` flag tells ArgoCD which namespace to deploy your application to. Since we want this namespace to be the one that the developers specify when running` okteto up` we use the` $OKTETO_NAMESPACE` environment variable here. The installation job automatically populates this variable with the namespace you specify when running` okteto up` (or your default dev namespace if you don’t specify anything).


This is the only configuration that platform engineers need to do in order to use their production manifests for development environments. ArgoCD will ensure that each developer's environment matches the state specified in the manifests from the designated repository.


While the example application is a single microservice and assumes that ArgoCD and Okteto are installed in the same cluster, the core concept of using ArgoCD with Okteto remains the same: adding your ArgoCD deploy commands to the` deploy` section of the Okteto manifest. Regardless of your specific configuration, this approach holds true!


## Conclusion


In this article, we explored how ArgoCD enhances the practices of continuous delivery by introducing GitOps into the workflow. We examined the benefits it offers through its approach of treating Git as the source of truth and continuously monitoring and reconciling the deployed application state with the one specified in Git repositories. Additionally, we discussed how ArgoCD, together with Okteto, simplifies the replication of production infrastructure during development. We delved into the advantages of this approach and examined a simple application that demonstrated how easily it can be accomplished.


In conclusion, integrating ArgoCD with Okteto can significantly streamline your modern development flow, ensuring that your development and production environment configurations are aligned seamlessly. By applying the same ArgoCD commands in the Okteto Manifest that you use for production deployments, you automate a standard, unified modern environment for the developers in your team. Remember, a well-structured and consistent development environment is a key foundation for successful modern app delivery.


Tools like ArgoCD are increasingly prevalent in the Kubernetes production ecosystem. That's why it's crucial for platform teams to utilize these very tools to seamlessly bridge the gap between development and production for their developers. Okteto not only provides platform teams with an easy, ready-to-use modern development experience but also enables them to unify development and production environments.


To explore the full potential of our platform, sign up for our[free 30-day trial](https://www.okteto.com/free-trial/) !


## Integrations With Okteto (8 Part Series)


4. Using ArgoCD With Okteto for a Unified Kubernetes Development Experience


Arsh Sharma


Developer Experience Engineer / Emojiologist 😜


[View all posts](https://www.okteto.com/blog/authors/arsh-sharma/)


[remote-development-environments](https://www.okteto.com/blog/tags/remote-development-environments/)


[kubernetes](https://www.okteto.com/blog/tags/kubernetes/)


#### Share this:
