---
schema_version: "1.0.0"
document_id: "5467cbe92a2a51c748612dcb1fea2ce7afe624c5f3d36cc3b4a7191dd9f8a3d3"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-embed-analytics-in-a-react-app-dashboards-charts-and-ai-queries/"
published_at: "2026-03-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:18:37.089432+00:00"
content_hash: "sha256:6633d94ea84d08166769c16129b7a1cf1ac368d2866fac715820667d512277ef"
---

# How to embed analytics in a React app: dashboards, charts, and AI-powered queries

Embedded analytics is the practice of integrating dashboards, charts, and data exploration directly into your own application instead of sending users to a separate BI tool. In a React app, this means rendering interactive visualizations, filters, and even AI-powered natural language queries inside your existing UI — so users never leave your product to access their data.


According to Dresner Advisory Services’ 2025 Embedded BI Market Study, products with embedded analytics see 2–3x higher feature engagement than those that link out to external dashboards (“Embedded BI Market Study,” Dresner Advisory Services, 2025). For SaaS products, embedded analytics is increasingly table-stakes — customers expect self-serve reporting inside the products they pay for.


## TL;DR


- Embedded analytics integrates dashboards, charts, and queries directly into your React app instead of sending users to a separate BI tool.
- Four integration approaches: iframe embedding (fastest), React SDK (best for production), API + custom charts (most control), and white-label platforms (best balance).
- Security is the hardest part — row-level security, token-based authentication, and tenant isolation are non-negotiable for multi-tenant apps.
- AI-powered natural language queries can be embedded as pre-built components or custom pipelines.
- The build vs. buy decision usually tips toward a platform once you need more than 2–3 dashboards.


## What is embedded analytics?


Embedded analytics is the integration of data visualizations, reports, and interactive query capabilities directly within a software application, so users access analytics inside the product they already use instead of exporting data to a spreadsheet or switching to a standalone BI tool. There are two common contexts: customer-facing (SaaS products embedding dashboards for customers) and internal (teams embedding analytics into admin panels, CRMs, or internal tools).


- **Customer-facing (external)** : SaaS companies embed dashboards so customers analyze their own data — usage analytics in a project management tool, performance reports in an ad platform, financial summaries in a banking app.
- **Internal (operational)** : Teams embed analytics into admin panels, CRMs, or internal tools so ops, support, and finance teams monitor metrics without leaving their workflow.


## Why embed analytics instead of using a separate BI tool?


Embedded analytics eliminates context-switching, increases data engagement, and lets you control the security model through your application’s existing authentication — making it the preferred approach when the data audience is product users rather than dedicated analysts.


- **Reduced context-switching** : Users stay in the app. A support agent sees account health metrics while working a ticket, without opening a second tool.
- **Faster adoption** : Non-technical users are more likely to engage with data when it appears in a familiar interface.
- **Better data governance** : Embedded analytics inherit your application’s authentication and authorization. You control who sees what at the row level.
- **Competitive differentiation** : For SaaS products, customer-facing analytics is increasingly expected, not optional.


The trade-off is complexity. Embedding analytics means your frontend team owns the integration, your backend team owns the data pipeline, and your security model has to extend to every chart and filter.


## What are the main approaches to embedding analytics in React?


There are four primary patterns for adding analytics to a React application, each with different tradeoffs between control, implementation speed, and maintenance burden: iframe embedding, React SDK integration, API with custom visualization, and white-label headless platforms.


### 1. Iframe embedding


The simplest approach. Your BI tool generates a URL for a dashboard or chart, and you render it inside an` <iframe>` in your React component.


```text
function   EmbeddedDashboard  ({   dashboardUrl   }) {
return   (
<  iframe
src  =  {dashboardUrl}
width  =  "100%"
height  =  "600"
frameBorder  =  "0"
allowFullScreen
/>
);
}
```


**Pros** : Fast to implement, no dependency on the BI tool’s frontend SDK, works with almost any analytics platform.


