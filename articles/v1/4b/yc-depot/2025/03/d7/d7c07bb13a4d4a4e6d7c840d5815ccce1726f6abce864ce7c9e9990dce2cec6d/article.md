---
schema_version: "1.0.0"
document_id: "d7c07bb13a4d4a4e6d7c840d5815ccce1726f6abce864ce7c9e9990dce2cec6d"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/tailscale-integration"
published_at: "2025-03-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:ddd96fc462cce625b02f13fa9b5768d24cdd33ef65f4373594bee1a39359336f"
---

# Now available: Connect Depot to your Tailscale tailnet

If you're not familiar,[Tailscale](https://tailscale.com/) offers secure, identity-first connectivity, allowing your devices, services, and cloud networks to seamlessly and privately connect across any infrastructure.


We use it internally for a bunch of different use cases both inside and outside our AWS workloads. It's a great tool for connecting your devices and services securely without having to worry about static IP allow lists.


So, today we're excited to announce that Depot now supports connecting to your Tailscale tailnet. This means you can enable secure access to private services, such as databases, within your tailnet without opening up those services to the public internet and without maintaining static IP allow lists.


## The why


One of the most frequent questions we field is, "Do you have a static set of outbound IPs that I can add to an allow list?" The answer is no, we don't. We don't have a static set of IPs that we use for our GitHub Actions Runners or Docker image builders. Rather, we use a dynamic set of IPs that are ephemeral.


This is one of the security features of our sandbox environments: an entire dedicated EC2 instance for your GitHub Actions job or Docker image build. Avoiding fixed IPs also has the added benefit of allowing you to not have to think about IP rate limits when using things like DockerHub 🙂


But, it's not without its downsides.


Asking for a static set of outbound IPs is a valid request. Many organizations put their most critical services into private VPCs or behind firewalls so that they're not residing on the public internet. This means that you need to have a static IP allow list to grant access to those services.


This is where Tailscale comes in. By connecting Depot to your Tailscale network, you can enable secure access to private services within your tailnet without opening up those services to the public internet and without maintaining static IP allow lists.


## How it works


Using Tailscale, Depot GitHub Actions runners and container builders join your tailnet as[ephemeral nodes](https://tailscale.com/kb/1111/ephemeral-nodes) , and you can control their access to the rest of your infrastructure using Tailscale ACLs.


### Connecting Depot to your tailnet


Connecting your Depot organization to a Tailscale tailnet is a three-step process:


1. Configure your[Tailnet ACLs](https://tailscale.com/kb/1018/acls) to define a tag for your Depot runners
2. Generate new[OAuth client](https://tailscale.com/kb/1215/oauth-clients) credentials using this new tag
3. Configure your Depot organization to use those OAuth client credentials


#### Step 1: Create a new tag in your Tailnet ACLs


We're going to tag the ephemeral nodes that Depot creates so that we can control their access to the rest of your infrastructure. We recommend creating a new tag named` tag:depot-runner` for this purpose. This tag will later be used in your ACL rules to determine what Depot runners should have access to.


Inside of your[Tailscale admin console](https://login.tailscale.com/admin/acls/file) , you can define a new tag under` tagOwners` :


```text
{
"tagOwners"  : {
"tag:depot-runner"  : [  "group:platform-team"  ]
}
}
```


#### Step 2: Generate a new OAuth client


Next, you need to create a[new OAuth client](https://login.tailscale.com/admin/settings/oauth) from your tailnet's settings. This client can be given a descriptive name and should be granted Write access to the` Keys > Auth Keys` scope. Select the tag you created in the previous step as the chosen tag for this scope.


We're going to need the client ID and client secret that you're given in the next step.


#### Step 3: Configure Depot to use the new OAuth client


Now you can configure your Depot organization to use the new OAuth client credentials from above. From your organization settings page, navigate to the Tailscale section and click **Connect to Tailscale** . Enter the client ID and secret from the previous step and click **Connect** :


Your Depot organization is now connected to your Tailscale tailnet. Depot runners and container builders will now join your tailnet as[ephemeral nodes](https://tailscale.com/kb/1111/ephemeral-nodes) , using the tag you have created.


### Granting access to private services


Now that your Depot GitHub Actions runners and remote container image builders are connected to your tailnet, you can use Tailscale ACLs to control their access to the rest of your infrastructure. Remember that Depot runners will be[tagged](https://tailscale.com/kb/1068/tags) with your chosen tag from step one, which you can then reference in your ACL rules.


For example, if you have a database service inside of your tailnet, you can grant Depot GitHub Actions runners access to it by creating a new[ACL rule](https://tailscale.com/kb/1337/acl-syntax#access-rules) in the[admin console](https://login.tailscale.com/admin/acls/file) :


```text
{
"acls"  : [{  "action"  :   "accept"  ,   "src"  : [  "tag:depot-runner"  ],   "dst"  : [  "database-hostname"  ]}]
}
```


As an added bonus, you can also use[Tailscale subnet routers](https://tailscale.com/kb/1019/subnets) to grant your Depot runners access to entire subnets in any cloud provider VPC or on-premises network.


```text
{
"acls"  : [{  "action"  :   "accept"  ,   "src"  : [  "tag:depot-runner"  ],   "dst"  : [  "192.0.2.0/24:*"  ]}]
}
```


### Disconnecting from Tailscale


If you ever need to disconnect Depot from your tailnet, you can navigate to the Tailscale section in your organization settings and click **Disconnect** . This will remove the OAuth client credentials from your organization and Depot runners will no longer join your tailnet.


An important thing to note on disconnecting Depot from Tailscale is that it will not remove the ephemeral nodes that have already joined your tailnet and are actively running GitHub Actions jobs or container builds. If you want to immediately disconnect any running builds, you can remove any of the connected nodes from your[Tailscale admin console](https://login.tailscale.com/admin/machines) .


## What's next


We think this new integration is going to makes everyone's life a little easier. It was a great opportunity to collaborate with one of our favorite tools and we can't thank the Tailscale team enough for their help.


If you have any questions or feedback, please don't hesitate to reach out in our[Discord Community](https://discord.gg/MMPqYSgDCg) server. We're always looking for ways to improve and make Depot better for everyone.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
