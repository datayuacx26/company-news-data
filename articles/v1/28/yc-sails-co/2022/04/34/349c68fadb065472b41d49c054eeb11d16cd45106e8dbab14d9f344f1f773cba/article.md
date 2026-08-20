---
schema_version: "1.0.0"
document_id: "349c68fadb065472b41d49c054eeb11d16cd45106e8dbab14d9f344f1f773cba"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/modifying-sails-log-prefix-with-prefix-themes/"
published_at: "2022-04-24T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:cdf8e38c41e0ff355577350707905559f792b988bc4bbd6c637dce396d692797"
---

# Modifying Sails log prefix with prefix themes

The de facto logger for Sails is[captains-log](https://github.com/balderdashy/captains-log) , it accepts several configuration options but the well known option is level which can be set to any one of the following levels:


```text
[
'silly'  ,
'verbose'  ,
'info'  ,
'blank'  ,
'debug'  ,
'warn'  ,
'error'  ,
'crit'  ,
'silent'
]
```


There is however an option that allows you to override the prefix for your logs. This option as any log related configuration can be set in` config/log.js` . Let’s have a look at that.


## The prefix option


You can pass a` prefix` property in` config/log.js` to a string and Sails will use that string as the prefix for every log across your Sails application. For example if we do this:


```text
module  .  exports  .log   =   {
prefix:   'my-custom-log-prefix: '
}
```


And you use the Sails log in say like an action:


```text
sails.  log  (  'Sails is awesome'  )
```


The output will then be


```text
my-custom-log-prefix:   Sails   is   awesome
```


Okay, I guess we can already see the limitation of this because by default, Sails will use different prefix for different logs i.e error logs have a different prefix than an info log for example. Let’s look at a more robust built-in way to modify the logs prefix


## The prefixTheme option


By default` captains-log` uses a shorthand to decide what prefix to use for each log types. This shorthand is called` prefixTheme` . The idea is that` captains-log` ships with built-in log themes that defaults to the` traditional` theme. Let’s look at the available themes and the prefix they expose.


### traditional


```text
traditional  : {
silly  :     '     : '  ,
verbose  :   'verbo: '  ,
info  :      ' info: '  ,
blank  :     ''  ,
debug  :     'debug: '  ,
warn  :      ' warn: '  ,
error  :     'error: '  ,
crit  :      ' crit: '
}
```


### abbreviated


```text
traditional  : {
silly  :     '     : '  ,
verbose  :   'verbo: '  ,
info  :      ' info: '  ,
blank  :     ''  ,
debug  :     'debug: '  ,
warn  :      ' warn: '  ,
error  :     'error: '  ,
crit  :      ' crit: '
}
```


### moderate


```text
moderate  : {
silly  :   '[silly] '  ,
verbose  :   '[verbose] '  ,
info  :   '    '  ,
blank  :   ''  ,
debug  :   '[-] '  ,
warn  :   '[!] '  ,
error  :   '[err] '  ,
crit  :   '[CRITICAL] '
}
```


### aligned


```text
aligned  : {
silly  :     '   silly | '  ,
verbose  :   ' verbose | '  ,
info  :      '    info | '  ,
blank  :     ''  ,
debug  :     '   debug | '  ,
warn  :      '    warn | '  ,
error  :     '   error | '  ,
crit  :      'CRITICAL | '
},
```


### minimalist


```text
minimalist  : {
silly  :     ' | '  ,
verbose  :   ' | '  ,
info  :      ' | '  ,
blank  :     ''  ,
debug  :     ' | '  ,
warn  :      'warn: '  ,
error  :     'error: '  ,
crit  :      'CRITICAL: '
}
```


### bubbles


```text
bubbles  : {
silly  :     '   '  ,
verbose  :   '˙˙  '  ,
info  :      '˙·  '  ,
blank  :     ''  ,
debug  :     '•·  '  ,
warn  :      'warn: '  ,
error  :     'error: '  ,
crit  :      'CRITICAL: '
}
```


### flowers


```text
flowers  : {
silly  :     '   '  ,
verbose  :   '˙˘˙   '  ,
info  :      '~%°  '  ,
blank  :     ''  ,
debug  :     '~∞%° '  ,
warn  :      'warn: '  ,
error  :     'error: '  ,
crit  :      'CRITICAL: '
}
```


### Using the built-in themes


Now that we know what themes are available to us we can pass in either one of the above theme name(defaults to traditional) to modify the prefix theme of our logs like so:


```text
// config/log.js
module  .  exports  .log   =   {
prefixTheme:   'bubbles'
}
```


So now if we write something like:


```text
sails.log.  verbose  (  'Ah! A verbose log'  )
```


You will get the following output


```text
˙˙   Ah!   A   verbose   log
```


Note: You will have to remove the` prefix` option if it’s set as its at a lower level than the` prefixTheme` option and will override it.


### Pro tip


Did you know you can use` sails.log.blank()` for giving line breaks in your logs sort of like using` <br/>` in HTML?


## Conclusion


Now you know about the possibility of modifying the Sails log prefix and how to modify it.


I think this can be useful if your decided you want less log prefix in your app(you can reach out for the minimalist theme!).


You can play with the different theme and have some fun(like I did when I discovered we can do this in Sails)
