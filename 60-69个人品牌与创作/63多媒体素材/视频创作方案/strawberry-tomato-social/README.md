# 🍓 草莓西红柿社交媒体短视频

基于 Remotion 最佳实践创建的竖屏营销视频，专为抖音/小红书等社交媒体平台设计。

## 📋 视频信息

- **时长**: 20 秒（600 帧 @ 30fps）
- **分辨率**: 1080x1920（竖屏 9:16）
- **风格**: 活泼鲜艳
- **目标平台**: 抖音、小红书、快手

## 🎬 视频结构

### 场景 1: 开场吸引 (0-4秒)
- 标题弹入："草莓西红柿"
- 副标题滑入："自然成熟 ✨ 口感极佳"
- 西红柿图标旋转弹入
- **动画**: Spring 弹性动画、滑入、旋转

### 场景 2: 产品展示 (4-9秒)
- 4 个产品特点依次展示
  - 🍅 果形饱满
  - ✨ 色泽鲜艳
  - 💧 汁水丰富
  - 🌿 自然成熟
- **动画**: Stagger 延迟（15 帧）、快速滑入

### 场景 3: 数据展示 (9-16秒)
- 3 个数据卡片展示
  - 🌿 有机认证 ✓
  - ⭐ 口感评分 9.2/10
  - 🍬 甜度 12°Bx
- **动画**: Spring 旋转、数字滚动、进度条

### 场景 4: 行动号召 (16-20秒)
- 主标题："立即购买"
- CTA 按钮："🛒 抢购"
- 装饰元素旋转
- 二维码提示
- **动画**: Spring 弹性、淡入、旋转

## 🎨 动画配置

### Spring 参数
```tsx
// 弹性效果（标题、按钮）
{ damping: 8 }

// 快速无弹跳（产品特点）
{ damping: 20, stiffness: 200 }

// 快速有轻微弹性（数据卡片）
{ damping: 18, stiffness: 200 }
```

### Stagger 延迟
```tsx
// 场景 2: 产品特点
const STAGGER_DELAY = 15; // 15 帧

// 场景 3: 数据卡片
const STAGGER_DELAY = 12; // 12 帧
```

### 转场效果
```tsx
// 场景 1→2: slide from-bottom (15 帧)
// 场景 2→3: fade (20 帧)
// 场景 3→4: fade (15 帧)
```

## 🚀 快速开始

### 安装依赖
```bash
npm install
```

### 开发预览
```bash
npm start
```

访问 http://localhost:3000 查看实时预览

### 渲染视频
```bash
# 渲染为 MP4
npm run build

# 渲染为 GIF
npm run build:gif
```

## 📂 项目结构

```
strawberry-tomato-social/
├── src/
│   ├── Root.tsx                    # 组合注册
│   ├── StrawberryTomatoSocial.tsx   # 主组件
│   ├── index.ts                    # 入口文件
│   ├── index.css                   # 样式
│   └── scenes/
│       ├── Scene1Intro.tsx         # 场景 1: 开场
│       ├── Scene2ProductShow.tsx   # 场景 2: 产品展示
│       ├── Scene3DataShowcase.tsx  # 场景 3: 数据展示
│       └── Scene4CTA.tsx           # 场景 4: 行动号召
├── package.json
└── README.md
```

## 🎨 配色方案

| 用途 | 颜色 | Hex |
|------|------|-----|
| 主色 | 鲜红 | #E63946 |
| 辅助色 | 浅红 | #FF6B6B |
| 强调色 | 橙红 | #FF6B8A |
| 绿色 | 自然绿 | #4CAF50 |
| 背景 | 渐变 | #FFE5E5 → #FFF5F5 |

## 💡 Remotion 最佳实践

✅ **所有动画使用 `useCurrentFrame()` 驱动**
✅ **使用 Spring 动画实现自然运动**
✅ **使用 TransitionSeries 管理场景**
✅ **限制数值范围使用 `extrapolateRight: 'clamp'`**
✅ **Stagger 延迟创造层次感**
✅ **组件拆分便于维护**

## 📊 自定义数据

修改场景 3 的数据展示：

```tsx
// src/scenes/Scene3DataShowcase.tsx
const dataItems = [
  {
    label: "有机认证",
    value: "✓",
    color: "#4CAF50",
  },
  {
    label: "口感评分",
    value: 9.2,  // 修改这个值
    max: 10,
    unit: "/10",
  },
  {
    label: "甜度",
    value: 12,   // 修改这个值
    max: 15,
    unit: "°Bx",
  },
];
```

## 🎯 使用场景

- 抖音短视频
- 小红书种草视频
- 快手视频
- 微信朋友圈视频
- 产品宣传视频

## 📝 修改建议

### 修改时长
```tsx
// src/Root.tsx
durationInFrames={600} // 修改这个值（20秒 @ 30fps）
```

### 修改分辨率
```tsx
// src/Root.tsx
width={1080}  // 横屏改为 1920
height={1920} // 横屏改为 1080
```

### 修改配色
在各个场景组件中修改 `backgroundColor` 和 `color` 属性。

### 添加/删除场景
1. 在 `src/scenes/` 创建新场景组件
2. 在 `StrawberryTomatoSocial.tsx` 中添加到 `TransitionSeries`

## 🔧 故障排除

### 视频无法预览
```bash
# 确保依赖已安装
npm install

# 重启开发服务器
npm start
```

### 渲染失败
```bash
# 检查 Node 版本（需要 >= 18）
node --version

# 清理缓存重新安装
rm -rf node_modules package-lock.json
npm install
```

### 动画不平滑
检查是否使用了 `useCurrentFrame()` 驱动动画，禁用了 CSS transition。

## 📚 参考资料

- [Remotion 官方文档](https://www.remotion.dev/docs)
- [remotion-best-practices Skill](~/.claude/skills/remotion-best-practices/)
- [TransitionSeries 文档](https://www.remotion.dev/docs/transition-series)

## 🎥 下一步

- 添加背景音乐（使用 `@remotion/media-utils`）
- 添加音效（按钮点击、数据展示）
- 使用真实的产品图片
- 添加品牌 Logo
- A/B 测试不同的动画时长

---

**创建时间**: 2026-01-27
**基于**: remotion-best-practices Skill
**视频时长**: 20 秒
**分辨率**: 1080x1920 (9:16 竖屏)
