---
schema_version: "1.0.0"
document_id: "532d74998aa7ff3063009e2f0579ad67d2462d36e170c438f586a893a12a6fa4"
company_key: "yc-expo"
company: "Expo"
source_id: "yc-expo-rss-ee8e6cc345e2"
canonical_url: "https://expo.dev/blog/how-to-build-a-resilient-activity-tracker-with-expo"
published_at: "2026-08-17T13:45:54+00:00"
first_seen_at: "2026-08-17T16:03:46.422408+00:00"
fetched_at: "2026-08-17T16:03:47.751142+00:00"
content_hash: "sha256:348f3661237467bf52f732ecb6cfacaf643c33949731d795a3352c6f6e998393"
---

# How to build a resilient activity tracker with Expo

*This is a post from Daniel Vinojčić, a Software Engineer at Calda.*


The feature sounds simple: a user starts a walking session that tracks steps, distance, and time, walks to a destination, and scans a QR code to finish. Tap start, walk, scan, done. The hard part is everything that happens in between. A session can run for up to an hour, and people will switch apps, lock their phone, take a call along the way. The moment they do, the OS is free to kill your process without warning. If your session state lives in memory, it's gone, and so is the user's progress.


So we needed sessions that survive backgrounding, app kills, and crashes, all without leaving the Expo workflow. Here's how we got there.


## The challenge: tracking activity on mobile


Our initial implementation kept the session state in memory. It worked when the app stayed in the foreground, but broke in three ways:


- **No background execution:** JavaScript execution is tied to the app's lifecycle. When a user switches away, the OS can suspend or terminate the process at any time, with no warning and no graceful shutdown.
- **GPS noise and drift:** Raw GPS data is unreliable. Stationary users accumulated phantom distance from signal drift, and users on buses were logging kilometers of "walking" distance.
- **No crash recovery:** If the app was killed mid-session, thirty minutes of walking was simply gone.


## The goal: an unkillable session


We needed three things working together:


- **Background tracking** that keeps running without the app in the foreground.
- **GPS filtering** that produces accurate walking distance, not vehicle distance.
- **Persistence and recovery** that protects state across kills and restores it on relaunch.


### Background location tracking with TaskManager


Expo's[TaskManager](https://docs.expo.dev/versions/latest/sdk/task-manager/) lets you define tasks that execute outside the React component tree. Combined with` expo-location` , we register a background task at module level that receives GPS updates even when the app is suspended.


Here's the core of our background tracking task:


```text
import     *     as   Location   from     'expo-location'  ;     import     *     as   TaskManager   from     'expo-task-manager'  ;
const     BACKGROUND_LOCATION_TASK     =     'background-location-task'  ;     const     FALLBACK_ACCURACY_METERS     =     30  ;
const   gpsFilter   =     new     GPSFilter  (  )  ;     const   distance   =     new     DistanceAccumulator  (  )  ;     let   seededSessionId  :     string     |     null     =     null  ;
TaskManager  .  defineTask  (  BACKGROUND_LOCATION_TASK  ,     async     (  {   data  ,   error   }  )     =>     {        if     (  error  )     return  ;
const     {   locations   }     =     (  data   ??     {  }  )     as     {   locations  ?  :   Location  .  LocationObject  [  ]     }  ;        if     (  !  locations  ?.  length  )     return  ;
const   state   =     await     loadActivityState  (  )  ;        if     (  !  state  )     return  ;
if     (  seededSessionId   !==   state  .  sessionId  )     {        seededSessionId   =   state  .  sessionId  ;        distance  .  restore  (  Math  .  max  (  distance  .  totalKm  ,   state  .  distanceKm   ??     0  )  )  ;        }
for     (  const   raw   of     [  ...  locations  ]  .  sort  (  (  a  ,   b  )     =>   a  .  timestamp   -   b  .  timestamp  )  )     {          const   filtered   =   gpsFilter  .  process  (            {            latitude  :   raw  .  coords  .  latitude  ,            longitude  :   raw  .  coords  .  longitude  ,            accuracy  :   raw  .  coords  .  accuracy   ??     FALLBACK_ACCURACY_METERS  ,            speed  :   raw  .  coords  .  speed  ,            timestamp  :   raw  .  timestamp  ,            }  ,          state  .  activityPausedAt   !=     null          )  ;
if     (  !  filtered  .  accepted  )     continue  ;
const   speed   =   filtered  .  dopplerSpeedMs   ??   filtered  .  speedMs  ;          if     (  speed   !=     null  )     {          distance  .  addVelocitySample  (  speed  ,   filtered  .  dtSeconds   ??     0  )  ;          }        }
await     updateActivityState  (  {        sessionId  :   state  .  sessionId  ,        distanceKm  :   distance  .  totalKm  ,        elapsedSeconds  :     deriveElapsedSeconds  (  state  )  ,        }  )  ;     }  )  ;
```


