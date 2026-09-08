* https://gemini.google.com/app/b0c052ce5cbdc06b
    * https://share.gemini.google/D0Mva6ZGYfgF


-----------------------------------------------------------------------------

# AI 版的瀑布(螺旋)模式

有了 AI 之後，傳統軟體生命週期（SDLC）的核心結構並沒有消失，但**每個階段的「動能」與「人機分工」發生了根本性的質變**。

整體流程從傳統的「流水線串行開發」，轉變為以 **「人類定義邊界，AI 負責極速實作，自動化鏈條持續驗證」** 為核心的微螺旋演進。

-----------------------------------------------------------------------------

### 各階段調整與重組比較

| 階段 | 傳統作法 (Traditional) | AI 時代作法 (AI-Augmented) | 調整重點與關鍵變革 |
| --- | --- | --- | --- |
| **1. 需求分析** | SA/PM 手動撰寫 PRD、訪談紀錄、繪製流程圖，耗時數日。 | **對話式邊界提煉：** 人類搜集真實痛點，AI 透過「逆向提問」幫忙補齊 Edge Cases 與沒想到的細節。 | 人類專注於**同理痛點與商業決策**；AI 負責將混亂訪談結構化為標準 User Stories。 |
| **2. 系統分析與設計** | SA 手繪 UML、畫 ER-Diagram、寫 Excel API 表格。 | **Spec-Driven 化：** 人類引導 AI 直接生成 Zod Schema、OpenAPI (Swagger)、BDD (Gherkin) 與 TypeScript Interfaces。 | **規格即代碼 (Spec is Code)**：產出的設計文件直接具備「可執行性」，能直接餵給 AI 寫程式。 |
| **3. 程式碼實作** | Coder 逐字編寫 Boilerplate、API 串接、UI 元件與 CRUD 邏輯。 | **Prompt 驅動生成 (Agentic Coding)：** 人類審核規格，AI 完成 80% 程式碼實作，工程師進行審查 (Code Review) 與微調。 | 人類從「寫代碼的建築工」轉變為**「審查代碼與架構的指揮官」**。 |
| **4. 測試 (Testing)** | QA 手寫測試案例、手動點擊 UI 測試、工程師手補 Unit Test。 | **契約驗證與自動生成：** AI 根據 Spec 自動補齊 Unit Test、Mock Data，並生成 E2E (Playwright) 腳本。 | 測試移至開發前/中段（Shift-Left）；人類僅需定義「驗證標準」，AI 負責極速生成與執行。 |
| **5. 除錯 (Debugging)** | 觀察 Log、打 Breakpoint、逐行排查、Google 搜尋錯誤訊息。 | **上下文診斷 (AI-Driven Debugging)：** 貼上 Stack Trace 或 Log，AI 讀取全局 Context 直接定位根因並提供 Fix PR。 | 除錯時間從數小時縮短至數分鐘，人類重點在於驗證修復邏輯是否帶來二次副作用。 |
| **6. 上線與維運 (CI/CD)** | DevOps 手寫 CI/CD 腳本、手動監控 Log、撰寫 Release Notes。 | **自動化自癒與監控：** AI 自動產出 Dockerfile / CI Pipeline、追蹤 Sentry/Log 並自動摘要故障排查報告。 | 上線流程極度自動化，部署風險大幅降低，釋放時間進行高頻小步快跑（Continuous Delivery）。 |

---

### 全流程重組後的新工作流 (The New Flow)

重組後的開發流程不再是傳統的一步一步往前走，而是形成一個「超高速回傳迴路」：

1. **Intent & Spec Phase (意圖與規格)：** 人類提出商業目標 ➔ 與 AI 對話探討邊界 ➔ 產出結構化 Spec（BDD / Schema）。
2. **Contract & Test First (契約與測試前置)：** AI 根據 Spec 自動生成 Test Cases 與 API Stub。
3. **Agentic Execution (AI 自動編寫)：** AI Agent（如 Cursor, Windsurf, Devin）根據 Spec 與測試代碼，自動填補實現邏輯，直到 Pass 所有測試。
4. **Human Review & Gatekeeping (人類審核與把關)：** 人類工程師審查安全性、架構一致性與效能。
5. **Instant Deploy & Feedback (即時部署與反饋)：** 自動化 CI/CD 上線，AI 自動追蹤運行 Log 並給出修正建議。

