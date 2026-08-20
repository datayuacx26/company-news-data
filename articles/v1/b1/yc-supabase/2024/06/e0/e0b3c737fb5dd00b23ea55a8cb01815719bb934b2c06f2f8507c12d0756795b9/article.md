---
schema_version: "1.0.0"
document_id: "e0b3c737fb5dd00b23ea55a8cb01815719bb934b2c06f2f8507c12d0756795b9"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/self-host-maps-storage-protomaps"
published_at: "2024-06-19T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:4d6cb974d27354b1a1d8c2f524ee925663fd4af9300bcfeb6906218337c0704a"
---

# Self-host Maps with Protomaps and Supabase Storage

[Do you prefer the audio-visual learning? Watch the video guide!](https://supabase.link/protomaps-storage-yt)


[Protomaps](https://protomaps.com/) is an open source map of the world, deployable as a single static file on[Supabase Storage](https://supabase.com/storage) .


In this tutorial, you will learn to


- Use Protomaps to excract an area into a static PMTiles file.
- Upload the PMTiles file to Supabase Storage.
- Use MapLibre to render the Map onto a Web Page.
- Use Supabase Edge Functions to restrict File Access.


## Extract an area into a static PMTiles file#


Protomaps provides a[pmtiles CLI](https://docs.protomaps.com/guide/getting-started#_1-install-the-cli) that can be used to cut out certain areas from the world map and compress those into a single static file.


For example, we can[extract a small area](https://docs.protomaps.com/guide/getting-started#_3-extract-any-area) around Utrecht in the Netherlands like this:


`
_10


pmtiles extract https://build.protomaps.com/20240618.pmtiles my_area.pmtiles --bbox=5.068050,52.112086,5.158424,52.064140


`


Note: make sure to update the date to the latest daily build!


This will create a` my_area.pmtiles` file which you can upload to Supabase Storage.


## Upload the PMTiles file to Supabase Storage#


In your[Supabase Dashboard](https://supabase.com/dashboard/project/_/storage/buckets) navigate to` Storage` and click "New Bucket" and create a new public bucket called` public-maps` .


Upload the` my_area.pmtiles` file created earlier to your public bucket. Once uploaded, click the file and tap "Get URL".


Supabase Storage supports the required[HTTP Range Requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Range_requests) out of the box, allowing you to use the public storage URL directly from your maps client.


## Use MapLibre to render the Map#


PMTiles easily works with both[MapLibre GL](https://docs.protomaps.com/pmtiles/maplibre) and[Leaflet](https://docs.protomaps.com/pmtiles/leaflet) . In our example we wil use[MapLibre GL](https://maplibre.org/maplibre-gl-js/docs/) , which is a TypeScript library that uses WebGL to render interactive maps from vector tiles in a browser.


This is a vanilla JS example which uses CDN releases of the libraries. You can very easily adapt it to work with React as well, for example using the[react-map-gl](https://visgl.github.io/react-map-gl/) library.


index.html


`
_54


<html>


_54


<head>


_54


<title>Overture Places</title>


_54


<meta charset="utf-8" />


_54


<meta name="viewport" content="width=device-width, initial-scale=1.0" />


_54


<link


_54


rel="stylesheet"


_54


href="https://unpkg.com/maplibre-gl@4.1.2/dist/maplibre-gl.css"


_54


crossorigin="anonymous"


_54


/>


_54


<script


_54


src="https://unpkg.com/maplibre-gl@4.1.2/dist/maplibre-gl.js"


_54


crossorigin="anonymous"


_54


></script>


_54


<script src="https://unpkg.com/protomaps-themes-base@2.0.0-alpha.5/dist/index.js"></script>


_54


<script src="https://unpkg.com/pmtiles@3.0.6/dist/pmtiles.js"></script>


_54


<style>


_54


body {


_54


margin: 0;


_54


}


_54


#map {


_54


height: 100%;


_54


width: 100%;


_54


}


_54


</style>


_54


</head>


_54


<body>


_54


<div id="map"></div>


_54


<script type="text/javascript">


_54


// Add the PMTiles Protocol:


_54


let protocol = new pmtiles.Protocol()


_54


maplibregl.addProtocol('pmtiles', protocol.tile)


_54


_54


// Load the Map tiles directly from Supabase Storage:


_54


const map = new maplibregl.Map({


_54


hash: true,


_54


container: 'map',


_54


style: {


_54


version: 8,


_54


glyphs: 'https://cdn.protomaps.com/fonts/pbf/{fontstack}/{range}.pbf',


_54


sources: {


_54


protomaps: {


_54


attribution:


_54


'<a href="https://github.com/protomaps/basemaps">Protomaps</a> © <a href="https://openstreetmap.org">OpenStreetMap</a>',


_54


type: 'vector',


_54


url: 'pmtiles://https://<your-project-ref>.supabase.co/storage/v1/object/public/public-maps/my_area.pmtiles',


_54


},


_54


},


_54


layers: protomaps_themes_base.default('protomaps', 'dark'),


_54


},


_54


})


_54


</script>


_54


</body>


_54


</html>


`


## Use Supabase Edge Functions to restrict Access#


A public Supabase Storage bucket allows access from any origin, which might not be ideal for your use case. At the time of writing, you're not able to modify the CORS settings for Supabase Storage buckets, however you can utilize[Supabase Edge Functions](https://supabase.com/docs/guides/functions) to restrict access to your PMTiles files, allowing you to even pair it with[Supabase Auth](https://supabase.com/docs/guides/auth) to restrict access to certain users for example.


In your Supabase Dashboard, create a new private storage bucket called` maps-private` and upload your` my_area.pmtiles` file there. Files in private buckets can only be accessed through either a short-lived signed URL, or by passing the secret service role key as an authorization header. Since our Edge Function is a secure server-side environment, we can utilize the latter approach here.


Using the[Supabase CLI](https://github.com/supabase/cli) , create a new Edge Function by running` supabase functions new maps-private` , then add the following code to your newly created function:


supabase/functions/maps-private/


index.ts


`
_31


const ALLOWED_ORIGINS = \['http://localhost:8000'\]


_31


const corsHeaders = {


_31


'Access-Control-Allow-Origin': ALLOWED_ORIGINS.join(','),


_31


'Access-Control-Allow-Headers':


_31


'authorization, x-client-info, apikey, content-type, range, if-match',


_31


'Access-Control-Expose-Headers': 'range, accept-ranges, etag',


_31


'Access-Control-Max-Age': '300',


_31


}


_31


_31


Deno.serve((req) => {


_31


// This is needed if you're planning to invoke your function from a browser.


_31


if (req.method === 'OPTIONS') {


_31


return new Response('ok', { headers: corsHeaders })


_31


}


_31


_31


// Check origin


_31


const origin = req.headers.get('Origin')


_31


_31


if (!origin || !ALLOWED_ORIGINS.includes(origin)) {


_31


return new Response('Not Allowed', { status: 405 })


_31


}


_31


_31


const reqUrl = new URL(req.url)


_31


const url = \`${Deno.env.get('SUPABASE_URL')}/storage/v1/object/authenticated${reqUrl.pathname}\`


_31


_31


const { method, headers } = req


_31


// Add Auth header


_31


const modHeaders = new Headers(headers)


_31


modHeaders.append('authorization', \`Bearer ${Deno.env.get('SUPABASE_SERVICE_ROLE_KEY')!}\`)


_31


return fetch(url, { method, headers: modHeaders })


_31


})


`


If you want to further restrict access based on authenticated users, you can pair your Edge Function with Supabase Auth as shown in[this example](https://supabase.com/docs/guides/functions/auth#fetching-the-user) .


Lastly, we need to deploy our Edge Function to Supabase by running` supabase functions deploy maps-private --no-verify-jwt` . Note that the[--no-verify-jwt flag](https://supabase.com/docs/reference/cli/supabase-functions-deploy) is required if you want to allow public access from your website without any Supabase Auth User.


Now we can simply replace the public storage URL with our Edge Functions URL to proxy the range requests to our private bucket:


index.html


`
_19


// ...


_19


const map = new maplibregl.Map({


_19


hash: true,


_19


container: 'map',


_19


style: {


_19


version: 8,


_19


glyphs: 'https://cdn.protomaps.com/fonts/pbf/{fontstack}/{range}.pbf',


_19


sources: {


_19


protomaps: {


_19


attribution:


_19


_19


type: 'vector',


_19


url: 'pmtiles://https://<project_ref>.supabase.co/functions/v1/maps-private/my_area.pmtiles',


_19


},


_19


},


_19


layers: protomaps_themes_base.default('protomaps', 'dark'),


_19


},


_19


})


_19


// ...


`


Now go ahead and serve your` index.html` file, for example via Python SimpleHTTPServer:` python3 -m http.server` and admire your beautiful map on[localhost:8000](http://localhost:8000/) !


## Expo React Native#


As you might know, I'm a big React Native fan, and when writing this tutorial I was very excited about making this work in Expo mobile apps also.


Unfortunately, at the time of writing, custom protocols are not supported in[maplibre-react-native](https://github.com/maplibre/maplibre-react-native) . There is an issues tracking this[here](https://github.com/maplibre/maplibre-react-native/issues/28) , so if there are any native mobile wizards out there, I'd very much appreciate your contributions!


In the meantime, however, the Expo team had a great idea, what about leveraging[React DOM components](https://docs.expo.dev/guides/dom-components/) , which are currently experimentally supported in Expo SDK 52 preview.


This approach allows you to utilize[react-map-gl](https://github.com/visgl/react-map-gl) and[maplibre-gl-js](https://github.com/maplibre/maplibre-gl-js) across your Expo web and mobile apps.


[Do you prefer the audio-visual learning? Watch the video guide!](https://supabase.link/react-native-maps-yt) .


Or jump straight into the[code](https://supabase.link/react-native-maps-gh) .


Follow the steps to[install the canary release](https://docs.expo.dev/versions/unversioned#canary-releases) .


To render a React component to the DOM, add the 'use dom' directive to the top of the web component file:


Map.jsx


`
_47


'use dom'


_47


_47


import 'text-encoding-polyfill'


_47


import { useEffect } from 'react'


_47


import Map from 'react-map-gl'


_47


import maplibregl from 'maplibre-gl'


_47


import 'maplibre-gl/dist/maplibre-gl.css'


_47


import { Protocol } from 'pmtiles'


_47


_47


export default function MapBox(_) {


_47


useEffect(() => {


_47


let protocol = new Protocol()


_47


maplibregl.addProtocol('pmtiles', protocol.tile)


_47


return () => {


_47


maplibregl.removeProtocol('pmtiles')


_47


}


_47


}, \[\])


_47


_47


return (


_47


<div style={{ width: '100%', height: '100%' }}>


_47


<Map


_47


style={{ width: '100%', height: 900 }}


_47


mapStyle={{


_47


version: 8,


_47


sources: {


_47


sample: {


_47


type: 'vector',


_47


url: 'pmtiles://https://r2-public.protomaps.com/protomaps-sample-datasets/cb_2018_us_zcta510_500k.pmtiles',


_47


},


_47


},


_47


layers: \[


_47


{


_47


id: 'zcta',


_47


source: 'sample',


_47


'source-layer': 'zcta',


_47


type: 'line',


_47


paint: {


_47


'line-color': '#999',


_47


},


_47


},


_47


\],


_47


}}


_47


mapLib={maplibregl}


_47


/>


_47


</div>


_47


)


_47


}


`


Inside the native component file, import the web component to use it:


App.jsx


`
_21


import { StatusBar } from 'expo-status-bar'


_21


import { StyleSheet, Text, View } from 'react-native'


_21


import Map from './Map.jsx'


_21


_21


export default function App() {


_21


return (


_21


<View style={styles.container}>


_21


<Map dom={{ autoSize: true }} />


_21


<StatusBar style="auto" />


_21


</View>


_21


)


_21


}


_21


_21


const styles = StyleSheet.create({


_21


container: {


_21


flex: 1,


_21


backgroundColor: '#fff',


_21


alignItems: 'stretch',


_21


justifyContent: 'center',


_21


},


_21


})


`


## Conclusion#


Protomaps is a fantastic open source project that allows you to host your own Google Maps alternative on Supabase Storage. You can further extend this with powerful PostGIS capabilities to programmatically generate[Vector Tiles](https://github.com/mapbox/vector-tile-spec) which we will explore in the next post in this series. So make sure you subscribe to our[Twitter](https://x.com/supabase) and[YouTube](https://www.youtube.com/@Supabase) channels to not miss out! See you then!


## More Supabase#


- [Watch the video guide](https://youtu.be/l7QBpiLRwJc)
- [Find the code](https://github.com/supabase/supabase/tree/master/examples/storage/protomaps)
- [Getting started with PostGIS video](https://youtu.be/MWfB0t5u3V0)
- [PostGIS docs guide](https://supabase.com/docs/guides/database/extensions/postgis)
