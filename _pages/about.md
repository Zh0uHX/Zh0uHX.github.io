---
permalink: /
title: "关于我"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

我是 Zhou Haoxiang，也可以叫我 Rick。

目前主要在做大模型训练相关的工程。在 nanochat 上从头训过一个 1.68B 的模型，之后大部分时间花在 SFT 数据打包和断点续训上。另外用 Qwen3-VL 做过开放词汇目标检测，还写了一个改代码仓库的 Agent，拿 SWE-bench Lite 的真实 issue 测它。

下面项目里的数字都附了原始结果，只跑过一次的实验我会直接写明。

## 项目

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
