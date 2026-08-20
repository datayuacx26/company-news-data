---
schema_version: "1.0.0"
document_id: "b471fa67783068a70522e46fd7b5e9540d4b7557c3296cbf4fb430e2c245ff21"
company_key: "airgain-inc-common-stock"
company: "Airgain Inc."
source_id: "airgain-inc-common-stock-news-import-92deaec5a802"
canonical_url: "https://airgain.com/blog/how-to-choose-the-right-iot-development-kit/"
published_at: "2022-12-19T18:27:47+00:00"
first_seen_at: "2026-07-24T14:54:55.214562+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:862fe960e217d13562fe34ae0078334d17d818edd7d457b50453febb877cbb10"
---

# How to Choose the Right IoT Development Kit

**When “One-Size-Fits-All” Doesn’t Quite Fit**


A friend of ours has a large head; some might say freakishly so. Not only does this mean he consumes copious quantities of sunscreen on beach days, but it also means that he has a tough time hat shopping. Many hats with stretch-fit or snap backs say “one-size-fits-all,” but they all end up looking like a horn-billed yarmulke on his pronounced melon. The same could be said for many other one-size-fits-all “solutions.” Really, they fit when they fit.


A new T-Mobile


**IoT Developer Kit** just hit the market. It is positioned as a one-size-fits all approach, which is a novel and thoughtful effort. Of course, if there is any industry that is unapologetically nuanced, it’s the Internet of Things. IoT customers have widely varying application needs that can change on a dime. So, when a development kit advertises ease of use, simple debugging, open software and hardware, flexible design capabilities, plus integrated Bluetooth & Wi-Fi … we say, “bring it on!” It just sounds so delightfully one-size fits all-ish.


Keep in mind, tech industry organizations are teeming with tech-obsessed folk. So, when something hits the market, we can’t wait to see it? So exciting! And beyond that, how could we NOT be somewhat curious about comparing the newest offer to our own IoT development kit solutions?


**A Quick Reference Snapshot**
Here’s a quick table to give you a feel for expected development costs of using a certified module vs. using a certified “integrated device.”


**Integrated Device** **Module**


Time to Market 3 to 6 months 6 to 18 months


R & D Budget <$100k


<$250k


Certification Cost 0 $60 to $100k


Manufacturer/PN Airgain’s Embedded Modems Module used in T-Mobile Development Kit


**Development Kit Architecture & Accessories Considerations**


The T-Mobile IoT Developer Kit comes with a main board, protective case, battery, power adapter, USB charging cable, user guide, and SIM card (with 500MB of data). Again, the main board uses a certified “module.” If the module is later used on a custom board for production, the final custom board will also need to be certified as an “Integrated Device.” Note: This kit can only connect to the T-Mobile network.


In comparison, Airgain offers multiple development kits and accessories to ultimately make development easier for your end application. So, the approach here is “many available options to fit all sizes, perfectly.”


All of Airgain’s development kits use a plug-in Airgain embedded modem. This plug-in approach allows the developer to choose the embedded modem that meets their needs, and then a complementary development kit with the right feature-set.


One ultra-nice feature about Airgain’s embedded modems is that they’re built on a small form factor and are pin compatible (essentially future-proof), allowing you to incorporate future cellular technologies without board level changes.


**Ease of Transitioning from Developer Kit to Production**


T-Mobile’s new kit uses a cellular module that is soldered directly to the PCB. This ultrasmall module approach may make the development kit’s BOM cost lower. However, transitioning from this kit to a custom design can add challenges and cost. You see, the module used in the kit is only PTCRB certified as – you guessed it – a module. In other words, this means that once you prove out your implementation on the development kit and want to move to a custom board (and production), you’ll need to submit your end-device to PTCRB for additional pre-deployment testing before deployment.


In comparison, Airgain’s embedded modems are certified as an integrated device (I.e. End-device) by PTCRB. This means that the work done on the Airgain development kit can be DIRECTLY used in the end-device because the Airgain modem is used in both the development kit – and the production unit.


**Modem Options & Considerations**


T-Mobile’s new kit comes with an LTE CAT-M modem at its core – an excellent offering for low-bandwidth applications, but insufficient for security cameras and other greater bandwidth-intensive applications.


Just to play devil’s advocate, Airgain offers “end-device certified” embedded modem options, such as


