---
title: "DSH Skills"
excerpt: "DSH 插件：在设置页搜索、安装、更新和卸载 Skills.sh 技能，带路径边界校验和失败回滚。"
permalink: /portfolio/dsh-skills/
collection: portfolio
---

[GitHub 仓库](https://github.com/Zh0uHX/dsh-skills) · TypeScript · [演示视频](https://github.com/Zh0uHX/dsh-skills/releases/tag/v0.1.0)

在 DSH 的 **设置 → Skills** 中搜索、安装和管理 [Skills.sh](https://skills.sh) 技能，不需要配置模型 API Key。

## 功能
- 搜索结果显示简介、来源、安装量和原页面链接；安装后可检查更新、更新或卸载，覆盖操作都需要确认
- 只管理自己安装的技能，不接管其他工具写入的目录；同名技能存在时中止安装
- 检查更新时比较完整文件内容摘要；下载或写入安装记录失败时保留旧版本

## 安全边界
- 拒绝路径穿越、符号链接、重复路径和文件/目录冲突，不执行下载的脚本
- 单个技能最多 256 个文件、单文件 2 MiB、总计 8 MiB
- 测试使用真实临时目录，覆盖安装、更新、卸载、失败回滚和路径边界

开发过程、AI 使用方式和人工输入范围记录在仓库的开发文档中。
