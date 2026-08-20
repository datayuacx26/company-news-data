---
schema_version: "1.0.0"
document_id: "6e250c42620bbfc0f1444fd7619d4583e0481e810daee9adcf9bb34ba2190b6e"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2016-06-01-dropping-ie8-support-consequences-for-javascript/"
published_at: "2016-06-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:31.756931+00:00"
content_hash: "sha256:9d376fd0511f17f8031973f76a9ca0b4cadd473fb3d229453c9f64fc6922d848"
---

# Dropping IE8 Support: Consequences for JavaScript

When people hear about dropping support for a certain Internet Explorer version, they mostly think of all the CSS hacks and workarounds that could be removed as a consequence. However, dropping support for IE8 can have subtle, but profound effects on your JavaScript. Read on to learn what we experienced when discontinuing support for IE8.


The short version of what it meant for trivago’s JavaScript developers to drop IE8 support is that it enabled them to:


- Drop unmaintained versions of external libraries and switch to newer, better performing versions that were incompatible with IE8.
- Use the JavaScript standard that was defined in December 2009 and is supported by every browser except IE8.
- Reduce testing overhead - for developers and QA.
- Spend less time cluelessly debugging problems without any help from the browser.
- Get rid of unstable and insecure workarounds that fixed problems with IE8.
- Improve performance in all other browsers for free.


All in all, dropping IE8 support was an important milestone for our JavaScript developers. Let us look at the individual benefits in more detail.


## Future Proofing


By dropping IE8 support, we have been able to update a few vital dependencies (jQuery, core-js, Babel) that had dropped support for IE8 a while ago. This helps developers because they get access to productivity increasing features added at a later release of these dependencies, and it avoids the situation where one of these libraries has a bug that won’t be fixed anymore.


## Code reduction and performance improvements


Without having to support IE8, we can avoid many previously required workarounds that were slow but necessary.


Examples include:


- The size of jQuery was reduced by 30% just by dropping IE8 support.
- Our Twig compiler does not need to put text into fake elements just to satisfy IE8. This reduces the size of the DOM, the memory usage, and it improves performance.
- [lodash](https://lodash.com/) can use native browsers’ features a lot more, and does not need to include the workarounds that would be required for IE8 to work. This results in less code and fewer feature tests, and, ultimately, much better performance.


## Productivity increase


The worst part about IE8 is not that it is missing features. There are ways around that and while they are not always easy, we have managed to get by so far.


The worst thing about IE8 is actually that, when something goes wrong and does not work, it is virtually impossible to find out why. Unlike every other major browser, IE8 does not have a useful debugging environment. If it fails, you will just get a rather cryptic error message and a location on the moon - i.e. the place where it reported the error did not exist.


In the months before we finally decided to drop IE8, we experienced many epic struggles to fix tiny bugs. And these struggles, which usually resulted in a one-line change, didn’t last for hours, they consumed days or more. With no inkling of where an error orginated, we often had to resort to putting notifications into the code at a number of places, just to identify the general region. Once we had identified the vicinity of the “bad” code, we used the same kind of notifications to try and find out what exactly went wrong. At that point, it was usually time to ask Google why IE8 didn’t like that particular code. In the standard case, the code was just fine and valid and worked in all other browsers but, somehow, triggered an IE8 bug. This is now a thing of the past.


## Dropping a build target


So far, we needed a special build for IE8 as the rules for code minification and various other parts were different for IE8. By dropping the IE8 specific build, we removed 8 out of 16 build targets, which greatly reduced the stress on our Jenkins servers.


## Conclusion


Dropping IE8 support saves a lot of developer time. It helps us to avoid situations where we have to work with unmaintained libraries and helps us to write cleaner and more maintainable code with fewer special cases. Maybe best of all, it reduces developer frustration over having to identify the root of a problem with no help at all from the only browser that shows the wrong behaviour - only to find out that they have hit a bug in IE8 that they can’t fix without including weird workarounds.
