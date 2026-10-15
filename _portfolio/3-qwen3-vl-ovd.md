---
title: "Qwen3-VL 生成式开放词汇目标检测"
excerpt: "用 Qwen3-VL + LoRA 自回归生成类别和边界框。相对 zero-shot，F1@0.5 从 0.321 提升到 0.578，负类误检率从 18% 降到 0.5%。"
permalink: /portfolio/qwen3-vl-ovd/
collection: portfolio
---

[GitHub 仓库](https://github.com/Zh0uHX/qwen3-vl-ovd) · Python / PyTorch · Qwen3-VL、PEFT LoRA、Gradio

模型接收图片和自然语言类别列表，自回归地生成类别及边界框。仓库覆盖流式数据转换、双卡训练、批量预测、严格评测和 Gradio Demo。

## 方法
- 坐标按图像宽高归一化到 `[0, 999]`，为 tokenizer 新增 1000 个坐标 token，并验证每个坐标只占一个 token
- 用基模原有 `<k>` 子词的 embedding/lm-head 均值初始化新 token，缓解小数据下扩词表的冷启动问题
- LoRA 微调语言模块，冻结视觉编码器；条件允许时只训练新增坐标 token 对应的 embedding 行
- COCO/LVIS 统一转为 JSONL，支持类别白名单、holdout 类别和负类别采样
- 评测包含 IoU 匹配、Precision/Recall/F1、严格格式有效率和负类别误检率

## 结果
在 2×A800-80GB 上用 2,932 张 COCO 子集训练，并做了四组固定集消融。

| 指标 | zero-shot | 微调（精度版） |
|---|---:|---:|
| F1@0.5（100 张验证图） | 0.321 | 0.578 |
| 严格格式有效率 | 0% | 81% |
| 负类别误检率 | 18% | 0.5% |

在独立的 500 张终评集上，F1@0.5 为 0.576，Precision 为 0.738，格式有效率为 82%。
