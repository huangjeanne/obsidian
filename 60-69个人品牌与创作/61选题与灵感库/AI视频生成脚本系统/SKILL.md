---
name: video-generator
description: AI automated video generation system for educational explainer videos and tutorial content. Use when user wants to generate video from topic create explainer video make tutorial video produce educational content convert script to video or automate video production. Full workflow includes script generation via Gemini TTS audio via Listenhub cover images via Nano Banana and video composition via FFmpeg. Supports Chinese and English content multiple visual styles academic tech minimal handdrawn and 3D and aspect ratios 16x9 and 9x16. Trigger phrases include generate video make video create video 生成视频 制作视频 创建视频 视频制作 科普视频 教学视频 教程视频 tutorial video explainer video educational video script to video and topic to video.
---

# AI Video Generator

## Overview

End-to-end automated video generation system for creating educational, tutorial, and knowledge-based videos. Transforms a simple topic into a complete video with script, narration, visuals, and professional composition.

**Use this skill when:**
- User wants to create an explainer or tutorial video from a topic
- User has a script and wants to convert it to video
- User needs audio narration (TTS) with voice synthesis
- User wants to generate cover images or scene visuals
- User needs to compose audio and visuals into a final video

## Quick Start

```bash
# From any topic
python3 scripts/video_workflow.py --topic "What is quantum computing?" --style tech

# From existing script
python3 scripts/video_workflow.py --script my_script.md --compose-video

# With config file
python3 scripts/video_workflow.py --config config/academic_talk.yaml --topic "Your Topic"
```

## Core Capabilities

### 1. Script Generation (`script_generator.py`)

Generate video scripts from topic descriptions using Gemini 3 Pro.

```bash
python3 scripts/script_generator.py \
  --topic "Explain machine learning" \
  --style academic \
  --duration 180 \
  --voices teacher student \
  --output script.md
```

**Style presets:**
- `academic` - Academic lectures (深蓝、白色、灰色)
- `tech` - Tech tutorials (深蓝、霓虹蓝、紫色)
- `minimal` - Minimalist concepts (黑白、单一强调色)
- `handdrawn` - Educational content (暖色调、手绘风格)
- `3d` - High-end demos (金属色、高对比)

**Refine existing scenes:**
```bash
python3 scripts/script_generator.py \
  --refine script.md \
  --scene 3 \
  --instruction "Make dialogue more engaging with examples"
```

**Analyze script:**
```bash
python3 scripts/script_generator.py --analyze script.md
```

### 2. Audio Generation (`listenhub_client.py`)

Generate TTS audio using Listenhub FlowSpeech API.

```bash
# List available speakers
python3 scripts/listenhub_client.py --action list

# Generate audio
python3 scripts/listenhub_client.py \
  --action tts \
  --text "Hello, world!" \
  --speaker-id teacher \
  --output hello.mp3

# Batch generate
python3 scripts/listenhub_client.py \
  --action tts \
  --text "Long text content..." \
  --speaker-id teacher \
  --no-wait  # Returns immediately with flowspeech_id
```

### 3. Visual Generation (`cover_generator.py`)

Generate cover images and scene visuals using Nano Banana.

```bash
# Generate video cover
python3 scripts/cover_generator.py \
  --action cover \
  --title "Introduction to AI" \
  --subtitle "Understanding Neural Networks" \
  --style tech \
  --aspect-ratio 16:9 \
  --output cover.png

# Batch generate scene covers
python3 scripts/cover_generator.py \
  --action batch \
  --scenes scenes.json \
  --style academic
```

### 4. Video Composition (`video_composer.py`)

Compose audio and visuals into final video using FFmpeg.

```bash
# Compose single scene (image + audio → video)
python3 scripts/video_composer.py compose-scene \
  --audio scene1.mp3 \
  --visual scene1.png \
  --output scene1.mp4 \
  --duration 30 \
  --aspect-ratio 16:9

# Compose full video from config
python3 scripts/video_composer.py compose-full \
  --config scenes_config.json \
  --output final_video.mp4 \
  --quality high \
  --add-transitions
```

