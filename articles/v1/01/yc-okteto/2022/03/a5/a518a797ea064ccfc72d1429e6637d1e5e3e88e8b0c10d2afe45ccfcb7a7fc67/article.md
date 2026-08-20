---
schema_version: "1.0.0"
document_id: "a518a797ea064ccfc72d1429e6637d1e5e3e88e8b0c10d2afe45ccfcb7a7fc67"
company_key: "yc-okteto"
company: "Okteto"
source_id: "yc-okteto-rss-a64bce3f80ea"
canonical_url: "https://www.okteto.com/blog/making-your-cli-more-accessible-using-fig/"
published_at: "2022-03-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.770894+00:00"
fetched_at: "2026-08-20T03:37:26.059895+00:00"
content_hash: "sha256:fee7cc73dbf90a12de07e42a11e1274f530d644011372bbe8cad53e34f4d4b68"
---

# Making Your CLI More Accessible Using Fig

No matter how many times I use` kubectl` or` docker` in a day, I just can't seem to remember the exact subcommands and what they do. I often find myself using` --help` and then reading through the output, trying to figure out what I need to run. This was getting exhausting, but I had come to terms with it, telling myself that there was no other way! That was until I got introduced to[Fig](https://fig.io/) .


Fig has been a game-changer in terms of how I use CLI tools. Since learning about it, not only have I been using it daily, but we've also added Fig support for[Okteto CLI](https://github.com/okteto/okteto) . Let's see what Fig does and how we integrated it with Okteto CLI so you, too, can leverage it and make working with CLIs a breeze.


## So What’s the Hype About?


To quote their website, "Fig adds VSCode-style autocomplete to your existing terminal". And that is precisely what Fig does! Let me show you:


As soon as you install Fig, you get autocomplete suggestions for almost all the common CLI tools you might be using. As you saw in the demo above, this list of tools now also includes Okteto CLI 😎


I really like the fact that now each time I write` okteto` , not only can I go through the list of subcommands but also see a description for what each of them does and how to use it. The same is true for all the flags which each subcommand supports.


But it doesn't just stop here, another really cool thing I liked about Fig was "Generators".[Generators](https://fig.io/docs/reference/generator) load up autocomplete suggestions for a command by running another relevant command behind the scenes for you. For example, if you were to type` okteto context use` , Fig would run` okteto context list` behind the scenes and load up autocomplete suggestions based on its output for you. Here's how this looks:


Pretty cool, right? Now that you see all the cool stuff Fig can do, let me take you behind the scenes and show you how we added Fig integrations for Okteto CLI so you too can leverage it for your CLI!


## Behind the Scenes


Fig's[documentation](https://fig.io/docs/getting-started) does a pretty good job of explaining how you can add support for your CLI in Fig. But what surprised me was how simple this process is if your CLI is built with[Cobra](https://github.com/spf13/cobra) . Don't take my word for it, just look how[Ramiro](https://twitter.com/rberrelleza) was able to integrate Fig with Okteto CLI following[this](https://fig.io/docs/guides/autocomplete-for-teams/cobra#fig-autocomplete--cobra) doc - LIVE in less than 15 minutes!


After this, all we had to do was raise a pull request to[withfig/autocomplete](https://github.com/withfig/autocomplete) with the generated typescript file. That covered most of the autocomplete suggestions you see for Okteto CLI. But we didn't want to stop here. Remember those super cool Generators we talked about above? Fig made[implementing](https://fig.io/docs/getting-started/generating-argument-suggestions#generators) those pretty simple too!


We just had to specify which command we wanted to run to generate the suggestions and how we wanted to process the output of that command to show the final suggestions. For example, in the case of` okteto namespace use` , we know that the user wants to switch their namespace, so we specified` okteto namespace list` as the command Fig should run in the background and then wrote some code telling Fig how to process the output.


```text
const namespaces: Fig.Generator = {
script: "okteto namespace list",
cache: {
ttl: 1000 * 60 * 30, // 30 minutes
},
postProcess: (output) => {
return output
.split("\n")
.slice(1)
.map((namespace, ind) => {
namespace = namespace.split(" ")[0];
return {
name: namespace.replace("*", "").trim(),
description: "Namespace",
icon: "fig://icon?type=okteto",
};
});
},
};


```


Another cool thing Fig allowed us to do was to cache the results of these commands we used to load the autocomplete suggestions so that the suggestions shown are almost instantaneous.


And that was it! Yes, it was this simple integrating Fig with Okteto CLI. If you're using Okteto CLI daily, do give Fig a try and let us know if you find it as useful as we do or not.


To conclude, I would say that Fig understands that a lot of people aren't as comfortable with the terminal as we usually like to believe. And even if you are on your terminal every day, the sheer amount of tools and subcommands to know keeps increasing every passing day. It is this very problem Fig solves - and they do a wonderful job of solving it. The trap that most developer tools fall into is that the solution they provide ends up being more complicated than the original problem they intended to solve. I was very glad to see Fig not falling down this hole and providing a simple yet extremely useful addition to my arsenal of daily tools!
