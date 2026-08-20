---
schema_version: "1.0.0"
document_id: "e0dcc58c996d6006be0c04666bac92ffc84cd0403c7398e638e73a85dd03d338"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/running-opencl-sample-application-open-q-820-system-module-powered-qualcomm-snapdragon-820-soc/"
published_at: "2020-12-21T19:18:23+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:12a8d9a6ca39e165e372fded12a0d8926ee07fe641245458b90068c4711d3ae6"
---

# Running OpenCL Sample Application on Open-Q™ 820 System on Module, powered by Qualcomm® Snapdragon™ 820 SOC

December 21, 2020


- [Former Intrinsyc Blog Archive](https://www.lantronix.com/blog/category/former-intrinsyc-blog-archive/)
- [Technical Articles](https://www.lantronix.com/blog/category/technical-articles/)


### Running OpenCL Sample Application on Open-Q™ 820 System on Module, powered by Qualcomm® Snapdragon™ 820 SOC


Running OpenCL Sample Application on Open-Q™ 820


OpenCL (Open Computing Language) is an industry standard that allows


​​ software developers


​​ to take advantage of


​​ multiple processors in order to increase the performance of


​​ computational tasks.


​​ While originally


​​ designed for desktop computers,


​​ OpenCL


​​ has


​​ in recent years slowly


​​ been


​​ becoming


​​ more


​​ available on


​​ embedded systems


​​ as more


​​ Graphics Processing Units


​​ (GPUs)


​​ become OpenCL compatible.


​​ With the help of OpenCL,


​​ GPUs


​​ are able to


​​ perform


​​ intensive


​​ parallel programming tasks


​​ in graphics-related as well as


​​ non-graphics related computational workloads, allowing for significantly improved processing times.


​​ ​​ One of the


​​ GPUs


​​ used in smartphones and tablets that


​​ now


​​ supports OpenCL


​​ is Qualcomm’s Adreno 530, which is used in Intrinsyc’s Open-Q 820 Development Kit.


​​


In order to demonstrate the​​


decreased​​


processing time enabled by​​


OpenCL on the Adreno 530


, a


​​ sample application​​


originally​​


taken from the Sony Mobile website​​


\[


1


\]​​


will be used.


​​


(This is not available on​​


the​​


Sony website any​​


more but


​​ is available as archived pages \[


3


\], and in Github \[


4


\]).​​


Since the source code was originally targeted toward Sony Xperia devices


​​ \[


2


\]


, it has been


​​ slightly


​​ modified for


​​ use by the different hardware of


​​ the Open-Q 820.​​


The sample application​​


applies​​


OpenCL to a math intensive bilateral filter operation on a​​


noisy​​


photo. In one case, the bilateral filter is applied using OpenCL, while in the second case it is applied using Native C.


​​ While the execution time​​


improvements​​


may vary across different OpenCL-supported devices due to differences in


​​ architecture


,​​


executing​​


the​​


bilateral filter using OpenCL on the Open-Q 820​​


(GPU)​​


has demonstrated to be​​


over 300 times faster


​​ than using Native C


​​ (ARM/CPU)


.


The following sections


​​ describe our modifications to


​​ Sony’s sample application


​​ that we performed for


​​ the Open-Q 820.


​​ Section


​​ 1


​​ illustrates


​​ the expected behavior of the application


​​ after modifying it for use on the Open-Q 820. Section


​​ 2


​​ describes the steps that were taken to modify the original sample application from Sony Mobile such that the source code could be built for Intrinsyc’s Open-Q 820. Section


​​ 3


​​ provides some suggestions for when troubleshooting is required.


Note: the instructions in this blog were tested for Open-Q 820 BSP Version


​​ 4.1. These instructions may still work for other versions if OpenCL is supported on them. Please see Section


​​ 3,


​​ step 2 to check if OpenCL is supported.


# Section


​​ 1: Using the OpenCL Sample Application


Upon opening OpenCLexample1 on the device, a noisy image is shown along with three buttons.


​​


The application will run a bilateral filter on the image to reduce the noise using either OpenCL or Native C, depending on which button is pressed.


​​ Press Reset between running OpenCL and NativeC to ensure the bilateral filter is applied to the noisy image.


​​ Pressing the OpenCL button should yield a much


​​ faster


​​ processing time than pressing the NativeC button


​​ (for example, 5ms versus 1638ms). The processing time will be displayed at the top of the screen.


# Section


​​ 2: Modifying the Original OpenCL Sample Application to be built for the Open-Q 820


The following tutorial outlines the steps required to


​​ modify


​​ and build


​​ Sony’s OpenCL code example,


​​ taken from the Sony Mobile website,


​​ using Android Studio


​​ for Intrinsyc’s Open-Q 820 Development Kit.


Before initiating the steps below, ensure the latest NDK is set up by following steps at​​


[https://developer.android.com/ndk/guides/index.html](https://developer.android.com/ndk/guides/index.html)


-


Download Sony’s OpenCL code example from


​​ Github:


git clone https://github.com/alon21034/android-opencl-example.git


​​


or from archived pages of original Sony website as in \[3\].


-


Ensure NDK is set up:


-


Import openclexample1 into Android Studio as a new project:


-


In Android Studio,


​​ select​​


File


​​





​​


New


​​





​​


Import Project


​​ (Gradle, Eclipse ADT, etc)


-


Select the location of


​​ android-opencl-example/OpenCL_code_example/openclexample1


-


-


Create an import destination directory.


-


-


Click Finish with the default options selected


-


Manually copy over any files from listed under "Ignored Files" in the generated import-summary.txt file.


-


-


For example, it may be necessary to manually copy over the following from the OpenCL_code_example/openclexample1 folder to the destination directory created in step 3c:


-


/OpenCL_code_example/openclexample1/extra_libs to /OpenCLSample/app/src/main/extra_libs


-


/OpenCL_code_example/openclexample1/include/ to /OpenCLSample/app/src/main/include


-


Link C++ project with gradle:


-


Right-click the project in Android Studio ("app" in the project view) and select "Link C++ Project with Gradle"


-


-


Select ndk-build and the location of Android.mk for the Build System and Project Path, respectively.


-


Replace libOpenCL.so with the one used by the device:


-


Use adb pull to get the file directly from the device.


adb pull vendor/lib64/libOpenCL.so


-


-


Replace


​​ openclexample1/OpenCLSample/app/src/main/extra_libs/libOpenCL.so with this new libOpenCL.so file.


-


The ABI used by the Open-Q 820 dev kit is arm64-v8a, which is supported as of SDK version 27. Therefore:


-


Modify the app level build.gradle file:


-


Change values of compileSdkVersion, minSdkVersion, and targetSdkVersion to 27.


​​ (This is for Android Oreo 8.1. For other Android versions use SDK level corresponding to the version)


-


Add abiFilters ‘arm64-v8a’ to ndk (which should have been generated under defaultConfig).


-


-


Modify Application.mk:


-


Change the value of APP_ABI to arm64-v8a


-


Change the value of APP_PLATFORM to android-27


-


Change the value of APP_STL to c++_static from gnustl_static


-


-


-


Open AndroidManifest.xml file, remove below lines from Manifest file.


<uses-sdk


android:minSdkVersion="14"


android:targetSdkVersion="17" />


Sync Project with Gradle Files.


​​ (Option is on “Files” tab).


-


Make Project using​​


Build


​​





​​


Make Project


Note:


​​ If an error pops up regarding build:gradle, then install Google Maven repository and sync project.


-


Either Run on the connected device (


Run


​​





Run ‘app’


​​





Select a connected device), or Build APK (


Build


​​





​​


Build APK


), which can be installed using the method in step 2.


​​ The application will appear on the device as OpenCLexample1.


# Section


​​ 3: Troubleshooting


If you are experiencing errors during the sync or build process, please try the following:


-


Ensure you are using the latest version of Android Studio and NDK.


​​ (The steps were tested on Android Studio 3.1)


-


If you are not using


​​ Open-Q820


​​ BSP Version


​​ 4.1, or if you are not using


​​ the Intrinsyc Open-Q 820 Development Kit


​​ you may need to:


-


Check to see if the OpenCL libraries exist on the device at the below locations


​​ (For Android releases Oreo and above)


​​ before proceeding to step b. If these files do not exist, it is very likely that OpenCL is not supported for the current software version.


/vendor/lib/libOpenCL.so


/vendor/lib64/libOpenCL.so


For Android releases up to Nougat, check below locations.


/system/vendor/lib/libOpenCL.so


/system/vendor/lib64/libOpenCL.so


-


-


Use


​​ libOpenCL.so


​​ taken directly


​​ from your device as in Section


​​ 2


​​ step


​​ 6.


-


Ensure the correct ABI filter is listed


​​ under abiFilters as in Section


​​ 2


​​ step


​​ 7. You can check which ABI filter your device is using ADB in a terminal.


Note: only list the ABI filter you need, otherwise you may need to modify the extra_libs folder to include different versions of libOpenCL.so for different ABIs.


# Section


​​ 4: References


\[


1


\]​​


Rasmusson


, J.​​


(2013, October 29). OpenCL code example. Retrieved May 2, 2017, from​​


[https://developer.sonymobile.com/downloads/code-example-module/opencl-code-example/](https://developer.sonymobile.com/downloads/code-example-module/opencl-code-example/)


\[


2


\]​​


Cuhrajs, S. (2013, November 14). Boost the performance of your Android app with OpenCL. Retrieved May 2, 2017, from​​


[https://developer.sonymobile.com/knowledge-base/tutorials/android_tutorial/boost-the-performance-of-your-android-app-with-opencl/](https://developer.sonymobile.com/knowledge-base/tutorials/android_tutorial/boost-the-performance-of-your-android-app-with-opencl/) ​​


\[


3


\]​​


[https://web.archive.org/web/20160614053404/http://developer.sonymobile.com/knowledge-base/tutorials/android_tutorial/boost-the-performance-of-your-android-app-with-opencl/](https://web.archive.org/web/20160614053404/http:/developer.sonymobile.com/knowledge-base/tutorials/android_tutorial/boost-the-performance-of-your-android-app-with-opencl/)


[https://web.archive.org/web/20160422125837/http://developer.sonymobile.com:80/downloads/code-example-module/opencl-code-example/](https://web.archive.org/web/20160422125837/http:/developer.sonymobile.com:80/downloads/code-example-module/opencl-code-example/)


​​ \[


4


\]


​​


[https://github.com/alon21034/android-opencl-example](https://github.com/alon21034/android-opencl-example)


Author: Angela Rosa, Embedded Software Developer
