---
schema_version: "1.0.0"
document_id: "2be5eafa6d9374a84d268190a297283bfc61a30f2f29fc3c0ea67f2b9bfc713b"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgis-generate-vector-tiles"
published_at: "2024-06-26T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:3d6cd656822a8950cd471fa5d36b6cf20768e51b7ee0c5c7bb2d836dd548383b"
---

# Generate Vector Tiles with PostGIS

[Do you prefer audio-visual learning? Watch the video guide!](https://supabase.link/supa-gis-yt-docs)


[Or jump straight into the code](https://github.com/bdon/supabase-vector-tile)


[Overture Maps Foundation](https://overturemaps.org/) is a[Joint Development Foundation Project](https://jointdevelopment.org/) initiated by Amazon, Meta, Microsoft, and tomtom, aiming to create reliable, easy-to-use, and interoperable open map data.


Overture Maps allows us to download open map data, like places of interest, as[GeoJSON](https://geojson.org/) which we can transform into SQL and ingest into our Postgres database on Supabase.


Using PostGIS we can then programmatically generate vector tiles and serve them to our MapLibre GL client using supabase-js.


Vector tiles are packets of geographic data, packaged into pre-defined roughly-square shaped "tiles" for transfer over the web. Map data is requested by a client as a set of "tiles" corresponding to square areas of land of a pre-defined size and location.


Especially for large datasets, this has the benefit that the data transfer is greatly reduced because only data within the current viewport, and at the current zoom level needs to be transferred.


In this tutorial, you will learn to


- Use Overture Maps to download open map places data in GeoJSON format.
- Use GDAL ogr2ogr to transform GeoJSON into SQL statements.
- Import location data and JSON metadata into your Supabase Postgres database using psql.
- Use PostGIS'` ST_AsMVT` to aggregate a set of rows corresponding to a tile layer into a binary vector tile representation.
- Use MapLibre's` addProtocol` to visualize large PostGIS tables by making remote procedure calls with supabase-js.
- Use supabase-js to fetch additional JSON metadata on demand


## Download open map data with Overture Maps#


Overture Maps provides a[python command-line tool](https://docs.overturemaps.org/getting-data/overturemaps-py/) to download data within a region of interest and converts it to several common geospatial file formats.


We can download places in Singapore into a GeoJSON file with this command:


`
_10


overturemaps download --bbox=103.570233,1.125077,104.115855,1.490957 -f geojson --type=place -o places.geojson


`


Depending on the size of the bounding box this can take quite some time!


## Transform GeoJSON into SQL#


In the next step, we can use[GDAL ogr2ogr](https://gdal.org/programs/ogr2ogr.html) to transform the GeoJSON file into a PostGIS compatible SQL file.


You can install` GDAL` via` homebrew brew install gdal` or follow the[download instructions](https://gdal.org/download.html) .


`
_10


PG_USE_COPY=true ogr2ogr -f pgdump places.sql places.geojson


`


## Import location data into Supabase#


Enable the PostGIS extension on your Supabase Database on a dedicated separate` gis` schema. To do so you can navigate to the[SQL Editor](https://supabase.com/dashboard/project/_/sql/new) and run the following SQL, or you can enable the extension from the[Database Extensions Settings](https://supabase.com/dashboard/project/_/database/extensions) .


As PostGIS can be quite compute heavy, we recommend enabling it on a dedicated separate schema, for example, named` gis` !


`
_10


CREATE SCHEMA IF NOT EXISTS "gis";


_10


CREATE EXTENSION IF NOT EXISTS "postgis" WITH SCHEMA "gis";


`


Import the open map data into a` places` table in Supabase:


`
_10


psql -h aws-0-us-west-1.pooler.supabase.com -p 5432 -d postgres -U postgres.project-ref < places.sql


`


You can find the credentials in the[project connect page](https://supabase.com/dashboard/project/_?showConnect=true) of your Supabase Dashboard.


### Enable RLS and create a public read policy#


We want the places data to be available publicly, so we can create a row level security policy that enables public read access.


In your Supabase Dashboard, navigate to the[SQL Editor](https://supabase.com/dashboard/project/_/sql/new) and run the following:


`
_10


ALTER TABLE "public"."places" ENABLE ROW LEVEL SECURITY;


_10


_10


CREATE POLICY "Enable read access for all users" ON "public"."places" FOR SELECT USING (true);


`


## Generate vector tiles with PostGIS#


To programmatically generate vector tiles on client-side request, we need to create a Postgres function that we can invoke via a[remote procedure call](https://supabase.com/docs/reference/javascript/rpc) . In your SQL Editor, run:


`
_44


CREATE OR REPLACE FUNCTION mvt(z integer, x integer, y integer)


_44


RETURNS text


_44


LANGUAGE plpgsql


_44


AS $$


_44


DECLARE


_44


mvt_output text;


_44


BEGIN


_44


WITH


_44


-- Define the bounds of the tile using the provided Z, X, Y coordinates


_44


bounds AS (


_44


SELECT ST_TileEnvelope(z, x, y) AS geom


_44


),


_44


-- Transform the geometries from EPSG:4326 to EPSG:3857 and clip them to the tile bounds


_44


mvtgeom AS (


_44


SELECT


_44


-- include the name and id only at zoom 13 to make low-zoom tiles smaller


_44


CASE


_44


WHEN z > 13 THEN id


_44


ELSE NULL


_44


END AS id,


_44


CASE


_44


WHEN z > 13 THEN names::json->>'primary'


_44


ELSE NULL


_44


END AS primary_name,


_44


categories::json->>'main' as main_category,


_44


ST_AsMVTGeom(


_44


ST_Transform(wkb_geometry, 3857), -- Transform the geometry to Web Mercator


_44


bounds.geom,


_44


4096, -- The extent of the tile in pixels (commonly 256 or 4096)


_44


0, -- Buffer around the tile in pixels


_44


true -- Clip geometries to the tile extent


_44


) AS geom


_44


FROM


_44


places, bounds


_44


WHERE


_44


ST_Intersects(ST_Transform(wkb_geometry, 3857), bounds.geom)


_44


)


_44


-- Generate the MVT from the clipped geometries


_44


SELECT INTO mvt_output encode(ST_AsMVT(mvtgeom, 'places', 4096, 'geom'),'base64')


_44


FROM mvtgeom;


_44


_44


RETURN mvt_output;


_44


END;


_44


$$;


`


To limit the amount of data sent over the wire, we limit the amount of metadata to include in the vector tile. For example we add a condition for the zoom level, and only return the place name when the user has zoomed in beyond level 13.


## Use supabase-js to fetch vector tiles from MapLibre GL client#


You can find the full` index.html` code on[GitHub](https://github.com/bdon/supabase-vector-tile/blob/main/index.html) . Here we'll highlight how to add a new protocol to MapLibreGL to fetch the bas64 encoded binary vector tile data via supabase-js so that MapLibre GL can fetch and render the data as your users interact with the map:


index.html


`
_26


const client = supabase.createClient('your-supabase-api-url', 'your-supabase-anon-key')


_26


_26


function base64ToArrayBuffer(base64) {


_26


var binaryString = atob(base64)


_26


var bytes = new Uint8Array(binaryString.length)


_26


for (var i = 0; i < binaryString.length; i++) {


_26


bytes\[i\] = binaryString.charCodeAt(i)


_26


}


_26


return bytes


_26


}


_26


_26


maplibregl.addProtocol('supabase', async (params, abortController) => {


_26


const re = new RegExp(/supabase:\\/\\/(.+)\\/(\\d+)\\/(\\d+)\\/(\\d+)/)


_26


const result = params.url.match(re)


_26


const { data, error } = await client.rpc('mvt', {


_26


z: result\[2\],


_26


x: result\[3\],


_26


y: result\[4\],


_26


})


_26


const encoded = base64ToArrayBuffer(data)


_26


if (!error) {


_26


return { data: encoded }


_26


} else {


_26


throw new Error(\`Tile fetch error:\`)


_26


}


_26


})


`


With the supabase protocol registered, we can now add it to our MapLibre GL sources on top of a basemap like[Protomaps](https://protomaps.com/) for example:


index.html


`
_22


// ...


_22


const map = new maplibregl.Map({


_22


hash: true,


_22


container: 'map',


_22


style: {


_22


version: 8,


_22


glyphs: 'https://cdn.protomaps.com/fonts/pbf/{fontstack}/{range}.pbf',


_22


sources: {


_22


supabase: {


_22


type: 'vector',


_22


tiles: \['supabase://boston/{z}/{x}/{y}'\],


_22


attribution: '© <a href="https://overturemaps.org">Overture Maps Foundation</a>',


_22


},


_22


protomaps: {


_22


type: 'vector',


_22


url: 'https://api.protomaps.com/tiles/v3.json?key=your-protomaps-api-key',


_22


attribution: 'Basemap © <a href="https://openstreetmap.org">OpenStreetMap</a>',


_22


},


_22


},


_22


},


_22


})


_22


// ...


`


## On demand fetch additional JSON metadata#


To limit the amount of data sent over the wire, we don't encode all the metadata in the vector tile itself, but rather set up an onclick handler to fetch the additional metadata on demand within the MapLibre GL popup:


index.html


`
_63


// ..


_63


const popup = new maplibregl.Popup({


_63


closeButton: true,


_63


closeOnClick: false,


_63


maxWidth: 'none',


_63


})


_63


_63


function loadDetails(element, id) {


_63


element.innerHTML = 'loading...'


_63


client


_63


.from('places')


_63


.select(


_63


\`


_63


websites,


_63


socials,


_63


phones,


_63


addresses,


_63


source: sources->0->dataset


_63


\`


_63


)


_63


.eq('id', id)


_63


.single()


_63


.then(({ data, error }) => {


_63


if (error) return console.error(error)


_63


element.parentElement.innerHTML = \`<pre>${JSON.stringify(data, null, 2)}</pre>\`


_63


})


_63


}


_63


_63


map.on('click', 'overture-pois-text', async (e) => {


_63


if (e.features.length > 0) {


_63


const feature = e.features\[0\]


_63


console.log(feature)


_63


popup.setHTML(


_63


\`


_63


<table style="font-size:12px">


_63


<tr>


_63


<td>id:</td>


_63


<td>${feature.properties.id}</td>


_63


</tr>


_63


<tr>


_63


<td>name:</td>


_63


<td>${feature.properties.primary_name}</td>


_63


</tr>


_63


<tr>


_63


<td>main_category:</td>


_63


<td>${feature.properties.main_category}</td>


_63


</tr>


_63


<tr>


_63


<td>details:</td>


_63


<td>


_63


<span onclick="loadDetails(this, '${feature.properties.id}')">


_63


load details


_63


</span>


_63


</td>


_63


</tr>


_63


</table>


_63


\`


_63


)


_63


popup.setLngLat(e.lngLat)


_63


popup.addTo(map)


_63


}


_63


})


_63


// ...


`


## Conclusion#


PostGIS is incredibly powerful, allowing you to programmatically generate vector tiles from table rows stored in Postgres. Paired with Supabase's auto generated REST API and supabase-js client library you're able to build interactive geospatial applications with ease!


Want to learn more about Maps and PostGIS? Make sure to follow our[Twitter](https://x.com/supabase) and[YouTube](https://www.youtube.com/@Supabase) channels to not miss out! See you then!


## More Supabase#


- [Watch the video guide](https://supabase.link/supa-gis-yt-docs)
- [Find the code](https://github.com/bdon/supabase-vector-tile)
- [Self-host Maps on Supabase Storage with Protomaps](https://supabase.link/protomaps-storage-yt)
- [Getting started with PostGIS](https://supabase.link/postgis-quickstart-yt)
- [PostGIS docs guide](https://supabase.com/docs/guides/database/extensions/postgis)
