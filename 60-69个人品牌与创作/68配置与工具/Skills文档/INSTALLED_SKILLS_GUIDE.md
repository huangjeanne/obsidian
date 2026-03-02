# PAI Skills 使用指南

> 更新时间：2026-01-26
> 总计 Skills：155 个

## 📋 目录

- [🎬 视频与媒体处理](#-视频与媒体处理)
- [🖼️ 内容创作与设计](#️-内容创作与设计)
- [🔧 核心系统技能](#-核心系统技能)
- [🔬 科学研究与数据分析](#-科学研究与数据分析)
- [📝 写作与文档处理](#-写作与文档处理)
- [🌐 网络与数据获取](️-网络与数据获取)

---

## 🎬 视频与媒体处理

### 1. **remotion-best-practices** - Remotion 视频创作最佳实践
**用途**：React 视频制作框架 Remotion 的最佳实践指南

**何时使用**：
- 使用 Remotion 创建视频内容
- 需要了解 Remotion 动画、音频、视频处理
- 开发 Remotion 组合和转场效果

**使用示例**：
```
"如何在 Remotion 中添加音频？"
"Remotion 视频转场的最佳实践是什么？"
```

**文件位置**：`~/.claude/skills/remotion-best-practices/`
**核心内容**：31 个规则文件，涵盖 3D、动画、音频、字幕、转场等

---

### 2. **Youtube-clipper-skill** - YouTube 视频智能剪辑
**用途**：下载 YouTube 视频，AI 分析生成章节，自动剪辑并生成双语字幕

**核心功能**：
- 下载 YouTube 视频和字幕
- AI 分析生成精细章节（几分钟级别）
- 用户选择片段后自动剪辑
- 翻译字幕为中英双语并烧录到视频
- 生成视频总结文案

**使用示例**：
```
"帮我剪辑这个 YouTube 视频：https://youtube.com/watch?v=xxx"
"下载这个视频并生成双语字幕版本"
"分析这个 YouTube 视频的主要内容"
```

**依赖要求**：yt-dlp, FFmpeg, Python 3

**文件位置**：`~/.claude/skills/Youtube-clipper-skill/`

---

### 3. **media-downloader** - 智能媒体下载器
**用途**：根据描述自动搜索和下载图片、视频片段，支持视频自动剪辑

**核心功能**：
- 🖼️ 图片下载：从 Pexels 图库搜索高清图片
- 🎬 视频素材：获取免费商用视频片段
- 📺 YouTube 下载：支持下载和时间段裁剪
- ✂️ 智能剪辑：自动裁剪到指定长度

**使用示例**：
```
"帮我下载 5 张星空的图片"
"下载一段城市夜景的视频，30秒以内"
"下载这个 YouTube 视频的第 2 分钟到第 3 分钟"
```

**API Key**：
- YouTube：无需配置
- 图片下载：首次使用时引导配置 Pexels API Key（免费）

**文件位置**：`~/.claude/skills/media-downloader/`

---

## 🖼️ 内容创作与设计

### 4. **document-illustrator** - 文档配图生成器
**用途**：AI 智能分析文档内容，自动生成专业配图

**核心特点**：
- ✨ AI 智能归纳：自动理解文档，提取核心主题
- 🎨 格式无关：支持 Markdown、纯文本、PDF 等
- 📐 灵活比例：16:9（横屏）和 3:4（竖屏）
- 🖼️ 封面图可选：可生成概括全文的封面图
- 🎭 三种风格：
  - `gradient-glass` - 渐变玻璃卡片（科技感）
  - `ticket` - 票据风格（信息图表）
  - `vector-illustration` - 矢量插画（教育内容）

**使用示例**：
```
"帮我为这个文档生成配图：~/Documents/article.md"
"为这篇技术文档生成配图，使用玻璃卡片风格"
"生成文档配图，16:9 横屏，包含封面图"
```

**依赖要求**：
- Python 库：google-genai, pillow, python-dotenv
- API Key：Gemini API Key（首次使用时引导配置）

**文件位置**：`~/.claude/skills/document-illustrator/`

---

## 🔧 核心系统技能

### 5. **CORE** - PAI 核心系统
**用途**：PAI 系统的权威参考文档，自动加载

**何时使用**：
- 了解 PAI 系统架构
- 查看配置和工作流程
- 理解内存、代理、技能系统

**文件位置**：`~/.claude/skills/CORE/`

---

### 6. **Agents** - 自定义代理系统
**用途**：创建和管理具有特定身份和声音的自定义 AI 代理

**使用示例**：
```
"创建一个自定义代理，名叫 '技术顾问'"
"查看所有可用的代理"
```

**文件位置**：`~/.claude/skills/Agents/`

---

### 7. **Browser** - 浏览器自动化
**用途**：Web 验证、截图、UI 测试和可视化验证

**使用示例**：
```
"用浏览器打开这个网站并截图"
"测试这个页面的响应式设计"
```

**文件位置**：`~/.claude/skills/Browser/`

---

### 8. **System** - 系统管理
**用途**：PAI 系统的完整性检查、安全验证和文档更新

**使用示例**：
```
"运行系统完整性检查"
"扫描敏感信息泄露"
```

**文件位置**：`~/.claude/skills/System/`

---

## 🔬 科学研究与数据分析

### 生物信息学技能（部分常用）

- **biopython** - 生物分子序列操作
- **scanpy** - 单细胞 RNA-seq 分析
- **pdb-database** - 蛋白质 3D 结构数据库
- **uniprot-database** - 蛋白质序列数据库
- **pubmed-database** - 医学文献检索
- **arxiv-search** - arXiv 预印本搜索

### 数据科学技能

- **scientific-visualization** - 科学可视化
- **statistical-analysis** - 统计分析
- **exploratory-data-analysis** - 探索性数据分析
- **matplotlib** / **seaborn** / **plotly** - Python 绘图库

---

## 📝 写作与文档处理

### 9. **scientific-writing** - 科学论文写作
**用途**：撰写科学论文，遵循学术规范

---

### 10. **research-grants** - 研究基金申请
**用途**：撰写 NSF、NIH 等基金申请书

---

### 11. **clinical-reports** - 临床报告
**用途**：撰写病例报告等临床文档

---

### 12. **peer-review** - 手稿评审
**用途**：结构化的学术论文评审

---

## 🌐 网络与数据获取

### 13. **media-downloader** - 媒体下载
详见上方"视频与媒体处理"部分

### 14. **OSINT** - 开源情报收集
**用途**：公开来源信息收集和情报分析

---

### 15. **Recon** - 安全侦察
**用途**：漏洞赏金和安全侦察

---

### 16. **BrightData** - 网页抓取
**用途**：使用 Bright Data 进行网页抓取

---

## 📌 MCP 服务器（已配置）

### **weixin-reader** - 微信文章阅读器
**用途**：读取微信公众号文章内容

**使用示例**：
```
"请帮我总结这篇文章：https://mp.weixin.qq.com/s/xxxxx"
"使用 weixin-reader MCP 读取这篇文章"
```

**配置位置**：`~/.claude/mcp-servers.json`
**服务器路径**：`~/.claude/tools/wexin-read-mcp/`

---

## 🎯 快速参考表

| 想要... | 使用 Skill | 示例命令 |
|---------|-----------|---------|
| 剪辑 YouTube 视频 | Youtube-clipper-skill | "剪辑这个 YouTube 视频" |
| 下载图片/视频 | media-downloader | "下载 5 张星空图片" |
| 生成文档配图 | document-illustrator | "为文档生成配图" |
| 学习 Remotion | remotion-best-practices | "Remotion 如何添加音频" |
| 读取微信文章 | weixin-reader MCP | "总结这篇微信文章" |
| 浏览网页验证 | Browser | "用浏览器打开并截图" |
| 创建自定义代理 | Agents | "创建一个技术顾问代理" |
| 系统检查 | System | "运行完整性检查" |
| 科学可视化 | scientific-visualization | "创建热图" |
| 单细胞分析 | scanpy | "分析 PBMC 数据" |

---

## 💡 使用技巧

### 1. **自动触发**
大多数 skills 会根据关键词自动触发，无需显式调用：
- "帮我剪辑..." → Youtube-clipper-skill
- "下载图片..." → media-downloader
- "生成配图..." → document-illustrator

### 2. **显式调用**
如果自动触发不工作，可以明确指定：
```
"使用 Youtube-clipper-skill 剪辑这个视频"
```

### 3. **查看 Skill 详情**
```
"查看 Youtube-clipper-skill 的详细信息"
"有哪些视频处理的 skills？"
```

### 4. **API Key 配置**
某些 skills 需要配置 API Key（首次使用时会引导）：
- **document-illustrator**：Gemini API Key
- **media-downloader**：Pexels API Key（仅图片下载）

---

## 📚 查看所有 Skills

```bash
# 列出所有已安装的 skills
ls ~/.claude/skills/

# 搜索特定技能
ls ~/.claude/skills/ | grep -i "关键字"

# 查看 skill 详情
cat ~/.claude/skills/SKILL_NAME/SKILL.md
```

---

## 🔄 更新日志

**2026-01-26**
- ✅ 安装 remotion-best-practices (Remotion 最佳实践)
- ✅ 安装 Youtube-clipper-skill (YouTube 智能剪辑)
- ✅ 安装 document-illustrator (文档配图生成)
- ✅ 安装 media-downloader (智能媒体下载)
- ✅ 配置 weixin-reader MCP (微信文章阅读)

---

## 📞 获取帮助

遇到问题时：
- 直接在 Claude Code 中询问
- 查看 skill 的 SKILL.md 文件
- 检查依赖是否完整

---

**让 AI 帮你更高效地完成工作！✨**
