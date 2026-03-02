# 🎵 音频集成方案 - Remotion 视频配音

## 🎯 音频需求分析

**解说要求**：
- 甜美女声
- 112 字解说词
- 14 秒有效语音
- 活泼热情的语调

**音效需求**：
- 12 种不同音效
- 背景音乐（16 秒）
- 轻快活泼风格

---

## 💡 解决方案

### 方案 1: ElevenLabs TTS（推荐）⭐⭐⭐⭐⭐

**优点**：
- ✅ 最自然的中文女声
- ✅ 可调整语速、语调
- ✅ 高质量输出
- ✅ 免费额度每月 10,000 字

**步骤**：

1. **注册账号**
   - 访问：https://elevenlabs.io
   - 注册免费账号
   - 获取 API Key

2. **选择声音**
   推荐声音：
   - `Rachel` - 美国女声，甜美清晰
   - `Mimi` - 亚洲女声，温柔
   - `Brynn` - 女声，活泼

   中文声音（如果有）：
   - `Li Li` - 中文女声
   - `Mei Jia` - 中文女声

3. **生成配音**
   ```bash
   # 安装 ElevenLabs CLI
   npm install -g elevenlabs

   # 设置 API Key
   export ELEVENLABS_API_KEY="your_api_key"

   # 生成音频
   elevenlabs generate \
     --text "哇！这就是传说中的草莓西红柿吗？" \
     --voice-id "21m00Tcm4TlvDq8ikWAM" \
     --output "scene1.mp3"
   ```

4. **成本估算**
   - 112 字 ≈ 0.03 美元
   - 免费额度：10,000 字/月

---

### 方案 2: Edge TTS（免费）⭐⭐⭐

**优点**：
- ✅ 完全免费
- ✅ 无限使用
- ✅ 中文支持

**缺点**：
- ❌ 需要在线使用
- ❌ 音质一般

**步骤**：

1. **访问 Edge TTS Online**
   - https://ttsmaker.com/edge-tts/
   - 或 https://www.ttsmp3.com/text-to-speech/chinese

2. **选择声音**
   - `zh-CN-XiaoxiaoNeural` - 女声，年轻活泼
   - `zh-CN-XiaoyiNeural` - 女声，温柔

3. **输入文案**（按场景分割）
   复制 VOICEOVER_SCRIPT.md 中的文案

4. **生成并下载**
   - 生成 4 个场景的音频
   - 下载到项目 `public/audio/` 目录

---

### 方案 3: 专业配音平台（付费）⭐⭐⭐⭐

**平台推荐**：

1. **网易配音**
   - 网址：https://dubbing.163.com/
   - 价格：约 20-50 元/分钟
   - 中文女声选择多

2. **腾讯云语音合成**
   - 网址：https://cloud.tencent.com/product/tts
   - 价格：按调用次数计费
   - 100 次免费调用

3. **阿里云语音合成**
   - 网址：https://www.aliyun.com/product/nls
   - 价格：按调用次数计费
   - 有免费额度

---

## 🎵 音效资源

### 免费音效网站

1. **Freesound**
   - 网址：https://freesound.org/
   - 搜索关键词：pop, spring, button, notification
   - 需要：免费账号

2. **Pixabay Sound Effects**
   - 网址：https://pixabay.com/music/sound-effects/
   - 完全免费
   - 无需注册

3. **Zapsplat**
   - 网址：https://www.zapsplat.com/
   - 免费音效库
   - 需要：免费账号

### 推荐音效（搜索关键词）

| 音效类型 | 搜索关键词 | 时长 |
|---------|-----------|------|
| 吸引注意 | "ding notification", "chime clear" | 0.5s |
| 弹簧音 | "spring boing", "bounce pop" | 0.3s |
| 弹出音 | "pop bubble", "click soft" | 0.2s |
| 背景音乐 | "upbeat ukulele", "happy piano" | 16s |
| 数字滚动 | "tick tock", "clock tick" | 1s |
| 按钮音 | "button click", "ui pop" | 0.3s |

---

## 💻 集成到 Remotion

### 1. 创建音频目录
```bash
mkdir -p public/audio
mkdir -p public/audio/voice
mkdir -p public/audio/sfx
```

