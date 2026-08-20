---
schema_version: "1.0.0"
document_id: "77e48c84203a6c1d5db6c8ce04b987f806b8a18c23460e1eab958fa3e98d86a4"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/lmos-6-8-practical-updates-for-enterprise-out-of-band-management/"
published_at: "2026-01-28T22:51:10+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T22:22:56.687510+00:00"
content_hash: "sha256:b36c1c3923561aa2b9dfd65aab24b4680f507730c30dc609a09e05452ffb7cad"
---

# LMOS 6.8: Practical Updates for Enterprise Out-of-Band Management

January 28, 2026


- [Technical Articles](https://www.lantronix.com/blog/category/technical-articles/)


### LMOS 6.8: Practical Updates for Enterprise Out-of-Band Management


Lantronix LMOS 6.8 is now available for the LM-Series Console Servers and Lantronix Control Center. This release focuses on expanding device connectivity, improving enterprise platform support, and streamlining management workflows with SAML and HTML5 improvements for enterprise network environments.


### **Expanded Serial Connectivity via USB**


The LM-Series Console Servers (top to bottom) LM4, LM80, and LM83X.


LMOS 6.8 allows USB-A ports on LM-Series console servers to function as serial connections, increasing the number of managed devices without adding hardware.


- **LM4** : Supports two additional serial devices over USB
- **LM83X and LM80** : Adds one additional serial port


This is particularly useful for the occasional network device that only has a USB console port. For more USB console port connections, we offer the USB Console Adapter (part # USB-CAA), an inline device that converts a RJ-45 serial connection on the LM-Series console server to a USB console connection on the managed device.


### ****


### **Improved Driver Support for Enterprise Networks**


LMOS 6.8 adds and expands device driver support for common enterprise platforms:


- **Cisco Viptela SD-WAN** : Enables better alignment with cloud-managed SD-WAN architectures and advanced routing and segmentation.
- **Cisco Catalyst IE3400 rugged switches** : Adds support for industrial and edge deployments requiring high-speed Gigabit Ethernet in compact form factors.
- **Juniper Networks** : Enhancements improve configuration recovery, supporting faster restoration during outages or misconfigurations.


These updates improve interoperability across mixed-vendor enterprise environments.


### ****


### ****


### **To Do:**


- **Current customers** : Upgrade to LMOS 6.8 to take advantage of these feature enhancements and security updates. Log in with your MyLantronix account at


[level.lantronix.com](https://level.lantronix.com/)


to download.
- **Learn more** :


[Request a demo of LMOS 6.8](https://www.lantronix.com/hubspot_072524/)


and related LM-Series capabilities with a Lantronix Field Application Engineer to see how these updates apply to your environment.


### ****


### **Expanded Out-of-Band Connectivity Options**


This release extends USB modem support for resilient out-of-band access when primary networks are unavailable:


- Multitech MTCM-L1G2D-B03 LTE modem (Verizon and AT&T support)
- Lantronix M113F00FS for improved reliability
- StarTech CX93001 v.92 USB modem for POTS-based connectivity


These options provide flexibility for remote, industrial, and constrained environments.


### **LLDP Support for Ethernet Ports**


LMOS 6.8 introduces configurable LLDP (Link Layer Discovery Protocol) on Ethernet ports, allowing devices to automatically share identity, capabilities, and neighbor information. This improves network visibility and supports automation and troubleshooting workflows.


### **Control Center Enhancements**


Key updates to Lantronix Control Center include:


- SAML authentication support for Single Sign-On (SSO) with enterprise identity providers
- Improved HTML5 web CLI usability, including right-click paste
- A unified Terminal button that works across all connection types
- Expanded inventory reporting with serial numbers, hostnames, and descriptions
- Enhanced IP filtering with Layer 4 port and protocol (TCP/UDP) criteria


### **Security and Maintenance Updates**


LMOS 6.8 includes:


- Over 200 RPM updates
- Security and stability improvements carried forward from LMOS 6.7
- A configurable connection timeout for the Lantronix terminal application


### **Next Steps**


- **Current customers** : Upgrade to LMOS 6.8! Log in with your MyLantronix account at[level.lantronix.com](https://level.lantronix.com/) to download.
- **Learn more** :[Request a demo of LMOS 6.8 and the LM-Series](https://www.lantronix.com/hubspot_072524/)
