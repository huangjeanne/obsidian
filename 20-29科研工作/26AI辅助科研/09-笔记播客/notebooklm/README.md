# AI视频生成脚本系统

## 项目简介

这是一个完整的视频生成自动化系统，支持从主题到视频组件的端到端生成。专门用于知识科普、教学视频和学术论文讲解。

## 核心功能

### 脚本与分镜头
- **脚本生成**：使用Gemini 3 Pro从主题自动生成符合格式规范的视频脚本
- **场景解析**：将Markdown脚本解析为结构化的场景数据
- **视觉映射**：自动生成Manim代码和Nano Banana提示词
- **格式验证**：确保脚本符合规范要求

### 音频生成
- **TTS合成**：使用Listenhub API生成高质量语音
- **声音克隆**：支持自定义声音克隆
- **多角色支持**：teacher/student等不同角色语音

### 视觉生成
- **封面生成**：Nano Banana自动生成视频封面和场景封面
- **数学动画**：Manim引擎生成公式推导和图表动画
- **风格模板**：5种预设风格（academic, tech, minimal, handdrawn, 3d）

### 完整工作流
- **一键生成**：从主题到完整视频组件的自动化流程
- **模块化设计**：各组件可独立使用或组合使用

## 目录结构

```
AI视频生成脚本系统/
├── README.md                    # 本文件
├── docs/                        # 文档目录
│   ├── 视频生成脚本格式规范.md   # 完整的脚本格式定义
│   └── 视频生成系统总结.md       # 系统总结文档
├── config/                      # 配置文件目录
│   ├── config.yaml              # 默认配置
│   ├── academic_talk.yaml       # 学术讲座配置
│   ├── tech_tutorial.yaml       # 科技教程配置
│   └── short_video.yaml         # 短视频配置
├── scripts/                     # 脚本工具目录
│   ├── script_generator.py      # Gemini 3 Pro脚本生成器
│   ├── scene_parser.py          # 场景解析器
│   ├── visual_mapper.py         # 视觉映射器
│   ├── config_loader.py         # 配置文件加载器
│   ├── manim_engine.py          # Manim动画引擎封装
│   ├── listenhub_client.py      # Listenhub FlowSpeech客户端
│   ├── cover_generator.py       # Nano Banana封面生成器
│   ├── video_composer.py        # FFmpeg视频合成器
│   └── video_workflow.py        # 完整工作流脚本
└── examples/                    # 示例文件
    ├── test_script.md           # 测试脚本
    ├── test_script.json         # 解析结果JSON
    └── test_scenes_list.md      # 场景列表
```

## 快速开始

### 方式1：使用配置文件（推荐）✨

配置文件让参数管理更简单，避免每次输入大量命令行选项：

```bash
# 使用预设配置文件
python3 scripts/video_workflow.py \
  --config config/academic_talk.yaml \
  --topic "什么是相对论"

# 或使用自定义配置文件
python3 scripts/video_workflow.py \
  --config my_project.yaml \
  --topic "我的主题"
```

### 方式2：完整工作流 - 包含视频合成

从主题一键生成完整视频：

```bash
python3 scripts/video_workflow.py \
  --topic "什么是相对论" \
  --style academic \
  --duration 180 \
  --compose-video \
  --video-quality high \
  --output ./my_video
```

### 方式2：使用现有脚本

```bash
python3 scripts/video_workflow.py \
  --script examples/test_script.md \
  --style academic \
  --compose-video
```

### 方式3：分步执行

#### 1. 生成视频脚本

```bash
python3 scripts/script_generator.py \
  --topic "什么是相对论" \
  --style academic \
  --duration 180 \
  --voices teacher student \
  --output examples/my_script.md
```

#### 2. 解析和验证脚本

```bash
python3 scripts/scene_parser.py examples/my_script.md \
  --validate \
  --output-json examples/my_script.json \
  --output-list examples/my_scenes.md
```

#### 3. 生成封面

```bash
python3 scripts/cover_generator.py \
  --action cover \
  --title "什么是相对论" \
  --subtitle "爱因斯坦的伟大发现" \
  --style academic \
  --output cover.png
```

#### 4. 生成Manim动画

