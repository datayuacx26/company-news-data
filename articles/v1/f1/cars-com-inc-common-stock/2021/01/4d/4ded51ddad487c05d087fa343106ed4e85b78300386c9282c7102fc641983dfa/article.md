---
schema_version: "1.0.0"
document_id: "4ded51ddad487c05d087fa343106ed4e85b78300386c9282c7102fc641983dfa"
company_key: "cars-com-inc-common-stock"
company: "Cars.com Inc."
source_id: "cars-com-inc-common-stock-rss-49a4db916ec1"
canonical_url: "https://tech.cars.com/teaching-an-elephant-to-fly-6820f68e5fdd"
published_at: "2021-01-28T20:37:20+00:00"
first_seen_at: "2026-07-20T23:17:02.597310+00:00"
fetched_at: "2026-07-28T22:26:37.862197+00:00"
content_hash: "sha256:fcad934d936dc11e96583de127b13ffc76069ec3e283d5b261c3d339a9591a75"
---

# Teaching an Elephant to Fly

# Teaching an Elephant to Fly


[Nikhil Patel](https://medium.com/@nikhil530?source=post_page---byline--6820f68e5fdd---------------------------------------)


6 min read


·


Jan 28, 2021


--


Press enter or click to view image in full size


Photo by[Nam Anh](https://unsplash.com/@bepnamanh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on[Unsplash](https://unsplash.com/s/photos/elephant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)


In early 2019, Cars.com’s data team started its journey from on-prem to Amazon Web Services (AWS). Being a hadoop administrator, I was tasked with creating a replacement for our on-prem cluster. There were two main functions we needed. First was to allow developers and data scientists to develop freely, mostly with jupyter. Second was to run production spark and docker code daily.


An easy lift and shift solution would be AWS Elastic Mapreduce (EMR), but there are some drawbacks. The cluster would need enough memory and CPU for all developers to use. Size it too small and some developers will not get enough resources. Size it too large and the company is wasting money. Adding packages for python is not simple. The package needs to be installed during cluster initialization. If a new package is needed, the cluster would need to be restarted. There must be a better solution since we were moving to the *infinite* cloud.


### Our Model T


Press enter or click to view image in full size


Photo by[The New York Public Library](https://unsplash.com/@nypl?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on[Unsplash](https://unsplash.com/s/photos/model-t?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)


The solution was to allow each developer and production job to spin up its own cluster. We debated using EMR but ultimately decided to utilize spot AWS Elastic Compute Cloud (EC2)s. It spins up quicker, gives more flexibility, and costs less. I started my journey to create this project with very limited python, AWS, and terraform experience. This is where the power of teamwork at CARS really shines. Through all my questions and walls I hit, there was always a co-worker willing to help. Without them, this project would have been dead before it started.


Since other teams at CARS started their move to the cloud before us, it was highly suggested that all infrastructure be done in terraform. The plan was to have multiple folders, each with a different sized EC2. For example, to get a spot instance using docker with 128GB of memory, you would use the docker_128 folder. If you wanted a spark cluster, you would use the master_8 and slave_large folder.


I first started to develop the needed AMIs. The docker AMI was easy to create. I ran into a few issues installing spark on EC2. Most notable was getting the correct jars to allow spark to work with AWS. Once the AMIs were created, I moved on to the terraform scripts.


Being new to terraform, this process took time to develop. After a few iterations, I was able to get a working script. I started adding variables slowly so it can be customized by the developer. For instance, they can change the EBS volume and how long the spot fleet would live for. After a few months of effort, the process was ready to be used.


A wrapper script was created for developers to quickly spin up a jupyter notebook. All they needed was to enter their name and variables if defaults didn’t apply.


```text
Spin up a spot instance, running for at most 18 hours, with 32 GB of RAM, 64 GB of disk, and 4 cores running the default jupyter packages:    ./start_docker.sh -v 18 npatel  Spin up a spot instance with 128 GB of RAM, 64 GB of disk, and 32 cores running a custom image from ECR:    ./start_docker.sh -m 128 -c 32 -d jupyter-dev npatel  Spin up a gpu-based instance running a custom tensorflow-based docker image from ECR:    ./start_docker.sh -g -d nlp-dev npatel
```


Within 5–7 minutes, the developer was presented with a link to their jupyter notebook. An AWS Elastic File System (EFS) was mounted on the instance at spin-up so that, if the spot instance was lost, their work would be saved on the EFS. For each batch production job, a wrapper script would be written to use the terraform scripts. They would change directory to the correct folder, run the terraform, rsync the files to the EC2, then run the scripts, and finally tear down the spot fleet.


```text
#starting master  cd terraform/spot/${MASTER}/${ENV}  echo "Running terraform init for master"  terraform init  terraform workspace new ${NAME}  terraform workspace select ${NAME}  terraform init  echo "Running terraform apply for master"  terraform apply ${OPTIONS} -var pip="${PIP}" -auto-approve  #starting worker  cd terraform/spot/${WORKER}/${ENV}  echo "Running terraform init for master"  terraform init  terraform workspace new ${NAME}  terraform workspace select ${NAME}  terraform init  echo "Running terraform apply for master"  terraform apply ${OPTIONS} -var pip="${PIP}" -auto-approve  #get ip, rsync and ssh  check master  echo "Run RSYNC"  rsync -rave "ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i ~/${PEM}" ${APP_DIR}/* ec2-user@${IP}:/home/ec2-user/  echo "Run Spark job"  ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i ~/${PEM} ec2-user@${IP} /home/ec2-user/scripts/${SSH} ${IP}  #successful run; destroy with terraform  copyLogs  destroy  exit 0
```


### Not so Fast


Press enter or click to view image in full size


Photo by[henry perks](https://unsplash.com/@hjkp?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on[Unsplash](https://unsplash.com/s/photos/fast-car?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)


As a first iteration, the project was adequate but far from perfect. The wrapper script required over 200 lines of code to spin up a spark cluster or docker instance, the majority of which didn’t change between projects. It was confusing, ugly, and had a few bugs. If there was a change to the terraform code, all the projects would need to be updated. We did a minor AWS Command Line Interface (CLI) package upgrade that broke the code. One by one, all of our scripts needed to be updated.


## Get Nikhil Patel’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Creating the terraform scripts was also a long and manual process. I created terraform scripts to capture most of the use cases like instances with 128GB, 32GB, or 64GB of memory. If there wasn’t a script for a use case, I would have to manually create it. I first gathered information such as spot prices, CPU, memory, and frequency of loss in Excel. I removed instances with a high chance of loss and high prices. Finally, I would enter these instances into the terraform scripts.


We faced some minor issues including running out of terraform workspaces and occasionally losing spot instances. The project served us well for over a year, but it was time to come up with a better solution with all these problems piling up. At this point, I was more familiar with AWS and python and decided to forgo terraform for python.


### Forging ahead


```text
usage: forge [-h] [--date DATE] [--env ENV]               {ecu,create,destroy,rsync,run,engine} ...  positional arguments:    {ecu,create,destroy,rsync,run,engine}                          job for forge to do      ecu                 create csv with ec2 specs      create              create ec2 spot fleet      destroy             destroy ec2 spot fleet      rsync               rsync scripts to spot ec2      run                 run scripts on spot ec2      engine              create, rsync, run, and destroy  optional arguments:    -h, --help            show this help message and exit    --date DATE           date, default yesterday    --env ENV             environment
```


I created a python package that does all the work of setting up a working instance/cluster with minimal boilerplate. Instead of over 200 lines of code, each job would need just a config file and one command. The project has defaults, but all of them can be overwritten in the config file. It also has job-specific items such as the name, quantity of resources needed, version of spark, instructions to run the job, etc. This project will then take the config file, spin up the EC2, rsync the scripts over, run the scripts, and terminate the spot fleet all with one command.


EC2s are automatically chosen by the project. It gathers all the EC2 information into a data frame. Rather than doing the process manually, it finds the right instances based on resource needs and price. The selected instances are used with boto3 to create a spot fleet.


```text
forge engine --yaml config/docker.yaml
```


After many challenges and hours developing, we are very excited to replace the old process. The project is still in development and is currently being tested by our developers, please stay tuned for more information.


Press enter or click to view image in full size
