你是独立审查者，对以下 2026-07-03 会话产出进行异后端交叉验证。使用你自己的搜索策略（PowerShell/rg/grep），不要依赖本 prompt 中未提供的任何路径或结论。

## 审查范围

四个产出物，按优先级排列：

### P0-A: IRT en/examples/methodology-review.json（英文翻译 JSON）

源文件：examples/methodology-review.json
译文：en/examples/methodology-review.json

检查项：
1. JSON 结构是否与源文件完全一致（键名、嵌套层级、数组长度）？逐键比对顶层和二级键。
2. 所有值字段是否已翻译为英文（无残留中文）？特别检查 metadata.note、rationale、auditor 等长文本字段。
3. 术语一致性：审查案例→Review Case、方法论→methodology、教学重构→pedagogical reconstruction、异后端→different backend——是否与 en/examples/methodology-review.md 中的术语一致？
4. 不变项是否保留：版本号 2.0.1、git commit dcac073、日期 2026-07-01、分数 3.8、后端名 GPT-5.5 (via Codex CLI) 等不应翻译的字段是否原样保留？
5. provenance 标注：是否标注了翻译模型和日期？

### P0-B: IRT zh-Hant/examples/methodology-review.json（正体中文 JSON）

源文件同上，译文：zh-Hant/examples/methodology-review.json

检查项：
1. JSON 结构一致性（同 P0-A#1）
2. 简→繁转换是否完整？检查常见一对多陷阱：项目→專案（非項目）、文件→檔案（非文件）、信息→資訊、脚本→指令碼、通过→透過（非通過）
3. 术语一致性：是否与 zh-Hant/examples/methodology-review.md 一致？
4. 不变项保留（同 P0-A#4）+ provenance 标注

### P1: write-claude-md SKILL.md 生命周期章节

文件：~/.claude/skills/write-claude-md/SKILL.md
新增章节：## 项目生命周期差异（在 ## 输出模板 之前）

检查项：
1. 反例检测：差异化权重表是否对所有项目类型都成立？找一个反例——是否存在"CLOSED 但仍需精确命令"或"活跃但命令权重应降低"的场景？
2. 逻辑自洽：CLOSED 项目的"避坑指南权重最高"与"可推导信息砍"是否矛盾？（避坑内容是否可能包含可推导信息？）
3. 可操作性：生命周期判定表的三个判据（近30天提交/稀疏提交/明确标记）是否互斥且完备？"维护"与"CLOSED"的边界是否足够清晰？
4. 缺失检查：是否缺少"维护"状态的额外要求？（当前只有活跃和 CLOSED 的额外要求）
5. 实证来源：引用的 2026-06-30 三项目审计是否与 ../AI协作项目全生命周期框架/project_status.md 中的记录一致？

### P2: PTM A3 scoresheet.csv

文件：../prompt-tdd-methodology/examples/a3-action-routing/scoresheet.csv
源数据：../prompt-tdd/tests/pocketflow_assets/a3_action_routing/scores/scoresheet_tier0.json

检查项：
1. CSV 中的 case_id 是否与 test_set.json 中的 cases[].id 完全对应（10 cases x 2 arms = 20 rows）？
2. expected_action 列是否与 test_set.json 中的 expected_action 一致？
3. correct 列是否全部为 1（Tier 0 两臂均为 10/10）？
4. CSV 格式是否合法（无多余逗号、引号转义正确）？

## 输出格式

```
P0-A: [PASS/FAIL]
  1. 结构: [PASS/FAIL] 证据: ...
  2. 翻译完整性: [PASS/FAIL] 发现: ...
  3. 术语一致性: [PASS/FAIL] 发现: ...
  4. 不变项: [PASS/FAIL] 发现: ...
  5. provenance: [PASS/FAIL] 发现: ...

P0-B: [PASS/FAIL]
  (同上格式)

P1: [PASS/FAIL]
  1. 反例: [有/无] ...
  2. 逻辑: [PASS/FAIL] ...
  3. 可操作: [PASS/FAIL] ...
  4. 缺失: [PASS/FAIL] ...
  5. 实证: [PASS/FAIL] ...

P2: [PASS/FAIL]
  (逐项)
```

每个 FAIL 附具体证据（文件路径+行号/内容摘录）。
