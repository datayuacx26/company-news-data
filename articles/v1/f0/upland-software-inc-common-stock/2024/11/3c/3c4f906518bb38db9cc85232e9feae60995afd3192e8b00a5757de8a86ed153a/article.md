---
schema_version: "1.0.0"
document_id: "3c4f906518bb38db9cc85232e9feae60995afd3192e8b00a5757de8a86ed153a"
company_key: "upland-software-inc-common-stock"
company: "Upland Software Inc."
source_id: "upland-software-inc-common-stock-rss-53a5709ba49e"
canonical_url: "https://olresourcecenter.uplandsoftware.com/blog/windows-tidbits-chapter-3/"
published_at: "2024-11-27T16:24:39+00:00"
first_seen_at: "2026-07-26T13:25:40.115128+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:2d57d76c65565cae3bad89c49e186bd34b66eba1980909cae23f75542e0c9c46"
---

# Windows tidbits: chapter 3

This series focuses on the various impacts the underlying operating system can have on your OL Connect implementation. This chapter describes an easy way to access the various web services hosted locally on your system.


## The goldfish memory syndrome


The human mind is a wonderous thing, especially as it pertains to memory: it decides, seemingly in random fashion, what must be remembered and what can be discarded. Some folks remember all the birthdays of all their acquaintances, while others can recite the phone numbers of everyone they ever knew.


As I’m sure I have stated before, I’m the lazy, goldfish type: I can’t be bothered to remember things, especially when there’s some technology that can do it in my place. For me, birthdays are handled by social medias, which send me timely reminders about so-and-so turning 42. And the Contacts app on my phone contains not just one, but all telephone numbers I could ever need to reach anyone I want to talk to. So while I used to take pride in my ability to remember both those lists of items before, it now seems like a waste of valuable brain real estate because I have technology that can do it for me with a single click.


There are, however, some lists of items that are not specifically being handled by a single piece of technology and that I still can’t seem to remember on my own, probably because they aren’t critical bits of information that my brain feels should be stored in permanent RAM. One such example is the list of ports that are being used by the various services on my system. I’ve got Workflow, whose config includes processes that use both the legacy HTTP Server Input and the NodeJS Server Input. I’ve got the OL Connect Server whose REST API I need to access regularly, and the MariaDB database that uses a specific port. I am also running the Automate tech preview. And then, I have numerous Docker containers and/or Hyper-V VMs running NodeJS, WordPress, NGINX, and sometimes IIS. (yes, my machine is pretty busy, thank you very much!)


At any given time, many of those servers might be running on my system. And while most of them can be accessed via basic` 127.0.0.1:XXXX` URLs, where` XXXX` is the number of the port used by each server, I just can’t seem to remember which port goes to which service.


## The gist of the issue


