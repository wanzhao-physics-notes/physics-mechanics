# 物理笔记项目 Agent 开发指南 (AGENTS.md)

## 1. 项目概览与技术栈

本项目是一个采用 LaTeX 编写的高中/大学物理竞赛及课程笔记集合。

- **主体类**：基于 `ctexbook` (`a4paper`, `UTF8`, `fontset=lxgw`)
- **编译引擎**：推荐使用 **XeLaTeX**（因涉及 `fontspec`, `unicode-math` 和中文字体加载）
- **模块化**：目前包含 `mechanics` (力学) 和 `electromagnetism` (电磁学) 等子模块
- **文献管理**：`biber` + `biblatex`
- **VSCode 支持**：使用 LaTeX Workshop 插件，编译输出目录统一配置为 `build/`

## 2. 编译、代码检查与测试指令

在 LaTeX 环境下，“测试”主要意味着能够成功通过编译并且排版符合预期。

### 2.1 全局/单模块编译（Build / Test）

要编译或测试某一个模块（例如力学 `mechanics`），请在对应模块目录下执行 `latexmk`：

```bash
cd src/mechanics
latexmk -xelatex -outdir=build mechanics_main.tex
```

如果需要带连续预览进行开发/写作：

```bash
cd src/mechanics
latexmk -pvc -xelatex -outdir=build mechanics_main.tex
```

### 2.2 运行单个文件的测试

LaTeX 并没有传统的单个单元测试。如果你只修改了某一个章节，并希望快速验证该章是否存在语法错误：

- 可临时编辑 `*_main.tex`（如 `mechanics_main.tex`），仅保留你正在编写的 `\subincludefrom{chapters/}{...}` 语句，或者临时使用 `\includeonly`。
- 然后执行上述 `latexmk` 命令进行单文件级别的快速编译验证。

### 2.3 代码格式化（Lint / Format）

代码缩进和格式建议使用 `latexindent` 保持统一：

```bash
# 格式化指定的章节文件
latexindent -w src/mechanics/chapters/01_particle-kinematics.tex
```

*注：缩进通常为 4 个空格或 Tab，并在标点后保持合理的空格以提升源码可读性。*

## 3. 代码风格与规范 (Code Style Guidelines)

### 3.1 文件组织与引入 (Imports)

- 各章节存放在 `src/<module>/chapters/` 中，**必须使用** `\subinputfrom{chapters/}{<filename>.tex}` 或 `\subincludefrom{chapters/}{<filename>.tex}` 引入，以确保相对路径在多级目录中正确解析。
- 共享的样式文件位于 `src/common/styles/physics-notes-style.sty`，在主文件中通过 `\usepackage{physics-notes-style}` 引入。
- 共享的参考文献数据位于 `src/common/bib/references.bib`，已在模块的 `.latexmkrc` 中配置环境变量自动寻址。

### 3.2 数学与物理符号 (Types & Math Conventions)

- **常数与专用算符必须正体**：
  - 自然对数底 $e$ 请使用 `\ee`。
  - 圆周率 $\pi$ 请使用 `\uppi`。
  - 虚数单位 $i$ 或 $j$ 请使用 `\ii` 或 `\jj`。
  - 矩阵/向量的转置 $T$ 请使用 `^{\trans}`。
  - 微分算符 $D$ 请使用 `\DD`。
  - 实部/虚部已重定义为正体，请直接使用 `\Re` 和 `\Im`。
- **微分符号**：本项目引入了 `fixdif` 宏包，应使用其提供的标准命令（例如直接用 `\d{x}` 或 `\dif` 输入微分 `d`）以保证完美的算符间距。
- **物理量与单位**：必须使用 `siunitx`，如 `\qty{1.2e3}{\metre\per\second}` 或 `\si{\angstrom}`，**绝不要**手动拼写斜体/正体单位或使用粗糙的文本模式控制。
- **现代物理符号**：项目加载了 `physics2` 宏包，但必须显式调用相应的模块（如 `\usephysicsmodule{ab}` 等）后方可使用特定物理命令。

### 3.3 定理、定义与提示环境 (Tcolorbox Environments)

所有结构化文本应使用项目中预定义的 `physicsbox` 派生环境（基于 tcolorbox，支持跨页 `breakable` 和智能引用）。不要手动使用 `\textbf` 配合边框来标出重点。
可用环境如下（颜色与语意已绑定）：

- `definition` (定义 - 深海蓝)
- `principle` (原理 - 宝蓝)
- `law` (定律 - 森林绿)
- `theorem` (定理 - 栗红)
- `example` (例 - 凫蓝)
- `note` (注释 - 焦橙)
- `physcite` (引用 - 梅紫)
- `physlist` (列举 - 石板灰)

**使用示例：**
（参数 1 为该环境的具体标题，参数 2 为引用标签的后缀）

```latex
\begin{definition}{质点的定义}{particle}
在某些物理问题中，当物体的形状和大小可以忽略不计时...
\end{definition}

通过 \cref{def:particle} 可知...
```

### 3.4 交叉引用与图表命名 (Naming Conventions)

- **引用指令**：不要使用原生的 `\ref{}`。**必须使用** `cleveref` 宏包提供的 `\cref{}`（小写，用于句中）或 `\Cref{}`（大写，用于句首）。它会自动加上“图”、“表”、“公式”等前缀。
- **标签前缀规范**（`\label{}` 的命名标准）：
  - 图形：`fig:` (如 `\label{fig:force-analysis}`)
  - 表格：`tab:` (如 `\label{tab:constants}`)
  - 公式：`eq:` (如 `\label{eq:newton-second}`)
  - 章节：`sec:` 或 `chap:` (如 `\label{sec:kinematics}`)
  - 特殊环境：使用上述 `physicsbox` 生成的环境会自动自带前缀（例如 `def:`, `thm:`, `law:` 等），引用时直接对应该前缀即可。

### 3.5 错误处理与调试 (Error Handling)

- 模板末尾设置了 `\hfuzz=10000pt`, `\vfuzz=10000pt` 等参数来全局屏蔽微小的 Overfull/Underfull 盒子警告。这**意味着严重的长公式截断也会被掩盖**，不会抛出显式编译失败。
- **AI 代理特别注意**：在新增大段公式后，务必确保合理使用了 `amsmath` 的 `\split`, `\align`, `\aligned` 等多行环境，避免公式写出一行之外而不报错。若涉及多行长文本或推导，请严格检查大括号的匹配情况。

## 4. Cursor / Copilot 规则集成

目前项目中暂未发现特定的 `.cursorrules` 或 `.github/copilot-instructions.md`。如果你作为 AI Agent 在此仓库中工作：

1. 请完全以此 `AGENTS.md` 为核心开发基准。
2. 无论何时生成代码，必须直接贴合第 3 节中约定的数学正体符号、引用规范以及物理宏包规范，以保持项目排版的绝对一致与专业性。
