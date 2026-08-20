---
schema_version: "1.0.0"
document_id: "a82273073f07bb0b6a15352a9d4e553cb96b7efc8ea1fbff0c6e4b9a28432ebf"
company_key: "yc-arc-prize-foundation"
company: "ARC Prize Foundation"
source_id: "yc-arc-prize-foundation-atom-3536270edb25"
canonical_url: "https://arcprize.org/blog/arc-agi-3-human-dataset"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:48.897807+00:00"
fetched_at: "2026-07-28T20:52:42.510574+00:00"
content_hash: "sha256:97c0f6708e4cbc62220a0abdef322d8c74c939958418227ff4b819b6212e1db2"
---

# Measuring Human Performance on ARC-AGI-3

# Measuring Human Performance on ARC-AGI-3


AGI is here when a system can learn like a human.


However there is still a gap between what humans can learn and what AI can learn.[ARC Prize Foundation](https://arcprize.org/) exists to understand this gap. The[ARC-AGI benchmarks](http://localhost:3000/arc-agi) are our tools for measuring it.


Using these tools requires understanding human performance. How do real people, our only proof point of general intelligence, actually learn and solve novel problems?


Today we’re releasing the[human dataset for ARC-AGI-3](https://dub.link/vfwCqvb) - a controlled study of 458 participants and the most exhaustive human testing study in the ARC-AGI series to date.


We do not yet have AGI. This dataset is the receipt.


ARC-AGI-3 Public Demo environments:[AR25](https://arcprize.org/tasks/ar25) ,[LF52](https://arcprize.org/tasks/lf52) ,[SB26](https://arcprize.org/tasks/sb26)


## ARC-AGI-3


[ARC-AGI-3](https://arcprize.org/arc-agi/3) is a series of 135 abstract reasoning environments.[Play them yourself](https://arcprize.org/tasks/ls20) .


The test taker, whether human or AI, is not given instructions on how to play. They must explore, infer the rules that govern the environment, and come up with a strategy on their own.


A key design constraint of ARC-AGI-3 is that every environment must be solvable by humans with *no prior training* . To ensure this, we conducted the largest formal study on ARC-AGI human performance ever done.


To dive deeper into ARC-AGI-3, check out the[benchmark](https://arcprize.org/arc-agi/3) , play the[environments](https://arcprize.org/tasks/ls20) , or watch our[launch video](https://youtu.be/f_xT45Pi0UQ) .


## Human Baselines on ARC-AGI-3


To gather human baselines we tested members of the general population. Participants included various levels of education, income, job sectors, and ages. We did not control for one particular demographic.


We held weekly, in-person, focus groups in a San Francisco-based testing center. No references to ARC Prize Foundation or AI testing were made at any time.


Each testing session lasted 90 minutes. Every participant received a base payment of ~$130, plus an extra $5 for each environment they successfully solved.


Tests were held under "first-run" conditions. This means every participant only saw environments once and had a single attempt to beat it. This measures the ability to learn and adapt to a novel problem, not the ability to repeat a previously learned solution.


Humans were given the same prior information and affordances that AI is given:


- Both humans and AI get the same "system prompt”
- Test takers are told available environment actions
- Neither are told that this was an ARC Prize or ARC-AGI test


Humans and AI received identical information. Neither had an informational advantage.


The instruction modal used during testing. These are the only instructions given to humans.


As testing progressed, the data we collect shows us what intelligence looks like in practice.


If an environment is too difficult, it was excluded from the dataset or revised.


The end result is a dataset that demonstrates human solvability of 100% of environments. Every environment is beaten by at least two independent participants and most are beaten by many more.


## ARC-AGI-3 Human Testing Dataset


We're open sourcing the full[Public Demo dataset](https://dub.link/vfwCqvb) , which includes 342 human step-by-step replays for our 25 public environments. With it researchers can analyze:


- Per-environment solvability
- Action counts and efficiency distributions
- Full replay data (step-by-step human interactions)


### Solvability


The most basic measure of human performance is *solvability* : can humans complete these environments? To answer that, we look at completion rates.


Our early findings:


- Not all environments are solved equally - 10 out of 10 participants solved[r11l](https://arcprize.org/tasks/r11l) , whereas only 6 out of 12 solved[tr87](https://arcprize.org/tasks/tr87)
- Per-level data reveals where players get stuck - in[cd82](https://arcprize.org/tasks/cd82) , 2 of 11 players couldn't get past level 2, but most who did went on to solve the entire environment. This suggests a steep onboarding curve rather than overall difficulty


The table below summarizes each of the 25 public demo environments, including how many participants attempted each environment, how many solved it, and links to the top 10 replays (scores = levels completed).


Env. Plays Solves Top 10 Replays (score)


[ar25](https://arcprize.org/tasks/ar25) 10 5[8](https://arcprize.org/replay/2a854897-cb79-48f4-92e1-0288df2cf6a9) ,[8](https://arcprize.org/replay/2ef9c7c0-8d10-4ab0-af3f-27d9e655e113) ,[8](https://arcprize.org/replay/41285539-37a7-4553-b731-a17a37a3f2df) ,[8](https://arcprize.org/replay/b36f18b1-39c1-4aec-9d65-73a083a09f6a) ,[8](https://arcprize.org/replay/f4698acc-06ff-418b-b419-c384194aef0a) ,[7](https://arcprize.org/replay/00589449-cfb4-4b98-a31e-2a344db66f89) ,[6](https://arcprize.org/replay/0cf890a2-b328-47f3-a845-0fa379d29358) ,[6](https://arcprize.org/replay/e7f9a7d6-273f-4a1f-b9dd-01e694cda138) ,[5](https://arcprize.org/replay/03665144-5518-4e4b-af99-ec7fe6ab546d) ,[0](https://arcprize.org/replay/e8c9d67c-83a5-4352-bd16-575577dabfd2)


[bp35](https://arcprize.org/tasks/bp35) 14 2[9](https://arcprize.org/replay/01e79bb8-4697-45c2-9bde-8de3b8c25169) ,[9](https://arcprize.org/replay/092c5051-3cda-4a35-bf7f-1323253b797d) ,[8](https://arcprize.org/replay/132b5f89-e602-4684-b845-de63146ef402) ,[8](https://arcprize.org/replay/3098fb46-f1d7-4eae-a2b2-60e03a55b294) ,[8](https://arcprize.org/replay/3493839a-d1a7-4abf-8d80-636daa3cd20c) ,[8](https://arcprize.org/replay/9cedf1a9-3337-4472-8208-c2042bb4ad73) ,[7](https://arcprize.org/replay/37623647-bd22-417c-831c-69b557ad1a05) ,[7](https://arcprize.org/replay/58bc55eb-02e2-4d0f-8731-d06fc1f94eb4) ,[7](https://arcprize.org/replay/d731594b-0b14-41f0-8615-d24fe0574776) ,[6](https://arcprize.org/replay/cf4004d8-6975-4a28-9033-9c91d047728d)


[cd82](https://arcprize.org/tasks/cd82) 11 8[6](https://arcprize.org/replay/130512d8-1d1b-4ee4-ab4b-9e0397fa6af8) ,[6](https://arcprize.org/replay/2e4399fe-f0f6-421d-bc32-7827ad792b96) ,[6](https://arcprize.org/replay/52879352-3a32-4bde-a55d-075f1d2a5a3f) ,[6](https://arcprize.org/replay/627766f8-bfb7-4a25-ba48-ca4a555d63a6) ,[6](https://arcprize.org/replay/68939ee7-b3fe-40f6-9307-3f143ddf03d2) ,[6](https://arcprize.org/replay/73141bdd-24a4-4725-8c51-514e23f107eb) ,[6](https://arcprize.org/replay/bac88f30-3772-4c39-8ed0-2b3fc6d8eab2) ,[6](https://arcprize.org/replay/eccc0126-e306-4e6e-8bff-3e76fef63777) ,[2](https://arcprize.org/replay/0c6d47d7-7bc3-40a6-97bd-9388d6411ebe) ,[2](https://arcprize.org/replay/33f6b2f7-9e0d-41c5-ab94-5f929cdb85c1)


[cn04](https://arcprize.org/tasks/cn04) 12 6[6](https://arcprize.org/replay/1e88ef7d-2a0c-4d4b-94c4-9d50ac0e6b58) ,[6](https://arcprize.org/replay/295a945d-dd3d-4608-b989-0e3fc98ab481) ,[6](https://arcprize.org/replay/368a1bda-05c2-4995-a0f3-947c28a6e9fc) ,[6](https://arcprize.org/replay/49345faa-7b95-4661-ba20-594e12326e76) ,[6](https://arcprize.org/replay/b2a7c347-81f0-4075-a54b-96ddd336bd36) ,[6](https://arcprize.org/replay/cc4c82aa-4768-4505-a109-c4a99a65c0bb) ,[3](https://arcprize.org/replay/6fcb0288-417d-44ce-a3a8-0d27bf16c270) ,[3](https://arcprize.org/replay/a52b9c50-af36-46fd-9c70-3132d7f422cb) ,[2](https://arcprize.org/replay/1fe0b79b-b173-4ac6-9bc0-9d232c074233) ,[1](https://arcprize.org/replay/14a28e82-4f24-4cb6-8810-39d70b9d0b4b)


[dc22](https://arcprize.org/tasks/dc22) 11 4[6](https://arcprize.org/replay/1b9a2041-bc99-4b7b-8d99-041f0503956f) ,[6](https://arcprize.org/replay/ab11e695-b9d2-42f4-871c-8f90aa8c3a85) ,[6](https://arcprize.org/replay/e8f76fe2-d90a-44ec-97cd-d09ec9b9cbcd) ,[6](https://arcprize.org/replay/f1995788-de5b-478a-9f68-88bb4eba2a0d) ,[5](https://arcprize.org/replay/1c34d3f9-d7da-4902-b41b-2e918419249f) ,[5](https://arcprize.org/replay/884a349e-56b9-42c0-8a57-934680d13128) ,[4](https://arcprize.org/replay/5e79ec77-f723-4b0c-8e66-971c13ff2f28) ,[4](https://arcprize.org/replay/a5156a49-8a56-4025-9264-d5f3fd840bec) ,[4](https://arcprize.org/replay/abb26776-6bf6-456f-88c9-3f20a73a24a0) ,[3](https://arcprize.org/replay/ef0eff5a-de59-4a57-a592-4ac189a50ffa)


[ft09](https://arcprize.org/tasks/ft09) 10 4[6](https://arcprize.org/replay/01f76fc7-c612-4006-8d08-c13ed0cb91de) ,[6](https://arcprize.org/replay/193a1bf4-f858-4f5e-a5dc-8d8f4159b7fe) ,[6](https://arcprize.org/replay/3b1ce31a-0af9-41f3-858f-4ead42360d15) ,[6](https://arcprize.org/replay/f58b7fe2-aafd-414a-a1fb-74d4b3e717e0) ,[3](https://arcprize.org/replay/f28bd7b8-3832-4b99-9583-5c7d8fdf93c4) ,[2](https://arcprize.org/replay/17938e15-217b-467c-91a3-a31aa638868a) ,[2](https://arcprize.org/replay/62ee83b0-5850-4a88-90af-693e9476507f) ,[1](https://arcprize.org/replay/84d4f82f-a33b-4744-8fc8-d5fb0ec43ebc) ,[0](https://arcprize.org/replay/70287c87-432e-447a-ba96-aa62780227c3) ,[0](https://arcprize.org/replay/dd8d1c51-06ba-46c0-a444-b8fc097e878a)


[g50t](https://arcprize.org/tasks/g50t) 14 3[7](https://arcprize.org/replay/3de1fb81-8541-4803-8624-5f5cb62dc78e) ,[7](https://arcprize.org/replay/c71254fe-00b6-4ee2-b68a-941263eab37f) ,[7](https://arcprize.org/replay/d2aef7cd-05f5-4af2-b736-2c54aa4f5e5f) ,[5](https://arcprize.org/replay/373cfc77-965d-48e7-91cd-3eea227116c8) ,[3](https://arcprize.org/replay/224bacfb-31d3-43e7-9d72-976a6b540435) ,[3](https://arcprize.org/replay/c7e359dd-11e2-44e1-984a-9b38e794ae01) ,[2](https://arcprize.org/replay/08d5d2c0-3a61-449d-9de6-62ec5e253e29) ,[2](https://arcprize.org/replay/42e8affb-ab23-4535-9831-a8bb04a1131a) ,[2](https://arcprize.org/replay/686fbe44-90d9-47f6-8a6b-16493cfe3fe0) ,[2](https://arcprize.org/replay/c27295c4-6885-4157-b990-d6f9a6290a55)


[ka59](https://arcprize.org/tasks/ka59) 10 3[7](https://arcprize.org/replay/021d8b1d-980f-4ff0-a40d-6a02dc27c415) ,[7](https://arcprize.org/replay/c714584f-2af3-442a-89d3-aed9bddbf705) ,[7](https://arcprize.org/replay/df12c800-c18c-494e-9949-c9fac56cf863) ,[6](https://arcprize.org/replay/7c3222bf-3d88-40df-8d35-eabd7a572642) ,[5](https://arcprize.org/replay/fa26a895-2e85-4227-ab94-29ab00efa8b8) ,[1](https://arcprize.org/replay/993191db-ce93-425f-ac62-c6d0a023afa8) ,[1](https://arcprize.org/replay/9cf1e190-5731-47fc-8a94-fe148312bb9d) ,[0](https://arcprize.org/replay/52dd648e-dc33-4b71-8bb8-6ce0f3bd61ed) ,[0](https://arcprize.org/replay/6a1d161a-29f9-497d-8ec5-574e61085adc) ,[0](https://arcprize.org/replay/9bd8dbeb-72a0-464d-b97e-117fbc3f3981)


[lf52](https://arcprize.org/tasks/lf52) 11 4[10](https://arcprize.org/replay/09ed6d45-ac12-462f-aa01-49164a5f7ec7) ,[10](https://arcprize.org/replay/2adcd0b8-5e21-42e6-8976-b163b0d96d4a) ,[10](https://arcprize.org/replay/c492c65f-dc07-4c5e-97ac-fce7a4366dbd) ,[10](https://arcprize.org/replay/d55d768c-0ead-4e34-9c5e-288449092557) ,[9](https://arcprize.org/replay/40bd319e-4d3a-451e-87e2-c88b02c2b024) ,[9](https://arcprize.org/replay/549e2635-b796-480c-b67f-f740c42bc086) ,[9](https://arcprize.org/replay/be7ef956-16ed-4bbd-9ce9-b8e92e666f2c) ,[9](https://arcprize.org/replay/da243e77-1d54-4996-815b-8ec8d9611e5f) ,[1](https://arcprize.org/replay/2b7ab5e3-7ba6-4e65-845e-fa50fa439f8a) ,[1](https://arcprize.org/replay/401bf4ae-848b-4c07-8f95-9e3d85ee1ebf)


[lp85](https://arcprize.org/tasks/lp85) 54 15[8](https://arcprize.org/replay/05b0a343-1666-4286-8c81-c5d212f3cf72) ,[8](https://arcprize.org/replay/072f2a2b-5624-48b5-a3f5-83edf1879de5) ,[8](https://arcprize.org/replay/18aeec67-f441-4ea1-92c2-e19dd9802cf6) ,[8](https://arcprize.org/replay/2bfe836e-615b-4ebf-8b1c-6763557fbdb7) ,[8](https://arcprize.org/replay/380aead7-3d4f-427b-8933-4b76d09b3a45) ,[8](https://arcprize.org/replay/543d2fc4-84f4-497d-a87a-47bd578d8410) ,[8](https://arcprize.org/replay/662864be-cfe2-4d9c-b1a5-fc57a7464786) ,[8](https://arcprize.org/replay/6b63d889-ccaf-434a-929e-f74076fb6c62) ,[8](https://arcprize.org/replay/72f71d14-f711-4964-bc44-cd10110dab84) ,[8](https://arcprize.org/replay/813ed237-7717-491d-8524-bab13aef81c9)


[ls20](https://arcprize.org/tasks/ls20) 13 6[7](https://arcprize.org/replay/34d098c6-df52-458c-b5ad-19cefeb75981) ,[7](https://arcprize.org/replay/3f80c449-59bc-4ad7-9b3e-a8f88def78d8) ,[7](https://arcprize.org/replay/57630d5e-4a01-42b6-bda1-43ced11c5bd7) ,[7](https://arcprize.org/replay/8aed7120-f7a9-45a1-837a-68bc7dc37a4f) ,[7](https://arcprize.org/replay/8f03134f-a5f6-4b76-95b6-fd70fcb81ee1) ,[7](https://arcprize.org/replay/cfe4acef-d6bc-499d-bede-0d397703362d) ,[5](https://arcprize.org/replay/6865b541-8885-4624-8c10-d31ba68c290a) ,[5](https://arcprize.org/replay/94b39ad1-a025-4d18-a20a-a14eb2aa1b6a) ,[2](https://arcprize.org/replay/637215bb-389a-475e-83e3-53baf74c8104) ,[1](https://arcprize.org/replay/af60632e-f620-4aac-87a6-21f4e90b7fbd)


[m0r0](https://arcprize.org/tasks/m0r0) 11 7[6](https://arcprize.org/replay/0e1adaaf-fe7e-47f3-923f-97791151e11e) ,[6](https://arcprize.org/replay/1e451cef-6c66-489d-8839-569949055264) ,[6](https://arcprize.org/replay/6fff490d-2cca-4467-95a9-ccd8317bf1c5) ,[6](https://arcprize.org/replay/a98ab7b3-64fe-41ab-a357-c65519897691) ,[6](https://arcprize.org/replay/b4f5fd23-2ede-4f2e-8086-20164cef9f77) ,[6](https://arcprize.org/replay/db341128-f220-46a6-925c-3be5056cc99e) ,[6](https://arcprize.org/replay/f03a5fda-7dc2-44ee-a0ce-0b9ad50074e1) ,[5](https://arcprize.org/replay/276e8a6c-5fdc-4b17-b5fa-9f8d640e0a99) ,[4](https://arcprize.org/replay/920ac028-2c8b-42f0-84b6-f0ba07108046) ,[1](https://arcprize.org/replay/7748b6e5-b3b0-4546-a75c-81d810ba25d4)


[r11l](https://arcprize.org/tasks/r11l) 10 10[6](https://arcprize.org/replay/0218265f-d04d-4f5e-8119-37d3aa7a481f) ,[6](https://arcprize.org/replay/1c820c2e-0bb7-4f54-bffe-e4d9a1385f89) ,[6](https://arcprize.org/replay/448ad63a-64a7-436e-8956-f2fe9163cd93) ,[6](https://arcprize.org/replay/669c3a6f-669a-42c4-b718-e5c3976310ad) ,[6](https://arcprize.org/replay/7bda9483-2c94-4d34-9657-e6fae7aa82ef) ,[6](https://arcprize.org/replay/9a66bffc-2f6e-46f1-8243-eab372227374) ,[6](https://arcprize.org/replay/bb1c0808-4214-4bb7-a3b0-ebd42d37a985) ,[6](https://arcprize.org/replay/c12f8e45-8cbf-461d-a1be-e3e001eb9ba0) ,[6](https://arcprize.org/replay/ceb779a0-75fa-47f3-a7e0-4347310fe8e5) ,[6](https://arcprize.org/replay/ffb9b53d-d7a0-4d0b-866e-c49839ee57c5)


Env. Plays Solves Top 10 Replays (score)


[re86](https://arcprize.org/tasks/re86) 11 5[8](https://arcprize.org/replay/0d461c1c-21e5-4dc8-b263-9922332a6485) ,[8](https://arcprize.org/replay/1bc8ef4d-deb5-423e-994d-87ceb5d7c496) ,[8](https://arcprize.org/replay/27857e7d-0959-4d33-aa11-6be1a5d5b13f) ,[8](https://arcprize.org/replay/ea40f804-1c1f-4db4-8354-8228ff01f8c2) ,[8](https://arcprize.org/replay/ed783823-e742-4ae5-a1f1-0a306fb42921) ,[6](https://arcprize.org/replay/79ae6a3a-0101-454c-8ede-74c363f4ab2b) ,[6](https://arcprize.org/replay/c9b181b2-6088-408a-8622-204fcb733b34) ,[6](https://arcprize.org/replay/dca94d28-b02d-450a-ad31-f421d9a94678) ,[2](https://arcprize.org/replay/37442af6-3490-4416-96d5-01e71ddd9757) ,[2](https://arcprize.org/replay/953a2d9f-12d2-4131-90b6-e57f49d4288b)


[s5i5](https://arcprize.org/tasks/s5i5) 11 4[8](https://arcprize.org/replay/157d3bc0-93da-449e-9186-67fc0402a9a4) ,[8](https://arcprize.org/replay/17941b1a-7e06-46d4-8bc4-5765a2a1656d) ,[8](https://arcprize.org/replay/4dd39f12-983c-471c-8821-0b01e55de6a1) ,[8](https://arcprize.org/replay/e9c797d3-363f-4f19-b532-9b5eb9a5eb75) ,[7](https://arcprize.org/replay/2f21a731-87b0-4a78-b1a3-4e392c3433ea) ,[4](https://arcprize.org/replay/583978f2-3d54-48c6-a5ba-388bc6a05cc4) ,[4](https://arcprize.org/replay/66d63c5e-5e2b-4c87-81c7-b46bd78a038b) ,[2](https://arcprize.org/replay/8a528ea6-633f-421f-9ad2-a4cf2f10ddc7) ,[2](https://arcprize.org/replay/8cefd372-6a72-4bc3-b3c7-5549633cb615) ,[2](https://arcprize.org/replay/960b9124-1de0-4eb1-a916-bdc51474664c)


[sb26](https://arcprize.org/tasks/sb26) 12 5[8](https://arcprize.org/replay/005a84c0-2969-488b-baa1-d57e68866b33) ,[8](https://arcprize.org/replay/06c7cb54-5e41-4599-bfc4-6983ca16b671) ,[8](https://arcprize.org/replay/0a3dac2e-1302-46df-b6ca-6250f3c1b6e1) ,[8](https://arcprize.org/replay/441ddef6-1d8d-4f84-9416-ac5fd969ff58) ,[8](https://arcprize.org/replay/72305960-4a13-4d9f-9044-463185ec707a) ,[7](https://arcprize.org/replay/22407b34-bff1-44be-97e2-ba99986e24a0) ,[6](https://arcprize.org/replay/4ad15cb2-a6e4-4f6f-9457-3b64b1d7ec07) ,[4](https://arcprize.org/replay/48aac0ed-7f3f-4b2a-9ee9-795131d24377) ,[4](https://arcprize.org/replay/f471f3fd-7867-41d9-9a75-57e917278f7e) ,[4](https://arcprize.org/replay/ffef2c25-8ea9-4a2b-a4a5-a109bbfc5156)


[sc25](https://arcprize.org/tasks/sc25) 15 10[6](https://arcprize.org/replay/0115a266-f750-41d0-9646-1b3113f8cf1c) ,[6](https://arcprize.org/replay/0966d61f-d6d1-43f9-8f87-f35883dfa020) ,[6](https://arcprize.org/replay/18dd1fff-bad5-436a-98aa-c8f2b21522ee) ,[6](https://arcprize.org/replay/55ca108d-e25d-4eb9-9ce4-02fc80685c71) ,[6](https://arcprize.org/replay/8f50e6d5-f1e4-46e1-86b3-fad3ffd1c76a) ,[6](https://arcprize.org/replay/b0b236ff-bd06-4f16-a350-5f1ca9f2446d) ,[6](https://arcprize.org/replay/b1877d54-7639-4f85-8836-c2701909eb77) ,[6](https://arcprize.org/replay/e46999e5-b318-4fe3-86e1-d9178e2e185d) ,[6](https://arcprize.org/replay/eb453464-f145-4cb1-8887-fcf2c47cbbfb) ,[6](https://arcprize.org/replay/f253a1f3-77a3-45b2-b0aa-dab4f3355f6c)


[sk48](https://arcprize.org/tasks/sk48) 14 7[8](https://arcprize.org/replay/20fb75ad-57fa-4c94-b030-9316f4e61313) ,[8](https://arcprize.org/replay/6b7d5bb2-5987-48e3-a350-11de56b83cee) ,[8](https://arcprize.org/replay/74db5f57-616a-4e99-8b23-12f18b5a1e6b) ,[8](https://arcprize.org/replay/ae7fa71b-5f0e-4460-8a76-ee13e54ac6f1) ,[8](https://arcprize.org/replay/b62df293-1d00-4689-83ef-75fa091b59d3) ,[8](https://arcprize.org/replay/d2d6fa4b-fdf2-4f63-85b9-37d2b18e5298) ,[8](https://arcprize.org/replay/da6b0954-9a3d-4c03-9cf6-dd5d66a8d23f) ,[7](https://arcprize.org/replay/03243599-4f62-41a9-bf9e-cc42bd41e74c) ,[4](https://arcprize.org/replay/01a5191c-a488-40b7-abf7-7cef06283cb0) ,[4](https://arcprize.org/replay/fc416db5-aa41-4259-988f-6d2bfe664d17)


[sp80](https://arcprize.org/tasks/sp80) 12 2[6](https://arcprize.org/replay/99c07645-c856-4c45-954d-e43e1b116fc5) ,[6](https://arcprize.org/replay/ee7752b0-a9ad-4c23-8de4-173307367f31) ,[5](https://arcprize.org/replay/8dd23daf-8643-40ec-975c-88a28080fdf7) ,[4](https://arcprize.org/replay/0e7037a5-ceaf-4654-b246-cc7067e47330) ,[4](https://arcprize.org/replay/83caf46a-8c23-4d49-9440-3971018d0dba) ,[2](https://arcprize.org/replay/0a573e1a-5010-4914-ba09-9c106ab21354) ,[2](https://arcprize.org/replay/dcc71276-e037-4e3a-8711-7f0d88b8b3c9) ,[1](https://arcprize.org/replay/25b5db58-46f8-45af-a679-32494f3a9f60) ,[1](https://arcprize.org/replay/45683b33-7bb5-4089-834a-2f993939b252) ,[1](https://arcprize.org/replay/add9ed90-535f-49f8-b90f-d804b5c96f68)


[su15](https://arcprize.org/tasks/su15) 13 3[9](https://arcprize.org/replay/20f8d100-ff47-420b-bf1d-3d239a255a77) ,[9](https://arcprize.org/replay/274da438-5f61-4688-b290-5d927e6fbd77) ,[9](https://arcprize.org/replay/fdeab422-e1e4-43de-a2c0-6441b16ae748) ,[4](https://arcprize.org/replay/3d4cbfde-01a1-44c1-8e1a-3e93e7971ad0) ,[4](https://arcprize.org/replay/645988a9-6eef-4eb9-867d-e44ec93a6926) ,[4](https://arcprize.org/replay/c2bf26cf-eaed-4492-a715-73007e936022) ,[3](https://arcprize.org/replay/4be2291c-367e-4927-8b3f-f53aa17b9395) ,[3](https://arcprize.org/replay/606d9b04-7bac-4892-b2e1-3072dea7062a) ,[1](https://arcprize.org/replay/3a47010c-3f84-454b-b3cd-3464b8f4992f) ,[1](https://arcprize.org/replay/823b42ee-9e93-4658-92ae-00f0619f7c6c)


[tn36](https://arcprize.org/tasks/tn36) 14 6[7](https://arcprize.org/replay/28c6b31e-993d-483f-9584-c88bd15d423d) ,[7](https://arcprize.org/replay/84115ab3-7401-4d00-b9ef-8ce102480e13) ,[7](https://arcprize.org/replay/90f13b17-9916-4d8a-81ad-dabf059bd943) ,[7](https://arcprize.org/replay/98d37f25-6357-4543-9eda-48136a48a3b4) ,[7](https://arcprize.org/replay/a343d0c8-ebbc-48a6-b0cf-24e831314ed0) ,[7](https://arcprize.org/replay/b7e8173a-76be-4a71-ae4b-b28561e86c9d) ,[6](https://arcprize.org/replay/0268d6a8-2294-473c-8ed0-f1fefbf4005b) ,[6](https://arcprize.org/replay/85db062c-c74d-43fe-b6a9-8bd09b1c254b) ,[6](https://arcprize.org/replay/eeb9ae5a-2abe-4f3a-8e49-af6b10f6c989) ,[1](https://arcprize.org/replay/47d0e2b1-25ee-442c-84ae-01d781f5e257)


[tr87](https://arcprize.org/tasks/tr87) 12 6[6](https://arcprize.org/replay/147def87-0b0e-44f9-aa34-bdaf51d0cdb9) ,[6](https://arcprize.org/replay/3519e988-59d8-433b-acee-6f98539352ad) ,[6](https://arcprize.org/replay/64fc55eb-cf12-4824-ad15-2b908edfdecb) ,[6](https://arcprize.org/replay/713a3919-0f6a-4a6f-83b7-7910071937b9) ,[6](https://arcprize.org/replay/7a166e4a-5a18-4ab7-931a-c36f8c496697) ,[6](https://arcprize.org/replay/c703a2d9-f8a3-4672-9f60-d06da8f62284) ,[5](https://arcprize.org/replay/4808fbde-18ba-40da-bf0f-afc426077b11) ,[5](https://arcprize.org/replay/a8d60184-988b-4e1f-a03f-3a9b3a6af90b) ,[5](https://arcprize.org/replay/eb28c424-e3a7-443d-8b09-9fade27f19e2) ,[4](https://arcprize.org/replay/3c0b8607-d7d7-47ad-955b-77a6a2153bfc)


[tu93](https://arcprize.org/tasks/tu93) 13 9[9](https://arcprize.org/replay/0578a2a4-9131-4b10-b1d9-fd43f85d4d6a) ,[9](https://arcprize.org/replay/2e61febd-8fcf-4b82-8f3e-ebec2d0e6923) ,[9](https://arcprize.org/replay/4efce135-0cd8-4a9c-97e7-8d670e3d343b) ,[9](https://arcprize.org/replay/66fb3310-f64d-4621-80c3-9b516aa68999) ,[9](https://arcprize.org/replay/74d1dff4-3de7-4911-9574-21c93fd257ae) ,[9](https://arcprize.org/replay/7f4b4e31-fcb8-4d16-ba3d-81df53efdfdd) ,[9](https://arcprize.org/replay/b1b2959e-39a7-4c4d-aefe-9c146c145048) ,[9](https://arcprize.org/replay/c56dafbb-0698-45ef-a007-f19464c472a2) ,[9](https://arcprize.org/replay/e5b51453-dcff-40d8-a068-4da064db741f) ,[4](https://arcprize.org/replay/7597d7f8-d4cb-4d67-b565-fd8f9d72a739)


[vc33](https://arcprize.org/tasks/vc33) 10 6[7](https://arcprize.org/replay/4cffe392-c5a8-47f7-a2db-86a97a4606ca) ,[7](https://arcprize.org/replay/74e0c891-bfa0-4b39-be15-db1f8dce4366) ,[7](https://arcprize.org/replay/837812ad-2c5c-4943-8632-b6f8cd4b5b4d) ,[7](https://arcprize.org/replay/918430fb-d0ad-4d3a-9551-1a806e6ddd48) ,[7](https://arcprize.org/replay/e31a16d6-345f-464f-abd4-077c20e51f77) ,[7](https://arcprize.org/replay/e60e880e-aed6-4d38-af81-3ef95be929e1) ,[6](https://arcprize.org/replay/1469cb95-ccf9-4a93-8356-f44443d20dce) ,[3](https://arcprize.org/replay/374887fc-b52f-4ab9-bcaa-9b3ad5fdd9d2) ,[2](https://arcprize.org/replay/f1addbb7-4708-48e4-8d47-095d049ad5d4) ,[0](https://arcprize.org/replay/7fdb11de-8464-40ae-926a-dcad63592822)


[wa30](https://arcprize.org/tasks/wa30) 14 5[9](https://arcprize.org/replay/0f88f0f8-aeeb-438d-a901-fe09de856cfb) ,[9](https://arcprize.org/replay/5fd30bfc-e7c4-48e1-ac4c-65b80de6f7d8) ,[9](https://arcprize.org/replay/e326447f-992b-4aba-bc39-21240eabe0e1) ,[9](https://arcprize.org/replay/f73b1e1d-dbfc-412b-93a5-561954a3e88a) ,[9](https://arcprize.org/replay/fbd3f8c7-8327-4ad4-ac4b-91e00e6ddd9a) ,[8](https://arcprize.org/replay/8ded76ac-a0e1-4f00-98f6-7de0b3f7984f) ,[8](https://arcprize.org/replay/9149006d-c076-477e-bc91-7304a870fa28) ,[8](https://arcprize.org/replay/d4fc9e5d-56c3-4715-b4f6-bc231214e789) ,[6](https://arcprize.org/replay/c070bccb-d8c1-4843-a094-4a997cbe6f5f) ,[5](https://arcprize.org/replay/5c0bed76-ae96-43c1-b99b-aa293848257e)


Public Demo Set 25 Environments: 342 plays, 145 solves. Note: lp85 has higher play counts due to early inclusion as an ARC-AGI-3 preview environment in July 2025.


### Efficiency


*How* humans beat the environments matters too. Efficiency measures not just whether a level is completed, but how many actions it took relative to the human baseline. This connects directly to the[definition of intelligence](https://arcprize.org/arc-agi#defining-agi) , skill-acquisition efficiency.


ARC-AGI-3 offers the first formal measure of *learning efficiency* in the ARC-AGI series. The chart below shows the distribution of human per-level efficiency across the public demo set.


Each bar represents how many human level-solves fell at that efficiency. 100% means the player solved the level in the same number of actions as the median human. Above 100% = fewer actions; below = more.


## ARC-AGI-3 Scoring Updates


Since previewing ARC-AGI-3, nearly one million scorecards have been submitted on public environments. That real-world data helps us stress-test and harden our scoring approach.


What we’ve observed:


1. **"Luck factor"** : In at least one level of one game, the environment restricted determining the optimal path from the start. An early choice could lock a player out of the minimum action count regardless of how well they played afterward. Imperfect information isn't inherently a problem, it's a natural feature of many environments. However, it is an issue when the baseline is tight enough that one unlucky early choice dominates a player's score. Our original human baseline (2nd best human) forced luck into what should be a pure measure of reasoning efficiency.
2. **First place doesn’t always get 100%** : We observed that test takers could beat the human baseline on every single level by a wide margin, but if they were subpar on just one level, the hard cap of 100% per level meant their overall score dropped below 100%. This did not accurately reflect the spirit of measuring a game's action efficiency.


Based on what we’ve observed, we’re announcing two updates to ARC-AGI-3 scoring:


1. **The per-level baseline is now less sensitive to outlier performances, reducing the impact of luck on individual levels.**
A single unusually efficient human run no longer defines the baseline for ARC-AGI-3 scoring. Rather the baseline now reflects more typical human play. Technical change: the human baseline which normalizes scores moves from 2nd-best player to median player per level.
2. **A single subpar level no longer disproportionately drags down an overall score**
A test taker who generalizes well across an entire environment is no longer penalized by a single, sub-par, level. Technical change: per-level score cap increases from 100% to 115%.


The net result of these changes is a marginal *increase* in scores for both humans and AI (+0.5pp) and better reflects our desire to fairly compare efficiency between test taskers.


Our core claim for ARC-AGI-3 remains:


- ARC-AGI-3 is 100% solvable by humans. Every environment was beaten by at least two humans, typically five or more, out of a small uncontrolled panel of around ten members of the general public.


To see how this works in practice, here's the action progression chart for[re86](https://arcprize.org/tasks/re86) from our 10 human testers.


Level progression vs Action chart for[re86](https://arcprize.org/tasks/re86) . Notice how five players finish the game, but only four score 100% - they were more efficient than the human baseline. The fifth player finished but scored 89% because they weren't as efficient as the baseline.


When AI scores 100% on ARC-AGI-3 it means AI beat every level of every environment at or above the median human-baseline action efficiency.


To read more about our scoring, see our[documentation](https://docs.arcprize.org/methodology) .


## Looking ahead


We believe our human study for ARC-AGI-3 is the largest of its kind for an AI benchmark and we’re proud to fully open source the dataset to support our mission.


Going forward, we’ll continue to develop benchmarks that are grounded the understanding of human performance.


We’ll continue releasing data, improving methodology, and refining scoring


If you’d like to join us on this journey, consider making a[tax-deductible donation](https://arcprize.org/donate) to ARC Prize Foundation or exploring[roles](https://arcprize.org/jobs) on our team.
