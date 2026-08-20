---
schema_version: "1.0.0"
document_id: "c884f3b3e305264a7b3e88c2d22fd44e7ac181c7ed01af4e50ddc79170363802"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgres-realtime-location-sharing-with-maplibre"
published_at: "2024-07-04T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:07195eebbbd7499dabe37d50cca30ab7966158cfed36d864e8536ea580bcfe38"
---

# Postgres Realtime location sharing with MapLibre

[Do you prefer audio-visual learning? Watch the video guide!](https://supabase.link/realtime-maps-yt)


[Or jump straight into the code](https://supabase.link/realtime-maps-gh)


This tutorial is building upon the previous learnings on Postgis and Supabase and adding Supabase Realtime on top. If you're new to this topic, we recommend you review the following first:


- Getting started with PostGIS and Supabase[\[video\]](https://supabase.link/postgis-quickstart-yt)[\[docs\]](https://supabase.com/docs/guides/database/extensions/postgis)
- Self-host Maps with Protomaps and Supabase Storage[\[video\]](https://supabase.link/protomaps-storage-yt)[\[blog\]](https://supabase.com/blog/self-host-maps-storage-protomaps)
- Generate Vector Tiles with PostGIS[\[video\]](https://supabase.link/supa-gis-yt)[\[blog\]](https://supabase.com/blog/postgis-generate-vector-tiles)


In this tutorial, you will learn to


- Use a Supabase Edge Function to build a Telegram Bot that captures live location data.
- Use an RPC (remote procedure call) to insert location data into Postgres from an Edge Function.
- Use Supabase Realtime to listen to changes in the database.
- Use MapLibre GL JS in React to draw live location data onto the map.


## Use an Edge Functions to write location data to Supabase#


In this section, you will create an Edge Function that will capture live location data from a Telegram Bot. The Telegram Bot will send location data to the Edge Function, which will then insert the data into Supabase.


For a detailed guide on how to create a Telegram Bot, please refer to our docs[here](https://supabase.com/docs/guides/functions/examples/telegram-bot) .


You can find the production ready code for the Telegram Bot Supabase Edge Function on[GitHub](https://github.com/thorwebdev/location-tRacer/blob/main/supabase/functions/telegram-bot/index.ts) . This is the relevant code that listens to the live location updates and writes them to the database:


/supabase/functions/telegram-bot/


index.ts


`
_58


import { Bot, webhookCallback } from 'https://deno.land/x/grammy@v1.20.3/mod.ts'


_58


import { createClient } from 'jsr:@supabase/supabase-js@2.39.7'


_58


import { Database } from '../_shared/database.types.ts'


_58


_58


const token = Deno.env.get('BOT_TOKEN')


_58


if (!token) throw new Error('BOT_TOKEN is unset')


_58


_58


const supabase = createClient<Database>(


_58


Deno.env.get('SUPABASE_URL')!,


_58


Deno.env.get('SUPABASE_SERVICE_ROLE_KEY')!


_58


)


_58


_58


const bot = new Bot(token)


_58


// ...


_58


_58


bot.on('edit:location', async (ctx) => {


_58


const {


_58


location,


_58


from: { id: user_id },


_58


edit_date,


_58


} = ctx.update.edited_message!


_58


if (location) {


_58


// Insert into db


_58


const { error } = await supabase.rpc('location_insert', {


_58


_user_id: user_id,


_58


_lat: location.latitude,


_58


_long: location.longitude,


_58


_timestamp: edit_date,


_58


})


_58


if (


_58


error &&


_58


error.message !==


_58


'null value in column "event_id" of relation "locations" violates not-null constraint' &&


_58


error.message !== 'duplicate key value violates unique constraint "locations_pkey"'


_58


) {


_58


return console.log(\`edit:location:insert:error:user:${user_id}: ${error.message}\`)


_58


}


_58


}


_58


return


_58


})


_58


_58


const handleUpdate = webhookCallback(bot, 'std/http')


_58


_58


Deno.serve(async (req) => {


_58


const headers = req.headers


_58


try {


_58


const url = new URL(req.url)


_58


if (url.searchParams.get('secret') !== Deno.env.get('FUNCTION_SECRET')) {


_58


return new Response('not allowed', { status: 405 })


_58


}


_58


_58


return await handleUpdate(req)


_58


} catch (err) {


_58


console.log(headers)


_58


console.error(err)


_58


}


_58


return new Response()


_58


})


`


## Use an RPC to insert location data into Postgres#


The edge function above uses an RPC (remote procedure call) to insert the location data into the database. The RPC is defined in our[Supabase Migrations](https://github.com/thorwebdev/location-tRacer/blob/main/supabase/migrations/20240227024905_init.sql#L80) . The RPC first validates that the user has an active session and then inserts the location data into the` locations` table:


`
_10


CREATE OR REPLACE FUNCTION public.location_insert(_timestamp bigint, _lat double precision, _long double precision, _user_id bigint)


_10


RETURNS void AS $$


_10


declare active_event_id uuid;


_10


begin


_10


select event_id into active_event_id from public.sessions where user_id = _user_id and status = 'ACTIVE'::session_status;


_10


_10


INSERT INTO public.locations(event_id, user_id, created_at, lat, long, location)


_10


VALUES (active_event_id, _user_id, to_timestamp(_timestamp), _lat, _long, st_point(_long, _lat));


_10


end;


_10


$$ LANGUAGE plpgsql VOLATILE;


`


## Use Supabase Realtime to listen to changes in the database#


In this section, you will use Supabase Realtime to listen to changes in the database. The Realtime API is a powerful tool that allows you to broadcast changes in the database to multiple clients.


The full client-side code for listening to the realtime changes and drawing the marker onto the map is available on[GitHub](https://github.com/thorwebdev/location-tRacer/blob/main/app/app/realtimemap/%5Bevent%5D/page.tsx) .


We're going to brake it down into a couple of steps:


Since we're working in React, we will set up the Realtime subscription in the` useEffect` hook. If you're using Next.js, it's important to mark this with` use client` as we will need client-side JavaScript to make this happen:


/app/app/realtimemap/%5Bevent%5D/


page.tsx


`
_41


// ...


_41


export default function Page({ params }: { params: { event: string } }) {


_41


const supabase = createClient<Database>()


_41


const \[locations, setLocations\] = useState<{


_41


\[key: string\]: Tables<'locations'>


_41


} | null>(null)


_41


const locationsRef = useRef<{


_41


\[key: string\]: Tables<'locations'>


_41


} | null>()


_41


locationsRef.current = locations


_41


_41


useEffect(() => {


_41


// Listen to realtime updates


_41


const subs = supabase


_41


.channel('schema-db-changes')


_41


.on(


_41


'postgres_changes',


_41


{


_41


event: 'INSERT', // Listen only to INSERTs


_41


schema: 'public',


_41


table: 'locations',


_41


filter: \`event_id=eq.${params.event}\`,


_41


},


_41


(payload) => {


_41


const loc = payload.new as Tables<'locations'>


_41


const updated = {


_41


...locationsRef.current,


_41


\[loc.user_id.toString()\]: loc,


_41


}


_41


_41


setLocations(updated)


_41


}


_41


)


_41


.subscribe()


_41


console.log('Subscribed')


_41


_41


return () => {


_41


subs.unsubscribe()


_41


}


_41


}, \[\])


_41


// ...


`


## Use MapLibre GL JS in React to draw live location data onto the map#


The realtime subscription listener above updates the state of the` locations` object with the new location data, anytime it is inserted into the table. We can now use` react-map-gl` to easily draw the location markers onto the map:


/app/app/realtimemap/%5Bevent%5D/


page.tsx


`
_33


// ...


_33


<Map


_33


className="map"


_33


cooperativeGestures={true}


_33


initialViewState={{


_33


longitude: 103.852713,


_33


latitude: 1.285727,


_33


zoom: 13,


_33


}}


_33


mapStyle={{


_33


version: 8,


_33


glyphs: 'https://cdn.protomaps.com/fonts/pbf/{fontstack}/{range}.pbf',


_33


sources: {


_33


protomaps: {


_33


attribution:


_33


'<a href="https://github.com/protomaps/basemaps">Protomaps</a> © <a href="https://openstreetmap.org">OpenStreetMap</a>',


_33


type: 'vector',


_33


url: 'pmtiles://https://<project_ref>.supabase.co/functions/v1/maps-private/my_area.pmtiles',


_33


},


_33


},


_33


transition: {


_33


duration: 0,


_33


},


_33


// @ts-ignore


_33


layers: layers('protomaps', 'light'),


_33


}}


_33


// @ts-ignore


_33


mapLib={maplibregl}


_33


>


_33


{Object.entries(locations).map((\[key, value\]) => (


_33


<Marker key={key} longitude={value.long} latitude={value.lat} color="red" />


_33


))}


_33


</Map>


`


Note that we're using[Protomaps](https://protomaps.com/) hosted on Supabase Storage here for the base map. To learn about this read our[previous tutorial here](https://supabase.com/blog/self-host-maps-storage-protomaps) .


That's it, this is how easy it is to add realtime location data to your applications using Supabase! We can't wait to see what you will build!


## Conclusion#


Supabase Realtime is ideal for broadcasting location data to multiple clients. Combined with the power of PostGIS and the broader Postgres extension ecosystem, its's a powerful solution for all your geospatial needs!


Want to learn more about Maps and PostGIS? Make sure to follow our[Twitter](https://x.com/supabase) and[YouTube](https://www.youtube.com/@Supabase) channels to not miss out! See you then!


## More Supabase#


- [Watch the video guide](https://supabase.link/realtime-maps-yt)
- [Find the code](https://supabase.link/realtime-maps-gh)
- [Build your own open source Google Maps API alternative](https://supabase.link/supa-gis-yt)
- [Self-host Maps on Supabase Storage with Protomaps](https://supabase.link/protomaps-storage-yt)
- [Getting started with PostGIS](https://supabase.link/postgis-quickstart-yt)
- [PostGIS docs guide](https://supabase.com/docs/guides/database/extensions/postgis)