**Cons** : Limited styling control, cross-origin restrictions can break interactivity, performance overhead from loading a full page inside a frame, and the UX often feels disconnected.


Iframes are fine for internal admin tools where polish matters less. For customer-facing products, they usually feel too rough.


### 2. JavaScript / React SDK


Many embedded analytics platforms provide a React SDK that renders charts and dashboards as native React components, giving far more control over layout, theming, and interactivity.


```text
import   { Dashboard }   from   '@analytics-platform/react'  ;


function   AnalyticsPage  ({   token   }) {
return   (
<  Dashboard
token  =  {token}
theme  =  "light"
filters  =  {{ team:   "engineering"   }}
onFilterChange  =  {(  filters  )   =>   console.  log  (filters)}
/>
);
}
```


**Pros** : Native look and feel, full layout and theming control, event hooks for interactivity, better performance than iframes.


**Cons** : Tighter vendor coupling, more frontend code to maintain, SDK updates can introduce breaking changes.


SDK embedding is the most common approach for production SaaS applications. It balances implementation speed with enough control to make analytics feel native.


### 3. API + custom visualization


Instead of using a vendor’s rendering layer, you fetch data from an analytics API and build your own charts using Recharts, Victory, Nivo, or D3.


```text
import   { useEffect, useState }   from   'react'  ;
import   { BarChart, Bar, XAxis, YAxis }   from   'recharts'  ;


function   RevenueChart  () {
const   [  data  ,   setData  ]   =   useState  ([]);


useEffect  (()   =>   {
fetch  (  '/api/analytics/revenue-by-month'  )
.  then  (  res   =>   res.  json  ())
.  then  (setData);
}, []);


return   (
<  BarChart   width  =  {  600  }   height  =  {  300  }   data  =  {data}>
<  XAxis   dataKey  =  "month"   />
<  YAxis   />
<  Bar   dataKey  =  "revenue"   fill  =  "#6366f1"   />
</  BarChart  >
);
}
```


**Pros** : Complete pixel-level control, no vendor lock-in on the frontend, combine data from multiple sources.


**Cons** : Significantly more engineering effort. You own all charting logic, filtering, drill-down, and export features.


### 4. White-label / headless analytics platform


Some platforms handle the data layer, caching, and query engine while letting you fully control the frontend. You get pre-built chart components that can be styled to match your product, plus a query API for custom views.


**Pros** : Fastest path to production-quality embedded analytics. Handles caching, query optimization, and access control.


**Cons** : Vendor dependency for the data layer, pricing can scale with usage.


Platforms like Basedash, Explo, and Cube each take different approaches — Basedash focuses on AI-powered querying, Explo on customer-facing dashboards, and Cube on a headless data API layer.


## Which tools support embedding analytics in React?


Eight major embedded analytics tools support React integration as of 2026, with varying approaches to SDK depth, AI capabilities, and pricing models. The comparison below covers the most relevant evaluation criteria for React developers.


Tool React SDK Iframe API AI queries Row-level security Pricing model


**Basedash** No Yes Yes Yes (NL to SQL) Yes Per-customer


**Explo** Web component Yes Yes No Yes Per-customer-logo


**Metabase** Yes Yes Yes Yes (Metabot) Yes (Pro+) Per-seat / open-source


**Looker** JS SDK Yes Yes Yes (Gemini) Yes Enterprise


**Tableau** Yes (Embedding API v3) Yes REST Yes (Pulse) Yes (with config) Per-seat


**Cube** Yes (headless) No Yes Yes (Chat API) Via data model Per-developer


**Superset** No (iframe wrapper) Yes Yes Limited Yes (with config) Open-source


**Sigma** Yes Yes Yes Yes (Ask Sigma) Yes Per-seat


Key observations:


- **AI-powered queries** are becoming a differentiator. Most major platforms now offer some form of natural language to SQL.
- **Row-level security** is table-stakes for multi-tenant applications. Every query must be scoped to the authenticated user’s data.
- **Pricing models diverge significantly.** Per-customer models (Basedash, Explo) scale better when embedding for thousands of external customers compared to per-seat models (Tableau, Sigma).


