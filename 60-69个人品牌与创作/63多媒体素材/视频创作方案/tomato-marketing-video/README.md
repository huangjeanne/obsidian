# 口感西红柿营销视频 - Remotion 视频方案

基于 Remotion 最佳实践，创建一个带动态数据展示的西红柿营销视频。

## 📋 视频结构（总时长 30 秒，1080p @ 30fps）

### 场景 1: 产品介绍（0-8 秒，240 帧）
- ✨ 标题渐入："自然成熟 口感极佳"
- 🍅 西红柿图片从左侧滑入
- 📝 副标题打字机效果："源自生态农场，新鲜直达"

### 场景 2: 数据展示（8-20 秒，360 帧）
- 📊 动态柱状图展示营养成分
  - 维生素 C: 98% （红色柱）
  - 番茄红素: 95% （深红柱）
  - 膳食纤维: 88% （橙色柱）
- 🎯 每个柱子依次弹入（spring 动画，stagger 延迟）
- 📈 百分比数字滚动增长效果

### 场景 3: 产品优势（20-26 秒，180 帧）
- ✅ 三个优势点依次飞入
  - "零农药残留" ✓
  - "自然成熟" ✓
  - "当日采摘" ✓
- 🎨 使用高亮动画强调关键词

### 场景 4: 行动号召（26-30 秒，120 帧）
- 🛒 CTA 按钮弹簧效果弹出
- 📱 二维码淡入
- 📞 联系方式打字机效果

## 🎨 动画配置

### Spring 动画参数
```tsx
const smooth = { damping: 200 };  // 平滑无弹跳（用于数据展示）
const snappy = { damping: 20, stiffness: 200 };  // 快速（用于 UI 元素）
const bouncy = { damping: 8 };  // 弹性（用于标题）
```

### 转场效果
- 场景 1 → 2: `slide({ direction: 'from-left' })`, 15 帧
- 场景 2 → 3: `fade()`, 20 帧
- 场景 3 → 4: `wipe()`, 15 帧

## 💻 代码实现

### 主组合文件

```tsx
// src/ TomatoMarketingVideo.tsx
import { Composition } from "remotion";
import { TomatoMarketing } from "./TomatoMarketing";

export const RemotionVideo: React.FC = () => {
  return (
    <>
      <Composition
        id="TomatoMarketing"
        component={TomatoMarketing}
        durationInFrames={900} // 30秒 @ 30fps
        fps={30}
        width={1920}
        height={1080}
        defaultProps={{
          // 可以传入参数
        }}
      />
    </>
  );
};
```

### 主组件

```tsx
// src/TomatoMarketing.tsx
import {
  AbsoluteFill,
  useCurrentFrame,
  useVideoConfig,
  interpolate,
  spring,
  Easing,
} from "remotion";
import { TransitionSeries, linearTiming } from "@remotion/transitions";
import { slide } from "@remotion/transitions/slide";
import { fade } from "@remotion/transitions/fade";
import { wipe } from "@remotion/transitions/wipe";
import { Scene1Intro } from "./scenes/Scene1Intro";
import { Scene2Data } from "./scenes/Scene2Data";
import { Scene3Advantages } from "./scenes/Scene3Advantages";
import { Scene4CTA } from "./scenes/Scene4CTA";

export const TomatoMarketing: React.FC = () => {
  return (
    <AbsoluteFill style={{ backgroundColor: "#FFF8F0" }}>
      <TransitionSeries>
        {/* 场景 1: 产品介绍 */}
        <TransitionSeries.Sequence durationInFrames={240}>
          <Scene1Intro />
        </TransitionSeries.Sequence>

        <TransitionSeries.Transition
          presentation={slide({ direction: "from-left" })}
          timing={linearTiming({ durationInFrames: 15 })}
        />

        {/* 场景 2: 数据展示 */}
        <TransitionSeries.Sequence durationInFrames={360}>
          <Scene2Data />
        </TransitionSeries.Sequence>

        <TransitionSeries.Transition
          presentation={fade()}
          timing={linearTiming({ durationInFrames: 20 })}
        />

        {/* 场景 3: 产品优势 */}
        <TransitionSeries.Sequence durationInFrames={180}>
          <Scene3Advantages />
        </TransitionSeries.Sequence>

        <TransitionSeries.Transition
          presentation={wipe()}
          timing={linearTiming({ durationInFrames: 15 })}
        />

        {/* 场景 4: 行动号召 */}
        <TransitionSeries.Sequence durationInFrames={120}>
          <Scene4CTA />
        </TransitionSeries.Sequence>
      </TransitionSeries>
    </AbsoluteFill>
  );
};
```

