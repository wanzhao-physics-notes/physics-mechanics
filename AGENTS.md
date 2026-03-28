# AGENTS.md — physics-notes (根目录)

## 项目概述

大学物理课程笔记，作者許秋迟（USTC）。纯 LaTeX 文档项目，无代码逻辑、无测试、无 CI/CD。
当前包含两本书：力学 (mechanics) 与 电磁学 (electromagnetism)，均持续更新中。

## 技术栈

- **文档类**：`ctexbook`（`a4paper, UTF8, fontset=lxgw`）
- **编译器**：XeLaTeX，通过 `latexmk -pdf` 构建
- **参考文献**：BibLaTeX + biber（`gb7714-2015` 样式）
- **数学字体**：`unicode-math`（XITS Math）
- **编辑器**：VSCode + LaTeX Workshop + LTEX
- **版权**：Copyright (c) 2026 許秋迟，MIT License

## 项目结构

```text
physics-notes/
├── AGENTS.md                        # ← 你在这里
├── LICENSE, README.md
└── src/
    ├── common/                      # 共享资源（详见 src/common/AGENTS.md）
    │   ├── bib/references.bib       # 统一参考文献库
    │   └── styles/physics-notes-style.sty  # 231 行共享样式包
    ├── mechanics/                   # 力学笔记（详见 src/mechanics/AGENTS.md）
    │   ├── mechanics_main.tex       # 主入口
    │   ├── chapters/                # 6 个章节文件
    │   ├── figures/                 # 按章节分子目录
    │   └── .latexmkrc
    └── electromagnetism/            # 电磁学笔记（详见 src/electromagnetism/AGENTS.md）
        ├── electromagnetism_main.tex
        ├── chapters/                # 5 个章节文件
        ├── figures/
        └── .latexmkrc
```

## 构建命令

```bash
# 在各模块目录下执行（src/mechanics/ 或 src/electromagnetism/）
latexmk -pdf {module}_main.tex   # 编译 PDF（输出至 ./build/）
latexmk -c                       # 清理编译产物
```

构建配置位于各模块的 `.latexmkrc`：`$pdf_mode = 1`，通过 `TEXINPUTS` 和 `BIBINPUTS` 环境变量引用 `src/common/` 下的共享资源。

---

## 编码习惯与命名规范

> **所有子目录 AGENTS.md 均引用本节。这是项目的唯一编码规范来源。**

### 文件命名

| 类型          | 格式                                                | 示例                                                |
| ------------- | --------------------------------------------------- | --------------------------------------------------- |
| 章节文件      | `NN_topic-name.tex`（两位数前缀 + kebab-case 英文） | `00_math-basics.tex`, `01_particle-kinematics.tex`  |
| 附录/特殊章节 | `0A_topic-name.tex`（十六进制风格前缀）             | `0A_special-relativity.tex`                         |
| 前言文件      | `99_preface-xxx.tex`                                | `99_preface-overview.tex`, `99_preface-contrib.tex` |
| 封面          | `titlepage.tex`（无前缀）                           | —                                                   |
| 图片文件      | `fig_topic-name.tex`（`fig_` 前缀 + kebab-case）    | `fig_vector-addition.tex`                           |
| 图片目录      | `figures/XX_topic-name/`（与章节前缀对应）          | `figures/00_math-basics/`                           |
| 主入口        | `{module}_main.tex`                                 | `mechanics_main.tex`                                |
| 样式包        | kebab-case `.sty`                                   | `physics-notes-style.sty`                           |

### LaTeX 标签（`\label{}`）规范

**统一格式**：`\label{prefix:english-kebab-case}`

| 前缀      | 用途 | 示例                               |
| --------- | ---- | ---------------------------------- |
| `chap:`   | 章   | `\label{chap:particle-kinematics}` |
| `sec:`    | 节   | `\label{sec:velocity}`             |
| `subsec:` | 小节 | `\label{subsec:angular-velocity}`  |
| `fig:`    | 图   | `\label{fig:vector-addition}`      |
| `tab:`    | 表   | `\label{tab:unit-conversions}`     |
| `eq:`     | 公式 | `\label{eq:newton-second-law}`     |

交叉引用**必须**使用 `\cref{}`（cleveref），**禁止**使用原生 `\ref{}`。中文引用名已在 `.sty` 中配置。

