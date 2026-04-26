---
layout: page
title: 动作条件化术中 X 射线视角预测
description: 基于条件扩散模型的 C 臂手术视角规划
img: assets/img/projects/diffusion.jpg
importance: 2
category: research
---

构建了一个用于闭环手术视角规划的观测预测模块：给定当前透视图像与候选 6-DoF C 臂运动，条件扩散模型（DDPM 训练 / DDIM 推理）预测目标视角下的 X 射线图像，为下游视觉语言模型（MedGemma）提供可视化预览。

## 方法

- 使用 DeepDRR 从 827 个 CT 体数据构建 DRR 训练集，每例采样 100 个位姿（5 个椎体中心 × 20 个角度，覆盖正位 / 侧位 / 斜位），并按角度与平移距离阈值筛选，每例保留约 1,500 对训练样本。
- 设计 U-Net 骨干网络，引入 cross-attention 实现源图像条件化，并用 AdaGroupNorm 注入 9 维相对位姿编码（6D 旋转 + 3D 平移）。
- 在 RTX 3090 上以混合精度（fp16）完成 DDPM 训练与 DDIM 推理。

**所属机构：** ARCADE Lab，约翰斯·霍普金斯大学
**导师：** Mathias Unberath 教授，博士生 Blanca Inigo Romillo
