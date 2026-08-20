---
schema_version: "1.0.0"
document_id: "d12573baa5e75dc4428b76a5550578334822fe5da88851c9b3f6012d695d66b4"
company_key: "yc-virtualmin"
company: "Virtualmin"
source_id: "yc-virtualmin-rss-1bbb5effb21e"
canonical_url: "https://forum.virtualmin.com/t/were-messing-with-dns-something-might-break/136253"
published_at: "2026-01-10T20:17:25+00:00"
first_seen_at: "2026-07-28T18:40:24.886262+00:00"
fetched_at: "2026-07-28T20:54:51.009577+00:00"
content_hash: "sha256:d1980c9a5720f06363157200ea3351dea23da03bdedfe0ac2cdb6d330dc91c2a"
---

# We're messing with DNS, something might break

[Joe](https://forum.virtualmin.com/u/Joe)


January 10, 2026, 8:17pm 1


Howdy all,


Just a heads up.


Ilia and I are messing around with DNS today, migrating to Route 53. Theoretically, we’re experts and we know what we’re doing, but realistically, we may have missed something and something will break. So, if anything goes offline, it’s DNS and we’ll probably fix it quickly. Since we’re migrating all zones to Route 53, all services have the potential to break. I doubt any of the obvious things will go wrong, but we might have to fight with email-related records.


If you do see something wrong, and the forum is still up feel free to let us know about it in this thread (and if the forum is not still up, we’ll already be working on it, and don’t need anyone to tell us).


Cheers,
Joe


4 Likes


[CTS](https://forum.virtualmin.com/u/CTS)


January 10, 2026, 8:20pm 2


No Idea what Route 53 is but wish you best of luck


[Joe](https://forum.virtualmin.com/u/Joe)


January 10, 2026, 8:24pm 3


Route 53 is one of the cloud DNS providers Virtualmin supports (Route 53 is in GPL, all the rest are only in Pro). It is a service provided by Amazon.


I’ve been hesitant to stop managing our own local DNS, as we “eat our own dog food” around here, but these days, more and more folks are hosting DNS in cloud providers, so it makes sense for us to do that, too. It’s also more reliable, faster, and more broadly distributed. Amazon has data centers all over the world and they have anycast DNS servers, so you get the closest fastest one. (Of course, we only have one web server and one forum server, so we’re not hyper resilient or redundant, if we have a catastrophic failure, we’ll be recovering from backups…so DNS having five 9s isn’t a big jump in our overall reliability.)


1 Like


[Joe](https://forum.virtualmin.com/u/Joe)


January 10, 2026, 8:31pm 4


Update: I changed the glue records, and I see AWS servers across all my servers when resolving for our zone. So, the damage is done.


If you see anything off with DNS, let us know. We’ll be watching our mail logs to make sure we didn’t break anything DKIM/SPF related.


[CTS](https://forum.virtualmin.com/u/CTS)


January 10, 2026, 8:33pm 5


ok so Route 53 has to do with you providing services and not what we are hosting on our pro virtualmin servers?


[Joe](https://forum.virtualmin.com/u/Joe)


January 10, 2026, 8:37pm 6


Has nothing to do with your servers. We have no control over your servers.


[CTS](https://forum.virtualmin.com/u/CTS)


January 10, 2026, 8:39pm 7


[@Joe](https://forum.virtualmin.com/u/joe)
right but I was meaning our servers connecting to updates for webmin/virtualmin
I hope all goes well with your migration


2 Likes


[Joe](https://forum.virtualmin.com/u/Joe)


January 10, 2026, 8:44pm 8


Oh, yeah, I see what you mean. Yes, it could have affected updates, but I’ve checked and` software.virtualmin.com` is resolving correctly on the new DNS servers. So there should be not outage for existing systems.


That said, for anyone who installed using the new install script with the new` download.virtualmin.com` repo, there may be an outage as we’re also putting a CDN in front of it ([Our Websites Might Get Weird](https://forum.virtualmin.com/t/our-websites-might-get-weird/136254/1) ).` software.virtualmin.com` is still on the old server, and not getting a CDN, so it’s definitely not breaking (at least not because of anything we’re doing).


1 Like


[CTS](https://forum.virtualmin.com/u/CTS)


January 10, 2026, 8:49pm 9


Thanks for the reply[@Joe](https://forum.virtualmin.com/u/joe)
Not sure what the new install script with[software.virtualmin.com](http://software.virtualmin.com/) is, maybe that is the pre-release of virtualmin 8 thing?


Thanks for being transparent about the changes


[Joe](https://forum.virtualmin.com/u/Joe)


January 10, 2026, 8:52pm 10


Yes, the prerelease script uses` download.virtualmin.com` (a new server), while the stable script uses` software.virtualmin.com` ( a server that has been online for six years in the same place and is not moving).


1 Like


[Margotak2](https://forum.virtualmin.com/u/Margotak2)


January 11, 2026, 7:30am 11


Oh now i understand why some of the elements were breaking. Thank your for the updates


[stefan1959](https://forum.virtualmin.com/u/stefan1959)


January 11, 2026, 9:09am 12


Don’t you run mail Joe?
I don’t see dmarc records.


[image 1158×427 116 KB](https://cdn.forum.virtualmin.com/uploads/default/original/3X/6/6/66342c167f6942ae878b173aca14b8f78f669573.png)


[shoulders](https://forum.virtualmin.com/u/shoulders)


January 16, 2026, 8:20pm 13


Why does Outlook recommend ARC for forwarding/mailing lists?


This was on the wishlist


[TiMoscow](https://forum.virtualmin.com/u/TiMoscow)


January 26, 2026, 10:23am 14


Good day!
I saw that my server stopped catching any updates from the[https://software.virtualmin.com](https://software.virtualmin.com/) .


I also get a long response when imaging to[https://forum.virtualmin.com](https://forum.virtualmin.com/)


Location Moscow.


```text
Building dependency tree...
Reading state information...
The following package was automatically installed and is no longer required:
nvidia-firmware-580-580.95.05
Use 'apt autoremove' to remove it.
Recommended packages:
libdbd-mariadb-perl libnet-libidn2-perl
The following packages will be upgraded:
usermin webmin webmin-virtual-server webmin-virtualmin-awstats
webmin-virtualmin-htpasswd
5 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 42.2 MB of archives.
After this operation, 1024 B of additional disk space will be used.
Get:1 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 usermin all 2.520 [10.3 MB]
Ign:1 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 usermin all 2.520
Get:2 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 webmin all 2.620 [27.3 MB]
Ign:2 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 webmin all 2.620
Get:3 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 webmin-virtual-server all 8.0.0.gpl-1 [4385 kB]
Ign:3 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 webmin-virtual-server all 8.0.0.gpl-1
Get:4 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 webmin-virtualmin-awstats all 7.0.0 [89.8 kB]
Ign:4 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 webmin-virtualmin-awstats all 7.0.0
Get:5 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 webmin-virtualmin-htpasswd all 3.7 [88.3 kB]
Ign:5 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 webmin-virtualmin-htpasswd all 3.7
Err:1 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 usermin all 2.520
Connection timed out [IP: 51.158.66.131 443]
Err:2 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 webmin all 2.620
Connection timed out [IP: 51.158.66.131 443]
Err:3 https://software.virtualmin.com/vm/7/gpl/apt virtualmin/main amd64 webmin-virtual-server all 8.0.0.gpl-1
Connection timed out [IP: 51.158.66.131 443]
Fetched 19.0 kB in 11min 39s (27 B/s)
E: Failed to fetch https://software.virtualmin.com/vm/7/gpl/apt/pool/main/u/usermin/usermin_2.520_all.deb  Connection timed out [IP: 51.158.66.131 443]
E: Failed to fetch https://software.virtualmin.com/vm/7/gpl/apt/pool/main/w/webmin/webmin_2.620_all.deb  Connection timed out [IP: 51.158.66.131 443]
E: Failed to fetch https://software.virtualmin.com/vm/7/gpl/apt/pool/main/w/webmin-virtual-server/webmin-virtual-server_8.0.0.gpl-1_all.deb  Connection timed out [IP: 51.158.66.131 443]
E: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?


```


[Joe](https://forum.virtualmin.com/u/Joe)


January 26, 2026, 10:28am 15


That’s unrelated to DNS changes two weeks ago, and is not a DNS problem (the DNS lookup worked, that is the correct address).


If the problem persists, please make a new topic about the problem and we’ll try to help.


[system](https://forum.virtualmin.com/u/system) Closed


February 5, 2026, 10:28am 16


This topic was automatically closed 10 days after the last reply. New replies are no longer allowed.
