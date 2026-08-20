---
schema_version: "1.0.0"
document_id: "f2c1a3a6934a346beae8ceccf0aef1cd16112ea3271052fbb1ab63a622a83552"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/why-we-had-to-move-away-from-react-query/"
published_at: "2022-07-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:862b90b711067f2756b2bcfa813207955047d3bbeec08a61e42fd59140d6f4d7"
---

# Why we had to move away from React Query

Last year we started using[React Query](https://react-query.tanstack.com/) for all of our API calls (and we talked about it in this article about[optimizing API calls](https://www.basedash.com/blog/optimizing-rest-api-calls) ). However, the more we were using it, the more obvious it became that React Query was not providing the ideal structure for our data. We ended up moving away from React Query, and are much happier with our new setup.


### How things work with React Query


When you make an API call with React Query using the[useQuery](https://react-query.tanstack.com/reference/useQuery) hook, the resulting data will be stored in a React Query cache. You can read from the cache using[getQueryData](https://react-query.tanstack.com/reference/QueryClient#queryclientgetquerydata) .


The benefit of this is that React Query handles all the complexity of caching and reloading data as needed, so you don’t have to do all of that manually.


The main problem with React Query appears when you need to update data in your application. React Query provides a[useMutation](https://react-query.tanstack.com/reference/useMutation) hook that you can use to make API calls that will update data on your server, and then you can update the data in the React Query cache in the` onSuccess` or` onMutate` callback functions.


The easiest way to update the data in the React Query cache is to call[invalidateQueries](https://react-query.tanstack.com/reference/QueryClient#queryclientinvalidatequeries) , where you can specify all the queries that should be refetched as a result of data having changed on the server. However, there are a few problems with having to invalidate queries like this:


- You need to make sure you’re always invalidating any query related to the data you’re updating. This is very error prone since it’s easy to forget which queries should be invalidated.
- You need to make sure that the query keys you are using in your` invalidateQueries` call line up with the query keys specified in the` useQuery` hook. This caused a lot of problems for us since some of our query keys are large objects and it was easy to forget a property in the query key object. TypeScript can help mitigate this to a certain extent, but many of the bugs we encountered were caused by this problem.
- Some API calls take a long time to complete, and so the UI would not update instantly if relying on` invalidateQueries` .


Using` invalidateQueries` is not the only way to update data in the React Query cache. You can also perform optimistic updates by surgically updating the data in the React Query cache using[setQueryData](https://react-query.tanstack.com/reference/QueryClient#queryclientsetquerydata) . Updating the data in the cache directly has the advantage of making the UI update instantly, but came with the following downsides:


- You need to know every place where the data could exist in the React Query store. The same piece of data could be fetched from different` useQuery` calls, so you would need to update each instance of that data in the store. It’s easy to forget to update data in a certain spot in the React Query cache.
- The data structure in which the data is structured in the React Query cache is not optimized to allow for updating pieces of data.


### Relying on Redux instead


After about a year of struggling with React Query, we decided to use a new (old) approach.


We opted to the use native[fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) API to make our API calls, and then stored the results in a global store using[Redux](https://redux.js.org/) . With Redux with[Redux Toolkit](https://redux-toolkit.js.org/) , we’re easily able to structure data in our store in such a way that there is a single source of truth thanks to a[normalized state structure](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape) .


As an example, we we can create a[slice](https://redux-toolkit.js.org/usage/usage-guide#creating-slices-of-state) for roles using[createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter) which has the following TypeScript types:


```text
type   RoleEntity   =   {
id  :   number  ;
createdAt  :   Date  ;
updatedAt  :   Date  ;
name  :   string  ;
workspaceId  :   number  ;


memberIds  :   number  [];
viewIds  :   number  [];
}


// Redux state for roles looks as follows:
type   ReduxState   =   {
...
roles  :   {
ids  :   number  [];
entities  :   {
[  id  :   number  ]  :   RoleEntity  ;
}
};
...
}
```


Notice how` RoleEntity` has` memberIds` and` viewIds` . These ID arrays are used to reference the` members` and` views` entities in our normalized store, rather than nesting objects, ensuring that there is only a single source of truth for our entities. As a nice side effect, this more closely matches how data is being stored in our SQL database.


Because the entities in our global store are never duplicated, we are able to update data in our store in a single place, and the UI elements that reference that piece of data will all update properly.


It’s also easy to update entities in our redux store since all entities are accessible via an` entities` object that is a mapping of entities by ID. Redux toolkit also provides a lot of[helper functions](https://redux-toolkit.js.org/api/createEntityAdapter#crud-functions) that can be used to easily update entities. Contrast that with React Query where some of the entities could be within deeply nested arrays making it quite inefficient to try to pinpoint and update those entities.


### Why Redux?


Redux wasn’t the only option we could have chosen to use for our global store. In fact, we made an effort at trying out[valtio](https://github.com/pmndrs/valtio) for a while (see the pros/cons comparison we made in the image below). However, we ultimately chose Redux since it provides better built-in tooling (via[Redux Toolkit](https://redux-toolkit.js.org/) ) for structuring our store and updating data.


Pros and cons list we made when deciding what global store solution we would like to use.


---


We are much happier with the state of our data fetching and caching in our web app now. We are running into fewer bugs with the usage of Redux, and the team is able to quickly develop in Redux with the help of Redux Toolkit.


If you’re interested in seeing the results of our newly Redux-ified app, you can sign up for Basedash and join our demo workspace. We’re building a tool that lets you view and edit your database with the ease of a spreadsheet. On top of that, you can build views of your data and share them with your teammates to give them limited read/write access to certain tables.


If you’re interested, you can sign up here:[https://charts.basedash.com/signup](https://charts.basedash.com/signup?email=)
