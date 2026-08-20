---
schema_version: "1.0.0"
document_id: "a1d3dc91846efbfc6a646554d1b303f5f29eb64a1a2454d9b0c7e83ca7b99db6"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/dynamic-graphql-polling-with-react-and-apollo-client-fb36e390d250"
published_at: "2017-12-05T23:00:12+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:34fd501b8446f51ad179d859097e96add16649e164534c55c4e1a57e850c030d"
---

# Dynamic GraphQL polling with React and Apollo Client

I recently added a feature to the admin interface on[Meteor Galaxy](https://www.meteor.com/hosting) , Meteor Development Group’s hosting platform for Meteor apps, that shows the state of our internal database migrations. I’m using[Apollo Client with react-apollo](https://www.apollographql.com/docs/react/) to build Galaxy’s UI, and I wanted the control panel to automatically update to show the state of any migrations. To do that, I had to decide how often the GraphQL query would poll our server.


[Photo credit to themtube2 on Flickr.](https://www.flickr.com/photos/59337469@N07/5432665289/in/photolist-9h4Q7V-8hxH3-6goMKZ-iCwSu9-a4EhJ1-WVAVyA-9STPDJ-Tg7gVn-7VkE2w-7EkcrR-j1kcX-pCD9wU-cCHJQN-6LV5ht-4RHW7b-6C7soE-8s4hyy-BWLN2-X79PG-9SsEgm-6cfWAX-WRgpm3-6Vjrjh-7Fyq5v-T6P94p-YNMG6b-62xk1s-8zMMiE-spFQYX-G2hMTK-DeFb4d-71jap-TThS7a-dHamd5-AXv58e-caXmuy-21RVhPE-D5brZh-3nZmYR-AnRMU-4G6Jkt-64YZ5y-oG1gRf-uPdbzh-Rc8Hg5-Zftu9n-8V9ehf-kCFz-uefEd6-gGwVn)


Now, we’re not usually running any migrations, so a nice, slow polling interval like 30 seconds seemed reasonable. But in the rare case where a migration *is* running, I wanted to be able to see much faster updates on its progress.


## Setting the poll interval from the result of a query


It turns out there’s an easy pattern with Apollo Client and react-apollo to let the poll interval be determined by the data returned by the query itself. I had to figure it out for myself, so I thought I’d share it here to save others the work! I think it’s easier to understand if I use some utilities from the great` <a href="https://github.com/acdlite/recompose/blob/master/docs/API.md" target="_blank" rel="noreferrer noopener">recompose</a>`[library](https://github.com/acdlite/recompose/blob/master/docs/API.md) , but you can implement this with the basic React API as well.


The key to this is knowing that the` <a href="https://www.apollographql.com/docs/react/basics/queries.html#options-from-props" target="_blank" rel="noreferrer noopener">options</a>`[parameter to react-apollo’s main](https://www.apollographql.com/docs/react/basics/queries.html#options-from-props)` <a href="https://www.apollographql.com/docs/react/basics/queries.html#options-from-props" target="_blank" rel="noreferrer noopener">graphql</a>`[function](https://www.apollographql.com/docs/react/basics/queries.html#options-from-props) can itself be a function that depends on its incoming React props. (The` options` parameter describes the options for the query itself, as opposed to React-specific details like what property name to use for data.) We can then use` recompose` ‘s` <a href="https://github.com/acdlite/recompose/blob/master/docs/API.md#withstate" target="_blank" rel="noreferrer noopener">withState</a>` to set the poll interval from a prop passed in to the` graphql` component, and use the` <a href="https://reactjs.org/docs/react-component.html#componentwillreceiveprops" target="_blank" rel="noreferrer noopener">componentWillReceiveProps</a>` React lifecycle event (added via the` recompose`` <a href="https://github.com/acdlite/recompose/blob/master/docs/API.md#lifecycle" target="_blank" rel="noreferrer noopener">lifecycle</a>` helper) to look at the fetched GraphQL data and adjust if necessary.


Here’s how it all fits together:


```text
import { graphql } from "react-apollo";
import gql from "graphql-tag";
import { compose, withState, lifecycle } from "recompose";


const DEFAULT_INTERVAL = 30 * 1000;
const ACTIVE_INTERVAL = 500;


const withData = compose(
// Pass down two props to the nested component: `pollInterval`,
// which defaults to our normal slow poll, and `setPollInterval`,
// which lets the nested components modify `pollInterval`.
withState("pollInterval", "setPollInterval", DEFAULT_INTERVAL),
graphql(
gql`
query getMigrationStatus {
activeMigration {
name
version
progress
}
}
`,
{
// If you think it's clear enough, you can abbreviate this as:
//   options: ({pollInterval}) => ({pollInterval}),
options: props => {
return {
pollInterval: props.pollInterval
};
}
}
),
lifecycle({
componentWillReceiveProps({
data: { loading, activeMigration },
pollInterval,
setPollInterval
}) {
if (loading) {
return;
}
if (activeMigration && pollInterval !== ACTIVE_INTERVAL) {
setPollInterval(ACTIVE_INTERVAL);
} else if (
!activeMigration &&
pollInterval !== DEFAULT_INTERVAL
) {
setPollInterval(DEFAULT_INTERVAL);
}
}
})
);
const MigrationPanelWithData = withData(MigrationPanel);
```


Note that we check the current value of` pollInterval` before changing it, because by default in React, nested components will get re-rendered any time we change state, even if you change it to the same value. You can deal with this using` <a href="https://reactjs.org/docs/react-component.html#shouldcomponentupdate" target="_blank" rel="noreferrer noopener">componentShouldUpdate</a>` or` <a href="https://reactjs.org/docs/react-api.html#reactpurecomponent" target="_blank" rel="noreferrer noopener">React.PureComponent</a>` , but in this case it’s straightforward just to only set the state when it’s actually changing.


Using this pattern successfully requires at least version 2.0.3 of` apollo-client` , as earlier versions had a[bug](https://github.com/apollographql/apollo-client/issues/2569) related to changing` pollInterval` .


I hope others find this pattern as useful as I did! This sort of feature is why I find Apollo to be the most delightful way to get data into my apps, even independently of the advantages of GraphQL itself.


Written by


David Glasser


[Read more by David Glasser](https://www.apollographql.com/blog/author/glasser)
