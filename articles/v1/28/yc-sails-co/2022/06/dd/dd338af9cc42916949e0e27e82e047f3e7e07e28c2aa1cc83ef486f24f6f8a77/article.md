---
schema_version: "1.0.0"
document_id: "dd338af9cc42916949e0e27e82e047f3e7e07e28c2aa1cc83ef486f24f6f8a77"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/good-bye-nuxt-hello-inertia-in-sails/"
published_at: "2022-06-22T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:a90e0d17d6119773352386f5dad5cecdbf864f043940975fa7f8ab29ed324626"
---

# Goodbye Nuxt, hello Inertia in Sails

## Prologue


Ever since I started developing UI with Vue, Nuxt was the next best thing since sliced bread. I really liked not having to worry about routing and a folder structure. Like every tech I love, Nuxt just gave me peace of mind by doing the heavy lifting when in comes to building UIs with Vue.


But there was this nudge at the back of my mind that my current setup is an overkill. Let’s look at why I think so.


So currently Sailscasts currently operate a client server architecture. That means[https://sailscasts.com](https://sailscasts.com/) is an SPA(sorta it’s something called an SSG) and it get’s its data via an API which is written in Sails.


So what this means is that, when you visit[https://sailscasts.com/courses](https://sailscasts.com/courses) the page is loaded as an HTML page and then an AJAX request is made to the API to fetch all courses so you can see them.


For years I loved this setup, heck I even advocated for it. The so called Jamstack. But like most things in web dev it was beginning to feel like it was overhyped and the benefit was just superficial.


This is because there is still a Network request to fetch those courses so even though it wasn’t server rendered it won’t make a difference and in my opinion seeing a loading spinner is not much of an improvement to letting the browser make a full page request.


So you may say “Kelvin, why don’t you just write server-side pages then?” Well the thing is I love Vue’s SFC and with Vue 3, SFC are more fun to write especially with the[setup script](https://vuejs.org/api/sfc-script-setup.html) .


Also, I love Sails and with the current setup, I don’t spend as much time as I want with the backend. Also I am duplicating authorization logic like Middlewares(Policies in Sails). And don’t get me started with authentication(and please don’t say JWT to the rescue).


Why can’t I have both Sails and Vue in a single codebase? It does exist actually with Parasails and that’s how most folks write fullstack Sails apps. But again I like Vue’s SFC and I do not really dig the Parasails API.


So how can I get the best of both worlds: Sails power in routing, authentication, authorization, ORM and write Vue SFC for my pages like I did with Nuxt(which drew me to Nuxt in the first place?). I found the answer in Jonathan Reinnik’s[Inertia](https://inertiajs.com/) .


Inertia poses to be the way to write modern monoliths. It’s a way to have the power of your server-side frameworks(thinks Sails, Rails and Laravel) and the joy and thrills of writing your UI in frameworks like Vue, React and Svelte.


I was beside myself with joy on discovering this. I researched how I can make this work for Sails and for a great number of weeks I couldn’t figure it out. I even read the entire Inertia documentation(no kidding). Finally I cracked it. I wrote the first spike of[inertia-sails](https://docs.sailscasts.com/inertia-sails/) - the Sails adapter for Inertia which let you have the power of Sails and the client-side framework of your choice in one codebase.


This was surreal for me. I quickly thought, this is the way to build fullstack modern Sails applications. So I created[create-sails](https://docs.sailscasts.com/create-sails/) - the quickest way to scaffold a modern fullstack Sails apps, which is basically a CLI to scaffold an Inertia powerered Sails project with Vue, React or Svelte as your frontend.


I plan on rewriting[sailscasts.com](http://sailscasts.com/) in this stack, I already did a POC of Inertia and see how it fairs in the realworld. If you want to see inertia at work,[laracasts.com](http://laracasts.com/) and[inertiajs.com](http://inertiajs.com/) are written in Inertia(their backend are Laravel)


I alway strive for simplicity and Inertia just take simplicity to another level. No more managing clientside states, No more duplicating routing as your Sails routes are your single source of truth for routing. Authorization and authentication are a breeze with policies and sessions. You also have Waterline to get your data and you can return the exact data you need for each page! B E A utiful!


If you like simplicity and want to escape the endless rabbit hole which is frontend development with SPA, this stack is for you. and it’s the best of both worlds. You won’t be missing out on anything and one other good thing is you will spend enough time server-side and develop your sever-side chops - which will in turn make you a one man super army and you can build your ideas and businesses easily.


I hope this setup will reduce the barrier to entry when you want to build your businesses or ideas on Sails.


So yes I am saying goodbye to Nuxt, not because it’s a terrible framework, but because I don’t like too much complexity and I can have everything I needed Nuxt for with Sails and Inertia.


I know there are cases you might need to build an API and that’s fine and with this setup, that’s also possible, your Inertia app can be exposed at[example.com](http://example.com/) and your api which is in the same codebase of course can be exposed at example.com/api/v1/you-get-the-idea


Again I say, the best of both world. You can get the SPA feel of writing a separate SPA with Inertia and you get to reuse your routes in the client, middleware or policies, ORM and everything you’ve come to love in your server-side framework of choice.


It’s amazing.


I’ll write again when the rewrite of[sailscasts.com](http://sailscasts.com/) in Inertia is done.


Here is to more simpler DX!


## FYI


Once I figure out how an amazing DX in getting` .md` in a Sails app to create blogs and documentation in Sails(perhaps powered by Inertia as well). Then I am an happy man. Think this blog, my personal website and every documentation I have ever written(and will write in the future) all on this stack.


P.S: My[talk](https://youtu.be/x7fkiLK9Sng) at Sailsconf is also about this amazing stack. You can check it out.
