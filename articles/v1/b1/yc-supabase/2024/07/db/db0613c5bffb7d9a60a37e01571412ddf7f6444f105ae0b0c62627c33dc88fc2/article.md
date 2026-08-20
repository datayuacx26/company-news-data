---
schema_version: "1.0.0"
document_id: "db0613c5bffb7d9a60a37e01571412ddf7f6444f105ae0b0c62627c33dc88fc2"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-js-on-jsr"
published_at: "2024-07-16T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:0fe8175f3a51eb7c62e9183c5e38944fed623899e104afe1352f52f15983e066"
---

# Announcing Supabase on JSR

Prefer Deno's awesome visuals? Read the announcement on[Deno's blog](https://deno.com/blog/supabase-on-jsr) .


[JSR](https://jsr.io/) is a modern[open source](https://github.com/jsr-io/jsr) JavaScript registry that simplifies publishing and importing JavaScript and TypeScript modules. JSR supports publishing TypeScript source code, auto-generating documentation and type definition files, provenance attestation for more security, and can be used with npm-like package managers. Since its launch, over 250 new packages are being published each week.


We're thrilled to announce that our[JavaScript client library](https://jsr.io/@supabase/supabase-js) is now available on JSR.


As many of you know, our JavaScript library,` supabase-js` , is composed of[many sub-clients](https://github.com/supabase-community#client-libraries) that let you query your Supabase[database](https://supabase.com/database) , subscribe to[real-time events](https://supabase.com/realtime) , upload and download[files](https://supabase.com/storage) , manage[auth](https://supabase.com/auth) , invoke Deno powered[Edge Functions](https://supabase.com/edge-functions) , and[more](https://supabase.com/vector) . It's fully isomorphic and can be used across any environment that speaks JavaScript and HTTPS, such as browsers, servers, and[the edge](https://deno.com/blog/the-future-of-web-is-on-the-edge) !


Using supabase-js via JSR offers an excellent developer experience, with first class TypeScript support, auto-generated documentation accessible in your code editor, and more.


## Installing Supabase#


You can get started with Supabase using the` deno add` command:


`
_10


deno add @supabase/supabase-js


`


Or using npm:


`
_10


npx jsr add @supabase/supabase-js


`


The above commands will generate a` deno.json` file, listing all your project dependencies.


deno.json


`
_10


{


_10


"imports": {


_10


"@supabase/supabase-js": "jsr:@supabase/supabase-js@2"


_10


}


_10


}


`


You can then import the client library to your` main.ts` file:


main.ts


`
_10


import { createClient } from '@supabase/supabase-js'


_10


_10


const supabase = createClient('https://xyzcompany.supabase.co', 'public-anon-key')


_10


_10


console.log('Supabase Instance: ', supabase)


`


Finally, you can run the following command to execute:


`
_10


deno run -A main.ts


`


Check out the[supabase-js README](https://jsr.io/@supabase/supabase-js) to see how to use it in other environments.


## What's next?#


With the Supabase client on JSR, you can easily and quickly add authentication or persistent storage to your projects, which can run in any JavaScript environment!


- [Check out JSR](https://jsr.io/) the JavaScript registry built for the modern web.
- Read the announcement on[Deno's blog](https://deno.com/blog/supabase-on-jsr) .
