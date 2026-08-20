---
schema_version: "1.0.0"
document_id: "2108da245e863ba9ae487f72465b6d0ab48ff65fb6f71d860987970e90c899ad"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/real-geometry-browser-geometric-js/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T13:02:10.800608+00:00"
fetched_at: "2026-08-10T13:02:12.655554+00:00"
content_hash: "sha256:b23e47d988f20296ac0b6d579f5db95df68e993c3dd9f29a1b248690ad181fc2"
---

# Doing Real Geometry in the Browser with Geometric.js

Geometric.js is a 2D geometry library where every primitive is a plain JavaScript array: a point is` \[x, y\]` , a line is` \[\[x, y\], \[x, y\]\]` , and a polygon is an array of points — so the same value you compute with is the value you serialize to JSON and draw to SVG or Canvas.


Anyone who builds interactive charts or maps has felt the friction of needing one small geometry operation and having to reinvent the math from scratch every time. If you’ve been hand-rolling ray-casting for point-in-polygon tests or copy-pasting bounding-box math into a D3 chart,[Geometric.js](https://github.com/HarryStevens/geometric) replaces those snippets with named, tested functions that operate directly on the arrays you already have. This tour covers three headline capabilities (polygon intersection, bounding boxes, and interpolation), then two use cases where reaching for the library beats writing the trig yourself.


## Key Takeaways


- Geometric.js represents a point as` \[x, y\]` , a line as two points, and a polygon as an array of points — no classes to instantiate and nothing to adapt before drawing to SVG or Canvas.
- ` pointInPolygon(point, polygon)` returns a boolean using ray casting, giving you a one-call canvas hit-test with no external geometry math.
- ` polygonBounds(polygon)` returns` \[topLeft, bottomRight\]` as two` \[x, y\]` points, or` null` for fewer than three vertices — enough to position a tooltip box around any shape.
- The geometry-returning boolean ops —` polygonIntersection` ,` polygonUnion` ,` polygonDifference` , and` polygonXor` — require geometric v3 or later; the` polygonIntersectsPolygon` predicate has shipped since v2.
- As of version 3.x, geometric bundles its own TypeScript declarations, so you install` geometric` and skip` @types/geometric` entirely.


## Primitives are plain arrays, and that’s the whole pitch


The reason Geometric.js is pleasant to use is that it introduces no data structures. A point is an` \[x, y\]` array, a line is an array of two points, and a polygon is an array of vertices. There are no custom classes to instantiate and no special data structures to learn, which means the values you pass in are the same values you can serialize, inspect, and draw. Because the output is an array, it drops directly into D3, an SVG` points` attribute, or a Canvas` ctx.lineTo` loop with no adapter code:


```text
import   {   polygonRegular   }   from   "  geometric  "  ;


const   pentagon   =   polygonRegular  (  5  ,   10000  , [  150  ,   150  ]);
// pentagon is [[x, y], [x, y], ...] — draw it straight through:


// SVG
polygon  .  setAttribute  (  "  points  "  ,   pentagon  .  map  (  p   =>   p  .  join  (  "  ,  "  )).  join  (  "   "  ));


// Canvas
ctx  .  beginPath  ();
pentagon  .  forEach  (([  x  ,   y  ],   i  )   =>   (  i   ?   ctx  .  lineTo  (  x  ,   y  )   :   ctx  .  moveTo  (  x  ,   y  )));
ctx  .  closePath  ();
```


That round-trip (compute, serialize, render, all on the same array) is what the README means by keeping geometry simple.


## Install and import


Install` geometric` from npm, pnpm, or yarn; it ships as an ESM-first package with CommonJS support and bundled TypeScript declarations. The[latest version on npm](https://www.npmjs.com/package/geometric) is 3.0.9 (July 2026).


```text
npm   i   geometric
# or: pnpm add geometric
# or: yarn add geometric
```


```text
import   {   pointInPolygon  ,   polygonBounds   }   from   "  geometric  "  ;   // ESM
const   geometric   =   require  (  "  geometric  "  );                       // CommonJS
```


Two notes on setup. First,[TypeScript declarations are generated from the source and published with the package](https://harryjstevens.com/geometric/getting-started/) , so you should **not** install` @types/geometric` . That DefinitelyTyped package is frozen at 2.5.3 and is redundant on v3. Second, the geometry-returning boolean operations below require` geometric@^3` ; on v2 they return` undefined` .


## Polygon intersection and boolean operations


For overlap tests,[polygonIntersection(a, b)](https://harryjstevens.com/geometric/api#polygonIntersection) returns the shared area as a new polygon, while` polygonIntersectsPolygon(a, b)` returns a boolean. The predicate is the cheap check; the geometry-returning op gives you an actual shape to render.


```text
import   {   polygonIntersection  ,   polygonIntersectsPolygon   }   from   "  geometric  "  ;


const   a   =   [[  0  ,   0  ], [  4  ,   0  ], [  4  ,   4  ], [  0  ,   4  ]];
const   b   =   [[  2  ,   2  ], [  6  ,   2  ], [  6  ,   6  ], [  2  ,   6  ]];


polygonIntersectsPolygon  (  a  ,   b  );   // true — boolean predicate (v2+)
polygonIntersection  (  a  ,   b  );        // overlapping region as a new polygon (v3+)
```


The full boolean set is` polygonIntersection` ,` polygonUnion` ,` polygonDifference` , and` polygonXor` , each returning a point array. All four are v3 additions. For containment rather than overlap,` polygonInPolygon(polygonA, polygonB)` returns a boolean indicating whether the first polygon sits entirely inside the second, treating boundary points as contained.


## How do you get a polygon’s bounding box?


` polygonBounds(polygon)` returns` \[topLeft, bottomRight\]` as two` \[x, y\]` points, or` null` for fewer than three vertices, which is enough to position a tooltip or label box around any shape in one call. It ignores points with invalid values (` null` ,` undefined` ,` NaN` ,` Infinity` ), so a stray gap in your data won’t corrupt the box.


```text
import   {   polygonBounds   }   from   "  geometric  "  ;


const   region   =   [[  12  ,   8  ], [  40  ,   20  ], [  30  ,   44  ], [  6  ,   30  ]];
const   [  topLeft  ,   bottomRight  ]   =   polygonBounds  (  region  );
// topLeft = [6, 8], bottomRight = [40, 44]


const   width   =   bottomRight  [  0  ]   -   topLeft  [  0  ];     // 34
const   height   =   bottomRight  [  1  ]   -   topLeft  [  1  ];    // 36
// position a <rect> or tooltip at topLeft with this width/height
```


This is the common data-viz overlay task: given an arbitrary region on a map or chart, drop a label frame around it without recomputing min/max across every vertex by hand.


## Interpolation: animating along a path or perimeter


` lineInterpolate(line)` and` polygonInterpolate(polygon)` return an interpolator function you call with` t` in` \[0, 1\]` , which is exactly what you need to animate a marker along a path or around a perimeter. On the current 3.x build,` clamp` defaults to` true` , restricting output to the segment; passing it explicitly keeps intent obvious.


```text
import   {   lineInterpolate   }   from   "  geometric  "  ;


const   path   =   [[  0  ,   0  ], [  100  ,   50  ]];
const   at   =   lineInterpolate  (  path  ,   true  );   // clamp = true


at  (  0  );     // [0, 0]
at  (  0.5  );   // [50, 25]
at  (  1  );     // [100, 50]
```


Drive` t` from` requestAnimationFrame` or a D3 transition and you have a marker gliding along the line.` polygonInterpolate` does the same around a closed perimeter, useful for tracing an outline or moving a dot around a shape’s edge.


## How do you test if a point is inside a polygon?


To test whether a click falls inside a region,` pointInPolygon(point, polygon)` returns a boolean using ray casting; no external geometry math required. This is the classic canvas hit-test, and it collapses a dozen lines of hand-rolled edge-crossing logic into a single, tested call.


```text
import   {   pointInPolygon  ,   pointOnPolygon   }   from   "  geometric  "  ;


const   region   =   [[  0  ,   0  ], [  100  ,   0  ], [  100  ,   100  ], [  0  ,   100  ]];


pointInPolygon  ([  50  ,   50  ],   region  );    // true
pointInPolygon  ([  150  ,   50  ],   region  );   // false
```


Interior tests have one predictable weakness: clicks that land exactly on an edge. Session replays of drag-to-select and map-region pickers frequently surface this failure mode: a “why didn’t my selection register?” bug that unit tests miss but is obvious when you watch the interaction. The craft fix is` pointOnPolygon(point, polygon, epsilon)` , which tests the boundary with an optional epsilon tolerance such as` 1e-6` for on-the-line ambiguity that a raw interior check ignores.


Capability Function Returns Min version


Point-in-region test` pointInPolygon` boolean (ray casting) v2


Boundary test` pointOnPolygon` boolean (epsilon) v2


Bounding box` polygonBounds`` \[topLeft, bottomRight\]` or` null` v2


Overlap shape` polygonIntersection` polygon v3


Overlap predicate` polygonIntersectsPolygon` boolean v2


Path interpolation` lineInterpolate` function` (t) => \[x, y\]` v2


Reach for Geometric.js the moment a chart, map, or canvas UI needs more than one geometry operation: the array-in, array-out model means every result feeds the next call or the renderer directly. Install` geometric` , import the two or three functions your feature needs, and delete the Stack Overflow trig. The API reference lists the full set when a fourth operation comes up.


## FAQs


What is the difference between polygonIntersection and polygonIntersectsPolygon in Geometric.js?


polygonIntersectsPolygon(a, b) returns a boolean indicating whether two polygons overlap, while polygonIntersection(a, b) returns the shared region itself as a new polygon array you can render. Use the predicate for a cheap yes/no hit check and the geometry-returning op when you need the actual overlapping shape. The predicate has shipped since v2, but polygonIntersection requires geometric v3 or later.


Do I need to install @types/geometric to use Geometric.js with TypeScript?


No. As of version 3.x, geometric bundles its own TypeScript declarations generated from the source and published with the package, so installing geometric alone gives you editor autocomplete and type checking. The separate DefinitelyTyped package @types/geometric is frozen at 2.5.3 from November 2023 and is redundant on v3; installing it can shadow the accurate bundled types with stale ones.


What does Geometric.js return when a point lands exactly on a polygon edge?


pointInPolygon uses ray casting to test the interior, and points that land exactly on an edge are treated as ambiguous and may return false, which surfaces as clicks that fail to register in drag-to-select or map-region UIs. Use pointOnPolygon(point, polygon, epsilon) for boundary cases; the optional epsilon tolerance, such as 1e-6, controls how close to the line counts as on it.


Can Geometric.js output be drawn directly to SVG or Canvas without conversion?


Yes. Because every primitive is a plain JavaScript array rather than a class instance, the arrays returned by functions like polygonRegular or polygonIntersection drop straight into an SVG points attribute or a Canvas ctx.lineTo loop with no adapter code. The same array you compute with is the value you serialize to JSON, pass to D3, or render, which is the library's core design pitch.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
