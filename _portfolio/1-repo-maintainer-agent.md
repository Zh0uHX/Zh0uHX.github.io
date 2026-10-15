---
title: "Repo Maintainer Agent"
excerpt: "面向本地代码仓库的 Coding Agent。在 44 个 SWE-bench Lite 真实 issue 上，原生 tool calling 把需要协议修复的模型调用从 7.63% 降到 0.06%，函数定位 recall@1 提升 9.1 pp。"
permalink: /portfolio/repo-maintainer-agent/
collection: portfolio
---

[GitHub 仓库](https://github.com/Zh0uHX/repo-maintainer-agent) · Python · OpenAI-compatible 模型接口

一个面向本地代码仓库的 Coding Agent，重点是模型协议的可靠性和可复现的评测，而不是封装聊天接口。

## 设计
- 显式的 plan–act–observe 状态循环，每轮只执行一个工具调用：检索、AST 符号定位、精确编辑、运行检查或查看 diff
- 路径、标识符、AST 符号和代码片段联合排序的上下文检索
- 安全边界：操作限制在仓库根目录内，过滤敏感文件，命令走白名单，写入和执行测试分别授权
- 每次运行保存 JSONL trace、最终结果、diff 和原文件备份
- benchmark 在临时仓库中执行，支持任务级回归测试

## 评测结果
评测集是从 SWE-bench Lite 按仓库规模分层抽取的 44 个真实 issue，任务是定位需要修改的函数。每个条件跑 3 次，按用例做配对 bootstrap，给出 95% 置信区间。

| 实验 | 对比 | function recall@1 | 其他 |
|---|---|---|---|
| 协议对比 | JSON 协议 → 原生 tool calling | 48.8% → 57.9%（+9.1 pp，95% CI +0.2 ~ +18.4） | 需修复的动作调用 7.63% → 0.06%；token +63% |
| 提示消融 | 默认提示 → AST 优先的定位提示 | 48.1% → 56.8%（+8.7 pp，95% CI +2.1 ~ +15.8） | token −13% |

两组实验在不同代码版本上运行，绝对数值不跨实验比较。

## 可靠性工作
第一次在真实仓库上运行时，10 条用例中有 3 条死在协议层（JSON 括号未闭合、JSON 模式返回空白）。我加入了本地补括号、空回复重发等恢复手段，并为每次模型调用记录修复计数，避免把“修过才跑通”的运行当成干净运行。随后换用原生 tool calling，并处理了它自己的问题，例如一次返回多个调用时只执行第一个，保证每个调用 id 都有回复。
