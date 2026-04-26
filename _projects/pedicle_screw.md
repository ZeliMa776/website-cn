---
layout: page
title: 椎弓根螺钉轨迹自动规划
description: 覆盖完整胸腰椎的手术路径规划流水线，处理 742 例患者 CT 数据
img: assets/img/projects/pedicle.jpg
importance: 1
category: research
---

设计并实现了一套全自动椎弓根螺钉轨迹规划流水线，覆盖完整胸腰椎节段（T1–L5），基于 NMDID 高分辨率数据集处理了 742 例患者 CT 数据，涵盖 17 个椎体层级。

## 方法

- 采用 Monte Carlo 采样结合区域自适应几何约束（进出点圆盘半径、锥角、皮质间隙）及基于 Euclidean Distance Transform 的体素化方法，生成 23,000+ 条候选轨迹，**规划成功率达 91.7%**，**验证通过率达 99.3%**。
- 集成 Statistical Shape Model（SSM）关键点标注以完成逐椎体路径初始化，通过基于法向量的终板过滤，实现**终板违规率为 0%**，各层级皮质间隙中位值为 2.5–3.3 mm。
- 构建多阶段批处理流水线（网格质检 → EDT 缓存 → Monte Carlo 规划 → 符号距离验证），可在单台 GPU 工作站上完成所有病例与椎体层级的处理。

**所属机构：** ARCADE Lab，约翰斯·霍普金斯大学
**导师：** Mathias Unberath 教授，博士生 Blanca Inigo Romillo
