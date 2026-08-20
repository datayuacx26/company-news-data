---
schema_version: "1.0.0"
document_id: "9ba1e7b1e476f812f5c0754f7dfc04bd15b5f35feb039187fe21c7eeb186b0c3"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2016-10-12-configuration-management-start-testing-saltstack/"
published_at: "2016-10-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:29.470006+00:00"
content_hash: "sha256:d06fd8b43e3b6007a43a0ea1e34b01655e549704a9bde7c1819bd0c6fab1236c"
---

# Configuration management - How to start testing your salt formulas

Configuration management tools have recently gained a lot of popularity. At trivago we use[SaltStack](https://saltstack.com/) to automate our infrastructure. As the complexity of configuration files and formulas is increasing, we need a fast, reliable way to test our changes.


In this post we explain how you can start building your own test setup.


## How it began


In January 2015 we started using salt from[SaltStack](https://saltstack.com/) as configuration management tool for our infrastructure. In the beginning we focused on our web-server provisioning because we had the biggest amount of servers in this area with different setups for applications like our[Hotel Search](https://www.trivago.com/) ,[Hotel Manager](https://www.trivago.com/hotelmanager/) or other services.


After a couple of months, we also moved our development environments away from[Chef](https://www.chef.io/) to[salt](https://saltstack.com/) . Our goal was to use the same technology stack to provision our development- as we do for our production-environment and hence provide developers with a more production-like environment.


By now we provision most of our infrastructure with salt and it became one of our most important tools in the IT operation and software engineering departments. That also means that our internal salt formula repository grew and more and more contributors create pull requests on a daily basis.


*Lines of code written for SaltStack (until September 2016)*


File type Files Blank Comment Code


salt (.sls) 909 3272 1750 23999


Bourne Shell 139 1480 1705 5382


YAML 64 435 1702 2257


PHP 55 1015 1248 5514


Python 33 1002 717 3708


XML 23 238 533 1395


INI 23 563 1655 696


Bourne Again Shell 20 408 253 2292


Perl 17 984 1134 6621


## The intention


Despite using pull requests for all changes and the two-man rule it has happened quite often that our salt master branch (the single source of truth for our internal configuration management) was broken.


Of course: Without any automated tests it’s hard to see every YAML issue and not to mention logical changes with some edge case conditions.


**But it is even harder to recognize quickly that only some of your formulas are broken on some machines!**


After hundreds of pull requests we found that our issues with salt provisioning fall into the following categories:


- YAML syntax
- Wrong top file definitions / conditions
- Jinja issues in template files
- Networking related issues (availability of resources)
- Issues regarding the order in which states are executed
- Logical changes (removing a required service)


But how you can you detect all of these issue types on every pull request? Manual checkouts and real state executions for every feature branch / pull request?


Of course that’s not really feasible. Otherwise the maintainer will stop with their daily business or the pull requests will queue up until the repository will lose the maintainability.


## The first step


At some point you have to start doing something – and every small step is better than nothing. So let’s check salt itself offers in terms of testing:


> [Salt.modules.state.show_highstate](https://docs.saltstack.com/en/latest/ref/modules/all/salt.modules.state.html#salt.modules.state.show_highstate)
> Retrieve the highstate data from the salt master and display it


SaltStack provides a state module named` show_highstate` . If executed, salt will render the highstate data but not execute it.


The big benefit: You don’t need a real target server to execute the state – You can easily simulate a server with a container solution and only handle the result.


So far, so good - Let’s see what possibilities` salt-call` offers:


```text
# salt-call -h
Usage:   salt-call   [options]   <function  >   [arguments]


Options:
[...]
--retcode-passthrough
Exit with the salt call retcode and not the salt
binary retcode.
[...]
Output Options:
Configure your preferred output format.
--out=OUTPUT, --output=OUTPUT
Print the output from the 'salt-call' command using
the specified outputter. The builtins are 'key',
'yaml', 'overstatestage', 'highstate', 'progress',
'pprint', 'txt', 'newline_values_only', 'raw',
'virt_query', 'compact', 'json', 'nested', 'quiet',
'no_return'.
[...]
```


The global` --retcode-passthrough` option from salt can also help us to handle the result and the` --out=OUTPUT` parameter is very useful to wrap the execution with tools or scripts later on.


But what can we detect with this call? Let’s try it out …


1.


First we provide some states:


```text
# `states/dummy/foo.sls`
foo_directory  :
file.directory  :
-   name  :   /foo
```


```text
# `states/dummy/bar.sls`
bar_user_group  :
group.present  :
-   name  :   fooBar
-   gid  :   5001


bar_user  :
user.present  :
-   name  :   fooBar
-   fullname  :   Foo bar
-   gid  :   5001
-   require  :
-   group  :   bar_user_group
```


2.


Second we declare them in the` states/top.sls` for a highstate call and test the call:


```text
# `states/top.sls`
base  :
'roles:blog.dummy'  :
-   match  :   grain
-   dummy.foo
-   dummy.bar
```


3.


Now we check what we get from salt:


```text
salt-call   state.show_highstate   --retcode-passthrough
```


```text
local  :
----------
bar_user  :
----------
__env__  :
base
__sls__  :
dummy.bar
user  :
|  _
----------
name:
fooBar
|  _
----------
fullname:
Foo bar
|  _
----------
gid:
5001
|  _
----------
require:
|_
----------
group:
bar_user_group
-   present
|  _
----------
order:
10002
bar_user_group  :
----------
__env__  :
base
__sls__  :
dummy.bar
group  :
|  _
----------
name:
fooBar
|  _
----------
gid:
5001
-   present
|  _
----------
order:
10001
foo_directory  :
----------
__env__  :
base
__sls__  :
dummy.foo
file  :
|  _
----------
name:
/foo
-   directory
|  _
----------
order:
10000
```


Alright, and now let’s test some issues and see what we can detect by executing` show_highstate` :


1.


YAML syntax issues:


```text
salt-call   state.show_highstate   --retcode-passthrough
[CRITICAL] Rendering SLS   'base:dummy.foo'   failed: mapping values are not allowed here;   line   3


---
foo_directory:
file.directory
-   name:   /foo      <  ======================


---
local:
-   Rendering   SLS   'base:dummy.foo'   failed:   mapping   values   are   not   allowed   here  ;   line   3


---
foo_directory:
file.directory
-   name:   /foo      <  ======================


---


echo   $?
1
```


2.


Wrong top file definitions / conditions


```text
# top file change:
# 'roles:blog.dummy':
#    - match: grainX
#    - dummy.foo
#    - dummy.bar


salt-call   state.show_highstate   --retcode-passthrough
[ERROR   ]   Attempting   to   match   with   unknown   matcher:   grainX
local:
----------


echo   $?
2
```


```text
# top file change:
# 'roles:blog.dummy':
#    - dummy.foo
#    - dummy.bar


salt-call   state.show_highstate   --retcode-passthrough
local:
----------


echo   $?
2
```


```text
# top file change:
# 'roles:blog.dummy':
#    - match: grain
#    - dummy.foo
#       - dummy.bar


salt-call   state.show_highstate   --retcode-passthrough
[ERROR   ] Template was specified incorrectly: False
local:
-   "No matching sls found for 'dummy.foo - dummy.bar' in env 'base'"


echo   $?
1
```


Nice – we are able to detect invalid YAML syntax and various top file definition issues with a simple` salt-call` . The only requirements are the states, a salt installation, and a running salt-minion config.


So now you might be thinking *“uh - that sounds easy”* and you are right - it really is.


## Get ready to test


We also started with this simple check. To run these checks we created a Debian based[Docker container](https://www.docker.com/) with a salt installation. We use the Docker volumes to mount the following directories / files:


- A valid grain file mounted to` /etc/salt/grains`
- A valid masterless minion config mounted to` /etc/salt/minion`
- Our salt formula repository


Here is an example Dockerfile:


```text
# Dockerfile
FROM   debian  :jessie


# see https://docs.saltstack.com/en/latest/topics/installation/debian.html
RUN   echo   'deb   http  ://httpredir.debian.org/debian jessie-backports main' >> /etc/apt/sources.list   \
&& echo 'deb http://httpredir.debian.org/debian stretch main' >> /etc/apt/sources.list   \
&& apt-get update   \
&& apt-get install --no-install-recommends -y python-zmq python-tornado/jessie-backports salt-common/stretch   \
&& apt-get install --no-install-recommends -y salt-minion
&& rm -rf /var/lib/apt/lists/*
```


```text
# build docker container
docker   build   -t   local/example   .


# execute state.show_highstate
docker   run   \
-v   /path/to/your/local/saltstack:/saltstack   \
-v   /path/to/your/local/saltstack/tests/grains:/etc/salt/grains   \
-v   /path/to/your/local/saltstack/tests/minion:/etc/salt/minion   \
local/example   salt-call   state.show_highstate
```


CI tools like[Jenkins](https://jenkins.io/) ,[Concourse](https://concourse.ci/) or[Travis CI](https://travis-ci.org/) have native/plugin support for Docker containers. Because of that it’s usually easy to integrate these kind of tests into your existing testing infrastructure in a small amount of time.


We did that with web hooks for every commit, but it’s also possible to use a time scheduling- or a pull- based approach against the master branch.


Even if you support several operating systems it’s possible to test them with a single docker container. You are able to simulate other operating systems (like FreeBSD or Windows Server) because we don’t actually execute real states.


In our case we set the[salt grain](https://docs.saltstack.com/en/latest/topics/grains/) for the os-family explicitly in our grain file:


```text
# /etc/salt/grains
roles  :
-   foo
-   bar
os_family  :   FreeBSD   # pin os_family
```


## The tip of the iceberg


*”What about the rest of the possible issues mentioned in the beginning”* you might ask:


As I already mentioned, this is only the beginning of testing your salt formulas. With this guide we are now able to detect nearly all syntactical issues. To detect the other issue types like jinja errors, network related problems or logical changes we need a kind of integration test for all available highstate definitions.


This topic is out of scope for now but I will provide a separate blog post where I explain how we realized integration testing later on.


In the meantime have fun playing around with your first test suite and enjoy the feeling that not every small change needs a manual test execution.
