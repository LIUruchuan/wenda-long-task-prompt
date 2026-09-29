# Prompt / Context Compiler（S5）

> 把"需求 + 确认决策 + 项目计划 + 工程规则"编译成下游 Coding Agent 能直接用的上下文。
> **编译器的职责是组装，不是创作。** 不允许在这里新增需求或发明决策。

---

## 输入

- `requirement_state`（经 Gate 1 确认）
- 用户在 Gate 2 确认的 `project_plan`，以及确认时用户的修改/否决记录（这些就是"已确认决策"，必须按用户改过的版本编译）
- 通用工程规则（来自 `templates/engineering-instructions.md`；其中【按项目填】小节须用确认过的需求/计划内容填写，方括号占位符不得流向下游）
- 领域清单（`references/` 随包，编译时按项目类型勾选适用领域，不勾选的领域不引入）

## 输出

- `engineering-instructions.md`（Part A）
- `project-context.md`（Part B）
- `final-prompt.md`（Part A + Part B 的组装，按 `templates/final-prompt.md`）
- `PROGRESS.md`（动态状态文件，从 `templates/progress.md` 初始化，交付时随包给出）
- `references/`（随包交付的领域清单：勾选适用的文件随包；Part A §3c 内联"适用领域表"

---

## 编译规则

### 1. 一一对应，不新增

- Part B 每个字段都来自确认过的需求/计划，**不许凭空补全**。
- 采访阶段标了 `partial` 的字段，若 Gate 1 没解决，Part B 里如实写"待确认"，不要替用户编一个答案。
- Schema 的每个 P0 字段在 Part B 里必须有落点（含 §3b 输入素材 → Part B §4b）；没有落点的字段等于白问。

### 2. Hard / Soft 分开写

- 硬约束（目标、non_goals、tech_limit、compliance、acceptance）写进 Part B 并保持显性。
- 软偏好（preferred_tech）写进 Part B 第 5 节，并注明"Agent 可优化"。
- 通用工作纪律（验证、回退、问人时机）写进 Part A，不污染 Part B 的项目事实。

### 3. 静态 / 动态分离

- Part B 只放**相对稳定**的项目事实（是什么、边界、计划方向）。
- 动态进度（做到哪了、当前卡在哪）**不写进 final-prompt**——那是 Coding Agent 跑起来后自己维护的 STATE。
- 编译时从 `templates/progress.md` 生成一份空的 `PROGRESS.md` 随包交付，交付说明里注明："长任务开始后，请在此文件维护进度与关键决策。"

### 4. 可验收优先

- Part B 的"怎么算完成"必须写成**可观测**的（跑什么命令/打开什么/看到什么结果），而不是"做得好用/体验流畅"。
- Agent 说"做完了"≠ 系统验证通过：完成声明必须挂可复现的验证证据。

### 4b. 领域选择规则

- 按项目类型/技术栈勾选适用领域，写入 Part A §3c 的"适用领域表"（领域名 + 必读文件 + 核心红线摘要）；完整清单文件随包放在 `references/`。
- 勾选原则：**只勾项目真正触碰的领域**。例如纯前端项目勾 frontend/accessibility/performance（必要时 security），不勾 api-design/ci-cd 等无关领域——避免给下游 Agent 无差别堆清单（信息过载）。
- 勾了就必须有对应 `references/<area>-checklist.md` 文件随包；未勾选的文件**不得**出现在随包清单引用中。
- 已交付的 `references/` 清单是随包资产：后续编辑按"增补不破坏"处理，改后回读校验（占位符、死链）。

### 5. 自检（编译完跑一遍）

- [ ] Part B 的 `original_intent` 是用户原文？
- [ ] `non_goals` 写清楚了？
- [ ] 硬约束都在，没有被软化？
- [ ] 项目接收外部输入时，Part B §4b 有 `input_source` / `input_path` / `input_toolchain` / `input_size` 的落点？
- [ ] 验收里的主观词（接近/像/流畅/好看）是否都挂上了可对照的参考锚点（Part B §7 `reference_anchor`）？
- [ ] 需求里的"全部/所有"类表述已枚举化，验收条目写明对账分母（全绿 = 分母内对平 + 分母外如实披露）？
- [ ] 规划里每个"降级/替代/跳过"对策，其受影响的验收锚点已按降级后的保真度改写？（不允许"对策说降级、验收仍写完全一致"并存）
- [ ] 几何/坐标类项目：坐标语义约定（参照系/单位/绝对 vs 相对）已写入 Part B §4b？非几何项目该行写"无"？
- [ ] `final-prompt.md` 已**内联** Part A 与 Part B 全文？（不允许写"见同目录 xx 文件"——下游 Agent 可能只拿到这一个文件）
- [ ] Part A / Part B 的方括号占位符已全部替换，没有 `[…]` 流向下游？
- [ ] Part A 含 §4 Iron Law"证据先于声明"与反合理化表？
- [ ] Part A 含 §3b 质量门槛（Floor 5 条 + 项目级门槛表，已按确认结果填写）？
- [ ] Part A 含 §3.2 测试纪律（默认 TDD + 豁免范围已按项目填写，留空即"无豁免"）？
- [ ] Part A 含 §9 评审协议（五轴 / 两段式 / 分级 / 3 轮上限）？
- [ ] Part A §3c 适用领域表已填写；每个勾选领域都有对应 `references/<area>-checklist.md` 随包；未勾选领域无引用？
- [ ] "开始之前"含"先读适用领域清单"步骤（若随包含 references/）？
- [ ] 验收方式可观测？
- [ ] 有没有把 How（具体实现细节）误写进 Part B？
- [ ] 最终回读 `original_intent`，有没有把用户没说过的方向写进去？（收尾必须回到原始意图）

全部通过才进入 S6 交付。