## How do you handle security and multi-tenancy?


Security is the hardest part of embedded analytics. In a multi-tenant application, you must guarantee that Customer A never sees Customer B’s data — even if both view the same dashboard template. Three mechanisms work together: row-level security at the database level, token-based authentication for embedded sessions, and architectural tenant isolation patterns.


### Row-level security (RLS)


Row-level security filters every query at the database level so users only see authorized rows. In PostgreSQL:


```text
ALTER   TABLE   orders   ENABLE   ROW   LEVEL   SECURITY  ;


CREATE   POLICY   tenant_isolation   ON   orders
USING   (tenant_id   =   current_setting(  'app.current_tenant'  )::uuid);
```


When your app sets` app.current_tenant` before each query, PostgreSQL automatically filters results regardless of how the query is constructed.


### Token-based authentication


Most embedded analytics SDKs use signed JWTs to authenticate embedded sessions. Your backend generates a token encoding the user’s identity and permissions, and the embedded component uses that token to authorize requests:


1. User logs into your React app
2. Your backend generates a signed analytics token with tenant ID, role, and data permissions
3. The React component passes this token to the embedded SDK
4. The analytics platform validates the token and applies permissions to every query


### Tenant isolation patterns


- **Shared database, shared schema** : All tenants in one table, isolated by` tenant_id` column with RLS. Simplest to manage, scales well for hundreds of tenants.
- **Shared database, separate schemas** : Each tenant gets their own PostgreSQL schema. Stronger isolation, but complex beyond a few dozen tenants.
- **Separate databases** : Each tenant gets their own database. Maximum isolation, but operationally expensive. Typically for regulated industries.


For most SaaS applications, the shared-schema approach with RLS provides the right balance.


## How do you add AI-powered queries to embedded analytics?


AI-powered analytics — specifically natural language to SQL — lets users type questions like “What was our revenue by region last quarter?” and get a chart back without writing SQL. There are two approaches for embedding this in React: using a pre-built AI query component from a platform, or building a custom pipeline.


### Pre-built AI query components


Some platforms provide a ready-made query interface. Basedash offers a natural language query bar that translates questions into SQL, runs them against your database, and renders results — all within your app’s UI, handling query parsing, error recovery, and visualization selection automatically.


### Custom AI query pipeline


If building your own:


1. **User input** : Text field in your React app captures the natural language question
2. **Schema context** : Backend sends database schema to an LLM with the question
3. **SQL generation** : LLM generates a SQL query
4. **Validation and execution** : Backend validates the query (checking for dangerous operations, applying RLS), then executes against a read replica
5. **Visualization** : Results rendered as a chart or table in the frontend


The custom approach gives full control but requires significant engineering: prompt engineering, query validation, error handling, and visualization logic. Most teams start with a platform and customize later.


## What are the performance considerations?


Embedded analytics adds load to both your frontend and your data infrastructure. The four main performance concerns are JavaScript bundle size, query latency, connection pooling, and production database isolation. Address all four before launching embedded analytics to production users.


- **Bundle size** : React SDKs can add 200KB–1MB to your bundle. Lazy-load analytics components with` React.lazy()` and` Suspense` .
- **Query latency** : Users expect sub-second dashboard response times. Use materialized views or a caching layer (Cube, or your BI platform’s built-in cache) to serve pre-computed results.
- **Connection pooling** : Each embedded session opening its own database connection exhausts your pool quickly. Use PgBouncer (for PostgreSQL) or route through a dedicated analytics database.
- **Read replicas** : Never run analytics queries against your primary production database. Use a read replica or data warehouse.


A common production architecture:


1. Production database replicates to a read replica (or Snowflake / BigQuery)
2. An analytics query engine sits in front of the replica with caching
3. The React frontend queries the analytics layer, not the database directly
4. RLS policies are enforced at the query engine or database level


## How does embedded analytics compare to building custom dashboards?


