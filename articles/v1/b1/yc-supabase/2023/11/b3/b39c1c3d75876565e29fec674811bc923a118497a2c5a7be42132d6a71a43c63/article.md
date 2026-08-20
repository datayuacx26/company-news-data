---
schema_version: "1.0.0"
document_id: "b39c1c3d75876565e29fec674811bc923a118497a2c5a7be42132d6a71a43c63"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-is-now-compatible-with-nextjs-14"
published_at: "2023-11-01T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:74432249bf4e49e10627b67dc2fb16dfdc9de8548f4ffabf4e469304b414fb50"
---

# Supabase is now compatible with Next.js 14

As part of[Next.js Conf 2023](https://nextjs.org/conf) , the team at Vercel released[Next.js 14](https://nextjs.org/blog/next-14) . The huge headline feature was...


That's right, the headline feature is no new features!


This may sound underwhelming at first, but is incredibly good news for the stability of Next.js. This release comes with a huge number of performance and stability improvements—such as[Server Actions](https://nextjs.org/docs/app/api-reference/functions/server-actions) being marked as stable. This means we can finally start promoting this fantastically simple way of authenticating users—entirely server-side!


`
_12


export default async function Page() {


_12


const signIn = async () => {


_12


'use server'


_12


supabase.auth.signInWithOAuth({...})


_12


}


_12


_12


return (


_12


<form action={signIn}>


_12


<button>Sign in with GitHub</button>


_12


</form>


_12


)


_12


}


`


With[Server Components](https://nextjs.org/docs/app/building-your-application/rendering/server-components) , fetching data in Next.js became as simple as:


`
_10


export default async function Page() {


_10


const { data } = await supabase.from('...').select()


_10


return ...


_10


}


`


With Server Actions, you can now place mutation logic alongside the Server Components responsible for fetching data and rendering the page:


`
_10


export default async function Page() {


_10


const { data } = await supabase.from('...').select()


_10


_10


const createNote = async () => {


_10


'use server'


_10


await supabase.from('...').insert({...})


_10


}


_10


_10


return ...


_10


}


`


> To hear more about our thoughts on Next.js Conf and the release of Next.js 14, check out our[Twitter space](https://twitter.com/i/spaces/1yoKMwNWbRjJQ?s=20) .[Yuri](https://twitter.com/yuricodesbot) ,[Alaister](https://twitter.com/alaisteryoung) ,[Terry](https://twitter.com/saltcod) and[myself](https://twitter.com/jonmeyers_io) talk through how we use Next.js at Supabase and what we personally found most exciting about Next.js Conf and the release of Next.js 14.


## Is Supabase compatible with Next.js 14?#


Yes, it is! So much so that[Guillermo Rauch](https://twitter.com/rauchg) shouted us out in the keynote!


Since the release of the[App Router](https://nextjs.org/docs/app) —launched as beta with Next.js 13—we have been working closely with the team at Vercel to ensure that Supabase is fully compatible with every part of Next.js.


So for the[App Router](https://nextjs.org/docs/app) , that's:


- [Client Components](https://nextjs.org/docs/app/building-your-application/rendering/client-components)
- [Server Components](https://nextjs.org/docs/app/building-your-application/rendering/server-components)
- [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)
- [Server Actions](https://nextjs.org/docs/app/api-reference/functions/server-actions)
- [Middleware](https://nextjs.org/docs/app/building-your-application/routing/middleware)


And for the[Pages Router](https://nextjs.org/docs/pages) :


- [getServerSideProps function](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)
- [getStaticProps function](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props)
- [API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)
- [Page Components](https://nextjs.org/docs/pages/building-your-application/data-fetching/client-side)


So why does it require work on the Supabase side to make it compatible with Next.js?


## Configuring Supabase to use Cookies#


By default,[supabase-js](https://supabase.com/docs/reference/javascript/introduction) uses` localStorage` to store the user's session. This works well for client-side apps, but will fail when you try to use[supabase-js](https://supabase.com/docs/reference/javascript/introduction) in a Server Component, as there is no concept of 'localStorage' on the server.


To do this, we need to configure` supabase-js` to use cookies instead of` localStorage` when running on the server. But this code is a little verbose to ask people to duplicate across every app they build with Supabase:


`
_19


const supabase = createClient(supabaseUrl, supabaseAnonKey, {


_19


auth: {


_19


flowType: 'pkce',


_19


autoRefreshToken: false,


_19


detectSessionInUrl: false,


_19


persistSession: true,


_19


storage: {


_19


getItem: async (key: string) => {


_19


cookieStore.get(key)


_19


},


_19


setItem: async (key: string, value: string) => {


_19


cookieStore.set(key, value)


_19


},


_19


removeItem: async (key: string) => {


_19


cookieStore.remove(key)


_19


},


_19


},


_19


},


_19


})


`


That takes care of the server-side pieces of Next.js, but since we recommend securing your apps with[Row Level Security (RLS) policies](https://www.youtube.com/watch?v=Ow_Uzedfohk) , you can safely access your user's session client-side too. Therefore, we need to tell the browser how access that cookie too:


`
_21


const supabase = createClient(supabaseUrl, supabaseAnonKey, {


_21


auth: {


_21


flowType: 'pkce',


_21


autoRefreshToken: true,


_21


detectSessionInUrl: true,


_21


persistSession: true,


_21


storage: {


_21


getItem: async (key: string) => {


_21


return parse(document.cookie\[key\])


_21


},


_21


setItem: async (key: string, value: string) => {


_21


document.cookie = serialize(key, value)


_21


},


_21


},


_21


removeItem: async (key: string) => {


_21


document.cookie = serialize(key, '', {


_21


maxAge: 0,


_21


})


_21


},


_21


},


_21


})


`


That is a lot of very confusing code! So we decided to[create a package called @supabase/ssr](https://supabase.com/docs/guides/auth/server-side/overview) that does all of this for you. Then we took it one step further and created a[Next.js and Supabase Starter Template](https://supabase.com/docs/guides/getting-started/quickstarts/nextjs) , so you can just focus on building your awesome app! 🚀


> Check out my[Next.js Conf talk](https://www.youtube.com/watch?t=3880&v=FdiX5rHS_0Y) to see this starter template in action!


## How do I get started?#


One command:


`
_10


npx create-next-app@latest -e with-supabase


`


The landing page will guide you through the process of creating a Supabase project and configuring your environment variables.


Build in a weekend on a Friday night! Scale to millions!


## Meme zone#


As you probably know, we love memes, so we are signing off with one about the least controversial commands coming out of Next.js Conf:


## More Supabase and Next.js resources#


- [Next.js Quickstart](https://supabase.com/docs/guides/getting-started/quickstarts/nextjs)
- [Build a User Management App with Next.js](https://supabase.com/docs/guides/getting-started/tutorials/with-nextjs)
- [Server-Side Auth Overview](https://supabase.com/docs/guides/auth/server-side/overview)
- [Fetching and caching Supabase data in Next.js 13 Server Components](https://supabase.com/blog/fetching-and-caching-supabase-data-in-next-js-server-components)
- [Infinite scroll with Next.js, Framer Motion, and Supabase](https://supabase.com/blog/infinite-scroll-with-nextjs-framer-motion)
