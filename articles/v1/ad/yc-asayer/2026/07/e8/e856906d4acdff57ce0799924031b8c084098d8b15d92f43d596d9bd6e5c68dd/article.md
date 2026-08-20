---
schema_version: "1.0.0"
document_id: "e856906d4acdff57ce0799924031b8c084098d8b15d92f43d596d9bd6e5c68dd"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/packaging-distributing-electron-apps/"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:0201a04a197adbb991fd6d3e9955fee23f27e58f607de50f075b3480553a4551"
---

# Packaging and Distributing Electron Apps

Shipping an Electron app to real users is a four-stage pipeline: **package** the app into a platform-native installer, **sign and notarize** it so operating systems trust it, **distribute** it through a channel users can reach, and wire up **auto-update** so they stay current. The tool you pick to run that pipeline is the first decision, and in 2026 it comes down to two: Electron Forge or electron-builder. This guide walks the full path with copy-pasteable configuration, the code-signing rules that changed in 2023 and 2024, and a working GitHub Actions matrix that builds all three platforms in parallel — because you cannot cross-build everything from one machine.


## Key Takeaways


- As of June 2026, Electron Forge is the official, Electron-maintained distribution tool; other tools are maintained by the community and[do not come with official support from the Electron project](https://www.electronjs.org/docs/latest/tutorial/forge-overview) — while electron-builder remains the most-downloaded alternative with a built-in auto-updater.
- The dependency chain most teams get wrong: your application must be signed for automatic updates on macOS —[this is a requirement of Squirrel.Mac](https://www.electronjs.org/docs/latest/api/auto-updater) , so code signing → notarization → working silent updates is one unbreakable chain.
- The Apple Developer Program is[99 USD per membership year, or in local currency where available](https://developer.apple.com/programs/whats-included/) , and once you’re a member you can[notarize Mac apps for no additional fee](https://developer.apple.com/forums/thread/746992) .
- Stop buying an EV certificate just to skip SmartScreen: EV certificates previously bypassed SmartScreen entirely on first download, but that behavior was removed in 2024, and EV-signed files now go through the[same reputation-building process as OV certificates](https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/code-signing-options) .
- You cannot cross-build everything locally, so the realistic path is a GitHub Actions matrix running` macos-latest` ,` windows-latest` , and` ubuntu-latest` with signing secrets injected as encrypted environment variables.


## Electron Forge vs electron-builder in 2026


Pick **Electron Forge** if you want an officially supported, all-in-one pipeline; pick **electron-builder** if you want the most mature built-in auto-updater and the widest community footprint. Both produce signed, updatable installers for macOS, Windows, and Linux. The split is about support model and philosophy, not capability.


[Electron Forge](https://www.electronforge.io/) is the project’s blessed tool. It is an all-in-one tool for packaging and distributing Electron applications that combines many single-purpose packages into a full build pipeline that works out of the box, complete with code signing, installers, and artifact publishing. Forge absorbed the older` @electron/packager` and` @electron/osx-sign` utilities into its Makers and Publishers system. Crucially, if you do not want to use Electron Forge, the other third-party tools are maintained by members of the Electron community and[do not come with official support from the Electron project](https://www.electronjs.org/docs/latest/tutorial/forge-overview) .


[electron-builder](https://www.electron.build/) is that community alternative — and the more downloaded one, in the range of[2–3 million weekly installs](https://www.npmjs.com/package/electron-builder) depending on which counter you trust. As of June 2026 the current stable release is 26.15.3, and it ships new versions several times a month, so pin it and re-check before each release cycle. The next major version, v27, is in alpha as of June 2026 (the` 27.0.0-alpha` line): it moves electron-builder to native ES modules, raises the minimum runtime to Node.js 22.12, and removes[long-deprecated APIs](https://www.electron.build/docs/migration/v27-breaking-changes) , so plan a Node and config check before adopting it — but the stable channel is still 26.x for now. The reason many teams reach for it is the integrated updater: electron-builder is a complete solution that adds a single dependency, manages further requirements internally, and replaces features used by the Electron maintainers — such as the auto-updater — with custom ones.


This article uses **electron-builder** for its configuration examples because its declarative` package.json` block maps cleanly onto every platform target and its` electron-updater` companion is the de facto standard for self-hosted updates. The concepts transfer to Forge if you prefer the official route.


### What about Tauri?


[Tauri](https://v2.tauri.app/) 2.x is the credible lighter-weight alternative to Electron when a ~100 MB bundle is a dealbreaker. It ships your UI through the operating system’s native WebView instead of bundling a full Chromium runtime, which produces dramatically smaller binaries (order of magnitude, not exact). The trade is real: you give up Electron’s rendering consistency across machines and its vast npm and native-module ecosystem, and you accept WebView version fragmentation across user OSes. Tauri is the right call for a small, security-sensitive utility; Electron stays ahead when UI fidelity and ecosystem depth matter more than disk footprint.


## Packaging per platform with electron-builder


electron-builder produces native installers from a single` build` block in` package.json` . Define a top-level config plus per-platform` mac` ,` win` , and` linux` keys naming the **targets** you want. A minimal multi-platform configuration looks like this:


```text
{
"  build  "  : {
"  appId  "  :   "  com.example.myapp  "  ,
"  productName  "  :   "  MyApp  "  ,
"  directories  "  : {   "  output  "  :   "  dist  "   },
"  files  "  : [  "  dist-app/**/*  "  ,   "  package.json  "  ],
"  mac  "  : {
"  category  "  :   "  public.app-category.productivity  "  ,
"  target  "  : [  "  dmg  "  ,   "  zip  "  ],
"  hardenedRuntime  "  :   true  ,
"  notarize  "  :   true
},
"  win  "  : {
"  target  "  : [  "  nsis  "  ]
},
"  linux  "  : {
"  target  "  : [  "  AppImage  "  ,   "  deb  "  ,   "  rpm  "  ],
"  category  "  :   "  Utility  "
},
"  publish  "  : [{   "  provider  "  :   "  github  "   }]
}
}
```


Each target maps to a real installer format:


Platform Targets When to use


macOS` dmg` ,` pkg` ,` zip` DMG is the standard drag-to-Applications image; PKG runs pre/post-install scripts for enterprise;` zip` is **required** for Squirrel.Mac auto-update


Windows` nsis` ,` msi` ,` portable` NSIS is the customizable wizard with per-user/per-machine install and an uninstaller; MSI suits GPO/Intune deployment; portable runs without installing


Linux` AppImage` ,` deb` ,` rpm` ,` snap` ,` flatpak` AppImage is a single self-contained executable;` deb` /` rpm` integrate with apt/dnf; Snap and Flatpak ship through their stores


Two electron-builder target details trip people up. The` zip` target on macOS is not optional if you auto-update: the zip target for macOS is[required for Squirrel.Mac](https://www.electron.build/docs/features/auto-update/) , otherwise latest-mac.yml cannot be created, which causes an autoUpdater error — and since the default macOS target is dmg+zip, there is usually no need to specify it explicitly. For auto-update specifically, the macOS application must be signed for auto updating to work, and the supported default targets are DMG (macOS), AppImage/DEB/Pacman/RPM (Linux), and NSIS (Windows). Run a build with` npx electron-builder --mac --win --linux` , but note you can only fully build a given platform’s signed installers on (or for) that platform — which is why CI matters later.


On the security baseline: this article assumes your app already runs with the[recommended Electron security posture](https://www.electronjs.org/docs/latest/tutorial/security) — a preload script with` contextIsolation` enabled and` nodeIntegration` disabled. Never ship the inverse; packaging does not fix an insecure renderer.


## Code signing and notarization: the dependency chain


Here is the rule that breaks more macOS releases than anything else: **an unsigned macOS app can notify a user that an update exists but cannot silently install it.** Code signing is a prerequisite for notarization, and notarization is a prerequisite for a working macOS auto-update. Your application must be signed for automatic updates on macOS —[this is a requirement of Squirrel.Mac](https://www.electronjs.org/docs/latest/api/auto-updater) . Skip signing and the entire downstream chain collapses.


### macOS: sign, then notarize


You need an Apple Developer ID certificate, which requires membership. The Apple Developer Program is[99 USD per membership year, with prices listed in local currency during enrollment](https://developer.apple.com/programs/enroll/) — not “EUR99,” and notarization is bundled: once you’re a member of the Apple Developer Program, you can[notarize Mac apps for no additional fee](https://developer.apple.com/forums/thread/746992) . Notarization (Apple’s automated malware scan) has been mandatory for Developer-ID apps distributed outside the Mac App Store since macOS 10.15 Catalina.


In electron-builder, set` hardenedRuntime: true` and` notarize: true` in the` mac` block (shown above), then supply credentials through environment variables at build time — never in the repo:


```text
# macOS signing certificate (base64-encoded .p12) and its password
export   CSC_LINK  =  "$(  base64   -i   developer-id.p12  )"
export   CSC_KEY_PASSWORD  =  "  ••••••  "
# Notarization credentials (App Store Connect API or Apple ID app-specific password)
export   APPLE_ID  =  "  you@example.com  "
export   APPLE_APP_SPECIFIC_PASSWORD  =  "  abcd-efgh-ijkl-mnop  "
export   APPLE_TEAM_ID  =  "  XXXXXXXXXX  "
```


### Windows: the 2024 SmartScreen reset


Lead your Windows decision with the change almost every older guide misses. EV certificates no longer bypass SmartScreen: years ago, signing with an Extended Validation certificate produced positive SmartScreen reputation by default, but that behavior no longer exists, and paying a premium for[EV solely to avoid SmartScreen warnings is no longer justified](https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/smartscreen-reputation) . An EV-signed installer now builds download reputation exactly like a cheaper Organization Validated (OV) one.


The other shift is where keys must live. Since June 1, 2023, code-signing private keys must be stored on certified hardware — which is precisely why a USB-token OV certificate breaks headless CI.[Tauri’s Windows signing docs](https://v2.tauri.app/distribute/sign/windows/) mark the same boundary: their standard guide applies only to OV certificates acquired before June 1st 2023, and for certificates received after that date they direct you to the issuer’s hardware-token process instead.


The cleanest 2026 default sidesteps the token entirely. Azure Artifact Signing — formerly Trusted Signing — is a[fully managed, end-to-end code signing service integrated with Azure](https://azure.microsoft.com/en-us/products/artifact-signing) . It signs from the cloud with no USB token, runs natively in CI/CD pipelines, and stores keys in Microsoft-managed HSMs. Use this decision order:


1. **Azure Artifact Signing first.** It is CI-native and inexpensive (Microsoft documents consumption-based plans in the ~$10/month range for small projects). Availability is[limited to organizations in the USA, Canada, the EU, and the UK](https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/code-signing-options) , while individual developers are currently limited to the USA and Canada.
2. **OV certificate** from DigiCert, Sectigo, or GlobalSign if you fall outside those regions. OV certificates are functionally equivalent to Azure Artifact Signing for SmartScreen purposes.
3. **Keep an EV cert you already own** until it expires — but if you already have an EV certificate, it is still valid and functional for signing, though paying the EV premium solely to avoid SmartScreen warnings is no longer justified. Don’t buy a new one for that reason.


### Signed vs unsigned


Signed + notarized Unsigned


macOS first launch Clean, Gatekeeper passes ”Unidentified developer” block; right-click → Open workaround


Windows first download SmartScreen warning until reputation builds SmartScreen warning, no reputation inheritance


macOS auto-update Works silently Can notify only — cannot replace the binary


Mac App Store eligible Yes No


Unsigned builds are acceptable for a developer audience comfortable with the override steps. For non-technical users — and for any app that auto-updates on macOS — signing is non-negotiable.


## Auto-updates with electron-updater


` electron-updater` (electron-builder’s companion) gives you cross-platform self-hosted updates with minimal code. It supports GitHub Releases, Amazon S3, DigitalOcean Spaces, Keygen, and a generic HTTP(S) server out of the box, and[needs only two lines of code to make it work](https://www.electron.build/docs/features/auto-update/) . The common case:


```text
const   {   autoUpdater   }   =   require  (  "  electron-updater  "  );


app  .  whenReady  ().  then  (()   =>   {
createWindow  ();
autoUpdater  .  checkForUpdatesAndNotify  ();
});
```


Call` autoUpdater.checkForUpdatesAndNotify()` — and do not call` setFeedURL` , because electron-builder automatically creates an internal` app-update.yml` file on build. Where you host releases is set by the` publish` block in your build config; pointing it at` { "provider": "github" }` means a pushed tag triggers CI, uploads artifacts to a GitHub Release, and the updater finds them automatically. On macOS this one-liner does nothing useful unless the build is signed and notarized — the same chain from the previous section. Linux has no universal auto-update mechanism; AppImage updates work through electron-updater, but` deb` /` rpm` users update through their package manager.


## Distribution channels


Choose channels by audience reach and how much of the revenue and trust you want to keep:


- **Your own website.** Full control, signed or unsigned, no commission, and it pairs directly with electron-updater via GitHub Releases or S3.
- **GitHub Releases.** Free hosting, version history, and the path of least resistance for auto-update integration.
- **Mac App Store.** Adds discovery but requires sandboxing and review, and carries Apple’s standard 30% commission — reduced to 15% under the[App Store Small Business Program](https://developer.apple.com/app-store/small-business-program/) for developers under $1M USD in annual proceeds.
- **Microsoft Store.** Worth it for the SmartScreen win alone: publishing an MSIX package through the Microsoft Store means[Microsoft re-signs your package automatically](https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/code-signing-options) , so users never see a SmartScreen warning and you never need to purchase or renew a certificate.
- **Homebrew Cask.** Free, developer-friendly distribution for macOS via a pull request to the casks repo — ideal for reaching technical users with` brew install` .


A craft note worth keeping in mind: because an Electron renderer is a Chromium window, browser session-replay instrumentation runs unmodified in a packaged desktop build, which is one of the few practical ways to observe UI regressions that surface only after packaging or auto-update on a user’s machine — native menu and tray paths, file-dialog flows, signed-build CSP differences — and never in` electron .` during development.


## Building all three platforms in CI with GitHub Actions


You cannot cross-build everything locally: DMG notarization needs macOS, and signed Windows installers need Windows tooling. The realistic 2026 path is a GitHub Actions matrix running` macos-latest` ,` windows-latest` , and` ubuntu-latest` in parallel, with signing secrets injected as encrypted environment variables. A complete` release.yml` that builds and publishes on tag:


```text
name  :   Release
on  :
push  :
tags  : [  "  v*  "  ]


jobs  :
release  :
runs-on  :   ${{ matrix.os }}
strategy  :
matrix  :
os  : [  macos-latest  ,   windows-latest  ,   ubuntu-latest  ]
steps  :
-   uses  :   actions/checkout@v4
-   uses  :   actions/setup-node@v4
with  :
node-version  :   22
-   run  :   npm ci
-   name  :   Build, sign, and publish
run  :   npx electron-builder --publish always
env  :
GH_TOKEN  :   ${{ secrets.GITHUB_TOKEN }}
# macOS signing + notarization (read on macos-latest only)
CSC_LINK  :   ${{ secrets.MAC_CSC_LINK }}
CSC_KEY_PASSWORD  :   ${{ secrets.MAC_CSC_KEY_PASSWORD }}
APPLE_ID  :   ${{ secrets.APPLE_ID }}
APPLE_APP_SPECIFIC_PASSWORD  :   ${{ secrets.APPLE_APP_PASSWORD }}
APPLE_TEAM_ID  :   ${{ secrets.APPLE_TEAM_ID }}
# Windows: Azure Artifact Signing credentials (read on windows-latest)
AZURE_TENANT_ID  :   ${{ secrets.AZURE_TENANT_ID }}
AZURE_CLIENT_ID  :   ${{ secrets.AZURE_CLIENT_ID }}
AZURE_CLIENT_SECRET  :   ${{ secrets.AZURE_CLIENT_SECRET }}
```


Each runner builds only its own platform’s targets, electron-builder signs using whichever credentials are present in that runner’s environment, and` --publish always` uploads every artifact plus the` latest*.yml` update metadata to a single GitHub Release. Store every secret in repository or environment secrets — never in the workflow file. Because[Azure Artifact Signing works with GitHub Actions and Azure DevOps](https://azure.microsoft.com/en-us/products/artifact-signing) through plain environment variables, the Windows job needs no USB token and no self-hosted runner.


Shipping an Electron app is less about one magic command and more about respecting the chain: sign so you can notarize, notarize so macOS auto-update works, and run the whole thing in a CI matrix because no single machine can build all three platforms. Wire up the` build` block, add` electron-updater` , commit a` release.yml` , and your next` git tag v1.0.0 && git push --tags` produces signed, updatable installers for every platform at once.


## FAQs


Can I build signed installers for macOS, Windows, and Linux on a single machine?


No. DMG notarization requires macOS tooling and signed Windows installers require Windows tooling, so no single machine can produce signed builds for all three platforms. You can build for the platform you are on, and electron-builder supports some cross-targeting, but signing and notarization are platform-bound. The realistic 2026 approach is a GitHub Actions matrix running macos-latest, windows-latest, and ubuntu-latest in parallel, where each runner builds and signs only its own platform's targets.


Does the Microsoft Store remove SmartScreen warnings without a code-signing certificate?


Yes. Publishing an MSIX package through the Microsoft Store means Microsoft re-signs your package automatically, so users never see a SmartScreen warning and you never need to purchase or renew a certificate. This makes the Microsoft Store an effective way to skip both certificate costs and reputation-building entirely. The trade-off is store submission, review, and packaging your app as MSIX rather than distributing a standalone NSIS or MSI installer directly from your own site.


Why does my macOS app notify users of updates but never install them automatically?


The build is almost certainly unsigned. On macOS, an application must be signed for automatic updates to work because Squirrel.Mac requires it. An unsigned app can detect and notify the user that a new version exists, but it cannot silently replace the binary. The fix is the full chain: obtain an Apple Developer ID certificate, sign the app, then notarize it. Code signing is a prerequisite for notarization, and notarization is a prerequisite for working silent macOS auto-update.


Is it still worth paying for an EV certificate to avoid SmartScreen warnings on Windows?


No. EV certificates previously bypassed SmartScreen entirely on first download, but Microsoft removed that instant-reputation behavior in 2024. An EV-signed installer now builds download reputation exactly like a cheaper Organization Validated certificate, so paying the EV premium solely to avoid SmartScreen is no longer justified. For most teams in 2026, the cleaner default is Azure Artifact Signing, which signs from the cloud with no USB token, or an OV certificate if you fall outside its eligible regions.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