The build vs. buy decision usually tips toward a platform once you need more than 2–3 dashboards, because the engineering cost of building filtering, drill-down, export, security, and AI capabilities from scratch exceeds the platform subscription cost within weeks.


Factor Build custom Use embedded analytics platform


**Time to first dashboard** 4–8 weeks 1–3 days


**Charting capabilities** Limited to what you build Full library out of the box


**Filtering and drill-down** Build from scratch Built-in


**AI / NL queries** Major project (LLM integration, prompt engineering, validation) Included or one config toggle


**Security (RLS, auth)** Build and maintain yourself Handled by the platform


**Maintenance burden** Ongoing (every schema change, new chart type, bug fix) Vendor-managed


**Customization** Unlimited Constrained by SDK API surface


**Cost** Engineering time (expensive) Platform subscription (predictable)


Where custom builds win: highly specialized domain-specific visualizations, collaborative annotations, real-time multiplayer cursors, or deep integration with your product’s interaction model.


## Frequently asked questions


### What is the fastest way to embed analytics in a React app?


Iframe embedding is the fastest — generate a dashboard URL from your BI tool and render it in an` <iframe>` . This works in minutes but has limitations: styling is restricted, interactivity is limited, and the UX can feel disconnected. For a production-quality experience, use a platform’s SDK or API, which takes 1–3 days to integrate.


### Which embedded analytics tool is best for React?


It depends on your requirements. Metabase, Tableau, Cube, and Sigma provide native React SDKs. Basedash and Explo use iframe/API approaches that work with any frontend framework. For AI-powered natural language queries, Basedash and Cube’s Chat API are the most mature options. For maximum frontend control, Cube’s headless approach with your own chart library gives the most flexibility.


### How do I handle multi-tenancy in embedded analytics?


Use row-level security at the database level (PostgreSQL RLS policies filter by tenant ID), token-based authentication (your backend generates JWTs with tenant context that the embedded SDK validates), and the shared-schema isolation pattern (one table with a tenant_id column). Test that tenant A cannot access tenant B’s data under any query pattern.


### How much does embedded analytics cost?


Costs vary by pricing model and scale. Per-customer platforms (Basedash starting at $1,000/month plus AI usage, Explo custom pricing) are most cost-effective when embedding for many external customers. Per-seat platforms (Tableau at $75/user/month, Sigma at $300/month base) work better for internal use cases with fewer users. Open-source options (Metabase, Superset) are free but require self-hosting and engineering maintenance.


### Can I embed AI-powered queries in my React app?


Yes. Basedash provides a natural language query bar that can be embedded in your application. Cube offers a Chat API for building custom AI query interfaces. You can also build a custom pipeline using an LLM for SQL generation, but this requires significant engineering effort for prompt engineering, query validation, and error handling. Most teams start with a platform.


### What is the difference between embedded analytics and embedded BI?


The terms are used interchangeably. Both refer to integrating data visualizations, dashboards, and query capabilities directly into another application. “Embedded BI” sometimes implies a more comprehensive feature set (governed metrics, semantic layers) while “embedded analytics” can refer to simpler chart embedding.


### Do I need a data warehouse for embedded analytics?


Not necessarily. Many embedded analytics platforms connect directly to production databases (PostgreSQL, MySQL) via read replicas. A data warehouse (Snowflake, BigQuery) becomes necessary when you need to combine data from multiple sources, handle complex transformations, or serve high concurrency workloads. For most early-stage SaaS products, a direct database connection is sufficient.


### How do I lazy-load embedded analytics in React to keep bundle size small?


Use` React.lazy()` with` Suspense` to code-split your analytics components so they’re only loaded when the user navigates to an analytics page. This keeps your initial bundle small and ensures analytics SDK code doesn’t affect load times for non-analytics pages:


```text
const   AnalyticsDashboard   =   React.  lazy  (()   =>   import  (  './AnalyticsDashboard'  ));


function   App  () {
return   (
<  Suspense   fallback  =  {<  div  >Loading analytics...</  div  >}>
<  AnalyticsDashboard   />
</  Suspense  >
);
}
```
