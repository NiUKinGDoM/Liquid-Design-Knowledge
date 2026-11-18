# 背景流光 / Aurora Background

> 来源：`ui/Aurora.kt`（1:1 搬 Web 版 `app-modal.css:1299` 的 `.aurora-bg`）。
> 用户原话："软件的背景你学一下 web 版的，照搬过来，有流动感的那种。"

## 三颗光斑

| # | 位置 | 直径 | 浅色 | 深色 | 周期 |
|---|---|---|---|---|---|
| ① 蓝 | 左上（-22vw / -10vh） | 95vw | `#60A5FA × 0.30` | `#6366F1 × 0.26` | 4.4s |
| ② 粉 | 右下 | 100vw | `#F472B6 × 0.26` | `#BE185D × 0.24` | 4.8s |
| ③ 青 | 中上（16vw / -6vh） | 80vw | `#2DD4BF × 0.22` | `#0F766E × 0.22` | 3.6s |

每颗是径向渐变圆（中心 → 68% 处透明）+ 三关键帧位移/缩放往返，`RepeatMode.Reverse` 原路返回。

**周期必须互质**（4.4/4.8/3.6）——一样的话三颗同步呼吸，不是流动。

## 为什么不用 Canvas 每帧重绘

每张玻璃卡都在采样背景。背景每帧真重绘，N 层玻璃就要每帧重新采样——帧率直接掉。

**正解**：位图只栅格化一次，只动 `graphicsLayer` 的 translation/scale，交给合成器搬。等价 Web 的 transform 动画。

## 关键实现

```kotlin
// 动画值在绘制阶段读（lambda），不在组合期读
dx: () -> Dp, dy: () -> Dp, scale: () -> Float

Box(Modifier
    .offset(x = baseX, y = baseY)
    .size(size)
    .graphicsLayer {
        translationX = dx().toPx()
        translationY = dy().toPx()
        scaleX = scale(); scaleY = scale()
        alpha = 1f   // 纯装饰，不参与命中测试
    }
    .background(brush))   // brush = remember(color) { radialGradient(...) }
```

**如果不传 lambda**（在组合期读动画值）：每个动画帧都重组这个 composable、重建 3 个 radialGradient——60fps 就是每秒 60 次无效重组 + 180 次分配。

## 校准历史

用户反馈："整个背景的光球再大一点，流动再快一点点。我好像只看到下方有，上方的光球呢？"

- 上方青色原来 baseY = -20%h、直径 54%w，圆心落在屏幕上方，只有极淡一条边露在顶部。
- 修正：直径放大（0.70→0.95 / 0.76→1.00 / 0.54→0.80），上方那颗下移到 -6%h 居中偏左。
- 周期加快（5.5/6.0/4.5 → 4.4/4.8/3.6s），仍互质。

> 光斑是纯装饰：不参与命中测试。`alpha = 1f` 固定。
