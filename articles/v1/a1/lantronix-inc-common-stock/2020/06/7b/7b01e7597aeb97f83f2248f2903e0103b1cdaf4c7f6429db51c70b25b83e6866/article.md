---
schema_version: "1.0.0"
document_id: "7b01e7597aeb97f83f2248f2903e0103b1cdaf4c7f6429db51c70b25b83e6866"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/smart-home-home-automation-made-easy/"
published_at: "2020-06-16T00:47:58+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:e241e56a83534e6a4f47fea7ba49589c224dab00e70e64aa0cfe2ea803541fec"
---

# Turn Your Home into a Smart Home: Home Hub Installation Made Easy

June 15, 2020


- [General](https://www.lantronix.com/blog/category/general/)


### Turn Your Home into a Smart Home: Home Hub Installation Made Easy


Home automation is a booming market with a growing CAGR of 11.3% predicted by 2022. The multi-billion-dollar smart home industry offers many opportunities for homeowners to take advantage of technology to make their lives easier.


Home automation refers to the control and security of smart home devices. It may be used for isolated programmable devices, such as thermostats and sprinkler systems. But home automation more precisely describes smart homes with lights, appliances, electrical outlets, heating and cooling systems and security systems that are connected and controlled via a remotely accessible network. Home automation systems give users control of energy usage resulting in cost savings and help to enhance their standard of living. Let’s take a deeper dive into making home automation a reality in your home.


### **** **Home Hub – The Center of Home Automation**


A Home Hub comprises hardware and software that allows you to remotely control the connected devices in your home. Connected devices in your house include smart devices such as lights, heating and air conditioning, home appliances, security systems and much more.


You can group multiple devices such as the lights in a room and turn them all on at once. Or you can set a schedule for your coffee maker or thermostat to ensure that you awake to a warm room and a hot cup of coffee. You can even remotely control your devices from virtually anywhere over Wi-Fi or the Internet. With the addition of voice-activation services such as Alexa and Google Assistant, you can control your connected smart home devices with voice commands.


Pull Quote:


“A home hub communicate with connected devices enabling users to remotely control the, conserving energy and adding more convenience to your home.”


### **Open-Q™ 212A** **Home Hub Dev Kit**


The[Open-Q™ 212A Development Kit](https://www.lantronix.com/products/open-q-212a-home-hub-development-kit/) has a Home Hub Platform Software release announced[here](https://www.intrinsyc.com/new-linux-home-hub-sw-release-available-for-the-open-q-212a/) . This Software release allows the Open-Q™ 212A to become a home hub to control the smart devices in your house.


**Smart Home With Open-Q™ 212A as the Home Hub**


The picture above shows the smart home components and how the[Open-Q™ 212A](https://www.lantronix.com/products/open-q-212a-som/) fits in as a Home Hub. What follows is a description of this picture from the bottom-up.


The Wi-Fi Access Point in your house provides the communication path to all your Smart devices. This is your own home’s Wi-Fi hotspot with secure password enabled. Most smart devices in your home can connect to 2.4GHz Wi-Fi.


Above the Wi-Fi Access Point are the smart devices installed in your house. These devices can include smart bulbs, smart plugs, smart light switches, and much more. Smart devices connect to your Wi-Fi network and will require constant power from your home’s wall electricity in order to be able to stay connected. Most smart light switches require a 4-wire connection: load (red), hot (black), ground (green), and neutral (white). Unlike a traditional light switch, a smart switch needs the neutral wire where they tap constant power.


Above the smart devices (on the right) is a mobile device (phone or tablet). This mobile device is used to “on-board” or configure the smart device. For example, the TP-Link smart bulb and smart plug can be configured with the TP-Link Kasa app available on Android and iOS.


Using these apps on your phone, you can configure each device with your Wi-Fi hotspot and password to allow it to connect. You can then use the app to directly control the device. Moreover, you can create an account (with TP-Link Kasa, for example) and have your device associated with your TP-Link Kasa account. The phone/tablet can also be used to configure the Open-Q™ 212A and this will be described later.


The middle box is a PC/Laptop which can be used to configure the Open-Q™ 212A. This is also be described below.


Finally, the last piece of the Smart Home picture is the Open-Q™ 212A. The[Open-Q™ 212A Home Hub Dev Kit](https://www.lantronix.com/products/open-q-212a-home-hub-development-kit/) includes all the hardware and software you need to control and manage your smart home. Lantronix’s custom Home Hub Software software release includes support for Amazon Alexa, Google Assistant, and Mozilla WebThings Gateway as described in this[blog](https://www.intrinsyc.com/new-linux-home-hub-sw-release-available-for-the-open-q-212a/) . The[Release Notes for the Open-Q™ 212A SOM](https://www.lantronix.com/products/open-q-212a-som/#tab-docs-downloads) and[Release Notes for Open-Q™ 212A Home Hub Development Kit](https://www.lantronix.com/products/open-q-212a-home-hub-development-kit/#tab-docs-downloads) provide detailed steps on how to configure each of these services. The next few sections will describe how you can use these services to connect, control, and manage your smart home devices.


### **Configuring Your Mozilla WebThings Gateway**


You can use your PC/Laptop or mobile device to configure your Mozilla WebThings Gateway as described in the Release Notes. Once configured, your Mozilla WebThings Gateway can be accessed via the website hosted by the Open-Q™ 212A.


For the full list of capabilities of the Mozilla WebThings Gateway, please see[https://iot.mozilla.org/gateway/](https://iot.mozilla.org/gateway/)


### **Amazon Alexa Enables Voice Commands of Smart Devices**


**** As described in the Release Notes, use qualcomm-avs-v1.11.apk and your Android device to configure the Amazon Alexa services on your Open-Q™ 212A. Once configured with your Amazon account, you can use your voice to communicate with Alexa and it will access the Amazon Server.


Example: *“Alexa, what is the weather today in Vancouver, Canada?”*


As shown here, on your phone/tablet, you can add more smart devices to your Amazon account.


Once they show up on your phone/tablet, the smart home devices will automatically be detected by your Open-Q™ 212A which is logged in with the same Amazon account. Furthermore, you can now control these devices using voice commands to your[Open-Q™ 212A Dev Kit](https://www.lantronix.com/products/open-q-212a-home-hub-development-kit/) .


Example: *“Alexa, turn on the smart bulb”*


### **Voice Commands Using Google Home and Google Assistant**


**** You use ADB to configure your 212A with your Google credentials as described in the Release Notes. Once configured, you can communicate with Google assistant using your voice.


Example: *“Hey Google, what is the weather today in Vancouver, Canada?”*


Now you will add smart IoT devices to your Google account. Install Google Home on your mobile device.


Once this is done, your smart device is associated with your Google account and you no longer need your mobile device. You can control your smart home devices using voice commands to the Google Assistant running on your Open-Q™ 212A.


Example: *“Hey Google, turn on the smart bulb!”*


The[Open-Q™ 212A Home Hub Dev Kit](https://www.lantronix.com/products/open-q-212a-home-hub-development-kit/) makes a great Home Hub. This is an open and flexible software platform that allows you to expand on the built-in capabilities and customize it for your own needs. Its platform software is the Linux Home Hub SW release v1.1.1 which comes with support for Amazon Alexa, Google Assistant, and Mozilla Gateway WebThings.


The hardware in the Open-Q™ 212A Home Hub Dev Kit also complements the features of a Home Hub. It includes Wi-Fi with AP and Station modes and Bluetooth A2DP and BLE support which allows it to easily connect to your PC, laptop, phone, and other devices. The Microphone array and Fluence Pro Soundfocus multi-microphone solution can provide excellent audio input performance to hear your voice commands to Alexa and Google Assistant. Finally, the audio output is driven by the WCD9326 codec and WSA8815 amplifier which provides great sound quality for Alexa and Google assistant voice.


Now that you’ve set up your Smart Home Hub, relax and control your connected devices from your phone, smart home controller or via the Web from virtually anywhere.


Click here to learn more about our[Open Q](https://www.lantronix.com/products-class/intelligentedge/) products.
