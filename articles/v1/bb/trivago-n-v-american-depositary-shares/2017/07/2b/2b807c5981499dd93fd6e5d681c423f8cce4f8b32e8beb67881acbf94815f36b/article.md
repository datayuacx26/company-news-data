---
schema_version: "1.0.0"
document_id: "2b807c5981499dd93fd6e5d681c423f8cce4f8b32e8beb67881acbf94815f36b"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/css-done-right-post-rtlcss/"
published_at: "2017-07-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:eaaf949b77d29b67e1ed916b195943bc46992616a9d5e88ed6e7ad9228fc4611"
---

# CSS done right - Post RTLCSS

Our first[right-to-left platform](https://www.trivago.co.il/) was released in 2014. We had developed a solution to generate right-to-left CSS with Sass mixins and variables as we have[described](https://tech.trivago.com/2015/04/27/right-to-left/) in a blog article. We used this approach for nearly 3 years but recently migrated the right-to-left generation from pre-processing to post-processing with[RTLCSS](http://rtlcss.com/) . With this article I would like to share the reasons for the migration as well as our experiences and lessons learned.


## The reasons


Why did we see a need to change the approach we used for generating right-to-left CSS which worked for us for more than 3 years? Actually we didn’t have much trouble with the existing approach but nevertheless we identified four areas where improvements could be made.


1.


The usage of rtl mixins “polluted” the SCSS sources and added a layer of complexity. Instead of writing` margin: 100px 10px 50px 40px;` we invoked a mixin` @include margin(100px,10px,50px,40px)`


2.


Who would have thought that, when we onboarded new people we had to tell them to not write any CSS which has impact on position / vertical flow with plain CSS but to use a mixin. It has been a common comment in code reviews over the last few years: Please use a mixin.


3.


Nothing beats automation as my admired colleague[Anil Anar](https://twitter.com/@mostruash) states. So why authoring right-to-left CSS manually if we can outsource the work to a tool which does the work for us? With fewer errors.


4.


We imposed this approach on everybody who used our CSS. So even if people did not use any right-to-left layout at all they had to include our variables and mixins.


## The opportunities


When we developed the inital solution in 2014, we used[Assetic](https://github.com/kriswallsmith/assetic) to build our CSS in our[Symfony](https://symfony.com/) application. While at that time there were already a number of different solutions other than creating right-to-left CSS through pre-processing we couldn’t use any of those approaches. Since we moved away from Assetic in 2015 and switched to a node based build stack we were able to integrate PostCSS. This is what we did after our large scale CSS refactoring project in 2015. We’ve integrated a couple of modules like cssnano (before we used YUI), mqpacker and Autoprefixer. Before we used Autoprefixer we wrote mixins to handle vendor prefixes which lead to some of the same issues with maintaining them manually as we had with our right-to-left CSS approach.


## The integration


We did the integration in different steps to be able to roll-back the change at any time. Better safe than sorry. So first we removed the right-to-left configuration variables from the application config and the build target config. Instead of passing the variables into the webpack variant we just built the same target twice and post-processed one with RTLCSS. After we found out that this doesn’t lead to any issues, we started removing the mixins from the applications as well as from our CSS framework.


Step 1 - add RTLCSS and remove Sass variables


` yarn add rtlcss`


Step 2 - adjust webpack config


```text
const rtlCss: require('rtlcss');


if (variant  .rtl  ) {
plugins  .  push  (  rtlCss  ());
}
```


Step 3 - remove mixins


## The numbers


Here are some numbers which show how much lines of code have been affected in different applications.


The git diff for the hotel search application was:


` 121 files changed, 1089 insertions(+), 1048 deletions(-)`


For[trivago accounts](https://accounts.trivago.com/) :


` 47 files changed, 1031 insertions(+), 489 deletions(-)`


I have to add that diff contained an updated` yarn.lock` with around 600 lines of code changed.


For our CSS Framework which is part of our Design System:


` 48 files changed, 376 insertions(+), 874 deletions(-)`


## The simplification


The high amount of deletions in the CSS framework is related to the fact that we could remove a lot of` @include is-rtl()` mixins which we invoked when we had no other solution or needed to make some adjustments for right-to-left platforms which were not possible with the existing mixins.


Let’s look at an example:


```text
.selector   {
@  include   is-rtl   {
left  :   -285  px  ;
}
}
```


Despite the fact that setting a left value to -285px in right to left looks in the first place highly suspicious, we continue with this real world example here. We learned with projects like[Ironman](https://tech.trivago.com/2016/02/02/large-scale-css-refactoring-at-trivago/) to scope refactorings as much as possible to avoid any side-effects. Instead of investing time in identifying whether there is a general issue, we concentrated exclusively on the right-to-left migration. Tomorrow is another day for another improvement.


In this example we replaced the mixin with a[value directive](http://rtlcss.com/learn/usage-guide/value-directives/) RTLCSS offers:


```text
left: auto   /*rtl:-285px*/  ;
```


This comment will be interpreted by RTLCSS and it will replace the value on the build target it is executed on.


The output of the left-to-right stylesheet:


```text
.selector   {
left  :   auto  ;
}
```


While the output for the right-to-left stylesheet looks like:


```text
.selector   {
left  :   -285  px  ;
}
```


And that’s what we wanted to have. Of course it’s minified. Thanks to cssnano.


As of writing this blog article our main pricesearch application uses 22 directives, mostly` /*rtl:ignore*/` . When seeing this number I was at a first glance surprised but on the other hand it is just the result of e.g. using more flexbox and newer layout technologies. In 2014 there has been much more adjustment needed for right-to-left layouts especially due to a high amount of float in the codebase.


## The surprise moment


The integration went very smooth and we didn’t encounter any major issues. Nevertheless when we integrated the changes into trivago accounts we were wondering why none of the directives in the SCSS were noticed by RTLCSS. After doing the usual things such as uncommenting all of the webpack config to see what step might remove the comments or downgrading RTLCSS to the former major version to skip the update, we realized that the config used node-sass with` "sass-output-style": "compressed"` . So before we passed the CSS to PostCSS we already had removed the necessary comments from the source. Clever us.


## The conclusion


We’ve met one of our[OKRs](https://en.wikipedia.org/wiki/OKR) for Q2/2017. The key result of our team’s objective “Make UI Code great again” was to migrate to RTLCSS. We’ll from now on spend less time on producing right to left stylesheets. Neither do we have to write right-to-left mixins while writing SCSS, nor we do we have to check right-to-left in code reviews and on top of all that RTLCSS offers us even more possibilities and easier solutions e.g. in the case of the` is-rtl` example.


The Pre-Processing approach had been developed within a couple of days in 2014, lasted for more than 3 years and has now been replaced in a comparable timeframe with the Post-Processing approach.
