---
schema_version: "1.0.0"
document_id: "bac3e26574972355aba380be5e0f5c2f6f0d1ee2838f82c200d238ae99ff0503"
company_key: "yc-embrace"
company: "Embrace"
source_id: "yc-embrace-news-import-2dd1f24c69b5"
canonical_url: "https://embrace.io/blog/how-does-an-anr-work/"
published_at: "2026-03-27T18:58:11+00:00"
first_seen_at: "2026-07-25T02:40:21.111018+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:0a9549a7e4c25771d2c0e3841af13f8bcecc267149fd9cd3788a75ee372f1d43"
---

# How does an ANR work?

A detailed look at how the Android OS monitors, processes, and triggers Application Not Responding (ANR) errors.


## **Key takeaways**


- An Application Not Responding (ANR) error occurs when the Android framework determines that an app has failed to respond within a component-specific timeout. The system enforces this by tracking deadlines for specific operations (such as input dispatch or broadcast delivery). If the operation has not finished by its deadline, the corresponding watchdog logic triggers an ANR.


- ANRs are triggered differently depending on the Android component involved. BroadcastReceiver, Service, ContentProvider, and Activity components each have distinct detection logic and timeout thresholds, with Activities primarily subject to input dispatch timeouts.


- ANR reports often misidentify the true cause. Blocking work earlier on the main thread can delay input handling so the Activity is flagged even though the real issue was triggered elsewhere.


- User-perceived ANRs directly impact your app’s ranking on the Play Store. ANRs triggered by input dispatch timeouts are surfaced in Android Vitals and used to assess app quality and visibility


Application Not Responding (ANR) errors are some of the most frustrating to encounter and often difficult to debug. Worse yet, if you can’t keep your ANR rate under control, your app will be downranked and less discoverable in the Google Play Store.


While Android documentation provides guidance on ANRs,


