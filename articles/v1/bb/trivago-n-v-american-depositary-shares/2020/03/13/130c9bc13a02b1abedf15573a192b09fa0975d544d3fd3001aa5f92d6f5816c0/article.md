---
schema_version: "1.0.0"
document_id: "130c9bc13a02b1abedf15573a192b09fa0975d544d3fd3001aa5f92d6f5816c0"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2020-03-10-thefirstprettierpluginfortwigishere/"
published_at: "2020-03-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:49229a0eb350c87b4375a840e6af29475024f25778cbebb5f2540a7c24f49ba0"
---

# The First Prettier Plugin For Twig is Here

trivago open sourced a Prettier plugin for the Twig template language. It is available under the Apache 2.0 license, and you can access it on trivago’s Github space.


The trivago core product runs on our own frontend framework[Melody](https://melody.js.org/) . Melody uses a Twig-inspired template language because when it was introduced, it had to be interoperable with our existing codebase, which was based on the[Symfony](https://symfony.com/) PHP framework with Twig as the default template language.


Twig looks something like this:


```text
{%   if   hasItemFlags %}
{%   include   "./hotel-flags.melody.twig"   %}
{%   else   %}
<  p   class  =  "{{   css  .  text   }}"  >
<  b   data-qa  =  "entire_place"  >{{   'entire_place'   | translate }}</  b  >
</  p  >
{%   endif   %}
```


Even more than programming languages, markup and template languages offer a lot of room for debate on how to format things. Should there be spaces around the pipe filter symbol?


```text
{{   'entire_place'   | translate }}
```


or


```text
{{   'entire_place'  |  translate }}
```


Should object literals always be broken across several lines?


```text
{%   set   replacement: {   '$address'  : address_attributes.address } %}
```


Or


```text
{%   set   replacement: {
'$address'  : address_attributes.address
} %}
```


And, should an empty object literal have a space between the curly braces, or not?


```text
{ }
```


or


```text
{}
```


These are all fair questions, but they should have to be answered only once by any software team, and then never again. In a perfect world, everybody after that sticks to this decision, and there are no two opinions on the formatting of any given piece of code.


## Prettier


This is where Prettier contributes enormous value by enforcing THE canonical code layout. Not by chance, it calls itself “an opinionated code formatter”: You might not like every single layout decision it makes, but there is no discussion.


Moreover, after a few days of having Prettier take care of your code formatting, my personal guess is that you will not want to go back ever again:


It takes a huge cognitive load off your shoulders.


It frees you from having to think about the right level of indentation and the best way to add line breaks or whitespace. This way, you can direct more of your focus to solving actual problems.


We had been applying Prettier to our JavaScript codebase for a long time, and been very happy with it. However, there was no Prettier support for Twig, so we had to rely on manual formatting for our template files - with all the nitpick comments and back-and-forth in code reviews that come with it.


This has an end now: Since last week, we are using our own[Prettier plugin for Twig](https://github.com/trivago/prettier-plugin-twig-melody/) on our core product. In case you are using Twig in one of your projects, you might want to look into it: It is open sourced and you are welcome to use it, give feedback, contribute ideas.


## The benefits of open source


In fact, this kind of contribution and feedback has already happened: Even before we started fully using it on our core product, we got valuable improvement suggestions and useful bug reports from the community.


For example, one user[suggested](https://github.com/trivago/prettier-plugin-twig-melody/issues/9) we implement` {# prettier-ignore #}` comments to prevent unwanted reformattings on critical pieces of code. This was, of course, a great idea, which was also welcomed by developers at trivago – this feature means that some control still lies in the hands of the developer, and they are not entirely at the mercy of the tool.


Another user made us aware that there were a few cases where we were not following the official Twig coding standards. The trivago developer community[greatly cares about open source](https://tech.trivago.com/2019/11/20/open-source-trivago./) anyway, but these valuable suggestions from total strangers were a nice reminder of the benefits of open source software.


## The result


One thing that immediately becomes clear when you start using auto-formatting is that Twig code tends to become more “vertical”. In a lot of cases, additional line breaks are added to – hopefully – improve readability.


For example, look at this import statement, which greatly exceeds the recommended line length:


After formatting, it becomes this:


In the latter one, it is easier for the human reader to see what is going on. With this example, we can also demonstrate one of the nicest things about auto-formatting: When you realize you do not need all these imports, but maybe only the` input` and` textarea` elements, the import statement will automatically be put on a single line again:


This way, your files stay as readable as possible, and also as “short” as makes sense. Since you can configure the maximum line length independent of other source code files, you can balance readability and vertical extension according to your team’s preferences.


Again, have a look at the documentation or at the npm package and feel free to use it, inspect it, or make suggestions. We hope you find it useful!
