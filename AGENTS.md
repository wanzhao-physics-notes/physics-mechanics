# Physics Notes - Agent Guidelines (AI 智能体开发指南)

欢迎阅读本项目（物理笔记）的 AI 智能体开发指南。本项目是基于 LaTeX 编写的高中物理竞赛及中科大物理课程笔记集合。作为操作本仓库的 AI 智能体（如 Cursor, Copilot 等），请严格遵循以下各项指南以确保代码风格一致性和成功编译。

## 1. 核心架构与工具链

本项目不是传统的软件工程项目，而是一个结构化、模块化的 LaTeX 排版项目。

- **核心工具**: `xelatex` (通过 `latexmk` 调度) 和 `biber` (处理参考文献)。
- **主要模块**: `src/mechanics/`（力学）、`src/electromagnetism/`（电磁学）。
- **共享资源**: `src/common/` 存放公共样式（`.sty`）与参考文献库（`.bib`）。

## 2. 构建、清理与测试命令

LaTeX 项目的“测试”即为无致命错误的**成功编译**。当用户要求“运行测试”、“检查”或“编译”时，请执行以下操作：

### 编译单个模块（例如：力学模块）

```bash
# 必须先进入对应模块的目录
cd src/mechanics
# 使用 xelatex 进行完整编译（会自动调用 biber 处理引用和目录）
latexmk -xelatex mechanics_main.**tex**
```

### 清理辅助文件

编译产生的中间文件（`.aux`, `.log`, `.fls` 等）在需要强制重新编译或遇到玄学报错时应清理：

```bash
cd src/mechanics
latexmk -c
```

> **注意**：如果开发者通过 VS Code 的 LaTeX Workshop 插件编译，系统配置文件 `.vscode/settings.json` 会将输出重定向至 `build/` 目录。使用命令行 `latexmk` 时，请关注当前目录下生成的 PDF 或 `build/` 目录中的产物。

## 3. 代码风格与排版准则

### 3.1 文件组织与导入规范

- **子文件包含**: 禁止使用传统的 `\input` 或 `\include`。本仓库已配置 `import` 宏包，请统一使用 `\subincludefrom{chapters/}{文件名.tex}` 或 `\subinputfrom{chapters/}{文件名.tex}` 进行子章节文件的导入。
- **文件命名**: 章节文件存放于 `chapters/` 目录下，并使用两级数字字母前缀，例如 `00_math-basics.tex`, `0A_special-relativity.tex`。

### 3.2 字体与核心排版

- **文档类**: 使用 `ctexbook` (A4纸张, UTF-8 编码, 字体集 `lxgw`)。
- **数学字体**: 启用了 `unicode-math`，并采用 `NewCMMath-Book.otf`，因此**必须**使用 `XeLaTeX` (或 `LuaLaTeX`) 引擎进行编译，绝对不要使用 `pdfLaTeX`。

### 3.3 数学与物理符号标准

不要使用随意的斜体或基础宏，请严格使用项目 `physics-notes-style.sty` 中定义的专用命令和现代宏包：

- **常数与算子**:
  - 自然对数底：使用 `\ee` (输出正体 e)
  - 圆周率：使用 `\uppi` (输出正体 $\pi$)
  - 虚数单位：使用 `\ii` 或 `\jj` (输出正体)
  - 转置符号：使用 `\trans`
  - 微分算子：使用 `\DD` (或由已引入的 `fixdif` 宏包提供的命令进行完美排版)
  - 实部/虚部：使用预置好的 `\Re` 和 `\Im`。
- **物理量单位**: 必须使用 `siunitx` 宏包（例如 `\qty{1.23}{\joule}` 或相应的 SI 规范），禁止手敲文本单位。单位间乘号已配置为点乘（`\cdot`）。
- **物理符号**: 项目启用了现代的 `physics2` 宏包替代了陈旧的 `physics` 宏包，请遵循 `physics2` 的现代书写规范。

### 3.4 环境与盒子定义

为了统一排版的美观度，项目封装了基于 `tcolorbox` 的多种物理环境（彩色提示框）。在添加相应级别的重点内容时，请优先使用以下环境：

格式样例：`\begin{definition}{中文标题}{引用标签名} ... \end{definition}`

- `definition`：**定义**（深海蓝 - 沉稳，打地基）
- `principle`：**原理**（宝蓝 - 核心的物理原理）
- `law`：**定律**（森林绿 - 揭示自然法则）
- `theorem`：**定理**（栗红色 - 重要的数学结论，醒目）
- `example`：**例题**（凫蓝 - 启发思考）
- `note`：**注释/易错点**（焦橙色 - 提醒注意）
- `physcite`：**文献引用**（梅紫 - 经典文献或名言）
- `physlist`：**结构化列举**（石板灰 - 结构清晰）

*提示*：每章最前的介绍和导读内容应当包裹在 `intro` 环境中（楷体、灰色）。

### 3.5 交叉引用与标签

- **引用命令**: 使用 `cleveref` 宏包的 `\cref{}` 或 `\Cref{}` 替代传统的 `\ref{}`。它会自动根据上下文补充“图”、“表”、“公式”等中文前缀，无需手动输入。
- **标签命名格式**:
  - 公式：`eq:描述` (例如 `\label{eq:newton-second-law}`)
  - 图形：`fig:描述`
  - 表格：`tab:描述`
  - 章节：`sec:描述` 或 `chap:描述`

## 4. 错误处理与日志分析

- **忽略次要警告**: 项目中 `\hfuzz`、`\vfuzz`、`\hbadness` 等均已被调高（10000pt 或 10000），这意味着微小的排版溢出（Overfull/Underfull `\hbox`）警告将被极大抑制。**你不需要**花时间去微调排版以消除盒子的溢出警告。
- **处理致命错误**: 遇到 `Undefined control sequence`（未定义命令）、`Missing $ inserted`（数学环境缺失）、`Runaway argument` 或由于缺失 `biber` 引用的报错时，你必须停下来修复。
- **图片与绘图**: 图片建议放置在模块特定的 `figures/` 目录下。项目中启用了 `tikz` 及其常用库（`calc`, `angles`, `quotes`, `patterns` 等），如果可能，遇到简单的物理原理图示请优先使用 `tikz` 直接绘制。

## 5. 现有规则与协同说明

- **规则覆盖**: 经扫描，本仓库目前不包含传统的 Cursor 规则 (`.cursorrules`, `.cursor/rules/`) 或 Copilot 指令 (`.github/copilot-instructions.md`)。因此，本 `AGENTS.md` 即为目前所有 AI 助手的权威行为与风格准则。
- **开发心智模型**:
  1. **观察**: 修改前先查阅 `src/common/styles/physics-notes-style.sty` 里的已有定义。
  2. **模仿**: 严格模仿目标文件上下文中已有的公式格式和排版结构。
  3. **验证**: 做完关键段落的撰写后，通过 `latexmk -xelatex` 跑一遍当前主文件，确保没有任何 Error 中断编译过程。
