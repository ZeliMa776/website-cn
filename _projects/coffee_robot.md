---
layout: page
title: "M — 咖啡馆社交机器人"
description: 基于 GPT-4o 的情感陪伴社交机器人，部署于真实咖啡馆环境
img: assets/img/projects/coffee_robot.jpg
importance: 3
category: course
tags: [GPT-4o, Python, ROS2, Docker, Whisper, Co-design]
---

在真实咖啡馆环境中设计并部署了一款基于 GPT-4o 的社交机器人，专注于情感陪伴与自然对话，而非单纯的点单交互。

## 系统架构

- 构建完整对话流水线，包含意图分类（SMALL_TALK / TASK_ORDER / TASK_RECOMMEND / TASK_ROBOT）、基于摄像头的情绪监测、STT/TTS 语音交互界面，以及经 Docker 容器化的 ROS2 机器人控制模块。

## 迭代共同设计

- 主导两轮迭代式 co-design：招募参与者开展实地会话，通过 Whisper API 转写交互内容，结合对话日志分析与会后访谈提炼设计需求，并进行纵向对比。

## LLM 行为工程

- 识别并修复了 LLM 的核心行为缺陷——话题锁定在咖啡、情绪触发强推饮品、回应流于泛泛——通过系统化 prompt 重设计，并在 system prompt 中注入 5 组 BAD/GOOD few-shot 示例对加以修正。
- 实现了基于对话上下文（情绪状态、疲劳程度、偏好表达）的情境感知饮品推荐模式（TASK_RECOMMEND），并引入状态机保证多轮对话的连贯性。

## 成果

- 第二轮迭代中，**8 个行为维度**均获得确认改善；两位参与者均表示无需调整自身沟通方式即可自然与机器人交流。

**课程：** 人机交互（EN601.691），约翰斯·霍普金斯大学
