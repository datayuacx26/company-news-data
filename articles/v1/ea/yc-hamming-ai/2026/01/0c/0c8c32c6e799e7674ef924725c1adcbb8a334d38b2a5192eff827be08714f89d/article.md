---
schema_version: "1.0.0"
document_id: "0c8c32c6e799e7674ef924725c1adcbb8a334d38b2a5192eff827be08714f89d"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-88fd2d772bc9"
canonical_url: "https://hamming.ai/blog/building-enterprise-voice-agents-in-europe-versaia"
published_at: "2026-01-30T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:38.132930+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:c6881ec13189fb1a359ed163a484f76f9406ffdd2a696106434557a7064f4e40"
---

# Building Enterprise Voice Agents in Europe

# Building Enterprise Voice Agents in Europe


*This post was adapted from Hamming's podcast conversation with Ronald van der Looy, co-founder of[Versaia](https://www.versaia.ai/) . Versaia is a Dutch conversational AI company building enterprise-grade voice agents for regulated sectors including healthcare, financial services, and real estate.*


Most voice AI coverage focuses on US-based companies and English-language use cases. But some of the most interesting challenges in voice AI emerge when you step outside that bubble: different languages, different regulatory environments, and different levels of enterprise readiness.


Versaia is tackling these challenges head-on in the Netherlands. They're one of the few AI-native startups in the region focused specifically on enterprise deployments rather than SMB quick wins.


**Quick filter:** If you're building voice agents for European markets or regulated industries, the lessons here apply directly. If you're US-only and English-only, this is still useful context for understanding where the broader market is headed.


## Why Enterprise Voice AI in Europe Is Different


Ronald and his team didn't start with enterprise ambitions. Like many voice AI companies, they initially targeted SMB use cases. The pivot came from a practical realization: building for SMB felt too easy for their engineering-heavy team.


But more importantly, they noticed a gap. When they looked at the European landscape, they couldn't find a single AI-native company building specifically for enterprise voice deployments in the Netherlands.


The competition in Europe looks different from the US:


- Cognigy and Parloa dominate the German market
- Many Dutch companies working on AI focus on workflow automation, not voice
- Most voice-focused startups are one-person operations using 11 Labs as their platform


Building a full platform from the ground up is hard. Building one that meets enterprise compliance requirements while handling Dutch language nuances is harder.


## The Trust Problem: Why Dutch Enterprises Hold Back


The biggest barrier to enterprise adoption isn't technical. It's trust.


Ronald shared a telling anecdote: seven or eight years ago, major Dutch banks deployed chatbots on their websites. These early chatbots were, in his words, "very, very bad." One famous example: a customer told the chatbot they wanted to change their address because they were moving. The chatbot responded: "Okay, have a nice weekend."


This history created what Ronald calls a "head start problem." Dutch consumers and enterprises have a negative connotation with AI agents because they remember when they didn't work.


> They're not afraid exactly, but they're holding back. They want to do pilots, implement in some use cases. Maybe only do the routing: when a customer calls, we ask what they want and route them. They're gradually implementing it.


The result is a longer sales cycle focused on proof of value. Versaia has learned to lead with live demos where prospects can talk directly to the agent. Showing that modern voice AI actually works is more persuasive than any pitch deck.


## The Dutch Language Challenge


For teams building in English, it's easy to take STT quality for granted. Whisper works well. Deepgram works well. You have options.


Dutch is a different story. Ronald's team spent months searching for TTS and STT systems that could handle Dutch properly.


- Many providers claim to support 20+ languages including Dutch
- When you actually test them, "it's rubbish, unusable"
- The main problem was speech-to-text accuracy, not just TTS


