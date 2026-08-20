---
schema_version: "1.0.0"
document_id: "3c6a83bb6d4487ac9f77a8a5d6f6346b42f0cc1b0b6c4150050dc49cc19ae5cf"
company_key: "yc-dialect"
company: "Dialect"
source_id: "yc-dialect-news-import-4cb56bc42b2e"
canonical_url: "https://www.dialect.to/blog/introducing-the-blinks-client"
published_at: "2025-07-09T12:41:00+00:00"
first_seen_at: "2026-07-21T16:13:21.108809+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:27c5c594bebd7812360dbafd3541eba2d0f96adf965ae566d5c152d74355d66f"
---

# Introducing the Blinks Client

Just 2 weeks ago, we launched[Solana Actions and blockchain links (blinks)](https://x.com/saydialect/status/1805588148212424901) in collaboration with[Solana Foundation](https://x.com/solana/status/1805587979723063440) . Since then, engagement has exploded. Hundreds of developers are now building blinks on Solana, and more than a dozen wallets are now integrating blinks into their Chrome extensions and mobile apps.


Today, blinks are usable on X via supporting wallet Chrome extensions — such as Phantom, Backpack, and OKX — and on dial.to our interstitial “popup” site.


Our mission with blinks, however, is to unfurl the internet and make product experiences sharable everywhere.


So today, we are introducing a major upgrade to our Blinks Client SDK. Any developer can now use this SDK to bring blinks to their Chrome extensions, websites, and other apps, *and* customize the experience to perfectly fit their brand.


Everywhere you can share a link, you should be able to share a blink.


In addition, we will be adding out-of-the-box support for new sites beyond X, including Reddit, Twitch, and others, in addition to wallets or other extensions looking for more coverage.


You can find our Blinks SDK at[https://github.com/dialectlabs/blinks](https://github.com/dialectlabs/blinks) .


Want to get started? Here is a step-by-step tutorial on:


1. Integrating blinks natively into your web3 dApp.


2. Adding support for more sites, like Reddit, Twitch, LinkedIn, & others via Chrome Extension.


Both of these use cases are supported out of the box with our Blinks SDK.


The rest of this article assumes you have built your own Action API and deployed it. If you haven’t yet, check out our[docs](https://docs.dialect.to/documentation/actions/actions) how to get started.


> *Note: The code samples in this guide are from the SDK as of the day of publication. In the case you find the samples are outdated, visit our[guide](https://docs.dialect.to/documentation/actions/guide-integrate-blinks-into-your-client) in our docs.*


#### Guide: Integrate Blinks natively into your dapp


You can now integrate blinks into your React app using Dialect’s Blinks SDK:[https://github.com/dialectlabs/blinks](https://github.com/dialectlabs/blinks) .


In this example, we’ll be using NextJS and TypeScript. If you want a more general look into this, check out the[guide in our docs](https://docs.dialect.to/documentation/actions/guide-integrate-blinks-into-your-client) .


> *# npm npm i @dialectlabs/blinks*


> *#yarn yarn add @dialectlabs/blinks*


Once you’ve installed these packages in the root of your application, make sure you’ve also installed the required wallet adapter packages and components and wrapped the body of the webpage with the adapter components in *layout.tsx.* Then, either in *page.tsx* or in a new component, you can include the logic for rendering the Blink UI.


For rendering a single blink, the logic would looks like this:


> *import'@dialectlabs/blinks/index.css'; import { useState, useEffect } from'react'; import { Action, Blink, ActionsRegistry } from"@dialectlabs/blinks"; import { useAction } from'@dialectlabs/blinks/react';*


> *// needs to be wrapped with <WalletProvider /> and <WalletModalProvider /> constApp = () => { const \[action, setAction\] = useState<Action | null>(null); const actionApiUrl = '...'; // useAction initiates registry, adapter and fetches the action. const { action } = useAction(actionApiUrl, { rpcUrlOrConnection: <YOUR_CONNECTION_OR_RPC> }); return action ? <Blink action={action} websiteText={new URL(actionApiUrl).hostname} /> : null; }*


Or if you want to render multiple blinks, the logic looks like this:


> *import'@dialectlabs/blinks/index.css'; import { useState, useEffect } from'react'; import { Action, Blink, ActionsRegistry, type ActionAdapter } from"@dialectlabs/blinks"; import { useAction, useActionsRegistryInterval } from'@dialectlabs/blinks/react';*


> *// needs to be wrapped with <WalletProvider /> and <WalletModalProvider /> constApp = () => { // SHOULD be the only instance running (since it's launching an interval) const { isRegistryLoaded } = useActionsRegistryInterval(); const { adapter } = useActionAdapter(<YOUR_RPC_URL_OR_CONNECTION>); return isRegistryLoaded ? <ManyActions adapter={adapter} /> : null; } constManyActions = ({ adapter }: { adapter: ActionAdapter }) => { const apiUrls = useMemo(() => (\['...', '...', '...'\]), \[\]); // Your Action APIs go here const \[actions, setActions\] = useState<Action\[\]>(\[\]); useEffect(() => { constfetchActions = async () => { const promises = apiUrls.map(url =>Action.fetch(url).catch(() =>null)); const actions = awaitPromise.all(promises); setActions(actions.filter(Boolean) asAction\[\]); } fetchActions(); }, \[apiUrls\]); // we need to update the adapter every time, since it's dependent on wallet and walletModal states useEffect(() => { actions.forEach((action) => action.setAdapter(adapter)); }, \[actions, adapter\]); return actions.map(action => ( <div key={action.url} className="flex gap-2"> <Blink action={action} websiteText={new URL(action.url).hostname} /> </div> ); }*


And then the blink renders on your webpage similar to this. For example, this is how blinks are rendered on Dialect’s interstitial “popup” site, dial.to.


Blinks can be brought to any kind of app, including new web3 social dApps, or even traditional websites that want to start adding web3 capabilities, such as The Block, who is using Access Protocol subscriptions for access to premium content.


The new Blinks SDK supports full customization of blink styles, so you can get blinks fully on brand with your product.


To customize the style of your blinks, add a *stylePreset* prop to the *<Blink />* component like this:


> *import { Blink } from'@dialectlabs/blinks'; ... return <Blink stylePreset="x-dark" ... />;*


> *// The available presets are mapped to CSS classes like // x-dark -> .x-dark // x-light -> .x-light // default -> .dial-light // custom -> .custom // Note that .custom does not contain any colors, radii or shadow*


You can also customize each preset by changing the CSS variables used for a specific preset as shown here.


> *.blink.x-dark { --blink-bg-primary: #202327; --blink-button: #1d9bf0; --blink-button-disabled: #2f3336; --blink-button-hover: #3087da; --blink-button-success: #00ae661a; --blink-icon-error: #ff6565; --blink-icon-error-hover: #ff7a7a; --blink-icon-primary: #6e767d; --blink-icon-primary-hover: #949ca4; --blink-icon-warning: #ffb545; --blink-icon-warning-hover: #ffc875; --blink-input-bg: #202327; --blink-input-stroke: #3d4144; --blink-input-stroke-disabled: #2f3336; --blink-input-stroke-error: #ff6565; --blink-input-stroke-hover: #6e767d; --blink-input-stroke-selected: #1d9bf0; --blink-stroke-error: #ff6565; --blink-stroke-primary: #1d9bf0; --blink-stroke-secondary: #3d4144; --blink-stroke-warning: #ffb545; --blink-text-brand: #35aeff; --blink-text-button: #ffffff; --blink-text-button-disabled: #768088; --blink-text-button-success: #12dc88; --blink-text-error: #ff6565; --blink-text-error-hover: #ff7a7a; --blink-text-input: #ffffff; --blink-text-input-disabled: #566470; --blink-text-input-placeholder: #6e767d; --blink-text-link: #6e767d; --blink-text-link-hover: #949ca4; --blink-text-primary: #ffffff; --blink-text-secondary: #949ca4; --blink-text-success: #12dc88; --blink-text-warning: #ffb545; --blink-text-warning-hover: #ffc875; --blink-transparent-error: #aa00001a; --blink-transparent-grey: #6e767d1a; --blink-transparent-warning: #a966001a; --blink-border-radius-rounded-lg: 0.25rem; --blink-border-radius-rounded-xl: 0.5rem; --blink-border-radius-rounded-2xl: 1.125rem; --blink-border-radius-rounded-button: 624.9375rem; --blink-border-radius-rounded-input: 624.9375rem; /* box-shadow */ --blink-shadow-container: 0px 2px 8px 0px rgba(59, 176, 255, 0.22), 0px 1px 48px 0px rgba(29, 155, 240, 0.24); }*


#### ** Guide: Adding Blinks to 3rd Party Sites via Your Chrome Extension


If you’re a Chrome extension developer who wants to add blinks to third party sites like X, our Blinks SDK can get you up and running in just minutes.


For support on X, add the following logic to your extension script:


> *// contentScript.tsimport { setupTwitterObserver } from "@dialectlabs/blinks/ext/twitter";import { ActionConfig, ActionContext } from "@dialectlabs/blinks";*


> *// your RPC_URL is used to create a connection to confirm the transaction after action executionsetupTwitterObserver(new ActionConfig(RPC_URL, { signTransaction: async (tx: string, context: ActionContext) => { ... }, connect: async (context: ActionContext) => { ... }}))// orimport { type ActionAdapter } from "@dialectlabs/blinks";class MyActionAdapter implements ActionAdapter { async signTransaction(tx: string, context: ActionContext) { ... } async connect(context: ActionContext) { ... } async confirmTransaction(sig: string, context: ActionContext) { ... }}setupTwitterObserver(new MyActionAdapter());*


And that’s it. Your extension is now rendering blinks on X.


#### Blinks support should be opt-in (& discoverable)


If you’re rendering blinks on third party sites via your Chrome extension, blinks should be opt in by default. We recommend adding controls from your extension settings to give users the ability to enable or disable blinks.


That said, this new feature should be discoverable. Consider adding promotional material in your extension highlighting this new behavior.


#### Beyond X


The Chrome extension example above adds blinks capabilities to X. Together with our new blinks component and styles APIs, however, we now have the tools we need to bring blinks to more 3rd party sites.


Next up: Reddit, Twitch, Facebook, & beyond. BlinkedIn, anyone?


In the coming days, we will be adding out of the box support for these and more sites via the blinks SDK.
