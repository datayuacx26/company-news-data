---
schema_version: "1.0.0"
document_id: "b83c248f575f9f79a7c826f27bc9403069a039cddcdabd9269cb2edadb80c14a"
company_key: "roblox-corporation-class-a-common-stock"
company: "Roblox Corporation"
source_id: "roblox-corporation-class-a-common-stock-news-import-fcd4c7b68777"
canonical_url: "https://about.roblox.com/zh-hant/newsroom/2025/07/roblox-guard-advancing-safety-for-llms-with-robust-guardrails"
published_at: "2025-07-22T21:37:00+00:00"
first_seen_at: "2026-07-23T23:14:36.468733+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:fa301d9d332219c36e8abd2f2cf6129bc571ace0c2cc671e410bad31d48a75bc"
---

# 尖端 LLM 技術協助保障 Roblox 上無限文本生成功能的安全

分享


[工程](https://about.roblox.com/zh-hant/newsroom?filter=%E5%B7%A5%E7%A8%8B)[新聞](https://about.roblox.com/zh-hant/newsroom?filter=%E6%96%B0%E8%81%9E)


# 尖端 LLM 技術協助保障 Roblox 上無限文本生成功能的安全


Roblox Guard 1.0：透過強健的防護措施提升安全性


發佈者


馬赫什·南德瓦納、亞當·麥克法林與尼莎奇·卡納


發佈日期


2025年7月22日


- 今日，我們宣布推出 Roblox Guard 1.0，這是一套專為開發者和平台設計的開源安全工具包。
- [Roblox Guard](https://github.com/Roblox/RobloxGuard-1.0) 的首項[功能](https://github.com/Roblox/RobloxGuard-1.0) ——一款用於大型語言模型（LLM）安全的最先進（SOTA）防護模型——現已推出，並在各大安全基準測試中樹立了新標竿。
- 我們同時發布了[Roblox Guard-Eval](https://huggingface.co/datasets/Roblox/RobloxGuard-Eval) ，這是一套用於安全基準測試的資料集。


##### 挑戰


我們最近發布了一項[文字生成 API](https://devforum.roblox.com/t/beta-introducing-text-generation-api/3556520) ，讓開發者能夠運用大型語言模型（LLM）的強大功能，在體驗中生成文字，從而打造更豐富、更沉浸式的體驗。例如，開發者可以建立一個完全互動的 NPC，或提供關於如何遊玩遊戲的互動式教學。


自 Roblox 創立初期，我們便[積極審查](https://about.roblox.com/newsroom/2025/07/roblox-ai-moderation-massive-scale) 平台上的[多數內容](https://about.roblox.com/newsroom/2025/07/roblox-ai-moderation-massive-scale) ，致力確保產品符合 Roblox 嚴格的安全與文明標準。在推出文字生成 API 之前，我們優先考量如何建立安全機制。我們開發了一種新模型，用以保護輸入內容（用戶的提示語）與輸出內容（API 生成的文字）。


##### 創新


Roblox Guard 1.0 工具包的首項功能，是一款針對指令進行微調的頂尖大型語言模型（LLM），旨在協助保護我們的文字生成 API。該模型會在提示詞和回應兩個層級進行安全分類，判斷每個輸入或輸出是否違反我們的政策。這種雙層級評估對於審查使用者查詢以及模型自身產生的輸出結果至關重要。


在標準評測基準上，我們的 LLM 目前表現優於 Meta 的 Llama Guard、Google AI 的 ShieldGemma、NVIDIA NeMo Guardrails 以及 OpenAI 的 GPT-4o 等熱門 LLM 防護模型。Roblox Guard 1.0 LLM 在面對未見過的分類體系（out-of-domain）資料集時，也展現出強大的泛化能力。 我們已將首項功能的[LLM 權重](https://huggingface.co/Roblox/Llama-3.1-8B-Instruct-RobloxGuard-1.0) ，以及[Roblox Guard-Eval](https://huggingface.co/datasets/Roblox/RobloxGuard-Eval) 基準測試資料集一併開源。


本系統的核心是一台基於 Llama-3.1-8B-Instruct 模型微調而成的 LLM。我們在訓練此 LLM 時，特別著重於高品質的指令微調，以優化安全判斷表現。此過程中的關鍵步驟在於精心策劃提示語與回應，以反映多樣化的真實世界安全情境。


我們的指令集未使用任何專有資料，僅結合了合成資料（由 LLM 生成）與開源資料，這使我們能更輕鬆地擴展訓練資料並運用規模化法則，從而使這款首款 Roblox Guard LLM 成為當前最先進（SOTA）的模型。在整合各種開源與合成資料集時，我們發現採用資料集專屬的分類法是編排指令的最佳方法，因為任務的多樣性有助於 LLM 針對不同類型的提示進行訓練。 這造就了一個堅實的模型，能夠泛化應用於不同的安全分類體系。我們還將「思維鏈理據」（chain-of-thought rationales）——即鼓勵模型闡述其推理過程——納入指令集。這些中間推理步驟賦予了模型更強的語境基礎。


##### 結果


我們的安全團隊針對 Roblox 的內容安全分類體系（涵蓋 25 個子類別）開發了一套客製化的高品質評估資料集。這套評估資料集是透過內部紅隊演練所建立，我們透過模擬對抗性攻擊來測試系統並尋找漏洞，且其中不包含任何用戶生成或個人資料。 此評估資料集包含提示詞與回應的配對，其中回應由政策專家團隊人工標註，以確保其品質。資料集涵蓋廣泛的違規類型，有助於我們建立更精確且具實質意義的評估標籤。最終評估資料集共包含 2,873 個範例。我們已將此評估資料集開源，其具備可擴展的安全分類體系，有助於對大型語言模型（LLM）的防護機制與內容審查系統進行基準測試。


我們在涵蓋提示詞與回應的全面性開源資料集，以及 Roblox Guard-Eval 上對模型進行基準測試。這使我們能夠同時在域內與域外資料集上評估模型。 我們以二元違規/非違規分類的 F-1 分數來報告結果。在上表中，我們將自身表現與數個知名模型進行比較。這項首個 Roblox Guard 功能在對域外資料集進行泛化時，表現優於其他模型。


我們持續改進安全系統，包括 Roblox Guard 1.0 工具，並計劃在不久的將來推出更多功能。請關注我們在[HuggingFace](https://huggingface.co/datasets/Roblox/RobloxGuard-Eval) 和[GitHub](https://github.com/Roblox/RobloxGuard-1.0) 上的頁面，以獲取未來的更新與改進資訊，以及後續的開源發布。
