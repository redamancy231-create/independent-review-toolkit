你是独立审查者，对以下内容进行交叉审查。

## 审查对象

仓库：E:/workspace/projects/independent-review-toolkit/
文件列表：
- README.md
- sop.md
- prompts/full-review.md
- prompts/adversarial.md
- examples/methodology-review.md

来源：DeepSeek-V4-Pro (via Claude Code CLI) 从 AI协作项目全生命周期框架 v1.6.4 提取并改编。
源材料对照：
- E:/workspace/projects/AI协作项目全生命周期框架/_protocols-and-tools/methodological-review-sop.md（框架版SOP v1.0.4）
- E:/workspace/projects/AI协作项目全生命周期框架/_protocols-and-tools/meta-audit-checklist.md（元审查清单 v1.0.4+）

## 审查方式

请先阅读工具包的全部5个文件，再对照源材料。审查以下5个维度：

### D1: SOP提取完整性
- 从框架版SOP到独立版SOP，去框架化的过程中有没有**误删**重要内容？
- 有没有**遗漏**关键操作步骤或约束？
- 通用化改写有没有**改变了原意**？

### D2: README准确性和可用性
- "5分钟上手"是真的5分钟吗？步骤是否清晰可执行？
- 表述是否有**误导**？数据和声明是否准确？
- 对不了解背景的读者是否**自包含**（不需要读框架就能用）？

### D3: Prompt模板质量
- 复制即用——真的能直接复制粘贴到新会话里用吗？
- 模板结构和措辞是否有问题？指令是否清晰？
- 步骤之间的顺序约束（评分必须在挑战前）是否在模板中得到正确体现？

### D4: 案例真实性
- 案例中的数据点和结论是否与原始审查报告一致？
- 对照：E:/workspace/projects/AI协作项目全生命周期框架/_reviews/codex_v16_crosscheck_*（相关审查报告）

### D5: 自反一致性（最关键）
- 工具包的内容是否遵守了它自己规定的标准？
- 例如：SOP §2.1 要求审查者和作者必须不同后端——但工具包全程由DeepSeek-V4-Pro单后端产出。README中是否对此做了透明声明？
- SOP §9.3 要求终判嵌入独立性声明——案例本身是否包含了完整的independence_declaration？
- SOP §13 要求md+json双件输出——工具包自身的prompts和案例是否也应配.json版本？

## 输出格式

对每个维度给出：
1. 发现问题（按严重度 CRITICAL / MAJOR / MODERATE / MINOR 分级）
2. 具体指向（文件名+行号或段落）
3. 修复建议

最后给出总体终判（Keep / Minor / Major / Discard）和理由。

独立审查者后端：请自报模型名称。
