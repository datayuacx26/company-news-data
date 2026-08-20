---
schema_version: "1.0.0"
document_id: "711f7dfa1e65dd7454147487cae0c4785a62069fc7a0ef7de89ecc39b569af22"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/simple-yet-powerful-how-granular-authorization-keeps-your-network-secure/"
published_at: "2022-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:fe011ed52f56dea6006fd4a81e7eafe5e06b08403deebe7c945167de1d4d3dfc"
---

# Simple Yet Powerful: How Granular Authorization Keeps Your Network Secure

September 10, 2022


- [Former Uplogix Blog Archive](https://www.lantronix.com/blog/category/former-uplogix-blog-archive/)


### Simple Yet Powerful: How Granular Authorization Keeps Your Network Secure


If the movie *Wargames* taught us anything, it’s that you shouldn’t leave your computer systems so wide open that even Ferris Bueller can just walk right in. Nowhere is that more important than in a remote management platform that is directly connected to your core routers and switches. Remote access and automation come at a cost, but that’s no reason they can’t be as secure as any other system in your network. In this post, we’ll look at how Lantronix respects the privileged access you give it with a robust AAA model that includes what we call Granular Authorization.


# Dial W for War


Just like traditional terminal servers before it, the Lantronix LM-Series provides users with direct console access to network equipment through both in-band and out-of-band channels. If a user can open a terminal session on one of the managed ports, they will essentially have the same access as if they were physically at the site and plugged into the network gear with a laptop and a console cable. With the LM-Series console server sitting on the network, we want to make sure we don’t give this direct console access to just anyone.


Lantronix uses a standard AAA model to define who can access an LM, what they can do once they’re in, and how their actions should be logged. We break out the AAA model in the following way:


- Authentication – local or integrated with existing TACACS, RADIUS, or LDAP environments
- Authorization – role-based, defined locally or through third-party AAA ACL names
- Auditing – local always, offload to third-party Accounting servers available


Authentication and Auditing are pretty straightforward—usernames and passwords are checked, and every action is logged. Where the LM-Series shines is in the definition of role-based Granular Authorization that allows you to specify what a user can do down to a single command. Not only that, you can also specify *where* that command can be run.


Let’s take a closer look.


## Who, What, and Where


If you were to create a user in your LM-Series deployment, give them a username and password, and then immediately try to log in, it would fail. Although the system can *authenticate* the user, it cannot yet *authorize* them to do anything. Without authorization (even the authorization to simply log in), the login attempt is denied.


Consider a set of statements like: **User X has role Y on resource Z.**


These statements define authorization throughout the entire LM-Series deployment.


It breaks down like this:


- User – can be a group or individual user
- Role – a list of allowed and denied commands
- Resource – system, port, modem, all, or server


When configuring your LM-Series deployment to match your security policy, most of the work will be done in the creation and customization of roles.


## A Role for Everyone


By default, LMs ship with just two roles: admin and guest. The admin role includes almost every privilege and is intended for initial setup and/or super users. The guest role allows limited, read-only access to the system. Most customers end up creating a couple custom roles to meet their needs.


As shown in the screenshot above, a role is simply a list of permissions that either allow or deny access to a command. Most of the permissions map to specific commands (e.g., config answer), but others like *login* and *use system auth* don’t have associated commands. Every command begins with a blanket deny; a user must be assigned an *allow* permission to run the command.


**Roles define which commands can be run.** Multiple roles can be combined and evaluated together. For example, if a user is assigned the *admin* role and a role that denies access to the *config answer* command, the user will have access to every command in the *admin* role except *config answer* , because the *deny* statement in the second role takes precedence.


If you are new to the LM-Series console servers, you may not know enough about every command to build a new role from scratch. One option would be to assign everyone the admin role and a second role that denies administrative actions like *config user* or *config system management* .


> Sound complicated? Don’t worry. A Lantronix LEVEL Technical Support Specialist will work with you to understand your security policy and help you configure your deployment to match it. We help every step of the way.


Once your roles are created, you can assign them to users on resources.


## We Have the Resources


In the LM-Series world, giving a user a role on a resource is called *Assigning Privileges* . Privileges can be assigned to the Lantronix Control Center server, Inventory Groups, and individual LMs. We generally assign users or groups a role on the Control Center server and a role on the root Inventory Group so that all LMs can inherit the privileges.


Assigning privileges is as easy as accessing the Privileges section in an Inventory Group or single LM. The menu shown above allows you to choose a Resource, a User/Group, and a Role.


At Inventory Group levels, options for Resource include all, modem, powercontrol, and system. When assigning privileges on a single LM, each of the ports are listed so that some users can be assigned to ports 1/1 – 1/8 while others are assigned to 2/1 – 2/8. Additionally, ports can be labeled throughout the deployment, and those labels will appear in the Resource dropdown, allowing you to assign privileges to all ports labeled with, for example, *firewall* , in the entire deployment.


## Putting it All Together


No two deployments are the same, and we’ve seen customers with just three users give everyone the admin role on every resource, and we’ve seen customers with thousands of users create multiple groups, multiple labels, and give end-users access to their routers or switches and nothing else. Granular Authorization allows you to get as detailed as necessary but isn’t so complicated that you can’t get up and running quickly.


By customizing roles and assigning privileges, we can do a lot of cool things like:


- Limit users to port-passthrough for a managed device (e.g., SSH to IP on port and get to router)
- Use labels to give a group access to all firewalls or switches throughout the deployment
- Limit administrative access by creating roles that deny those tasks and use them in conjunction with the admin role
- Give a user access to just one port
- Give a user access to the system and no ports
- Give read-only access to resources
- Assign privileges to multiple User groups and have user inherit those privileges when assigned to that User group


To summarize: **when you add a user, you will be able to define which commands they can and cannot access on every system, modem, and port resource in your entire deployment.**


By limiting users to only the commands they need to perform their work, we add an extra layer of protection in the event their account credentials are compromised. Role-based Granular Authorization would limit the exposure to only the resources for which they have privileges.


Essentially, Matthew Broderick isn’t doing anything on your LM-Series console server that you, as the administrator, haven’t given him permission to do. And really, isn’t that why computer security was invented?
