# 🎵 如何添加音频到视频

## 快速指南（3 步完成）

### 第 1 步：生成配音（10 分钟）

**免费方案（推荐）**：
1. 打开：https://www.ttsmp3.com/text-to-speech/chinese
2. 选择声音：`zh-CN-XiaoxiaoNeural`（年轻女声）
3. 复制以下文案，分别生成 4 个音频：

**场景 1**：
```
哇！这就是传说中的草莓西红柿吗？
```
→ 下载为 `scene1.mp3`

**场景 2**：
```
看看这果形，饱满又可爱～
颜色鲜艳得让人心动！
咬一口汁水丰富，
完全自然成熟～
```
→ 下载为 `scene2.mp3`

**场景 3**：
```
最棒的是～
有机认证，健康安全！
口感评分高达 9.2 分！
甜度刚刚好，12 度甜度指数～
```
→ 下载为 `scene3.mp3`

**场景 4**：
```
还等什么？
立即抢购，
品尝来自大自然的甜蜜吧！
```
→ 下载为 `scene4.mp3`

---

### 第 2 步：下载音效（5 分钟）

1. 打开：https://pixabay.com/music/sound-effects/
2. 搜索并下载以下音效：

| 搜索关键词 | 文件名 | 用途 |
|-----------|--------|------|
| `ding chime` | attention.mp3 | 吸引注意 |
| `boing spring` | spring.mp3 | 弹簧音 |
| `pop bubble` | pop.mp3 | 弹出音 |
| `ukulele happy` | bgm.mp3 | 背景音乐 |
| `clock tick` | number_roll.mp3 | 数字滚动 |
| `button click` | button_pop.mp3 | 按钮音 |

---

### 第 3 步：放入音频文件（1 分钟）

```bash
cd "/Users/jeanne/60-69个人品牌与创作/63多媒体素材/视频创作方案/strawberry-tomato-social"

# 放置配音文件
mv ~/Downloads/scene1.mp3 public/audio/voice/
mv ~/Downloads/scene2.mp3 public/audio/voice/
mv ~/Downloads/scene3.mp3 public/audio/voice/
mv ~/Downloads/scene4.mp3 public/audio/voice/

# 放置音效文件
mv ~/Downloads/attention.mp3 public/audio/sfx/
mv ~/Downloads/spring.mp3 public/audio/sfx/
mv ~/Downloads/pop.mp3 public/audio/sfx/
mv ~/Downloads/bgm.mp3 public/audio/sfx/
mv ~/Downloads/number_roll.mp3 public/audio/sfx/
mv ~/Downloads/button_pop.mp3 public/audio/sfx/
```

---

## ✅ 完成！

刷新浏览器（http://localhost:3000），视频就会自动播放音频！

---

## 🎨 替代方案：使用 ElevenLabs（最佳效果）

### 注册账号
1. 访问：https://elevenlabs.io
2. 注册免费账号（每月 10,000 字免费额度）

### 生成音频
1. 点击 "Speech Synthesis"
2. 选择声音：
   - 推荐：`Mimi`（亚洲女声，甜美）
   - 或：`Rachel`（美国女声，清晰）
3. 输入文案（按场景）
4. 点击 "Generate"
5. 下载 MP3

### 成本估算
- 总字数：112 字
- 预计成本：约 $0.03
- 免费额度：10,000 字/月（足够使用）

---

## 📞 需要帮助？

**查看详细指南**：
```bash
cat AUDIO_GUIDE.md
```

**运行交互式脚本**：
```bash
chmod +x generate-audio.sh
./generate-audio.sh
```

---

**总时间**：15-30 分钟
**总成本**：免费（Edge TTS）或 $0.03（ElevenLabs）
