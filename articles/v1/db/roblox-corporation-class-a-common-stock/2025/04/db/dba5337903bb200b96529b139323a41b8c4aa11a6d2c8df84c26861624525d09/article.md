---
schema_version: "1.0.0"
document_id: "dba5337903bb200b96529b139323a41b8c4aa11a6d2c8df84c26861624525d09"
company_key: "roblox-corporation-class-a-common-stock"
company: "Roblox Corporation"
source_id: "roblox-corporation-class-a-common-stock-news-import-fcd4c7b68777"
canonical_url: "https://about.roblox.com/zh-hant/newsroom/2025/04/updating-ml-voice-safety-more-languages"
published_at: "2025-04-02T19:00:00+00:00"
first_seen_at: "2026-07-23T23:14:36.468733+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:30466b6fd2d2af5d2f97fec8777a488e870056292b78664196c2c5129d8fd9c8"
---

# 為我們的開源語音安全模型新增更多語言

分享


[工程](https://about.roblox.com/zh-hant/newsroom?filter=%E5%B7%A5%E7%A8%8B)[安全與文明](https://about.roblox.com/zh-hant/newsroom?filter=%E5%AE%89%E5%85%A8%E8%88%87%E6%96%87%E6%98%8E)[新聞](https://about.roblox.com/zh-hant/newsroom?filter=%E6%96%B0%E8%81%9E)


# 為我們的開源語音安全模型新增更多語言


發佈者


工程副總裁 Naren Koneru 與 Janne Pylkkonen


發佈日期


2025年4月2日


- 我們正在更新開源[語音安全分類器](https://about.roblox.com/newsroom/2024/07/deploying-ml-for-voice-safety) ，將其參數從 9,460 萬增加至 1.202 億，並擴展至七種新增語言。
- 自分類器首版發布以來，我們已將英文語音聊天數據的準確度提升至 59.1% 的召回率，且假陽性率維持在 1%。相較於前一版本 30.9% 的召回率，這代表了 92% 的顯著提升。


促進安全與文明始終是 Roblox 所有工作的基石。我們花了近二十年時間建立強大的安全系統，並隨著新技術的出現持續發展和改進這些系統。在 2024 年，我們推出了超過 40 項安全改進，包括[對家長控制功能的全面改版](https://about.roblox.com/newsroom/2024/11/major-updates-to-our-safety-systems-and-parental-controls) ，而我們[今天](https://about.roblox.com/newsroom/2025/04/new-parental-controls-on-roblox) 將[再次](https://about.roblox.com/newsroom/2025/04/new-parental-controls-on-roblox) 對其[進行更新](https://about.roblox.com/newsroom/2025/04/new-parental-controls-on-roblox) 。 我們還推出了業界首批開源[語音安全分類器](https://about.roblox.com/newsroom/2024/07/deploying-ml-for-voice-safety) 之一，迄今已被[下載](https://huggingface.co/Roblox/voice-safety-classifier) 超過 23,000 次。今天，我們發布了[更新](https://huggingface.co/Roblox/voice-safety-classifier-v2) 版本，其準確度更高，且支援更多語言。


許多協助保護用戶的安全系統（包括此分類器）皆由 AI 模型驅動。我們將其中部分模型開源，因為我們深知[分享 AI 安全技術的進展](https://about.roblox.com/newsroom/2025/02/becoming-a-founding-partner-of-open-source-initiative-key-to-online-safety-approach) 將惠及整個產業。這也是我們近期以創始夥伴身分[加入 ROOST](https://about.roblox.com/newsroom/2025/02/becoming-a-founding-partner-of-open-source-initiative-key-to-online-safety-approach) 的原因——這是一個致力於透過推廣開源安全工具來解決數位安全重要議題的新成立非營利組織。


面對全球平台上每日產生的海量內容與互動，人工智慧是保障用戶安全不可或缺的關鍵要素。我們確信所建構的模型正有效支援我們的運作需求。以 2024 年第四季為例，Roblox 用戶上傳了 3,000 億項內容。 在這數以百億計的影片、音訊、文字、語音聊天、虛擬角色及 3D 體驗中，僅有 0.01% 被偵測到違反我們的政策。而且，幾乎所有違反政策的內容都在用戶看到之前，就被自動預先篩選並移除了。


我們已更新開源版語音安全分類器，以提升其準確性，並協助我們對更多語言的內容進行審查。新模型：


- 透過多語言數據的訓練，新增了七種語言的違規偵測能力——包括西班牙語、德語、法語、葡萄牙語、義大利語、韓語和日語。
- 整體召回率提升至 59.1%，較前一版本的 30.9% 召回率提升 92%，且誤報率極低。
- 經過大規模部署優化，高峰時段每秒可處理多達 8,300 次請求（其中大部分不包含違規內容）。


自首個模型發布以來，我們觀察到美國用戶每小時語音內容的濫用舉報率降低了超過 50%。該模型還協助我們每日[審核](https://about.roblox.com/safety-civility-resources?section=safety&article=enforcing-the-rules) 數百萬分鐘的語音聊天內容，其準確度甚至超越人工審核員。我們永不停歇地推進安全系統的進展，並將持續更新開源版本。


### 高效的多語言語音安全分類器


我們最初的開源語音安全分類器基於 WavLM base+ 模型，並透過機器標記的英文語音聊天音訊樣本進行微調。此端到端架構的令人鼓舞的成果，促使我們進一步嘗試自訂架構。我們運用知識蒸餾來優化模型的複雜度與準確性，這對於大規模推論服務而言極具吸引力。 我們的新分類器採用了這些基礎構建模塊，並在數據使用與架構優化方面進行了擴展與深化。


透過多語言數據的訓練，我們的單一分類器模型能無縫處理我們支援的前八種語言中的任何一種。此外，訓練方法的改進使該模型不僅更精準，在典型的推論情境下運行速度也比第一版快了 20% 至 30%。


新的語音安全分類器仍基於 WavLM 架構，但層級配置與先前版本及 WavLM 預訓練模型有所不同。特別是，我們新增了一層卷積層，以降低變壓器層的內部時間解析度。 整體而言，我們的新模型架構擁有 1.202 億個參數，相較於前一版本的 9,460 萬個參數增加了 27%。儘管參數增加，但當處理 4 至 15 秒的輸入片段時，新模型的運算時間仍減少了 20% 至 30%。這是因為模型將輸入訊號壓縮成比以往更短的表示形式。


#### 運用多種標註策略


端到端模型的監督式訓練需要經過精心整理的音訊與類別標籤配對。我們對資料處理流程進行了重大改進，確保標籤資料能穩定持續地輸入。訓練資料的基礎是一套龐大的機器標記資料集，包含超過 10 萬小時涵蓋所有支援語言的語音內容。 我們自動將語音轉錄為文字，並透過內部開發的文字型毒性分類器進行處理，該分類器採用了預設的政策與毒性分類標準。在資料收集過程中，我們以高於正常語音的機率採樣具攻擊性的內容，藉此更精準地捕捉邊界案例及較不常見的政策違規情形。


基於語音轉錄本和文字分類的標籤，無法完全捕捉語音聊天內容中的細微差異。因此，我們利用人工標註的資料，對前一階段訓練出的模型進行微調。雖然分類任務相同，但後期的訓練階段有助於精煉決策邊界，並強化模型對語音聊天特有表達方式的反應能力。這是一種課程式學習，能幫助我們充分發揮這些寶貴的人工標註範例的價值。


端到端模型訓練面臨的一項挑戰是，若標註政策隨時間變動，目標標籤可能會變得過時。因此，當我們優化可接受的語音政策時，對於採用舊版標註標準的資料，需要採取特殊處理。 為此，我們採用了多任務方法，讓模型能夠從不符合當前語音聊天政策的資料集學習。這涉及為舊政策專門設置一個獨立的分類頭，使模型主幹能夠從舊資料集學習，同時不影響目標標籤或主要分類頭。


#### 經校準的模型，部署更輕鬆


使用分類模型時，需根據任務需求決定運作點並調整分類器的靈敏度。為簡化模型部署流程，我們針對語音聊天內容審查任務對模型輸出進行了校準。 我們從保留的測試資料集中估算分段線性轉換函數，並針對每個輸出頭和支援的語言分別進行調整。這些轉換函數在模型蒸餾過程中被應用，確保最終模型已原生校準。這消除了在推論階段進行後處理的必要性。


我們很高興能與社群分享這個新的[開源模型](https://github.com/Roblox/voice-safety-classifier) ，並期待在未來有新進展時與大家分享。