```bash
python3 scripts/manim_engine.py \
  --formula "E = mc²" \
  --output ./animation
```

#### 5. 生成TTS音频

```bash
python3 scripts/listenhub_client.py \
  --action tts \
  --text "你好，世界" \
  --speaker-id teacher \
  --output hello.mp3
```

#### 6. 合成最终视频（FFmpeg）

```bash
# 合成单个场景（图片 + 音频 → 视频）
python3 scripts/video_composer.py compose-scene \
  --audio scene1.mp3 \
  --visual scene1.png \
  --output scene1.mp4 \
  --duration 30 \
  --aspect-ratio 16:9

# 合成完整视频（多场景 → 最终视频）
# 首先创建配置文件
cat > scenes_config.json << EOF
{
  "segments": [
    {
      "scene_number": 1,
      "audio_path": "./audio/audio_001.mp3",
      "visual_path": "./visuals/scene_001/cover.png",
      "duration": 30,
      "transition": "fade",
      "transition_duration": 0.5
    }
  ]
}
EOF

# 然后合成
python3 scripts/video_composer.py compose-full \
  --config scenes_config.json \
  --output final_video.mp4 \
  --quality high
```

## 配置要求

在 `~/.nanobanana.env` 文件中配置API密钥：

```bash
# Gemini API (用于脚本生成和Nano Banana图像生成)
GEMINI_API_KEY=your_gemini_api_key_here

# Listenhub API (用于声音克隆和TTS)
LISTENHUB_API_KEY=your_listenhub_api_key_here
```

## 安装依赖

```bash
# Python依赖
pip install pyyaml requests tqdm

# Manim (可选，用于数学动画)
pip install manim

# FFmpeg (用于视频合成)
# macOS:
brew install ffmpeg

# Ubuntu/Debian:
sudo apt update
sudo apt install ffmpeg

# Windows:
# 从 https://ffmpeg.org/download.html 下载
```

**检查 FFmpeg 是否已安装：**
```bash
ffmpeg -version
python3 scripts/video_composer.py --check-ffmpeg
```

## 风格预设

系统提供5种预设风格：

| 风格 | 适用场景 | 配色方案 |
|------|----------|----------|
| `academic` | 学术论文讲解、专业课程 | 深蓝、白色、灰色 |
| `tech` | 技术讲解、AI科普 | 深蓝、霓虹蓝、紫色 |
| `minimal` | 概念讲解、哲学思辨 | 黑白、单一强调色 |
| `handdrawn` | 轻松科普、儿童教育 | 暖色调 |
| `3d` | 高级展示、科技演示 | 金属色、高对比 |

## 视觉类型

支持11种视觉类型：

| 类型 | 说明 | 生成引擎 |
|------|------|----------|
| `title_card` | 标题卡片 | Nano Banana |
| `text_bullet` | 文字列表 | Manim |
| `formula` | 数学公式 | Manim |
| `chart` | 图表 | Nano Banana |
| `diagram` | 示意图 | Nano Banana |
| `scene` | 场景描述 | Nano Banana |
| `code` | 代码展示 | Manim |
| `quote` | 引用 | Nano Banana |
| `comparison` | 对比表格 | Nano Banana |
| `timeline` | 时间线 | Nano Banana |
| `process` | 流程图 | Nano Banana |

## 工作流选项

完整工作流支持以下选项：

```bash
python3 scripts/video_workflow.py [OPTIONS]

选项:
  --topic TEXT          视频主题（自动生成脚本）
  --script PATH         现有脚本文件路径
  --style STYLE         风格预设
  --aspect-ratio RATIO  视频比例
  --duration SECONDS    目标时长
  --voices VOICES       角色列表
  --output PATH         输出目录

生成选项:
  --generate-audio      生成音频（TTS）
  --no-audio           跳过音频生成
  --generate-visuals   生成视觉内容
  --no-visuals         跳过视觉生成
  --generate-cover     生成视频封面
  --no-cover           跳过封面生成
```

## 进度条显示

系统使用 `tqdm` 显示实时进度条，改善长时间运行任务的用户体验：

