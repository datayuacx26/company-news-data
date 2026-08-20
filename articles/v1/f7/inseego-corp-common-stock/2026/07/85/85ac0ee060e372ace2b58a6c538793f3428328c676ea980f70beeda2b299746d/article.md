---
schema_version: "1.0.0"
document_id: "85ac0ee060e372ace2b58a6c538793f3428328c676ea980f70beeda2b299746d"
company_key: "inseego-corp-common-stock"
company: "Inseego Corp."
source_id: "inseego-corp-common-stock-news-import-2399808c2103"
canonical_url: "https://inseego.com/resources/blog/how-to-set-up-cellular-ip-passthrough/"
published_at: "2026-07-30T19:47:21+00:00"
first_seen_at: "2026-07-31T20:28:37.761465+00:00"
fetched_at: "2026-07-31T21:48:33.066422+00:00"
content_hash: "sha256:d213d14012561c09d6d6e6ba4b11ef973647a9bd309d72c9fc5fb3087879f595"
---

# How to set up cellular IP Passthrough

When enterprise branch networks drop offline, every second counts. Fiber cuts, ISP outages, and regional service disruptions happen, which is why pairing your primary wireline WAN with a high-speed 5G cellular link has become standard architecture for enterprise reliability.


However, adding a cellular router to your network stack often introduces a frustrating hurdle: **Double NAT** .


If your cellular device performs its own Network Address Translation (NAT) alongside your primary firewall, inbound VPN tunnels break, VoIP call quality plummets, and network telemetry becomes a mess.


The solution is **IP Passthrough** (sometimes referred to as Bridge Mode).


By configuring an enterprise cellular router for IP Passthrough, you pass the public cellular IP address directly to the WAN port of your downstream firewall (such as a Cisco Meraki, Fortinet FortiGate, or Palo Alto gateway). The cellular hardware becomes a transparent 5G pipe, leaving all routing, security policies, and SD-WAN failover rules to your primary firewall.


Here is a step-by-step guide on how IP Passthrough works in cellular deployments and how to set it up cleanly in an enterprise environment.


## What happens during IP Passthrough?


In standard routing mode, a cellular router sits between the internet and your firewall, assigning a local private IP (e.g.,` 192.168.1.100` ) to your firewall’s WAN interface.


\[Cellular Network\] ---> (Public IP) \[Cellular Router\] (192.168.1.1) ---> (192.168.1.100) \[Firewall WAN2\]


When you enable **IP Passthrough** , the cellular router disables its internal NAT and routing logic on the designated LAN port, handing the carrier's assigned public IP directly to your firewall:


\[Cellular Network\] ---> (Public IP) \[Cellular Router\] (Transparent) ---> (Public IP) \[Firewall WAN2\]


Your firewall now sees the 5G connection as a direct, native WAN interface.


Unlock lightning-fast 5G internet almost anywhere


## 4 steps to deploy cellular IP passthrough


While interface navigation varies slightly between cloud management platforms and local Web UIs, the enterprise deployment workflow follows four key steps.


### Step 1: Verify APN and carrier connectivity


Before altering any routing behavior or toggling passthrough features, you must verify that the cellular modem has established a stable RF link and successfully attached to the mobile network operator's packet core.


#### 1. Establish an administrative session