Wouldn’t it be easier if you could just go into your Windows Hosts file and specify easy-to-remember URLs that point to those individual ports? (For those unfamiliar with the Windows Hosts file, read this[very short how-to article](https://en.wikiversity.org/wiki/Hosts_file/Edit#:~:text=The%20hosts%20file%20is%20a,System32%5Cdrivers%5Cetc%20folder.) . But essentially, it allows you to map a human-readable name to an IP address e.g.` 127.0.0.1 mymachine.local` ).


The problem is that the Hosts file only allows you to map a name to an *IP address* , but not to a specific port. For instance, the following entries in a Hosts file, while you’d think they’d make perfect sense, are invalid:


```text
127.0.0.1:8080    httpServer.local
127.0.0.1:9090    nodeServer.local
```


Now of course I could use a reverse proxy application (both NGINX and IIS, and probably all standard web servers allow you to do that), but it seems like overkill to be running a web server solely for the purpose of redirecting URLs to specific port.


As often happens, while scouring the web for an answer to a completely unrelated question, I happened upon a blog post that caught my eye: it stated you *could* , indeed, use the Hosts file to implement the redirections, even though it doesn’t support ports. HOW?!?


## The implementation


The solution involves two tools that are natively available in Windows: the **Hosts** file, and the **netsh** utility. But first, you need to understand one thing: the IP addresses in the` 127.0.0.1/8` range (that is, from` 127.0.0.1` to` 127.255.255.255` ) all loop back into your own system. In other words, packets or requests sent to those IP addresses never leave your computer, they are handled locally. As a general rule, none of them are actually *in use* , so you are free to use any of them to loop back into you own system. There are rare exceptions to this, but for all intents and purposes, they are free to use. And that will come in handy in the first step of the procedure.


To illustrate the procedure, here’s what we will try to achieve.


1. Access Workflow’s HTTP server with` **httpServer.local**` instead of` 127.0.0.1:8080`
2. Access Workflow’s NodeJS server with` **nodeServer.local**` instead of` 127.0.0.1:9090`
3. Access the OL Connect Server with` **connectServer.local**` instead of` 127.0.0.1:9340`


### First step: the Hosts file


- Open your favorite text editor in **administrator mode** (this is important because the Hosts file is write-protected)
- At the bottom of the file, add the following lines and save the file


```text
127.10.0.1      httpServer.local
127.10.0.2      nodeServer.local
127.10.0.3      connectServer.local
```


What are these` 127.10.0.#` addresses, you ask? Well as we saw earlier, they are all loopback addresses so ultimately, they are all equivalent to` 127.0.0.1` . But we don’t want to redirect all our new names to the same address, so we just pick any free address in the` 127.0.0.1/8` range and assign a different one to each name.


### Second step: port redirection with netsh


Another bit of information that’s critical, but you probably know it already, is that when you point your browser to an IP address or to a domain name without specifying any port, it will default to sending the request to port 80 (for HTTP) or 443 (for HTTPS). Any HTTP(s) request sent via scripting will also do the same. We’ll be using that to our advantage in the second step: we will redirect port 80 on our three IP addresses to the actual port being used by the services on the local system.


To do that, we’ll be using the **netsh** command line utility. Open up a CMD (or Terminal) window in administrative mode. Then copy/paste the following command (on a single line):


```text
netsh interface portproxy add v4tov4 listenport=80 listenaddress=127.10.0.1 connectport=8080  connectaddress=127.0.0.1
```


This instructs the system to listen for requests on` 127.10.0.1:80` and redirect them to` 127.0.0.1:8080` . And since our Hosts file already knows that` 127.10.0.1` can be accessed through the` httpServer.local` name, that means requests sent to` httpServer.local:80` are redirected to` 127.0.0.1:8080` , which is the port being monitored by Workflow’s HTTP Server. And since port 80 is the default HTTP port, it doesn’t need to be specified; hence,` httpServer.local` gets redirected to` 127.0.0.1:8080` .


The effect of the **netsh** command is immediate and permanent. No need to re-issue it in between reboots.


All that’s left to do to complete our task is to issue the same command again, with the appropriate parameters, to redirect requests to the` nodeServer.local` and` connectServer.local` names we created earlier.


## More information


If you change the preferences of the applications that are listening on a specific port, you’ll also have to make the changes with the **netsh** utility. To get a list of all current redirections, use the following command:


```text
netsh interface portproxy show all
```


To delete one of the redirections, use something like:


```text
netsh interface portproxy delete v4tov4 listenport=80 listenaddress=127.10.0.1
```


Note that the **netsh** utility is not limited to local addresses. It’s possible to use the same technique in order to redirect request to a different machine, VM or container. This can be handy if you run some of your applications inside VMs or containers as it allows you to specify easy-to-remember aliases to addresses. For instance, I have the Tech Preview version of Automate running inside a Docker container which is set up up to forward requests from local port 10880 to Automate’s native 1880 port inside the container. Using the technique explained above, I created an` automate.container` name to access the container’s Automate configuration from my web browser. I also have an equivalent` automate.local` name for my locally-hosted instance of Automate. That way, a quick glance at my web browser’s address bar tells instantly me if the page is coming from the container or from the host.


## Conclusion


The technique described simplifies the creation of easy-to-remember URLs for each of the servers running on your machine. These URLs are also great for presentations or demos because they feel more like the browser is accessing a remote URL, rather than some arcane IP address and port number.


And that, my fishy friends, is how you can give your brain a break!


### Share this:


- [Share on Facebook (Opens in new window) Facebook](https://olresourcecenter.uplandsoftware.com/blog/windows-tidbits-chapter-3/?share=facebook)
- [Share on X (Opens in new window) X](https://olresourcecenter.uplandsoftware.com/blog/windows-tidbits-chapter-3/?share=twitter)
- [Share on LinkedIn (Opens in new window) LinkedIn](https://olresourcecenter.uplandsoftware.com/blog/windows-tidbits-chapter-3/?share=linkedin)
-


### *Next best reads for you!*


### Leave a Reply[Cancel reply](https://olresourcecenter.uplandsoftware.com/blog/windows-tidbits-chapter-3/#respond)
