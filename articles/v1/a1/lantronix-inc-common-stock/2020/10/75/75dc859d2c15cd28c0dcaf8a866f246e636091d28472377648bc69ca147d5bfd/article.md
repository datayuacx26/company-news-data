---
schema_version: "1.0.0"
document_id: "75dc859d2c15cd28c0dcaf8a866f246e636091d28472377648bc69ca147d5bfd"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/lantronix-sensors-power-wearables-in-low-power-mode/"
published_at: "2020-10-05T05:05:37+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:a12ca013b91e074ab0e7bf3c07b70fe790572c24c661542208239e1e792ecccb"
---

# Lantronix Sensors Power Wearables in ‘Low Power Mode’

October 4, 2020


- [General](https://www.lantronix.com/blog/category/general/)


### Lantronix Sensors Power Wearables in ‘Low Power Mode’


**Helps Get Wearables to Prototype and Market Faster and Easier with Open-Q™ 2500 Development Kit**


With leading market researcher IDC predicting that worldwide wearables market will reach sales of nearly 500 million units in 2023, there is a demand to jumpstart product development in this growing market. Based on application, the wearable technology sector is segmented into lifestyle, healthcare, consumer applications, security, fitness and sports, entertainment and enterprise, and industrial.


“Wearables started out as a device for early adopters and morphed into multiple devices for the mass market,” said[Ramon T. Llamas](https://www.idc.com/getdoc.jsp?containerId=PRF002081) , research director for IDC’s[Wearables](https://www.idc.com/getdoc.jsp?containerId=IDC_P29207) team. “In the process, wearables have accomplished several objectives: enable greater convenience, surface new insights, and keep the user connected in ways that other devices – even the smartphone – have been unable to do. Continued development and innovation will cater to current customers and attract new ones in the years to come.”


Seeing this trend Lantronix entered the wearables market and began addressing the industry’s challenges. This is an example of how our team with our[Open-Q™ 2500 Development Kit](https://www.lantronix.com/products/open-q-2500-development-kit/) provides a robust design and development platform that helps developers get their wearables designs to prototype and products to market faster and easier, allowing our customers to meet the challenges of this growing market and reap the benefits.


The Lantronix Open-Q 2500 Development Kit, based on the Qualcomm® Snapdragon™ Wear 2500 platform, is ideal for low-power heterogeneous processing needed on new wearables.


The platform’s sensor core software, Sensor Manager, runs on the Hexagon DSP core to interface with the physical sensors connected through I2C and SPI. The sensor core acts like a sensor hub in the[sensor stack](https://source.android.com/devices/sensors/sensor-stack) . Since the lowest-level sensor driver runs on the Hexagon DSP, sensor data collection and processing take place in a power-efficient environment independently of ARM core activity.


The sensor core software, Sensor Algorithm Manager, supports complex sensor processing algorithms for motion detection, sensor fusing and gesture detection etc. The processed and fused sensor data is passed to ARM core using a shared memory interface. The Qualcomm Sensor Framework HAL is responsible for the interface between sensor clients and sensor core through the Qualcomm Message Interface (QMI) and shared memory.


Among its capabilities are:


- Sensor Control and Data Retrieval
- Sensor Wakeup and Power Profiling
- Sensor Batching Feature
- Sensor Test Utility


### **** **Sensor Driver Software Support and Customization**


Sensor driver software which runs within the Hexagon DSP is a part of Qualcomm proprietary code that can be customized by Lantronix. Through the Open-Q™ 2500 Development Kit and its BSP, Lantronix supports a range of reference sensor driver ICs including ST Microelectronics.


Evaluation modules containing these sensors are available from Lantronix and support these base features:


- Accelerometer and Gyro (LSM6DS)
- Magnetometer (LIS2MDL)
- Ambient Pressure (LPS22HH, LPS22HB)
- Humidity, Temperature (HTS221)


In addition, Lantronix provides software development services to help develop and integrate support for virtually any sensor interfaced to the sensor DSP using SPI, I2C, or UART protocols on custom hardware designs. Lantronix also provides engineering services for the most effective interfacing of sensors in custom designs.


Click[here](https://source.android.com/devices/sensors/sensor-types) for more information on Android sensor types.


### **Composite Sensors with Complex Behaviors**


The sensor algorithm manager within the Sensor Core DSP processes raw base sensor data and provides composite sensor devices with complex behavior. These include:


**Feature** **Purpose** **Use case scenarios** **Sensors required**


Significant Motion Detection (SMD) · Once enabled, operates even when the device is asleep


· Triggers when significant motion occurs and automatically wakes up the device


· After notification, automatically disables itself


· Enables low power motion detection for navigation/ context awareness use cases to all devices to sleep when not in significant motion state


· Low power geofencing


Accelerometer


Absolute Motion Detection (AMD) · Reports stationary state when the device is at absolute rest, for example, on a desk or table


· Makes use of the accelerometer motion detect interrupt to further reduce power


· Conserves power by stopping services or reducing frequency of operation when a device is idle and in absolute rest state


· Allows sensors and other subsystems to be shut down when the device is stationary


Accelerometer


Relative Motion Detection (RMD) · Reports stationary state when the device is not moving significantly with respect to gravity


· Uses the accelerometer motion detect interrupt to further reduce power


· Detects device stationary state relative to user when device is held steadily in the user’s hand


· Used by other algorithms, for example, gestures to detect motion of interest, such as picking up phone to answer a call


Accelerometer


### **Sensor Control and Data Retrieval**


Android applications can use the Android sensor APIs and framework to retrieve API to control the sensors and retrieve data.


The Open-Q™ 2500 Development Kit Android software image includes the “ *sns_hal_batch* ” test program. Testing includes configuring sensors for various wakeup modes and also allows sensor data to be read.


Click[here](https://developer.android.com/guide/topics/sensors/sensors_overview.html) for more information on accessing the sensor data from applications.


For example, as shown in the example below, running the **‘sns_hal_batch -l** ‘ command in the console will show all the supported sensors.


msm8909w:/ # sns_hal_batch -l


List.


HAL open
HAL module_api_version: 0x1
HAL hal_api_version : 0x256
HAL hal_id : sensors
HAL hal_name : QTI Sensors Module
HAL hal_author : Qualcomm Technologies, Inc.
get_sensors_list took 4079219 nanoseconds
Sensor list:
\[Type: 1\] android.sensor.accelerometer (non-wakeup)
Name:LSM6DS3 Accelerometer Vendor:STMicroelectronics Version:1 Handle:1
maxRange: 78.453201 resolution: 0.002396 power: 0.900000 mA
minDelay: 10000 us maxDelay: 1000000 us
fifoReservedEventCount: 3000 fifoMaxEventCount: 40960
requiredPermission:
\[Type: 35\] android.sensor.accelerometer_uncalibrated (non-wakeup)
Name:LSM6DS3 Accelerometer Uncalibrated Vendor:STMicroelectronics Version:1 Handle:22
maxRange: 78.453201 resolution: 0.002396 power: 0.900000 mA
minDelay: 10000 us maxDelay: 1000000 us
fifoReservedEventCount: 3000 fifoMaxEventCount: 40960
requiredPermission:
…
\[Type: 18\] android.sensor.step_detector (non-wakeup)
Name:Step Detector Vendor:QTI Version:2 Handle:51
maxRange: 1.000000 resolution: 1.000000 power: 0.899994 mA
minDelay: 0 us maxDelay: 0 us
fifoReservedEventCount: 150 fifoMaxEventCount: 41912
requiredPermission:
\[Type: 19\] android.sensor.step_counter (non-wakeup)
Name:Step Counter Vendor:QTI Version:2 Handle:52
maxRange: 1.000000 resolution: 1.000000 power: 0.899994 mA
minDelay: 0 us maxDelay: 2147483647 us
fifoReservedEventCount: 150 fifoMaxEventCount: 41912
requiredPermission:
\[Type: 17\] android.sensor.significant_motion (wakeup)
Name:Significant Motion Detector Vendor:QTI Version:2 Handle:55
maxRange: 1.000000 resolution: 1.000000 power: 0.899994 mA
minDelay: -1 us maxDelay: 0 us
fifoReservedEventCount: 0 fifoMaxEventCount: 0
requiredPermission:


**** *Base* sensors such as accelerometer and gyroscope have names starting with ‘LSM6DS3’ are provided by STMicroelectronics. *Composite* sensors with vendor name ‘QTI’ are provided by Qualcomm. *Composite* sensors use the raw data from the *Base* sensors and provide higher-level events and notifications. The processing of these composite sensors is done on the Hexagon DSP.


### **Sensor Wakeup and Power Profiling/Saving**


One of the key features of locating the sensor core on the Hexagon DSP is power saving. The heterogeneous processing allows the main ARM core to go into lowest power suspend state even though the Hexagon DSP is collecting and processing sensor events from the connected sensors. The Hexagon DSP can be configured to only wake up the ARM core for specific sensors and for specific thresholds.


Click[here](https://source.android.com/devices/sensors/suspend-mode) for more information on Wake-up and Non-wake-up sensors.


The following example shows how to use the *sns_hal_batch* program to configure the Significant Motion Detection (SMD) sensor to wake up the ARM core.


msm8909w:/ # sns_hal_batch
HAL open
HAL module_api_version: 0x1
HAL hal_api_version : 0x256
HAL hal_id : sensors
HAL hal_name : QTI Sensors Module
HAL hal_author : Qualcomm Technologies, Inc.
get_sensors_list took 3644479 nanoseconds
Sensors HAL TEST APP, version 1


Usage:
Choose a sensor to interact with by inputting the sensor’s type and whether or not is the wakeup version as shown in the ‘Sensors list’
Next, choose a command, by inputting one of the following characters:


a – Activate the sensor that was previously chosen.
d – Deactivate the sensor.
f – Flush the sensor.
b – Batch. The program will prompt for additional information.
e – Exit


Sensors list:


\[Type: 1\] (android.sensor.accelerometer) Name:LSM6DS3 Accelerometer Vendor:STMicroelectronics Version:1 Handle:1
. . .
Please choose a listed sensor type to interact with> 17
Do you want the wakeup sensor or the non-wakeup sensor? (w for wakeup, n for non-wakeup, d for don’t care)> w
Please choose a command (a,d,s,b,f,e)> a
Activating sensor Significant Motion Detector @ handle 55 …
Activating sensor Significant Motion Detector @ handle 55 …


At this point, the ARM core can go to its lowest-power sleep state. It will be awakened by the Hexagon DSP when there is enough accelerometer activity to be deemed “significant motion” to trigger the SMD sensor. The reporting mode of this sensor is ‘One-Shot’ and it deactivates itself after reporting the event. The user has to reactivate this sensor to get the next SMD event.


### **Sensor Batching Feature**


Waking up the system for each individual underlying sensor event, such as each Accelerometer sensor event, can be very inefficient. It may consume a lot of power and/or prevent the system from going to its lowest-power sleep state. There is a batching feature that can be used to buffer the events in sensor hub and/or hardware FIFO. With the Open-Q™ 2500 Development Kit, the batching feature is performed by the Hexagon DSP. users can configure the max_report_latency_ns which is the duration of the buffering. It is also the timeout to wake-up the system for wakeup sensors.


Click[here](https://source.android.com/devices/sensors/batching) for more information on sensor batching. Also check the sensor header file <android>/ hardware/libhardware/ include/hardware/sensors.h


The batching feature can be tested using *sns_hal_batch* utility. The following example demonstrates how to configure the accelerometer to be a wake-up source, batch the data for 1 second between events, and set the timeout to be 30 seconds before waking up the ARM core.


msm8909w:/ # sns_hal_batch
HAL open
HAL module_api_version: 0x1
HAL hal_api_version : 0x256
HAL hal_id : sensors
HAL hal_name : QTI Sensors Module
HAL hal_author : Qualcomm Technologies, Inc.
get_sensors_list took 3644479 nanoseconds
Sensors HAL TEST APP, version 1


Usage:
Next, choose a command, by inputting one of the following characters:


a – Activate the sensor that was previously chosen.
d – Deactivate the sensor.
f – Flush the sensor.
b – Batch. The program will prompt for additional information.
e – Exit


Sensors list:


. . .
Please choose a listed sensor type to interact with> 1


Do you want the wakeup sensor or the non-wakeup sensor? (w for wakeup, n for nonwakeup,
d for don’t care)> w
Please choose a command (a,d,s,b,f,e)> b
Please specify a delay period (in Hz)> 1
Please specify a timeout (in Hz)> 0.0333


d. Activate the selected sensor.


Please choose a command (a,d,s,b,f,e)> a
Activating sensor LSM6DS3 Accelerometer -Wakeup @ handle 22 …
Activating sensor LSM6DS3 Accelerometer -Wakeup @ handle 22 …


#### **Sensor Test Utility**


The sns_hal_batch test program is part of Qualcomm proprietary source code and it is available only in binary format with the Open-Q™ 2500 Development Kit. Users can utilize the open-source sensor test application available in the android tree <android>/hardware/libhardware/tests/nusensors as a reference for their own sensor test application. Lantronix’s Open-Q™ 2500 BSP version 2.1 includes a patch for the nusensor test application to support the batching feature.


For more information, contact Lantronix at[\[email protected\]](https://www.lantronix.com/cdn-cgi/l/email-protection#660f03153915070a0315260a0708121409080f1e4805090b) .
