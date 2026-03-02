# 🍅 西红柿营销视频 - Remotion 快速参考

## 🎯 视频规格

- **总时长**: 30 秒（900 帧 @ 30fps）
- **分辨率**: 1920x1080 (Full HD)
- **配色**: 红色系（#E63946, #F4A261）

## 📬 4 个场景

### 场景 1: 产品介绍（0-8秒）
- 标题弹入：spring({ damping: 8 })
- 副标题打字机效果
- 西红柿图片滑入

### 场景 2: 数据展示（8-20秒）
- 3 个营养数据柱状图
- Stagger 延迟：10 帧
- Spring 配置：{ damping: 200 } （平滑）

### 场景 3: 产品优势（20-26秒）
- 3 个优势点飞入
- Spring 配置：{ damping: 20, stiffness: 200 } （快速）

### 场景 4: 行动号召（26-30秒）
- CTA 按钮弹簧弹出
- 二维码淡入
- 联系方式打字机

## 🎨 核心 Spring 配置

```tsx
// 平滑（数据展示）
const smooth = { damping: 200 };

// 快速（UI 元素）
const snappy = { damping: 20, stiffness: 200 };

// 弹性（标题）
const bouncy = { damping: 8 };
```

## 🔄 转场效果

```tsx
import { slide } from '@remotion/transitions/slide';
import { fade } from '@remotion/transitions/fade';
import { wipe } from '@remotion/transitions/wipe';

// 使用
<TransitionSeries.Transition
  presentation={slide({ direction: 'from-left' })}
  timing={linearTiming({ durationInFrames: 15 })}
/>
```

## 📊 动画数据结构

```tsx
// 营养数据
const nutritionData = [
  { label: "维生素C", value: 98, color: "#E63946" },
  { label: "番茄红素", value: 95, color: "#C1121F" },
  { label: "膳食纤维", value: 88, color: "#F4A261" },
];

// 优势点
const advantages = [
  { text: "零农药残留", icon: "✓" },
  { text: "自然成熟", icon: "✓" },
  { text: "当日采摘", icon: "✓" },
];
```

## 🚀 快速启动命令

```bash
# 创建项目
npx create-video@latest tomato-marketing --template blank

# 安装转场包
npm install @remotion/transitions

# 开发预览
npm start

# 渲染视频
npm run build
```

## ⚠️ 关键注意事项

1. ✅ **所有动画用 `useCurrentFrame()`** - 禁用 CSS transition
2. ✅ **限制数值范围** - 使用 `extrapolateRight: 'clamp'`
3. ✅ **Stagger 创造层次** - 元素依次出现
4. ✅ **Spring 配置选择** - 根据场景选择合适的 damping

## 📦 文件结构

```
tomato-marketing/
├── src/
│   ├── TomatoMarketing.tsx      # 主组件
│   ├── TomatoMarketingVideo.tsx  # 组合定义
│   └── scenes/
│       ├── Scene1Intro.tsx       # 产品介绍
│       ├── Scene2Data.tsx        # 数据展示
│       ├── Scene3Advantages.tsx  # 产品优势
│       └── Scene4CTA.tsx         # 行动号召
└── README.md
```

## 🎨 配色速查

| 用途 | 颜色 | Hex |
|------|------|-----|
| 主色 | 鲜红 | #E63946 |
| 辅助色 | 橙 | #F4A261 |
| 深色 | 深红 | #C1121F |
| 背景 | 米白 | #FFF8F0 |
| 文本 | 深蓝绿 | #264653 |

---

**基于 remotion-best-practices 技能**
