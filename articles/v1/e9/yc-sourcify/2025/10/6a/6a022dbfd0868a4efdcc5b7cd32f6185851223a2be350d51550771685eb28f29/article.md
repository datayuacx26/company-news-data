---
schema_version: "1.0.0"
document_id: "6a022dbfd0868a4efdcc5b7cd32f6185851223a2be350d51550771685eb28f29"
company_key: "yc-sourcify"
company: "Sourcify"
source_id: "yc-sourcify-atom-032fbdbe2ab3"
canonical_url: "https://docs.sourcify.dev/blog/signatures-analysis/"
published_at: "2025-10-30T00:00:00+00:00"
first_seen_at: "2026-07-24T01:53:14.949974+00:00"
fetched_at: "2026-07-28T21:59:41.762292+00:00"
content_hash: "sha256:a0845239aa7985b969e141b851aabdc946db98a15dbbd6d697393e8ecfdf948a"
---

# Spam or Legit? Analyzing 4byte Selector Collisions

Recently Sourcify took over openchain.xyz's 4byte signature APIs as well as the domain itself, maintained by[@samczsun](https://x.com/samczsun) . We also built the database and I wanted to run a quick analysis on the selector collisions and see how many collisions are legit vs. deliberately generated (spam).


We built the dataset and a service to serve the data. The dataset contains:


