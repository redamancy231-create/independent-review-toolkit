你是独立审查者，对以下修复进行 R2 重审。

## R1 审查回顾

2026-07-01，GPT-5 Codex 对 independent-review-toolkit v2.0.0 进行了 R1 交叉审查。终判：Major。发现 16 项（CRITICAL×3, MAJOR×10, MODERATE×2, MINOR×1）。

R1 审查报告：E:/workspace/projects/independent-review-toolkit/_reviews/codex_crosscheck_v2.0.0_output.txt（行 4400-4510）

## R2 审查任务

验证以下 16 项是否已正确修复。每项只判：已修复/未修复/部分修复。给出具体证据（文件名+行号或段落内容）。

CRITICAL:
- C1: 案例来源是否从 codex_v16_crosscheck_* 改为框架§6.3.2 + A1实验？
- C2: README是否新增了[NON]独立性透明声明？
- C3: 案例是否新增了完整YAML independence_declaration？

MAJOR:
- M1: SOP §13是否恢复了JSON schema？
- M2: SOP §7.2是否恢复了"评推理不评结论"完整约束？
- M3: README English abstract是否降级了营销语言(battle-tested/unbiased)？
- M4: "5分钟上手"是否增加了作者后端/版本绑定/rubric准备步骤？
- M5: README是否声明了工具包自身由单后端生成？
- M6: Prompt YAML是否补全了axis1/axis2/session_id？
- M7: Prompt是否要求双件输出(.md+.json)？
- M8: 案例数据点是否追溯到正确源（框架§6.3.2+A1）？
- M9: 发现数量是否更正？
- M10: 是否创建了配套.json并加了版本绑定？

MODERATE:
- M11: SOP是否新增了§8.3挑战后评分调整IF规则？
- M12: Prompt步骤一是否要求VERIFIED附来源？

MINOR:
- M13: README English描述是否修正？

## 输出格式

```
C1: [已修复/未修复/部分修复] 证据: ...
C2: ...
...
M13: ...

总体R2终判: [PASS / FAIL_WITH_CAVEATS / FAIL]
理由: ...
```

请先读取工具包当前版本的以下文件（这些是修复过的）：
- E:/workspace/projects/independent-review-toolkit/README.md
- E:/workspace/projects/independent-review-toolkit/sop.md
- E:/workspace/projects/independent-review-toolkit/prompts/full-review.md
- E:/workspace/projects/independent-review-toolkit/examples/methodology-review.md
- E:/workspace/projects/independent-review-toolkit/examples/methodology-review.json

独立审查者后端：请自报模型名称。
