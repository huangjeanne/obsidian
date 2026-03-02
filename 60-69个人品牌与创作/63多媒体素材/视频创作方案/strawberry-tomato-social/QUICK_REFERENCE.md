# 🍓 草莓西红柿短视频 - 快速参考

## 🎯 视频规格

- **时长**: 20 秒（600 帧 @ 30fps）
- **分辨率**: 1080x1920 (竖屏 9:16)
- **平台**: 抖音、小红书
- **风格**: 活泼鲜艳

## 📬 4 个场景

| 场景 | 时长 | 内容 | 动画 |
|------|------|------|------|
| 1. 开场 | 0-4s | 标题弹入、西红柿旋转 | Spring 弹性 |
| 2. 产品 | 4-9s | 4 个特点 stagger 展示 | 快速滑入 |
| 3. 数据 | 9-16s | 3 个数据卡片 | 数字滚动 |
| 4. CTA | 16-20s | 按钮、装饰旋转 | Spring 弹性 |

## 🎨 Spring 配置

```tsx
// 弹性（标题、按钮）
{ damping: 8 }

// 快速无弹跳（特点）
{ damping: 20, stiffness: 200 }

// 轻微弹性（数据）
{ damping: 18, stiffness: 200 }
```

## 📊 数据展示

- 🌿 有机认证 ✓
- ⭐ 口感评分 9.2/10
- 🍬 甜度 12°Bx

## 🚀 快速命令

```bash
# 开发预览
cd "/Users/jeanne/60-69个人品牌与创作/63多媒体素材/视频创作方案/strawberry-tomato-social"
npm start

# 渲染视频
npm run build

# 渲染 GIF
npm run build:gif
```

## 🎨 配色速查

- 主色: #E63946 (鲜红)
- 辅助: #FF6B6B (浅红)
- 绿色: #4CAF50 (自然)
- 背景: #FFF5F5 (淡粉)

## 📝 修改数据位置

`src/scenes/Scene3DataShowcase.tsx` 第 12-36 行

---

**项目位置**: `/Users/jeanne/60-69个人品牌与创作/63多媒体素材/视频创作方案/strawberry-tomato-social/`
