---
schema_version: "1.0.0"
document_id: "067486dd52e3f5c63da6220b77e1614d3b3e2da70ad89170fd41c5466ea470e7"
company_key: "yc-dialect"
company: "Dialect"
source_id: "yc-dialect-news-import-4cb56bc42b2e"
canonical_url: "https://www.dialect.to/blog/alerts-stack-upgrade-get-started-in-under-10-minutes"
published_at: "2025-07-06T11:29:00+00:00"
first_seen_at: "2026-07-21T16:13:21.108809+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:ec496a341f18536d17a24e4b6004384f89d1e38fd4150f830b8fa0f8609f284e"
---

# Alerts Stack Upgrade: Get started in under 10 minutes

Today, we are launching a major upgrade to our Alerts Stack. You can now integrate our notifications bell into your dapp with 80% less code. We listened to your feedback and completely rewrote how you interact with our react SDK.


Alongside that, we shipped a fresh new set of default styles, so you can get started branding to your dapp with a better design baseline. Check out our live example here[https://alerts-example.dialect.to/](https://alerts-example.dialect.to/) .


This 80% reduction in code is so significant we believe you can get a notification bell in your dapp and receive your first alert in less than 10 minutes. Seriously. All you need to do is:


1. Register your dapp


2. Add the Dialect Bell to your React app in 10 lines of code, subscribe a test user


3. Login to our dapp dashboard, send your first alert


All of this code can be found in the quickstart guide in our docs. But let’s run through it here.


#### Register your dApp


First, register a wallet for your dapp. To do this, first create a keypair if you don’t already have one.


> *// Create a dapp keypair solana-keygen new -o <insert-dapp-name>-messaging-keypair.json solana-keygen publickey <insert-dapp-name>-messaging-keypair.json > dapp-pubkey.pub // \[OPTIONAL\] Create a test user keypair (or use one you already have in Phantom or another chrome extension wallet) solana-keygen new -o test-user-messaging-keypair.json*


Then install these dependencies in a node environment of your choice, and run these code snippets.


> *// install dependencies npm install @solana/wallet-adapter-base --save npm install @dialectlabs/sdk --save npm install @dialectlabs/blockchain-sdk-solana --save*


> *// run code snippet tocreate wallet solana-keygen new-o <insert-dapp-name>-messaging-keypair-devnet.json solana-keygen publickey <insert-dapp-name>-messaging-keypair-devnet.json > dapp-pubkey.pub*


You’ll need this dapp public key in the next section, for use in your react front end code. Do not share your private key with anyone.


#### Add the Dialect Bell to your app


If your project is built with React, all you need is a single React context provider and UI component to add Dialect’s notification bell to your app. But first, let’s install dependencies.


> *// install dependencies npm add @dialectlabs/react-ui @dialectlabs/react-sdk-blockchain-solana*


For convenience, let’s bundle this code snippet into a single file and call it *DialectNotificationComponent.tsx.*


> *// DialectNotificationComponent.tsx 'use client'; import'@dialectlabs/react-ui/index.css'; import { DialectSolanaSdk } from'@dialectlabs/react-sdk-blockchain-solana'; import { NotificationsButton } from'@dialectlabs/react-ui'; /* Set DAPP_ADDRESS variable to the public key generated in previous section */ constDAPP_ADDRESS = '...'; exportconstDialectNotificationComponent = () => { return ( <DialectSolanaSdk dappAddress={DAPP_ADDRESS}> <NotificationsButton /> </DialectSolanaSdk> ); };*


This context provider, *DialectSolanaSdk* abstracts away a ton of configuration that used to be explicit, such as environment target, session caching, local storage, and other customizable configurations. All of that has now been abstracted away with smart defaults, resulting in just 10 lines of code you need to get started. All you need to provide is the dapp public key you generated above. Check out our[complete docs](https://docs.dialect.to/documentation/notifications-and-monitoring/advancing-react-notifications/styling-notifications-widget) on how to add custom configuration.


Inside of this context provider is the single UI component, *NotificationsButton* . This component is a notification bell you can add to your navbar, and it comes with all of the configuration your users need to subscribe to, manage, and receive alerts from you.


This component is style-able to your brand, but comes with a fresh new out-of-the-box set of styles with this latest release, giving you a better baseline to design from as you get it on brand.


We will cover the styling API in a future post. For now, let’s work with the defaults.


Now, take this file you wrote above, *DialectNotificationComponent.tsx* , and import it into your app to add to your navbar. This code has been simplified for demonstration purposes.


> */* App.tsx */ import { ConnectionProvider, WalletProvider } from'@solana/wallet-adapter-react'; import { DialectNotificationComponent } from'<wherever-you-created-the-file-above>'; constApp = () => { return ( // We're using @solana/wallet-adapter-react package for wallet api // Assuming WalletProvider and ConnectionProvider are properly configured with necessary wallets and network. <ConnectionProvider endpoint={endpoint}> <WalletProvider wallets={wallets} autoConnect> {/* Your app's components go here, nested within the context providers. */} <DialectNotificationComponent /> </WalletProvider> </ConnectionProvider> ); }*


A couple things to note:


1. Dialect’s notification bell takes the user’s wallet as input. All actions that the user takes within the notification bell, such as subscribing to notifications or fetching notifications, are all tied to their wallet, and require authentication. Therefore you’ll want to make sure whatever wallet context provider you’re using is higher in the component tree and makes the wallet object available.


2. You provide the public key of the dapp keypair you created above, to tell Dialect which dapp’s alerts the SDK target.


That’s it. Let’s now create a test user to subscribe to notifications.


#### Subscribe, and receive your first notification


All notifications are opt-in. To receive your first alert you will need to subscribe to notifications using a test wallet. To do this, run your app and open the notification bell to enable alerts.


Now that you have a user subscribed to your test dapp, let’s send your first notification. Dialect’s SDK can be used to send programmatic alerts for things like filled orders, liquidation warnings, NFT bids and offers, and anything else. But for the purposes if this demo let’s let’s use the Dialect dapp dashboard, from which you can manually write and send alerts.


Take the keypair you generated above for your dapp, import it into Phantom, and then head over to[the Dialect dashboard](https://docs.dialect.to/alerts) and connect with this dapp wallet.


From the dashboard you can craft announcements such as company announcements, product updates, etc. These alerts can be sent to various channels, such as in-app, Telegram, email, and SMS.


Let’s start with in-app:


Any channel that has a check mark is marked for sending. Those without a checkmark will be ignored. Let’s send.


Now head back to your app as a test user. You should see a red dot appear on the notification bell, indicating you have a new unread alert. Open the bell to check for the new alert.


That’s it! In just minutes you’ve registered a dapp, installed a notification bell in your navbar, and used our dapp dashboard to send your first alert.


We can’t wait to see what you build with Dialect’s Alerts Stack. Check out our[full documentation](https://docs.dialect.to/alerts) for more in-depth usage.