There's no React here: no` useState` , no hooks, no component tree. The task reads and writes storage directly, and the persisted snapshot — not module state — is what a new process trusts on its first tick.


The loop walks every delivered fix, oldest first, rather than taking the newest: a locked phone gets batched deliveries, so using only the latest drops the walking in between and starves the filter of measurements.


The reseed is the whole game. When the OS reclaims the process under memory pressure and later restarts the service to deliver a batch, the JS bundle re-executes from scratch and every module-level accumulator is back at zero. Without it the session doesn't lose its stored total (the write path is monotonic, as we'll get to) but it flat-lines: every meter walked after the kill is discarded until the fresh accumulator climbs past the old one. One caveat on which kill we mean — if the *user* terminates the app, background location stops and Android won't restart it for a location event. That gap is unrecoverable, which is why the next option matters.


On iOS, we set` activityType: ActivityType.Fitness` for optimized GPS behavior. On Android, a foreground service notification keeps the task alive — with two options that are easy to get wrong:


```text
await   Location  .  startLocationUpdatesAsync  (  BACKGROUND_LOCATION_TASK  ,     {      accuracy  :   Location  .  Accuracy  .  High  ,      timeInterval  :     5000  ,      distanceInterval  :     0  ,      foregroundService  :     {        notificationTitle  :     'Activity in progress'  ,        notificationBody  :     'Tracking your walk…'  ,        killServiceOnDestroy  :     false  ,        }  ,      activityType  :   Location  .  ActivityType  .  Fitness  ,      showsBackgroundLocationIndicator  :     true  ,      pausesUpdatesAutomatically  :     false  ,     }  )  ;
```


` killServiceOnDestroy: false` is not a nice-to-have. Without it, "the foreground service keeps us alive" holds right up until the user swipes the app away, then silently stops being true.


` distanceInterval` is the subtler trap. A "sensible" 10 meters looks like an optimization, but on Android it filters the provider rather than hinting to it: your sample rate drops three- or four-fold, precisely when the screen is off and fixes are coarsest. Distance is integrated over time, so steady cadence beats culling near-duplicates.


A word on permissions, because they're easy to underestimate on this stack. iOS background tracking requires the *Always* authorization via` requestBackgroundPermissionsAsync` , not just *When In Use* . Android needs` ACCESS_BACKGROUND_LOCATION` plus, as of Android 14,` FOREGROUND_SERVICE_LOCATION` .` expo-location` adds the manifest entries automatically, but Google Play requires a written justification and review for both background location and the foreground service before your app can ship, so budget time for that review, not just the code.


Permissions also aren't the only thing that can be "granted" while tracking can't work. With location services off device-wide but every app permission intact,` startLocationUpdatesAsync` still succeeds, the notification still appears, and no fix ever arrives.` Location.hasServicesEnabledAsync()` is a separate check worth running before a session starts.


### Filtering GPS with a Kalman filter


Our early version used a 3-meter threshold: ignore any movement under 3 meters. It handled stationary drift, but not the real production problem: users on buses and in cars were racking up kilometers of "walking" distance. One user logged 128 km in a single day with only 8,600 steps.


