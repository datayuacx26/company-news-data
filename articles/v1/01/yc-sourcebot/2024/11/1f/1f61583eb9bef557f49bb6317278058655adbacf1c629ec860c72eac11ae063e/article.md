---
schema_version: "1.0.0"
document_id: "1f61583eb9bef557f49bb6317278058655adbacf1c629ec860c72eac11ae063e"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-399c01bf3b04"
canonical_url: "https://www.sourcebot.dev/blog/a-better-way-to-search-across-gitlab-projects"
published_at: "2024-11-22T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:49.815269+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:95585b98514067d6939693fdff77d97fce75da372f2bcbebabd9de83cf08d0e7"
---

# A better way to search across GitLab projects

# A better way to search across GitLab projects


Let's say you're a maintainer of` Veloren` (if you actually are - hi!), and you're looking for some function that you know contains` render` in one of your projects. You don't have all of your projects checked out locally, so you search on GitLab:


Unfortunately, as many GitLab users can relate to, the[built-in code search](https://docs.gitlab.com/ee/user/search/) functionality is pretty limited. What we'd like to have is something similar to[GitHub search’s](https://github.com/search) functionality, but for our groups GitLab projects.


That's exactly why we developed Sourcebot: a blazingly fast, powerful, and self-hosted search tool. We can index all our GitLab projects locally on our own machine (so no sensitive information gets sent externally), and search through them all using a single modern web interface.


This is what that same query looks like when using Sourcebot:


# Hello Sourcebot


All we need to do to setup Sourcebot is create a small config that tells it what projects/groups to index.


To get started, we’ll create a folder to store our configs:


```text
mkdir sourcebot_workspace
cd sourcebot_workspace


```


Next, we create an example config:


```text
touch my_config.json
echo '{
"$schema": "https://raw.githubusercontent.com/sourcebot-dev/sourcebot/main/schemas/v2/index.json",
"repos": [
{
"type": "gitlab",
"projects": [
"gitlab-com/www-gitlab-com"
]
}
]
}' > my_config.json


```


This example config will index the` gitlab-com/www-gitlab-com` project.


Now we can run this simple docker command to spin up Sourcebot and point it to our example config:


```text
docker run -p 3000:3000 --rm --name sourcebot -v $(pwd):/data -e CONFIG_PATH=/data/my_config.json docker.sourcebot.dev/sourcebot-dev/sourcebot:latest


```


Here’s Sourcebot in action indexing and searching` gitlab-com/www-gitlab-com` on` localhost:3000` :


# Configuring Sourcebot


The Sourcebot schema is very simple, and can be modified to pull in any repo you have read access to. Sourcebot automatically picks up config changes, so you won’t need to restart it as you go through this section.


## Private projects


If you’d like to pull in a private project, you’ll need to provide a[personal access token](https://gitlab.com/-/user_settings/personal_access_tokens) (PAT). The only permission the PAT needs is` read_api` :


We can provide the PAT when defining the GitLab object in the config:


```text
{
"repos": [
{
"type": "gitlab",
--> "token": "your-gitlab-token",
"projects": [
"gitlab-com/www-gitlab-com"
]
}
]
}


```


## Self-hosted GitLab instances


If you’re self-hosting, you can provide a URL to your GitLab instance:


```text
{
"repos": [
{
"type": "gitlab",
--> "url": "htpps://aperature-labs.gitlab.com",
/**
additional configs
**/
}
]
}


```


## Indexing all visible GitLab projects


NOTE: This is currently only supported for self-hosted GitLab instances


If you just want to index all the GitLab projects visible to your PAT, simply set the` all` field to true:


```text
{
"repos": [
{
"type": "gitlab",
"url": "htpps://aperature-labs.gitlab.com",
"token": "required-pat-token",
--> "all": true,
/**
additional configs
**/
}
]
}


```


## Indexing specific GitLab projects


You can specify GitLab projects, groups, subgroups, and/or users to index. When specifying a group/subgroup/user, Sourcebot will index all their visible projects.


```text
{
"repos": [
{
"type": "gitlab",
"url": "htpps://yoursupercoolinstance.gitlab.com",
"users": [
"gabe-newell"
],
"groups": [
"aperature-labs",
"black-mesa/research-subgroup"
],
"projects": [
"aperature-labs/portal-tech"
]
}
]
}


```


Sourcebot supports several other config options, which you can check out in the[most recent config schema](https://github.com/sourcebot-dev/sourcebot/tree/main/schemas)


# That's it!


One simple config and a docker command later and we’ve got a powerful search interface for all our GitLab projects!


If you have any questions or run into any issues, please feel free to reach out directly:michael@sourcebot.dev