1. Data from openchain's dataset
2. Data from[4byte.directory](https://4byte.directory/)
3. Signatures from verified contracts in Sourcify.


You can see the database schema in the[database docs](https://docs.sourcify.dev/docs/repository/sourcify-database/#schema) and the service in[services/4byte](https://github.com/argotorg/sourcify/tree/staging/services/4byte) in the repo. As of now we have 4.7 million signatures, of which 1.9 million are not from verified contracts, and the rest of the majority appear in at least one verified contract ([stats](https://api.4byte.sourcify.dev/signature-database/v1/stats) ).


While it's possible to submit signatures to the database via the` /import` endpoint, we also add the signatures to the database automatically when a contract is verified. The 4byte databases are known to be spam prone, as function signatures are only 4 bytes and it's trivial to find a collusion to an otherwise legit signature.


For example, see the ERC20` transfer(address,uint256)` function's collisions under its selector` 0xa9059cbb` in our 4byte.sourcify.dev page:[https://4byte.sourcify.dev/?q=0xa9059cbb](https://4byte.sourcify.dev/?q=0xa9059cbb)


Seeing this and having the data I wanted do a quick analysis on the selector collisions and see how many collisions are legit vs. deliberately generated (spam).


## Analysis​


Running a simple query to find the signatures that share the same 4byte selector:


Query


```text
SELECT       concat  (  '0x'  ,   encode  (  signature_hash_4  ,     'hex'  )  )     AS   signature_hash_4  ,         COUNT  (  *  )     AS   num_signatures  ,       ARRAY_AGG  (  signature   ORDER     BY   signature  )     AS   signatures     FROM     public  .  signatures     GROUP     BY   signature_hash_4     HAVING     COUNT  (  *  )     >     1      ORDER     BY   num_signatures   DESC  ;
```


In the end we find **2789** 4byte selectors that have more than one signature. Here are the top 5 with most collisions:


[collisions.csv](https://docs.sourcify.dev/assets/files/collisions-d7a8c3de3e042da4371f585f0dd57ff6.csv/)


[collisions.json](https://docs.sourcify.dev/assets/files/collisions-430b889b1dc4840c380a70b322739aa0.json/)


signature_hash_4 num_signatures signatures


[0x00000000](https://4byte.sourcify.dev/?q=0x00000000) 61` AaANwg8((address,address,address,uint136,uint40,uint40,uint24,uint8,uint256,bytes32,bytes32,uint256))`
` abcei51243fdgjkh(bytes)`
` adfepixw()`
...


[0x00000001](https://4byte.sourcify.dev/?q=0x00000001) 15` account_info_rotate_tine(uint256)`
` exec_606BaXt(bytes\[\])`
` f00000001_bdmvamqo()`
...


[0xa9059cbb](https://4byte.sourcify.dev/?q=0xa9059cbb) 8` _____$_$__$___$$$___$$___$__$$(address,uint256)`
` fakeTransfer_4570999670(bytes)`
` func_2093253501(bytes)`
...


[0x095ea7b3](https://4byte.sourcify.dev/?q=0x095ea7b3) 8` __$$$$___$$_$_$$__$_$_$$__$$$$(address,uint256)`
` approve(address,uint256)`
` as9q06we_7x8z(uint256,address\[\],address\[\],uint256)`
...


[0x70a08231](https://4byte.sourcify.dev/?q=0x70a08231) 7` $_$$$_$$$$$_$_$____$$$$_$$_$__(address)`
` balanceOf(address)`
` branch_passphrase_public(uint256,bytes8)`
...


Looking at the top collisions, it might look a lot. But still the spamming seems not excessive and spammers generally find a single funny signature and call it a day. Out of the 2789 selectors, 2740 have only 2 signatures (ie. a single collusion) and only 49 with more than 2 signatures.


The interesting question is, how many of these collisions are actually unintended collisions vs. how many are deliberately generated (spam)?


Looking at them one by one would take some time. First I want to actually see only the collisions that have a verified contract. Ie. if` f00000001_bdmvamqo()` is not seen on a verified contract, let's assume it's a spam.


Query


```text
SELECT       concat  (  '0x'  ,   encode  (  s  .  signature_hash_4  ,     'hex'  )  )     AS   signature_hash_4  ,         COUNT  (  *  )     AS   num_signatures  ,       ARRAY_AGG  (  DISTINCT   s  .  signature   ORDER     BY   s  .  signature  )     AS   signatures     FROM     public  .  signatures s     WHERE     EXISTS     (         SELECT     1         FROM     public  .  compiled_contracts_signatures ccs        WHERE   ccs  .  signature_hash_32   =   s  .  signature_hash_32     )      GROUP     BY   s  .  signature_hash_4     HAVING     COUNT  (  *  )     >     1      ORDER     BY   num_signatures   desc  ;
```


Now we're left with 1023 "verified" collisions:


[collisions_verified.csv](https://docs.sourcify.dev/assets/files/collisions_verified-d3222ed2d572cec9be07c1751506f1a4.csv/)


[collisions_verified.json](https://docs.sourcify.dev/assets/files/collisions_verified-85fb6e632f58987dff57fb472459f4c6.json/)


signature_hash_4 num_signatures signatures


[0x00000000](https://4byte.sourcify.dev/?q=0x00000000) 28` AaANwg8((address,address,address,uint136,uint40,uint40,uint24,uint8,uint256,bytes32,bytes32,uint256))`
` arb_wcnwzblucpyf()`
` batchLock_63efZf()`
` buyAndFree22457070633(uint256)`
` call_g0oyU7o(address,uint256,bytes32,bytes)`
...


... ... *(4 rows with 4-5 signatures skipped)*


[0x415565b0](https://4byte.sourcify.dev/?q=0x415565b0) 3` JunionYoutubeXD_clgqmmkfvuba()`
` Sub2JunionOnYouTube_wuatcyecupza()`
` transformERC20(address,address,uint256,uint256,(uint32,bytes)\[\])`


[0x00000002](https://4byte.sourcify.dev/?q=0x00000002) 3` callWithPlaceholders4845164670(address,uint256,bytes32,bytes,(address,bytes,uint64,uint64,uint64)\[\])`
` wipeBlockchain_EkJWPe()`
` yoov6(address,address,uint256)`


[0x6c5b47d2](https://4byte.sourcify.dev/?q=0x6c5b47d2) 3` addDegree(uint256,string)`
` isBlacklisted5(address)`
` RenounceFungibleOwnership()`


[0x9aa7c0e5](https://4byte.sourcify.dev/?q=0x9aa7c0e5) 3` gain_network883718828((address,uint256,uint256,uint256,uint256,uint256,bool,uint256,uint256,uint256),uint8,uint256,uint256,address)`
` openTrade((address,uint256,uint256,uint256,uint256,uint256,bool,uint256,uint256,uint256),uint8,uint256,uint256,address)`
` TigrisTrade(int8,int56,uint80,bytes15,int88,int16)`


[0x014ed8d2](https://4byte.sourcify.dev/?q=0x014ed8d2) 2` CannotChangePaymentToken()`
` ModelRegistered(uint256,address,string,uint256)`


[0x0161a64a](https://4byte.sourcify.dev/?q=0x0161a64a) 2` cleanupExpiredListing(uint256)`
` MissingRole(address,bytes32)`


[0x0182a6da](https://4byte.sourcify.dev/?q=0x0182a6da) 2` initiateWalletTransfer(address)`
` withdrawStakingAmount(uint256)`


[0x01a754a3](https://4byte.sourcify.dev/?q=0x01a754a3) 2` AutoSwap()`
` updateTeamFeeContract(address)`


[0x025313a2](https://4byte.sourcify.dev/?q=0x025313a2) 2` getACLRole5999294130779334338()`
` proxyOwner()`


Now it starts to get interesting. Again the selectors with many collisions have mostly spam. But for 3 and less collisions we have some legitimate collisions.


For example for[0x01a754a3](https://4byte.sourcify.dev/?q=0x01a754a3) we have` AutoSwap()` and` updateTeamFeeContract(address)` . It's really difficult to tell if this is a spam or not.


But for the last row,[0x025313a2](https://4byte.sourcify.dev/?q=0x025313a2) , we have` getACLRole5999294130779334338()` and` proxyOwner()` . Here the former is clearly a spam and the latter is not.


Next, we can actually ask an LLM to filter the ones looking like a spam! Since the data is not excessive, I shoved all of it into Claude and asked it to filter the ones looking like a spam. In the end it gave me a list of **648** collisions that it thinks are legitimate. I peeked in the list and it seems to be mostly accurate:


[legitimate_collisions.csv](https://docs.sourcify.dev/assets/files/legitimate_collisions-4e19465afc1ed5448dedf2b063d3a7ab.csv/)


Here are 10 interesting examples of legitimate unintended collisions:


signature_hash_4 num_signatures signatures


[0x04d742dc](https://4byte.sourcify.dev/?q=0x04d742dc) 2` adminResetRank()`
` startSale(uint256,uint256,uint256)`


[0x0536f755](https://4byte.sourcify.dev/?q=0x0536f755) 2` FreeMintTokenSent(address,uint256)`
` NFTReward(address)`


[0x092338cc](https://4byte.sourcify.dev/?q=0x092338cc) 2` maxPurchasableInOneTx()`
` usdcGHSTOracle()`


[0x17915e8d](https://4byte.sourcify.dev/?q=0x17915e8d) 2` getCluster(address)`
` getTotalFeeBps()`


[0x2025e52c](https://4byte.sourcify.dev/?q=0x2025e52c) 2` createSaleTokensVault()`
` mintWithERC721(uint256)`


[0x22061379](https://4byte.sourcify.dev/?q=0x22061379) 2` getBaseStakeAmountForPlay()`
` vaultFees(uint256)`


[0x55fcd027](https://4byte.sourcify.dev/?q=0x55fcd027) 2` DepositAmountTooLow()`
` masterLogicAddress()`


[0x667022fd](https://4byte.sourcify.dev/?q=0x667022fd) 2` bought(address)`
` iceCreamVan()`


[0x67bf975c](https://4byte.sourcify.dev/?q=0x67bf975c) 2` NotAllowedToRecover()`
` RewardThresholdReached(uint256)`


[0x706b8722](https://4byte.sourcify.dev/?q=0x706b8722) 2` pauseAtId()`
` USDTBorrowed(address,uint256)`


These are all legitimate functions and events from different smart contracts that happen to share the same 4-byte selector purely by chance. This demonstrates that while 4-byte collisions are rare, they do happen naturally in the wild!


At this stage I just ran this for fun. We only have a list of popular signatures that we know the "correct" signature for ([canonical-signatures.json](https://github.com/argotorg/sourcify/blob/staging/services/4byte/src/utils/canonical-signatures.json) ). We filter out non-canonical ones by default and have a` filtered` field (can turn off filtering[in the API response](https://api.4byte.sourcify.dev/signature-database/v1/lookup?function=0xa9059cbb&filter=false) ). But if the community thinks this is useful, we can do a more thorough analysis and filter out the spam via LLMs.