- **Via Local Web UI:** Plug a Cat 6 Ethernet cable from your laptop into an active LAN port on the cellular router (e.g., Port 2 on an[Inseego Wavemaker FX4200](https://inseego.com/products/5g-indoor-routers/fx4200/) ). Open a web browser, navigate to` https://192.168.1.1` , and log in using your admin credentials.
- **Via Inseego Connect:** Log in to your tenant dashboard at[Inseego Connect Cloud Management](https://inseego.com/products/cloud-management/inseego-connect/) (or directly at` connect.inseego.com` ) and select the target device from your inventory.


#### 2. Inspect RF metrics and cellular connection state


- Navigate to **About > Cellular** or **Network > Cellular status** .
- Verify that **Connection state** reads *Connected* and displays an active network operator (e.g., Verizon, AT&T, T-Mobile).
- Evaluate signal health using key RF parameters:


- **RSRP** ( *Reference Signal Received Power* ): Ideal is better than **-90 dBm** . Values worse than **-115 dBm** indicate poor signal and require antenna adjustment or relocation.
- **RSRQ** ( *Reference Signal Received Quality* ): Ideal is better than **-10 dB** .
- **SINR** ( *Signal-to-Interference-plus-Noise Ratio* ): Ideal is **13 dB** or higher.


#### 3. Verify APN (Access Point Name) assignment


- **Dynamic / default consumer APNs:** If using standard mobile broadband SIMs, the router auto-detects the APN from the SIM profile.
- **Custom / static corporate APNs:** If deploying private network SIMs, static IP pools, or corporate VPN access:


1. Go to **Network > Cellular > APN settings** .
2. Toggle APN selection to **Manual** .
3. Input your carrier-assigned APN string (e.g.,` ne01.vzwstatic` or custom enterprise profile name).
4. Save settings and confirm that the device re-attaches to the network with a valid public or static IP address.


**Critical verification step:** Open a new browser tab on your laptop and verify full internet connectivity (e.g., run a speed test or ping` 8.8.8.8` ). Troubleshooting carrier attachment issues after enabling IP passthrough is significantly more difficult once local DHCP and routing functions are bypassed.


### Step 2: Enable IP passthrough mode


Enabling IP passthrough reconfigures the router’s Ethernet bridge to forward raw cellular WAN packets directly to a designated physical interface without applying local Network Address Translation (NAT) or firewall inspection.


#### 1. Choose your Inseego configuration method


Depending on the scale of your deployment and field workflows, Inseego provides three primary administrative paths for enabling IP Passthrough:


- **Local Admin Web UI (Single-site & bench testing):** Connect a laptop directly to the router via Ethernet, navigate to` https://192.168.1.1` or` http://inseego.local` , and sign in with your admin credentials. Go to **Network > IP passthrough** to toggle the feature on. This local approach is ideal for single-location setups, initial provisioning, or bench-testing hardware before deployment. Here’s more information on this section:[Inseego’s local web UI for IP Passthrough](https://inseego.com/resources/product-documentation/wavemaker-fx4100/user-guide/configuration/managing-your-network/lan-tab/ip-passthrough/)
- **Inseego Connect (Multi-site fleet management):** For enterprise deployments spanning dozens or hundreds of locations, logging into individual Web UIs isn't practical. Network teams can sign into the[Inseego Connect Platform](https://inseego.com/products/cloud-management/inseego-connect/) , select target device groups or site profiles, and push an IP Passthrough configuration template remotely across the entire fleet in just a few clicks.
- **Inseego Mobile App (Field technician installation):** Field installers deploying gateways on-site can pair directly with the device over Bluetooth or Wi-Fi using the[Inseego Mobile App](https://inseego.com/products/cloud-management/inseego-mobile-app/) . Installers can run real-time RF signal checks, optimize physical antenna placement using live RSRP/SINR metrics, and enable IP Passthrough right before plugging the final Ethernet hand-off cable into the downstream firewall.


#### 2. Toggle passthrough status


Inside your selected management interface, navigate to **Network > IP passthrough** (or **LAN > Networking modes** depending on firmware release) and change the status from **Disabled** to **Enabled** .


#### 3. Select the physical interface


Choose the designated physical Ethernet interface wired to your downstream firewall.


**Performance tip — hardware port selection:**


On Multi-Gigabit Sub-6 5G routers like the[Inseego Wavemaker FX4200](https://inseego.com/products/5g-indoor-routers/fx4200/) , explicitly select **Port 1 (2.5 GbE)** . Selecting a standard 1 GbE port can choke peak 5G cellular speeds under heavy aggregate throughput conditions.


#### 4. Configure lease mode and MAC binding


- **DHCPS (DHCP Server Mode - Recommended):** Select **DHCPS** . In this mode, the cellular router acts as a lean DHCP server solely for the connected host. When the mobile operator assigns or updates the WAN IP address, the cellular router automatically updates the lease granted to your firewall's WAN port without requiring manual network intervention.
- **MAC binding (Optional / security hardening):** Select **MAC Manual** and input the exact MAC address of your downstream firewall’s WAN interface to ensure the public cellular IP is handed off only to your designated security appliance.


#### 5. Apply configuration


Click **Save changes** or **Apply** . The device will update its internal routing tables. Note that local LAN DHCP and Wi-Fi broadcasting on indoor routers will shut down once applied.


**Cloud management note:**


Enabling IP passthrough turns off local routing, but it **does not** cut off out-of-band management. Hardware managed via[Inseego Connect](https://inseego.com/products/cloud-management/inseego-connect/) maintains an active cloud connection over the cellular interface. Network administrators retain full remote management, firmware update, and telemetry access directly through the Inseego Connect portal without needing local IP access.


## Hardware-specific considerations across Inseego lines


While the fundamental IP passthrough workflow applies to all Inseego devices, specific product families feature distinct physical hand-offs and hardware behaviors:


- **Wavemaker Indoor Cellular Routers ([FX4200](https://inseego.com/products/5g-indoor-routers/fx4200/) ):** Enabling IP passthrough automatically disables local Wi-Fi broadcasting and internal mesh features. Ensure you select the highest-speed available Ethernet port (e.g., 2.5 GbE Port 1) to hand off the cellular WAN to your downstream firewall.
- **Wavemaker Outdoor Gateways ([FW2000](https://inseego.com/products/5g-outdoor-cpe/fw2000/) ,[FW3000](https://inseego.com/products/5g-outdoor-cpe/fw3000/) ):** These high-gain 5G outdoor units feature weatherized PoE (Power over Ethernet) ports. The passed-through public IP travels directly down the single Ethernet drop cable from the roof or exterior wall directly into your indoor security appliance.
- **MiFi Mobile Routers ([MiFi PRO M4](https://inseego.com/products/mobile-hotspot-routers/mifi-pro-m4/) ):** Supports IP passthrough either via **USB-C tethering** directly to a compatible router/firewall or via **Ethernet** when placed in an enterprise cradle attachment.


### Step 3: Connect to your downstream firewall or SD-WAN gateway


Once passthrough mode is active, physical handoff shifts the cellular public IP directly to your central enterprise security appliance (e.g., Cisco Meraki, Fortinet FortiGate, Palo Alto Networks, or VMware SD-WAN).


#### 1. Physical cabling


- Disconnect your configuration laptop from Port 1.
- Connect a shielded Cat 6 or Cat 6A Ethernet cable from **Port 1 (2.5 GbE)** on the cellular router directly into the designated secondary WAN port (e.g.,` WAN2` ,` Internet 2` , or` AUX` ) on your firewall.


#### 2. Configure downstream firewall interface


- Access your firewall's management interface (e.g., FortiOS Web Console, Meraki Dashboard, or PAN-OS).
- Select the designated secondary WAN interface (e.g.,` port2` or` WAN2` ).
- Set the addressing mode / IP assignment to **DHCP (Dynamic)** .
- Disable administrative protocols (like HTTP/HTTPS/SSH) on that WAN interface unless explicitly required and protected by strict source-IP access lists.


#### 3. Validate public IP hand-off


Refresh the firewall interface status. A successful passthrough will show the firewall’s WAN interface displaying the public IP address assigned by the cellular carrier (e.g.,` 172.x.x.x` ,` 12.x.x.x` , or a static public IPv4 address).


**Warning — Carrier-Grade NAT (CGNAT):** If the firewall interface displays an IP address in the` 100.64.x.x` to` 100.127.x.x` range (RFC 6598) or a` 10.x.x.x` block, your carrier is utilizing Carrier-Grade NAT. Outbound traffic will function normally, but incoming site-to-site IPsec VPN tunnels initiated from remote head-ends will fail unless a static public APN is provisioned by the operator.


### Step 4: Configure failover and SLA thresholds on the firewall


With the cellular router operating as a transparent 5G modem, you must configure your firewall’s SD-WAN engine or WAN link controller to manage failover triggers, health probes, and failback behavior.


#### 1. Establish active performance probes (health checks)


- Configure automated health checks on your firewall's cellular WAN interface using ICMP (Ping) or HTTP/HTTPS GET requests.
- Target two distinct, highly available public IP targets (e.g., Primary:` 8.8.8.8` / Secondary:` 1.1.1.1` or corporate head-end IPs) to prevent false-positive failovers caused by single-DNS outages.
- Set probe intervals (e.g., send a probe every 1,000 ms; fail after 3 consecutive dropped responses).


#### 2. Define performance SLA and loss thresholds


Because cellular networks exhibit higher variance in latency than fixed fiber, adjust your SD-WAN SLA rules accordingly:


- **Latency threshold:** Trigger failover if primary WAN latency exceeds **150 ms** for more than 5 seconds.
- **Packet loss threshold:** Trigger failover if primary WAN packet loss exceeds **2% to 5%** .
- **Jitter threshold:** Set jitter tolerance around **30 ms** .


#### 3. Configure SLA hold-down timers (flapping prevention)


When primary wireline connections (fiber/cable) experience intermittent physical damage or degradation, they often alternate rapidly between UP and DOWN states ("flapping").


Set a **hold-down / restore delay timer** (recommended: **60 to 300 seconds** ). This forces the firewall to keep traffic routed over the stable 5G cellular link until the primary wireline WAN has maintained a 100% clean health check status continuously for the entire duration of the timer.


## Building a resilient enterprise edge


Setting up cellular IP passthrough isn't just about adding a backup connection, it's about protecting your existing security and routing architecture. By allowing high-speed 5G hardware to operate as a transparent WAN pipe, enterprise network teams maintain full control over firewall policies, preserve IPsec tunnel health, and eliminate double NAT bottlenecks across every remote location.
