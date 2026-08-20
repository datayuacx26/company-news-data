---
schema_version: "1.0.0"
document_id: "62bfc420e81aabc1acd6588fc159517ee2c0aa467d815290beda7b396400e9b5"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/new-capability-flags"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:9b0e1bb795e732508d247dbce0d33074e2e7167b411cf2d695a395df4ea41aff"
---

# New Capability Flags

Seam’s goal is to provide a streamlined developer experience to the fragmented world of IoT devices and their APIs. This is a complex goal to achieve because each device brand, model, and set of operating conditions creates an ever-growing matrix of edge cases. However, as experts on these nuances, we constantly work to improve the Seam API to give you clearer, more granular, and more intuitive visibility into what each device can do. We make it easy for you to create awesome applications that best leverage these devices.


**To that end, we are excited to announce our latest API feature: capability flags.**


Seam capability flags enable you to see both what your device *should* be able to do and what it *can currently do* –regardless of the device manufacturer.


For example, if your application needs to display an unlock button to your users, you can use the new` device.can_remotely_unlock` capability flag to check if a specific device supports this action. Similarly, you can use` can_program_online_access_codes` if you need to program PIN codes.


We’re introducing these flags as a successor to the`[capabilities_supported](https://docs.seam.co/latest/api-clients/devices#capabilities_supported-values)` enums, which were useful but lacked precision and granularity. The new flags are computed in real time on each device and go much further in determining the precise functionality of a specific device instance.


Use capability flags before performing specific actions in your app. For example, before performing a[remote unlock](https://docs.seam.co/latest/capability-guides/smart-locks/lock-and-unlock#unlocking-a-door) operation, you can check to make sure that the target device supports remote unlocking.


```text
1  if     (  device  .  can_remotely_unlock  )     {
2      await   seam  .  locks  .  unlockDoor  (  device  )
3   }
```


It is also easy to use capability flags in your UI.


```text
1  <  LockButton
2    disabled  =  {  !  device  .  can_remotely_lock  }
3    deviceId  =  {  device  .  device_id  }
4   /  >
```


For details and code samples, see the corresponding[capability guides](https://docs.seam.co/latest/capability-guides/device-and-system-capabilities) .


# What are capabilities? Why are they important?


Each device that you connect to Seam has a specific set of things that it can do. These *capabilities* define what actions you can perform through the Seam API.


For example, some devices support[remote unlock actions](https://docs.seam.co/latest/capability-guides/smart-locks/lock-and-unlock) , while others support[programming access codes](https://docs.seam.co/latest/capability-guides/smart-locks/access-codes) . Some devices support both of these capabilities. When developing your application, it’s imperative to be able to identify the capabilities of each device so that your app logic and UI can correctly reflect a device’s abilities to perform a specific function.


For example, if a device supports programming online access codes, your app can call` seam.access_codes.create` on the device or present the`[<CreateAccessCodeForm/> Seam Component](https://docs.seam.co/latest/ui-components/overview/react-components/create-access-code-form)` to your user. On the other hand, if a connected device does not support the remote unlock action, you cannot call` seam.locks.unlock_door` , and you should also not display unlock functionality for your app user.


If you’ve used the Seam API before, you’re likely already familiar with`[capabilities_supported](https://docs.seam.co/latest/api-clients/devices#capabilities_supported-values)` . This array of device capabilities was a good way for us to get started with reporting what a device can do in its current state, such as` lock` ,` access_code` , and` thermostat` . However, over time, we learned that these` capabilities_supported` values were too broad.


For example, *what, exactly, is a lock?* If a device loses power, is the lock still a lock? Obviously, yes, but we needed to represent the difference between a device model’s capability and the instance’s capability in its current state.


Also, not all devices that are marketed as locks have exactly the same capabilities, and the` capabilities_supported` array did not handle these complex situations well. We knew that we needed a better solution that would provide more specificity and improved developer experience through Boolean flags.


# How are the new capability flags different?


To start, the new capability flags are no longer string enums. Instead, each capability flag is a top-level Boolean property of the`[device object](https://docs.seam.co/latest/api-clients/devices#device-properties)` . Seam computes each capability flag in real time for each device. Our implementation of capability flags provides your application with maximum precision on the current capabilities of a specific device instance.


All capability flags share the following behavior:


- If` true` , the device has the stated capability, and this capability is functional on this device instance.
- If` false` , the device has the stated capability, and the capability is not currently functional.
- If the flag is not returned, the device does not have this capability, regardless of its current state, software configuration, or physical setup.


A capability flag might be` false` if the device is currently offline or when a required hardware accessory is not connected to the device (see image below).


See the device[errors](https://docs.seam.co/latest/api-clients/devices#device-error-types) and[warnings](https://docs.seam.co/latest/api-clients/devices#device-warning-types) for more details about the cause of this issue. You can also examine the[properties](https://docs.seam.co/latest/api-clients/devices#device-properties) of the device and[events](https://docs.seam.co/latest/api-clients/events#event-types) related to the device to learn more about the cause of this issue.


# The first four flags


For this feature launch, we are releasing four capability flags that work uniformly across all the existing devices and providers that we support.


- ` can_remotely_unlock` : Indicates whether the device can perform a[remote unlock operation](https://docs.seam.co/latest/capability-guides/smart-locks/lock-and-unlock) . (see example below with an Igloo lock and its bridge)
- ` can_remotely_lock` : Indicates whether the device can perform a[remote lock operation](https://docs.seam.co/latest/capability-guides/smart-locks/lock-and-unlock) .
- ` can_program_online_access_codes` : Indicates whether the device can[program online access codes](https://docs.seam.co/latest/capability-guides/smart-locks/access-codes) . If` true` , it is currently possible to create new online access codes for the device, and Seam programs the device the next time it's online.
- ` can_program_offline_access_codes` : Indicates whether the device can[program offline access codes](https://docs.seam.co/latest/capability-guides/smart-locks/access-codes/offline-access-codes) . When this flag is` true` , Seam can generate an offline code for this device, regardless of the current online status of the device.


# Next steps


We are actively developing additional capability flags to provide you with even more robust capability checking abilities for your app. Our plan is to introduce these new capability flags across all devices and brands that Seam currently supports, including thermostats, sensors, and access systems.


This feature is still in beta. If you encounter an inconsistent behavior, please let us know by contactingsupport@getseam.com , and we will investigate further.