**Quality presets:**
- `low` - CRF 28, faster encoding
- `medium` - CRF 23, medium quality
- `high` - CRF 18, high quality
- `ultra` - CRF 15, maximum quality

**Transition types:**
- `none` - No transition
- `fade` - Crossfade
- `slide` - Slide transition
- `zoom` - Zoom transition
- `wipe` - Wipe transition
- `dissolve` - Dissolve effect

### 5. Complete Workflow (`video_workflow.py`)

Orchestrate all components from topic to final video.

```bash
python3 scripts/video_workflow.py \
  --topic "Your video topic" \
  --style academic \
  --duration 180 \
  --generate-audio \
  --generate-visuals \
  --generate-cover \
  --compose-video \
  --video-quality high \
  --output ./my_video
```

**With config file:**
```bash
python3 scripts/video_workflow.py \
  --config config/academic_talk.yaml \
  --topic "Your topic"
```

## Configuration File System

Create reusable configuration files for consistent video generation.

**Initialize config:**
```bash
python3 scripts/config_loader.py init -o my_project.yaml
```

**Config structure:**
```yaml
project:
  name: "My Video"
  description: "Video description"
  author: "Author Name"
  version: "1.0"

generation:
  style: "academic"          # Style preset
  aspect_ratio: "16:9"       # Video ratio
  default_duration: 180      # Target duration (seconds)

  voices:
    teacher:
      voice_id: "speaker_001"
      speed: 1.0
      emotion: "neutral"
      description: "Teacher role"

output:
  quality: "high"            # Video quality
  add_transitions: true      # Add transitions
  add_background_music: false

retry:
  enabled: true
  max_attempts: 3
  base_delay: 2.0
  max_delay: 30.0
  strategy: exponential
  jitter: true
```

**Validate config:**
```bash
python3 scripts/config_loader.py validate my_config.yaml
```

**Show config:**
```bash
python3 scripts/config_loader.py show my_config.yaml
```

## Preset Configurations

| Config | Use Case | Style | Duration | Ratio |
|--------|----------|-------|----------|-------|
| `config/config.yaml` | General | academic | 180s | 16:9 |
| `config/academic_talk.yaml` | Academic lecture | Formal | 300s | 16:9 |
| `config/tech_tutorial.yaml` | Tech tutorial | Lively | 240s | 16:9 |
| `config/short_video.yaml` | Short video | Minimal | 60s | 9:16 |

## Error Retry Mechanism

Automatic retry with exponential backoff for API calls:

- **Retry strategies**: exponential, linear, fixed, random
- **Default**: 3 attempts, 2s base delay, 30s max delay
- **Applies to**: Gemini API, Listenhub API, Nano Banana API
- **Configurable** via YAML config file

## Script Format

Generated scripts follow this structure:

```markdown
---
title: "Video Title"
duration: 180
aspect_ratio: "16:9"
style: "academic"
voices:
  narrator: {...}
---

## Scene 1: Opening

**Metadata**:
- Role: narrator
- Emotion: enthusiastic
- Duration: 30 seconds

**Dialogue**:
Narrator: Welcome to this video about...

**Visual Content**:
Type: title_card
Content: Detailed description...
```

## Visual Types

| Type | Description | Generator |
|------|-------------|-----------|
| `title_card` | Title card | Nano Banana |
| `text_bullet` | Bullet points | Manim |
| `formula` | Math formulas | Manim |
| `chart` | Charts/graphs | Nano Banana |
| `diagram` | Diagrams | Nano Banana |
| `scene` | Scene illustration | Nano Banana |
| `code` | Code display | Manim |

## Environment Setup

**API Keys** (in `~/.nanobanana.env`):
```bash
GEMINI_API_KEY=your_key_here
LISTENHUB_API_KEY=your_key_here
```

**Dependencies:**
```bash
# Python packages
pip install pyyaml requests tqdm google-genai

# FFmpeg (required)
brew install ffmpeg  # macOS
# or
sudo apt install ffmpeg  # Linux

# Manim (optional, for math animations)
pip install manim
```