---

> **核心總結：**
> 調整後的軟體工程，將人類從繁重的**「重複性勞動（實作、除錯、寫測試）」**中解放出來，讓人類能 100% 聚焦在最需要人類智慧的兩端：**「前端的需求洞察」**與**「後端的品質與架構審查」**。

...

-----------------------------------------------------------------------------

## 1. 系統分析


```
【傳統需求分析焦點】                      【AI 時代需求分析焦點】
使用者說什麼 → 手工紀錄 → 畫圖寫文件    問對的問題 → 讓 AI 挖邊界 → 人類做商業裁決
 (焦點：產出完整的文件)                    (焦點：洞察真實痛點與商業價值)

```


-----------------------------------------------------------------------------

### 二、 分析方法的變革：對話式需求提煉與可執行規格 (Executable Specs)

需求分析的產出物，從「給人類閱讀的靜態 Word/PDF」，變成了「給人類審核、同時能直接給 AI 執行的結構化規格」。

```
傳統 SRS 檔 (Word/PDF) ──[需人工看文檔寫程式]──> 容易產生理解偏差
AI 時代 Spec (BDD/Schema) ──[人類審核 ➔ AI 直接執行]──> 0 轉譯損耗

```

...

### 三、 需求分析流程的具體對比 (Before vs. After)

| 分析階段 | 傳統需求分析 (Traditional Requirements Analysis) | AI 導向需求分析 (AI-Augmented Requirements Analysis) |
| --- | --- | --- |
| **1. 需求收集** | 開會記錄、發放問卷、整理逐字稿，手動歸納需求列表。 | **Context 採集 + AI 摘要：** 人類專注於實地觀察與同理心訪談，AI 負責將龐大的語音/文字紀錄結構化。 |
| **2. 邏輯拆解與邊界分析** | 靠 SA 個人記憶與經驗，開會討論邊界狀況（容易遺漏）。 | **AI 逆向質疑 (Red Teaming)：** SA 與 AI 進行對抗式問答，AI 主動列出並發、過期、權限等極端情境由 SA 裁決。 |
| **3. 規格產出** | 耗時數日編寫數十頁的 SRS 文件、動態圖、API 欄位表。 | **極速生成 Executable Spec：** AI 一鍵生成 OpenAPI、Zod Schema 與 BDD Gherkin 規格檔。 |
| **4. 需求確認與變更** | 客戶看文件簽字；開發後期發現需求錯了，變更成本極高。 | **即時原型反饋 (Prototyping Loop)：** SA 直接用 AI 生成動態 UI/流程與客戶確認，變更需求只需重調 Spec。 |

---



## 2. 系統分析與設計

在 AI 時代（尤其是 Large Language Models 與 Code Generation Tools 爆發後），軟體開發的產出瓶頸已從「如何編寫程式碼（Implementation）」**轉移到**「如何精確定義問題與系統邊界（Specification & Alignment）」。


```
傳統 SA/SD 產物                AI 時代 SA/SD 產物
┌───────────────────────┐      ┌──────────────────────────────────────────────┐
│ • 系統需求規格書 (SRS) │  ➔   │ • AI Context Pack (Glossary, Business Rules) │
│ • 靜態 UML 繪圖檔     │      │ • Machine-Readable BDD Gherkin Feature Files │
│ • Excel 欄位對照表    │      │ • OpenAPI 3.0 / Zod Schema (可自動驗證)     │
│ • UI Wireframe 截圖   │      │ • .cursorrules / CLAUDE.md 專案規範檔        │
└───────────────────────┘      └──────────────────────────────────────────────┘

```

SA/SD 不再是寫文件的文員，而是系統的**邏輯建築師 (Logic Architect)**。能夠掌握「將混亂業務轉化為精準 AI 脈絡（Context Pack）」的 SA/SD，將成為軟體工程團隊中最具核心價值的靈魂人物。

-----------------------------------------------------------------------------

## 3. 程式碼實作

