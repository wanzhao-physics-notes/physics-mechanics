# AGENTS.md - src/electromagnetism

## OVERVIEW

电磁学笔记 (Electromagnetism)，包含电磁学基本理论和拔高内容。

## STRUCTURE

```text
electromagnetism/
├── chapters/                    # 章节 .tex 文件
├── figures/                     # 图片 (按章节子目录)
├── build/                       # 编译输出
├── .latexmkrc                   # 构建配置
└── electromagnetism_main.tex   # 主入口
```

## WHERE TO LOOK

| Task         | Location            |
| ------------ | ------------------- |
| 添加章节     | `chapters/`         |
| 添加图片     | `figures/XX_topic/` |
| 修改编译配置 | `.latexmkrc`        |

## COMMANDS

```bash
latexmk -pdf electromagnetism_main.tex  # 编译 PDF
latexmk -c                                 # 清理产物
```