**Check FFmpeg:**
```bash
ffmpeg -version
python3 scripts/video_composer.py --check-ffmpeg
```

## Working Directory

The skill scripts are located at:
```
/Users/jeanne/60-69个人品牌与创作/61选题与灵感库/AI视频生成脚本系统/
```

Always navigate to this directory before running scripts, or use absolute paths.

## Usage Patterns

**Pattern 1: Topic to Video (Full Automation)**
```bash
cd /Users/jeanne/60-69个人品牌与创作/61选题与灵感库/AI视频生成脚本系统/
python3 scripts/video_workflow.py --topic "Subject" --compose-video --output ./output
```

**Pattern 2: Script to Video**
```bash
# User has existing script
python3 scripts/video_workflow.py --script script.md --compose-video
```

**Pattern 3: Component Only**
```bash
# Generate just the script
python3 scripts/script_generator.py --topic "Subject" --output script.md

# Generate just the audio
python3 scripts/listenhub_client.py --action tts --text "..." --output audio.mp3

# Generate just the cover
python3 scripts/cover_generator.py --action cover --title "Title" --output cover.png
```

**Pattern 4: Custom Workflow**
```bash
# Generate script → Review → Refine specific scenes → Generate components → Compose
python3 scripts/script_generator.py --topic "Subject" --output script.md
# Review script.md
python3 scripts/script_generator.py --refine script.md --scene 2 --instruction "Add examples"
# Continue with generation...
```

## Troubleshooting

**FFmpeg not found:**
```bash
brew install ffmpeg  # macOS
# or verify with: ffmpeg -version
```

**API key errors:**
```bash
# Check ~/.nanobanana.env exists and contains keys
cat ~/.nanobanana.env
```

**Manim errors:**
```bash
# Manim is optional, install if needed
pip install manim
```

**Import errors:**
```bash
# Install dependencies
pip install pyyaml requests tqdm google-genai
```

## Advanced Features

**Progress bars**: Automatic with tqdm (install for progress display)

**Retry mechanism**: Configurable via YAML, automatic for network failures

**Batch processing**: Generate multiple scenes in parallel

**Quality presets**: low/medium/high/ultra for video output

**Style presets**: academic/tech/minimal/handdrawn/3d for visual consistency

**Aspect ratios**: 16:9 (landscape), 9:16 (vertical/shorts)

## References

- `config/` - Preset configuration files
- `examples/` - Example scripts and outputs
- `docs/视频生成脚本格式规范.md` - Complete script format specification
- `docs/视频生成系统总结.md` - System overview and architecture

## Quick Health Check

Run the dependency checker to verify your system is ready:

```bash
cd ~/60-69个人品牌与创作/61选题与灵感库/AI视频生成脚本系统/
python3 scripts/check_dependencies.py
```

This will verify:
- ✓ FFmpeg installation
- ✓ Python dependencies (pyyaml, requests, tqdm, google-genai)
- ✓ API keys configuration (~/.nanobanana.env)

All checks must pass for full video generation workflow.

## Quick Test Commands

Run the comprehensive test script to verify all functionality:

```bash
cd ~/60-69个人品牌与创作/61选题与灵感库/AI视频生成脚本系统/
bash examples/quick_test.sh
```

This will verify:
- ✓ System dependencies (FFmpeg, Python packages)
- ✓ Example script validation
- ✓ Configuration file loading
- ✓ Script generator interface
- ✓ Video composer functionality

### Individual Quick Tests

**Test dependencies only:**
```bash
cd ~/60-69个人品牌与创作/61选题与灵感库/AI视频生成脚本系统/
python3 scripts/check_dependencies.py
```

**Validate a script:**
```bash
python3 scripts/scene_parser.py examples/test_script.md --validate
```

**Validate a config:**
```bash
python3 scripts/config_loader.py validate config/short_video.yaml
```

**Test FFmpeg:**
```bash
python3 scripts/video_composer.py --check-ffmpeg
```