### 定理类环境

通过 `\NewPhysicsBox` 工厂宏批量定义，格式：

```latex
\begin{environment}{中文标题}{english-label}
    内容...
\end{environment}
```

| 环境名       | 中文名 | 颜色        | 标签前缀   |
| ------------ | ------ | ----------- | ---------- |
| `definition` | 定义   | NavyBlue    | `def`      |
| `principle`  | 原理   | RoyalBlue   | `princ`    |
| `law`        | 定律   | ForestGreen | `law`      |
| `theorem`    | 定理   | Maroon      | `thm`      |
| `example`    | 例     | TealBlue    | `ex`       |
| `note`       | 注释   | BurntOrange | `note`     |
| `physcite`   | 引用   | Plum        | `physcite` |
| `physlist`   | 列举   | SlateGray   | `physlist` |

另有 `intro` 环境（灰色边框，楷书字体），用于章节引言。

### 数学符号约定

| 宏                        | 用途                    | 说明                  |
| ------------------------- | ----------------------- | --------------------- |
| `\symbfit{r}`             | 粗斜体矢量              | unicode-math          |
| `\symbfit{e}_x`           | 单位矢量                | —                     |
| `\e`                      | 自然常数 e              | 正体                  |
| `\uppi`                   | 圆周率 π                | 正体                  |
| `\ii`, `\jj`              | 虚数单位                | 正体                  |
| `\trans`                  | 转置 T                  | 正体                  |
| `\DD`                     | 微分算子 D              | 正体                  |
| `\d`                      | 微分 d（fixdif 包）     | 正体                  |
| `\symup{A}`               | 正体数学符号            | 排列组合等            |
| `\square`, `\blacksquare` | 证毕符号                | —                     |
| `\qty{}{}`, `\unit{}`     | 物理量与单位（siunitx） | 单位间用 `\cdot` 连接 |

### 文档结构模板

每本书的 `*_main.tex` 遵循固定结构：

```latex
\documentclass[a4paper,UTF8,fontset=lxgw]{ctexbook}
\usepackage{physics-notes-style}     % 唯一共享样式包
\addbibresource{references.bib}

\begin{document}
\subincludefrom{chapters/}{titlepage.tex}

\frontmatter
\subincludefrom{chapters/}{99_preface-overview.tex}
\tableofcontents

\mainmatter
\setcounter{chapter}{-1}             % 章节从 0 开始
\subincludefrom{chapters/}{00_math-basics.tex}
\subincludefrom{chapters/}{01_particle-kinematics.tex}
% ...

\backmatter
\printbibliography[heading=bibliography,title=参考文献]
\end{document}
```

关键点：

- 章节编号从 -1 开始（`\setcounter{chapter}{-1}`），使第 0 章为数学基础
- 章节通过 `\subincludefrom{chapters/}{}` 引入
- 图片通过 `\subinputfrom{../figures/XX_topic/}{fig_name.tex}` 引入

### 排版格式

- **缩进**：4 空格（全项目统一）
- **行尾**：LF（`.gitattributes` 强制）

### 注释风格

- 正文注释使用中文：`% 矢量的加法示意图`
- 样式包中使用编号分节注释：`% 1. 预配置选项`，`% 2. 基础与工具宏包`
- 分隔线：`% ============...`
- 修复标注：`【修复】说明`
- 优化标注：`【优化说明】说明`

### 引号规范

**禁止**使用原生引号（`""`, `''`）。必须使用 LaTeX 引号命令或 csquotes 提供的宏。

### Git 提交规范

- 提交消息格式：`[tag] 描述`，方括号标签前缀
- 常用标签：`[update]`、`[Fix]`、`[add]` 等
- 消息语言：中英文混用

## WHERE TO LOOK

| 任务                 | 位置                                        |
| -------------------- | ------------------------------------------- |
| 添加新书/模块        | `src/` 下新建目录，参照现有模块结构         |
| 修改共享样式         | `src/common/styles/physics-notes-style.sty` |
| 添加参考文献         | `src/common/bib/references.bib`             |
| 修改编译配置         | 各模块下 `.latexmkrc`                       |
| 编辑器配置           | `.vscode/settings.json`                     |
| 了解某本书的具体内容 | 对应目录下的 `AGENTS.md`                    |
