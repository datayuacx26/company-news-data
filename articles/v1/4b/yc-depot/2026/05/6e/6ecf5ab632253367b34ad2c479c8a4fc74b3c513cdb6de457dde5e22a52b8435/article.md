---
schema_version: "1.0.0"
document_id: "6ecf5ab632253367b34ad2c479c8a4fc74b3c513cdb6de457dde5e22a52b8435"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-nested-virtualization-on-depot-ci"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:50:41.166574+00:00"
content_hash: "sha256:f8926fd752862de14c9dbee3fdfd4deef859ec715bf5734605ce5b29ef480c40"
---

# Now available: Nested virtualization support on Depot CI sandboxes

Depot aims to be the fastest and most versatile CI out there, which means constantly shipping the features people are asking for.


We also build Depot on Depot, so most of what we ship is something we end up using ourselves.


Nested virtualization is no exception: it's been one of our most requested features for a while, and turning it on unlocks a lot of use cases for our users and for our own team.


## Use cases


The most obvious use case is running the Android Emulator at native speed for tests, but nested virtualization opens up plenty of others: spinning up VMs for end-to-end tests (a full Kubernetes cluster, for example), running QEMU/KVM workloads inside a job, or doing hypervisor development.


Since the emulator is the headline use case, Depot CI ships with a ton of Android tooling out of the box. The base image ships with the full Android SDK surface preloaded, so you can target almost any combination of API level, architecture, and form factor without installing anything at build time.


### Emulator system images


Available for API` 10` through` 36` (plus` 36.1` ,` 37.0` , Baklava, CinnamonBun, CANARY), across:


- **Architectures** :` arm64-v8a` ,` x86_64` ,` x86` ,` armeabi-v7a` (plus MIPS on very old API levels)
- **Form factors** : phone, tablet, Android TV, Google TV, Wear OS (3 through 6.1), Android Automotive (including Distant Display), Android Desktop, Google Play XR
- **Variants** :` default` ,` google_apis` ,` google_apis_playstore` ,` aosp_atd` ,` google_atd` , plus pre-release 16 KB page-size builds for API 35+


### Build and command-line tools


- **Build-tools** : every release from` 19.1.0` through` 37.0.0-rc2` , including RC builds for` 32` ,` 34` ,` 35` ,` 36` , and` 37`
- **Platform-tools** :` 37.0.0`
- **Command-line tools** : versions` 1.0` through` 20.0` , plus` latest`
- **Emulator** :` 36.5.11`


### Native development


- **NDK (side-by-side)** : every release from` 16.1.4479499` through` 30.0.14904198 rc1`
- **NDK bundle** :` 22.1.7171670` for legacy setups
- **CMake** :` 3.6` ,` 3.10` ,` 3.18` ,` 3.22` ,` 3.30.x` ,` 3.31.x` ,` 4.0.x` ,` 4.1.x`


### API levels and sources


- **Stable platforms** :` android-7` through` android-36`
- **Extension SDKs** :` 33-ext4/5` ,` 34-ext8/10/11/12` ,` 35-ext14/15` ,` 36-ext18/19`
- **Versioned previews** :` android-36.1` ,` android-37.0`
- **Codename previews** : Baklava (with` ext19` ), CinnamonBun (with` ext23` ), CANARY
- **Sources** : every release from` android-15` through` android-37.0`


### Extras


Google Play services, Google Repository, Android Support Repository, Android Auto Desktop Head Unit, Android Auto API Simulators, Google Web Driver, Play Licensing Library, Play APK Expansion library.


## How to get started


Nested virtualization is enabled by default on every Depot CI sandbox, with no extra configuration and no extra cost. That said, we recommend at least a 4 vCPU sandbox type for reasonable emulator performance.


To help you get started, we put together a small example workflow that installs the Android tooling, boots the emulator, and runs Gradle tests against it. Depot Cache handles the Gradle cache so subsequent runs stay fast.


```text
permissions  :
contents  :   'read'
id-token  :   'write'


jobs  :
android-espresso-test  :
runs-on  :   depot-ubuntu-24.04-4
steps  :
-   name  :   Checkout repo
uses  :   actions/checkout@v4


-   name  :   Set up JDK
uses  :   actions/setup-java@v3
with  :
distribution  :   'zulu'
java-version  :   '17'


-   name  :   Setup Gradle
uses  :   gradle/actions/setup-gradle@v4


-   name  :   Setup Android SDK
uses  :   android-actions/setup-android@v3


-   name  :   Install emulator and system image
run  :   sdkmanager "emulator" "system-images;android-34;google_apis;x86_64"


-   name  :   Create AVD
run  :   |
echo "no" | avdmanager create avd \
-n test_avd \
-k "system-images;android-34;google_apis;x86_64" \
--device "pixel_6" \
--force


-   name  :   Start emulator
run  :   |
"${ANDROID_HOME}/emulator/emulator" -avd test_avd \
-no-window -no-audio -no-boot-anim \
-gpu swiftshader_indirect -accel on &


-   name  :   Wait for emulator boot
run  :   |
"${ANDROID_HOME}/platform-tools/adb" wait-for-device
timeout 300 bash -c 'while [[ "$("${ANDROID_HOME}/platform-tools/adb" shell getprop sys.boot_completed 2>/dev/null)" != "1" ]]; do sleep 5; echo "  still booting..."; done'
echo "=== Emulator booted ==="
"${ANDROID_HOME}/platform-tools/adb" shell getprop ro.build.version.sdk
"${ANDROID_HOME}/platform-tools/adb" shell getprop ro.product.cpu.abi


-   name  :   Disable animations
run  :   |
"${ANDROID_HOME}/platform-tools/adb" shell settings put global window_animation_scale 0
"${ANDROID_HOME}/platform-tools/adb" shell settings put global transition_animation_scale 0
"${ANDROID_HOME}/platform-tools/adb" shell settings put global animator_duration_scale 0


-   name  :   Configure Depot build cache
run  :   |
mkdir -p ~/.gradle/init.d
cat > ~/.gradle/init.d/depot-cache.gradle << 'EOF'
gradle.settingsEvaluated { settings ->
settings.buildCache {
remote(HttpBuildCache) {
url = "https://cache.depot.dev"
enabled = true
push = true
credentials {
username = ""
password = System.getenv("DEPOT_TOKEN") ?: ""
}
}
}
}
EOF
echo "org.gradle.caching=true" >> ui/espresso/BasicSample/gradle.properties


-   name  :   Run Espresso instrumented tests
env  :
DEPOT_TOKEN  :   ${{ secrets.DEPOT_TOKEN }}
run  :   |
cd ui/espresso/BasicSample
./gradlew connectedAndroidTest


-   name  :   Cleanup emulator
if  :   always()
run  :   adb emu kill 2>/dev/null || true
```


## What's next


We're always keen to hear how we can make Depot better for the people building on it, so don't hold back: tell us what you're shipping, what's frustrating, or what you wish worked differently. If you run into any issues with nested virtualization or have ideas for making it more useful, reach out on our Discord community or open a support ticket. Happy building!


## Related posts


- [Now available: Depot CI](https://depot.dev/blog/now-available-depot-ci)
- [Migrating my workflows from GitHub Actions to Depot CI](https://depot.dev/blog/migrate-to-depot-ci)
- [How we got microVMs booting in under a second](https://depot.dev/blog/optimizing-microvm-boot-times)


Peter Hasko-Nagy


Staff Engineer at Depot
