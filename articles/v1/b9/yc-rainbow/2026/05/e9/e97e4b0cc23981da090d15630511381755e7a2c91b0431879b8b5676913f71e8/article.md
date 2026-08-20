---
schema_version: "1.0.0"
document_id: "e97e4b0cc23981da090d15630511381755e7a2c91b0431879b8b5676913f71e8"
company_key: "yc-rainbow"
company: "Rainbow"
source_id: "yc-rainbow-atom-0b2ea2826f28"
canonical_url: "https://github.com/rainbow-me/rainbowkit/releases/tag/%40rainbow-me%2Frainbowkit-siwe-next-auth%400.6.0"
published_at: "2026-05-06T08:43:04+00:00"
first_seen_at: "2026-07-25T20:24:22.592367+00:00"
fetched_at: "2026-08-20T01:52:49.727604+00:00"
content_hash: "sha256:1d9815a7eea0c29ff8c0f8c820af96a628f02aa56379560a353eff1ef4b5627d"
---

# @rainbow-me/rainbowkit-siwe-next-auth@0.6.0

### Minor Changes


-


[e90c2dd](https://github.com/rainbow-me/rainbowkit/commit/e90c2dd0814d69ac9ba73e944461327617185ed9) : Upgraded to NextAuth v5. This is a breaking change.


Key changes:


- Requires NextAuth v5 (` next-auth >=5.0.0-0 <6` ); NextAuth v4 apps must migrate before upgrading.
- NextAuth server configuration now uses v5 APIs like` NextAuthConfig` ,` Credentials` , and the exported` auth` helper.
- Pages Router server calls must pass` req` and` res` separately to` auth` ; passing the full` GetServerSidePropsContext` is no longer valid.
- NextAuth v5 internal cookies use` authjs` names, including` authjs.csrf-token` or` __Host-authjs.csrf-token` for CSRF depending on secure-cookie settings.
- CSRF nonce validation now compares the SIWE nonce against the` csrfToken` value that NextAuth v5 posts to the Credentials provider, instead of parsing CSRF cookies from request headers.


Migration guide:


1. Upgrade` next-auth` to v5 and upgrade` @rainbow-me/rainbowkit-siwe-next-auth` .


```text
-   npm install next-auth@^4 @rainbow-me/rainbowkit-siwe-next-auth
+   npm install next-auth@5.0.0-beta.31 @rainbow-me/rainbowkit-siwe-next-auth
```


1. Update your NextAuth server configuration to the v5 API.


```text
-   import type { NextAuthOptions } from 'next-auth';
-   import CredentialsProvider from 'next-auth/providers/credentials';
+   import NextAuth from 'next-auth';
+   import type { NextAuthConfig } from 'next-auth';
+   import Credentials from 'next-auth/providers/credentials';


-   export const authOptions: NextAuthOptions = {
+   export const authOptions: NextAuthConfig = {
providers: [
-       CredentialsProvider({
+       Credentials({
async authorize(credentials) {
/* your SIWE validation */
},
}),
],
};
+
+   export const { handlers, auth, signIn, signOut } = NextAuth(authOptions);
```


1. Update Pages Router server-side session lookups to use the exported` auth` helper. Pass` req` and` res` separately; passing the full` GetServerSidePropsContext` is not supported by the v5 overloads.


```text
-   import { getServerSession } from 'next-auth';
-   import { authOptions } from '../auth';
+   import { auth } from '../auth';


export const getServerSideProps: GetServerSideProps = async (context) => {
-     const session = await getServerSession(
-       context.req,
-       context.res,
-       authOptions,
-     );
+     const session = await auth(context.req, context.res);


return {
props: {
session,
},
};
};
```


1. Update SIWE nonce checks that call` getCsrfToken` inside` authorize` . When using` signIn('credentials', ...)` , NextAuth v5 validates the CSRF cookie before` authorize` runs and includes the verified token in` credentials.csrfToken` .


```text
-   import { getCsrfToken } from 'next-auth/react';


-   if (
-     siweMessage.nonce !==
-     (await getCsrfToken({ req: { headers: req.headers } }))
-   ) {
-     return null;
-   }
+   const csrfToken =
+     credentials && 'csrfToken' in credentials
+       ? credentials.csrfToken
+       : undefined;


+   if (siweMessage.nonce !== csrfToken) {
+     return null;
+   }
```


1. If upgrading from before` @rainbow-me/rainbowkit-siwe-next-auth@0.5.0` , also follow the` 0.5.0` changelog entry for the` viem/siwe` migration and the` 0.3.0` changelog entry for the earlier` getCsrfToken` request-shape change.