[NB-IOT, LTE-M1, CAT1, CAT4](https://airgain.com/technology/lte-cat-1-cat-4-lte-m-nb-iot/) (and even 2G/3G fallback options on some products so customers can choose the right speed grade for their solution). As a result, developers who go the Airgain route, are able to choose the modem they desire, already certified for various carriers in different regions of the world. So, there are options here if the fits all option doesn’t quite accommodate.


**Antenna Implementation**
If a fixed antenna implementation works, the one-size offering is adequate as it will work for some applications. However, (and this could prove a bit troublesome for some customers)


***you cannot put it in a metal box*** . Beyond that significant obstacle,


*any* enclosure around this kit will negatively impact the antenna’s performance.


On the other hand, Airgain’s embedded modems allows customers more antenna implementation flexibility. This way, developers can pair the idyllic antenna approach to meet the needs of their specific product offering. Airgain’s embedded modems use standard U.FL antenna connectors (and the end-device certification is antenna independent) – meaning, the Airgain embedded modem


*retains its certification* even when selecting separate plug-in antennas.


**Getting Connected**


The T-Mobile Dev Edge IoT Developer Kit offers what appears to be an easy connection path to T-Mobile’s network, to get started quick. Whereas Airgain’s embedded modems and development kits offer additional flexibility with similar ease of use.


Each of Airgain’s embedded modems has a dedicated web page, complete with application notes, examples, data sheets and more … for example, each embedded modem has a detailed user manual that covers setting up your workstation for communications, setting up the development kit, and making sure your modem registers on the chosen network.


**Activating your SIM Card & Choosing a Data Plan**


T-Mobile’s IoT Developer Kit includes a pre-activated SIM card with 500MB of data (only for T-Mobile’s network), whereas Airgain offers data plans from all the leading carriers.


Of course, pricing will depend upon your planned usage, so Airgain offers a variety of plans for different amounts of data with no hidden costs. We even provide access to our easy-to-use self-service portal for quick and easy multiple-carrier activations, mobile networks and geographies. What’s more, once you go to production, Airgain offers


[“Name Your Price”](https://airgain.com/products/embedded-modems/iot-data-plans/?utm_source=google&utm_medium=cpc&utm_campaign=sitelink&utm_term=airgain&utm_campaign=Airgain+-+Branded&utm_source=adwords&utm_medium=ppc&hsa_acc=5358925279&hsa_cam=17641806203&hsa_grp=138365510637&hsa_ad=607779317922&hsa_src=g&hsa_tgt=kwd-14618467817&hsa_kw=airgain&hsa_mt=b&hsa_net=adwords&hsa_ver=3&gclid=Cj0KCQiA4uCcBhDdARIsAH5jyUkGVheVvP6gFialfmYVhTLRYoX_YZyRg1Wjs1SfsajCxGk1YxOF6eoaAl1WEALw_wcB) data plans, providing the very best match with access to virtually every carrier data plan across the globe. Just submit a bid for what you’d like to pay and we’ll get back to you.


**IoT Support Services**


In addition to the standard web-based information, Airgain additionally offers 1-on-1 schematic reviews, layout reviews, design and integration support, and RF/antenna evaluation as a standalone service. Contact Airgain for more details.


**(Almost) Final Thoughts**
There are countless numbers of IoT products with new features being imagined each and every day. Companies such as T-Mobile offered their new development kit with the intention of allowing developers to get a faster start – and they should be commended for their effort. Of course, just like our friend with the big head, one size fits all … really doesn’t mean everybody.


For those of you whose product-in-development does not fit the mold of “basic” or “usually” or even “in some cases,” to seamlessly align with the parameters of said “one size fits all” offer – Airgain’s development kit solutions allow designers to be significantly nimbler.


So, before selecting a cellular IoT development kit, it’s important to consider the potential costs to production. One BIG ticket item (in both cost and time) is the benefit of Airgain’s embedded modems being carrier certification complete – no additional certification work is required. That alone dramatically reduces the cost, risk, and timeline associated with this segment of IoT product development.


**This Article is Over … BUT**


You do not have to read anymore. Unless, of course, you have decided to choose an Airgain embedded modem development kit. If you have, please continue.


**How to Select The Perfect Airgain Embedded Modem Development Kit for Your Project**


Step 1: Select an Airgain[embedded modem](https://airgain.com/product-category/nimbelink/embedded-modems/)


.


- There are options for different carriers, cellular modem speeds, regions, and sizes of the modem.


Step 2: Select an Airgain development kit. There are multiple options to choose from depending on your needs.


- [Skywire Development Kit](https://airgain.com/products/skywire-dev-kit-swdk2/)


-


- Connect to the modem in any of these 3 ways:


-


-


- Directly from PC to the modem’s UART port.


-


-


- From a microcontroller development kit that supports an Arduino shield interface.


-


-


- Through the two modem breakout headers.


- [Skywire Nano Development Kit](https://airgain.com/products/4g-lte-m-skywire-nano-swndk/)


-


- Connect to the modem over a USB cable.


- [MikroElectronika Skywire Click](https://airgain.com/products/mikroelectronika-skywire-click/)


-


- Plug Airgain modems into any device that supports mikroBUS click board interface.


- [Mini-PCI Express Adapter Board](https://airgain.com/products/mini-pci-express-half-size/)


-


- Add an Airgain embedded modem through a mPCIe interface.


- [Skywire S2C-Link Accessory Kit](https://airgain.com/products/skywire-s2c-link-accessory-kit/)


-


- Create a fully assembled boxed modem.


- [Raspberry Pi Skywire Adapter](https://airgain.com/products/raspberry-pi-skywire-adapter/)


-


- Deploy a small but powerful computer almost anywhere with Cellular connectivity.


- [Skywire BeagleBone Black Cape](https://airgain.com/products/skywire-beaglebone-black-cape/)


-


- Add a cellular cape to a BeagleBone Black development kit.


- [Skywire BeagleBone Cape Lite](https://airgain.com/products/skywire-beaglebone-cape-lite/)


-


- Add a cellular cape with less features to a BeagleBone development kit.


**Because Airgain Project Managers Will Delight That The Following Was Added**


Airgain’s development kits come with 12V power supply, antenna, USB cables, schematics, BOM, gerbers, SIM cards, etc. (where applicable).


As stated before, it’s important to develop with Airgain’s embedded modems because they are already end-device (i.e., “integrated device”) certified. For production designs, the embedded modem is the same as what was used on the development kit.
