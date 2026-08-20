---
schema_version: "1.0.0"
document_id: "b2bd5d672d659a429f3b74756b10ecbb4239038d712664b8ec453182121187fc"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2017-10-16-boerewors/"
published_at: "2017-10-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:884e69bf6ddb5043e879fc0e7dda329a2fd2fd850b51cc1f96ef345da3411d5c"
---

# How we got rid of 5k lines of our bash release process

When I joined trivago a year ago, we had problems with our releases. The traffic was increasing each day. When we put the server back into the load balancer without warming up the OPcache it would die. From time to time the warmup failed silently. Our DCO (data center operations) crew had to log into the servers and restart a few processes manually. During this time every release was very intense.


My first task was to write a new warmup script that would test if the warmup was actually successful. The warmup is a simple curl call to the machine itself.


```text
# create a symlink to the new version
ln   -snf   "{remote_directory}/{package}/{app_php_path}"   "{warmup_webroot_path}/{next_release_link_name}"


# Give the server 1 second to realize the symlink
sleep   1


# curl the new version to warmup the opcache.
time   /usr/local/bin/curl --silent --show-error --connect-timeout 5 --max-time 30 -I \
{  warmup_headers_str}   http://{frontend_ip}/{warmup_url}


rm   -f   "{release_path}/{next_release_link_name}"
```


If it took less than a second or more than five something was fishy and needed a deeper manual investigation. Someone from DCO had to log into each problematic web server, place a dummy check_opcache.php file in the web root, curl it, and see if the files of the new release are in fact in the cache. Short timing would also happen when it was already warmed up. But if for whatever reason the OPcache was empty a simple restart of the Apache process and a second warmup attempt fixed the problem.


## Our historic Release Process


This big bash beast grew historically. It was running stable for a long time and brought the company where it is now. The SOP (software operations) crew was the owner and responsible for releases. But they had many more responsibilities. They took care of our Redis servers, internal package repositories, git server, our[Gollum](https://github.com/trivago/gollum) instances, and development. If any of those systems had an issue they had to drop everything and start firefighting.


Nobody had time to refactor the bash code or even wanted to touch it. It ran stable in the state it was in but it also was very fragile. With the problems of the warmup, there was a big need to automate the process and detect if the warmup was fine. It became clear, that the current structure was not ideal and a new team was created. I was the first external hire for this team. I got the responsibility to rewrite the bash tool from scratch. Some people felt sorry for what lied ahead of me, but I was so happy :). It was a tough task but also gave me a lot of freedom how to solve it.


## Pain Points of the Legacy Deploy Tool Chain


- The code path was not obvious
- Complex UI with many required inputs without validation (you could deploy a wrong application to the wrong server and path and shut down another app without a prior warning)
- Big monolith with strong coupling
- Hard to change
- Impossible to test changes, other than in production
- Took usually 1-3 hotfixes after a change (didn’t occur often since nobody wanted to touch it and the current state worked)
- This stopped creativity and the will to improve it


## Goal of the New Tool


When we planned to change or replace a more or less stable, running system we needed a good reason for it. Since we ran into the above- mentioned problems with the warmup, that required big changes anyway, we could start our work. So we had to decide to patch the old system or create a new one that is maintainable. The estimation was that we would need almost the same effort for both outcomes. The new tool would be less risky and at the same time more fun. Here are the goals we had in mind when we planned the software sorted by their importance:


1. Detect if the warmup was successful
2. Support the old config files. (We wanted to run both versions side by side until everything works.)
3. Full test coverage


- Test on stage, use exactly the same code for stage and live deploys
- Extensive unit tests
- Extensive integration tests


## Early Work, Implementing Warmup (boerewors)


