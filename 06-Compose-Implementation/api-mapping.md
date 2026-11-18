# API 映射 / API Mapping

设计概念 → Compose/库 API。查"这个概念在代码里叫什么"。

| 概念 | API / 源码标识 | 所在文件 |
|---|---|---|
| 真液态玻璃 | `Modifier.liquidGlassSurface(...)` | `ui/liquid/LiquidGlass.kt` |
| 背景采样层 | `Modifier.drawBackdrop` + `CompositionLocal` | `ui/liquid/LiquidGlass.kt` |
| 背景 backdrop | `bgBackdrop` / `LocalLiquidBackdrop` | 各页面宿主 |
| 内容 backdrop | `contentBackdrop` / `LocalContentBackdrop` | 页面根 |
| 弹窗 backdrop | `dialogBackdrop` / 导出 `exported` | 弹窗 |
| 内容层标记 | `LocalInContentLayer` | `ui/liquid/LiquidGlass.kt` |
| 淡框卡片 | `Modifier.glassPanel(corner, dark)` | `ui/Glass.kt` |
| 底栏玻璃条 | `glassBar(...)`（自绘） | `ui/liquid/LiquidGlass.kt` / `AppChrome.kt` |
| 底栏胶囊 | `glassPill(...)`（自绘 + clipPath） | 同上 |
| 平面按钮 | `liquidGlassChip` → `glassStatic(flat=true)` | `ui/liquid/LiquidGlass.kt` |
| 主操作按钮 | `GlassButton` | `ui/GlassWidgets.kt` |
| 顶栏 | `TopBar` | `ui/AppChrome.kt` |
| 底栏 | `BottomBar` | `ui/AppChrome.kt` |
| 帧率胶囊 | `FpsBadge` | `ui/AppChrome.kt` |
| 背景流光 | `AuroraLayer` / `Spot` | `ui/Aurora.kt` |
| 档位表 | `Radii` / `Type` / `Palette` / `DialogSize` | `ui/theme/Design.kt` |
| 黑白文字 | `Ink` / `InkSecondary` / `InkMuted` | `ui/theme/Theme.kt` |
| 强调色 | `AccentIndigo` / `OnAccent` / `DangerRed` | 同上 |
| 字体族 | `AppSerif` / `AppLetterSpacing` | 同上 |
| 字号缩放保护 | `AppTextScale` | 同上 |
| 主题 | `HikingTheme` / `LocalIsDark` | 同上 |
| 安全点击 | `safeClickable` / `SafeTap.run` / `SafeTap.tap` | `ui/SafeTap.kt` |
| Toast | `showToast`（Popup 实现） | `ui/Toast.kt` |

## 常用组合

### 一张大浮层玻璃（弹窗）

```kotlin
Modifier.liquidGlassSurface(
    corner = Radii.CARD.dp,
    frost = 0.62f,          // 浮层透出背景
    refract = 10f,
    exported = dialogBackdrop  // 给内部按钮导出
)
```

### 一张内容卡片

```kotlin
Modifier.liquidGlassSurface(
    frost = 0.5f,
    veilAlpha = 0.6f,       // >=0.5，正文压上面读得清
    backdrop = bgBackdrop   // 只采样背景，不采样内容
)
```

### 一个平面按钮

```kotlin
GlassButton(strong = true, danger = false) { ... }
// 内部走 liquidGlassChip → glassStatic(flat=true)，白膜 0.16，描边 1dp
```

> ⚠️ 以上是**结构示意**，不是可直接编译的完整代码。实际参数名以当前源码为准——本库不固化 API 签名，升级时重查 `ui/liquid/LiquidGlass.kt`。
