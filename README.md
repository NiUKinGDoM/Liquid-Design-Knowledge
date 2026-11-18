# Liquid Design Knowledge Base

面向 AI 的 **Liquid Glass 设计语言知识库**（徒步小记原生版实战沉淀版）。

这是一份个人整理的知识库，记录「徒步小记」Android 原生版（Kotlin + Jetpack Compose）在 **143 版迭代**中沉淀出的整套 Liquid 玻璃设计语言。不是 Google 官方规范，也不是 Material Design 的替代品——是一套在真机上被反复验证、每条结论都能追到具体源码文件的实战规范。

## 这个库解决什么问题

让 AI（或人）在写这套视觉风格的界面时**不靠记忆编造**：

- 圆角用几档？字号有没有半号？——有固定档位表，不现拍数字。
- 玻璃该用真折射还是自绘？——按场景分四种实现，有明确选择口诀。
- 换个颜色为什么要改两个地方？——强调色随主题变，文字只有黑白。
- 出现方块残影怎么办？——六个真凶按出现顺序排查，没有一次是靠删玻璃解决的。

**每条数值型结论都标注了来源文件和核验方式。** 本库不把"看起来像"写成"规范如此"。

## 快速开始

1. 把 `AI-INSTRUCTIONS.md` 设为 AI 项目的指令文件。
2. 从 `00-Metadata/sources.yaml` 了解一手来源与权威层级。
3. 按 `README.md` 的「阅读路径」查对应分册，不要跨层猜。
4. 需要改视觉时，先读 `07-Development-Rules/` 的三份规则，再动手。
5. 更新时记录来源文件、核验日期和变化摘要（见 `00-Metadata/change-log.md`）。

## 内容状态

| 目录 | 状态 | 说明 |
|---|---|---|
| `00-Metadata/` | ✅ 可用 | 来源索引、术语表、版本追踪、变更记录、来源核验记录 |
| `01-Foundations/` | ✅ 可用 | 6 篇基础系统：原则 / 色彩 / 字体 / 形状 / 层级 / 动效 |
| `02-Glass-Materials/` | ✅ 可用 | 6 篇材质：三层 backdrop / 四种玻璃实现 / 核心参数 / 降级 / 背景流光 |
| `03-Components/` | ✅ 可用 | 6 篇组件：按钮 / 弹窗 / 顶栏 / 底栏 / Toast / 卡片 |
| `04-Interaction/` | ✅ 可用 | 交互反馈：按压 / 底栏形变 / 震动 / 点击兜底 |
| `05-Design-Tokens/` | ✅ 可用 | 机器可读令牌：颜色、圆角、字号、玻璃参数（JSON） |
| `06-Compose-Implementation/` | ✅ 可用 | 实现指南：API 映射、性能红线、能力降级 |
| `07-Development-Rules/` | ✅ 可用 | 铁律、评审清单、视觉 bug 排查手册 |
| `08-Source-Map/` | ✅ 可用 | 知识点 → 源码文件映射表，双向可追 |

## 阅读路径

| 你要做的事 | 先看 |
|---|---|
| 做一个新界面 | `01-Foundations/design-principles.md` → `01-Foundations/shape-system.md` |
| 定颜色 | `01-Foundations/color-system.md` |
| 定字号 / 字体 | `01-Foundations/typography.md` |
| 做一块玻璃 | `02-Glass-Materials/README.md` → `glass-implementations.md` |
| 调折射 / 模糊 / 高光 | `02-Glass-Materials/core-parameters.md` |
| 做底栏胶囊动效 | `04-Interaction/bottom-bar-motion.md` |
| 处理深色 / 旧设备兼容 | `02-Glass-Materials/degradation.md` |
| 界面掉帧 / 闪退 | `06-Compose-Implementation/performance.md` |
| 出现方块残影 | `07-Development-Rules/visual-bug-triage.md` |
| 查某个数值从哪来 | `08-Source-Map/README.md` |

## 资料原则

- **一手来源**：徒步小记原生版源码（`app/src/main/java/com/xixi/hiking/nativeapp/`）。所有规范以实际实现为准。
- **对照基准**：Web 版 CSS（`app.css` / `app-modal.css`）。原生版多处标注"1:1 搬 Web"，这是设计意图的原始出处。
- **技术依赖**：Kyant0/backdrop（AGSL 液态玻璃实现）。API 行为以该库文档和源码为准，不猜版本。
- **用户决策**：源码注释里标「用户原话」的，是产品所有者的明确决定，优先级高于通用审美惯例。
- **次级来源**：第三方液态玻璃教程、博客只用于定位思路，不作为数值依据。
- **本库不固化"看起来差不多"的值**：未能从源码或官方文档确认的数值，一律标注「⚠️ 推断」或「待确认」。

## 重要前提

这套语言的核心技术依托 **AGSL RuntimeShader**（Android API 33+ 的真实背景像素折射），这是 Android 专有能力。所以：

- 这不是纯 CSS glassmorphism——那是毛玻璃，不是液态玻璃。液态玻璃要有厚度感、色散、折射暗示、弹性四个特征。
- 低端机 / 旧 API 走四档降级，静态档用近实心厚玻璃兜底，绝不硬上 shader 崩溃。
- iOS 无对应物，所以本库只覆盖 Android 原生 + Web 对照。

本库所有数值型结论都来自源码实读。源码注释里的 ★ 标记是迭代过程中留下的"为什么是这个值"的解释——**这些解释比数值本身更重要**，本库尽量保留。

## 许可

知识库正文为个人整理，可自由参考。引用其中结论时请标注来源文件。