**支持的进度条位置：**
- 🔊 TTS 音频生成 - 显示每条对话的生成进度
- 🎨 视觉内容生成 - 显示场景渲染进度（Manim/Nano Banana）
- 🎬 视频合成 - 显示场景片段合成和合并进度
- ⏳ FlowSpeech 等待 - 显示 TTS 任务状态轮询

**进度条示例：**
```
生成音频: 100%|███████████| 12/12 [00:45<00:00,  3.75s/个]
生成视觉内容: 100%|███████████| 6/6 [02:30<00:00, 25.00s/个]
合成场景片段: 100%|███████████| 6/6 [00:20<00:00,  3.33s/个]
```

**安装 tqdm：**
```bash
pip install tqdm
```

如果未安装 tqdm，系统会自动降级为简单的文本进度显示。

## 配置文件系统 ✨ (新增)

使用配置文件可以简化命令行操作，统一管理项目参数。

### 配置文件格式

配置文件使用 YAML 格式：

```yaml
project:
  name: "我的视频"
  description: "视频描述"
  author: "作者名称"
  version: "1.0"

generation:
  style: "academic"          # 风格预设
  aspect_ratio: "16:9"       # 视频比例
  default_duration: 180      # 目标时长（秒）

  voices:
    teacher:
      voice_id: "speaker_001"
      speed: 1.0
      emotion: "neutral"
      description: "教师角色"

    student:
      voice_id: "speaker_002"
      speed: 1.1
      emotion: "engaged"
      description: "学生角色"

output:
  quality: "high"            # 视频质量
  add_transitions: true      # 添加转场
  add_background_music: false
```

### 配置管理工具

```bash
# 初始化配置文件
python3 scripts/config_loader.py init -o my_project.yaml

# 验证配置文件
python3 scripts/config_loader.py validate my_project.yaml

# 显示配置内容
python3 scripts/config_loader.py show my_project.yaml
```

### 预设配置模板

| 配置文件 | 适用场景 | 特点 |
|---------|----------|------|
| `config/config.yaml` | 通用 | 默认配置 |
| `config/academic_talk.yaml` | 学术讲座 | 正式、慢节奏 |
| `config/tech_tutorial.yaml` | 科技教程 | 活泼、快节奏 |
| `config/short_video.yaml` | 短视频 | 60秒、竖屏 |

### 使用配置文件

```bash
# 使用预设配置
python3 scripts/video_workflow.py \
  --config config/academic_talk.yaml \
  --topic "什么是深度学习"

# 使用自定义配置
python3 scripts/video_workflow.py \
  --config my_config.yaml \
  --topic "我的主题"

# 配置文件 + CLI覆盖（CLI优先级更高）
python3 scripts/video_workflow.py \
  --config config/short_video.yaml \
  --topic "我的主题" \
  --duration 120  # 覆盖配置文件中的60秒
```

## 示例输出

查看 `examples/` 目录中的示例脚本：

- `test_script.md` - 马铃薯AI知识中心主题脚本（6个场景，160秒）
- `test_script.json` - 解析后的结构化数据
- `test_scenes_list.md` - 场景拆分列表

## 技术栈

| 组件 | 技术 |
|------|------|
| 脚本生成 | Gemini 3 Pro API |
| TTS/声音克隆 | Listenhub API |
| 图像生成 | Nano Banana (Gemini 3 Pro) |
| 数学动画 | Manim |
| 视频合成 | FFmpeg |
| 配置管理 | YAML + PyYAML |
| 进度显示 | tqdm |
| 格式解析 | Python + 正则表达式 |
| 数据结构 | dataclasses |
| API管理 | ~/.nanobanana.env |

## 进阶功能

### Listenhub FlowSpeech TTS

Listenhub 使用 FlowSpeech 异步 API 进行语音合成：

```bash
# 列出可用的说话人
python3 scripts/listenhub_client.py --action list

# 创建语音（使用说话人ID）
python3 scripts/listenhub_client.py \
  --action tts \
  --text "你好，世界" \
  --speaker-id speaker_001 \
  --output hello.mp3

# 不等待完成（异步模式）
python3 scripts/listenhub_client.py \
  --action tts \
  --text "长文本内容" \
  --speaker-id speaker_001 \
  --no-wait
```

