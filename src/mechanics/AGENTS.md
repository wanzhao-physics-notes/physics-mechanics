# AGENTS.md — src/mechanics

## 概述

力学笔记 (Mechanics)，包含经典力学及狭义相对论内容。作者許秋迟（USTC）。

> **编码习惯与命名规范**：见根目录 `AGENTS.md`。本文件仅包含力学模块的特有信息。

## 结构

```text
mechanics/
├── mechanics_main.tex           # 主入口
├── .latexmkrc                   # 构建配置
├── chapters/
│   ├── titlepage.tex            # 封面
│   ├── 99_preface-overview.tex  # 前言
│   ├── 99_preface-contrib.tex   # 贡献者说明
│   ├── 00_math-basics.tex       # 第 0 章：数学基础
│   ├── 01_particle-kinematics.tex  # 第 1 章：质点运动学
│   └── 0A_special-relativity.tex   # 附录 A：狭义相对论
├── figures/
│   ├── 00_math-basics/          # 数学基础图片
│   │   ├── fig_vector-addition.tex
│   │   ├── fig_vector-substraction.tex
│   │   ├── fig_scalar-product.tex
│   │   ├── fig_cross-product.tex
│   │   └── fig_mixed-product.tex
│   ├── 01_particle-kinematics/  # 质点运动学图片
│   │   ├── fig_curvature.tex
│   │   ├── fig_elliptical-orbit.tex
│   │   ├── fig_instantaneous-velocity.tex
│   │   ├── fig_natural-coordinates.tex
│   │   ├── fig_position-vector.tex
│   │   └── fig_velocity-vs-speed.tex
│   └── 0A_special-relativity/   # 狭义相对论图片（暂空）
└── build/                       # 编译输出（gitignore）
```

## 章节内容

| 章节    | 文件                         | 内容                                   |
| ------- | ---------------------------- | -------------------------------------- |
| 第 0 章 | `00_math-basics.tex`         | 矢量代数、微积分基础、坐标系           |
| 第 1 章 | `01_particle-kinematics.tex` | 位矢、速度、加速度、自然坐标系、极坐标 |
| 附录 A  | `0A_special-relativity.tex`  | 狭义相对论基础（持续更新）             |

## WHERE TO LOOK

| 任务         | 位置                                                                                 |
| ------------ | ------------------------------------------------------------------------------------ |
| 添加新章节   | `chapters/` 下新建 `NN_topic-name.tex`，在 `mechanics_main.tex` 中 `\subincludefrom` |
| 添加图片     | `figures/XX_topic-name/` 下新建 `fig_name.tex`                                       |
| 修改章节顺序 | `mechanics_main.tex` 中调整 `\subincludefrom` 顺序                                   |
| 修改编译配置 | `.latexmkrc`                                                                         |
| 共享样式/宏  | `../common/styles/physics-notes-style.sty`                                           |
| 参考文献     | `../common/bib/references.bib`                                                       |

## 构建命令

```bash
latexmk -pdf mechanics_main.tex   # 编译 PDF（输出至 ./build/）
latexmk -c                        # 清理编译产物
```