```
傳統模式 (Traditional)               Prompt 驅動生成 (Agentic Coding)
┌─────────────────────────┐         ┌─────────────────────────┐
│ • 70% 時間：打字寫 Code  │  ➔      │ • 20% 時間：編寫 Prompt 與 Context
│ • 20% 時間：查文件/Debug│         │ • 50% 時間：Code Review 與測試驗證
│ • 10% 時間：討論與規劃  │         │ • 30% 時間：架構設計與安全性防禦
└─────────────────────────┘         └─────────────────────────┘
```


## 4. 測試 (Testing)

```
傳統測試金字塔                           AI 時代測試金字塔
         /  E2E  \                             /  E2E  \  <-- AI 視覺代理/Agent 自動跑
        / Integration \                       / Integration \ <-- 重點防守！(跨模組/資安/效能)
       /   Unit Test   \                     /   Unit Test   \ <-- 成本降至趨近於 0 (AI 全自動生成)
      -------------------                   -------------------
```

## 5. 除錯 (Debugging)

```
【傳統除錯 Paradigm】
收到 Bug Report ➔ 看 Stack Trace ➔ 下 Breakpoint 逐步執行 ➔ 猜測問題點 ➔ 手動改 Code ➔ 重新測試

【AI 時代除錯 Paradigm】
複製 Error Log / 描述現象 ➔ AI 分析根因 (Root Cause Analysis) ➔ AI 重現並生成 Fix ➔ 執行測試驗證 ➔ Review 並套用
```

## 6. 上線與維運 (CI/CD)

這部分我們的焦點先放在沒有 LLM 呼叫的

1.  AI-Augmented Software Engineering（AI 增強型軟體工程）

```
【傳統開發流程】
  [ 寫 Code (慢) ] ──> [ PR / Code Review (中) ] ──> [ CI 測試與部署 (快) ]

  【AI 輔助傳統開發】
  [ 寫 Code (極快) ] ──> ⚠️ [ PR 爆炸 / 審查瓶頸 (極慢) ] ──> ⚠️ [ CI 壓力與測試涵蓋率考量 ]
```

當我們討論的不是「包含 AI/LLM 的軟體系統」，而是「工程師使用 AI 工具（如 GitHub Copilot, Cursor, ChatGPT, Claude）來編寫傳統程式碼（沒有 LLM 參與運行）」**時，CI/CD 與維運（DevOps）的重點會從「維護不確定性的模型」轉變為**「處理爆炸式增長的程式碼產出量與潛在的品質/安全風險」。

這種模式常被称为 **AI-Augmented Software Engineering（AI 增強型軟體工程）**。以下詳細分析在 CI/CD 與維運層面發生的核心改變：

---

### 一、 CI/CD 核心痛點轉變：從「寫程式 bottleneck」到「審查與驗證 bottleneck」

在沒有 AI 之前，開發流程的核心瓶頸通常在於「編寫程式碼的速度」；有了 AI 輔助後，寫程式碼的速度提升了 3~5 倍，**瓶頸迅速轉移到了「CI 驗證、Code Review 與安全測試」**。

```
  【傳統開發流程】
  [ 寫 Code (慢) ] ──> [ PR / Code Review (中) ] ──> [ CI 測試與部署 (快) ]

  【AI 輔助傳統開發】
  [ 寫 Code (極快) ] ──> ⚠️ [ PR 爆炸 / 審查瓶頸 (極慢) ] ──> ⚠️ [ CI 壓力與測試涵蓋率考量 ]

```


| 維度 | 傳統人手寫程式時代 | AI 輔助寫傳統程式時代 |
| --- | --- | --- |
| **開發瓶頸** | 寫程式的速度慢 | PR 審查與測試驗證的速度跟不上 |
| **CI 重點** | 基本單元測試與自動建置 | **AI 輔助 Code Review、套件供應鏈安全、強化的 SAST** |
| **測試生成** | 工程師手寫測試，經常遺漏 | **AI 自動產生邊界測試與單元測試**，CI 強制覆蓋率 |
| **CD 部署** | 定期發布（週/月），人工監控 | **高頻率發布、Feature Flag、自動金絲雀部署與自動 Rollback** |
| **維運重點** | 人類看 Dashboards 排查 Log | **AIOps 自動收斂告警、自動找出 Root Cause** |

一句話總結：**AI 幫你把寫 Code 的速度拉滿，但 CI/CD 必須變成最嚴格的守門人，防止「壞代碼以光速衝上生產環境」。**


