---
schema_version: "1.0.0"
document_id: "20b34af8967addcdd8a76336e98269cdca6a3376b4f4162365721f9fefcc5a0e"
company_key: "yc-namecard-ai"
company: "Namecard.ai"
source_id: "yc-namecard-ai-rss-f3518cbae1fb"
canonical_url: "https://chainsigtweb3.medium.com/steaker%E9%87%91%E6%B5%81%E8%BF%BD%E8%B9%A4%E5%A0%B1%E5%91%8A-45da6ed7951e"
published_at: "2022-12-13T12:25:31+00:00"
first_seen_at: "2026-08-10T00:54:06.811990+00:00"
fetched_at: "2026-08-20T03:26:10.503825+00:00"
content_hash: "sha256:e2c13985c66a917ccee8ecd9a17a1366cc507d6d8772cd09bdb2bc9d87b2a4da"
---

# Steaker金流追蹤報告

Thomas Lefebvre/Unsplash


> 免責聲明:
> 本文資料來源為 EtherScan、 TronScan 和 BscScan。
> 筆者同時有使用調查工具 Breadcrumbs 進行資料分析。
> 本文不具備鑑識證據的效力，目的是協助讀者了解Steaker的鏈上金流情形。


更新：2022年12月15日18時30分
1. 更新數據內容。
2. 新增Steaker提款錢包（用戶出金）的金流來源
3. 加強說明受FTX影響而無法提領金額的相關鏈上資訊。


### 前言


台灣的加密貨幣投資管理平台 Steaker 由於受到 FTX 平台倒閉事件影響，於台灣時間2022年11月11日晚間7點停止了部分投資用戶出金。


這個不幸的事件影響著許多用戶，也讓受災戶們不禁疑惑自己的錢去了哪裡?Steaker是如何管理資產的？


我們的報告將會先聚焦於Steaker的穩定幣金流，包含等值 1 美金的 USDT 、 USDC 、 BUSD 及 DAI 。


> 什麼是穩定幣?
> 穩定幣是一種價值錨定法幣的加密貨幣，最常見的穩定幣大多錨定美金。


藉由維權群中 Steaker 用戶們好意提供的 Steaker 錢包地址及鏈上資訊分析，我們整理了以下 3 個重要的發現：


### 1. Steaker 疏於風險管理，高達 83.1% 的用戶資金直接存入 FTX


從 Steaker 用戶提供的資料，我們成功辨別下列錢包地址（下稱「存款錢包」）屬於 Steaker 所有，並且我們分析推測這些熱錢包是用於收受及管理用戶的入金資金：
0x5425A0f2633B60B9F2400D82ed6596b5A4B41523BSC (ETH) 0xf4Ad3ba2be7089092AbB55766E07953a50952139 (BSC)
TAXBLYJUYDokkUQLscuSfAAh6VGK1HPCV1 (TRX)


> 什麼是熱錢包?
> 熱錢包通常是服務提供商的錢包地址，用於集中管理用戶的資金。藉此服務提供商可以進行批次交易以達成節省交易手續費（礦工費）的目的。


藉由交易的紀錄、金額、次數及模式，我們推測下列錢包地址（下稱「提款錢包」）是 Steaker 用於處理用戶出金請求的錢包地址：
0x9680Eb0a1eca7a81AbAA0e923Decb3C0b4FA0E2a(ETH)
0x9680eb0a1eca7a81abaa0e923decb3c0b4fa0e2a(BSC)
TGXoNjiQrVUsNLaZQu3CakkxDyPAAmkryL (TRX)


我們收集了鏈上的交易數據發現自 2021 年 2 月起共 4824 萬美元存入了存款錢包（用戶存款），另外有 4016 萬美元存入了提款錢包（用戶提款）。總存款與總提款的差額大約為 808 萬美元，這同時應是 Steaker 在不計利息的情形下積欠使用者的額度。


Steaker存款錢包及提款錢包的入金情形。


用戶存款資金中，約 4824 萬美元的用戶資金中，約 4018 萬美元 (83.1%) 存入了 FTX ，約 661 萬美元 (13.6%) 存入了提款錢包，約 145 萬美元 (3%) 存入了幣安，剩餘 5 萬多美元 (0.1%) 的資金存入了 Steaker 自己控制的多重簽名錢包中。