**注意：** FlowSpeech 是异步 API，默认会等待任务完成。使用 `--no-wait` 可以立即返回 `flowspeech_id` 进行后台处理。

### 精修场景

```bash
python3 scripts/script_generator.py \
  --refine examples/my_script.md \
  --scene 3 \
  --instruction "让对话更生动，增加具体案例"
```

### 分析脚本统计

```bash
python3 scripts/script_generator.py \
  --analyze examples/my_script.md
```

## 注意事项

### Listenhub FlowSpeech API

Listenhub 客户端使用 FlowSpeech 异步 API：

- **异步工作流**：创建任务 → 轮询状态 → 下载音频
- **预定义说话人**：使用 `get_speakers()` 获取可用说话人列表
- **API 端点**：`/v1/flowspeech`（创建）、`/v1/flowspeech/{id}`（状态）、`/v1/speakers`（列表）
- **传统声音克隆**：Listenhub 可能不支持从音频文件创建自定义语音

如需验证 API 端点，请访问官方文档：https://blog.listenhub.ai/openapi-docs

### Manim

Manim需要单独安装，推荐使用conda环境：

```bash
conda create -n manim python=3.10
conda activate manim
pip install manim
```

## 错误重试机制

系统内置了智能重试机制，提高 API 调用的稳定性和可靠性。

### 重试策略

支持 4 种重试策略：

| 策略 | 说明 | 适用场景 |
|------|------|----------|
| `exponential` | 指数退避（默认） | API 调用、网络请求 |
| `linear` | 线性退避 | 快速重试场景 |
| `fixed` | 固定间隔 | 简单稳定的重试 |
| `random` | 随机延迟 | 避免雷击群效应 |

### 配置重试

在配置文件中设置重试参数：

```yaml
retry:
  enabled: true          # 是否启用重试
  max_attempts: 3        # 最大重试次数
  base_delay: 2.0        # 基础延迟（秒）
  max_delay: 30.0        # 最大延迟（秒）
  strategy: exponential  # 重试策略
  jitter: true           # 是否添加随机抖动
  jitter_range: 0.5      # 抖动范围（±50%）
```

### 预设配置

系统提供三种预设重试配置：

- **API_RETRY_CONFIG**: API 调用（3次，2秒基础延迟，30秒最大延迟）
- **NETWORK_RETRY_CONFIG**: 网络请求（5次，1秒基础延迟，60秒最大延迟）
- **FAST_RETRY_CONFIG**: 快速重试（2次，0.5秒基础延迟，5秒最大延迟）

### 重试范围

重试机制已集成到以下模块：

- ✅ **script_generator.py** - Gemini API 脚本生成
- ✅ **listenhub_client.py** - Listenhub FlowSpeech API
- ✅ **cover_generator.py** - Nano Banana 图像生成

### 重试行为

- **可重试异常**: `TimeoutError`, `ConnectionError`, `OSError`
- **不可重试异常**: `ValueError`, `KeyError`, `TypeError`, `FileNotFoundError`
- **指数退避**: 每次重试延迟时间加倍（2s → 4s → 8s...）
- **随机抖动**: 延迟时间 ±50% 随机波动，避免多个客户端同步重试

### 重试日志

当重试发生时，系统会显示警告信息：

```
⚠️ Gemini API 调用 失败 (尝试 1/3): Connection timeout
   等待 2.3 秒后重试...
⚠️ Gemini API 调用 失败 (尝试 2/3): Connection timeout
   等待 4.7 秒后重试...
✅ Gemini API 调用 成功
```

## 下一步计划

- [x] FFmpeg视频合成器
- [x] 错误重试机制
- [ ] 自动字幕生成
- [ ] Web界面
- [ ] 更多风格模板
- [ ] 多语言支持

## 参考资料

- [Gemini API文档](https://ai.google.dev/docs)
- [Listenhub API文档](https://listenhub.ai/zh)
- [Manim文档](https://docs.manim.community/)
- [FFmpeg文档](https://ffmpeg.org/documentation.html)

---

*创建日期：2026年1月19日*
*版本：2.1*
*最后更新：新增错误重试机制、配置文件系统*
