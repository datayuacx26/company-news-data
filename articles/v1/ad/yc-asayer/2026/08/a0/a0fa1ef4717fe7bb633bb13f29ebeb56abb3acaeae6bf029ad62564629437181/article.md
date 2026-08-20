---
schema_version: "1.0.0"
document_id: "a0fa1ef4717fe7bb633bb13f29ebeb56abb3acaeae6bf029ad62564629437181"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/ios-feature-detection/"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T18:37:30.227787+00:00"
fetched_at: "2026-08-18T18:37:31.619257+00:00"
content_hash: "sha256:2bafa273a4d5fe4887d82f8575d9d50a8bd49e99e804a7d436538dd4f5dbc419"
---

# Modern iOS Feature Detection (Without the Pain)

Feature-detect by default: test the capability you actually need with` 'IntersectionObserver' in window` or` CSS.supports('property', 'value')` , and reserve user-agent checks for the handful of iOS cases that genuinely cannot be feature-detected.


Anyone who has kept a table of iOS version numbers in a codebase knows the routine: Apple ships an update, the table goes stale, and something breaks in production before anyone files a ticket. Capability checks skip that cycle entirely.


Feature detection resolves most Safari and iOS pain: you stop maintaining brittle version tables and start asking the browser what it can do. The hard part is the residue: a small set of iOS edge cases (old iPads that report as desktop macOS, the iOS 26 user-agent freeze) where no capability check exists and you have to sniff carefully. This article gives you the modern playbook for both.


## Key Takeaways


- Feature-detect capabilities directly with` 'x' in window` ,` CSS.supports()` , and optional chaining.[MDN calls this “a much more reliable strategy”](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/userAgent) than parsing the user-agent string.
- Presence is not proof: Safari can report` @supports` as true for a feature it doesn’t actually apply, so for known “liars” render the element off-screen and measure it with` getBoundingClientRect()` .
- Because every browser on iOS runs on WebKit and Safari’s version tracks the OS major version, an` isMobileWebKit()` check plus a` CSS.supports()` gate infers the iOS version without any UA parsing.
- On iOS 26+, Safari freezes the OS token in its UA to a pre-26 value that has itself drifted (` 18_6` →` 18_6_2` →` 18_7` ). Never hardcode it; parse the` Version/` token instead.
- The freeze is Safari’s own behavior, documented for iOS and iPadOS 26; Chrome and Firefox on iOS still report the true iOS version.


## Feature detection is the default


Test for the capability, not the browser. A feature check adapts automatically when Apple ships an update, needs no maintenance table, and works identically across engines, which is exactly why Apple and MDN recommend it over user-agent sniffing. You have three tools for the job.


For JavaScript APIs, probe the global or use optional chaining:


```text
if (  "  IntersectionObserver  "   in   window  ) {
// wire up lazy-loading
}
navigator  .  share  ?.({   title  :   "  Modern feature detection  "   });
```