We replaced it with a pipeline of independent gates, the same family of techniques fitness apps use to turn jittery fixes into a believable track:


1. **Accuracy gate:**` expo-location` reports accuracy in meters on every update. We reject anything over 30 meters, and anything with a non-positive accuracy (iOS reports` -1` for an invalid fix). Most implementations ignore this field, which is a missed opportunity.
2. **Speed gate:** if speed exceeds 8 m/s (~29 km/h), we drop the point — this alone eliminated most vehicle-distance inflation. The catch is that` coords.speed` is` null` on many Android fused fixes and` -1` on iOS when invalid, and a negative sentinel is worse than a missing one: every` speed < threshold` check silently reads it as *standing still* . Use the platform value when present and non-negative, otherwise derive speed from displacement over time. Track which of the two you got — one is a Doppler measurement and the other isn't, which matters below.
3. **Outlier gate:** compare each fix against the position the filter predicts. Multipath "teleport" spikes that still claim good accuracy get dropped; three consecutive rejections mean a genuine relocation (a GPS gap, a tunnel exit), so we re-anchor rather than lock out forever.
4. **Kalman smoothing:** accepted points pass through a constant-velocity Kalman filter on each coordinate axis, run in a local meters projection rather than on raw degrees (a degree of longitude isn't a fixed distance, though at city scale the approximation barely matters). Treating the axes independently ignores cross-axis correlation, but for pedestrian smoothing the simplification holds up. The filter predicts the next position from estimated velocity, then corrects with the actual reading, weighted by reported accuracy.


Then there's the step most implementations get wrong, including ours at first: **how you turn filtered positions into distance.**


The obvious approach is to sum the distance between consecutive accepted fixes, and it carries an inherent positive bias.` |Δposition|` is a norm, so it's never negative, and residual filter wobble — which always exists, even post-Kalman — accumulates the way a random walk's path length keeps growing while net displacement stays near zero. A minimum-movement threshold doesn't fix it; it just also throws away real slow motion.


We integrate speed over time instead —` distance += speed × dt` , with a 0.3 m/s deadband so noise contributes nothing, an 8 m/s ceiling, and a 10-second clamp on` dt` :


```text
addVelocitySample  (  speedMs  :     number  ,   dtSeconds  :     number  )  :     number     {        if     (  speedMs   <     MIN_INTEGRATION_SPEED_MS  )     return     this  .  accumulatedKm  ;        const   speed   =   Math  .  min  (  speedMs  ,     MAX_WALKING_SPEED_MS  )  ;        const   dt   =   Math  .  min  (  Math  .  max  (  dtSeconds  ,     0  )  ,     MAX_PREDICT_DT_S  )  ;        this  .  accumulatedKm   +=     (  speed   *   dt  )     /     1000  ;        return     this  .  accumulatedKm  ;     }
```


Which speed you integrate matters more than the integration itself. The filter's estimate comes from positions, so it inherits their error, and` hypot(vN, vE)` is a magnitude over two *uncertain* components — uncertainty that can only push the number up, never down, and that grows with fix accuracy. On the 25–30 m fixes Android returns with the screen off, it rivals the whole signal for a slow walker.


` coords.speed` is a better number, free: GNSS derives it from carrier frequency shift rather than by differencing positions, so it inherits neither position error nor multipath drift. Most implementations use it only to reject vehicles. It's the better thing to integrate, which is why the task above prefers` dopplerSpeedMs` and falls back to the filter only when the OS omits it.


Two more gates run outside the filter, because a smoothed track can be smooth and still wrong. A **net-displacement check** asks whether the device actually went anywhere over a trailing window — path length is inflated by every wobble, net displacement isn't — with a threshold that scales up as fixes coarsen. On Android a **motion-activity gate** suppresses distance unless the device reads as in locomotion; Activity Recognition works from the accelerometer and gyroscope, so multipath can't fool it the way it fools a position-based check.


` expo-location` now ships` getMotionActivityAsync` and` watchMotionActivityAsync` , wrapping the same platform APIs with per-type confidence — reach for those before writing native code. We keep our own because the step tracker gates sensor emission on locomotion *natively* , inside the Kotlin module, so the subscription has to exist there anyway; reading it from JS as well would mean running two.


## Three layers of persistence


You might reach for a single source of truth here: write to storage, call it done. But each failure mode wants a different layer, so we run three:


Layer Store What it survives Write frequency


1 Zustand (in-memory) Nothing, it's RAM Real-time


2 AsyncStorage App kills, crashes Every 30s + on background


3 Supabase Phone loss, reinstalls Every 30s + on background


The most important moment is the` AppState` transition to background, our last chance to write before the OS might kill us:


```text
AppState  .  addEventListener  (  'change'  ,     async     (  nextState  )     =>     {        const   state   =   useActivityStore  .  getState  (  )  ;        if     (  state  .  phase   !==     'running'     &&   state  .  phase   !==     'paused'  )     return  ;
if     (  nextState   ===     'inactive'  )     {          await   state  .  persistToStorage  (  )  ;        }
if     (  nextState   ===     'background'  )     {          await   state  .  persistToStorage  (  )  ;          await     flushRemoteProgress  (  )  ;        }
if     (  nextState   ===     'active'  )     {          const   persisted   =     await     loadActivityState  (  )  ;          if     (  persisted   &&   persisted  .  distanceKm   >   state  .  distanceKm  )     {          state  .  updateDistance  (  persisted  .  distanceKm  )  ;          }        }     }  )  ;
```


### Two writers, one key


Here's the failure mode that cost us the most debugging time, and it isn't the OS killing anything. The background task and the foreground store persist the same snapshot, and each holds only part of the truth: GPS distance lives in the task, steps in the foreground listener. Write the whole snapshot from either side and you revert whatever the other just did — our symptom was a *paused activity flipping back to running* , from a background write that read the state before the pause landed.


Two rules fix it: serialize every read-modify-write against the key, and never overwrite — merge, keeping the higher value for anything that must not move backwards in a session:


```text
function     mergeMonotonic  (  current  :   PersistedActivityState  ,   patch  :   Partial  <  PersistedActivityState  >  )     {        const   merged   =     {     ...  current  ,     ...  patch   }  ;        if     (  patch  .  distanceKm   !=     null  )   merged  .  distanceKm   =   Math  .  max  (  current  .  distanceKm   ??     0  ,   patch  .  distanceKm  )  ;        if     (  patch  .  steps   !=     null  )   merged  .  steps   =   Math  .  max  (  current  .  steps   ??     0  ,   patch  .  steps  )  ;        return   merged  ;     }
```


The remote layer needs the same treatment: a 30-second autosave queued before a newer one can land after it and lower the numbers your leaderboard reads. A plain` UPDATE` lets last-write-win; a` GREATEST(...)` RPC doesn't. Monotonic writes are also what make the background task's cold start safe rather than destructive — a zeroed accumulator can't erase a stored total, it can only fail to advance it, which is precisely the failure that reseeding closes.


One caveat worth being honest about: reading persistence from a *fully killed* app on iOS is the path with the sharpest edges. AsyncStorage has a long history of returning` null` for reads in a headless background task when the process was terminated. It works reliably when the app is merely backgrounded, but the killed-and-relaunched case is exactly the one this whole architecture depends on. Test it with the app force-quit from the app switcher, not just backgrounded. If you hit it,` expo-sqlite` or` expo-file-system` are more dependable stores for the data the background task has to read on a cold start.


## Recovery on relaunch


When the app starts, we check storage for an orphaned session and verify it against the backend. The subtlety is in what "verify" means when the network is down:


```text
async     function     initRecovery  (  )     {        const   persisted   =     await     loadActivityState  (  )  ;        if     (  !  persisted  )     return  ;
const     {   status  ,   session   }     =     await     lookupSessionById  (  persisted  .  sessionId  )  ;
const   canRecover   =        status   ===     'unreachable'     ||     (  status   ===     'found'     &&   session   !=     null     &&     !  session  .  ended_at  )  ;
if     (  canRecover  )     {          setRecoveryData  (  persisted  )  ;          setIsRecovering  (  true  )  ;        }     else     {          await     clearActivityState  (  )  ;        }     }
```


That three-way status is the point. The obvious implementation returns a nullable session, which collapses "the server says this session doesn't exist" into the same value as "we couldn't reach the server" — and then discards the user's walk every time they relaunch offline, which is one of the scenarios recovery exists for. Whatever backend you're on, a genuine empty result and a failed request arrive differently; keep them apart, and default to the local snapshot when you simply don't know.


If the session is recoverable, the user sees a prompt: resume or discard. Resuming hydrates the Zustand store from the persisted snapshot, restores accumulated distance, restarts background location tracking in resume mode (a plain start zeroes the accumulators, and the next GPS tick would persist 0 km), and relaunches the iOS Live Activity on the lock screen.


And when local and server state disagree, the server's` ended_at` wins: if the backend says the session already closed, we discard the local copy rather than resurrecting a finished session.


There's one more thing to restart, and it's easy to forget: the location task itself. An aggressive OEM battery manager can kill your foreground service without killing your app. Nothing re-registers the task on its own, so the rest of the session would silently record 0 km. On every return to the foreground we check whether the task is still registered and restart it in resume mode if it isn't.


## Cross-platform step tracking


Step counting is where iOS and Android diverge completely. On iOS,` expo-sensors` exposes a high-level` Pedometer` API backed by Core Motion. We poll` getStepCountAsync()` over the current step-segment window. Because Core Motion logs steps system-wide, that query returns steps accrued even while our app was backgrounded.


Android gave us no such shortcut.` getStepCountAsync()` is iOS-only, and pedometer updates aren't delivered in the background on either platform. Expo's docs point Android users at[Health Connect](https://developer.android.com/health-and-fitness/guides/health-connect) , which is worth evaluating first — we needed live per-session counts and a baseline surviving process death, so we went to the sensor directly.


We built a custom[Expo Module](https://docs.expo.dev/modules/overview/) in Kotlin wrapping` SensorManager` , following[Android's recommended approach](https://developer.android.com/health-and-fitness/fitness/basic-app/read-step-count-data) . It tries` STEP_COUNTER` first (cumulative, resets only on reboot, but can batch readings by several seconds) and falls back to` STEP_DETECTOR` . Both need the` ACTIVITY_RECOGNITION` runtime permission on Android 10+. We also gate emission on Google Play's Activity Recognition API via` ActivityRecognitionClient` , so steps are suppressed when the device reads as stationary or in a vehicle.


A single hook abstracts the platform difference. Note the shape of it — this is another place where async and cleanup bite:


```text
function     startAndroidStepTracking  (  )  :     (  )     =>     void     {        let   isActive   =     true  ;        const   removeListener   =   stepTracker  .  addStepListener  (  onSteps  )  ;
void     (  async     (  )     =>     {          const   started   =     await   stepTracker  .  startTracking  (  )  ;          if     (  !  isActive   &&   started  )     await   stepTracker  .  stopTracking  (  )  .  catch  (  (  )     =>     {  }  )  ;        }  )  (  )  ;
return     (  )     =>     {        isActive   =     false  ;          removeListener  (  )  ;          void   stepTracker  .  stopTracking  (  )  ;        }  ;     }
```


Each platform's start function returns a **synchronous** teardown. Returning the result of an` async` function from` useEffect` hands React a Promise it will never call, and the native listener survives the unmount — a leak that shows up as steps accumulating into a session the user already ended.


A live listener has the same weakness as in-memory state: it only counts while your process is alive, so a locked 20-minute walk on an aggressive OEM build silently undercounts.` STEP_COUNTER` fixes it — cumulative since boot, maintained in the sensor hub, independent of your process. We record its value at the session's first event as a baseline and take the delta on every foreground return, bounded by the time the GPS pipeline actually saw walking-speed movement so an unfiltered counter can't inflate the total.


## Results


This system has been running in production with real users completing walking sessions across a city.


- **No lost sessions** since the persistence, monotonic-merge, and recovery work landed.
- **Accurate distance tracking.** Gating on accuracy and speed, then integrating velocity instead of summing position deltas, eliminated both vehicle-distance inflation and slow phantom accumulation — the user who previously logged 128 km daily now shows accurate walking numbers.
- **Distance that matches dedicated tracking hardware.** Integrating the platform's Doppler speed rather than a position-derived velocity is what closed the gap on slow walks, where a position-derived speed is least reliable. Walking a route side by side with other tracking units now produces near-identical distances.
- **Clean recovery.** Sessions survive backgrounding, app kills, and crashes, online or off. Users resume exactly where they left off.


## Where this can still fail


Resilience means surviving the process dying, not preventing it, and the OS doesn't always let you keep running. On Android, stock OS is the easy case; the variability comes from OEM battery managers. Xiaomi/MIUI, Huawei/EMUI, Samsung's One UI, Oppo, and Vivo ship aggressive task-killers that can stop a foreground service even when you've done everything right ([dontkillmyapp.com](https://dontkillmyapp.com/) tracks the per-vendor behavior, and Expo's background docs link there too). We mitigate with a persistent foreground-service notification,` killServiceOnDestroy: false` , a battery-optimization exemption prompt where it matters, and the foreground self-heal check — but we treat continuous execution there as best-effort. Doze is the gentler, stock-Android version of the same pressure.


iOS is more predictable but not unlimited: it can still suspend a long session, and since iOS 16.4 you have to keep continuous updates configured correctly to stay alive:` kCLDistanceFilterNone` and ~100 m accuracy or better, or the background indicator enabled. Anything coarser and updates quietly stop.


Distance has a residual limitation worth naming too. Accuracy on slow walks leans on the platform supplying a Doppler speed; when it doesn't, we fall back to the position-derived velocity, whose uncertainty grows with fix accuracy. The gates bound that rather than eliminate it. Tightening it further means fusing step cadence with a GPS-calibrated stride length, the approach dedicated fitness hardware takes.


So we don't try to win by staying alive. When the OS cuts us off, the persistence layers and recovery prompt turn "killed" into "resumable" — which is the whole point.


## Why this matters for Expo


Building a resilient activity tracker requires background tasks, native sensor APIs, real-time widgets, and multi-layer persistence across two platforms. Four years ago, this would have meant ejecting from Expo. That’s obviously no longer the case.


[TaskManager](https://docs.expo.dev/versions/latest/sdk/task-manager/) runs our GPS tracking outside the app lifecycle. The[Expo Modules API](https://docs.expo.dev/modules/overview/) let us write a native Android step tracker in Kotlin without leaving the Expo workflow. For the Live Activity, we used[@bacons/apple-targets](https://github.com/EvanBacon/expo-apple-targets) to generate the widget extension as a native Apple target through Continuous Native Generation, so the ActivityKit code lives outside` /ios` and survives every prebuild — and that path is shorter today than when we walked it, with iOS widgets and Live Activities stable in[expo-widgets](https://docs.expo.dev/versions/latest/sdk/widgets/) since SDK 56.[EAS Build](https://docs.expo.dev/build/introduction/) handles the multi-target compilation (main app and widget extension) without manual Xcode configuration.


The architecture comes down to one principle: **don't trust the process.** The app will get killed, GPS will lie, the network will drop, and two of your own writers will race each other. Every layer (background tasks, GPS filtering, monotonic local persistence, monotonic remote sync, recovery) works independently, so if any one fails, the others still protect the user's progress.
