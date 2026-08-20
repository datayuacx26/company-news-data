---
schema_version: "1.0.0"
document_id: "cf1a6697f26fd103000fa9f2361fe2110a3c2ccb35869289581eac8aefa8a748"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/quicksort-javascript-examples/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T08:38:37.747115+00:00"
fetched_at: "2026-08-19T08:38:39.866328+00:00"
content_hash: "sha256:d152506ddd96ea6dc78b3316accd362da6a87c5f5d16a340fade5789ab753846"
---

# Understanding Quicksort with JavaScript Examples

Quicksort is a divide-and-conquer sorting algorithm: it picks a pivot element, partitions the array so smaller elements go left and larger go right, then recursively sorts each side in place.


Most of us meet it for the first time at a whiteboard with someone watching, and the partition loop is where the confidence tends to drain away. It runs in **O(n log n)** time on average, sorts without allocating a second array, and is the algorithm most interviewers reach for when they ask you to “sort this by hand.”


This article covers three things: a readable version to build intuition, a hand-traced partition pass so you *see* elements move, and the in-place implementation you would actually write in an interview, plus notes on complexity, pivot choice, and stability.


## Key Takeaways


- Quicksort picks a pivot, partitions the array so smaller elements go left and larger go right, then recurses on each side in place.
- Quicksort runs in O(n log n) time on average, degrades to O(n²) in the worst case, and uses O(log n) extra space for the recursion stack when done in place.
- Choosing the first or last element as the pivot triggers the O(n²) worst case on already-sorted input, the most common quicksort pitfall. Fix it with the middle element, median-of-three, or a random pivot.
- Quicksort is not a stable sort: equal elements can be reordered relative to their original positions.
- Although the quicksort algorithm itself is unstable, JavaScript’s built-in` Array.prototype.sort` is guaranteed stable since ES2019, and V8 implements it with Timsort, not quicksort.


## How does the quicksort algorithm work?


Quicksort sorts by repeatedly splitting the array around a chosen **pivot** . One partition pass rearranges elements so everything less than the pivot sits before it and everything greater sits after it; the pivot is then in its final sorted position. Apply the same step to the left and right sub-ranges, and the whole array sorts itself.


It shares the divide-and-conquer shape of merge sort, but the trade-offs differ. Both quicksort and merge sort average O(n log n), but quicksort sorts in place with O(log n) auxiliary space while merge sort needs O(n) extra space, and merge sort is stable where quicksort is not. Quicksort trades that guaranteed stability for lower memory overhead and strong real-world speed.


## The readable version first (filter and spread)


The easiest quicksort to read partitions with` filter` and rebuilds the array with the spread operator. It’s the fastest way to grasp the recursion, and it’s a fine teaching tool, but it allocates new arrays on every call, so it is not truly in-place and uses extra memory.


```text
function   quickSort  (  arr  ) {
if (  arr  .  length   <=   1  )   return   arr  ;


const   [  pivot  ,   ...  rest  ]   =   arr  ;
const   left   =   rest  .  filter  ((  x  )   =>   x   <   pivot  );
const   right   =   rest  .  filter  ((  x  )   =>   x   >=   pivot  );


return   [  ...  quickSort  (  left  ),   pivot  ,   ...  quickSort  (  right  )];
}


quickSort  ([  3  ,   7  ,   2  ,   5  ,   1  ,   4  ,   6  ,   8  ]);   // [1, 2, 3, 4, 5, 6, 7, 8]
```


The base case (` length <= 1` ) stops the recursion, since a zero- or one-element array is already sorted. Every call builds three fresh arrays, so this version’s memory use grows with the input rather than staying constant. Reach for it to explain the idea; reach for the in-place version below when memory or interview expectations matter.


## How does partitioning work?


Partitioning is the engine of quicksort, so it pays to watch one pass closely. The Lomuto scheme takes the last element as the pivot, walks a scanning index` j` across the range, and keeps a boundary index` i` marking where the next “smaller than pivot” element belongs. Each time` arr\[j\]` is less than the pivot, it swaps` arr\[i\]` and` arr\[j\]` and advances` i` . At the end it swaps the pivot into position` i` .