> 什麼是多重簽名錢包?
> 多重簽名錢包顧名思義是需要複數不同的私鑰/簽章才能存取的錢包，這類錢包有別於單一私鑰的錢包有更高的安全性。


扣除轉入提款錢包的資金，幾乎所有資金都是進入 FTXETH 存款錢包 0x5…BSC 的主要金流 (Source: Breadcrumbs)


用戶提款資金中，約 4016 萬美元的用戶資金中，約 2764 萬美元 (68.8%) 來自 FTX ，約 567 萬美元 (14.1%) 來自幣安，約 661 萬美元 (16.5%) 來自存款錢包，約 22 萬多美元 (0.6%) 的資金來自其他錢包中。


提款錢包的資產來源


統整鏈上資訊，我們可以發現2點：


- 自 2021 年 2 月起 **** 4018 萬美元的用戶存款曾進入了 FTX， 並有約 2764 萬美元的資金從 FTX 轉入 Steaker 提供用戶出金的提款錢包， **差額約 1253 萬美元** (FTX帳戶理論的應有餘額，不考慮FTX提供的利息及Steaker後續出金)。
- 存入 Steaker 幣安帳戶的資金 145 萬美元小於從幣安流入提款錢包的資金 567 萬美元，代表 **Steaker 應曾從 FTX 或其他渠道轉帳資金到幣安至少 422 萬美元** 。


根據 2022 年 11 月 13 日 Steaker 發布的公告， Steaker 的用戶資產約有 1060 萬美元受到 FTX 事件的影響。


如果我們把Steaker在FTX的理論餘額 — 出金至幣安的資金 + FTX的利息，鏈上資訊似乎可以佐證 Steaker 聲稱受 FTX 影響的用戶資產數額尚屬合理。


同時用Steaker聲稱的 FTX 資產餘額 1060 萬美元扣除總存款與總提款的差額808 萬美元， Steaker 在過去兩年至少有 252 萬美元以上的獲利。


Steaker 公告的 FTX 資產截圖 (Source: Steaker) 及 Steaker與FTX的鏈上互動數據


### 2. Steaker 的幣安錢包中，可能存在創辦人混用客戶資金及個人資金的情形


鏈上資訊顯示幣安錢包 0x901532F40941962BE4525497376e40513E0E8d16 有收受 Steaker 創辦人私人錢包的資金及 Steaker 用戶的資金，這意味著 Steaker 在管理用戶資產有混用資金的情形，不過這個情形是到 FTX 事件後才出現。


由於在台灣時間 2022 年 11 月 9 日 19:03:59 之前，這個錢包都只有與 Steaker 創辦人的私人錢包有過交易，因此該錢包地址可能是創辦人私人幣安帳戶錢包。


自從 FTX 停止用戶出金後， 16 萬的 USDC 就從 Steaker 的存款錢包轉帳至 0x9…d16 的幣安錢包。


幣安帳號0x9…d16中同時有來自Steaker創辦人錢包及Steaker用戶的資金 (Source: EtherScan)


### 3. FTX 破產後剩餘的資金跑哪去了？


Steaker 自 2022 年 11 月 09 日到 2022 年 12 月 11 日間，從存款錢包中大約提領 82 萬美元的用戶資金，其中：
- 16 萬美元存入了可能為創辦人私人的幣安帳戶錢包 0x9…d16
- 3 萬美元存入了另一個幣安錢包 0x7…649 (該地址並無創辦人私人錢包資金混用之情形）
- 5 萬多美金存入了 Steaker 自己控制的多重簽名錢包中
- 剩餘 57 萬多美元存入了提款錢包中


如果您喜歡Chainsight的文章，歡迎關注或聯繫我們：


- [Twitter](https://twitter.com/ChainsightWeb3)
- [Facebook](https://www.facebook.com/ChainsightOfficial)
- [Discord](https://discord.gg/sfABMBCN)
- [Mastodon](https://cryptodon.lol/@ChainsightWeb3)
- Email


我們提供下列服務，如果有需要歡迎隨時聯繫：


- [Web3Check](https://www.web3check.com/) **—** 加密貨幣/NFT詐騙偵測瀏覽器擴充外掛（開放測試候補名單中）
- **Chainsight調查服務** — 金流追蹤及資產取回諮詢
