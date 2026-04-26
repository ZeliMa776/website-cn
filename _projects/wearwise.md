---
layout: page
title: "WearWise — AI 穿搭推荐"
description: 个人云端衣橱，支持 AI 穿搭推荐与虚拟试穿
img: assets/img/projects/wearwise.jpg
importance: 4
category: course
tags: [TypeScript, React, Express, MongoDB, Gemini, Cloudflare R2]
---

**在线体验：** [wearwise-cs423.web.app](https://wearwise-cs423.web.app/)

WearWise 是一款个人云端衣橱应用，用户上传服装照片后，系统可根据天气、场合与个人风格提供 AI 穿搭推荐，并支持虚拟试穿图像生成。

## AI 推荐引擎

- 设计多段式 system prompt 架构（`buildWardrobeSystemMessage()`），将用户衣橱数据（单品 ID、分类、标签、描述）、个人档案、天气信息及带 ISO 时间戳的消息历史一并注入 LLM 上下文。
- 构建完整的投票 → 风格摘要 → 推荐偏好循环；通过 Gemini 摘要提取属性级风格模式（如"偏好休闲棉质，不喜正装"），并将激活的 styleNote 应用规则注入 system prompt。

## 天气与场合感知

- 设计四步推理链（位置 → 天气 → 本地时间 → 场合），每个节点均设有优雅降级处理。
- 将时区与地理位置解耦，独立传递 `Intl.DateTimeFormat().resolvedOptions().timeZone`，避免位置权限被拒时影响时间感知逻辑。
- 位置获取失败时，检索对话历史中曾提及的城市名；若找到则推断 IANA 时区并继续天气流程；若未找到，则将城市与场合合并为单次提问，避免连续追问用户。
- 修复了一处边界条件 bug：原代码将"位置是否存在"误用为"时区是否可用"的判断依据，导致仅有时区而无完整位置时，天气步骤被跳过。

## 流式输出与校验

- 将批量推荐输出重构为 `submit_outfit` 工具调用模式，每次调用触发独立 SSE 事件，实现带骨架屏动画和 ThinkingDots 过渡效果的卡片渐进式渲染。
- 在 `submit_outfit` 中实现双重校验（无效单品 ID 拦截 + 重复品类拒绝并触发 LLM 自动重试），配饰类单品豁免唯一性约束，允许一套穿搭包含多件配饰。
- 实现安全伪流式文本管道：缓冲 LLM 输出，用正则过滤前缀时间戳，以伪流式方式重放（每块 3 字符，间隔 15ms），在保留打字机 UX 效果的同时防止内部推理过程泄露。

## 工程贡献

- 在 4 轮迭代中提交 **40+ 个 PR**，涵盖后端集成测试、前端 E2E 测试与服务级单元测试，参与代码审查与 CI/CD 维护。

**课程：** 软件工程（CS423），约翰斯·霍普金斯大学
