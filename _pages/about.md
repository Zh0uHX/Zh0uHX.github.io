---
permalink: /
title: "关于我"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

你好，我是 Zhou Haoxiang（Rick Zhou）。我关注大语言模型的训练系统、强化学习后训练、多模态模型和 Coding Agent 工程。

做项目时，我习惯把实验协议、原始数据和局限写清楚：每个数字都能追溯到对应的代码版本和原始结果，结论不超出实验能支撑的范围。

## 方向
- **LLM 训练系统**：分布式 SFT packing、精确断点续训、checkpoint 兼容与可复现 benchmark
- **RL 后训练**：基于 [verl](https://github.com/verl-project/verl) 的奖励函数与评测
- **多模态模型**：用 Qwen3-VL 做生成式开放词汇目标检测
- **Agent 工程**：可靠的工具调用协议，以及基于真实 issue 的定量评测

## 精选项目

### [Repo Maintainer Agent](https://github.com/Zh0uHX/repo-maintainer-agent)
面向本地代码仓库的 Coding Agent，采用显式的 plan–act–observe 循环，所有操作限制在仓库内，命令走白名单。在从 SWE-bench Lite 分层抽取的 44 个真实 issue 上（每个条件 3 次，配对 bootstrap）：

- JSON 协议换成原生 tool calling 后，动作阶段需要修复的模型调用从 7.63% 降到 0.06%
- 函数级定位 recall@1 从 48.8% 提高到 57.9%（+9.1 pp）

[详细介绍](/portfolio/repo-maintainer-agent/)

### [Nanochat：LLM 训练系统](https://github.com/Zh0uHX/Nanochat)
基于 karpathy/nanochat 的训练系统项目，聚焦确定性的分布式 SFT packing、精确中断恢复、checkpoint 兼容和可复现 benchmark。

- 在 8×A100 上完成 1.68B 参数模型预训练（7.8B tokens，MFU 45.86%）
- Best-fit packing 把 padding 比例从 14.97% 降到 0.19%；在固定算力预算下多处理 29% 的有效 token

[详细介绍](/portfolio/nanochat/)

### [Qwen3-VL 生成式开放词汇目标检测](https://github.com/Zh0uHX/qwen3-vl-ovd)
输入图片和类别列表，用 Qwen3-VL + LoRA 自回归生成类别和边界框，坐标用 1000 个离散 token 表示。

- 100 张验证图上，F1@0.5 从 zero-shot 的 0.321 提升到 0.578，格式有效率从 0% 提高到 81%
- 独立 500 张终评：F1@0.5 0.576，Precision 0.738

[详细介绍](/portfolio/qwen3-vl-ovd/)

### [DSH Skills](https://github.com/Zh0uHX/dsh-skills)
DSH 插件：在设置页里搜索、安装、更新和卸载 Skills.sh 技能，带路径边界校验和失败回滚。

[详细介绍](/portfolio/dsh-skills/)

## 开源贡献 {#contributions}
- **[verl-project/verl#8231](https://github.com/verl-project/verl/pull/8231)**：修复 GSM8k 奖励函数的答案归一化（审阅中）。
  原实现会把 `#### 72.`、`#### $300`、`1,000` 这类格式正确的答案判为 0 分。我用 Qwen2.5-0.5B/1.5B-Instruct 在全部 1,319 道测试题上采样验证：修复后 1.5B 的 flexible 奖励从 53.1% 升到 63.3%，原来得 1 分的回答没有一个变成 0 分。
