# AGENTS.md - src/mechanics

## OVERVIEW

力学笔记 (Mechanics)，包含经典力学及狭义相对论内容。

## STRUCTURE

```text
mechanics/
├── chapters/                    # 章节 .tex 文件
│   ├── 00_math-basics.tex      # 数学基础
│   ├── 01_particle-kinematics.tex
│   ├── 0A_special-relativity.tex
│   └── 99_preface-overview.tex
├── figures/                     # 图片 (按章节子目录)
│   ├── 00_math-basics/
│   ├── 01_particle-kinematics/
│   └── 0A_special-relativity/
├── build/                       # 编译输出
├── .latexmkrc                   # 构建配置
└── mechanics_main.tex           # 主入口
```

## WHERE TO LOOK

| Task         | Location            |
| ------------ | ------------------- |
| 添加章节     | `chapters/`         |
| 添加图片     | `figures/XX_topic/` |
| 修改编译配置 | `.latexmkrc`        |

## COMMANDS

```bash
latexmk -pdf mechanics_main.tex  # 编译 PDF
latexmk -c                       # 清理产物
```
