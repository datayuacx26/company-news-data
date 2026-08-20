---
schema_version: "1.0.0"
document_id: "3c74f4c8d61128bf6556077938944a4b6efe7e9cbb56cb3feb0de10419d1a628"
company_key: "wealthfront-corporation-common-stock"
company: "Wealthfront Corporation"
source_id: "wealthfront-corporation-common-stock-rss-9fbe6ef385e3"
canonical_url: "https://eng.wealthfront.com/2025/10/08/shipping-containers-how-we-built-an-easy-to-use-jenkins-pipeline-for-ecr/"
published_at: "2025-10-08T19:15:22+00:00"
first_seen_at: "2026-07-20T23:18:22.615485+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:b2eaafa1b657d417a3a85bdd8a3b2b1f67559df348dfa4d2625a01ed7afa21c8"
---

# Shipping Containers: How We Built an Easy to Use Jenkins Pipeline for ECR

The DevOps team at Wealthfront has been in the process of migrating services to run inside containers. As part of this process we need a way to build containers using our CI/CD tool, Jenkins. While we could write a Jenkinsfile for each container image we wanted to build, we identified this was a good opportunity to codify and normalize our container build pipelines so that every team at Wealthfront can leverage the effort we apply to creating a useful container build pipeline.


Until now, we haven’t had a significant need for a container build pipeline because the vast majority of our pipelines have been relatively homogeneous: Java, Chef cookbooks, and a smattering of custom Jenkinsfiles. Now that we’re focusing on shipping more containers we need to create a similarly consistent pipeline for container releases.


In this post we will walk through why we want a centralized pipeline, design decisions for the pipeline, as well as provide an example of how we test the pipeline code. Finally, we hope to inspire you to codify your own build and release processes with a view for fast, easily-configurable pipelines.


## Why Centralize the Pipeline Configuration


Before starting a project, it’s important to identify the benefits and tradeoffs of a solution. A little forethought now can ensure we’re building the right solution at the right time in the right way. In that light, what are the benefits we hope to glean from a centralized container build pipeline?


- **The ability to enforce a consistent naming scheme for container repositories.** If we leave the name open, there is a risk two teams will decide they both want to name their repository “web-server”.
- **The ability to centrally manage ECR authentication.** Individual teams won’t need to figure out how to inject the correct credentials to connect with ECR if we can handle it for them.
- **The ability to standardize how ECR repos are configured.** For example, ensuring that ECR repos are given the correct AWS tags. This has the added benefit that we can obviate the possibility of typos in tag fields.
- **Reduce the need for specialized knowledge to spin up a new container project.** By abstracting the container build and push process we can flatten the learning curve to ship a new container project.
- **Multiply the effect of pipeline work.** The effort we put into this one library can be leveraged by future projects when they need to ship containers.
- Finally, a centralized pipeline **should set us up for future work** : we want to easily add features that benefit all image build pipelines without needing to touch each of the pipelines.


That’s a pretty compelling list of benefits! Before we move on, though, let’s think through what downsides–if any–might exist:


- A centralized pipeline means that **introducing a bug affects** ***every*** **image build pipeline** .
- Abstracting away the complexity of image build commands **deprives engineers of the opportunity to learn the build commands** and can inadvertently build knowledge silos. A motivated engineer can learn the image build commands but we won’t be giving them a natural on-ramp to learn them.
- **The pipeline doesn’t run locally** , so if someone is trying to build the image locally, they will need to write their own build commands rather than leverage the work we put into the pipeline. This also further compounds the missed learning opportunities.


As we think about these tradeoffs, the amount of flexibility and consistency we can achieve through centralized pipelines easily outweighs the downsides. In addition, if we write guides for engineers walking through the happy path of building a container on their local machine, we can help reduce some of the downsides.


This seems like a great idea, so let’s run with it!


## Writing a Pipeline Helper Using the Builder Pattern


One way to achieve a shared pipeline is to have a pipeline library which parses default values from a config struct. Below is an overly-simplified example:


```text
// jenkinsLibraries/vars/dockerReleasePipelineHardToTest.groovy


def   call(Map config = [:]) {
def   target = (config.remove( 'target'  ) ?:  'release'  )  as   String
def   owner = config.remove( 'owner'  )  as   String
if   (owner ==  null   || owner.isEmpty()) {
throw    new   Exception( 'owner must be defined'  )
}


pipeline {
agent {
label  'dockerce'
}


stages {
stage( 'Build'  ) {
steps {
sh  """
aws ecr create-repository --region=us-west-2 --repository-name=example --tags 'Key=owner,Value=${owner}'"
docker buildx build --target='${target}' --tag example:latest .
docker push example:latest
"""
}
}
}
}
}


Code language:    Groovy    (  groovy  )
```