[we’ve found](https://get.embrace.io/solving-anrs-101/) that it only scratches the surface on exactly how these particular errors work in the wild.


To help you get a better handle on how to detect and capture ANR data, we asked our Android architect Jamie Lynch to describe in detail how the Android OS monitors, processes, and triggers ANRs.


The content of this post is accurate with regard to Android 12 only. The implementation may subtly vary on different versions of Android.


**


## Detecting an ANR


An Application Not Responding (ANR) error occurs when an Android app blocks the main thread for too long (approximately for about 5 seconds), preventing the system from processing user input events.


To be more precise, a


n ANR begins when the Android framework calls[AnrHelper.appNotResponding()](https://cs.android.com/android/platform/superproject/+/master:frameworks/base/services/core/java/com/android/server/am/AnrHelper.java;l=67?q=anrrecord) .


This is called from four places – once for each of the[four fundamental components](https://developer.android.com/guide/components/fundamentals#Components) of Android.


Fundamentally, the Android Framework triggers an ANR when a component has not responded to the system within an expected time threshold. The framework usually implements this by having a background thread schedule a delayed call to` AnrHelper.appNotResponding()` . If work is finished before the threshold, this call is cancelled; if work is not finished, an ANR is born.


### BroadcastReceiver


A` BroadcastReceiver` triggers an ANR if it doesn’t[complete](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/BroadcastQueue.java;l=997)` onReceive` within[10 seconds](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/BroadcastConstants.java;l=45) . This is achieved by[posting then removing](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/BroadcastQueue.java;l=1779) a` Runnable` to the` Handler` of` ActivityManagerService` .


` BroadcastReceiver` components have a[couple of unique exemptions](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/BroadcastQueue.java;l=1794) from ANRs:


1. If the system is booting, as some early broadcasts setup system services.
2. If the[timeoutExempt flag](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/BroadcastRecord.java;l=80) is set on the` BroadcastRecord` . This only seems to be enabled for the` Intent.ACTION_PRE_BOOT_COMPLETED`[broadcast](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java;l=13179) .


### Service


ANRs can trigger in two places for a[Service](https://developer.android.com/guide/components/services) :


1. A foreground service triggers an[ANR](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActiveServices.java;l=5232) if the component doesn’t call[startForeground](https://developer.android.com/reference/android/app/Service#startForeground(int,%20android.app.Notification)) in[under 10s](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActiveServices.java;l=202) . Interestingly, this is configurable via the` Build.HW_TIMEOUT_MULTIPLIER`[property](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/core/java/android/os/Build.java;l=1180) (although manufacturers aren’t supposed to alter this). Another interesting point is that doing this will crash the app, and that the main thread doesn’t necessarily have to be blocked for this to happen.
2. A regular service[triggers an ANR](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActiveServices.java;l=5181) if the component doesn’t start/bind in[under 20s](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActiveServices.java;l=197) , or in under 200 seconds. The threshold is lower if the app is[in the foreground](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActiveServices.java;l=3368) .


In both cases the ANR is triggered by[posting then removing](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActiveServices.java;l=5282) a` Runnable` to the` Handler` of` ActivityManagerService` .


### ContentProvider


A` ContentProvider` triggers an ANR if[getProviderMimeType()](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ContentProviderHelper.java;l=901) doesn’t respond within **1000ms** . This is achieved by posting then removing a` Runnable` to the` Handler` of[ActivityManagerService](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java) .


Developers can avoid ContentProvider ANRs by using the non-deprecated` getProviderMimeTypeAsync` method.


### Activity


An` Activity` triggers an ANR if[input dispatching times out](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java;l=16372) . This is the most complicated of all the components:


- The[WindowManagerService](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/wm/WindowManagerService.java;l=4742) creates an` InputManagerCallback` .
- The[native InputDispatcher](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/native/services/inputflinger/dispatcher/InputDispatcher.cpp;l=572;drc=android-12.0.0_r1;bpv=0;bpt=1) polls to check whether an ANR has occurred:


- The native[AnrTracker](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/native/services/inputflinger/dispatcher/AnrTracker.h) returns the time when the first event on the connection queue would trigger an ANR, or` LONG_LONG_MAX` if the queue is empty.
- The[InputDispatcher checks](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/native/services/inputflinger/dispatcher/InputDispatcher.cpp;l=635;drc=android-12.0.0_r1;bpv=0;bpt=1) if the current time exceeds the returned time. If this is the case it[calls into InputManagerService](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/native/services/inputflinger/dispatcher/InputDispatcher.cpp;l=5515;drc=android-12.0.0_r1;bpv=0;bpt=1) .


- The[InputManagerService](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/input/InputManagerService.java;l=2728) forwards the call onto` InputManagerCallback` .
- [InputManagerCallback](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/wm/InputManagerCallback.java) forwards the calls onto` AnrController` .
- [AnrController](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/wm/AnrController.java) maps an app/input channel token onto a` ProcessRecord` or PID. This allows for the misbehaving process to be identified, and the` AnrController` then calls` inputDispatchingTimedOut` in either` AnrRecord` or` ActivityManagerService` .
- [AnrRecord.inputDispatchingTimedOut()](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/wm/ActivityRecord.java;l=6070) calls[ActivityManagerService.inputDispatchingTimedOut()](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java;l=16372) .


#### Other interesting points around Activities


1. The developer[can trigger an ANR](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java;l=5956) directly via the` ActivityManagerService` by calling` appNotResponding()` . This API was added in[Android 11](https://developer.android.com/reference/android/app/ActivityManager#appNotResponding(java.lang.String)) for testing purposes.
2. Triggering an ANR causes all input events to be dropped (until the ANR resolves).


#### NDK code


If the NDK layer blocks the main thread this would trigger an ANR if input dispatching times out.


` SIGQUIT` does not appear to be raised anywhere obvious from within the native code so the input dispatch scenario is the only way an ANR can occur.


#### An important note on the origin of ANRs


It is very important to note that` BroadcastReceiver` and` Service` components always run on the main thread.` ContentProvider` runs on the calling thread (which is usually the main thread).


In a production app there might be dozens of these components performing work sequentially on the main thread. This can easily starve the main thread so that an` Activity` has no time to respond to input events, leading to ANRs that originate from within an` Activity` but are actually a problem in a different component!


For example, if a` BroadcastReceiver` blocks for 4.8 seconds and an` Activity` then blocks for 0.3 seconds, the` Activity` will be the culprit in the ANR stacktrace. This can make ANR error reports highly misleading when viewed in isolation. It’s a numbers game — looking at aggregate counts & metrics is the best way to tackle ANRs.


#### Why does this matter?


In practice, ANR stack traces often highlight the last piece of main-thread work, rather than the dominant contributor. As a result, fixing individual ANRs in isolation frequently fails to reduce overall ANR rates, making aggregate analysis across many occurrences essential for identifying consistently slow main-thread work.


## Capturing Activity data for an ANR


The` Activity` component stands out from other components in that it captures some initial data by calling[preDumpIfLockTooSlow()](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/wm/AnrController.java;l=168) and[dumpAnrStateLocked()](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/wm/AnrController.java;l=233) before it calls the` AnrHelper.`


### preDumpIfLockTooSlow()


This call is throttled to once per 20s and collects a ‘pre-dump’ of information if the` ActivityManager/WindowManager` monitor is blocked for >1 s. This is required to get an accurate stacktrace if either of these services are in the main thread’s callstack.


The` ActivityManagerService`[dumps the stacktraces](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java;l=3184) and performs CPU sampling, writing to the` /data/anr/` directory, which contains a maximum of 64 of the most recent ANRs.


A[stack dump](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java;l=3343) has a limit of 20s and captures[Java PIDs](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/core/jni/android_os_Debug.cpp;l=771) , followed by native PIDs, followed by an extra PIDs.


### dumpAnrStateLocked()


This collects information on the window state/app state and[caches it in an ActivityManagerService field](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/wm/WindowManagerService.java;l=6425) .


## Capturing data for an ANR


` AnrHelper` is the point at which the data sources for all Components converge and follow the same codepath.` ActivityManagerService` effectively delegates all calls to` AnrHelper` with the exception of some sanity checks that confirm the process exists.


### AnrHelper


[AnrHelper.appNotResponding()](https://cs.android.com/android/platform/superproject/+/master:frameworks/base/services/core/java/com/android/server/am/AnrHelper.java;l=67?q=anrrecord) is called which adds a` Runnable` to an` AnrConsumerThread` . The consumer thread throttles data collection by only collecting the main stacktrace if the system takes >1 minute to collect an ANR.


` AnrConsumerThread` additionally[samples the Binder](https://cs.android.com/android/platform/superproject/+/master:frameworks/base/services/core/java/com/android/server/am/AnrHelper.java;l=108?q=anrrecord) if an ANR has not occurred within the last 2 minutes, which simply[logs out information](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java;l=16722) on Binder transactions.


` AnrHelper` then calls into[ProcessRecordErrorState.appNotResponding()](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=219) .


**


AnrHelper specifically calls out that stacktraces[may not be useful](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/AnrHelper.java;l=110) if multiple ANRs are happening at once across the system. This is because capturing an ANR trace is an intensive operation and taking multiple traces causes contention for system resources.


### ProcessRecordErrorState


A lot of the ANR logic happens[in ProcessRecordErrorState](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=219) :


- CPU usage is monitored.
- ANRs are skipped at this point if:


- The device is shutting down.
- There is already an ongoing ANR.
- The app is crashing.
- The app is being killed.


- A brief entry is[written to the system EventLog](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/core/java/android/util/EventLog.java;l=363) to record the reason an ANR happened.
- A similar log is written to the[FrameworkStatsLog](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:out/soong/.intermediates/frameworks/base/core/java/statslog-framework-java-gen/gen/com/android/internal/util/FrameworkStatsLog.java;l=5078) to record the ANR happened. This is used by[Perfetto](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=275) .
- [Memory pressure information](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=355) is appended to the ANR report.
- The` ActivityManagerService` attempts to[dump the stacktraces](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=386) which are[written to the /data/anr/ directory](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java;l=3184) . A few important notes:


- This can collect more in-depth information than when Activities ‘pre dump’ the ANR trace.
- There is a[timeout of 20000ms](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java;l=3352) for the JVM to create a trace file.
- This method can return` null` if creating the ANR trace failed and` SIGQUIT` will be raised. A native tracing mechanism will capture information for the culprit’s threads instead, and no` ApplicationExitInfo` will be recorded.


- If the ANR trace file is` null` then a[SIGQUIT is raised](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=401) which is handled by a[native signal handler](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:art/runtime/signal_catcher.cc;l=151;drc=e187da0f34efdb1e23b86e0e48ef5811725ad318;bpv=0;bpt=1) that blocks the signal for other handlers.
- The` ApplicationExitInfo`[is recorded](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=406) . The` traceInputStream` contains a subsection of the full ANR trace file (specifically, it only contains information relevant to the current process).
- The system checks whether the app’s package[is still being loaded](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=410) .
- The ANR report[is added](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=479) to the[system dropbox](https://developer.android.com/reference/android/os/DropBoxManager) .
- The famous[ANR dialog](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=518) is triggered.


#### Silent ANRs


Silent ANRs can significantly inflate background process deaths without appearing in user-facing metrics or dialogs. This often leads teams to underestimate main-thread blocking in production, particularly when relying on testing or foreground-only ANR reports.


#### Why this matters


A[“silent” ANR](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=584) happens when a process is in the background and Android doesn’t consider it an interesting process (i.e. it is not showing any UI).


Silent ANRs do record an ANR trace but the collected information is restricted to the current process only. The system[immediately kills the process](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=504) without showing any dialog after the trace has been collected.


**


Developers can force showing a dialog for silent ANRs by altering Developer Options on their device.


#### User-perceived ANRs


A[user-perceived](https://developer.android.com/topic/performance/vitals/anr) ANR is effectively an ANR that a user noticed. Currently this is just an ANR that was triggered via the Input Dispatch detection mechanism. Google Play Console and others likely detect this by searching for ‘Input dispatch timed out’ in the subject string of an ANR trace file.


### Native SIGQUIT


` SignalCatcher::HandleSigquit()`[handles a SIGQUIT signal](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:art/runtime/signal_catcher.cc;l=117;bpv=0;bpt=1) which is usually sent from` ProcessRecordErrorState` . This handler gathers an ANR trace including a dump of native threads.


## Displaying an ANR dialog


### ActivityManagerService


` ActivityManagerService` registers a` Handler` on the[system UI thread](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java;l=16804) .


The Handler[reacts to an ANR message](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java;l=1553) sent via` ProcessErrorState` by calling` AppErrors.handleShowAnrUi()` .


### AppErrors


[AppErrors.handleShowAnrUi()](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/AppErrors.java;l=1050) displays the dialog to users. Whether a dialog shows or not is based on various bits of logic:


- Skip showing if it is a background ANR unless the “show background anr” developer option is enabled.
- If an ANR dialog is already showing, skip.
- If a dialog couldn’t be shown, just kill the process immediately.
- If the ANR has stopped blocking, as reported by the[AnrController](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/core/java/android/app/AnrController.java) interface. The` AnrController` interface is also used by the system to perform[custom actions](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/StorageManagerService.java;l=990) when an ANR occurs.


### User selects to kill the app or wait


[AppNotRespondingDialog](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/AppNotRespondingDialog.java;l=145) responds allows a user to either wait or kill the app.


If the user waits, the ANR state is cleared on the` ProcessRecord` and the dialog is dismissed.


[AppErrors.killAppAtUserRequestLocked()](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/AppErrors.java;l=448) is invoked when the user selects to kill the app, which then kills the process in[ProcessRecord.killLocked()](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessRecord.java;l=997) , which sends a[SIGKILL](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/core/java/android/os/Process.java;l=1170) .


## When can ANRs be discarded?


There are a lot of reasons and points during capture where an ANR could be discarded. This can generally be summed up as:


- If an app has a debugger attached (breakpoints pause execution and it would be annoying to get ANRs while debugging).
- If the component triggering the ANR can no longer be found (e.g. it has died or finished its work).
- If the process is aborting/exiting (the process record/thread/component might no longer be found).
- An ANR is already in progress.


## ANR trace file


The ANR trace file contains a lot of information about the current system state. There are two mechanisms for capturing it: a JVM mechanism that gathers a lot of information, and a native mechanism that is used as a fallback and captures less information.


Silent ANRs only collect information about the current process and skip intensive captures (such as CPU sampling). This is to avoid overwhelming the system.


The Android docs contain a[good guide](https://source.android.com/setup/contribute/read-bug-reports#anrs-deadlocks) on how to read an ANR trace. To summarize the report may contain info on:


- CPU usage of the top apps using CPU at the time of an ANR.
- Stacktraces of the most important PIDs, native PIDs, and other interesting PIDs.


### Sample ANR trace file


#### Click here to see a full ANR trace file


**


```text
Subject: Input dispatching timed out (8925275 io.embrace.embracetestsuite.demo.debug/io.embrace.embracetestsuite.ui.activities.MainActivity (server) is not responding. Waited 5000ms for MotionEvent(deviceId=5, eventTime=55757072000, source=0x00005002, displayId=0, action=DOWN, actionButton=0x00000000, flags=0x00000000, metaState=0x00000000, buttonState=0x00000000, classification=NONE, edgeFlags=0x00000000, xPrecision=30.3, yPrecision=14.8, xCursorPosition=nan, yCursorPosition=nan, pointers=[0: (881.9, 740.0)]), policyFlags=0x62000000)----- pid 1869 at 2022-03-16 16:52:00.478077189+0000 -----
Cmd line: io.embrace.embracetestsuite.demo.debug
Build fingerprint: 'google/sdk_gphone64_arm64/emulator64_arm64:12/S2B2.211203.006/8015633:userdebug/dev-keys'
ABI: 'arm64'
Build type: optimized
Zygote loaded classes=19611 post zygote classes=2136
Dumping registered class loaders
#0 dalvik.system.PathClassLoader: [], parent #1
#1 java.lang.BootClassLoader: [], no parent
#2 dalvik.system.PathClassLoader: [/data/app/~~xKnkShWWeYIQwVk0ByBZgw==/io.embrace.embracetestsuite.demo.debug-lOmWrTsgiL2dBDY-LdpSJQ==/base.apk:/data/app/~~xKnkShWWeYIQwVk0ByBZgw==/io.embrace.embracetestsuite.demo.debug-lOmWrTsgiL2dBDY-LdpSJQ==/base.apk!classes4.dex:/data/app/~~xKnkShWWeYIQwVk0ByBZgw==/io.embrace.embracetestsuite.demo.debug-lOmWrTsgiL2dBDY-LdpSJQ==/base.apk!classes3.dex:/data/app/~~xKnkShWWeYIQwVk0ByBZgw==/io.embrace.embracetestsuite.demo.debug-lOmWrTsgiL2dBDY-LdpSJQ==/base.apk!classes8.dex:/data/app/~~xKnkShWWeYIQwVk0ByBZgw==/io.embrace.embracetestsuite.demo.debug-lOmWrTsgiL2dBDY-LdpSJQ==/base.apk!classes7.dex:/data/app/~~xKnkShWWeYIQwVk0ByBZgw==/io.embrace.embracetestsuite.demo.debug-lOmWrTsgiL2dBDY-LdpSJQ==/base.apk!classes5.dex:/data/app/~~xKnkShWWeYIQwVk0ByBZgw==/io.embrace.embracetestsuite.demo.debug-lOmWrTsgiL2dBDY-LdpSJQ==/base.apk!classes9.dex:/data/app/~~xKnkShWWeYIQwVk0ByBZgw==/io.embrace.embracetestsuite.demo.debug-lOmWrTsgiL2dBDY-LdpSJQ==/base.apk!classes10.dex], parent #1
Done dumping class loaders
Classes initialized: 0 in 0
Intern table: 30906 strong; 1230 weak
JNI: CheckJNI is on; globals=382 (plus 836 weak)
Libraries: libandroid.so libaudioeffect_jni.so libcompiler_rt.so libframework-connectivity-jni.so libicu_jni.so libjavacore.so libjavacrypto.so libjnigraphics.so libmedia_jni.so libopenjdk.so librs_jni.so librtp_jni.so libsoundpool.so libstats_jni.so libwebviewchromium_loader.so (15)
Heap: 59% free, 11MB/27MB; 268784 objects
Dumping cumulative Gc timings
Average major GC reclaim bytes ratio inf over 0 GC cycles
Average major GC copied live bytes ratio 0.801921 over 4 major GCs
Cumulative bytes moved 27586968
Cumulative objects moved 574529
Peak regions allocated 83 (20MB) / 768 (192MB)
Total madvise time 1.288ms
Start Dumping Averages for 1 iterations for young concurrent copying
ProcessMarkStack:	Sum: 1.766ms Avg: 1.766ms
ScanImmuneSpaces:	Sum: 1.009ms Avg: 1.009ms
VisitConcurrentRoots:	Sum: 604us Avg: 604us
InitializePhase:	Sum: 154us Avg: 154us
ClearFromSpace:	Sum: 152us Avg: 152us
GrayAllDirtyImmuneObjects:	Sum: 125us Avg: 125us
FlipOtherThreads:	Sum: 97us Avg: 97us
SweepSystemWeaks:	Sum: 92us Avg: 92us
ScanCardsForSpace:	Sum: 60us Avg: 60us
ForwardSoftReferences:	Sum: 30us Avg: 30us
EnqueueFinalizerReferences:	Sum: 23us Avg: 23us
SweepArray:	Sum: 21us Avg: 21us
(Paused)ClearCards:	Sum: 15us Avg: 15us
RecordFree:	Sum: 14us Avg: 14us
(Paused)GrayAllNewlyDirtyImmuneObjects:	Sum: 7us Avg: 7us
VisitNonThreadRoots:	Sum: 5us Avg: 5us
FreeList:	Sum: 4us Avg: 4us
ProcessReferences:	Sum: 3us Avg: 3us
CopyingPhase:	Sum: 3us Avg: 3us
ThreadListFlip:	Sum: 3us Avg: 3us
SwapBitmaps:	Sum: 2us Avg: 2us
ResetStack:	Sum: 1us Avg: 1us
ReclaimPhase:	Sum: 1us Avg: 1us
UnBindBitmaps:	Sum: 1us Avg: 1us
MarkZygoteLargeObjects:	Sum: 1us Avg: 1us
EmptyRBMarkBitStack:	Sum: 1us Avg: 1us
ResumeRunnableThreads:	Sum: 0 Avg: 0
(Paused)SetFromSpace:	Sum: 0 Avg: 0
ResumeOtherThreads:	Sum: 0 Avg: 0
(Paused)FlipCallback:	Sum: 0 Avg: 0
FlipThreadRoots:	Sum: 0 Avg: 0
Done Dumping Averages
young concurrent copying paused:	Sum: 32us 99% C.I. 5us-27us Avg: 16us Max: 27us
young concurrent copying freed-bytes: Avg: 3792KB Max: 3792KB Min: 3792KB
Freed-bytes histogram: 2560:1
young concurrent copying total time: 4.194ms mean time: 4.194ms
young concurrent copying freed: 58397 objects with total size 3792KB
young concurrent copying throughput: 1.45992e+07/s / 925MB/s  per cpu-time: 970802000/s / 925MB/s
young concurrent copying tracing throughput: 444MB/s  per cpu-time: 444MB/s
Average minor GC reclaim bytes ratio 1.41519 over 1 GC cycles
Average minor GC copied live bytes ratio 0.12819 over 2 minor GCs
Cumulative bytes moved 1227560
Cumulative objects moved 15361
Peak regions allocated 83 (20MB) / 768 (192MB)
Total time spent in GC: 4.194ms
Mean GC size throughput: 883MB/s per cpu-time: 885MB/s
Mean GC object throughput: 1.39239e+07 objects/s
Total number of allocations 327181
Total bytes allocated 14MB
Total bytes freed 3792KB
Free memory 16MB
Free memory until GC 16MB
Free memory until OOME 180MB
Total memory 27MB
Max memory 192MB
Zygote space size 7204KB
Total mutator paused time: 32us
Total time waiting for GC to complete: 334ns
Total GC count: 1
Total GC time: 4.194ms
Total blocking GC count: 0
Total blocking GC time: 0
Histogram of GC count per 10000 ms: 0:1
Histogram of blocking GC count per 10000 ms: 0:1
Native bytes total: 56839563 registered: 676331
Total native bytes at last GC: 27970387
/system/framework/oat/arm64/android.test.base.vdex: verify
/system/framework/oat/arm64/android.hidl.manager-V1.0-java.vdex: verify
/system/framework/oat/arm64/android.hidl.base-V1.0-java.vdex: verify
Current JIT code cache size (used / resident): 247KB / 256KB
Current JIT data cache size (used / resident): 210KB / 224KB
Zygote JIT code cache size (at point of fork): 14KB / 32KB
Zygote JIT data cache size (at point of fork): 19KB / 32KB
Current JIT mini-debug-info size: 91KB
Current JIT capacity: 512KB
Current number of JIT JNI stub entries: 0
Current number of JIT code cache entries: 432
Total number of JIT baseline compilations: 588
Total number of JIT optimized compilations: 9
Total number of JIT compilations for on stack replacement: 14
Total number of JIT code cache collections: 5
Memory used for stack maps: Avg: 209B Max: 11KB Min: 16B
Memory used for compiled code: Avg: 806B Max: 27KB Min: 28B
Memory used for profiling info: Avg: 117B Max: 4368B Min: 24B
Start Dumping Averages for 636 iterations for JIT timings
Compiling baseline:	Sum: 98.770ms Avg: 155.298us
Compiling OSR:	Sum: 16.441ms Avg: 25.850us
Code cache collection:	Sum: 13.583ms Avg: 21.356us
Compiling optimized:	Sum: 9.506ms Avg: 14.946us
TrimMaps:	Sum: 5.324ms Avg: 8.371us
Done Dumping Averages
Memory used for compilation: Avg: 71KB Max: 4000KB Min: 13KB
ProfileSaver total_bytes_written=0
ProfileSaver total_number_of_writes=0
ProfileSaver total_number_of_code_cache_queries=0
ProfileSaver total_number_of_skipped_writes=0
ProfileSaver total_number_of_failed_writes=0
ProfileSaver total_ms_of_sleep=5000
ProfileSaver total_ms_of_work=0
ProfileSaver total_number_of_hot_spikes=45
ProfileSaver total_number_of_wake_ups=8*** ART internal metrics ***
Metadata:
timestamp_since_start_ms: 42214
Metrics:
ClassLoadingTotalTime: count = 36832
ClassVerificationTotalTime: count = 173048
ClassVerificationCount: count = 929
WorldStopTimeDuringGCAvg: count = 32
YoungGcCount: count = 1
FullGcCount: count = 0
TotalBytesAllocated: count = 12988048
TotalGcCollectionTime: count = 4
YoungGcThroughputAvg: count = 696
FullGcThroughputAvg: count = 0
YoungGcTracingThroughputAvg: count = 418
FullGcTracingThroughputAvg: count = 0
JitMethodCompileTotalTime: count = 997804
JitMethodCompileCount: count = 611
YoungGcCollectionTime: range = 0...60000, buckets: 1,0,0,0,0,0,0,0,0,0,0,0,0,0,0
FullGcCollectionTime: range = 0...60000, buckets: 0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
YoungGcThroughput: range = 0...10000, buckets: 0,1,0,0,0,0,0,0,0,0,0,0,0,0,0
FullGcThroughput: range = 0...10000, buckets: 0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
YoungGcTracingThroughput: range = 0...10000, buckets: 1,0,0,0,0,0,0,0,0,0,0,0,0,0,0
FullGcTracingThroughput: range = 0...10000, buckets: 0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
*** Done dumping ART internal metrics ***suspend all histogram:	Sum: 257.867ms 99% C.I. 0.021ms-236.462ms Avg: 15.168ms Max: 256.859ms
DALVIK THREADS (37):
"main" prio=5 tid=1 Runnable
| group="main" sCount=0 ucsCount=0 flags=0 obj=0x7263a8a8 self=0xb40000701cff0010
| sysTid=1869 nice=-10 cgrp=top-app sched=0/0 handle=0x71681404f8
| state=R schedstat=( 5629259124 1841174350 3359 ) utm=553 stm=9 core=0 HZ=100
| stack=0x7fc8a4c000-0x7fc8a4e000 stackSize=8188KB
| held mutexes= "mutator lock"(shared held)
at io.embrace.embracetestsuite.ui.fragments.AnrFragment.lambda$triggerLongAnr$4(AnrFragment.java:124)
at io.embrace.embracetestsuite.ui.fragments.AnrFragment$$ExternalSyntheticLambda4.run(unavailable:-1)
at android.app.Activity.runOnUiThread(Activity.java:7136)
at io.embrace.embracetestsuite.ui.fragments.AnrFragment.triggerLongAnr(AnrFragment.java:121)
at io.embrace.embracetestsuite.ui.fragments.AnrFragment.lambda$setListeners$1$AnrFragment(AnrFragment.java:69)
at io.embrace.embracetestsuite.ui.fragments.AnrFragment$$ExternalSyntheticLambda1.onClick(unavailable:-1)
at android.view.View.performClick(View.java:7455)
at android.view.View.performClickInternal(View.java:7432)
at android.view.View.access$3700(View.java:835)
at android.view.View$PerformClick.run(View.java:28810)
at android.os.Handler.handleCallback(Handler.java:938)
at android.os.Handler.dispatchMessage(Handler.java:99)
at android.os.Looper.loopOnce(Looper.java:201)
at android.os.Looper.loop(Looper.java:288)
at android.app.ActivityThread.main(ActivityThread.java:7842)
at java.lang.reflect.Method.invoke(Native method)
at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:548)
at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1003)"Signal Catcher" daemon prio=10 tid=2 Runnable
| group="system" sCount=0 ucsCount=0 flags=0 obj=0x13680238 self=0xb40000701cff6f50
| sysTid=1874 nice=-20 cgrp=top-app sched=0/0 handle=0x6e97b17cb0
| state=R schedstat=( 2100208 17114832 28 ) utm=0 stm=0 core=0 HZ=100
| stack=0x6e97a20000-0x6e97a22000 stackSize=991KB
| held mutexes= "mutator lock"(shared held)
native: #00 pc 0000000000460608  /apex/com.android.art/lib64/libart.so (art::DumpNativeStack(std::__1::basic_ostream<char, std::__1::char_traits<char> >&, int, BacktraceMap*, char const*, art::ArtMethod*, void*, bool)+120)
native: #01 pc 00000000006ffec4  /apex/com.android.art/lib64/libart.so (art::Thread::DumpStack(std::__1::basic_ostream<char, std::__1::char_traits<char> >&, bool, BacktraceMap*, bool) const+252)
native: #02 pc 000000000070799c  /apex/com.android.art/lib64/libart.so (art::DumpCheckpoint::Run(art::Thread*)+304)
native: #03 pc 000000000031b7dc  /apex/com.android.art/lib64/libart.so (art::ThreadList::RunCheckpoint(art::Closure*, art::Closure*)+628)
native: #04 pc 00000000006e6ea8  /apex/com.android.art/lib64/libart.so (art::ThreadList::Dump(std::__1::basic_ostream<char, std::__1::char_traits<char> >&, bool)+476)
native: #05 pc 00000000006e53dc  /apex/com.android.art/lib64/libart.so (art::ThreadList::DumpForSigQuit(std::__1::basic_ostream<char, std::__1::char_traits<char> >&)+360)
native: #06 pc 00000000006e4c78  /apex/com.android.art/lib64/libart.so (art::Runtime::DumpForSigQuit(std::__1::basic_ostream<char, std::__1::char_traits<char> >&)+188)
native: #07 pc 0000000000422204  /apex/com.android.art/lib64/libart.so (art::SignalCatcher::HandleSigQuit()+456)
native: #08 pc 0000000000421580  /apex/com.android.art/lib64/libart.so (art::SignalCatcher::Run(void*)+256)
native: #09 pc 00000000000b1810  /apex/com.android.runtime/lib64/bionic/libc.so (__pthread_start(void*)+264)
native: #10 pc 00000000000512f0  /apex/com.android.runtime/lib64/bionic/libc.so (__start_thread+64)
(no managed stack frames)"HeapTaskDaemon" daemon prio=5 tid=7 WaitingForTaskProcessor
| group="system" sCount=1 ucsCount=0 flags=1 obj=0x13684328 self=0xb40000701cffa6f0
| sysTid=1878 nice=4 cgrp=top-app sched=0/0 handle=0x6e96717cb0
| state=S schedstat=( 4428833 19360252 12 ) utm=0 stm=0 core=0 HZ=100
| stack=0x6e96614000-0x6e96616000 stackSize=1039KB
| held mutexes=
native: #00 pc 000000000004c25c  /apex/com.android.runtime/lib64/bionic/libc.so (syscall+28)
native: #01 pc 0000000000394f3c  /apex/com.android.art/lib64/libart.so (art::ConditionVariable::WaitHoldingLocks(art::Thread*)+148)
native: #02 pc 00000000003d84cc  /apex/com.android.art/lib64/libart.so (art::gc::TaskProcessor::GetTask(art::Thread*)+568)
native: #03 pc 00000000003d8248  /apex/com.android.art/lib64/libart.so (art::gc::TaskProcessor::RunAllTasks(art::Thread*)+32)
at dalvik.system.VMRuntime.runHeapTasks(Native method)
at java.lang.Daemons$HeapTaskDaemon.runInternal(Daemons.java:531)
at java.lang.Daemons$Daemon.run(Daemons.java:139)
at java.lang.Thread.run(Thread.java:920)
```


## ApplicationExitInfo


A subsection of the full ANR trace for historical processes can be accessed[via ApplicationExitInfo](https://developer.android.com/reference/android/app/ApplicationExitInfo#getTraceInputStream()) from Android 11 onwards. To access the full trace it’s necessary to have read access to the` /data/anr` directory; to access the portion of the trace relevant to the current process,` ApplicationExitInfo` is sufficient.


The exit info is recorded after the[trace is captured](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/services/core/java/com/android/server/am/ProcessErrorStateRecord.java;l=404) . This is fairly straightforward — the trace is gzip compressed and stored in a global circular buffer at` ${Environment.getDataDirectory()}/procexitstore` .


The trace might be:


- Overwritten by more recent process exits.
- Present if the process recovered from an ANR and then terminated due to some other reason.


### Why this matters


Because ANR traces are captured at the point of failure and under heavy system contention, they disproportionately reflect the final blocking state. This makes them poorly suited for identifying cumulative main-thread work, even when that work is the primary cause of repeated ANRs.


## "ANRs" which don’t show a dialog


Interestingly there are a couple of places that raise SIGQUIT but never show an ANR dialog. These still result in a trace being written to the` /data/anr` directory.


These don’t count towards Android Vitals metrics but are noted for completeness.


### App Slices


[App Slices](https://developer.android.com/guide/slices) use a slightly different mechanism for triggering ANRs. The system posts a[delayed SIGQUIT runnable](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:frameworks/base/core/java/android/app/slice/SliceProvider.java;l=428) so that if a slice does not pin/unpin within 2 seconds an ANR is triggered.


This mechanism is interesting as a dialog is never shown to users and the app is effectively being hosted within Google’s application process.


### Finalizer


A finalizer can[raise SIGQUIT](https://cs.android.com/android/platform/superproject/+/android-12.0.0_r1:libcore/libart/src/main/java/java/lang/Daemons.java;l=302) if it takes longer than 5000ms to finalize any one object. If this timeout occurs it will then throw an uncaught exception.


## Competing approaches of ANRs detection in production


There are a few widely used approaches for observability. These methods are better than nothing but can often give misleading or unactionable stacktraces when viewed in isolation.


### ANR trace written to the system


Google Play Console uses the ANR trace file recorded on the device. This guarantees a that each ANR should have a corresponding error report (if a user has enabled diagnostic data), but is fairly primitive as the stacktrace can be inaccurate.


Google Play console also[displays ANR KPIs](https://developer.android.com/topic/performance/vitals/anr#android_vitals) that affect the Play Store ranking.


It’s unclear how exactly this information is sent, but seems probable that Google Play Services scans the` /data/anr/` directory and possibly the process exit reasons to calculate the KPIs.


### ApplicationExitInfo


Firebase Crashlytics[use the ApplicationExitInfo](https://firebase.google.com/docs/crashlytics/troubleshooting?platform=android#anr-requires-android-11-plus) recorded on the device. This has similar constraints to Google Play’s approach and doesn’t give insight on Android versions below 11 – which tends to be where most ANRs actually occur due to lower device specs! This also complicates the calculation of ‘user-perceived’ ANRs because AEI doesn’t contain the necessary info to calculate this.


### Using a watchdog thread


[ANR Watchdog](https://github.com/SalomonBrys/ANR-WatchDog) uses a thread to post a message to the main thread. If the process doesn’t process the message within 5s then it grabs the main thread’s stacktrace.


This approach is prone to false positives and is often too late to see the real issue.


### Catching SIGQUIT


Catching SIGQUIT in a signal handler is a mechanism that allows application to get the stacktrace at the time of an ANR. However, this approach only works for foreground ANRs and suffers the same issues where the stacktrace.


## Distributed ANR sampling


Embrace takes a holistic approach by showing you an ANR flame graph in combination with the AEI approach. This shows you exactly what the main thread was doing in the few seconds before an ANR occurred.


This is achieved with distributed sampling. The SDK uses a monitor thread to check whether the main thread has been blocked for ~1 second, and if so, takes a stacktrace at regular configurable intervals.


On its own this information is not useful. However, our backend will stitch together thousands of similar sessions and constructs a flame graph which shows you exactly which methods were called in the build-up to an ANR.


This approach wins because:


1. ANRs are often caused by multiple slow method calls being invoked sequentially. The flame graph shows you exactly how long each call takes and allows you to address the dominating factor, whereas a stacktrace approach only gives you the last method call
2. It is capable of detecting when the main thread was blocked for a long time (~4.5 seconds) but an ANR didn’t technically trigger.
3. Embrace also captures` ApplicationExitInfo` and links it to flamegraphs generated from distributed samples. This helps weed out false positives that are a disadvantage of the watchdog approach.


This requires a change of philosophy from “how many ANRs occurred in my app?” to “how many times was the main thread blocked for an unacceptable amount of time and what caused it?”.


If you’d like to see how Embrace can help your mobile team identify and solve ANRs more effectively, you can[request a demo here](https://embrace.io/request-demo/) .


Put the freeze on ANRs


ANRs impact your brand and revenue. Learn how to put a stop to them with this eBook.


[Download eBook](https://get.embrace.io/put-the-freeze-on-anrs/)


Author


[Jamie Lynch](https://embrace.io/author/jamie-lynch/) Jamie Lynch is a software engineer at Embrace with a decade of experience in Android engineering. Joining Embrace in January 2022 after his tenure at Bugsnag, Jamie has been pivotal in Embrace’s Android development efforts. In his free time, he's created the world's smallest Android app, ApkGolf.


Related Content


Android


13 January 2026


• 3 min read


### [Android Performance Monitoring Best Practices](https://embrace.io/blog/android-performance-monitoring-best-practices/)


Android


3 January 2026


• 3 min read


### [Common causes of Android app crashes and how to fix them](https://embrace.io/blog/andriod-app-crash-causes/)


Learn the most common causes of Android app crashes—memory leaks, exceptions, network issues, and more—and discover practical fixes to improve app stability.


Android


2 January 2026


• 3 min read


### [Key Android performance monitoring metrics](https://embrace.io/blog/android-monitoring-metrics/)


Learn the essential Android performance metrics—startup time, UI rendering, crashes, ANRs, network speed, and resource usage—to improve app speed and stability.