### 场景 1: 产品介绍

```tsx
// src/scenes/Scene1Intro.tsx
import { AbsoluteFill, useCurrentFrame, useVideoConfig, interpolate, spring } from "remotion";

export const Scene1Intro: React.FC = () => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // 标题弹入动画
  const titleScale = spring({
    frame,
    fps,
    config: { damping: 8 },
  });

  const titleOpacity = interpolate(frame, [0, 30], [0, 1], {
    extrapolateRight: "clamp",
  });

  // 副标题打字机效果
  const subtitle = "源自生态农场，新鲜直达";
  const subtitleChars = Math.floor(interpolate(frame, [60, 150], [0, subtitle.length], {
    extrapolateRight: "clamp",
  }));
  const subtitleText = subtitle.slice(0, subtitleChars);

  // 西红柿图片滑入
  const tomatoX = interpolate(frame, [0, 60], [-500, 100], {
    extrapolateRight: "clamp",
  });

  return (
    <AbsoluteFill style={{ background: "linear-gradient(135deg, #FFF8F0 0%, #FFE8D6 100%)" }}>
      <div
        style={{
          display: "flex",
          flexDirection: "column",
          alignItems: "center",
          justifyContent: "center",
          height: "100%",
        }}
      >
        {/* 标题 */}
        <h1
          style={{
            fontSize: 120,
            fontWeight: "bold",
            color: "#E63946",
            transform: `scale(${titleScale})`,
            opacity: titleOpacity,
            marginBottom: 40,
          }}
        >
          自然成熟 口感极佳
        </h1>

        {/* 副标题 */}
        <h2
          style={{
            fontSize: 48,
            color: "#F4A261",
            fontFamily: "monospace",
          }}
        >
          {subtitleText}
          <span
            style={{
              opacity: frame % 60 < 30 ? 1 : 0,
              animation: "blink 1s infinite",
            }}
          >
            |
          </span>
        </h2>

        {/* 西红柿图片 */}
        <div
          style={{
            position: "absolute",
            left: tomatoX,
            top: "50%",
            transform: "translateY(-50%)",
          }}
        >
          <div
            style={{
              width: 300,
              height: 300,
              borderRadius: "50%",
              background: "radial-gradient(circle at 30% 30%, #FF6B6B, #E63946)",
              boxShadow: "0 20px 60px rgba(230, 57, 70, 0.4)",
            }}
          />
        </div>
      </div>
    </AbsoluteFill>
  );
};
```

### 场景 2: 数据可视化