Trace` \[7, 2, 1, 8, 6, 3, 5, 4\]` with pivot` 4` (the last element), starting with` i = 0` :


` j`` arr\[j\]`` arr\[j\] < 4` ? Action Array after` i`


0 7 no none` \[7,2,1,8,6,3,5,4\]` 0


1 2 yes swap` i` ,` j`` \[2,7,1,8,6,3,5,4\]` 1


2 1 yes swap` i` ,` j`` \[2,1,7,8,6,3,5,4\]` 2


3 8 no none` \[2,1,7,8,6,3,5,4\]` 2


4 6 no none` \[2,1,7,8,6,3,5,4\]` 2


5 3 yes swap` i` ,` j`` \[2,1,3,8,6,7,5,4\]` 3


6 5 no none` \[2,1,3,8,6,7,5,4\]` 3


end n/a n/a swap pivot into` i`` \[2,1,3,4,6,7,5,8\]` pivot at 3


The pivot` 4` lands at index 3, with` \[2,1,3\]` to its left and` \[6,7,5,8\]` to its right. Neither side is sorted yet, but the pivot is permanently placed, and the two sides are now independent subproblems.


```text
function   partition  (  arr  ,   lo  ,   hi  ) {
const   pivot   =   arr  [  hi  ];            // last element as pivot
let   i   =   lo  ;                       // boundary for elements < pivot
for (  let   j   =   lo  ;   j   <   hi  ;   j  ++  ) {
if (  arr  [  j  ]   <   pivot  ) {
[  arr  [  i  ],   arr  [  j  ]]   =   [  arr  [  j  ],   arr  [  i  ]];
i  ++  ;
}
}
[  arr  [  i  ],   arr  [  hi  ]]   =   [  arr  [  hi  ],   arr  [  i  ]];   // move pivot into place
return   i  ;
}
```


## The in-place quicksort you’d write in an interview


The production-shaped quicksort keeps the partition helper above and recurses over index ranges (` lo` ,` hi` ) instead of building new arrays. This is the version to reach for when someone asks you to implement quicksort: it mutates one array and uses only the recursion stack for extra space.


```text
function   quickSort  (  arr  ,   lo   =   0  ,   hi   =   arr  .  length   -   1  ) {
if (  lo   <   hi  ) {
const   p   =   partition  (  arr  ,   lo  ,   hi  );
quickSort  (  arr  ,   lo  ,   p   -   1  );
quickSort  (  arr  ,   p   +   1  ,   hi  );
}
return   arr  ;
}


quickSort  ([  7  ,   2  ,   1  ,   8  ,   6  ,   3  ,   5  ,   4  ]);   // [1, 2, 3, 4, 5, 6, 7, 8]
```


Each call partitions its range, then recurses on the two sub-ranges around the pivot. The` lo < hi` guard is the base case: a range of zero or one element is already sorted. If recursion is off the table (a common interview follow-up), the same logic converts to an iterative version by pushing` lo` /` hi` pairs onto an explicit stack instead of the call stack.


## Complexity, pivot choice, and stability


Quicksort’s cost is decided almost entirely by the pivot. With balanced splits, each level of recursion touches every element once across roughly` log n` levels, giving O(n log n). When splits are consistently lopsided, the recursion depth grows to` n` and cost degrades to O(n²). In place, the recursion stack accounts for O(log n) space on balanced input.


The classic trap: **choosing the first or last element as the pivot makes quicksort hit its O(n²) worst case on already-sorted input** , because every partition peels off just one element. You avoid the sorted-input worst case by choosing the middle element, using median-of-three, or picking a random pivot. Median-of-three orders the first, middle, and last elements and uses the median, which resists both sorted and reverse-sorted adversarial inputs:


```text
function   medianOfThree  (  arr  ,   lo  ,   hi  ) {
const   mid   =   Math  .  floor  ((  lo   +   hi  )   /   2  );
if (  arr  [  mid  ]   <   arr  [  lo  ]) [  arr  [  lo  ],   arr  [  mid  ]]   =   [  arr  [  mid  ],   arr  [  lo  ]];
if (  arr  [  hi  ]    <   arr  [  lo  ]) [  arr  [  lo  ],   arr  [  hi  ]]    =   [  arr  [  hi  ],   arr  [  lo  ]];
if (  arr  [  hi  ]    <   arr  [  mid  ]) [  arr  [  mid  ],   arr  [  hi  ]]   =   [  arr  [  hi  ],   arr  [  mid  ]];
// median now sits at mid; move it to hi so Lomuto uses it as the pivot
[  arr  [  mid  ],   arr  [  hi  ]]   =   [  arr  [  hi  ],   arr  [  mid  ]];
return   arr  [  hi  ];
}
```


One property no pivot strategy buys back: **quicksort is not a stable sort, so equal elements can be reordered relative to their original positions.** That matters when you sort records by a secondary key and expect the primary ordering to survive.


Property Quicksort Merge sort


Average time O(n log n) O(n log n)


Worst-case time O(n²) O(n log n)


Extra space O(log n) (in place) O(n)


Stable? No Yes


In place? Yes No


## Is JavaScript’s built-in sort quicksort?


No. Although the quicksort algorithm is unstable, JavaScript’s built-in[Array.prototype.sort](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) has been guaranteed stable since[ES2019, the tenth edition of the language standard](https://262.ecma-international.org/10.0/) . V8 has sorted arrays with[Timsort](https://v8.dev/blog/array-sort) since v7.0 and Chrome 70: a merge sort that takes advantage of runs of already-ordered data and leaves equal elements in the order it found them. Every other major engine is held to the same stability requirement by the spec. So “quicksort is unstable” describes the algorithm, not the built-in` sort` in any modern browser.


Quicksort earns its reputation from a tight partition loop, in-place operation, and O(n log n) average speed, as long as you keep the pivot away from the array’s ends. Implement the in-place version above with a median-of-three or random pivot, run it against a sorted input to confirm it doesn’t blow up, and you’ll be able to both write quicksort and explain its trade-offs on demand.


## FAQs


When should I use quicksort instead of merge sort?


Use quicksort when memory is constrained and you want to sort in place, since it needs only O(log n) auxiliary space for the recursion stack versus merge sort's O(n) extra array. Both average O(n log n), but quicksort is faster in practice on typical data. Choose merge sort when you need guaranteed stability or a guaranteed O(n log n) worst case, since quicksort can degrade to O(n squared).


Why does quicksort hit O(n squared) on an already-sorted array?


A fixed first-or-last pivot degrades to O(n squared) on sorted input because every partition places the pivot at one end and produces one empty sub-range and one range of n minus 1 elements. That yields n levels of recursion instead of log n, each doing linear work. The fix is picking the middle element, using median-of-three, or choosing a random pivot, all of which restore balanced splits on sorted data.


Is JavaScript's Array.prototype.sort implemented with quicksort?


No. Modern engines do not use quicksort for the built-in sort. V8 has used Timsort since v7.0 and Chrome 70, a merge sort that exploits runs of already-ordered data and preserves the order of equal elements. Since ES2019, the ECMAScript specification requires Array.prototype.sort to be stable, and every major engine ships a stable sort. So the algorithm quicksort is unstable, but the built-in sort is not quicksort and is guaranteed stable.


What's the difference between the Lomuto and Hoare partition schemes?


Lomuto uses a single scanning index and typically takes the last element as pivot, swapping smaller elements toward a boundary index; it is simpler to write and trace. Hoare uses two pointers moving inward from both ends and generally performs fewer swaps, making it faster in practice. Both partition in place and return a split point, but Hoare's returned index does not place the pivot in its final position the way Lomuto's does.


How do I convert recursive quicksort to an iterative version?


Replace the call stack with an explicit stack of index ranges. Push the initial lo and hi pair, then loop while the stack is non-empty: pop a range, partition it to get a pivot index p, and push the two sub-ranges lo to p minus 1 and p plus 1 to hi back onto the stack when they contain more than one element. This produces the same result while avoiding recursion, a common interview follow-up.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