Python is great language for rapid development, and on the same time it is easy to write code that is easy to read and therefore to maintain. This was the time when the name was chosen. It was clear to solve this with Python and it had to something with warmup. This immediately reminded me of a typical dish for a[braai](https://en.wikipedia.org/wiki/Braai) in Namibia🇳🇦, the[boerewors](https://en.wikipedia.org/wiki/Boerewors) .


It was not easy to understand everything in the code. There were dead code, functions with the same name that did similar jobs. Most variables where globally active and seldom defined in the same file, but modified in many places. One function had a bug and was rewritten and used for all new code but the old one where still using the bugged one. Nobody dared to fix it since it was not obvious what a change will impact. This small script evolved over time to a software and was not maintainable anymore.


Luckily you don’t need to understand everything to rewrite this code. You only need to learn the intentions. I was even encouraged to look as little as possible at the old code so that I could solve the problems with a open mind. I’m so happy that we got the support to plan this task carefully and create a proper software with extensive unit and integration tests and not just the next version of a broken legacy release system.


To be able to test the software before using it live we needed a parity between our stage and live system. The scripts that were used for stage deployments were separated. The warmup, for example was only used for live deployments. At least for the warmup part, we created one script that would run on stage and on live. After about 6 weeks we were confident that the new warmup was working. :| When we activated the new warmup live I was really nervous. I knew how much money we would lose if it killed the web servers and we would have to go offline for a short time.


When we ran the warmup it instantly failed. My heart rate increased drastically and it didn’t help that I was still in the probation period. It didn’t cause an outage, but release was blocked until this issue was fixed. I also feared that this accident will burn the trust other teams put into us.


The source of the error was found in a fast manner. Our web servers have two IP addresses: one front end and one back end. We can only connect via ssh to the back end but the Apache listens only on the front end. In our stage setup, Apache listens on both IPs. The fix was created fast and the warmup ran.


What I learned from this is, that at trivago we don’t search for a guilty person when something bad happens. We try to learn from accidents. This gave me a lot of trust in the colleagues and the company.


## The Next Steps


The warmup was running fine, but there were a few cases when it indicated a failure for no obvious reason. I couldn’t explain it. I couldn’t keep my hand in the fire that the new warmup job is right. So we ran the old warmup job again. The warmup times indicated that the warmup was fine and the Jenkins job itself didn’t fail. We went on with the release and some servers crashed again.


This incident increased my trust in the new software. We needed to improve the logs a lot to make it immediately clear what was the reason to fail the job and give proper instructions on how to fix it. We collaborated on the rest of the deploy toolchain. We built the tools for package distribution, activation, and rollback in parallel.


This created a lot of code duplication. Every task needed to read the config file, parse it, find the necessary servers, have a main loop to connect to each server and execute the actual job there. The advantage was that if someone changed one tool he needed only to test this tool and they didn’t need to be afraid to break something else.


On the other hand, if we found a bug or security issue, we had to touch multiple files in order to fix the same problem on different locations. We decided to refactor the code and create one instance that handles the config parsing and the main loop. Each job describes only what it wants to do on the server.


Now we can trust the main loop part even more because it is battle tested by all other jobs. And we are also sure that this part is fine even for tools we rarely use, like the rollback.


Another advantage was that an improvement will affect all tools. I got the chance to invest some time to rewrite the main loop and add asynchronous executions. This resulted in a speed improvement of a factor of 7.


## How we Managed the Switch to the New Tool


Before we could try a completely new code base on our live servers that could cause a serious outage, we needed to trust it first. This was only possible by using exactly the same deploy code for our stage and live environment. We had to accept that the deploy process is part of our software and treat changes like other changes in the apps. First, test them on stage and if everything is fine there roll it out live.


For our live deploys, we had to connect to an admin server in each data center and deploy from there to the web servers. On stage, we could reach the web server from our Jenkins. So nobody bothered to create an admin server for our only stage testing server. Live we have many web servers behind a load balancer. On stage, we had just one server. This meant we couldn’t detect bugs that occur when you hit different servers during one session. We used a CDN for our assets live but delivered them from the same server in stage. While we were on it we tried to remove all those differences and copied the structure of our live systems. Of course, we didn’t set up a lot of new bare metal servers like we had live but spawned a bunch of VMs in our[Open Nebula cluster](https://tech.trivago.com/tags/opennebula/) .


With those changes in place we could test a lot of new features in our main application:


- If they are truly RESTful and work with switching web server
- Asset loading
- HTTPS redirects


We spawned new web servers that were only deployed by the new tools that nobody used while the old scripts were used like before. Almost nobody was blocked because of the new tools. Every time we encountered errors on a stage deploy we updated our unit tests or integration tests to make sure this kind of error will not happen again. In the end, we had an extensive integration test suite that we ran before accepting a merge request. We used[rumi](https://github.com/trivago/rumi) to spin up a few Docker containers and tested the following steps:


- Fresh distribution
- Warmup
- Activation
- Distribution of the same package again
- Distribution of another package
- Warmup
- Activation on a subset of servers
- Activation on the remaining servers
- Repeated activation
- Warmup last activated package
- Rollback


After each step, we made sure that the symlinks were as expected and that the packages were downloaded and extracted to the right path. We also mocked our Artifactory where the packages are usually stored. We could run those tests even without a working internet connection after the Docker images were downloaded.


## Some Implementation Details


The first challenge was to read the original config files with our new script. Previously we used shell scripts, which created global variables. My first try was to write some regular expressions and parse it myself.


Every time I could parse a live config file I tried to run a script to parse all. I found a lot of inconsistencies. Some had double quotes for strings, other single quotes, some even didn’t use quotes at all. When I saw this expression I stopped with this approach and searched for an alternative:


```text
WEB_SERVERS  =  (  "webserver1"   "webserver2"  )
SERVERS  =  (  "${  WEB_SERVERS  [@]}"  )
```


Luckily there is the` set` command that prints all variables from the current scope. To get rid of all global environment variables we need to gather the output of` set` . When we run` <config_file>;set` we can omit everything that occurred also in the first run. The best part was that I had a canonical representation of the variables. This simplified the parser a lot.


```text
# to get the same set of env variables, we need to execute also multiple statements in one line
default_env: check_output_bourne_shell(  "true;set"  )


config_data_list:   list  (_load_config_file(  self  .filename))
config_data_list.append(  "set"  )
# we join the lines with ; so the the BASH_EXECUTION_STRING will not contain newlines
config_env_list: check_output_bourne_shell(  ";"  .join(config_data_list)).splitlines()
config:   dict  (_get_dict_tuples(l)   for   l   in   config_env_list
if   l   not   in   default_env   and   not   l.startswith(  'BASH_EXECUTION_STRING'  ))
return   config
```


This is a way to read config files written in bash. I strongly suggest to avoid this if you can, and otherwise, you have to make sure to load only trusted config files. They are executed with the rights of the current process and could do all nasty stuff that you can run with bash.


## How to Distribute the Boerewors Binaries


We had a few requirements for the distribution of the deploy tools:


- Rollbacks should be easy.
- It has to run on FreeBSD and Debian.
- It should be installable without the root user.
- It should work in all our data centers around the world.


We looked into a lot of options, but none of them seemed to be a good fit. We couldn’t use the public *PyPi* since some parts couldn’t be open sourced. A private package index, on the other hand, would mean we have to maintain a new critical service. Building a Debian package, like we did in the past, was not an option since it needed to run mainly on FreeBSD. It would also require root privileges to install those.


We didn’t use any solution where we needed to check out a git repository because most data centers had a bad connection to our git server. Since *pip* and *virtualenv* where not installed on any of our servers yet and new software could only be installed by another team I tried to avoid it and only ask when I have a working demonstration. I didn’t want to require *virtualenv* to run boerewors. It should be easy to start them also manually. No` source ./env/bin/activate && boerewors` nor some *virtualenv* enable magic in the Python scripts.


I took a quick look at *[pants](http://docs.pantspowered.org/en/latest/)* and *[pex](https://github.com/pantsbuild/pex)* , but they were quite complex and needed either PyPi or a private package index with a proxy for public dependencies.


Luckily I remembered that[youtube-dl](https://github.com/rg3/youtube-dl/blob/master/Makefile) created a fake binary package for Python code. I took a look at their build script and created a similar one for us. It is a zip of all modules, a` __main__.py` as an entry point and a shebang. Here is an example how we did it:


```text
zip   -r   dist/tmp.zip   trivago   -x   \*  .pyc   >   /dev/null
zip   -r   dist/tmp.zip   vendor   -x   \*  .pyc   >   /dev/null
zip   dist/tmp.zip   __main__.py
cat   dist/tmp.zip   >>   "dist/boerewors_main"
chmod   +x   "dist/boerewors_main"
rm   dist/tmp.zip
```


To be able to run the exact same build with either Python 2 and Python 3 we added some lines to our` __main__.py` entry point. We can now ship Python 2 and Python 3 only code and make it available depending on the current Python version.


```text
from   __future__   import   absolute_import
import   sys
import   os
sys.path.insert(  0  , os.path.abspath(os.path.join(os.path.dirname(  __file__  ),   'vendor'  ,   'pyall'  )))
if   sys.version_info.major  :=   2  :
sys.path.insert(  0  , os.path.abspath(os.path.join(os.path.dirname(  __file__  ),   'vendor'  ,   'py2'  )))
elif   sys.version_info.major  :=   3  :
sys.path.insert(  0  , os.path.abspath(os.path.join(os.path.dirname(  __file__  ),   'vendor'  ,   'py3'  )))
from   trivago.command_line   import   main
main()
```


## Final Thoughts


It is definitely possible to change big code parts if you have proper tests. And you should do it if the code smells are very intense and nobody wants to change them. Now that we have done it, and the main issue was fixed we could improve the overall performance a lot. Right now the main release is five times faster as it was before. I am also very happy that I had the opportunity to tackle this task and that I could learn a lot on the way.


Here at trivago, we love open-source and we believe that we create better software if we intend to publish it later. You can find[the source code of boerewors](https://github.com/trivago/boerewors) at[GitHub](https://github.com/trivago/boerewors) .
