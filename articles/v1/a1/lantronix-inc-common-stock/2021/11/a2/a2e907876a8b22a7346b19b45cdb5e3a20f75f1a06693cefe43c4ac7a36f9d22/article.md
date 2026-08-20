---
schema_version: "1.0.0"
document_id: "a2e907876a8b22a7346b19b45cdb5e3a20f75f1a06693cefe43c4ac7a36f9d22"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/open-q-610-device-ai-demo/"
published_at: "2021-11-04T16:13:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:95f1e27e7a68aa673d1339ff5c597a3371b91b2bbca2d3ffd951f5c43cdd6039"
---

# Open-Q 610 On-Device AI Demo

November 4, 2021


- [General](https://www.lantronix.com/blog/category/general/)
- [Industry Trends](https://www.lantronix.com/blog/category/industry-trends/)
- [Solutions](https://www.lantronix.com/blog/category/solutions/)
- [Technical Articles](https://www.lantronix.com/blog/category/technical-articles/)


### Open-Q 610 On-Device AI Demo


**By: Russell Fretenburg, Technical Product Manager, Lantronix, Inc.**


Lantronix recently introduced the[Open-Q 610 µSOM](https://www.lantronix.com/products/open-q-610-usom/) and[Development Kit](https://www.lantronix.com/products/open-q-610-usom-development-kit/) , based on the[Qualcomm QCS610](https://www.qualcomm.com/products/qcs610) system-on-chip (SoC), and this article provides detailed instructions on how to install and run a couple of demos to show off the powerful capabilities of the platform out of the box. The QCS610 is built specifically for advanced IoT vision intelligence applications, with exceptional power and thermal efficiency, advanced image signal processors, digital signal processor, connectivity, and security. The QCS610 platform is designed to support the following key capabilities:


- Multiple options for AI inference engines
- Heterogeneous computing (CPU/GPU/DSP) for on-device machine learning
- Dual ISPs and 4K ultra-HD video
- RTSP streaming and effortless connectivity using Wi-Fi or native Ethernet interface.


The platform SW is a Linux release based on[Yocto](https://www.yoctoproject.org/) Thud (v2.6) with built-in support for the[GStreamer](https://gstreamer.freedesktop.org/) multimedia framework, including Qualcomm-added GStreamer elements for control of QCS610 specific features, such as camera source, HW encoders and decoders, Wayland display sink, and various AI-specific elements.


As described in the Qualcomm blog post, “[On-device AI with Developer-ready SW Stacks](https://www.qualcomm.com/news/onq/2021/03/31/device-ai-developer-ready-software-stacks) ”, the Qualcomm QCS610 includes powerful AI capabilities, right out the box. Combining this full-featured SW with the Lantronix Open-Q 610 µSOM Dev Kit and available camera and display accessories enables demonstration of the AI framework with minimal effort.


This article explains how to set up the Open-Q610 µSOM Dev Kit with camera and LCD accessories, how to connect it to your network, and how to run a couple of GStreamer demos to show how easy it is to get started with multimedia and AI use-cases on the Open-Q 610 µSOM Dev Kit.


## **Video**


This Tech Talk demonstrates the ease with which users can get started on vision AI projects with the QCS610 SoC and Linux AI software stack, with the Lantronix Open-Q 610 µSOM Dev Kit and accessories.


[Tech Talk: On-device Artificial Intelligence](https://gateway.on24.com/wcc/eh/1987199/lp/3500202/tech-talk-on-device-artificial-intelligence)


# Setup and Demo Instructions


## **1.** **Prerequisites**


- Lantonix Open-Q 610 µSOM Development Kit with the **OpenQ-610_Linux-v1.1** software release installed.


- A PC with ADB utilities installed (either Windows or Linux)


- A PC, phone, or other device with VLC video player installed (https://www.videolan.org/)


## **2.** **HW Setup:**


- Connect the two camera accessories to the dev kit CAM1 and CAM2 connectors


- Connect the USB cable between the Type-C connector on the dev kit to your PC


- Connect the 12V power adapter to the dev kit


See Figure 1 below for a picture of the setup with Dev Kit, LCD, two cameras, and a Linux PC for the ADB connection.


*Figure 1: Demo Setup with Two Cameras Attached*


## **3.** **Connect with ADB and prepare target filesystem:**


Assuming you are starting with a new Open-Q 610 dev kit with freshly installed OS SW (either fresh from the factory or if you have reinstalled it), a few steps are required before we can copy files to it or make other changes:


- Confirm that you can connect to the 610 dev kit with ADB – should list the serial number of the SOM on your dev kit:


$ adb devices


List of devices attached


085300001 device


- Next, a few steps are required to disable DM-Verity security (this must be done only once on a freshly installed OS):


$ adb root


$ adb disable-verity


Slot is _a


Verity disabled on /


Now reboot your device for settings to take effect


$ adb reboot


- After it has rebooted, remount the target filesystem as read-write so that you can copy files to it:


$ adb shell


/ # mount -o rw,remount /


/ # exit


$


- Now it’s ready to copy files from your PC onto the target device.


## **4.** **Copy required scripts to target device**


Four shell scripts are used for this project and they are available online from the Lantronix Open-Q 610 Embedded Compute Portal at:[https://tech.intrinsyc.com/projects/open-q-610-usom-dev-kit/files](https://tech.intrinsyc.com/projects/open-q-610-usom-dev-kit/files)


Open-Q_610_Streaming_Demo_Scripts.zip


- After downloading and unzipping these scripts, you should see the following files:


dspl_init.sh


wifi_st_init.sh


dual-cam_prv.sh


dual-cam-tflite_to_rtsp.sh


- Copy these four files onto the 610 dev kit:


$ adb push dspl_init.sh /home/root/


$ adb …


- Do the same for all four files.
- Now make these files executable on the target device:


$ adb shell


/ # cd /home/root


/ # chmod 755 dspl_init.sh


/ # chmod …


- Do the same for all four files.


## **5** . **Wi-Fi Setup:**


- Open an adb shell on your PC:


$ adb shell


/ #


- Edit the wpa_supplicant.conf file to correspond to your Wi-Fi access point SSID and passphrase, then save it.


/ # vi /etc/misc/wifi/wpa_supplicant.conf


network={


ssid=” **YOUR_SSID_NAME** “


proto=RSN


key_mgmt=WPA-PSK


pairwise=CCMP


group=CCMP


psk=”PASSWD”


}


- Next, run the Wi-Fi initialization script to connect to your access point and start the dhcp client:


/ # cd /home/root


~ # ./wifi_st_init.sh


- You should see something like this (details will depend on your specific Wi-Fi access point):


Successfully initialized wpa_supplicant


udhcpc: started v1.29.3


udhcpc: sending discover


udhcpc: sending discover


udhcpc: sending select for 10.0.0.32


udhcpc: lease of 10.0.0.32 obtained, lease time 86400


/etc/udhcpc.d/50default: Adding DNS 10.0.0.1


~ #


## **6. Display Initialization:**


- To start the LCD display run the dspl_init.sh script to initialize the Weston display server


~ # ./dspl_init.sh


- After a lot of messages in the terminal you should finally see this message at the bottom and see the LCD panel change from black to a gray pattern.


Create output successful


Disable tool bar


~ #


## **7. Dual Camera Preview on LCD:**


Now that the basics are initialized, we can demonstrate a dual concurrent camera use-case that uses a gstreamer pipeline to stream video from each camera, combine them into one stream using the qtivcomposer gstreamer plugin, and then display the combined video on the LCD panel.


Figure 2 below shows a flow chart of the gstreamer pipeline created by this script.


*Figure 2: Dual Camera Preview Flowchart*


- Start the demo by running the dual-cam_prv.sh script:


~ # ./dual-cam_prv.sh


- After a few seconds and a bunch of messages in the terminal you should see two images side-by-side on the LCD – one from each camera.


- Stop the script with Control-C


- See Figure 3 below for the expected


** *Figure 3: Dual Camera Preview Display on LCD*


## **8. Dual Camera Tensorflow Lite Streaming Demo:**


Taking things a few steps further, the next demo takes the video streams from the two cameras, runs them through a Tensorflow Lite live camera inferencing model, adds object boxes and labels, combines the two video streams into one, and streams the result over the network via rtsp. The resulting stream can be viewed using the VLC video player on another device connected to the network.


Figure 4 below shows a flow chart of the gstreamer pipeline created by this script.


*Figure 4: Dual Cam TF-Lite Flowchart*


*•* This demo uses an example TF-Lite model from Google (https://www.tensorflow.org/lite). First, we need to download the model files and copy them onto the 610 dev kit.


• Download the coco ssd mobilenet model onto your PC from here:
https://storage.googleapis.com/download.tensorflow.org/models/tflite/coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip


• Unzip coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip.


• Copy the model and labels files onto the target device:
adb push detect.tflite /data/misc/camera/.
adb push labelmap.txt /data/misc/camera/.


• Now start the demo script to set up the gstreamer pipeline and start streaming. It will stream at the IP address of your Wi-Fi interface on port 8900.


$ adb shell
/ # cd /home/root
~ # ./dual-cam-tflite_to_rtsp.sh


Ctl-C to stop


Gbm_create_device(156): Info:backend name is: msm_drm
Stream ready at rtsp:// <dev kit IP addr>:8900/live
…
…
Gbm_create_device(156): Info: backend name is: msm_drm


• Note the address output above that you will need to receive the stream in the next step.


• Now you can connect to the stream from a PC, phone, or tablet connected to the same network as your 610 dev kit, using the VLC video player.


• Select “Open Network Stream” from the “Media” menu and enter the following:
rtsp://<dev kit IP addr>:8900/live


• You should now see something like the below image from the VLC player – two separate video streams with objects detected and labeled. **


*Figure 5: Dual Camera TensorFlow Lite Demo Output*


## **9. Summary:**


This article has shown how quick and easy it is to get started with embedded on-device AI applications using the Lantronix Open-Q 610 µSOM Dev Kit and accessories running the Qualcomm QCS610 AI SW stack. We have seen how multimedia and AI use-cases can be prototyped quickly using a few lines to set up a gstreamer pipeline. Many different configurations can be set up similarly with different example TensorFlow Lite models from Google, such as image classification, pose estimation, segmentation. The QCS610 SW stack also supports executing ML models from the[Qualcomm Neural Processing SDK](https://developer.qualcomm.com/software/qualcomm-neural-processing-sdk) to provide the flexibility of developing your models on a variety of industry standard frameworks such as TensorFlow, TensorFlow Lite, ONNX, and Caffe2.


If you do not yet have an Open-Q 610 µSOM Development Kit you can get yours here:


[https://shop.intrinsyc.com/products/open-q-610-usom-development-kit](https://shop.intrinsyc.com/products/open-q-610-usom-development-kit)


## **10. References:**


- **Open-Q 610** **µSOM Development Kit web page:**


- [https://www.lantronix.com/products/open-q-610-usom-development-kit/](https://www.lantronix.com/products/open-q-610-usom-development-kit/)


- **Open-Q 610** **µSOM Development Kit documentation** :


- [https://tech.intrinsyc.com/projects/open-q-610-usom-dev-kit/documents](https://tech.intrinsyc.com/projects/open-q-610-usom-dev-kit/documents)
- (Development Kit purchase and registration required)


- ***“QCS610/QCS410 Linux Platform Development Kit Quick Start Guide”***


- Qualcomm doc # 80-PL631-300
- Available from[https://www.qualcomm.com/products/qcs610/documentation](https://www.qualcomm.com/products/qcs610/documentation) with www.qualcomm.com login.


Author: Russell Fretenburg, Technical Product Manager, Lantronix, Inc.
