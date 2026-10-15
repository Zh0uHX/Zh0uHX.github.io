---
title: "Nanochat：LLM 训练系统"
excerpt: "基于 karpathy/nanochat 的训练系统项目：1.68B 模型预训练、确定性分布式 SFT packing、逐位一致的断点续训，以及可复现的 A800 系统 benchmark。"
permalink: /portfolio/nanochat/
collection: portfolio
---

[GitHub 仓库](https://github.com/Zh0uHX/Nanochat) · Python / PyTorch · 基于 [karpathy/nanochat](https://github.com/karpathy/nanochat) 并明确标注来源

上游项目提供了完整的基线：BPE 分词、Decoder-only 预训练、SFT/RL、分布式 Muon/AdamW、评测、KV-cache 推理和 Web UI。我的工作集中在**确定性分布式 SFT packing、精确中断恢复、checkpoint 兼容、实验溯源和可复现 benchmark**。

## 预训练记录

| 项目 | 数值 |
|---|---:|
| 参数量 | 1,681,790,292 |
| 预训练 token | 7,806,124,032 |
| 硬件 | 8× A100-SXM4-80GB，BF16 |
| 用时 / MFU | 701.81 min / 45.86% |
| 重新评测 CORE（step 14,889） | 0.2680 |

## SFT packing
在 10,000 条长尾分布的合成对话上（3 个随机种子），best-fit 策略把 padding 比例从 14.97% 降到 0.19%，CPU 端组 batch 的中位耗时低于 2 ms。

从同一个 1.68B checkpoint 出发、在 2×A800 上做 30 步固定算力预算的试验：best-fit 处理的有效 token 比顺序 packing 多 29.05%，验证 BPB 从 0.4615 降到 0.4498。这是单次短程试验，只支撑“算力利用率”层面的结论，不声称端到端训练加速。

## 系统 benchmark（2×A800）
- **精确续训**：双卡确定性训练在第 3/8 步中断后恢复，重放的 batch、loss、模型参数和优化器状态与不中断的运行完全一致（最大绝对差 0.0）
- **优化器 kernel**：编译后的 AdamW 中位延迟 0.454 ms，eager 为 1.877 ms（4.14×），BF16 数值一致性通过
- **KV-cache 边界**：上下文 1,984 时 TPOT 加速 2.78×，但上下文 128 时反而慢（0.65×），因此不声称与上下文无关的加速

所有结果都附带原始 JSON、Git commit 和配置哈希。