### 2. 修改项目配置

**安装音频处理包**：
```bash
npm install @remotion/media-utils
```

### 3. 更新场景组件

**示例：Scene1Intro.tsx 添加音频**
```tsx
import { AbsoluteFill, useCurrentFrame, useVideoConfig } from "remotion";
import { Audio } from "remotion";
import { staticFile } from "remotion/static-file";

export const Scene1Intro: React.FC = () => {
  return (
    <AbsoluteFill>
      {/* 原有的视觉内容 */}
      <div>...</div>

      {/* 吸引注意音效 */}
      <Audio
        src={staticFile("audio/sfx/attention.mp3")}
      />

      {/* 弹簧音效 */}
      <Audio
        src={staticFile("audio/sfx/spring.mp3")}
        startFrom={15} // 0.5秒后
      />
    </AbsoluteFill>
  );
};
```

### 4. 创建音频管理组件

创建 `src/AudioManager.tsx`：
```tsx
import { Audio } from "remotion";
import { staticFile } from "remotion/static-file";

export const AudioManager: React.FC = () => {
  return (
    <>
      {/* 背景音乐（循环） */}
      <Audio
        src={staticFile("audio/bgm.mp3")}
        volume={0.3}
      />

      {/* 场景 1 配音 */}
      <Audio
        src={staticFile("audio/voice/scene1.mp3")}
      />

      {/* 场景 2 配音 */}
      <Audio
        src={staticFile("audio/voice/scene2.mp3")}
        startFrom={120} // 4秒
      />

      {/* 场景 3 配音 */}
      <Audio
        src={staticFile("audio/voice/scene3.mp3")}
        startFrom={270} // 9秒
      />

      {/* 场景 4 配音 */}
      <Audio
        src={staticFile("audio/voice/scene4.mp3")}
        startFrom={480} // 16秒
      />
    </>
  );
};
```

### 5. 更新主组件

修改 `StrawberryTomatoSocial.tsx`：
```tsx
import { AudioManager } from "./AudioManager";

export const StrawberryTomatoSocial: React.FC = () => {
  return (
    <AbsoluteFill>
      <AudioManager /> {/* 添加音频管理 */}
      <TransitionSeries>
        {/* 原有的场景 */}
      </TransitionSeries>
    </AbsoluteFill>
  );
};
```

---

## 🚀 快速开始

### 推荐流程（最快）

1. **使用 Edge TTS 生成配音**（10 分钟）
   - 访问 https://www.ttsmp3.com/text-to-speech/chinese
   - 输入解说文案
   - 生成并下载 4 个场景音频

2. **下载免费音效**（15 分钟）
   - 访问 https://pixabay.com/music/sound-effects/
   - 搜索并下载所需音效

3. **集成到项目**（5 分钟）
   - 将音频文件放到 `public/audio/`
   - 更新组件代码

**总时间**：约 30 分钟

---

## 📝 音频文件清单

### 配音文件（需要生成）
```
public/audio/voice/
├── scene1.mp3   # 场景 1 配音（4 秒）
├── scene2.mp3   # 场景 2 配音（5 秒）
├── scene3.mp3   # 场景 3 配音（7 秒）
└── scene4.mp3   # 场景 4 配音（4 秒）
```

### 音效文件（需要下载）
```
public/audio/sfx/
├── attention.mp3     # 吸引注意音
├── spring.mp3        # 弹簧音
├── pop.mp3           # 弹出音（4个）
├── bgm.mp3           # 背景音乐
├── card_appear.mp3   # 卡片出现音
├── number_roll.mp3   # 数字滚动音
├── progress.mp3      # 进度条音
└── button_pop.mp3    # 按钮弹出音
```

---

## 🎯 下一步行动

### 立即可做
1. ✅ 使用 Edge TTS 生成配音
2. ✅ 从 Pixabay 下载音效
3. ✅ 将音频文件放入 `public/audio/`
4. ✅ 更新代码集成音频

### 需要账号
1. 注册 ElevenLabs（免费）
2. 生成更高质量配音
3. 下载专业音效

---

**创建时间**: 2026-01-27
**预计成本**: 免费（使用 Edge TTS）或 $0.03（ElevenLabs）
**预计时间**: 30-60 分钟