The breakthrough came when they discovered[Soniox](https://soniox.com/) . The accuracy improvement was dramatic, especially for the hardest parts of Dutch voice AI:


- **Last names:** Dutch surnames are complex and varied
- **Phone numbers:** Spoken digit sequences were consistently incorrect with other providers
- **Email addresses:** Complex addresses now work reliably
- **Loan words:** Dutch has adopted many English words - "feedback," "management," "update" - that are now part of everyday vocabulary


That last point is critical, and it's more nuanced than simple code-switching. The Dutch language itself has absorbed English words that speakers use without thinking of them as English. When someone says "Ik heb feedback over de meeting," they're speaking Dutch - but a voice agent might interpret those English-origin words as a signal to switch languages entirely.


## The Loan Word Trap


The challenge isn't just that Dutch speakers occasionally switch to English. It's that Dutch has integrated so many English words that they're now part of the language itself. Words like "feedback," "management," "update," and "meeting" appear constantly in business Dutch.


This creates a trap for voice agents: hearing "management" might trigger the STT to switch to English mode for the rest of the utterance, even though the speaker never left Dutch. The result is garbled transcription and a confused agent.


Versaia solved this with careful STT configuration:


- **Minimum word threshold:** Require multiple consecutive English words before allowing a language switch, so isolated loan words don't trigger it
- **Language hints:** Tell the STT that Dutch is the primary language, treating English words as expected vocabulary
- **Language restrictions:** Limit which languages are allowed in a given context to prevent false switches


These seem like small details, but they're the difference between a demo that impresses and a production system that works. A voice agent that switches to English every time someone says "update" will frustrate users fast.


## Context-Aware STT for Specialized Domains


Beyond code-switching, Versaia is pushing STT accuracy further with domain-specific context injection.


For their **real estate agents** , they pass street names directly to the STT. Dutch street names are notoriously difficult but when the STT knows the possible options, it can match phonetically similar input to the correct street.


For their **pharmacy agents** , the challenge is medicine names. These are complex, often multi-syllable terms that no general-purpose model handles well.


> We can give a list of medicines. It's like 10,000 characters maximum and then it just works. It's not 100% perfect, but like 90% perfect.


The key insight: Soniox allows per-API-call context injection. This means you can dynamically adjust what the model expects to hear based on the conversation state.


Without this capability, pharmacy use cases would require constant clarification ("Did you mean meloxicam?"). With it, the system gets it right on the first try most of the time.


## What Enterprise Deals Actually Look Like


Versaia's approach to enterprise sales has evolved. Early pitch decks, Ronald admits, were embarrassing in retrospect.


What works now:


1. **Live demos** where prospects interact with the agent directly
2. **Partnerships** with consultancies already embedded in enterprise accounts
3. **Scalable integrations** that can be replicated across many clients


On the integration side, Versaia is building with a multiplier mindset. Their real estate agent connects to a CRM system used by thousands of agencies. Build the integration once, deploy to thousands of customers.


The same pattern applies to pharmacy: build the integration with the pharmacy management system once, then scale across the market.


This is different from the bespoke enterprise approach where every deployment is custom. It's a bet that standardized integrations can meet enterprise needs while maintaining the economics to actually scale.


## Predictions for 2026


Ronald expects 2026 to be the year European enterprise adoption accelerates:


- C-level executives are pushing teams to "do something with AI"
- The technology has caught up to enterprise requirements
- Trust is slowly rebuilding as modern systems prove themselves


The constraint now is education. Many decision-makers still don't understand what's possible:


> We should try to teach them what is possible. In the beginning we tried to sell agents. Now we say: this is possible, this is what we can do, this is how it works. We need to teach them because they don't know.


On the technical side, Ronald is watching for:


- **Better emotional TTS:** Google Gemini's emotional voices sound good but don't support streaming yet
- **More natural speech patterns:** Stop words, pauses, and filler sounds that make agents feel human
- **Emotion detection:** Understanding not just what users say but how they say it


## Testing Voice Agents with Voice Agents


Ronald often explains to prospects that Versaia tests their agents using other AI agents (through Hamming).


> They're totally mind-blown. For us it's quite easy to understand; agents connect to a system, there's integration tooling, function tooling. But for them it's all a mystery. Agents testing agents? How does that work?


This disconnect between what's technically possible and what buyers understand highlights the education gap that still exists in enterprise voice AI.


## Key Takeaways for Teams Building in Europe


1.


**Don't underestimate language-specific challenges.** What works for English may completely fail for Dutch, German, or other European languages. Test STT providers rigorously in your target language.


2.


**Handle loan words, not just code-switching.** Many languages have absorbed English words that speakers use without thinking of them as English. Your STT must recognize these as part of the primary language, not triggers to switch.


3.


**Context injection unlocks specialized domains.** Pharmacy, real estate, and other verticals become viable when you can tell the STT what to expect.


4.


**Trust is the real barrier, not technology.** Early chatbot failures created lasting skepticism. Live demos that let prospects experience the quality difference are more effective than slides.


5.


**Build for replication, not bespoke.** Enterprise economics improve dramatically when integrations can be reused across many customers.


The European enterprise voice AI market is earlier than the US, but the trajectory is clear. Teams that solve the language and trust challenges now will have significant advantages as adoption accelerates.


[Listen to the full conversation on The Voice Loop](https://www.youtube.com/watch?v=YxWtBLArXm8&feature=youtu.be)
