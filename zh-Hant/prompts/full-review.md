# 完整獨立審查 Prompt

> **適用場景**：方法論資產、正式交付物、重大決策——需要完整四步審查流程的場景。  
> **不適用場景**：日常程式碼片段、非正式內容、只需快速壓力測試——請使用 `adversarial.md`。  
> **用法**：複製以下內容到全新 AI 會話中。**不要修改 prompt 的順序和結構**——評分必須在挑戰之前。

---

## 任務：獨立審查

你將審查一份材料。你的角色是**獨立審查者**——你不認識作者、不知道材料的背景、也沒有接觸過任何中間版本。

### 審查流程（嚴格按順序執行，不可跳過或顛倒）

---

### 步驟一：事實提取

從材料中提取所有事實性宣告。對每條宣告標註：

**置信層級**：
- `[VERIFIED]` — 可被公開來源驗證（**每個 [VERIFIED] 須附驗證來源：URL / 檔案路徑 / 工具名稱**）
- `[CONSISTENT]` — 與你自身知識一致
- `[PLAUSIBLE]` — 邏輯自洽但無法驗證
- `[UNVERIFIABLE]` — 無法判斷真偽

**宣告型別**：
- `[FACT]` — 事實斷言
- `[INFERENCE]` — 推導結論
- `[ASSUMPTION]` — 未宣告的假設
- `[VALUE]` — 價值判斷

如果你因工具限制無法驗證某宣告，標註 `[TOOL-LIMITED]` 而非降級為 CONSISTENT 或 UNVERIFIABLE。

---

### 步驟二：獨立評分

在閱讀材料後、進行任何挑戰之前，給出你的獨立評分。從以下六個維度打分（1=最差, 5=最好）：

**D1. 邏輯有效性 (1-5)**：推理鏈是否無邏輯跳躍/迴圈/矛盾？
**D2. 事實準確性 (1-5)**：可驗證事實是否正確？
**D3. 方法適當性 (1-5)**：所選方法是否適合研究問題？
**D4. 完整性 (1-5)**：是否覆蓋必要維度/邊界條件/替代解釋？
**D5. 清晰度 (1-5)**：表達是否清晰、結構是否合理？
**D6. 洞察深度 (1-5)**：是否有超出常規的洞察？

每項評分附一句錨定說明（為什麼給這個分）。某維度不適用時標記 N/A 並說明理由。缺乏領域知識時標記 `[OUT-OF-DOMAIN]`。

綜合評分 = 各維度算術平均（N/A 排除）。

---

### 步驟三：對抗式挑戰

現在，切換角色。你是**魔鬼代言人**——你的任務是盡最大努力找出材料的問題。從以下五個方面逐項提出具體挑戰。**每個挑戰必須指向材料中的具體段落/宣告/推理步驟**。不得附加緩和語（禁止"不過總體來說還是不錯的"等表述）。

**(A) 替代解釋**：有沒有其他解釋能同樣好地說明這些事實？

**(B) 隱藏假設**：哪些假設被當成了既定事實？

**(C) 邊界條件**：在什麼情況下這個結論會失效？

**(D) 反例/反證**：有沒有與結論矛盾的事實或案例？

**(E) 方法論替代**：用另一種方法會不會得出不同結論？

完成 (A)-(E) 後，重新審視你步驟二的評分。如果挑戰說服了你，調整評分並記錄調整幅度和理由。

---

### 步驟四：終判

給出以下判決之一：
- **保留 (Keep)**：材料合格，可直接使用
- **小改 (Minor)**：有可改進之處但不影響核心結論
- **重大修改 (Major)**：存在影響核心結論的問題，修改後須重審
- **棄用 (Discard)**：核心結論不可靠

終判須包含：
1. 綜合評分 + 各維度明細
2. 關鍵挑戰摘要（最重要的 2-3 個）
3. 終判結論 + 理由
4. 獨立性聲明（嵌入以下格式）：

```yaml
independence_declaration:
  reviewer_backend: "<你的模型名称>"
  author_backend: "<作者模型名称，如未知则标注 UNKNOWN>"
  axis1_different_backend: true/false
  axis2_context_isolated: true/false
  independence_level: "<IND | SEMI | NON>"
  degradation_notes: "<如有退化，说明原因>"
  session_id: "<审查者会话标识，如无平台级 ID 则用模型名+开始时间>"
  timestamp: "<ISO 8601>"
```

5. **雙件輸出**：除本 Markdown 報告外，同時輸出一份結構化 JSON。JSON 須包含以下欄位（缺少則視為未完成）：

```json
{
  "metadata": {"title": "...", "date": "...", "model": "<你的模型>", "sop_version": "v2.0"},
  "source_material": {"title": "...", "author_backend": "...", "version_binding": "..."},
  "step1_fact_extraction": [{"text": "...", "confidence": "VERIFIED|CONSISTENT|PLAUSIBLE|UNVERIFIABLE", "type": "FACT|INFERENCE|ASSUMPTION|VALUE"}],
  "step2_scoring": {"D1_logic": 0, "D2_accuracy": 0, "D3_method": 0, "D4_completeness": 0, "D5_clarity": 0, "D6_insight": 0, "composite_score": 0},
  "step3_adversarial_challenge": {"A": "...", "B": "...", "C": "...", "D": "...", "E": "..."},
  "step4_final_judgment": {"verdict": "KEEP|MINOR|MAJOR|DISCARD", "composite_score": 0, "independence_declaration": {"reviewer_backend": "...", "author_backend": "...", "axis1_different_backend": true, "axis2_context_isolated": true, "independence_level": "IND|SEMI|NON", "degradation_notes": "...", "session_id": "...", "timestamp": "..."}},
  "post_hoc_audit": {"items": {}, "conclusion": "PASS|CONDITIONAL_PASS|FAIL"}
}
```

---

## 被審查的材料

[在此貼上被審查的材料]

*正體中文：OpenCC 轉換 + GPT-5.5 (via Codex CLI) 潤色 · 2026-07-01*
