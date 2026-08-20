---
schema_version: "1.0.0"
document_id: "0441e5de9e62a26573f8f1e9945e9edcb062b02545ea960b7bc811e439964a51"
company_key: "yc-oneleet"
company: "Oneleet"
source_id: "yc-oneleet-news-import-d01376dbbc95"
canonical_url: "https://www.oneleet.com/blog/practical-web-security-in-react"
published_at: "2025-05-12T00:00:00+00:00"
first_seen_at: "2026-07-24T07:26:35.695106+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:47980a2513846b349edf6e385e9c7fed3bf48320d3e96398c302682caa42e9e5"
---

# Practical Web Security in React

Most frontend engineers have heard of the[OWASP top 10](https://owasp.org/www-project-top-ten/) , and fewer could list all 10. But working on React frontends in your actual day-to-day work goes beyond just knowing about these vulnerabilities. We take a lot of things for granted that React already offers us in terms of security, but it is still possible to make some *huge* mistakes that could be prevented with a few additional minutes of effort.


## Injection


This is one of the OWASP top 10. React already helps us by not rendering text elements as HTML/JS. From my view, there are really only two ways to screw this up:


1.


dangerouslySetInnerHTML


2.


PDF rendering libraries


The first one may be obvious for some people, but I've seen the impact of implementing these wrong first-hand.


When using dangerouslySetInnerHTML, you just need to be careful that the HTML you're passing has *no user input* or is already properly sanitized by your backend. If you're rendering HTML that is submitted by another user, they could absolutely put malicious code in.


```text
import    React    from    "react"  ;


const    ScriptInjection   =  (  )    =>    {
return    (
<  div
dangerouslySetInnerHTML  = {  {
__html  :    `
<script>
console.log("hello");
</script>
`  ,
}  }
/>
)  ;
}  ;


export    default    ScriptInjection  ;
```


```text
import    React    from    "react"  ;


const    ScriptInjection   =  (  )    =>    {
return    (
<  div
dangerouslySetInnerHTML  = {  {
__html  :    `
<script>
console.log("hello");
</script>
`  ,
}  }
/>
)  ;
}  ;


export    default    ScriptInjection  ;
```


```text
import    React    from    "react"  ;


const    ScriptInjection   =  (  )    =>    {
return    (
<  div
dangerouslySetInnerHTML  = {  {
__html  :    `
<script>
console.log("hello");
</script>
`  ,
}  }
/>
)  ;
}  ;


export    default    ScriptInjection  ;
```


```text
import    React    from    "react"  ;


const    ScriptInjection   =  (  )    =>    {
return    (
<  div
dangerouslySetInnerHTML  = {  {
__html  :    `
<script>
console.log("hello");
</script>
`  ,
}  }
/>
)  ;
}  ;


export    default    ScriptInjection  ;
```


If they can put a script tag in, malicious users could most definitely put malicious JavaScript in, such as calling arbitrary endpoints on another user's behalf, accessing sensitive data in local storage, and much more. So be careful when using the appropriately named` dangerouslySetInnerHTML` .


PDF rendering is another way for hackers to inject malicious scripts into other users' browsers. Most PDF libraries you'll use in React are using PDF.js under the hood. I invite you to take a look at[some of the few CVEs recorded for PDF.js](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=pdf.js+firefox) , some more recent than others. You need to be very careful to always keep your PDF dependencies up to date, which we will touch on later with Dependabot.


I recommend using[this list of vulnerable PDFs](https://github.com/luigigubello/PayloadsAllThePDFs/blob/main/README.md) (they typically just try to` console.log` and won't actually cause you any harm) to test your PDF renderer with.


## Content Security Policies


Content Security Policies can help a lot with preventing attackers trying to load malicious content from external sources. React 19 now supports adding CSPs directly in your React code, whereas previously you'd need to use a library like Helm.


I won't dive[super deep into CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP) , since its usage varies on your use case, but here are a few examples:


```text
// React 19+ <head> component
<  head  >
<  meta
httpEquiv  = "Content-Security-Policy"
content  = "default-src 'self'; script-src 'self'; object-src 'none'; base-uri 'self';"
/>
</  head  >
```


```text
// React 19+ <head> component
<  head  >
<  meta
httpEquiv  = "Content-Security-Policy"
/>
</  head  >
```


```text
// React 19+ <head> component
<  head  >
<  meta
httpEquiv  = "Content-Security-Policy"
/>
</  head  >
```


```text
// React 19+ <head> component
<  head  >
<  meta
httpEquiv  = "Content-Security-Policy"
/>
</  head  >
```


This will only allow scripts, objects, and other content to load from the same origin as your app is hosted on. External scripts (like from a CDN) will be blocked unless explicitly allowed, which helps lock down the attack surface.


If your app *does* need to load third-party content—like analytics, fonts, or scripts from a CDN—you'll have to extend the policy accordingly:


```text
<  meta
httpEquiv  = "Content-Security-Policy"
content  = "default-src 'self'; script-src 'self' <https://cdn.example.com>; style-src 'self' <https://fonts.googleapis.com>; font-src <https://fonts.gstatic.com>;"
/>
```


```text
<  meta
httpEquiv  = "Content-Security-Policy"
/>
```


```text
<  meta
httpEquiv  = "Content-Security-Policy"
/>
```


```text
<  meta
httpEquiv  = "Content-Security-Policy"
/>
```


Just be careful—relaxing these rules too much defeats the purpose. Start strict, loosen only where necessary, and always validate CSP headers using tools like[CSP evaluator](https://csp-evaluator.withgoogle.com/) .


## Supply Chain Attacks and Automatic Version Updates


These are truly the biggest risk factors for React web vulnerabilities. Let's do a little exercise. Try guessing how many dependencies you have in your app. When you have that number in mind, you can run:


```text
npm   ls   -- parseable   |  wc   - l
```


```text
npm   ls   -- parseable   |  wc   - l
```


```text
npm   ls   -- parseable   |  wc   - l
```


```text
npm   ls   -- parseable   |  wc   - l
```


This will show you the number of direct dependencies in your package/workspace (it may be off by 1). This may not be too shocking a number, but you have to remember each of these also has subdependencies.


```text
$   npm   i
up   to   date  ,    audited   1553    packages    in    6  s
```


```text
$   npm   i
up   to   date  ,    audited   1553    packages    in    6  s
```


```text
$   npm   i
up   to   date  ,    audited   1553    packages    in    6  s
```


```text
$   npm   i
up   to   date  ,    audited   1553    packages    in    6  s
```


When you run` npm i` , it will show you all the packages that are either a direct dependency or a subdependency of your packages. In this case, I have *1553* packages that I need to hope nobody has deployed malicious code in.


Manually checking all 1553 packages for vulnerabilities is definitely impractical. That's why tools like Snyk and Dependabot exist. Luckily, if you're using GitHub, Dependabot security is enabled by default, and you'll get notifications about vulnerabilities directly in your notifications tab. It's important to frequently check for these and patch/update vulnerable packages when they arise.


In my opinion, it's better to be hyper-vigilant about package updates. GitHub has[a really easy guide](https://docs.github.com/en/code-security/dependabot/dependabot-version-updates/configuring-dependabot-version-updates) for setting up weekly package updates in your repository. Frequently updating your packages can help keep you off vulnerable dependencies, while Dependabot Security will scan for vulnerable packages you currently have installed. They play well together.


## Token Storage Best Practices


There's a lot of debate online about where to store tokens—` localStorage` ,` sessionStorage` , or cookies. And honestly, it *really* depends on your use case, but there are some clear pros and cons you should be aware of.


If an attacker finds a way to run JavaScript in your app, they can read from storage and send tokens to themselves. That means you should not store any sensitive tokens or data in local storage or session storage.


A safer alternative is to use` **httpOnly**` **cookies** , which JavaScript can't access. These are set by the server and automatically sent with every request to the domain. If your app is server-rendered or has an API backend on the same domain, this is a great option for protecting session tokens. The tradeoff is that you'll lose some flexibility with things like token-based auth on 3rd-party APIs, or you'll need CSRF protection for any state-changing requests.


If you do end up using` localStorage` , you should at the very least:


-


**Set a short expiration time** and refresh tokens often


-


**Clear tokens on logout** or after periods of inactivity


-


**Avoid storing refresh tokens** client-side altogether


And most importantly, XSS prevention becomes non-negotiable. Every vulnerability that allows arbitrary JS execution becomes a full-on auth breach.


Don't get me wrong - local storage is a great place to store long-lived data that isn't sensitive. The only data you need to worry about is user tokens that can be used to steal someone's auth session.


In general, use` httpOnly` cookies where you can. Only fall back to local storage for tokens if your app really requires it, and if you do, double down on sanitization and CSP.