# AI 版的敏捷模式

## 1. 概論

**如果寫 Code 變成了「光速生成」，但我們卻在 CI/CD 設定了極度厚重的審查、安全大壩與門檻，這本質上就是在用「傳統/瀑布式（Waterfall）的嚴格檢驗」去硬塞 AI 帶來的產能。**

AI 時代**確實非常需要一套全新、專屬的敏捷方法論**。傳統敏捷（Agile/Scrum）是以「人類開發者的產出速率（Velocity）」為核心設計的，但 AI 完全打破了這個假設。

這套專為 AI 時代設計的敏捷方法論，正在被產業定義為 **AI-Native Agile（原生 AI 敏捷）** 或 **Continuous Evolution Methodology（持續演進法）**，其核心改變包含以下幾個維度：

---

### 一、 核心哲學的轉變：從「Scrum 迭代」到「微微流（Micro-Flows）」

傳統 Scrum 以 2 週為一個 Sprint，因為人類需要時間理解需求、寫 Code、測試。但在 AI 時代，2 週的 Sprint 太慢了。

```
  【傳統 Scrum 敏捷】
  [ 2 週 Sprint 規劃 ] ──> [ 開發 (7天) ] ──> [ CI/CD & Review (3天) ] ──> [ Demo / 回顧 ]

  【AI 原生敏捷 (Micro-Flows)】
  [ 需求 (小時級) ] ──> [ AI 生成 + 人類導引 ] ──> [ 實時灰度測試 ] ──> [ 用戶反饋 (天級) ]

```

* **無 Sprint（Sprint-less）：** 敏捷單位從「週」縮短到「小時」甚至「分鐘」。
* **從「寫程式」轉向「規格即程式（Spec as Code）」：** 開發者的角色從「撰寫者」變成「Product Architect / Evaluator」。你的敏捷 Backlog 裡寫的不再是「刻出某個畫面」，而是「定義好這項功能的 Input/Output 邊界條件與 Evals（評估指標）」，交由 AI 實作。

---

### 二、 AI 敏捷方法論的 4 大核心支柱

為了避免回到傳統瀑布模式的「審查大壩」，AI 敏捷提出了幾種全新的運作機制：

#### 1. Shift-Left Everything & Test-Driven-Generation (TDG)

傳統瀑布是「先寫 Code，後置審查大壩」；AI 敏捷強調「測試前置」：

* **先寫 Evals / 測試，再讓 AI 寫 Code：** 在工程師下 Prompt 或讓 AI Agent 執行任務前，先定義好測試案例（Test Cases）。
* AI 在 Sandbox 裡自己寫 Code、自己跑測試，**直到 Pass 所有的 Test Cases 才會拉 PR**。這樣審查大壩被「分散並前置」到了開發當下，CI 就不會積壓一堆壞程式碼。

#### 2. Human-in-the-Loop (HITL) 到 Human-on-the-Loop (HOTL)

傳統 Code Review 是 **HITL（每行程式碼都要人類手動簽核）**，這導致了審查瓶頸。
AI 敏捷改採用 **HOTL（人類在旁監督/例外管理）**：

* **低風險變更（Low-Risk）：** 覆蓋率達標、安全掃描通過的小修補，CI/CD 自動 Merge 並上線（Fully Automated）。
* **高風險變更（High-Risk）：** 涉及架構、金融邏輯、核心 API 的變更，才觸發人類 Review。
* 人類不再逐行讀 Code，而是重點審查 **架構設計（Architecture）、商業邏輯（Business Logic） 與 系統介面（Interfaces）**。

#### 3. 實時用戶反饋代替預防性審查（Shift-Right & Observability）

傳統敏捷靠 Sprint Review 拿反饋；AI 敏捷認為「AI 寫的程式碼既然成本極低」，不如**直接上線讓小部分真實流量驗證**：

* 比起在 CI 階段花 3 天做死板的靜態測試，不如花 3 分鐘部署到 Canary 環境，靠 AIOps 監控真實使用者的 Error Rate 與 Latency。
* **用「極速回滾（Instant Rollback）」代替「完美的預防」**，這才是敏捷的終極體現。

#### 4. 漸進式架構（Evolutionary Architecture）

