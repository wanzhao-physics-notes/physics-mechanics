# PROJECT KNOWLEDGE BASE

**Generated:** 2026-03-18
**Commit:** 8e52bb9
**Branch:** main

## OVERVIEW

LaTeX 物理笔记项目，包含力学和电磁学笔记文档。

## STRUCTURE

```text
physics-notes/
├── README.md
├── .vscode/              # VSCode LaTeX 配置
├── .sisyphus/            # Sisyphus 任务配置
└── src/
    ├── common/           # 共享资源
    │   ├── bib/          # 参考文献
    │   └── styles/       # 样式文件
    ├── mechanics/        # 力学笔记
    │   ├── chapters/     # 章节文件 (XX_name.tex)
    │   ├── figures/      # 图片 (按章节子目录)
    │   ├── build/        # 编译输出
    │   ├── .latexmkrc    # 构建配置
    │   └── mechanics_main.tex
    └── electromagnetism/ # 电磁学笔记
        ├── chapters/
        ├── figures/
        ├── build/
        ├── .latexmkrc
        └── electromagnetism_main.tex
```

## WHERE TO LOOK

| Task       | Location                | Notes                                    |
| ---------- | ----------------------- | ---------------------------------------- |
| 编译力学   | `src/mechanics/`        | `latexmk -pdf mechanics_main.tex`        |
| 编译电磁学 | `src/electromagnetism/` | `latexmk -pdf electromagnetism_main.tex` |
| 添加章节   | `chapters/`             | 命名: `XX_topic-name.tex`                |
| 添加图片   | `figures/XX_topic/`     | 与章节对应子目录                         |
| 修改样式   | `src/common/styles/`    | 全局样式配置                             |
| 参考文献   | `src/common/bib/`       | biblatex 引用                            |

## CODE MAP

| Symbol                    | Type   | Location              | Refs | Role         |
| ------------------------- | ------ | --------------------- | ---- | ------------ |
| mechanics_main.tex        | 主文件 | src/mechanics/        | -    | 力学入口     |
| electromagnetism_main.tex | 主文件 | src/electromagnetism/ | -    | 电磁学入口   |
| .latexmkrc                | 配置   | 各模块根目录          | -    | latexmk 配置 |

## CONVENTIONS

- 章节文件命名: `XX_name.tex` (01, 02, 0A 前缀)
- 图片目录: `figures/XX_topic/` 对应章节
- 编译工具: latexmk
- 引用系统: biblatex + biber

## ANTI-PATTERNS (THIS PROJECT)

- 禁止在 `chapters/` 目录放置 .aux/.log 等编译产物
- 禁止提交 build/ 目录的编译结果

## UNIQUE STYLES

- 双文档独立编译 (力学/电磁学分开)
- 中文文档 + 英文物理术语混合

## COMMANDS

```bash
# 编译力学
cd src/mechanics && latexmk -pdf mechanics_main.tex

# 编译电磁学
cd src/electromagnetism && latexmk -pdf electromagnetism_main.tex

# 清理编译产物
latexmk -c
```

## NOTES

- 这是文档项目，非软件项目，无测试无CI
- .aux 文件出现在源码目录是构建产物遗留
