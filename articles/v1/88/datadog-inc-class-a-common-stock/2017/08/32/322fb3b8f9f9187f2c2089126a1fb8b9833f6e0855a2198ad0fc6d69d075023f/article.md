---
schema_version: "1.0.0"
document_id: "322fb3b8f9f9187f2c2089126a1fb8b9833f6e0855a2198ad0fc6d69d075023f"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/scaling-support-with-vagrant-terraform/"
published_at: "2017-08-23T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:19efcc312c1b9cac349beb885cbbcc804d11b964854df1691ab04811a1c84948"
---

# Scaling support with Vagrant and Terraform

Stephen Lechner


A large part of our job on the Datadog Solutions Team is to reproduce problems that customers run into while they try using our many integrations in their own, always-unique environments. If, for example, someone’s Ceph` use_sudo` configuration option doesn’t seem to work in their agent check when they’re running CentOS 7, the best way to verify that a real problem exists is to just try it yourself, mimicking that environment as closely as possible.


[Container technologies like Docker are hot](https://www.datadoghq.com/docker-adoption/) , but when you need to replicate environments that may be running containers and orchestrators, or require specific operating systems and kernels, virtual machines are a great solution. But how do you manage VMs in a scalable way that enables fast provisioning and easy sharing among a growing team?


## Reproducing environments with Vagrant


Enter Hashicorp’s[Vagrant](https://www.vagrantup.com/) , a super easy tool for managing local virtual machine technologies, like Oracle’s Virtualbox or VMware’s Desktop. Many programmers are already very familiar with how easy it is to` vagrant init` ,` vagrant up` , and` vagrant ssh` into a brand new environment that you have strong control over. And so from the very beginning, that’s been our Solutions Team’s go-to approach for reproducing customer issues and narrowing down to their roots.


The problem we ran into, though, is that setting up a full reproduction environment takes time. It’s easy enough to` vagrant up` a box, but making sure that box has everything installed on it that you need to mimic the customer use-case is much more involved. Furthermore, learning takes time, and with over 200 integrations, nobody can be an expert in all of them. There will be a day for every Solutions Engineer when they have to troubleshoot an issue with an integration they’ve never installed or even used before – whether that be Apache Kafka, MS SQL Server, or RabbitMQ run in a Mesos cluster that uses Consul for Autodiscovery. Having to teach yourself these technologies is certainly an amazing learning opportunity, but given the amount of customer requests a Solutions Engineer handles on a daily basis, we needed a solution that would expedite the setup process and make our team more efficient.


That solution was easy and hinged on a simple feature in Vagrant: Provisioning. Provisioning can run configuration management tools, like Chef, Puppet, or Ansible. It can even run simple bash scripts that contain all the commands to install a technology and its integration with the Datadog agent. All the installation and configuration steps a Solution Engineer once had to take time to learn in order to reproduce an environment now need only to have been learned once by anyone on the team; they can be contained in a provisioning script for future use.


To use provisioning, we made a shared github repository for all the virtual machines that have been used for reproduction of customer issues. Every virtualbox gets its own directory, and in that directory you’ll find a template-built Vagrantfile that uses a` setup.sh` file for its provisioning. In that` setup.sh` you find all the commands the Solution Engineer had to run in order to install and configure whatever technology needs tested, as well as a` /data` subdirectory that contains any additional files that may be needed (configurations, scripts, etc.). Any local variables that will be specific to a Solution Engineer’s own environment (tags, hostnames, etc.) will be set in a local` .sandbox.conf.sh` file in their home directory and reference by the` setup.sh` script. All an engineer need do to build a reproduction environment now is cd to a relevant, pre-existing sandbox repo and run` vagrant up` . A few minutes later they can ssh into their box and make the specific changes to it that mimic the customer’s use-case.


Our sandbox repo directory structure:


```text
1   sandbox/    2   ├── README.example.md    3   ├── Vagrantfile.example    4   ├── setup.sh.example    5   ├── operating_system_or_distribution    6   │   └── version_or_provider    7   │       └── sandbox_name    8   │           ├── README.md    9   │           ├── Vagrantfile    10   │           ├── setup.sh    11   │           └── data    12   │               └── any_additional_files    13   └── ubuntu    14          └── xenial_16.04    15              └── kafka    16                  ├── README.md    17                  ├── Vagrantfile    18                  ├── setup.sh    19                  └── data    20                      ├── kafka-server-start.sh    21                      ├── kafka.yaml    22                      ├── zk.yaml    23                      └── zoo.cfg
```


And so we have a way to share our reproduction environments with each other; we no longer have to reinvent the wheel every time we want to reproduce an issue. But our work here wasn’t done. We’ve taken this one step further to something even more interesting.


## Sharing environments with Terraform


Another tool developed by Hashicorp is[Terraform](https://www.terraform.io/) . The same utility that Vagrant offers us for managing local virtual machines, Terraform offers for managing remote instances in various cloud environments, among them AWS. Using Terraform, we’ve been able to maintain the same directory structure, and even use the same exact repo, to quickly provision reproduction environments on small EC2 instances that we can leave running and share with teammates. Our Terraform module uses the same` setup.sh` and` /data` files that Vagrant draws from for its Vagrantfile configuration, so with no redundant effort, we’re able to terraform apply the same way we vagrant up. Each directory just also has to have a` .tf` file that terraform can use to spin up an EC2 instance, copy the` /data` directory to, and remotely execute the` setup.sh` provisioning script on. The work involved in making each individual` .tf` file is minimized by a repo-wide module to handle all the heavy lifting, and a` tf.example` file to offer an easy starting point for the rest.


The result is this:


```text
1   sandbox/    2   ├── README.example.md    3   ├── Vagrantfile.example    4   ├── main.tf    5   ├── tf.example    6   ├── setup.sh.example    7   ├── operating_system_or_distribution    8   │   └── version_or_provider    9   │       └── sandbox_name    10   │           ├── README.md    11   │           ├── Vagrantfile    12   │           ├── descriptive_name.tf    13   │           ├── setup.sh    14   │           └── data    15   │               └── any_additional_files    16   └── ubuntu    17          └── xenial_16.04    18              └── kafka    19                  ├── README.md    20                  ├── Vagrantfile    21                  ├── kafka.tf    22                  ├── setup.sh    23                  └── data    24                      ├── kafka-server-start.sh    25                      ├── kafka.yaml    26                      ├── zk.yaml    27                      └── zoo.cfg
```


Now we can leave our remote sandboxes running for longer time periods without our local RAM suffering. With the right network security management, we can share access to our sandboxes with the rest of our Solutions Team. Even when someone is on a live chat, they can reproduce a customer’s configuration settings in minutes using a previously provisioned integration. We can cooperate on reproduction efforts, so that if an urgent investigation needs attention long into the night in one timezone, teammates in subsequent time zones can continue the work. And if a (prospective) customer needs a demonstration of specific integrations, a solution member doesn’t need to spend valuable time building an example that will speak to a relevant use-case.


## Where to go from here


Of course there is still more to be done. Our next steps include better modularization of the setup scripts so that individual sandboxes can more easily use varying combinations of integrations, for more complex and still more tailored demonstrations. With the help of a terraform backend system like[Consul](https://www.consul.io/) (also by Hashicorp!), we could cooperatively manage our terraform states themselves, and introduce additional modules. But with our Solutions Team sandbox repository, we’ve made an important part of our support efforts much more scalable, and we couldn’t have done it without Vagrant and Terraform. Thanks Hashicorp.


-
-
-
