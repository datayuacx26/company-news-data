---
schema_version: "1.0.0"
document_id: "cd401411d072711bf789a358477d3ea268f979778335852b3440a34bf97aa79c"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/building-data-grid-tanstack-table/"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-18T18:37:30.227787+00:00"
fetched_at: "2026-08-18T18:37:31.619257+00:00"
content_hash: "sha256:d5c5fabc193590588fc68a3d685f671a9f5b3a0b836bc6c8cc017f21c20849e5"
---

# Building a Data Grid with TanStack Table

TanStack Table is a headless library: it owns the table logic (sorting, filtering, pagination, and row models) while you own 100% of the markup and styling.


If you have ever spent an afternoon fighting a prebuilt grid’s stylesheet just to nudge one border into line, that trade will make immediate sense. You get a battle-tested state engine and a blank canvas for the DOM, which is exactly what you want when the design system is yours.


This guide wires up a real data grid on the current stable release,[@tanstack/react-table v8](https://www.npmjs.com/package/@tanstack/react-table) : a single table that sorts, globally filters, and paginates. Then the two scaling concerns that actually bite in production: the memoization bug that causes infinite re-renders, and virtualization for thousands of rows.


## Key Takeaways


- In TanStack Table v8, render every header and cell with` flexRender` :` flexRender(header.column.columnDef.header, header.getContext())` for headers and` flexRender(cell.column.columnDef.cell, cell.getContext())` for cells. The v7-era` cell.render('Cell')` and` column.render('Header')` calls no longer exist.
- Sorting, filtering, and pagination are opt-in row models: pass` getSortedRowModel()` ,` getFilteredRowModel()` , and` getPaginationRowModel()` into one` useReactTable` call and the order no longer matters.
- The single most common TanStack Table bug is an infinite re-render loop from passing a fresh` data` or` columns` array every render. Wrap both in` useMemo` or define them at module scope.
- Past a few thousand client-side rows, stop paginating and virtualize with` @tanstack/react-virtual` ’s` useVirtualizer` over` table.getRowModel().rows` .
- Stable is v8, currently 8.21.3 on npm’s` latest` tag. v9 entered beta in June 2026, so keep production tables on v8.


## What “headless” means and why it matters


Headless means the library ships behavior, not UI. TanStack Table computes row models and exposes state and handlers, but it puts nothing in the DOM:[v8 stopped shipping default styles and role attributes](https://tanstack.com/table/v8/docs/guide/migrating) so the core could stay framework-agnostic. The upside is total control. The responsibility is that you own semantics too, so` <table>` ,` <th scope>` , and any ARIA roles are on you.


Practically, this means styling is bring-your-own: Tailwind utility classes, CSS Modules, or styled components on your own` <table>` markup. The library never touches it.


## Set up a basic TanStack Table grid


Install the package and build the table with the` useReactTable` hook. Define columns with` createColumnHelper` for type inference, pass your` data` and` columns` , and register` getCoreRowModel()` , the base row model that maps your data into rows.


```text
npm   install   @tanstack/react-table
```


The adapter supports[every React version from 16.8 through 19](https://tanstack.com/table/v8/docs/installation) , with the caveat that it may not behave under the React Compiler that ships alongside React 19.


```text
import   {
createColumnHelper  ,
useReactTable  ,
getCoreRowModel  ,
flexRender  ,
}   from   '  @tanstack/react-table  '


type   User   =   {   firstName  :   string  ;   lastName  :   string  ;   age  :   number   }


const   columnHelper   =   createColumnHelper  <  User  >()


const   columns   =   [
columnHelper  .  accessor  (  '  firstName  '  , {   header  :   '  First Name  '   }),
columnHelper  .  accessor  (  '  lastName  '  , {   header  :   '  Last Name  '   }),
columnHelper  .  accessor  (  '  age  '  , {   header  :   '  Age  '   }),
]
```


Render by walking` getHeaderGroups()` ,` getRowModel().rows` , and each row’s` getVisibleCells()` . Pass the column def and its context into` flexRender` so string headers, JSX, and component cells all resolve correctly. Per the v8 migration guide,` flexRender` replaces the removed` cell.render('Cell')` /` column.render('Header')` calls.


```text
function   DataGrid  ({   data   }  :   { data  :   User  [] }) {
const   table   =   useReactTable  ({
data  ,
columns  ,
getCoreRowModel  :   getCoreRowModel  (),
})


return   (
<  table  >
<  thead  >
{table  .  getHeaderGroups  ().  map  (  hg   =>   (
<  tr   key  =  {hg  .  id}>
{hg  .  headers  .  map  (  header   =>   (
<  th   key  =  {header  .  id}>
{header  .  isPlaceholder
?   null
:   flexRender  (  header  .  column  .  columnDef  .  header  ,   header  .  getContext  ())  }
</  th  >
))  }
</  tr  >
))  }
</  thead  >
<  tbody  >
{table  .  getRowModel  ().  rows  .  map  (  row   =>   (
<  tr   key  =  {row  .  id}>
{row  .  getVisibleCells  ().  map  (  cell   =>   (
<  td   key  =  {cell  .  id}>
{  flexRender  (  cell  .  column  .  columnDef  .  cell  ,   cell  .  getContext  ())  }
</  td  >
))  }
</  tr  >
))  }
</  tbody  >
</  table  >
)
}
```


## Add sorting


Enable sorting by adding` getSortedRowModel()` , holding` sorting` state with` onSortingChange` , and wiring each header to` header.column.getToggleSortingHandler()` . Use` header.column.getIsSorted()` (which returns` 'asc'` ,` 'desc'` , or` false` ) to draw the direction indicator.


```text
const   [  sorting  ,   setSorting  ]   =   useState  <  SortingState  >([])


const   table   =   useReactTable  ({
data  ,
columns  ,
state  :   {   sorting   },
onSortingChange  :   setSorting  ,
getCoreRowModel  :   getCoreRowModel  (),
getSortedRowModel  :   getSortedRowModel  (),
})
```


```text
<  th   onClick  =  {header  .  column  .  getToggleSortingHandler  ()  }   style  =  {  {   cursor  :   '  pointer  '   }  }>
{  flexRender  (  header  .  column  .  columnDef  .  header  ,   header  .  getContext  ())  }
{  {   asc  :   '   ↑  '  ,   desc  :   '   ↓  '   }[  header  .  column  .  getIsSorted  ()   as   string  ]   ??   ''  }
</  th  >
```


## Filtering and global search


Global search is one piece of state: add` getFilteredRowModel()` , keep a` globalFilter` string in` state` , and update it with` onGlobalFilterChange` . The default global filter matches substrings across all columns.


```text
const   [  globalFilter  ,   setGlobalFilter  ]   =   useState  (  ''  )


const   table   =   useReactTable  ({
data  ,
columns  ,
state  :   {   sorting  ,   globalFilter   },
onSortingChange  :   setSorting  ,
onGlobalFilterChange  :   setGlobalFilter  ,
getCoreRowModel  :   getCoreRowModel  (),
getSortedRowModel  :   getSortedRowModel  (),
getFilteredRowModel  :   getFilteredRowModel  (),
})


// <input value={globalFilter} onChange={e => setGlobalFilter(e.target.value)} />
```


Per-column filters use the same row model, driven by` column.getFilterValue()` and` column.setFilterValue()` from a header input. Fuzzy matching is available but requires the separate` @tanstack/match-sorter-utils` helper as a custom` filterFn` . Reach for it only when substring matching isn’t enough.


## Pagination


Add` getPaginationRowModel()` and drive it with` pageIndex` /` pageSize` state, then gate your Next/Previous buttons with` table.getCanNextPage()` and` table.getCanPreviousPage()` . Row-model registration order doesn’t matter in v8, so these compose freely alongside sorting and filtering in the same hook call.


```text
const   [  pagination  ,   setPagination  ]   =   useState  ({   pageIndex  :   0  ,   pageSize  :   10   })


const   table   =   useReactTable  ({
data  ,
columns  ,
state  :   {   sorting  ,   globalFilter  ,   pagination   },
onSortingChange  :   setSorting  ,
onGlobalFilterChange  :   setGlobalFilter  ,
onPaginationChange  :   setPagination  ,
getCoreRowModel  :   getCoreRowModel  (),
getSortedRowModel  :   getSortedRowModel  (),
getFilteredRowModel  :   getFilteredRowModel  (),
getPaginationRowModel  :   getPaginationRowModel  (),
})
```


```text
<  button   onClick  =  {  ()   =>   table  .  previousPage  ()  }   disabled  =  {  !  table  .  getCanPreviousPage  ()  }>
Previous
</  button  >
<  button   onClick  =  {  ()   =>   table  .  nextPage  ()  }   disabled  =  {  !  table  .  getCanNextPage  ()  }>
Next
</  button  >
```


That is a complete, working grid: one` useReactTable` call that sorts, filters, and paginates together.


## Scaling: the memoization gotcha, virtualization, and custom cells


The single most common TanStack Table bug is an infinite re-render loop caused by passing a fresh` data` or` columns` array on every render. Declaring` const columns = \[...\]` or` const data = \[...\]` inside the component body creates a new reference each pass; TanStack Table sees the identity change, recomputes row models, triggers a re-render, and the cycle repeats. Wrap both in` useMemo` (or define them at module scope, as in the earlier examples) so their references stay stable.


```text
const   columns   =   useMemo  (()   =>   [  /* ... */  ], [])
const   data   =   useMemo  (()   =>   fetchedRows  , [  fetchedRows  ])
```


**Paginate or virtualize?** Paginate when page sizes are bounded or data is fetched per-page from a server. Virtualize when you hold thousands of client-side rows in one continuous scroll view. For that case, render only the visible rows with[@tanstack/react-virtual](https://www.npmjs.com/package/@tanstack/react-virtual) and its` useVirtualizer` hook over` table.getRowModel().rows` . Note that the current v3 hook is` useVirtualizer` , not the old` useVirtual` .


```text
import   {   useVirtualizer   }   from   '  @tanstack/react-virtual  '


const   rows   =   table  .  getRowModel  ().  rows
const   parentRef   =   useRef  <  HTMLDivElement  >(  null  )


const   rowVirtualizer   =   useVirtualizer  ({
count  :   rows  .  length  ,
getScrollElement  :   ()   =>   parentRef  .  current  ,
estimateSize  :   ()   =>   40  ,
overscan  :   10  ,
})
```


You then render` rowVirtualizer.getVirtualItems()` inside a spacer sized by` getTotalSize()` , as the[TanStack Virtual docs](https://tanstack.com/virtual/latest/docs/introduction) show. This is the failure mode session replay is good at exposing: an unvirtualized 10k-row grid feels fine on a seed dataset locally, but replaying a real user sorting or fast-scrolling it in production is where dropped frames and scroll jank surface.


Custom cells are just the column’s` cell` renderer: return a status badge, a formatted date, or an editable input, and the library stays out of your way because it never renders UI itself. Column pinning, resizing, and reordering are also supported through their own state APIs.


## Wrapping up


You now have a grid that sorts, filters, and paginates in a single` useReactTable` call, plus the two moves that keep it fast: stable` data` /` columns` references and virtualization past a few thousand rows. Build against v8 today, since[v9 has been in beta since June 2026](https://tanstack.com/blog/tanstack-table-v9-taking-form) , and reach for` useVirtualizer` the moment your row count outgrows a page.


## FAQs


Why is my TanStack Table re-rendering infinitely?


An infinite re-render loop almost always comes from passing a fresh data or columns array on every render. Declaring const columns = \[\] or const data = \[\] inside the component body creates a new reference each pass, so TanStack Table sees the identity change, recomputes its row models, and triggers another render. Wrap both in useMemo or define them at module scope so their references stay stable.


Should I use TanStack Table v8 or v9?


Use v8 for production. Stable is v8 (8.21.3 on npm's latest tag), while v9 has been in beta since June 2026 and still ships only as prerelease builds under npm's beta tag. v8 works with React 16.8, 17, 18, and 19. Keep production tables on v8 and only experiment with v9 in non-critical code until it reaches a stable release.


When should I paginate versus virtualize a TanStack Table?


Paginate when page sizes are bounded or data is fetched per-page from a server, using getPaginationRowModel and pageIndex/pageSize state. Virtualize when you hold thousands of client-side rows in one continuous scroll view: render only visible rows with @tanstack/react-virtual's useVirtualizer over table.getRowModel().rows. The two are not mutually exclusive, but past a few thousand client-side rows in a single scroll area, virtualization is the correct move.


Does TanStack Table provide any default styling or accessibility roles?


No. TanStack Table v8 is fully headless. It produces no markup, no styling, and no role attributes; the v8 rewrite dropped all of that so the core could work with any framework. You own the entire DOM, so semantic markup like table, th with scope, and any ARIA roles are your responsibility. Style it with Tailwind, CSS Modules, or styled components on your own markup.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
