---
schema_version: "1.0.0"
document_id: "9e2647c439c540714809dd0de59d199c54aa1d112e481ab332f9b4aaca89adcc"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2019-12-20-makefiles-in-2019/"
published_at: "2019-12-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:d6a5364157ca9c1b69e35d4feacf853e012f92fa959e8578b2a2f8b1960bc6a4"
---

# Makefiles in 2019 — Why They Still Matter

### Teaching old tools new tricks


Make was created in 1976 by Stuart Feldman at Bell Labs to help build C programs. But how can this 40+ year old piece of software help us develop and maintain our ever-growing amount of cloud-based microservices?


If you worked long enough in a company that welcomes change, this has probably happened to you too: a tool that you haven’t touched before is now yours to maintain. Either because somebody gave it to you during an organizational restructuring; or you depend on it, but the old developer already left the company; or it was your tool that ran perfectly fine for three years and now it broke suddenly; or because you just wanted to adopt that cute little puppy 🐶 and take it off somebody else’s hands. Or it’s 1 AM 🕐, your product is broken 🧨 and you need to release a fix 🚒. There are plenty of reasons why you now need to make changes to and/or release a project you haven’t touched before. But the problems are usually the same.


## Example


Let’s look at an example:


```text
$ ls
README.md                   inventory_rules/
cf-templates/               resourcemanagement/
Cloudtrail_crawler.service  setup.py
event_rules/                tests/
Inventory_crawler.service   tox.ini
Inventory_crawler.timer
```


Just by looking at it, how would you build this project? If you know a bit about Python packaging you might know of` python3 ./setup.py bdist_wheel` . If you know what` tox` is, then you would probably try to test it with just a simple` tox` . Or you have read the` tox.ini` and found` python3 ./setup.py test` that is used in there. And if you have read the` setup.py` , you’d know that this project uses` pytest` :` python3 -m pytest tests/` . Once we have built and tested our project, how do we install it on a machine? I found this example in one of our Slack channels:


```text
pip3   install   --upgrade   ${PATH_TO_WHEEL}
cp   -r   ./{event,inventory}_rules   /etc/resourcemanagement
install   ./inventory_crawler.timer   /etc/systemd/system/inventory_crawler.timer
install   ./inventory_crawler.service   /etc/systemd/system/inventory_crawler.service
install   ./cloudtrail_crawler.service   /etc/systemd/system/cloudtrail_crawler.service
useradd   -r   resourcemanagement
systemctl   daemon-reload
```


The` cf-templates/` folder also contains a few AWS CloudFormation templates that will help you deploy this project. They will create an EC2 machine that you can run this program on, and create the IAM roles that it needs to do its job. What it doesn’t tell you is in which account and region these should be executed. Good luck with figuring those out.


This is all a lot of guesswork and requires a lot of what my colleague calls “sherlocking” and trial-and-error. How could we avoid that?


## Pitfall I: It’s in the docs


This project already has a` README.md` , so let’s just add all the required steps in a code block there. Then everybody can read and apply them. But that would involve a lot of copy and paste from the README to the command line for medium-sized projects. And it will probably require a handful of adjustments here and there, because the README is not a script for computers, but a manual for humans. Also, the README might become out-of-date if not taken care of, and that’s exactly what happened in the example project above. And it will be especially difficult for irregularly maintained projects, where there is no one person that takes care of updating and releasing the project. After a certain amount of projects with stale READMEs, people will stop reading them altogether.


## Pitfall II: That’s what we have a CI system for


