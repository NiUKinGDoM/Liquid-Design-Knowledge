# 动效 / Motion

> 来源：`ui/Aurora.kt`、`ui/AppChrome.kt`（胶囊形变）、`ui/liquid/LiquidGlass.kt`（按压）。
> 这套语言的动效分两类：背景的"流动"和交互的"弹性"。

## 一、背景流动（Aurora）

三颗光斑，周期互质（4.4 / 4.8 / 3.6s）：

- 周期**必须互不相同**——一样的话三颗同步放大缩小，看起来是整块在"呼吸"，不是"流动"。
- 用 `keyframes` 精确落在 0%/50%/100% 三点，`RepeatMode.Reverse` 原路返回（等价 CSS `alternate`）。
- `seg()` 三点折线插值，端点用 smoothstep 收口（贴近 CSS ease-in-out）。

### 性能红线

**只动 `graphicsLayer` 的平移/缩放**，位图只栅格化一次，交给合成器搬——零重绘、零重新采样。

**动画值在绘制阶段读**（`Spot` 传 lambda 而不是结果）：在组合期读会让每个动画帧都重组、重建 3 个 radialGradient（60fps = 每秒 60 次无效重组 + 180 次分配）。

详见 `02-Glass-Materials/aurora-background.md`。

## 二、交互弹性（squash & stretch）

| 动效 | 参数 | 来源 |
|---|---|---|
| 胶囊按住放大 | 1.39 倍 | 官方 DampedDragAnimation 配方 |
| 速度形变上限 | ±3.5%（多次校准后） | 用户来回给三次反馈收敛 |
| 整条底栏跟手位移 | 封顶 4dp，EaseOut | 让"整条被拖"比"只有胶囊动"真实 |
| 回弹 | 临界阻尼 dampingRatio = 1 | 直接躺平，不过冲 |
| 按压反馈底座 | 不要 | 用户："把按压底删掉，不需要" |

详见 `04-Interaction/bottom-bar-motion.md`。

## 三、按压反馈

| 控件 | 反馈 |
|---|---|
| 平面按钮 | 缩放 0.97，白膜 0.10 → 0.20，径向柔光 |
| 玻璃表面 | 缩放 0.985（变小），整体提亮白 0.10 |
| 液态胶囊 | 放大 1.39 + 上浮 1.5dp + squash&stretch + 径向高光 |

**⚠️ 方向相反**：平面按钮按下放大/提亮，玻璃表面按下缩小（`pressed` 固定 0.985）。别搞混。

## 四、震动反馈

`HapticFeedbackConstants.VIRTUAL_KEY`，偏好（`KEY_HAPTIC`）可关，任何异常吞掉——震动失败绝不该影响点击本身。

## 动效禁用清单

- 默认涟漪（`indication = null`）：在玻璃/圆角上扩散成方块。
- 按压缩放改 Modifier 链长度：重建渲染层留残影。
- 点击先缩到 0.86 再弹回（底栏胶囊）：已撤掉，用户要的就是形变本身。