For CSS, use[CSS.supports()](https://developer.mozilla.org/en-US/docs/Web/API/CSS/supports_static) in JS or the[@supports](https://developer.mozilla.org/en-US/docs/Web/CSS/@supports) at-rule in your stylesheet:


```text
@supports (text-wrap-style: stable) {
h1   { text-wrap-style:   balance  ; }
}
```


Note that[navigator.userAgentData](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/userAgentData) is not a fallback here: it’s Chromium-only and marked experimental, so Safari and Firefox don’t implement it. It is never a substitute for feature detection when your target is iOS Safari.


## When feature detection lies: presence isn’t proof


Feature detection has two failure modes worth naming, because the fix differs. The first is an anti-pattern: detecting an *unrelated* Feature B to infer Feature A.[As A Beautiful Site documents](https://www.abeautifulsite.net/posts/not-everything-can-be-feature-detected/) , the moment the browser ships one feature before the other, your check silently breaks. Don’t couple checks to proxies.


The second is subtler: presence is not proof. Safari can report` @supports` as` true` for a value it doesn’t actually apply, as[Evil Martians found with the CSS safe alignment keyword](https://evilmartians.com/chronicles/how-to-detect-safari-and-ios-versions-with-ease) . For those known “liars,” don’t trust the support flag. Render the element off-screen and measure the real result with` getBoundingClientRect()` :


```text
const   supportsSafeAlign   =   ()   =>   {
const   box   =   document  .  createElement  (  "  div  "  );
const   child   =   document  .  createElement  (  "  span  "  );
child  .  textContent   =   "  measure me  "  ;
Object  .  assign  (  box  .  style  , {
display  :   "  flex  "  ,
justifyContent  :   "  safe center  "  ,
width  :   "  5%  "  ,
position  :   "  absolute  "  ,
top  :   "  -9999px  "  ,
left  :   "  -9999px  "  ,
});
box  .  appendChild  (  child  );
document  .  body  .  appendChild  (  box  );
const   applied   =   child  .  getBoundingClientRect  ().  left   >=   box  .  getBoundingClientRect  ().  left  ;
document  .  body  .  removeChild  (  box  );
return   applied  ;
};
```


A behavioral test needs no version claim to be correct: it observes what actually rendered. This is the class of bug session replay is good at surfacing: a gated feature that passes its support check in your test environment but silently misbehaves on a real user’s iOS build.


## Detecting iOS versions with feature detection


Because every browser on iOS runs on WebKit and Safari’s version is tied to the OS major version, an` isMobileWebKit()` check plus a` CSS.supports()` gate for a property introduced in a known Safari release lets you infer the iOS version without parsing a user-agent string at all. The mobile-WebKit heuristic keys off a gesture event WebKit exposes:


```text
const   isMobileWebKit   =   ()   =>   "  ongesturechange  "   in   window  ;
```


For the version gate, look up a property’s first-supported release in[Apple’s Safari release notes](https://developer.apple.com/documentation/safari-release-notes) or MDN’s compatibility data, then test it. The[text-wrap-style longhand landed in Safari 17.5](https://webkit.org/blog/15383/webkit-features-in-safari-17-5/) , which shipped its` balance` ,` stable` , and` auto` values together, so any one of those values cleanly gates iOS 17.5+:


```text
const   isAtLeastIOS175   =   ()   =>
window  .  CSS  ?.  supports  (  "  text-wrap-style  "  ,   "  stable  "  )   ??   false  ;
```


Verify the property↔release mapping yourself before shipping. Release notes occasionally omit changes, and a “supported” flag can be a liar (see the previous section). Treat` isMobileWebKit()` as a strong heuristic, not a spec guarantee: Apple permits alternative browser engines in the EU on iOS 17.4+, so “iOS means WebKit” is overwhelmingly true rather than absolute.


## When user-agent sniffing is the scoped last resort


Reach for the user-agent only when two things are true at once. The capability genuinely has no feature test, *and* a wrong guess costs you nothing worse than a cosmetic glitch. Two iOS cases meet that bar.


**Old iPads.** Feature detection can’t separate an iPad from a Mac, because[since iPadOS 13 an iPad’s default user-agent is the same string a Mac sends](https://evilmartians.com/chronicles/how-to-detect-safari-and-ios-versions-with-ease) . Combine the signals instead. A user-agent that reads as desktop macOS Safari, paired with a positive mobile-WebKit check and a non-zero touch-point count, means an iPad wearing a Mac’s clothes. The touch-point test matters: Macs report` navigator.maxTouchPoints` as` 0` , so it keeps a real Mac from matching even if the gesture-event signal is present on desktop Safari.


```text
const   looksLikeMacSafari   =   /  Macintosh  /  .  test  (  navigator  .  userAgent  );
const   isIPad   =   ()   =>
looksLikeMacSafari   &&   isMobileWebKit  ()   &&   navigator  .  maxTouchPoints   >   0  ;
```


**The iOS 26 UA freeze.** On iOS 26 and iPadOS 26,[Safari stopped putting the running OS version in its user-agent](https://developer.apple.com/documentation/safari-release-notes/safari-26-release-notes) and pinned the token to an earlier release instead. That pinned value has itself drifted across point releases (` 18_6` at launch, then[18_6_2 in Safari 26.1](https://releasebot.io/updates/apple/safari) , then[18_7 from iOS 26.2 onward](https://theapplewiki.com/wiki/Safari) ), which is exactly why you must never hardcode it. Parse the` Version/` token, which still reflects the real Safari (and therefore iOS) major:


```text
const   iosMajor   =   ()   =>   {
const   m   =   navigator  .  userAgent  .  match  (  /  Version\/  (  \d  +  )/  );
return   m   ?   Number  (  m  [  1  ])   :   null  ;   // 26 on iOS 26.x Safari
};
```


Two things sharpen this. First, the freeze is Safari-only: independent[server-log analysis from AppleInsider](https://appleinsider.com/articles/26/01/12/ios-26-adoption-isnt-record-breaking-but-reports-of-extremely-low-rates-are-flawed) and[Lapcat Software](https://lapcatsoftware.com/articles/2026/1/3.html) both confirm Chrome and Firefox on iOS still report the true OS version, so “you can’t detect iOS 26” is a Safari-specific problem, not an iOS-wide one. Second, if you’d rather not own the parsing,[ua-parser-js](https://www.npmjs.com/package/ua-parser-js) is at 2.0.10 in its latest npm release, so pin` 2.0.10+` . The package also has a[documented supply-chain incident history](https://security.snyk.io/package/npm/ua-parser-js) , so check what you install.


## The playbook


The full flow is short, and it runs in order:


1. **Feature-detect first.**` 'x' in window` ,` CSS.supports()` ,` @supports` , optional chaining. This covers the overwhelming majority of gating decisions.
2. **Behavioral-test the liars.** When a support flag can’t be trusted, render off-screen and measure with` getBoundingClientRect()` .
3. **Infer iOS version without UA.**` isMobileWebKit()` plus a` CSS.supports()` gate on a release-note-confirmed property.
4. **Sniff only the undetectable edges.** The old-iPad combinator and the` Version/` token for the iOS 26 freeze. Use them only where a failed sniff loses no functionality.
5. **Test on real devices and simulators.** Release notes and support flags both leave gaps, and only real hardware settles it.


Feature detection is the default because it survives updates you didn’t plan for; user-agent sniffing is the scoped exception for the two or three iOS cases the platform makes genuinely undetectable. Wire the checks in that order, pin your` Version/` -token parsing instead of any frozen OS string, and confirm the result on a real old iPad before you ship.


## FAQs


Can you detect iOS 26 in JavaScript despite the user-agent freeze?


Yes. Starting with iOS 26, Safari freezes the OS token in its user-agent to a pre-26 value that has drifted across point releases (18_6, then 18_6_2, then 18_7 from iOS 26.2 onward), so hardcoding it fails. Instead parse the Version/ token, which still reports the real Safari major version and therefore the iOS major version. On iOS 26.x Safari, matching Version/(\\\\d+) returns 26.


Does the iOS 26 user-agent freeze affect Chrome and Firefox on iOS?


No. Apple documents the frozen OS token as Safari's own behavior on iOS and iPadOS 26. Independent server-log analyses from AppleInsider and Lapcat Software both confirm that Chrome and Firefox on iOS still report the true OS version in their user-agent strings, even though every iOS browser runs on WebKit. So 'you can't detect iOS 26' is a Safari-specific problem, not an iOS-wide one.


How do you tell an iPad from a Mac in the browser?


Combine two signals, because feature detection alone cannot separate them: since iPadOS 13, an iPad's default user-agent is the same string a Mac sends. If the user-agent looks like desktop macOS Safari but a mobile-WebKit check indicates you are on a touch device, you have an iPad masquerading as a Mac. Because gesture events also appear on desktop Safari, gate on navigator.maxTouchPoints greater than 0, since Macs report 0.


Does navigator.userAgentData work in Safari?


No. navigator.userAgentData is Chromium-only and marked experimental on MDN, so Safari and Firefox do not implement it. It is never a viable substitute for feature detection when your target is iOS Safari. Test capabilities directly with 'x' in window or CSS.supports() instead, which MDN rates as a far more dependable approach than reading the user-agent.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
