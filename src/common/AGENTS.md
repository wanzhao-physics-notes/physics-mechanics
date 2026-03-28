# AGENTS.md — src/common

## 概述

全项目共享资源目录，包含统一样式包和参考文献库。所有书籍模块通过 `.latexmkrc` 中的 `TEXINPUTS` 和 `BIBINPUTS` 环境变量引用本目录。

> **编码习惯与命名规范**：见根目录 `AGENTS.md`。

## 结构

```text
common/
├── bib/
│   └── references.bib               # 统一参考文献库
└── styles/
    └── physics-notes-style.sty      # 231 行共享样式包
```

## 参考文献（`references.bib`）

- 样式：`gb7714-2015`（中国国标引用格式）
- 后端：biber
- 内容：中国大学物理教材（舒幼生《力学》、赵凯华《电磁学》等）
- 引用方式：`\cite{key}`，书目在各书 `\printbibliography` 处输出

## 样式包（`physics-notes-style.sty`）

231 行，全项目唯一样式文件。主要功能：

### 组织结构（按注释分节）

1. **预配置选项** — 页面布局、目录深度
2. **基础与工具宏包** — amsmath, unicode-math, siunitx, cleveref 等
3. **数学字体设置** — XITS Math，自定义正体常数宏（`\e`, `\uppi`, `\ii`, `\jj`, `\trans`, `\DD`）
4. **物理量单位** — siunitx 配置（`\cdot` 连接单位）
5. **定理类环境** — `\NewPhysicsBox` 工厂宏，批量定义 8 种彩色盒子环境
6. **交叉引用** — cleveref 中文引用名配置
7. **章节引言** — `intro` 环境（灰色边框，楷书字体）
8. **证毕符号** — `\square`, `\blacksquare`

### 修改注意事项

- 修改此文件影响**所有书籍**，务必在各模块中测试编译
- 新增宏包应放在对应分节注释下
- 新增定理环境使用 `\NewPhysicsBox` 工厂宏
- 注释使用中文 + 编号分节格式

## WHERE TO LOOK

| 任务             | 位置                                                 |
| ---------------- | ---------------------------------------------------- |
| 添加新宏包       | `styles/physics-notes-style.sty` 对应分节            |
| 添加新定理环境   | `styles/physics-notes-style.sty` 中 `\NewPhysicsBox` |
| 添加新数学符号宏 | `styles/physics-notes-style.sty` 数学字体设置节      |
| 添加参考文献条目 | `bib/references.bib`                                 |
| 修改引用样式     | 各书 `*_main.tex` 中 BibLaTeX 选项                   |