AI 非常擅長在局部寫出優質程式碼，但缺乏對「長期系統架構」的宏觀視野。
AI 敏捷方法論強調：**人類工程師專注於架構邊界（Domain-Driven Design / DDD）**，將系統拆解為極度模組化的微服務或套件，AI 則在限定的邊界內做極速開發。邊界清晰，AI 就不會破壞整體系統。

---

### 三、 傳統敏捷 vs. AI 原生敏捷 對比

| 敏捷維度 | 傳統 Scrum 敏捷 | AI 原生敏捷 (AI-Native Agile) |
| --- | --- | --- |
| **最小交付單元** | User Story (需要數天實作) | **Micro-Prompt / Spec (小時級實作)** |
| **開發速度瓶頸** | 工程師敲鍵盤的速度 | **需求定義的清晰度與架構邊界** |
| **品質保證 (QA)** | 人類寫 Test Case + 審查大壩 | **Test-Driven-Generation (TDG) + 自動化 Evals** |
| **Code Review** | 人類逐行閱讀 (Line-by-line) | **例外管理 (HOTL) + 架構級審查** |
| **團隊角色演變** | 開發者、測試員、Scrum Master | **AI 導引師 (AI Pilots)、系統架構師、Evals 設計師** |

---

### 結論

你看到的「AI 瀑布模式」確實是目前許多企業轉型時的**陣痛期**——用上了 AI 工具，卻沿用傳統嚴格的檢驗流程，導致「開發 5 分鐘，審查 5 天」。

真正的 **AI 時代敏捷**，重點在於**把 CI/CD 的測試與安全驗證完全「內嵌」進 AI 生成的過程中（TDG）**，並結合**例外管理與漸進式灰度部署**。人類工程師的角色將徹底從「水泥工（寫 Code）」轉變為「建築師（架構與規格）」與「品酒師（評估與驗證）」。

## 2. 測試驅動生成 (TDG)

傳統 TDD 是「人類寫測試 $\rightarrow$ 人類寫 Code $\rightarrow$ 人類修復」；而 TDG 則是 「人類（或 AI）定義規格與測試 $\rightarrow$ AI Agent 在隔離沙盒（Sandbox）中自動生成 Code、執行測試、閉環修復（Self-Correction Loop） $\rightarrow$ 綠燈後提交 PR」。

```
               ┌──────────────────────────────────────────┐
               ▼                                          │
    [ 1. Context Read ] ──> [ 2. Code Gen / Patch ]       │
            ▲                         │                   │ (Fail)
            │                         ▼                   │
    [ 4. Error Parsing ] <─── [ 3. Run Test in Sandbox ] ─┘
            │
            └─────────────── (Pass) ───> [ 5. Complete PR ]
```

# 人類與 AI 協作？

以上的想法，通常是人類不介入 AI 寫程式的過程，只負責規格與驗收

但是這樣真的行得通嗎？

有時我們只想要修改某個功能，但 AI 可能到處改，而非限縮在該功能上

人類和 AI 如何協作呢？

將人類與 AI 的協作模式拆解為四個層級（Level 1 至 Level 4），能讓團隊根據「風險高低」**與**「系統複雜度」彈性切換模式，而不是一昧追求全自動。

以下為各協作模式的適用場景、核心價值與決策矩陣：

---

| 評估維度 | 系統複雜度 / 風險 | AI 的自主程度 | 適合採用的協作模式 | 典型任務範例 |
| --- | --- | --- | --- | --- |
| **高風險 / 高複雜** | 老舊核心系統、金融/資安模組 | 0% ~ 20% | **Level 1 (In-line Copilot)** | 修改支付流轉邏輯、記憶體洩漏排查 |
| **中高風險 / 中高複雜** | 跨模組重構、架構異動 | 20% ~ 50% | **Level 2 (Architect Pair)** | 資料庫 Schema 變更、系統設計討論 |
| **中低風險 / 邊界清晰** | 日常 Feature 開發、Bug 修復 | 50% ~ 80% | **Level 3 (Scoped Agent)** | 新增 API 介面、寫單元測試、修復前端 Bug |
| **低風險 / 獨立隔離** | 獨立小元件、例行維護 | 80% ~ 100% | **Level 4 (Full Auto)** | 依賴套件升級、0 到 1 POC 專案搭建 |