While it is possible to test Jenkinsfiles like this using[JenkinsPipelineUnit](https://github.com/jenkinsci/JenkinsPipelineUnit) , as we add complexity the JenkinsPipelineUnit tests would quickly balloon in complexity.


Instead, we can use the builder pattern to handle most of the work of our pipeline logic. We see several advantages of using a builder rather than putting all the logic in the pipeline:


- It’s easy to define **default values**
- **Discoverability** of available options is easy
- We can use the class to **generate commands** for our pipeline
- It’s **simple to test** the class as well as the generated commands


Here is the most basic example of the builder pattern in groovy:


```text
// jenkinsLibraries/src/com/wealthfront/jenkins/DockerReleaseTarget.groovy


package   com.wealthfront.jenkins


class    DockerReleaseTarget   {
String target
String owner


private   String region =  'us-west-2'
private   String awsAccount =  '1234567890'


DockerReleaseTarget() {
this  .target =  'release'
this  .name =  ''
}


DockerReleaseTarget setName(String name) {
this  .name = name
return    this
}


DockerReleaseTarget withTarget(String s) {
this  .target = s
return    this
}


DockerReleaseTarget withOwner(String s) {
def   allowedOwners = [  'devops'  ,  'trading'  ,  'etc....'   ]
if   (!s) {
throw    new   IllegalStateException( 'Owner must be non-empty.'  )
}
if   (!allowedOwners.contains(s)) {
throw    new   IllegalStateException( "Invalid owner: '${s}'. Must be one of: ${allowedOwners.join(', ')}"  )
}
this  .owner = s
return    this
}


void   runValidation() {
if   (! this  .owner?.trim()) {
throw new IllegalStateException("The 'owner' field must not be empty, use withOwner(owner).")
}
if (!this.target.trim()) {
throw new IllegalStateException('The target must not be empty, use withTarget("targetName")')
}
if (!this.name.trim()) {
throw new IllegalStateException('The name must not be empty, this is an issue with the dockerReleasePipeline.')
}
}


String getEcrRepo() {
return "${this.awsAccount}.dkr.ecr.${this.region}.amazonaws.com/${this.name}"
}


String ecrRepoLoginScript() {
return "aws ecr get-login-password --region ${this.region} | docker login --username AWS --password-stdin ${this.getEcrRepo()}"
}


String createEcrScript() {
return "aws ecr create-repository --region=${this.region} --repository-name=${this.name} --tags 'Key=owner,Value=${this.owner}'"
}


String dockerBuildScript() {
return "docker buildx build --target='${this.target}' ${this.name} ."
}


String dockerPushScript() {
return "docker push ${this.name}"
}


}


Code language:    Groovy    (  groovy  )
```


Notice how this allows us to set standard values for the AWS Account and Region. At the same time, we define a default value for the owner tag, but allow users to override the tag with a list of allowed options. Finally, we are generating the relevant docker commands in this class so we can test that the commands will work as expected.


Of course, we have many more configuration options available so that engineers can pick target platforms, naming suffixes, dockerfile names, and more. This wide variety of options is important to let us support the scope of config the various teams using these pipelines require.


```text
addTag(String)
withShellTag(String)
withTags(String... tagList)
withPush(Boolean shouldPush =  true  )
withRepoSuffix(String s)
withTarget(String s)
withParallel(Boolean p)
withOwner(String s)
withBuildArg(String... args)
withRawBuildArg(String... args)
withArchitectures(String... archs)
withDockerfile(String dockerfile)
withArchitectureSpecificDockerfile(String arch, String dockerfile)
withLatestTag(Boolean tagLatest =  true  )
asPrivateRepo(Boolean asPrivate =  false  )
Code language:    Groovy    (  groovy  )
```


Looking at all these options, consider how important it is that we test how the options combine. With that in mind, let’s look at how we can test this library.


## Testing


Wealthfront values testing, which is a topic[we’ve written about many times before](https://eng.wealthfront.com/tag/testing/) . Our Jenkins libraries are no exceptions. Below is an example of how we can test the withOwner function to ensure it behaves as expected:


```text
// jenkinsLibraries/srctest/com/wealthfront/jenkins/DockerReleaseTargetTest.groovy


package   com.wealthfront.jenkins


import   com.lesfurets.jenkins.unit.BasePipelineTest
import   org.junit.Test


class    DockerReleaseTargetTest    extends    BasePipelineTest   {
@Test
void   testDockerReleaseTarget_withOwner() {
def   script = loadScript( 'vars/helper.groovy'  )
def   drt = script.newDockerReleaseTarget()
.withOwner( 'devops'  )


assert   drt.createEcrScript().contains( 'Key=owner,Value=devops'  )
}


@Test
void   testDockerReleaseTarget_invalidOwnerShouldFail() {
def   script = loadScript( 'vars/helper.groovy'  )
try   {
def   drt = script.newDockerReleaseTarget()
.withOwner( 'invalid-owner-name'  )
fail  'Expected exception was not thrown'
}  catch   (IllegalStateException x) {
}
}
}


Code language:    Groovy    (  groovy  )
```


This testing pattern allows us to confirm that validations work as expected and that the docker commands are generated as expected.


These two example tests show that the owner value is propagated correctly to the createECRScript as well as ensuring that invalid owners will throw an exception.


## Implementation in a Pipeline


Because we’re using our tested library for generating our commands, the final example pipeline becomes much simpler:


```text
// jenkinsLibraries/vars/dockerReleasePipeline.groovy


def   call(Map config = [:]) {
def   drt = config.remove( 'releaseTarget'  )
if   (drt ==  null  ) {
throw    new   Exception( 'releaseTarget must be defined'  )
}


drt
.setName(env.JOB_NAME)
.runValidation()


pipeline {
agent {
label  'dockerce'
}


stages {
stage( 'Build'  ) {
steps {
withCredentials([
usernamePassword( credentialsId:    'ecr-aws-creds'  ,
usernameVariable:    'AWS_ACCESS_KEY_ID'  ,
passwordVariable:    'AWS_SECRET_ACCESS_KEY'  ),
]) {
sh drt.ecrRepoLoginScript()
}
sh drt.createEcrScript()
sh drt.dockerBuildScript()
sh drt.dockerPushScript()
}
}
}
}
}


Code language:    Groovy    (  groovy  )
```


Notice how we can force the repo name using the .setName(env.JOB_NAME) function in the final pipeline. In addition, we call .runValidation() as a final check of the config before we start executing on the runner.


Because the validation occurs before we claim an agent, if there is a config error, the pipeline will fail right away. This gives immediate feedback to the engineer what parts they need to fix.


When users want build a container image, they can now use the pipeline template and pass in a builder:


```text
@Library  ("JenkinsFiles") _


dockerImageBuildPipeline(
releaseTarget:   helper.newDockerReleaseTarget()
.withOwner( 'devops'  )
.withTarget( 'release-debug'  )
)


Code language:    CSS    (  css  )
```


## Conclusion


And that’s it! While this blog post has presented the most basic implementation of a shared container build pipeline, hopefully you can see how this lays a foundation that sets us up to achieve our original goals of centralizing configuration, abstracting away ECR auth, standardizing repo tags, and setting us up to add new features down the road.


Overall this pattern of centralizing build pipelines, parameterizing them using the builder pattern, and testing the pipeline code leads to a pleasant engineering experience both when writing and using the pipeline. Discoverability of features is high and iteration cycles are tight because bad config pushes fail quickly.


If you want to join us in driving the future of containers at Wealthfront—prioritizing engineering experience, testing, and automation—consider[joining our engineering team](https://www.wealthfront.com/careers) !


---


#### Disclosures


The information contained in this communication is provided for general informational purposes only, and should not be construed as investment or tax advice. Nothing in this communication should be construed as a solicitation or offer, or recommendation, to buy or sell any security. Any links provided to other server sites are offered as a matter of convenience and are not intended to imply that Wealthfront Corporation or its affiliates endorses, sponsors, promotes and/or is affiliated with the owners of or participants in those sites, or endorses any information contained on those sites, unless expressly stated otherwise.


Investment advisory services are provided by Wealthfront Advisers LLC, an SEC-registered investment adviser. Brokerage services are provided by Wealthfront Brokerage LLC, Member[FINRA](https://www.finra.org/) /[SIPC](https://www.sipc.org/) . Financial planning tools are provided by Wealthfront Software LLC.


All investing involves risk, including the possible loss of money you invest, and past performance does not guarantee success. Please see our[Full Disclosure](https://www.wealthfront.com/legal/disclosure) for important details.


Wealthfront Advisers LLC, Wealthfront Brokerage LLC, and Wealthfront Software LLC are wholly owned subsidiaries of Wealthfront Corporation.


© 2025 Wealthfront Corporation. All rights reserved.
