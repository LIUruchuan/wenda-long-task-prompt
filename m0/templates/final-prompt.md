# Final Prompt（组装骨架）

> 这是 S5 编译完成后，交给下游长任务 Coding Agent 的**最终初始化上下文**。
> 它由两部分拼成：
> - **Part A · Engineering Instructions**（怎么工作）
> - **Part B · Project Context**（做什么/不做什么）
>
> 下面是骨架。真实项目使用时，把 `<<<PART_A>>>` 和 `<<<PART_B>>>` 换成实际文件内容——
> **必须内联全文，不允许写"见同目录 xx 文件"**：下游 Agent 可能只拿到这一个文件。
> 这不是把聊天记录拼成长 Prompt——它是一份结构化的 Operating Context。
>
> 若交付包内附 `references/` 领域清单（渐进披露，随包交付），本骨架的"开始之前"注明其存在与适用领域（详见 Part A §3c）；清单全文不内联进本文件。

---

# 长任务项目初始化

你即将开始一个长周期编码任务。请完整阅读本文件后再动手。
它分两部分：**Part A 规定你的工作方式，Part B 规定项目内容与边界。**

---

## Part A · Engineering Instructions

<<<PART_A>>>

---

## Part B · Project Context

<<<PART_B>>>

---

## 开始之前

1. 先读完 Part B，确认你理解了"做什么、不做什么、怎么算完成"。
2. 把进度状态文件 `PROGRESS.md` 建在项目里（结构见交付包内模板），进度、关键决策、验证证据都写进去，不要只靠对话记忆。
3. 若交付包含 `references/` 领域清单：先读 Part A §3c 列出的适用领域清单，再动手。
4. 从 Part B 第 6 节"建议开发顺序"的第一步开始。
5. 遇到 Part A 第 2 节列出的决策点，停下来问用户；其余自主推进。
