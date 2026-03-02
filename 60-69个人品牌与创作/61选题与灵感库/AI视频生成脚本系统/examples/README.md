# 示例输出目录

本目录包含视频生成系统的示例文件和预期输出结构。

## 目录结构

```
examples/
├── README.md                      # 本文件
├── test_script.md                 # 示例视频脚本（6个场景，160秒）
├── test_script.json               # 解析后的结构化数据
├── test_scenes_list.md            # 场景拆分列表
├── example_config.yaml             # 示例配置文件
├── quick_test.sh                  # 快速测试脚本
└── expected_output/               # 预期输出目录结构
    ├── script.md                   # 生成的脚本
    ├── audio/                      # 音频文件
    │   ├── audio_001.mp3
    │   ├── audio_002.mp3
    │   └── ...
    ├── visuals/                    # 视觉内容
    │   ├── scene_001/
    │   │   └── cover.png
    │   ├── scene_002/
    │   └── ...
    ├── covers/                     # 封面图片
    │   └── cover.png
    └── final_video.mp4             # 最终视频
```

## 示例脚本说明

### test_script.md
- **主题**: 马铃薯AI知识中心 - 基因命名问题
- **风格**: academic（学术风格）
- **时长**: 160秒（约2分40秒）
- **场景数**: 6个场景
- **角色**: teacher（教师）, student（学生）
- **视觉类型**: title_card, text_bullet, diagram, scene, comparison

### test_script.json
- 脚本解析后的结构化 JSON 数据
- 包含所有场景的元数据、对话、视觉配置
- 用于程序化处理脚本内容

### test_scenes_list.md
- 场景拆分列表格式
- 快速查看所有场景的概览
- 适合人类阅读的场景清单

## 使用示例

### 1. 从主题生成脚本
```bash
cd ..
python3 scripts/script_generator.py \
  --topic "什么是相对论" \
  --style academic \
  --duration 180 \
  --output examples/generated_script.md
```

### 2. 解析和验证脚本
```bash
python3 scripts/scene_parser.py examples/test_script.md \
  --validate \
  --output-json examples/parsed.json \
  --output-list examples/scenes.md
```

### 3. 生成封面
```bash
python3 scripts/cover_generator.py \
  --action cover \
  --title "示例视频" \
  --subtitle "使用 video-generator skill" \
  --style tech \
  --output examples/covers/example_cover.png
```

### 4. 生成音频（需要 API）
```bash
python3 scripts/listenhub_client.py \
  --action tts \
  --text "你好，这是测试音频" \
  --speaker-id teacher \
  --output examples/audio/test.mp3
```

### 5. 合成视频片段
```bash
python3 scripts/video_composer.py compose-scene \
  --audio examples/audio/test.mp3 \
  --visual examples/covers/example_cover.png \
  --output examples/scenes/scene_001.mp4 \
  --duration 30 \
  --aspect-ratio 16:9
```

## 快速测试

运行快速测试脚本：
```bash
bash examples/quick_test.sh
```

这会：
1. 检查所有依赖
2. 验证示例脚本
3. 测试配置加载
4. 显示系统状态

## 预期输出

### 脚本生成输出
```
✅ 脚本已保存到: examples/generated_script.md

📊 脚本统计:
   场景数: 6
   总时长: 180秒
```

### 视频合成输出
```
合成场景片段: 100%|████████████████████| 6/6 [00:20<00:00]
合并视频: 100%|████████████████████| 1/1 [00:05<00:00]

✅ 视频已保存到: examples/final_video.mp4
```

## 风格预设示例

### Academic（学术风格）
- 配色：深蓝、白色、灰色
- 背景：几何图形、网格
- 适用：学术论文、专业课程

### Tech（科技风格）
- 配色：深蓝、霓虹蓝、紫色
- 背景：粒子效果、光效
- 适用：技术讲解、AI科普

### Minimal（极简风格）
- 配色：黑白、单一强调色
- 背景：纯色或渐变
- 适用：概念讲解、哲学思辨

## 更多示例

### 短视频示例（9:16 竖屏）
```bash
python3 scripts/script_generator.py \
  --topic "量子纠缠一分钟科普" \
  --style minimal \
  --duration 60 \
  --aspect-ratio 9:16 \
  --output examples/short_video.md
```

### 双角色对话示例
```bash
python3 scripts/script_generator.py \
  --topic "机器学习入门" \
  --style tech \
  --voices teacher student \
  --duration 240 \
  --output examples/dialogue_video.md
```

### 使用配置文件
```bash
python3 scripts/video_workflow.py \
  --config config/short_video.yaml \
  --topic "深度学习基础" \
  --output examples/short_video_output
```
