---
schema_version: "1.0.0"
document_id: "961dbff9d4877428455b095ee3cd6b82b7629af919be7cd3585026881b2ff073"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T22:12:44.576485+00:00"
content_hash: "sha256:0443606b1b6133f5775c5c5960d9a7927eccd19daa49b1c9c7ce498f642a8944"
---

# TCLBANKER: Brazilian Banking Trojan Spreading via WhatsApp and Outlook

7 May 2026 •


[Jia Yu Chan](https://www.elastic.co/security-labs/author/jia-yu-chan) •


[Daniel Stepanic](https://www.elastic.co/security-labs/author/daniel-stepanic) •


[Seth Goodwin](https://www.elastic.co/security-labs/author/seth-goodwin) •


[Terrance DeJesus](https://www.elastic.co/security-labs/author/terrance-dejesus)


# TCLBANKER: Brazilian Banking Trojan Spreading via WhatsApp and Outlook


REF3076 uses a trojanized Logitech installer to deploy TCLBANKER, a Brazilian banking trojan with environment-gated payloads, WPF fraud overlays, and self-propagating WhatsApp and Outlook worm modules.


17 min read


[Malware Analysis](https://www.elastic.co/security-labs/category/malware-analysis) ,


[Threat Intelligence](https://www.elastic.co/security-labs/category/threat-intelligence)


Elastic Security Labs identified a new Brazilian banking trojan that we are tracking as TCLBANKER, a malware family we assess is a major update of the[MAVERICK](https://securelist.com/maverick-banker-distributing-via-whatsapp/117715/) /[SORVEPOTEL](https://www.trendmicro.com/en_us/research/25/j/self-propagating-malware-spreads-via-whatsapp.html) family. The campaign, tracked as REF3076, features a loader with robust anti-analysis capabilities that deploys two embedded .NET Reactor-protected modules: a full-featured banking trojan and a worm module for self-propagation.


The banking trojan monitors the victim's browser address bar via UI Automation, targeting 59 Brazilian banking, fintech, and cryptocurrency domains. Beyond the usual remote access commands, its most notable capability is a WPF-based full-screen overlay framework designed for operator-driven social engineering.


A second module handles distribution through spam agents, of which we recovered two variants: a WhatsApp worm that hijacks authenticated browser sessions to message the victim's contacts, and an Outlook email bot that sends phishing emails through the victim's own accounts via COM automation.


Through this report, we provide a detailed technical breakdown of each stage.


##


Key takeaways


- TCLBANKER uses environment-gated payload decryption; incorrect environments, such as sandboxes, silently fail to decrypt the payload
- A comprehensive watchdog subsystem continuously monitors for analysis tools, debuggers, instrumentation frameworks, and integrity violations throughout execution
- The banking trojan targets 59 Brazilian banking, fintech, and cryptocurrency domains, activating a WebSocket C2 session when a victim navigates to a monitored site
- A WPF-based full-screen overlay framework enables operator-driven social engineering, including credential harvesting, vishing wait screens, and fake Windows Update stalls, while hiding overlays from screen capture tools
- Worm modules propagate the malware: a WhatsApp bot and an Outlook email bot
- All C2 and distribution infrastructure is hosted on Cloudflare Workers under a single account, with developer artifacts (debug logging paths, test process names) and an incomplete phishing page, suggesting the campaign was identified in an early operational stage


##


Delivery


TCLBANKER is a Brazilian banking trojan that contains a dynamic infection chain with a heavy anti-analysis loading component that can deploy two embedded payloads (worm, banker). The observed infection chain bundles a malicious MSI installer inside a ZIP file. These MSI installer packages are abusing a signed Logitech program called[Logi AI Prompt Builder](https://www.logitech.com/en-us/software/logi-ai-prompt-builder) .


TCLBANKER abuses DLL sideloading against` LogiAiPromptBuilder.exe` , a legitimate Logitech application built on the[Flutter](https://flutter.dev/) framework. The malicious DLL` screen_retriever_plugin.dll` masquerades as a legitimate Flutter plugin of the same name and is loaded automatically when the host application starts.


After the MSI installation, the malicious DLL is immediately loaded and starts at the DllMain entry point.


##


Loader


The loader component for TCLBANKER is packed with features, including anti-debugging features, anti-analysis checks, string encryption, system language checks, ETW patching, and a watchdog capability. While it has many features, it lacks depth and has references to older malware analysis tooling. It’s not entirely clear whether the developer used LLM-assisted workflows, but our team wouldn’t be surprised if that were the case.


At the beginning of the execution, TCLBANKER aligns the corresponding .NET assembly payloads based on whether the string (` --renderer=sw` ) is used in the command-line. Within its main loader function, it first performs allow-list/blocklist operations based on how the DLL was loaded. The malicious DLL will only execute if the host process comes from the following two processes:


- ` logiaipromptbuilder.exe`
- ` tclloader.exe` (Possible reference to developer string during testing)


If the DLL was loaded by the following processes, it will refuse to run. These processes are traditionally used by analysts to load and debug DLLs.


- ` rundll32.exe`
- ` regsvr32.exe`
- ` dllhost.exe`
- ` svchost.exe`


Next, TCLBBANKER removes any user-mode hooking by replacing` ntdll.dll` from disk. For more evasion, the malware generates the following syscall trampolines used later:


- ` NtQueryInformationProcess`
- ` NtSetInformationThread`
- ` NtSetInformationProcess`
- ` NtTerminateProcess`
- ` NtAllocateVirtualMemory`
- ` NtProtectVirtualMemory`


After installing syscall stubs, the malware patches` EtwEventWrite` in` ntdll.dll` with the classic` xor eax, eax; ret` to disable user-mode ETW telemetry.


TCLBANKER performs an initial sandbox check by capturing a start tick using` GetTickCount64()` , sleeping for 500 ms, and measuring the elapsed time. If fewer than 450 ms have actually passed, the malware bails — this detects sandboxes or emulation frameworks that hook Sleep to return immediately..


One of the more interesting features of TCLBANKER is an enumeration function that generates three fingerprints based on the following criteria:


- Anti-debugging checks
- System disk information and memory checks
- Language checks


The developer uses magic constants assigned to “clean” paths for each category, then performs an XOR against each one to generate the environment hash. This environment hash value is significant because it affects downstream decryption of the embedded payload.


For example, if a debugger is present, it will produce an incorrect hash, so when the malware attempts to derive the decryption keys from the hash, the payload will not decrypt correctly, and TCLBANKER will stop executing.


##


Anti-debugging checks


TCLBANKER implements six different anti-debugging checks:


- Identify the debugger through the` Peb->BeingDebugged` flag
- Checks heap-tail/heap-free/check-heap flags set when a process is launched under a debugger
- Leverages` NtQueryInformationProcess()` using` ProcessDebugPort`
- Uses` NtQueryInformationProcess()` using` ProcessDebugObjectHandle`
- Hardware breakpoint detection via the debug registers (` DR0-DR3` )
- Measures the elapsed time using` QueryPerformanceCounter()` deltas and` RDTSC` cycle counts


##


System information checks


TCLBANKER has the following five different checks based on virtualization, system, and user information:


- Checks for virtualization software using vendor signature


Hypervisor Vendor signature


VMware VMwareVMware


VirtualBox VBoxVBoxVBox


KVM KVMKVMKVM


Xen XenVMMXenVMM


Parallels prl hyperv


QEMU/TCG TCGTCGTCGTCG


- Verify the root system drive (` C:\\\\` ) via` GetDiskFreeSpaceExW()` has at least 64 GB
- Calls` GlobalMemoryStatusEx()` to verify the system has more than 2 GB of RAM
- Checks for 2 or CPU processors via` GetSystemInfo()`
- Checks for generic sandbox/malware usernames


- ` sandbox` ,` malware` ,` virus` ,` sample` ,` john doe` ,` currentuser`


##


Language checks


For the last environment fingerprint check, TCLBANKER retrieves geographical information of the infected machine using` GetUserGeoID()` , targeting Brazilian users based on the geographical ID (` 0x20` ). A second locale check via` GetUserDefaultLCID()` also ensures the user's default language is Brazilian Portuguese (pt-BR, LANGID 0x0416).


After these sets of checks, TCLBANKER will either bail out of execution if anything is detected or, if not, produce another anti-debugging check by patching` DbgUiRemoteBreakin()` . The malware patches its first byte to` a ret` instruction so that any attempt to remotely break into the process does nothing — the injected thread immediately returns, and the target keeps running, unsuspended.


After this, the malware derives an AES-256 CBC key and IV by using hard-coded constants from the` .rdata` section along with the environmental hash calculated earlier. TCLBanker uses` BCryptDecrypt()` to decrypt the embedded payload, then decompresses via` RtlDecompressBuffer` using the LZNT1 compression algorithm.


Once the respective payload is decrypted, TCLBANKER initializes COM via` CoInitializeEx()` and uses the CLR hosting APIs to load the .NET runtime in-process. Before launching the payload entry point, TCLBanker creates two new threads: one serves as the watchdog, and the other monitors the watchdog thread as a heartbeat check.


##


Watchdog


TCLBANKER has a comprehensive watchdog feature that targets various analysis tools, including disassemblers, debuggers, instrumentation products, anti-virus products, and sandbox products. This section will outline the various techniques used by this feature:


- Debugger check via` PEB→BeingDebugged`
- Watches for hardware breakpoints` DR0` /` DR1` /` DR2` /` DR3`
- Checks Windows functions (` BCryptDecrypt()` ,` BCryptOpenAlgorithmProvider()` ) for in-line hooks by scanning the first 12 bytes of each function
- Monitors for instrumentation tools and related strings (` frida` ,` cydia` ,` user-path injection` ,` hook framework` )
- Reviews all kernel named pipes searching for` frida` or` linjector`
- Performs process enumeration via` CreateToolhelp32Snapshot()` targeting the following process names:


- ` frida` ,` de4dot` ,` dnspy` ,` megadumper` ,` extremedumper` ,` processhacker` ,` x64dbg` ,` x32dbg` ,` pe-sieve` ,` scylla` ,` Ilspy` ,` dotpeek` ,` netreactorslayer` ,` cheatengine`


- Employs Windows title detection via` GetWindowTextW()` with these titles:


- ` x64dbg` ,` x32dbg` ,` ida -` ,` ida pro` ,` ghidra` ,` dnspy` ,` megadumper` ,` extremedumper` ,` processhacker` ,` ollydbg` ,` windbg)_` ,` pe_sieve` ,` scylla`


- Identifies analyst tooling based on the following window class names via` FindWindowW()` :


- ` IDATopLevelWindow` ,` idaabortwndclass` ,` TIdaWindow` ,` x64dbg` ,` x32dbg` ,` OLLYDBG` ,` WinDbgFrameClass` ,` ProcessHacker` ,` SystemInformer` ,` CheatEngine` ,` HxdClass`


- Checks for the following loaded modules


- ` dbeng.dll` ,` dbgcore.dll` ,` SbieDll.dll` ,` snxhk.dll` ,` cmdvrt32.dll` ,` cmdvrt64.dll` ,` cuckoomon.dll` ,` pstorec.dll` ,` vmcheck.dll` ,` wpespy.dll`


- Targets the following mutexes and events:


- ` Ida_trusted_idbs` ,` IDA_COMM_PIPE_` ,` Local\\\\x64dbg` ,` Local\\\\x32dbg` ,` Frida` ,` YOURAPPNAMEHERE`


- Performs` CRC32` integrity check on the` .text` section to prevent any tampering


##


Banking Trojan Module


` Tcl.Agent` is a banking trojan, the main component of the chain. It is .NET Reactor-protected, and although we failed to deobfuscate it using available open-source tooling such as de4dot and NETReactorSlayer, we managed to statically deobfuscate this stage up to a satisfiable state using a custom deobfuscation pipeline to tackle .NET Reactor’s string encryption, control flow flattening, IL mutation, delegate proxies, and encrypted method bodies (Necrobit). Although it is a new malware, much of the code structure still follows ESET’s LATAM banking trojan[implementation blueprint](https://web-assets.esetstatic.com/wls/2020/09/ESET_LATAM_financial_cybercrime.pdf) , published in 2020.


At start, the malware performs geofencing, requiring >= 2 of the following indicators to match Brazil; otherwise, it exits immediately if not on a Brazilian machine:


Check Implementation


Region Code new RegionInfo(CultureInfo.CurrentCulture.LCID).TwoLetterISORegionName == "BR"


Timezone TimeZoneInfo.Local.BaseUtcOffset.TotalHours: if >= -5.0, check if == -2.0


LCID CultureInfo.CurrentCulture.LCID == 1046 (Portuguese-Brazil)


Keyboard GetKeyboardLayoutList() - check each layout: (ToInt32() & 0xFFFF) == 1046


##


Installation and Persistence


On the first run, the malware copies the entire application directory into` %LocalAppData%\\LogiAI` . It computes a SHA-256 hash over all` .dll` and` .exe` files in the source directory and writes it to a` .version` marker file. On subsequent runs, it compares hashes to skip redundant copies. After copying, it launches the new instance from the install path and exits.


It creates a scheduled task named` RuntimeOptimizeService` using COM interop with the Task Scheduler (` CLSID 0F87369F-A4E5-4CFC-BD3E-73E6154572DD` ). The task is configured as hidden, enabled, with no execution time limit, allowed on battery, start-when-available, and fires on a logon trigger (` type 9` ) scoped to the current user. It registers with` TASK_CREATE_OR_UPDATE` and` TASK_LOGON_SERVICE_ACCOUNT` .


After persistence is established, the agent sends a first-run POST beacon to` https://campanha1-api.ef971a42.workers\[.\]dev/api/installs` with the agentId (` MachineName-UserName` ), MachineName, UserName (redundant), and the OS version. The request is authenticated with a hardcoded campaign authentication token` 0d21613a-2609-45fc-83ff-d0feaa0c891f` . The newer variant adds debug logging around this call (` C:\\temp\\tcl-debug.txt` ), a developer artifact that inadvertently exposes agent presence on disk.


##


Self-Update


The agent implements a hash-based self-update gate that runs early in the startup pipeline. It reads a local version hash from` flutter_engine.cfg` in its install directory (migrating from a legacy` version.hash` filename if present), then fetches the current hash from the file server endpoint` documents.ef971a42.workers\[.\]dev/api/version` using a truncated User-Agent string` (Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36` .


The response is parsed for the "hash" key. If the remote hash matches the local hash, execution continues normally. On first install, the remote hash is written to disk, and execution proceeds without updating.


When a hash mismatch is detected, the agent downloads the update payload from` documents.ef971a42.workers\[.\]dev/api/update` as an MSI to` %TEMP%\\update_{8hexchars}.msi` , authenticated with Bearer token` b7ba9e80-0d04-4d9e-b217-c8b3cce335a2` .


The download is validated against a 100KB minimum size as a sanity check. The agent then writes a self-deleting batch script to` %TEMP%` that polls the tasklist until the current process exits, executes` msiexec /i /qn REINSTALLMODE=amus` for silent installation, and deletes itself. The batch file is launched via a hidden` cmd.exe` process before the agent terminates, handing off execution to the updated payload.


##


Browser URL Monitor and C2 Session Initialization


Every second, the malware agent calls a browser URL monitor function that reads the foreground browser's address bar via[UI Automation](https://learn.microsoft.com/en-us/dotnet/framework/ui-automation/ui-automation-overview) . It calls` GetForegroundWindow` , resolves the owning process, checks the process name against Chrome, Firefox, Microsoft Edge, Brave, Opera, and Vivaldi, then uses` AutomationElement.FromHandle -> FindFirst(Descendants, ControlType.Edit) -> ValuePattern.Current.Value` to extract the URL, similar to this Stack Overflow[implementation](https://stackoverflow.com/questions/5317642/retrieve-current-url-from-c-sharp-windows-forms-application/5318791#5318791) .


The extracted URL is matched against a fixed list of targeted banks embedded in the binary, encoded via XOR with a 16-byte key and base64 encoding.


This[GitHub Gist](https://gist.github.com/jiayuchann/e298effb68bd472c9e577a630d0ceb20) contains a list of 59 targeted domains, including Brazilian banking, fintech platforms, and cryptocurrency exchanges, grouped by the target IDs appended to each decrypted domain.


When a match hits, the domain target ID is passed to the next state, which initializes the official C2 communication by establishing a WebSocket connection to` wss://mxtestacionamentos\[.\]com/ws` . The` OnConnect` handler fires, sending a registration packet containing the agent ID (a random GUID at runtime), MachineName, UserName, machine info, timestamp, domain target ID (so the C2 knows which website the victim opened), and a signature.


To produce a handshake signature, HMAC-SHA256 is used to sign the victim identifier (agent ID, MachineName, UserName, OSVersion, timestamp) using the campaign GUID` 70e4f943-e323-4484-97d7-35401bf6812c` as the key.


The server then responds with a registration acknowledgment, officially starting the session, and enters the command dispatch loop. At session start, a Task Manager killer is fired every 500ms to prevent the victim from inspecting or terminating the agent's process.


##


C2 Command Table


A capability summarization is described through the opcode table below:


Opcode Purpose


2 Registration ACK, start the Task Manager killer


4 Graceful WebSocket disconnect


5 Suicide: Kill all sibling processes and exit


6 Forced reboot (` shutdown.exe /r /t 0 /f` )


7 Suicide-then-Uninstall: kill all processes with the same host binary name except itself (siblings) → uninstall → exit


16 Screenshot


17 Start streaming the screen


18 Stop streaming the screen


19 Set screen capture quality (1-100)


20 Enumerate monitors


32 Mouse move (X, Y, MonitorIndex)


33 Mouse click through overlay: parse` {X,Y,Button,MonitorIndex}` → translate to absolute desktop coords → find the implant's own overlay window covering that point → punch a 2x2 region hole in the overlay at that pixel →` SetCursorPos` +` SendInput` mouse-down/up (which lands on whatever real desktop content is underneath the overlay).


34 Mouse scroll (Delta,` SendInput` )


35 Key tap (KeyCode,` SendInput` )


37 Key down (KeyCode,` SendInput` )


38 Key up (KeyCode,` SendInput` )


39 Start keylogger (` WH_KEYBOARD_LL` hook)


40 Flush keylogger, exfil to C2


41 Clipboard hijack (` Clipboard.SetText` )


48 File system directory listing


65 Get running processes information


67 Shell command execution (` cmd.exe /c` )


80 Enumerate all visible windows


81 Window manager: Kill process of a window / minimize window / restore window / bring window to foreground / close window / move window to another monitor


83 Show stall overlay: either progress-steps or fake Windows Update screen


84 Teardown overlay


85 Toggle screen capture immunity. Enables/disables` WDA_EXCLUDEFROMCAPTURE` on all overlay windows to hide them from screen sharing / screenshots.


86 Refresh overlay content


87 Show cutout overlay: pin external window inside overlay with visible region cutout


96 Show credential prompt overlay


##


Social Engineering UI Framework


A more interesting capability of the banking trojan is a[WPF-based](https://learn.microsoft.com/en-us/dotnet/desktop/wpf/overview/) full-screen overlay subsystem that orchestrates bank-themed fraud flows during active C2 sessions.


###


Overlay Lifecycle


The overlay manager spawns one full-screen WPF window per monitor. Windows are configured as borderless, topmost, and hidden from the taskbar (` WindowStyle.None` ,` Topmost = true` ,` ShowInTaskbar = false` ), with a custom` Closing` handle that refuses dismissal until an internal flag is flipped by the operator through the overlay teardown command, preventing the windows from being closed.


At startup, the manager captures a PNG screenshot of every display via` CopyFromScreen` as the overlay backdrop, creating a “frozen desktop” look. Depending on the currently active overlay, the victim perceives their real desktop environment behind it.


A` 500ms` timer continuously reapplies` HWND_TOPMOST` via` SetWindowPos` to defeat any window attempting to surface above the overlay.


In addition, an anti-capture feature calls` SetWindowDisplayAffinity` with` WDA_EXCLUDEFROMCAPTURE` , rendering the overlay invisible to any screen-capturing tools, allowing the operator to see through their own overlay through the screenshot and screenstreaming commands.


###


Input Blocker


On the primary monitor, two hooks are installed:` WH_KEYBOARD_LL` and` WH_MOUSE_LL` . Both hooks check their respective injected flags (` LLKHF_INJECTED` for keyboard,` LLMHF_INJECTED` for mouse), allowing input injected via` SendInput` by the operator’s remote commands to pass through untouched. The keyboard hook swallows Tab, Escape, Alt+F4, Win keys, PrintScreen, Ctrl, Alt, and all navigation keys; the mouse hook blocks right-click, middle-click, and scroll, but allows left-click and movement, so the victim can still interact with the overlay prompts.


###


Social Engineering UI Builders


Five interchangeable content renderers plug into the overlay framework:


Credential Prompt: Supports three input modes, selected based on the operator's request parameters.


- Phone mode applies real-time Brazilian format masking ((##) ####-#### for 10-digit landlines, (##) #####-#### for 11-digit mobiles) with max 11 digits.
- Virtual keypad mode renders an on-screen numeric keypad (buttons 0–9 plus "limpar"/clear), displaying input as bullet characters to mimic PIN entry.
- Default mode accepts plain text with a configurable max length.


All modes run input through a quality validator that algorithmically rejects same-digit sequences (` 000000` ) and ascending/descending runs (` 123456` ,` 654321` ) to prevent victims from entering throwaway values.


The submit action fires the captured values to the C2.


Vishing Wait Screen: Triggers after the victim submits their phone number in the credential prompt. Displays "Estamos entrando em contato" ("We are getting in touch") with a central image “breathing” animation and three dots with staggered opacity animations (` 300ms` offset per dot) producing a "connecting" visual. The operator or an accomplice can then call the victim's real phone, impersonating bank security staff.


Progress Steps: A fully operator-templated stall screen displaying a list of fake processing steps with a randomized animation. Step durations are randomized to total approximately 15 minutes. A timer advances through each step sequentially, visually marking completed steps, highlighting the current one, and dimming the remaining ones. When all steps are complete, the sequence resets to an earlier position with new randomized timings and continues.


Fake Windows Update: An alternative stall screen mimicking the Windows 10/11 update-restart screen. Renders a solid` #0078D7` (Windows accent blue) background with a five-ellipse spinning indicator arranged in a circle. A percentage readout jumps by a random 25–35% at randomly selected 50–81-second intervals to mimic the irregular progress behavior of real Windows Updates. Default subtitle: "Trabalhando em atualizacoes" ("Working on updates").


Cutout Overlay: Cuts a rectangular hole in the full-screen overlay, exposing the underlying application window. The operator specifies hole dimensions via opcode 87, in which the overlay manager builds a themed card with a transparent-border placeholder, computes its screen coordinates post-layout, and cuts a matching region hole using` CreateRectRgn + CombineRgn(RGN_DIFF) + SetWindowRgn` . The target window is repositioned underneath the hole. The result is a real application window framed within the overlay, and the victim interacts with the actual application while the surrounding overlay provides deceptive context.


##


Worm module


The second module invoked by the loader is` Tcl.WppBot` , is designed to propagate spam and phishing messages at scale, to distribute TCLBANKER. Two distinct agent types were recovered from two different loaders and analyzed:


- A WhatsApp worm that hijacks browser sessions
- An Outlook email bot that abuses Microsoft Outlook through COM interop


` Tcl.WppBot` is also .NET Reactor-protected with the same version used to protect` Tcl.Agent` , and so we also managed to statically deobfuscate payloads in this stage.


Both agents share the same C2 backend, authentication credentials, and operational infrastructure. The C2 URL and API key are decrypted at startup using XOR decryption with a hardcoded key.


- C2 URL:` campanha1-api.ef971a42.workers\[.\]dev` (Cloudflare Workers app)
- API key / Bearer token:` 0d21613a-2609-45fc-83ff-d0feaa0c891f`


The C2 serves a single superset campaign configuration object via the` https://campanha1-api.ef971a42.workers\[.\]dev/api/campaign` endpoint. Each agent variant deserializes the full object but only reads the fields relevant to its channel.


Captured configuration:


```text
message            : Ola tudo bem?
Preciso de um orÃ§amento, estarei encaminhado caso tenha os produtos por favor me retorne para
darmos continuidade no atendimento.


https://arquivos-omie[.]com ð


âï¸*IMPORTANTE*: Este orÃ§amento foi otimizado para visualizaÃ§Ã£o em Computadores Desktop,
pois o mesmo necessita de visualizador de excel, word ou pdf.
fileUrl            : https://documents.ef971a42.workers[.]dev/file
delayMin           : 1
delayMax           : 3
maxPerSession      : 3000
updatedAt          : 2026-04-17T15:54:07.003Z
type               : gmail
subject            : Prezado(a), NFe disponÃ­vel para impressÃ£o
emailMessage       : <!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Nota Fiscal DisponÃ­vel</title>
<style>
body {
font-family: Arial, sans-serif;
margin: 20px;
padding: 0;
text-align: center;
background-color: #f4f4f4; /* Cor de fundo mais clara */
color: #333; /* Cor do texto ajustada para ser visÃ­vel */
}
h1 {
font-size: 24px;
margin-bottom: 20px;
font-weight: normal; /* TÃ­tulo sem negrito */
}
p {
font-size: 16px;
margin-bottom: 20px;
line-height: 1.6;
color: #333; /* Garantir que o texto esteja visÃ­vel */
}
.btn {
background-color: #007BFF;
color: #fff;
padding: 10px 20px;
border: none;
border-radius: 5px;
cursor: pointer;
}
.btn:hover {
background-color: #0056b3;
}
</style>
</head>
<body>


<h1>Prezado(a)</h1>
<p>
Sua Nota Fiscal EletrÃ´nica (NFe) estÃ¡ disponÃ­vel e pronta para ser acessada.
Para facilitar, basta clicar no botÃ£o abaixo para abrir o documento.
</p>


<p>
Caso tenha alguma dÃºvida sobre os detalhes da nota ou precise de alguma alteraÃ§Ã£o, por
favor, entre em contato conosco.
</p>


<a href="https://arquivos-omie[.]com" target="_blank">
<button class="btn">Abrir Nota Fiscal</button>
</a>


<p>
Agradecemos pela confianÃ§a e ficamos Ã  disposiÃ§Ã£o para qualquer outra necessidade.
</p>


</body>
</html>
emailDelayMin      : 30
emailDelayMax      : 90
emailMaxPerSession : 100
```


The same Cloudflare account` ef971a42` also hosts the payload delivery CDN (domain for` fileUrl` in the configuration object) at` documents.ef971a42.workers\[.\]dev` . Accessible through the` /file` endpoint, it currently serves a zip file containing the TCLBANKER-trojanized LogiAI Prompt Builder MSI. This infrastructure decision allows the operator to rapidly redeploy infrastructure without maintaining dedicated servers.


As of the time of writing, the phishing domain` arquivos-omie\[.\]com` identified in the configuration above, created on 2026-04-15, is not at an operable state (Welcome! This portal is currently undergoing scheduled maintenance. Please try again later.) The campaign could be in its early operational stages or staged for tasking. This domain is also named to impersonate a popular Brazilian Enterprise Resource Planning (ERP) suite.


##


Agent 1: WhatsApp Bot


The WhatsApp agent silently takes over the victim’s authenticated WhatsApp Web session to send spam messages and distribute TCLBANKER to Brazilian contacts.


###


Session Hijacking


The malware starts by discovering Chromium-based browsers on the target system, then scanning both the` App Paths` registry entries and common installation directories for Chrome, Edge, Brave, Opera, and Vivaldi. It then walks each browser's user profiles (e.g., "Default," "Profile 1," etc.), looking for evidence of an active WhatsApp Web session. A profile is flagged as having an authenticated session if its IndexedDB storage contains the WhatsApp Web LevelDB directory at` <profile_dir>/IndexedDB/https_web.whatsapp\[.\]com_0.indexeddb.leveldb/` .


Then each profile is sent to the profile-cloning and session-hijacking function.


For each qualifying profile, the malware clones it to a temporary directory at` %TEMP%\\<GUID>\\` , copying only the files needed to resume the WhatsApp Web session:` IndexedDB` ,` Local Storage` ,` Session Storage` ,` databases` ,` Web Data` ,` Login Data` , and` Cookies` . It then launches a headless Chromium instance via Selenium WebDriver, with` --user-data-dir` pointing at the cloned profile.


The matching` chromedriver.exe` is resolved at runtime by a disguised Selenium Manager binary dropped at` %TEMP%\\msvc-rt14\\bin\\hostfxr.exe` , which is invoked with` --browser chrome --output json` and returns the path of a chromedriver compatible with the victim's installed Chrome version.


Immediately after launch, the malware injects JavaScript to bypass bot-detection frameworks by hiding` navigator.webdriver` , populates` chrome.runtime` , reconciles the` Notification.permission` /` permissions.query` state mismatch, fakes a non-empty` navigator.plugins` array, and sets` navigator.languages` to` \['pt-BR', 'pt', 'en-US', 'en'\]` to match the target demographic.


```text
Object.defineProperty(navigator, 'webdriver', {
get: () = > undefined
}


);
delete navigator.__proto__.webdriver;
if (window.chrome)  {
window.chrome.runtime = window.chrome.runtime || {};
}


const origQuery = window.navigator.permissions.query;
window.navigator.permissions.query = (p) = > (
p.name ==  = 'notifications' ?
Promise.resolve( {
state: Notification.permission
}


) :
origQuery(p)
);
Object.defineProperty(navigator, 'plugins', {
get: () = > [1, 2, 3, 4, 5], }


);
Object.defineProperty(navigator, 'languages', {
get: () = > ['pt-BR', 'pt', 'en-US', 'en'], }


);
window.navigator.chrome = {
runtime: {}


};
```


With the cloned profile loaded, the browser navigates to` web.whatsapp\[.\]com` , and the malware waits up to 45 seconds to observe the resulting page state. If the chat interface appears (the cloned IndexedDB was valid and the session resumed without a QR scan), it injects an embedded WA-JS ([WPPConnect](https://github.com/wppconnect-team/wppconnect) ) library and waits for` WPP.contact.list` and` WPP.chat.sendTextMessage` to become callable before starting the campaign dispatch loop. If the QR code prompt is shown instead, the engine returns` "qr_code"` without attempting injection, then tries the next candidate profile.


###


Spam Functionality


Once the injection succeeds, the malware retrieves the active campaign from the C2 endpoint` https://campanha1-api.ef971a42.workers\[.\]dev/api/campaign` , which supplies the message body, optional attachment URL, caption, and timing parameters. TCLBANKER is then downloaded from the file server at` https://documents.ef971a42.workers\[.\]dev/file` and reconstructed in the browser context as a[File object](https://developer.mozilla.org/en-US/docs/Web/API/File/File) , without being dropped to disk.


The malware then calls` WPP.contact.list` to harvest the victim's address book, filters out groups, broadcasts, and non-Brazilian numbers, and begins dispatching messages through` WPP.chat.sendTextMessage` and` sendFileMessage` . The malware reports progress to the C2 endpoint` /api/progress` after each batch, and polls` /api/control` for remote pause or resend commands from the operator.


##


Agent 2: Outlook Email Bot


The Outlook agent is an email spambot that abuses the victim’s installed Microsoft Outlook application to send phishing emails from the victim’s email address, making them harder to detect as spam than emails sent from attacker-controlled infrastructure.


###


Outlook Discovery & COM Attachment


If` OUTLOOK.EXE` is not already running, it attempts to locate the installation in` App Paths` registry entries and known installation directories and launches it in a new process. The malware then attaches to the process via COM interop:[Marshal.GetActiveObject("Outlook.Application")](https://learn.microsoft.com/en-us/office/vba/outlook/how-to/security/obtain-and-log-on-to-an-instance-of-outlook) , and validates that it has at least one email account configured.


###


Contact Harvesting


The malware then drops a PowerShell script,` %TEMP%\\oc<guid>.ps1,` that harvests contacts via Outlook COM from a separate process. It harvests contacts from two sources: first, it reads the default Contacts folder for all contact entries, extracting email addresses and full names from each contact item. Second, it iterates every store’s root folders to find inbox-like folders, sorts inbox messages by latest, and extracts sender email addresses and names before writing them to a` .txt` file in` email|name` format.


For each candidate email, additional filtering is done to maximize deliverability.


###


Spam Functionality


Similar to the WhatsApp bot, the malware retrieves the active campaign from` https://campanha1-api.ef971a42.workers\[.\]dev/api/campaign` , then sends emails through the victim's own Outlook accounts via COM automation. Each email is constructed via` outlookApp.CreateItem(0)` (` MailItem` ) with the recipient in` To` , the campaign subject line, and the campaign content` emailMessage` , sent using the victim's actual account via` SendUsingAccount` .


Between sends, the agent applies a randomized delay and periodically checks the C2 control endpoint` /api/control` for pause or resend commands, and reports progress to` /api/progress` .


##


Infrastructure


The REF3076 actors have leveraged the` worker\[.\]dev` Cloudflare Serverless infrastructure for C2 and file hosting. This decision allows them to inherit any trust victims might already have in Cloudflare and to rotate infrastructure quickly as needed.


Pivoting on the body-hash (` 91fafaa1240676afe5c55d931261e3798797c408` ) of the phishing site above (` arquivos-omie\[.\]com` ), we were able to identify additional domains that are likely being prepared for weaponization:


Domain First Seen Info ASN (Providor)


arquivos-omie\[.\]com 2026-04-17 Squatting - Brazilian SaaS for SMBs AS 13335 (Cloudflare)


documentos-online\[.\]com 2026-04-11 Generic AS 13335 (Cloudflare)


afonsoferragista\[.\]com 2026-04-22 Hardware store - Likely used in a B2B lure AS 13335 (Cloudflare)


doccompartilhe\[.\]com 2026-04-15 Generic - “Shared a document” AS 13335 (Cloudflare)


recebamais\[.\]com 2026-04-20 Squatting - Brazilian credit/loan brokerage AS 13335 (Cloudflare)


More Brazilian phishing infrastructure was discovered after a broader pivot in the banner title (` Portal`` Corporativo` ), but it’s unclear whether it was directly related to REF3076 or to other actors in the Latin American banking trojan ecosystem. Notably, one cluster leveraged the Cloudflare` pages\[.\]dev` free static-site hosting product.


The C2 domain` mxtestacionamentos\[.\]com` previously pointed to a Brazilian-hosted IP` 191.96.224\[.\]96` .


Last year, this IP concurrently hosted a REF3076 C2 domain, a REF3076 phishing domain, and a domain previously[associated with the Water Saci campaign](https://www.trendmicro.com/en_gb/research/25/j/self-propagating-malware-spreads-via-whatsapp.html) and SORVEPOTEL/MAVERICK malware by TrendMicro (` saogeraldoshiping\[.\]com` ).


##


Conclusion


TCLBANKER reflects a broader maturation happening across the Brazilian banking trojan ecosystem. Techniques that were once the hallmark of more sophisticated threat actors: environment-gated payload decryption, direct syscall generation, real-time social engineering orchestration over WebSocket, are now being packaged into commodity crimeware. The barrier to entry continues to drop, especially when powerful LLMs are readily accessible for code generation.


The inclusion of self-propagating worm modules marks a notable shift in this space. The campaign inherits the trust and deliverability of legitimate communications by hijacking victims' WhatsApp sessions and Outlook accounts. This is a distribution model that traditional email gateways and reputation-based defenses are ill-equipped to catch. As Latin American banking trojans continue to adopt these self-spreading mechanisms, organizations should expect the volume and reach of these campaigns to scale accordingly.


Developer artifacts throughout the chain, including debug logging paths, test process names, and a phishing site still under construction, suggest REF3076 is in its early operational stages. This is a campaign still being built out, not wound down.


##


REF3076 through MITRE ATT&CK


Elastic uses the[MITRE ATT&CK](https://attack.mitre.org/) framework to document common tactics, techniques, and procedures that threats use against enterprise networks.


###


Tactics


Tactics represent the why of a technique or sub-technique. It is the adversary’s tactical goal: the reason for performing an action.


- [Initial Access](https://attack.mitre.org/tactics/TA0001/)
- [Execution](https://attack.mitre.org/tactics/TA0002/)
- [Persistence](https://attack.mitre.org/tactics/TA0003/)
- [Defense Evasion](https://attack.mitre.org/tactics/TA0005/)
- [Credential Access](https://attack.mitre.org/tactics/TA0006/)
- [Discovery](https://attack.mitre.org/tactics/TA0007/)
- [Collection](https://attack.mitre.org/tactics/TA0009/)
- [Command and Control](https://attack.mitre.org/tactics/TA0011/)
- [Exfiltration](https://attack.mitre.org/tactics/TA0010/)
- [Impact](https://attack.mitre.org/tactics/TA0040/)


###


Techniques


Techniques represent how an adversary achieves a tactical goal by performing an action.


- [Phishing: Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001/)
- [System Binary Proxy Execution: Msiexec](https://attack.mitre.org/techniques/T1218/007/)
- [Hijack Execution Flow: DLL Side-Loading](https://attack.mitre.org/techniques/T1574/002/)
- [Command and Scripting Interpreter: PowerShell](https://attack.mitre.org/techniques/T1059/001/)
- [Command and Scripting Interpreter: Windows Command Shell](https://attack.mitre.org/techniques/T1059/003/)
- [Scheduled Task/Job: Scheduled Task](https://attack.mitre.org/techniques/T1053/005/)
- [Deobfuscate/Decode Files or Information](https://attack.mitre.org/techniques/T1140/)
- [Obfuscated Files or Information](https://attack.mitre.org/techniques/T1027/)
- [Debugger Evasion](https://attack.mitre.org/techniques/T1622/)
- [Virtualization/Sandbox Evasion: System Checks](https://attack.mitre.org/techniques/T1497/001/)
- [Virtualization/Sandbox Evasion: Time Based Evasion](https://attack.mitre.org/techniques/T1497/003/)
- [Impair Defenses: Disable or Modify Tools](https://attack.mitre.org/techniques/T1562/001/)
- [Native API](https://attack.mitre.org/techniques/T1106/)
- [Process Injection](https://attack.mitre.org/techniques/T1055/)
- [Process Discovery](https://attack.mitre.org/techniques/T1057/)
- [Application Window Discovery](https://attack.mitre.org/techniques/T1010/)
- [System Information Discovery](https://attack.mitre.org/techniques/T1082/)
- [System Location Discovery: System Language Discovery](https://attack.mitre.org/techniques/T1614/001/)
- [Screen Capture](https://attack.mitre.org/techniques/T1113/)
- [Input Capture: Keylogging](https://attack.mitre.org/techniques/T1056/001/)
- [Clipboard Data](https://attack.mitre.org/techniques/T1115/)
- [Input Capture: Web Portal Capture](https://attack.mitre.org/techniques/T1056/003/)
- [Browser Session Hijacking](https://attack.mitre.org/techniques/T1185/)
- [Application Layer Protocol: Web Protocols](https://attack.mitre.org/techniques/T1071/001/)
- [Web Service](https://attack.mitre.org/techniques/T1102/)
- [Ingress Tool Transfer](https://attack.mitre.org/techniques/T1105/)
- [Email Collection: Local Email Collection](https://attack.mitre.org/techniques/T1114/001/)
- [System Shutdown/Reboot](https://attack.mitre.org/techniques/T1529/)


##


Remediating REF3076


###


Prevention


- [NTDLL Memory Protection Change via Unsigned DLL](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_ntdll_memory_protection_change_via_unsigned_dll.toml)
- [NTDLL library loaded for a second time](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_ntdll_library_loaded_for_a_second_time.toml)
- [Potential NTDLL Memory Unhooking](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_potential_ntdll_memory_unhooking.toml)
- [Parallel NTDLL Loaded from Unbacked Memory](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_parallel_ntdll_loaded_from_unbacked_memory.toml)
- [Suspicious Windows Core Module Change](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_suspicious_windows_core_module_change.toml)
- [AMSI Bypass via Unbacked Memory](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_amsi_bypass_via_unbacked_memory.toml)
- [Potential AMSI Bypass via SetThreadContext](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_potential_amsi_bypass_via_setthreadcontext.toml)


####


YARA


Elastic Security has created YARA rules to identify this activity.


- [Windows.Trojan.TCLBanker](https://github.com/elastic/protections-artifacts/blob/main/yara/rules/Windows_Trojan_TCLBanker.yar)


##


Observations


The following observables were discussed in this research.


Observable Type Name Reference


701d51b7be8b034c860bf97847bd59a87dca8481c4625328813746964995b626 SHA-256 screen_retriever_plugin.dll TCLBanker loader component


8a174aa70a4396547045aef6c69eb0259bae1706880f4375af71085eeb537059 SHA-256 screen_retriever_plugin.dll TCLBanker loader component


668f932433a24bbae89d60b24eee4a24808fc741f62c5a3043bb7c9152342f40 SHA-256 screen_retriever_plugin.dll TCLBanker loader component


63beb7372098c03baab77e0dfc8e5dca5e0a7420f382708a4df79bed2d900394 SHA-256 XXL_21042026-181516.zip TCLBanker initial ZIP file


campanha1-api.ef971a42\[.\]workers.dev domain-name TCLBanker C2


mxtestacionamentos\[.\]com domain-name TCLBanker C2


documents.ef971a42.workers\[.\]dev domain-name TCLBanker file server


arquivos-omie\[.\]com domain-name TCLBanker phishing page (under development)


documentos-online\[.\]com domain-name TCLBanker phishing page (under development)


afonsoferragista\[.\]com domain-name TCLBanker phishing page (under development)


doccompartilhe\[.\]com domain-name TCLBanker phishing page (under development)


recebamais\[.\]com domain-name TCLBanker phishing page (under development)


#### Jump to section


- [Key takeaways](https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan#key-takeaways)
- [Delivery](https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan#delivery)
- [Loader](https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan#loader)
- [Anti-debugging checks](https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan#anti-debugging-checks)
- [System information checks](https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan#system-information-checks)
- [Language checks](https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan#language-checks)
- [Watchdog](https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan#watchdog)
- [Banking Trojan Module](https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan#banking-trojan-module)
- [Installation and Persistence](https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan#installation-and-persistence)
- [Self-Update](https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan#self-update)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=TCLBANKER:%20Brazilian%20Banking%20Trojan%20Spreading%20via%20WhatsApp%20and%20Outlook&url=https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan&title=TCLBANKER:%20Brazilian%20Banking%20Trojan%20Spreading%20via%20WhatsApp%20and%20Outlook)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/tclbanker-brazilian-banking-trojan&title=TCLBANKER:%20Brazilian%20Banking%20Trojan%20Spreading%20via%20WhatsApp%20and%20Outlook)
