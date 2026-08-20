---
schema_version: "1.0.0"
document_id: "7bd62112288edb07e910d0b16bfa23cef193b2a70e37f2580f4e1fdd7ddb6814"
company_key: "cars-com-inc-common-stock"
company: "Cars.com Inc."
source_id: "cars-com-inc-common-stock-rss-49a4db916ec1"
canonical_url: "https://tech.cars.com/cars-forge-spot-ec2s-made-easy-7d47f0707d8"
published_at: "2022-09-27T17:04:02+00:00"
first_seen_at: "2026-07-20T23:17:02.597310+00:00"
fetched_at: "2026-07-28T22:26:30.015715+00:00"
content_hash: "sha256:ec8047e8376a16802dc6a96cba76d714cd9005034f772bd1bf5f309814c687b2"
---

# Cars-Forge: Spot EC2s Made Easy

Aws Ec2


AWS


Spot Ec2


Python


Spark


# Cars-Forge: Spot EC2s Made Easy


[Nikhil Patel](https://medium.com/@nikhil530?source=post_page---byline--7d47f0707d8---------------------------------------)


5 min read


·


Sep 27, 2022


--


Press enter or click to view image in full size


Photo by[Ian Battaglia](https://unsplash.com/@ianjbattaglia?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


About a year and a half ago, I wrote a[blog](https://tech.cars.com/teaching-an-elephant-to-fly-6820f68e5fdd) about how we migrated from our on-prem Hadoop cluster to AWS and the challenges we faced. We wanted a better solution than doing a lift and shift to AWS Elastic Mapreduce (EMR). We needed a solution that would give us more flexibility and cost less. We are happy to announce that after over two years of testing and development, we have open-sourced our internal tool,[cars-forge](https://github.com/carsdotcom/cars-forge) .


> Forge is a Python-based command-line tool that starts a single or cluster of[Elastic Compute Cloud (EC2)](https://aws.amazon.com/ec2/) instances, and runs the required scripts on these instances. Once the EC2 is started you can do anything like develop products, analyze data, host interactive dashboards, or run production workloads on terabytes of RAM. All this can be done on spot or on-demand instances. By using the former, you can save up to 90% compared to on-demand instances. Currently, at Cars.com, we are using Forge clusters to run pyspark jobs and our single instance for[Jupyter](https://jupyter.org/) and Machine Learning (ML) training and scoring.


## Value Proposition


There are three main advantages to cars-forge (or simply Forge), below we discuss them in detail.


1. **Cost savings:** If you choose, Forge will spin up your EC2s using[spot instances](https://aws.amazon.com/ec2/spot/) . Spot instances are EC2 instances that are not being used and AWS allows you to use them at a discounted rate. The main downside is that you can lose them if another user wants to use that same instance at full price.
Forge uses the[capacity-optimized](https://aws.amazon.com/about-aws/whats-new/2019/08/new-capacity-optimized-allocation-strategy-for-provisioning-amazon-ec2-spot-instances/) allocation strategy which takes into account availability, price, and the likelihood of the instance being lost to pick the right instance for you. This greatly reduces the chance of your spot instance being lost before your job completes. For very critical workloads, you have the option of using on-demand instances.
2. **Ease of use:** Without Forge, the developer would need to know how to use the AWS console, terraform, or boto3 to spin up EC2s. Once the administrator sets up the environment, Forge makes it very easy for the end user to spin up EC2s. Forge only requires a yaml file with a few parameters. Within minutes, you will have a single instance or a cluster of instances.
Forge can be used by students or anyone even if they have limited experience with AWS. Below, we will walk you through an example of how to set up Forge to run a Jupyter notebook.
3. **Flexibility:** Back when we were using on-prem Hadoop, there was always a fight for resources either between developers or production jobs. Forge removes that issue by allowing everyone to spin up their own instance to do anything they need. In our development environment, developers can spin up a small instance to run jupyter, a medium instance to build their code, and a large instance to train their models at the same time. They no longer have to wait for resources or accidentally crash an instance. In production, each of our jobs spins up its own cluster. We could set up a large EMR that we scale in and out but with over 200 jobs it can be difficult to manage.


## Additional Features


- Create a single or cluster of EC2 instances of any size using spot or on-demand pricing
- Rsync files to the instance(s)
- Run a script on the instance(s)
- Destroy your EC2 fleet
- SSH into your master or single instance
- Start and Stop your on-demand instance(s)


For more detailed information, please check out the[documentation](https://carsdotcom.github.io/cars-forge/) .


## Install


In order to use Forge you need Python 3.7 (or later). The installation guide is pretty much like any other python package (via pip).


` pip install cars-forge`


## Get Nikhil Patel’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


To install from source, you need to copy or clone the repository onto your machine. Then, navigate to the root of the project and install.
` pip install .`


For more information please visit the[install page](https://carsdotcom.github.io/cars-forge/install.html) .


## Example


Below we will walk through an example of how to use Forge to set up a python jupyter notebook. Before starting, please clone the git repo and complete the[setup steps](https://carsdotcom.github.io/cars-forge/setup.html) .


1. Referring to the[example yaml](https://github.com/carsdotcom/cars-forge/blob/main/examples/env_yaml_example/example.yaml) , create your environment yaml file. For more information about the environment yaml, please refer to[environmental yaml](https://carsdotcom.github.io/cars-forge/environmental_yaml.html) document.
— The excluded_ec2s, secret name, and user data are prefilled with examples. All of the above and be used as is or updated.
— The AWS AZ, AMI, aws_subnet and aws_security_group need to be updated for your environment.
2. I have also uploaded an[example user data script](https://github.com/carsdotcom/cars-forge/blob/main/examples/env_yaml_example/single.sh) to be used as the default user data; this can be changed.
—` echo "$(cat /root/.ssh/authorized_keys | sed 's/^.*ssh-rsa/ssh-rsa/')" > /root/.ssh/authorized_keys` is needed in all user data scripts because Forge runs all commands as root.
3. Run` forge configure -h` . This will print out the location of the Forge config folder. Create a new directory under the config folder.
E.g.` mkdir -p /home/ec2-user/forge/config/example`
4. Move the user data and environment yaml created above to the config/example folder. Your Forge environment is now setup.
E.g.` cp example.yaml single.sh /home/ec2-user/forge/config/example/`
5. In the Git repo, you will find an[example folder](https://github.com/carsdotcom/cars-forge/tree/main/examples/single_example) we will be using for this example. It included the yaml, user data script, and run script. For more information about the user yaml, please see the[yaml](https://carsdotcom.github.io/cars-forge/yaml.html) document.
— The single EC2 instance will have 16 or 32GB of RAM.
— It will only stay up for 3 hours.
— If there is a failure or if you cancel out of jupyter, the instance will be killed.
6. From inside the single_example folder, run` forge create —-yaml single_example.yaml --user_data single_ud.sh` . Once the instance is created the hourly price will be printed.
7. Run` forge rsync —-yaml single_example.yaml`
8. Run` forge run —-yaml single_example.yaml`
9. Once completed, the jupyter URL will be printed. Copy and paste that into your browser and you will be able to run a python notebook.
10. Once done you can run` forge destroy —-yaml single_example.yaml`
11. To run create, rsync, run, and destroy in one command, run` forge engine —-yaml single_example.yaml --user_data single_ud.sh` .


## **Contribute**


Forge is open-source and anyone is welcome to make contributions to the project. If you would like to make a contribution, please read our[Contributor Code of Conduct](https://github.com/carsdotcom/cars-forge/blob/main/docs/code_of_conduct.md) .


## **Links**


GitHub:[https://github.com/carsdotcom/cars-forge](https://github.com/carsdotcom/cars-forge)
Docs:[https://carsdotcom.github.io/cars-forge/](https://carsdotcom.github.io/cars-forge/)


Follow this space for technical content on Elixir and Python Programming Languages. We also have good resources on our Machine Learning & Data Engineering Practices. Also, check out[open positions](https://www.cars.com/careers/) at CARS.