```tsx
// src/scenes/Scene2Data.tsx
import { AbsoluteFill, useCurrentFrame, useVideoConfig, spring } from "remotion";

const nutritionData = [
  { label: "维生素C", value: 98, color: "#E63946" },
  { label: "番茄红素", value: 95, color: "#C1121F" },
  { label: "膳食纤维", value: 88, color: "#F4A261" },
];

export const Scene2Data: React.FC = () => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const STAGGER_DELAY = 10; // 每个柱状图延迟 10 帧

  const bars = nutritionData.map((item, i) => {
    const delay = i * STAGGER_DELAY;
    const heightProgress = spring({
      frame: frame - delay,
      fps,
      config: { damping: 200 }, // 平滑无弹跳
    });

    const height = heightProgress * item.value;
    const opacity = spring({
      frame: frame - delay - 20,
      fps,
      config: { damping: 200 },
    });

    return (
      <div key={i} style={{ display: "flex", alignItems: "center", marginBottom: 40 }}>
        {/* 标签 */}
        <div
          style={{
            width: 200,
            fontSize: 36,
            color: "#264653",
            textAlign: "right",
            marginRight: 30,
          }}
        >
          {item.label}
        </div>

        {/* 柱状图 */}
        <div
          style={{
            width: 600,
            height: 60,
            backgroundColor: "#F0F0F0",
            borderRadius: 30,
            overflow: "hidden",
          }}
        >
          <div
            style={{
              width: `${height}%`,
              height: "100%",
              backgroundColor: item.color,
              borderRadius: 30,
              transition: "width 0.3s",
            }}
          />
        </div>

        {/* 百分比数字 */}
        <div
          style={{
            width: 150,
            fontSize: 48,
            fontWeight: "bold",
            color: item.color,
            marginLeft: 30,
            opacity: Math.max(0, Math.min(1, opacity)),
          }}
        >
          {Math.floor(height)}%
        </div>
      </div>
    );
  });

  return (
    <AbsoluteFill style={{ backgroundColor: "#FFF8F0", padding: 100 }}>
      <h2
        style={{
          fontSize: 64,
          color: "#264653",
          marginBottom: 80,
          textAlign: "center",
        }}
      >
        营养价值分析
      </h2>
      <div style={{ display: "flex", flexDirection: "column", justifyContent: "center" }}>
        {bars}
      </div>
    </AbsoluteFill>
  );
};
```

### 场景 3: 产品优势

```tsx
// src/scenes/Scene3Advantages.tsx
import { AbsoluteFill, useCurrentFrame, useVideoConfig, spring, interpolate } from "remotion";

const advantages = [
  { text: "零农药残留", icon: "✓" },
  { text: "自然成熟", icon: "✓" },
  { text: "当日采摘", icon: "✓" },
];

export const Scene3Advantages: React.FC = () => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const STAGGER_DELAY = 20;

  const items = advantages.map((item, i) => {
    const delay = i * STAGGER_DELAY;
    const slideIn = spring({
      frame: frame - delay,
      fps,
      config: { damping: 20, stiffness: 200 }, // 快速
    });

    const x = interpolate(slideIn, [0, 1], [-100, 0]);
    const opacity = interpolate(slideIn, [0, 1], [0, 1]);

    return (
      <div
        key={i}
        style={{
          display: "flex",
          alignItems: "center",
          marginBottom: 40,
          transform: `translateX(${x}px)`,
          opacity,
        }}
      >
        {/* 图标 */}
        <div
          style={{
            width: 80,
            height: 80,
            borderRadius: "50%",
            backgroundColor: "#E63946",
            color: "white",
            fontSize: 48,
            display: "flex",
            alignItems: "center",
            justifyContent: "center",
            marginRight: 30,
          }}
        >
          {item.icon}
        </div>

        {/* 文本 */}
        <div
          style={{
            fontSize: 56,
            color: "#264653",
            fontWeight: "bold",
          }}
        >
          {item.text}
        </div>
      </div>
    );
  });

  return (
    <AbsoluteFill style={{ backgroundColor: "#FFF8F0", padding: 100 }}>
      <div style={{ display: "flex", flexDirection: "column", justifyContent: "center" }}>
        {items}
      </div>
    </AbsoluteFill>
  );
};
```

### 场景 4: 行动号召

