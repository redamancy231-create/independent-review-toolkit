# Independent Review Toolkit · 獨立審查工具包

> **English**: A field-tested protocol for multi-model review of AI-generated content. Derived from 50+ review rounds across 5 LLM backends in the [AI Collaboration Framework](https://github.com/redamancy231-create/ai-collaboration-framework) project. Includes a step-by-step SOP, copy-paste prompt templates, adversarial challenge framework, and annotated examples. **CC BY 4.0**.

**語言**：簡體中文（核心文件） / English abstract available（英文摘要，核心文件和 prompt 模板為中文）  
**來源**：提煉自 [AI協作專案全生命週期框架](https://github.com/redamancy231-create/ai-collaboration-framework) §9.2 + 50+ 輪實戰審查  
**成熟度**：SOP 核心流程在源專案中經多後端驗證；本工具包自身的提取/改編過程見下方驗證狀態

> ⚠️ **驗證狀態**：本倉庫 v2.0.1 初版由 **DeepSeek-V4-Pro 單後端**完成提取和改編。已通過 Codex GPT-5.5 異後端獨立審查鏈閉合：
> - **R1**（2026-07-01）：終判 Major，發現 16 項（CRITICAL×3, MAJOR×10, MODERATE×2, MINOR×1）→ 全部修復
> - **R2**（2026-07-01）：終判 **PASS**，16/16 全部閉合 ✅
> 
> 審查報告見 `_reviews/`。工具包方法論（SOP §1-§12）在源專案中經 50+ 輪實戰驗證；提取/改編/撰寫品質經本輪異後端審查確認。

---

## 這是什麼

當你用 AI 產出了一份文件/程式碼/分析結論，你想知道：**"它真的沒問題嗎？"**

讓同一個 AI 檢查自己的產出 = 自我審查（不可靠）。找一個**不同後端**的 AI，在**完全隔離**的環境下審查 = 獨立審查。

這個工具包提供一套完整的操作協議，讓你能在 **5 分鐘內啟動第一次獨立審查**。

### 核心信念

- **獨立性 = 不同後端 × 上下文隔離**（缺一不可）
- **評分必須在挑戰之前**（先獨立判斷，再對抗檢驗）
- **魔鬼代言人不得軟化**（審查不是找優點，是找問題）
- **陰性結果和陽性結果同等重要**（"沒發現問題"也是有效結論）

---

## 極速版（30 秒開始）

複製下面這段到一個新 AI 會話中，替換 `[材料]` 為你要審查的內容：

```
你是獨立審查者。對以下材料進行審查：

1. 列出所有事實性宣告，標註置信度（可驗證/一致/合理/不可驗證）
2. 從邏輯/事實/方法/完整性/清晰度/洞察 6 個維度打分（1-5）
3. 從替代解釋/隱藏假設/邊界條件/反例/方法論替代 5 個角度找問題
4. 終判：保留/小改/重大修改/棄用 + 理由

材料：[在此貼上]
```

> 極簡版省去了 SOP 的完整約束。正式場景請用 [完整審查 Prompt](prompts/full-review.md)。

---

## 5 分鐘上手

### 第一步：理解獨立性

獨立審查需要兩個條件同時滿足：
1. **不同後端**：審查者用的 AI 模型 ≠ 產出內容的 AI 模型
2. **上下文隔離**：審查者不能看到作者的中間推理、自評、結論傾向

> ❌ "我用 Claude Code 的另一個 agent 審查" → 同後端，非獨立  
> ✅ "我用 ChatGPT 審查 Claude 產出的內容，且開了一個全新會話" → 獨立

### 第二步：選一個場景

| 場景 | 用什麼 |
|------|--------|
| 審查一份 AI 寫的方法論文件 | [完整審查 Prompt](prompts/full-review.md) |
| 只做對抗式挑戰（壓力測試） | [對抗式挑戰 Prompt](prompts/adversarial.md) |
| 想完整理解審查流程 | [完整 SOP](sop.md) |

### 第三步：準備材料，開一個全新會話

**準備階段**（操作者完成）：
1. 記錄**作者後端**（哪個模型產出了被審材料）
2. 記錄被審材料的**版本標識**（git commit hash / 檔案 hash / 版本號——至少一項）
3. 準備或選用評分標準（可從 [完整審查 Prompt](prompts/full-review.md) 的 D1-D6 六維框架開始；如無特定 rubric，六維框架作為預設標準即可）

**執行階段**：
1. 開一個**全新會話**（確認無歷史上下文洩露）
2. 只貼上：**prompt + 被審查的材料**（不附帶作者自評、中間版本、結論傾向）
3. 不要告訴審查者"這是誰寫的""我覺得哪裡有問題"
4. 收集結果後，對照 [SOP §10 後置核驗](sop.md) 驗證審查品質和獨立性

---

## 目錄結構

```
independent-review-toolkit/
├── README.md                 ← 你在這裡
├── LICENSE                   ← CC BY 4.0
├── sop.md                    ← 完整獨立審查標準操作程式
├── prompts/
│   ├── full-review.md        ← 完整四步審查 prompt（事實提取→評分→挑戰→終判）
│   └── adversarial.md        ← 對抗式挑戰專用 prompt（魔鬼代言人模式）
└── examples/                  ← 教學示例（基於真實審查發現重構，非逐字轉錄）
    ├── methodology-review.md  ← 審查案例演示（含完整 SOP 四步流程 + 註釋）
    └── methodology-review.json ← 同案例的結構化 JSON 配套
```

---

## 實戰資料

本工具包的方法論來自以下實戰積累：

| 指標 | 資料 |
|------|------|
| 審查總輪次 | 50+ |
| 涉及 LLM 後端 | 5 種（GPT-5.5 / DeepSeek-V4-Pro / Kimi-K2.7 / Qwen3.7-Max / GLM-5.2） |
| CLI house | 4 個（Codex / Claude / Kimi / Qwen） |
| 互補性實證 | 同 prompt 多後端 → 盲點互補率 ~50%（三後端實證） |
| 審查衰減實證 | 已識別並內建防衰減約束 |

---

## 與其他工具的關係

| 工具 | 時機 | 焦點 |
|------|------|------|
| **本工具包** | 事後（產出後） | "做得好不好"——完整審查 + 對抗挑戰 |
| kill-test-first | 事前（動手前） | "該不該做"——否決式預審 |
| Code Review (自動化) | 事前/事後 | 程式碼正確性/風格 |

---

## 許可與引用

CC BY 4.0。引用請註明版本號。

*生成模型：DeepSeek-V4-Pro (via Claude Code CLI) · 2026-07-01*