Following best practices, we should add a CI/CD pipeline to the project. Then people just have to merge their branch, and everything else will be taken care of. But often enough it’s simply not possible to have a fully automated CI/CD pipeline because your organization still requires some human approval or other kind of interaction for releases of certain projects. When you have an only partially automated pipeline, you can still share a link to the web-interface in the README. Then every developer can see the state of their builds and releases and could also approve them; which is really nice as long as it works, because you can’t test the pipelines of many CI systems locally or without a commit. You end up having to do a lot of repetitions of` git add . && git commit -m "fixing pipeline" && git push` in case your pipeline fails. And when you’re working on later parts of the pipeline, you’ll have to wait for the build process to reach the point where you made your modification. That’s never fun. You could also[add tests](https://github.com/jenkinsci/JenkinsPipelineUnit) for the pipeline that runs your tests, if you’re into that sort of thing, but it won’t solve this problem.


## Pitfall III: My language’s ecosystem has great tooling


Maybe you write your project in a language that has really good tooling for building, testing, and deploying. Maybe you even picked that language because of its great tooling. Good tooling for a language is great for people who know the language really well, because it allows them to use the ecosystem to its full potential. Newcomers will have a good experience with it because they will be able to find answers on StackOverflow and the tooling is still good overall. But eventually, a new language gets introduced to the project — say we’d like to add a user interface written in JavaScript. You suddenly end up maintaining two separate toolchains within your project, which increases complexity. And then you probably have two languages and their tooling in your project. How will you combine them?


## Pitfall IV: Script hell


You’ll start to use a language agnostic build tool.[CMake](https://cmake.org/) ,[redo](https://redo.readthedocs.io/en/latest/) ,[Gradle](https://gradle.org/) ,[Meson](https://mesonbuild.com/) ,[SCons](https://scons.org/) ,[Ninja](https://ninja-build.org/) ,[ant](https://ant.apache.org/) ,[Bazel](https://bazel.build/) ,[tup](http://gittup.org/tup/) ,[…](https://alternativeto.net/software/gnu-make/) There are so many to choose from. So let’s start with a simple shell script for a fictional PHP and JavaScript project:


```text
#!/bin/bash


composer   install   \\
--no-interaction   \\
--no-dev
yarn   install
yarn   build


tar   czf   "dist/  $1  .tar.gz"   web/
```


This script leverages the domain knowledge of` yarn` and` composer` and combines their output with a simple` tar` . You can use it without much prior knowledge of either` yarn` or` composer` . But there are some shortcomings: it doesn’t run the tests, there’s no error handling, and it fails in weird ways when` $1` is empty. Let’s try to address that:


```text
#!/bin/bash
set   -ex


if   [   -n   "  $1  "   ];   then
echo   "PACKAGE_ID missing"
exit   1
else
PACKAGE_ID  =  "  $1  "
fi


composer   install   \\
--no-interaction   \\
--no-dev
composer   test
yarn   install
yarn   build
yarn   test


tar   czf   "dist/  $PACKAGE_ID  .tar.gz"   \\
web/   \\
vendor/
```


This is just a tad verbose with 17 lines for basically 6 commands. It fixes all previous issues in a decent manner. If you just want to run the tests without knowing about` yarn` or` composer` , you will reinstall all` node_modules` every single time you execute the script. You might also add a` ./test.sh` next to your` ./build.sh` . And a ./release.sh and a new script for every use-case you can come up with. In the end you’ll have a bunch of scripts for your frontend devs, some for your backend devs, some for your DevOps engineers, and a few for your CI system for good measure. Sooner or later you will want to centralize your collection of shell scripts among most of your projects, and you will end up with The Legacy Release Process and nobody knows if a fix for one project is going to break the pipeline of some other project.


## Makefiles to the rescue


Is there a better alternative? Well, let’s look at a simple Makefile for the same project as our` ./build.sh` :


```text
test  : dependencies
composer test
yarn test


# alias for the actual Artifact
build  : dist/my_project.tar.gz


dist/my_project.tar.gz  : dependencies
yarn build
mkdir -p dist/
tar czf   $@   web/ vendor/


dependencies  :
composer install --no-interaction --no-dev
yarn install


.PHONY  : test build dependencies
```


This Makefile has four targets you can invoke:` test` ,` build` (which is an alias for` dist/my_project.tar.gz` ), and` dependencies` . And you as a human can actually skim over this and see which targets are available to you. To add back the` PACKAGE_ID` functionality, you can use this little snippet:


```text
# alias for the actual Artifact
build  : dist/  ${  PACKAGE_ID  }  .tar.gz


dist/  ${  PACKAGE_ID  }  .tar.gz  : dependencies env-PACKAGE_ID
yarn build
mkdir -p dist/
tar czf   $@   web/ vendor/


# guard against being called without parameters or environment variables
env-  %  :
@  test -n "  ${  $*  }  " || \  \
(echo "Argument   $*   not set"; exit 1)
```


Instead of forcing your users to supply a` PACKAGE_ID` , you could also generate one in the Makefile with` git` :


```text
VERSION := $(shell git describe --tags --always --dirty)
PACKAGE_ID := "my_package-${VERSION}"
```


With this at the top of your Makefile you can remove the` env-PACKAGE_ID` dependency, because if one isn’t provided to` make` , it will automatically be set with the last` git` commit hash of your repository.


Just testing and building your application doesn’t deploy it though. You might use Docker and Kubernetes for that, so you need to build and push an image and deploy a manifest. Because we want to be able to see which version is currently live, we will specify a concrete image tag in our manifest, so we’ll “template” that file as well.


```text
DOCKER_REGISTRY := "registry.hub.docker.com/"
DOCKER_NAME := "me/my-project:  ${  VERSION  }  "


docker-image  : dist/  ${  PACKAGE_ID  }  .tar.gz
docker build \  \
--build-arg PACKAGE=  $<   \  \
-t "  ${  DOCKER_REGISTRY  }${  DOCKER_NAME  }  " \  \
./


push  : docker-image
docker push "  ${  DOCKER_REGISTRY  }${  DOCKER_NAME  }  "


deploy  : k8s/manifest.yaml push
sed 's#my-project:VERSION#  ${  DOCKER_NAME  }  #' <   $<   \  \
| kubectl apply -f -
```


When you run` make deploy` ,` make` will build your package, build the image and push it, template your Kubernetes manifest, and then apply it in your current context. Make itself will just ensure there is a` *.tar` , and then always invoke` docker build` , leveraging Docker’s build cache. We also want the templating to always happen, because even though the template file might not change, the version we template will. But since one` sed` invocation is rather quick, it won’t hurt us.


With this Makefile, everything you have to do in your CI/CD pipeline is to run` make test deploy` . If there are tests that fail in the pipeline, the developers can just run` make test` and execute the exact same steps as your CI system. And the DevOps engineers don’t have to guess and push potential fixes just to learn that their assumptions were wrong and try again. They can just fix it on their local machine, commit and push once, and be done after one successful build. This approach has made the life of our Release Engineering team (which I am part of) a whole lot easier.


## Why Makefiles are great


We chose` make` , and you should too, over other agnostic build tools because of its rather simple language and its ubiquitous availability. Despite its shortcomings (` .PHONY` , only being able to compare timestamps of files, etc.), which I omitted here on purpose, it’s still the 20% solution for 80% of our problems. When there is a problem that can’t easily be solved by` make` , we can still use it to start a program or shell script that encapsulates that complexity.


The main advantage of this approach is that it is the documentation of your build and release process as code, which is not only used regularly by your build system, but also by your developers and DevOps. This means that when your documentation breaks, people will notice and fix it. No more trial and error when onboarding new developers. And at 1 AM you can release your new version without fighting with your CI system and just do that the next day.