```tsx
// src/scenes/Scene4CTA.tsx
import { AbsoluteFill, useCurrentFrame, useVideoConfig, spring, interpolate } from "remotion";

export const Scene4CTA: React.FC = () => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // CTA 按钮弹簧动画
  const buttonScale = spring({
    frame,
    fps,
    config: { damping: 8 }, // 弹性效果
  });

  const buttonOpacity = interpolate(frame, [0, 30], [0, 1], {
    extrapolateRight: "clamp",
  });

  // 二维码淡入
  const qrOpacity = interpolate(frame, [30, 90], [0, 1], {
    extrapolateRight: "clamp",
  });

  const qrScale = interpolate(frame, [30, 90], [0.8, 1], {
    extrapolateRight: "clamp",
  });

  // 联系方式打字机
  const contactText = "扫码订购，新鲜直达 📞 400-888-8888";
  const contactChars = Math.floor(interpolate(frame, [60, 100], [0, contactText.length], {
    extrapolateRight: "clamp",
  }));
  const contactDisplay = contactText.slice(0, contactChars);

  return (
    <AbsoluteFill style={{ backgroundColor: "#FFF8F0" }}>
      <div
        style={{
          display: "flex",
          flexDirection: "column",
          alignItems: "center",
          justifyContent: "center",
          height: "100%",
        }}
      >
        {/* CTA 按钮 */}
        <button
          style={{
            padding: "30px 80px",
            fontSize: 48,
            fontWeight: "bold",
            color: "white",
            backgroundColor: "#E63946",
            border: "none",
            borderRadius: 60,
            cursor: "pointer",
            transform: `scale(${buttonScale})`,
            opacity: buttonOpacity,
            boxShadow: "0 10px 30px rgba(230, 57, 70, 0.3)",
            marginBottom: 60,
          }}
        >
          🛒 立即订购
        </button>

        {/* 二维码 */}
        <div
          style={{
            width: 200,
            height: 200,
            backgroundColor: "white",
            padding: 10,
            borderRadius: 10,
            opacity: qrOpacity,
            transform: `scale(${qrScale})`,
            marginBottom: 40,
            display: "flex",
            alignItems: "center",
            justifyContent: "center",
            boxShadow: "0 5px 15px rgba(0, 0, 0, 0.1)",
          }}
        >
          <div style={{ fontSize: 24, color: "#E63946" }}>二维码</div>
        </div>

        {/* 联系方式 */}
        <div
          style={{
            fontSize: 36,
            color: "#264653",
            fontFamily: "monospace",
          }}
        >
          {contactDisplay}
        </div>
      </div>
    </AbsoluteFill>
  );
};
```

## 📦 安装依赖

```bash
# 安装 Remotion 转场包
npm install @remotion/transitions

# 或使用 bun
bunx remotion add @remotion/transitions
```

## 🎬 渲染视频

```bash
# 开发预览
npm start

# 渲染视频
npm run build

# 指定分辨率和帧率
npx remotion render TomatoMarketing out/video.mp4 --codec=h264 --pixel-format=yuv420p
```

## 🎨 配色方案

- **主色**: `#E63946` (鲜红)
- **辅助色**: `#F4A261` (橙)
- **深红**: `#C1121F` (深红)
- **背景**: `#FFF8F0` (米白)
- **文本**: `#264653` (深蓝绿)

## 📊 数据来源

可以替换为真实的西红柿营养数据：
- 维生素 C 含量（mg/100g）
- 番茄红素含量（mg/100g）
- 膳食纤维含量（g/100g）

## ✨ 动画最佳实践要点

1. ✅ **所有动画使用 `useCurrentFrame()` 驱动** - 不使用 CSS transition
2. ✅ **Spring 配置根据场景选择** - 数据展示用平滑，UI 用快速
3. ✅ **Stagger 延迟创造层次感** - 多个元素依次出现
4. ✅ **使用 `TransitionSeries` 管理场景** - 专业的转场效果
5. ✅ **限制数值范围使用 `extrapolateRight: 'clamp'`** - 防止溢出

## 🚀 扩展建议

- 添加背景音乐（使用 `@remotion/media-utils`）
- 添加音效（按钮点击、数据展示）
- 使用真实的产品图片
- 添加用户评价轮播
- 集成实际的购买链接

---

**基于 Remotion 最佳实践 v2.0**
