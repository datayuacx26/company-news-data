---
schema_version: "1.0.0"
document_id: "ab2641ed190e34dfb983a0fa66b7e3f79f922e50b02111a3220273af412d554b"
company_key: "yc-evidence"
company: "Evidence"
source_id: "yc-evidence-news-import-47bf0dc75044"
canonical_url: "https://evidence.dev/blog/hugging-face-spaces"
published_at: "2024-07-31T00:00:00+00:00"
first_seen_at: "2026-07-21T19:04:43.661863+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:b8e5acb98939d340619fa7baf71df1c07eeaf160f171232aab667f28da906568"
---

# Deploy Evidence on Hugging Face Spaces

# Deploy Evidence on Hugging Face Spaces


*Deploy your Evidence app on Hugging Face Spaces by combining two Spaces: one to store the source code, and one to host the Evidence App as a static website*


[Archie Wood Jul 31st, 2024 · 3 min read](https://www.linkedin.com/in/archiesarrewood/)


## Hugging Face 🤗?


[Hugging Face](https://huggingface.co/) is a popular platform for sharing machine learning models and datasets.


Hugging Face *Spaces* is a feature that allows you to deploy applications. It’s a great way to host static sites, and it’s free!


Here you can see the finished product:


- [The Hosting Space](https://huggingface.co/spaces/evidence-dev/template-deployed) with the Evidence app ([see without Hugging Face header](https://evidence-dev-template-deployed.static.hf.space/) )
- [The Source Code Space](https://huggingface.co/spaces/evidence-dev/template/tree/main) with the Evidence source code and build process


## How to Create and Host an Evidence App on Huggingface Spaces


### Step 1: Create a Space for the Source Code


First, create a Huggingface Space to store your code and build the app. For this one, use the Docker SDK, and select a blank template. This will be the “Source Code Space”.


### Step 2: Get the Source Code


We need to get our Evidence source code into the Space. Huggingface Spaces are just git repositories, so we start by cloning the repo.


With the local copy of the repo, put an Evidence project inside it.


```text
git   clone https://huggingface.co/spaces/ [  your-username ]  / [  your-repo ]
cd    [  your-repo ]
npx degit evidence-dev/template  --force    .
```


Use the` --force` flag because the directory already contains some Huggingface files. Once we push these files to git, our source control setup is complete!


### Step 3: Set Up the Builder with Docker


Evidence is deployed as a static site, meaning the source code is built into HTML, CSS, and JS. We need something to run this process, and that’s where Docker comes in.


Add this Dockerfile at the root of your project and push it to git.


```text
FROM   ubuntu:22.04


# Install Node.js 20 - https://github.com/nodesource/distributions?tab=readme-ov-file#installation-instructions-deb
# And python3
RUN   apt update
&& apt install -y curl python3 python3-pip
&& rm -rf /var/lib/apt/lists/*


RUN   curl -fsSL https://deb.nodesource.com/setup_20.x -o nodesource_setup.sh
&& bash nodesource_setup.sh
&& apt update
&& apt install -y nodejs
&& rm -rf /var/lib/apt/lists/*
&& rm nodesource_setup.sh


RUN   pip install --upgrade  "huggingface_hub[cli]"


# Build the app
WORKDIR   /usr/app
COPY   ./ /usr/app
RUN   npm install
RUN   npm run sources && npm run build


# Upload the app


## The site space name must be passed as an environment variable
## https://huggingface.co/docs/hub/spaces-sdks-docker#buildtime
ARG   STATIC_SPACE
## The Hugging Face token must be passed as a secret (https://huggingface.co/docs/hub/spaces-sdks-docker#buildtime)
## 1. get README.md from the site space
RUN    --mount  =  type=secret,id=HF_TOKEN,mode=0444,required=true
huggingface-cli download --token=$(cat /run/secrets/HF_TOKEN) --repo-type=space --local-dir=/usr/app/build  $STATIC_SPACE   README.md && rm -rf /usr/app/build/.cache
## 2. upload the new build to the site space, including README.md
RUN    --mount  =  type=secret,id=HF_TOKEN,mode=0444,required=true
huggingface-cli upload --token=$(cat /run/secrets/HF_TOKEN) --repo-type=space  $STATIC_SPACE   /usr/app/build . --delete  "*"


## Halt execution because the code space is not meant to run.
CMD   [ "true"  ]
```


This Dockerfile does the following:


- Installs Node.js, Python, and the Hugging Face CLI
- Builds the app
- Uploads the app to the other “Hosting Space” (more on this later!)


### Step 4: Configure Credentials


Go into your Source Code Space and observe a build attempting to run. It’ll fail because we need to add two environment variables for the Dockerfile:


- **STATIC_SPACE** : Name of your static Space
- **HF_TOKEN** : Token with write access to the static Space


Add these via the[Space settings](https://huggingface.co/docs/hub/en/spaces-overview#managing-secrets)


### Step 5: Prepare the Hosting Space


Now, create a second Huggingface Space as a static site host. This Space will contain the built Evidence app, which we’ll refer to as the “Hosting Space”.


Choose the “Static SDK” and select a blank template.


### Step 6: Try a Build


If it doesn’t start automatically, you can trigger this by clicking “Factory Rebuild” in the Space settings.


If you have a small Evidence app, expect this to take about a minute.


N.B. The Souce Code Space will say it has a` Runtime Error` when it finishes building, but that’s just because we kill the Docker container at the end of the build, and we don’t actually use this Space to *run* anything once the build is complete.


Your browser does not support the video tag.


### Step 7: Check the Published App


Go look at the published static app in your Hosting Space. It should be live!


Your browser does not support the video tag.


### Step 8: Update the App


To change the app, simply make a change to your local copy of the source code repo and push it. Your app rebuilds automatically!


Your browser does not support the video tag.


### Step 9: View Without Header


You can also view the app without the Huggingface header by going to “Embed this Space” and clicking on the “Direct URL” link.


Your browser does not support the video tag.


### Conclusion


That’s it! You’ve successfully deployed an Evidence App on Hugging Face Spaces.


See the[demo app on Hugging Face →](https://huggingface.co/spaces/evidence-dev/template-deployed) .
