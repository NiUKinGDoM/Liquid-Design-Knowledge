# 来源映射 / Source Map

**每个知识点 → 源码文件**，双向可追。这是本库"可溯源"的核心。

来源项目：徒步小记 Android 原生版，仓库 `NiUKinGDoM/xixi-hiking-note`，路径前缀 `app/src/main/java/com/xixi/hiking/nativeapp/`。

## 按源码文件反查

| 源码文件 | 行数 | 产出本库哪些文档 |
|---|---|---|
| `ui/theme/Design.kt` | 129 | `01-Foundations/shape-system.md`、`05-Design-Tokens/{shapes,typography}.json`、色彩语义部分 |
| `ui/theme/Theme.kt` | 261 | `01-Foundations/color-system.md`、`typography.md`、`05-Design-Tokens/colors.json` |
| `ui/liquid/LiquidGlass.kt` | 1401 | `02-Glass-Materials/*`、`06-Compose-Implementation/*`、`05-Design-Tokens/glass.json`、`07-Development-Rules/*` |
| `ui/Glass.kt` | 122 | `glass-implementations.md`（glassPanel）、`03-Components/cards.md` |
| `ui/GlassOverlay.kt` | 739 | `03-Components/dialogs.md` |
| `ui/GlassWidgets.kt` | 402 | `03-Components/buttons.md`、`dialogs.md`、`04-Interaction/haptics.md` |
| `ui/GlassPickers.kt` | 514 | 选择器（本库未单独成篇） |
| `ui/AppChrome.kt` | 943 | `03-Components/{top-bar,bottom-bar}.md`、`04-Interaction/bottom-bar-motion.md`、`01-Foundations/motion.md` |
| `ui/Aurora.kt` | 204 | `02-Glass-Materials/aurora-background.md`、`01-Foundations/motion.md` |
| `ui/SafeTap.kt` | 146 | `04-Interaction/safe-tap.md`、`haptics.md` |
| `ui/Toast.kt` | 187 | `03-Components/toast.md` |
| `app.css` / `app-modal.css`（Web 版） | — | `.glass-panel` / `.glass-btn` / `.aurora-bg` 的 1:1 对照基准 |

## 按主题正查

| 知识点 | 源码位置 | 标注 |
|---|---|---|
| 圆角 6 档 | `Design.kt:29` `object Radii` | 2026-11-02 建立 |
| 字号 9 档 | `Design.kt:56` `object Type` | 无半号 |
| 语义色板 | `Design.kt:76` `object Palette` | 固定色不随主题 |
| 弹窗 3 档 | `Design.kt:123` `object DialogSize` | 对齐 Web |
| 液态玻璃铁律 | `LiquidGlass.kt:3-12` 文件头 | 用户 2026-11-02 亲定 |
| 四特征（厚度/色散/折射/弹性） | `Glass.kt:34-47` | v1→v2 的说明 |
| 玻璃四实现 | `LiquidGlass.kt` 各函数 | 成本分层 |
| 三层 backdrop | `LiquidGlass.kt` CompositionLocal 定义 | bg/content/dialog |
| 核心参数 | `LiquidGlass.kt` `liquidGlassSurface` 签名 | 默认值 + 注释 |
| 效果链数值 | `LiquidGlass.kt` lens/blur/highlight 实现 | 硬编码 |
| 四档降级 | `LiquidGlass.kt` `gpuEffectsUsable` / `lensBroken` / `forceStatic` | 能力守卫 |
| 文字黑白 | `Theme.kt:78-103` | 用户原话 |
| 强调色随主题 | `Theme.kt:38-65` | AccentIndigo / OnAccent |
| 宋体内嵌 | `Theme.kt:149-174` | 实测 Serif 中文仍黑体 |
| 字号缩放保护 | `Theme.kt:209-238` `AppTextScale` | 防内容显示不全 |
| Aurora 三光斑 | `Aurora.kt:24-144` | 1:1 搬 Web |
| 胶囊形变 | `AppChrome.kt:115-129` `pillScale` | 四次校准 |
| 顶栏羽化 | `AppChrome.kt:628+` | 用户嫌丑后重做 |
| 点击兜底 | `SafeTap.kt:11-30` | 防闪退 |
| Toast Popup | `Toast.kt:91-96` | 盖在弹窗之上 |

## 覆盖缺口

- [ ] `ui/GlassPickers.kt`（选择器，514 行）未单独成篇——其设计规则散落在其他组件里。
- [ ] 各业务页面（`ui/overview/`、`ui/records/`、`ui/plans/` 等）的具体布局未纳入——本库只覆盖通用设计语言，不覆盖业务页实现。
- [ ] 源码注释里未在本次提取中读到的 ★ 标记——定期 grep 补漏。
