---
schema_version: "1.0.0"
document_id: "e48cbc1b9a2122299284455fde18c0a9d9ad904151cd3d93e1e47c0c99365163"
company_key: "forrester-research-inc-common-stock"
company: "Forrester Research Inc."
source_id: "forrester-research-inc-common-stock-rss-7ea008fcdcc6"
canonical_url: "https://www.forrester.com/blogs/openai-makes-hardware-passkeys-mandatory-for-its-highest-end-cyber-model/"
published_at: "2026-07-31T17:11:00+00:00"
first_seen_at: "2026-08-01T00:05:58.384786+00:00"
fetched_at: "2026-08-01T00:06:00.242614+00:00"
content_hash: "sha256:3dda866641d6d79283d0b2f0e5c792853d21dd912ce04fc8ff29ae8b218e41bb"
---

# OpenAI Makes Hardware Passkeys Mandatory For Its Highest-End Cyber Model

In April 2025, OpenAI[announced](https://www.forrester.com/blogs/openai-requires-identity-verification-for-access-to-its-latest-models/) that it will require its users to complete formal, government-issued, national ID document-based identity verification (IDV). Now[OpenAI announced](https://openai.com/index/gpt-5-6/) that, effective September 1, 2026, Trusted Access Cyber (TAC) accounts will be required to authenticate using hardware passkeys to access OpenAI’s most advanced cyber AI models. This capability will need to be enabled via OpenAI’s Advanced Account Security. OpenAI announced special hardware passkey pricing for[Yubico Yubikeys](https://www.yubico.com/blog/openai-mandates-hardware-backed-passkeys-for-trusted-access-cyber-members-to-log-into-chatgpt-accounts/) , namely YubiKey C NFC (predominantly for mobile devices) and YubiKey C Nano (for desktops that have a mini-USB connector). OpenAI also[supports other FIDO-compatible hardware keys](https://chatgpt.com/advanced-account-security) for Advanced Account Security.


Hardware-based FIDO passkeys improve security, but they also introduce new challenges that customers should consider:


- **Using API keys for TAC services cannot be fully automated.** If invocation of GPT-5.6 models via APIs requires hardware FIDO passkey-based authentication, then an authorized user must manually perform this ceremony at least once (provided that API keys can be stored on the API caller’s end). This may result in service outages. If not every API model invocation requires authenticating via FIDO passkeys, then the mechanism is vulnerable to session theft — something that is unique to the OpenAI case. This can be especially inconvenient when trying to meet DevOps CICD integration or emergency access requirements.
- **Hardware keys means organizations and users must absorb new device and management costs.** Hardware keys are — in a way — a return to one-time password generators (such as RSA SecurID tokens) that create higher management costs (e.g., shipping keys to users, replacing lost hardware tokens). Because Yubico does not store the seed of its hardware keys centrally, OpenAI must provide self-services that allow for recovery of hardware keys. Currently, when users enroll in Advanced Account Security,[OpenAI](https://help.openai.com/en/articles/20001221-advanced-account-security) disables all SMS and email account recovery permanently and mandates that users register a hardware security key, plus another either software or hardware FIDO passkey. Organizations should also consider that USB-C-only-based hardware keys may not be compatible with all mobile devices, which could create user experience challenges. Users with limited dexterity and vision may also encounter additional friction when trying to use hardware keys.
- **Hardware keys could hinder equal access to higher-end AI models.** Given that hardware FIDO passkeys may be unavailable in certain geographies, mandating passkeys may de facto automatically exclude access to these high-end models from certain countries or regions.


Device-bound, software-only FIDO passkeys (which should be required only for researching and using high-risk security AI technologies), complemented with a secure synchronization mechanism (such as password managers including 1Password or Dashlane) and continuous, contextual authentication, could be more convenient alternative than hardware passkeys. Organizations should understand that there are trade-offs associated with implementing hardware-based strong authentication.


Forrester clients who want to dive deeper into this topic and discuss how they should implement IAM for LLMs can schedule[an inquiry or guidance session](https://www.forrester.com/contact-us/) with me.


Share


-
-
