---
schema_version: "1.0.0"
document_id: "a32ee4e0d7df8343f56e22e4b723d8c2b98aa0b2855d3dbee6719c3ee6beed78"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/packaging-pytorch-in-docker"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:5dda5fae5dae711605532dbd1bf68dbf24879eba3f8d3d11b2f0ba9fc3a92806"
---

# Packaging your PyTorch project in Docker

Have you ever struggled setting up your deep learning project on a new machine? In this article, we will discover how to package your project inside Docker. You **won't even need to install CUDA and cuDNN** ! Thanks to[NVIDIA-docker](https://github.com/NVIDIA/nvidia-docker) , you can harness the power of your machine's GPU at **no[performance cost](https://github.com/NVIDIA/nvidia-docker/wiki#does-it-have-a-performance-impact-on-my-gpu-workload)** . Training and inference will be as fast as on your machine ✨🚤


With **2 simple commands** , you'll be able to run your project inside a Docker container. It works on any machine, as long as Docker and Nvidia-Docker are installed. No more time spent meddling with requirements or obscure dependencies.


Here is a small summary of how the project will look like:


*(This article assumes the use of GPU, but it is entirely optional)*


## Setting up


Follow the[instructions here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker) to install NVIDIA-docker (it takes a few minutes). Then, create a file named` Dockerfile` at the root of your project. In it, copy and paste the following:


```text
FROM pytorch/pytorch:1.9.0-cuda10.2-cudnn7-runtime


COPY ./requirements.txt /install/requirements.txt
RUN pip install -r /install/requirements.txt


WORKDIR /code
```


### Only two commands to know:` build` and` run`


Go in your project directory, and type:


```text
sudo docker build -t my-project .
```


(Note the dot at the end). It will download all the dependencies specified in the` Dockerfile` you just created.


When it is ready, run the following command:


```text
sudo docker run -it --gpus "device=1" -v $(pwd):/code my-project:latest python3 train.py
```


### Troubleshooting


You may run into an error. It can be that a version is not set correctly or that a dependency is missing. For instance, attempting to` import cv2` will yield the following error:


```text
ImportError: libGL.so.1: cannot open shared object file: No such file or directory
```


Thankfully, **this issue is easy to resolve** . Update the command above to the following one:


```text
sudo docker run -it --gpus "device=1" -v $(pwd):/code my-project:latest bash
```


This **opens a shell inside the docker container** . Now that we are running inside it run


```text
python3 train.py
```


Use your extensive Google-fu to find out the missing dependency and install it. Note down what you are typing, as we will migrate those precious commands to the` Dockerfile` .


Update the` Dockerfile` to add the missing dependencies. Prefix any command with RUN. For instance, if you'd like to add support for OpenCV, the best way is to let` apt install` all dependencies by adding the following line in 2nd:


```text
RUN apt-get update && apt-get install -y python3-opencv
```


Now, we are ready for one last` docker build .` and we are good to go! Type the` run` command from above, and your training should start right away 🚀


## Tips and other useful commands


-


The command` docker build .` will attempt to copy all files in your directory to create a "Context". This might be slow if you have a lot of data in this folder. Therefore we recommend creating a` .dockerignore` file with your models and other large files.


-


Docker can leave stale containers hanging on your disk. Run` sudo docker system prune -a` if too much disk space is used.


-


Each line in the Dockerfile has its own cache. It means that running` docker build` a second time is usually much faster if you did not change many lines


-


One benefit is that you don't need to install CUDA or cuDNN on the host system. NVIDIA drivers for your GPU still need to be installed
