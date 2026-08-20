---
schema_version: "1.0.0"
document_id: "2a16582fd88ff2fcfe93419aa5e04efc32d4c067b3f08b51bfe619f52496927d"
company_key: "roblox-corporation-class-a-common-stock"
company: "Roblox Corporation"
source_id: "roblox-corporation-class-a-common-stock-news-import-fcd4c7b68777"
canonical_url: "https://about.roblox.com/zh-hant/newsroom/2025/04/roblox-creator-analytics"
published_at: "2025-04-15T16:00:00+00:00"
first_seen_at: "2026-07-23T23:14:36.468733+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:dda0aa9916fe956706efe237b08629fb4ade7339007afb741c79402783328501"
---

# Roblox 如何運用 Theta Sketches 擴展創作者分析功能

分享


[工程](https://about.roblox.com/zh-hant/newsroom?filter=%E5%B7%A5%E7%A8%8B)


# Roblox 如何運用 Theta Sketches 擴展創作者分析功能


發佈者


威利斯·甘迺迪


發佈日期


2025年4月15日


分析工具對於當今的即時多人線上遊戲至關重要。在 Roblox，我們致力於開發衡量工具，以協助創作者蓬勃發展。我們免費且開箱即用的分析工具，能讓創作者即時掌握其體驗的成長、用戶獲取及留存狀況，協助他們將成功機率最大化。


建構數百萬 Roblox 創作者所倚賴的即時分析系統，是一項重大挑戰。為此，我們已優化分析查詢引擎，使一個 120 核心的處理叢集能夠處理來自約 30 萬名每日訪客的 600 萬筆查詢，並存取 86 TB 的資料。 我們解決方案的核心是一套線上分析處理 (OLAP) 資料庫，我們選擇它正是因為其可擴展性以及與近似演算法的整合能力。透過結合資料彙總技術，加上[HyperLogLog](https://datasketches.apache.org/docs/HLL/HllSketches.html) 和[Theta](https://datasketches.apache.org/docs/Theta/ThetaSketches.html) Sketch 演算法，我們為數百萬個 Roblox 體驗提供分析服務¹。


##### OLAP 分析入門指南


查詢的資料量越大，產生結果所需的時間就越長。當我們能夠減少所需資料量並加速分析流程時，創作者就能從他們的行動中獲得近乎即時的洞察。我們採用的技術包括：


1. **列式儲存：** OLAP 系統[Druid](https://druid.apache.org/) 僅讀取必要的欄位。
2. **分區與排序篩選器：** OLAP 僅讀取直接索引至所需資料區塊的相關檔案。
3. **匯總 (Rollup)：** OLAP 透過常見的分組方式對事件進行部分彙總。


其中，匯總功能特別能讓 OLAP 在大型 SQL 查詢引擎（如 Spark 或 Presto，延遲達數十秒）與點查詢或有限 SQL（通常提供完全彙總的資料）之間取得平衡。透過匯總，查詢將以維度分組作為索引，從而大幅降低總行數的基數。 當面對數十億甚至數兆筆原始事件時，將其彙總為數百萬個群組，並以低於一秒的延遲進行彙總，往往能大幅提升效率。例如：


雖然匯總技術具備上述提到的簡化優勢，但某些指標卻難以透過此方式處理，包括需要對原始資料進行全表排序的查詢，例如唯一計數、百分位數及頻率查詢。


所幸，我們可以透過特定技術來克服這些限制，這些技術會基於儲存完整資料集樣本的複雜資料結構，返回具有統計界限的近似結果。這些資料結構專為匯總技術設計，並能透過合併運算將兩個不同計數結合起來，類似於將兩個數字相加。


##### 解析 Roblox 分析工作負載


在 Roblox，我們為創作者提供一個[集中式儀表板](https://create.roblox.com/docs/production/analytics/analytics-dashboard) ，讓他們能夠在此查閱最重要的洞察資訊。這些資訊包括：


- 參與度：每日活躍用戶 (DAU)、每月活躍用戶 (MAU)、留存率及流量漏斗
- 變現：營收、平均每用戶營收、銷售額及經濟數據
- 用戶獲取數據
- 縮圖個人化與實驗結果
- [首頁推薦](https://devforum.roblox.com/t/boost-your-discovery-with-the-improved-recommended-for-you-algorithm-and-analytics-for-creators/3587441) 分析
- 更多功能即將推出。


在建構系統時，我們著重於優化最壞情況下的查詢。這類查詢通常涉及大量唯一計數（>1 億 UUID），例如熱門服務的月活躍用戶數（MAU），此類查詢可能導致載入時間從數秒延長至數分鐘。 我們建置了一個統計近似框架，以確保查詢延遲控制在兩秒內。我們採用了[業界標準函式庫](https://datasketches.apache.org/docs/Background/TheChallenge.html) 中的 HyperLogLog 和 Theta Sketch 技術，將最壞情況下的查詢讀取量從超過 1 億筆資料行，大幅降低至約 500 萬筆。


##### 查詢引擎的選擇與優化


在選定 OLAP 解決方案後，我們載入了六個月的互動數據，並對系統的效能極限進行了壓力測試。 憑藉約 100 個處理核心與 500 GB 記憶體，我們發現能在兩秒內隨機合併 500 萬個二進位 Theta Sketch 物件（總計約 100 MB）。此測試是在冷啟動查詢環境下進行，資料直接從磁碟讀取且未存取任何記憶體快取。至於 Clickhouse 和 Duckdb 預設提供的 S3 讀取等網路儲存選項，其效能表現則明顯較差。


##### 克服效能挑戰


在最後一輪的生產環境模擬測試中，我們發現了一個重大挑戰：當我們將查詢模式從單一大型查詢轉換為每日聚合模式後，MAU 查詢的效能需要加以強化。這些查詢對於我們的創作者分析視覺化至關重要。


我們發現，查詢結構對 OLAP 解決方案的底層效能影響甚鉅。具有多重執行階段的標準查詢（例如嵌套的「GROUP BY」陳述式2），往往會將大量工作負載轉移至輕量級中介節點上。


這是一個典型的巨量資料問題：查詢的一部分最終會在關鍵的小型服務節點上執行。我們原本預期近似資料結構能像簡單的計數或求和那樣運作，但發現它們的實際行為卻大相逕庭。


下圖說明了這個問題。它展示了我們的歷史節點如何進行部分彙總：先針對每一天建立一個 Theta Sketch，然後將資料推送回中介節點。接著，中介節點試圖將每個龐大的每日草圖合併為單一的每日月度值。 對於 30 天的 MAU 數據，這意味著要在中介伺服器上合併 1,800 個最大尺寸的 Theta Sketches，導致查詢速度變慢、容易失敗，並佔用中介伺服器的 CPU 資源。


我們的解決方案是減少大型歷史工作節點的數量來執行 OLAP，以最大化依賴近似查詢的資料來源之資料局部性。實際上，這將原本可能需要處理超過 100 MB 資料的合併操作，重新推回至我們的歷史節點上。


為在 SQL 中實現此目標，我們採用[內聯聯結](https://github.com/Roblox/data-tools/blob/main/AnalyticsForEveryone/druid_workflow/sql/query/mau_query_example.sql) （[inline join）](https://github.com/Roblox/data-tools/blob/main/AnalyticsForEveryone/druid_workflow/sql/query/mau_query_example.sql) 讓查詢將必要資訊傳遞至歷史節點，並預先準備一份包含內聯結果日期的查詢清單。每個結果日期隨後可從歷史節點區段中匯集相關資料。資料接著傳回至中介伺服器（broker），在那裡結果會迅速合併為單一的「結果日期對應指標資料」映射，如下所示。


這項優化對大規模查詢的效能產生了顯著影響。以下圖表所示，針對某主要服務的各國月活躍用戶（MAU）分佈查詢，平均查詢效能提升了 5 倍（從 17.53 秒縮短至 3.23 秒）。此外，中繼伺服器的 CPU 耗時也減少了 50 倍（從 16.83 秒降至 0.34 秒）。


雖然結果因情況而異，但這凸顯了謹慎處理複雜操作（例如合併數百萬個草圖）的重要性。若將這些操作視為等同於簡單的聚合，可能會導致嚴重的效能問題，特別是在常見「最後一哩」客戶端聚合的系統中。


#### 彙總與理論性 Theta 立方體


我們平台上的平均分析查詢通常僅包含極少數的細分維度，且極少涉及高基數維度（例如國家）。基於此，我們決定複製資料，建立一個維度較少的彙總表，此表足以滿足超過 98%³ 的查詢需求。在平均查詢上，其效能提升了四倍。


我們還探索了 theta 立方體，這是一種透過近似集合交集來彌合基本匯總表與完全原始表之間差距的通用方法。此方法解決了一項根本限制：當查詢需要觸及多層高基數維度時，匯總表便會失去其優勢。這是因為每個維度都會導致匯總基數隨 ∏dim（維度乘積）而增加。


我們設計了一套系統，能根據具有基數上限的共同維度群組進行彙總，從而允許對群組內的任何項目執行具備彙總效能的查詢。接著，當需要跨群組尋找維度組合時，我們會嘗試在集合間執行近似 join4 運算，並回傳指標結果及誤差估計值。若查詢的預估誤差過高，則會將其轉發至原始資料表，在那裡，眾多篩選條件應能實現大規模的推下優化。


這種 theta 立方體方法會切換維度，導致行數採用 ∑dim（維度總和）展開式，而非 ∏dim 展開式。當然，這可能會犧牲精確度，而這種動態變化與兩個維度群組之間的重疊大小5 成正比。其根本原因直接與 Theta Sketches 如何儲存底部 K 式排序清單有關，這種方式會最大化兩個具有高度內在重疊之集合之間的碰撞。


由於我們能快速計算此錯誤率，這也成為一個強有力的訊號，表明讀取原始資料表很可能具有良好的效能。在重疊資料相對於並集而言較小的情況下（例如德國的日語使用者），原始資料表中的大量資料行將會被過濾掉。這將導致高效的下推優化。 一個同時運用維度群組、近似聯結以及基於錯誤率的原始資料表讀取的系統，將能真正最大化近似友善查詢的匯總效能。


對於 Roblox 而言，此解決方案將更適用於我們下一階段的規模擴展——例如動態漏斗分析或自訂事件分析——而我們當前的簡易匯總副本則足以滿足現階段的需求。


##### 打造自助服務平台


隨著中介軟體的優化完成，我們轉而著手開發工具，用於導入並查詢新增至 OLAP 解決方案的資料集。我們為資料摘要函式建置了一個開源的[Spark 與 Trino UDAF](https://github.com/Roblox/data-tools) 函式庫，讓 Spark 能使用與 OLAP6 相同的二進位資料摘要格式。此舉使大部分運算工作負載維持在 Spark 環境中，並協助在 Roblox 內部標準化近似運算，對於特定資料集而言，這可能使運算成本降低多達 80%。


我們透過對批次工作排程器的內部擴充功能簡化了資料導入流程，並定義了一種[DataFrame 風格的 API](https://github.com/Roblox/data-tools/blob/main/AnalyticsForEveryone/druid_workflow/cube/schemas/Engagement.js) ，引導開發人員確定明確的度量和維度，從而降低開放式查詢的影響。我們還開源了一些[範例工作流程](https://github.com/Roblox/data-tools/tree/main/AnalyticsForEveryone/druid_workflow) ，展示如何在 OLAP 中載入和查詢這些資料。


經過優化的分析資料集現已為我們的創作者提供深入的洞察。這些優化措施使平均效能提升 4 倍，最差情況下的效能更提升 50 倍。這套自助服務平台讓我們的創作者分析團隊能夠持續為開發者迭代新的資料集。我們非常期待看到各規模的開發者運用這些工具，在 Roblox 上打造令人驚嘆的體驗。


1 根據過去 60 天內曾有過任何存取
行為的唯一用戶群計算得出 2 類似此簡易[MAU 查詢](https://github.com/Roblox/data-tools/blob/main/AnalyticsForEveryone/druid_workflow/sql/query/mau_query_example.sql)
3 結果來自 2025
年 3 月 21 日至 28 日 4 執行方式如下：SELECT c.experience_id, c.country, p.platform, THETA_INTERSECT(c.user_theta, p.user_theta) from (select experience_id, country, user_theta from theta_cube where agg_level = country) c union (select experience_id, platform, user_theta from theta_cube where agg_level = platform) p
5[https://datasketches.apache.org/docs/Theta/ThetaSketchSetOpsAccuracy.html](https://datasketches.apache.org/docs/Theta/ThetaSketchSetOpsAccuracy.html)
6 透過 Druid SQL 函數` COMPLEX_DECODE_BASE64('HLLSketch', sketch_col_name`` )` .
