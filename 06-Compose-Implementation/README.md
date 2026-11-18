# Compose 实现 / Compose Implementation

从设计语言到 Compose 代码的落地指南。

## 文档索引

| 文档 | 内容 |
|---|---|
| `api-mapping.md` | 概念 → Compose/库 API 的映射表 |
| `performance.md` | 性能红线：层数、重绘、重组 |
| 四档降级 | 见 `02-Glass-Materials/degradation.md`（降级属材质话题，未在本目录重复） |

## 技术栈

- Kotlin + Jetpack Compose，不是 WebView 套壳。
- 液态玻璃视觉基于 **Kyant0/backdrop**（AGSL RuntimeShader，API 33+ 真折射）。
- 最低 Android 7.0（API 24），target 36（来源项目 README）。
- 具体依赖版本读来源项目 `app/build.gradle.kts`，本库不固化。

## 三条实现红线（再说一次）

1. `effects` / `shapeProvider` / `highlightProvider` 用 `remember` 锁住——lambda 引用变化会让绘制缓存全废。
2. 动画值传 lambda，在绘制阶段读——组合期读会每帧无效重组。
3. 状态变化放进常驻 `graphicsLayer`，只改参数不改 Modifier 链结构——链长度变化留残影。
