# AGENTS.md — src/electromagnetism

## 概述

电磁学笔记 (Electromagnetism)，包含电磁学基本理论和拔高内容。作者許秋迟（USTC）。

> **编码习惯与命名规范**：见根目录 `AGENTS.md`。本文件仅包含电磁学模块的特有信息。

## 结构

```text
electromagnetism/
├── electromagnetism_main.tex    # 主入口
├── .latexmkrc                   # 构建配置
├── chapters/
│   ├── titlepage.tex            # 封面
│   ├── 99_preface-overview.tex  # 前言
│   ├── 00_math-basics.tex       # 第 0 章：数学基础
│   ├── 01_electrostatics.tex    # 第 1 章：静电学
│   └── 02_static-electric-field-in-conductors-and-dielectrics.tex  # 第 2 章
├── figures/                     # 图片（按章节子目录，暂空）
└── build/                       # 编译输出（gitignore）
```

## 章节内容

| 章节    | 文件                                                         | 内容                   |
| ------- | ------------------------------------------------------------ | ---------------------- |
| 第 0 章 | `00_math-basics.tex`                                         | 电磁学所需数学基础     |
| 第 1 章 | `01_electrostatics.tex`                                      | 静电学基本理论         |
| 第 2 章 | `02_static-electric-field-in-conductors-and-dielectrics.tex` | 导体与电介质中的静电场 |

## WHERE TO LOOK

| 任务         | 位置                                                                                        |
| ------------ | ------------------------------------------------------------------------------------------- |
| 添加新章节   | `chapters/` 下新建 `NN_topic-name.tex`，在 `electromagnetism_main.tex` 中 `\subincludefrom` |
| 添加图片     | `figures/XX_topic-name/` 下新建 `fig_name.tex`                                              |
| 修改章节顺序 | `electromagnetism_main.tex` 中调整 `\subincludefrom` 顺序                                   |
| 修改编译配置 | `.latexmkrc`                                                                                |
| 共享样式/宏  | `../common/styles/physics-notes-style.sty`                                                  |
| 参考文献     | `../common/bib/references.bib`                                                              |

## 构建命令

```bash
latexmk -pdf electromagnetism_main.tex  # 编译 PDF（输出至 ./build/）
latexmk -c                              # 清理编译产物
```
