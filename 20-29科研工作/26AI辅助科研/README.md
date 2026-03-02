# 科研技能统一输出目录

## 概述

所有科研相关的 skills 输出统一保存在此目录下，按技能名称和文件类型分类管理。

**目录路径**: `/Users/jeanne/20-29科研工作/AI 科研/`

## 目录结构

```
/Users/jeanne/20-29科研工作/AI 科研/
├── 01-文献综述/literature-review/     # Literature Review 产出
├── 02-科学写作/scientific-writing/     # Scientific Writing 产出
├── 03-科学图表/scientific-schematics/ # Scientific Diagrams 产出
├── 04-科学可视化/scientific-visualization/ # Data Visualization 产出
├── 05-学术PPT/ppt-master/             # PowerPoint 幻灯片
├── 06-学术幻灯片/scientific-slides/  # LaTeX Beamer 幻灯片
├── 07-引用管理/citation-management/   # Citation Management 产出
├── 08-AI生图/modelscope/              # ModelScope Image Generation 产出
├── 09-笔记播客/notebooklm/           # NotebookLM Podcasts & Reports
├── 10-科研头脑风暴/                   # Brainstorming & Critical Thinking
└── README.md                          # 本说明文件
```

## 当前文件清单

### 📄 文献综述 (01-文献综述)

| 文件名 | 描述 | 日期 |
|--------|------|------|
| `GAT作物种植应用综述.md` | 图注意力网络在作物种植生产中的应用文献综述 | 2026-01-19 |

### 🖼️ 科学图表 (03-科学图表)

| 文件名 | 描述 | 日期 |
|--------|------|------|
| `gat_architecture.jpg` | GAT (图注意力网络) 架构图 | 2026-01-19 |
| `gat_agriculture_applications.jpg` | GAT 在农业中的应用框架图 | 2026-01-19 |

## 便捷命令

### 1. 加载科研环境

在终端中运行：

```bash
source /Users/jeanne/.claude/skills/research_functions.sh
```

建议添加到 `~/.zshrc` 自动加载：

```bash
echo 'source /Users/jeanne/.claude/skills/research_functions.sh' >> ~/.zshrc
source ~/.zshrc
```

### 2. 可用命令

| 命令 | 功能 | 输出目录 |
|------|------|----------|
| `schematic <prompt>` | 生成科学图表 | 03-科学图表/ |
| `msimg <prompt>` | AI生图 (ModelScope) | 08-AI生图/ |
| `nbpodcast` | 下载 NotebookLM 播客 | 09-笔记播客/ |
| `nbreport` | 下载 NotebookLM 报告 | 09-笔记播客/ |
| `cd_research` | 进入科研根目录 | - |
| `cd_literature` | 进入文献综述目录 | 01-文献综述/ |
| `cd_writing` | 进入科学写作目录 | 02-科学写作/ |
| `cd_schematic` | 进入科学图表目录 | 03-科学图表/ |
| `cd_aiimg` | 进入AI生图目录 | 08-AI生图/ |
| `cd_notebook` | 进入笔记播客目录 | 09-笔记播客/ |
| `research_help` | 显示帮助信息 | - |

## 使用示例

### 生成科学图表

```bash
# 生成 GAT 架构图
schematic "GAT architecture with attention mechanism" --doc-type report

# 生成流程图
schematic "PRISMA flow diagram for systematic review" --doc-type journal

# 生成神经网络架构图
schematic "Transformer encoder-decoder with multi-head attention" --doc-type presentation
```

### AI 生图

```bash
# 生成农业产品海报
msimg "福建特色农产品建阳翠冠梨海报，中文文字，专业设计"

# 生成科研图表
msimg "植物细胞结构示意图，科学出版风格"
```

### NotebookLM 下载

```bash
# 下载播客到科研目录
nbpodcast

# 下载报告到科研目录
nbreport
```

## 已配置的技能列表

| 技能名称 | 便捷命令 | 说明 |
|----------|----------|------|
| scientific-schematics | `schematic` | 科学图表生成 |
| modelscope-image-gen | `msimg` | ModelScope AI生图 |
| notebooklm | `nbpodcast`, `nbreport` | 笔记播客下载 |
| literature-review | - | 文献综述（手动保存） |
| scientific-writing | - | 科学写作（手动保存） |
| ppt-master | - | 学术PPT（手动保存） |
| scientific-slides | - | 学术幻灯片（手动保存） |
| citation-management | - | 引用管理（手动保存） |

## 更新日志

**2026-01-19**
- ✅ 创建统一的科研输出目录结构
- ✅ 配置便捷命令函数
- ✅ 更新各技能 SKILL.md 添加科研目录说明
- ✅ 移动已生成的文献综述和图表到相应目录
- ✅ 清理临时文件

---

*此目录由 Claude Code 科研技能统一管理*
