# 能力降级 / Degradation

> 来源：`ui/liquid/LiquidGlass.kt`（`gpuEffectsUsable` / `lensBroken` / `forceStatic` / `tierLabel`）。
> 核心原则：**应用侧不写 if (API >= 33)，由库内部四档自动降级；但绘制前的守卫必须早退。**

## 四档

| 档位 | 条件 | 表现 |
|---|---|---|
| 真实折射 + 磨砂 | API ≥ 33 且硬件渲染 | lens + blur + 高光全开 |
| 磨砂模糊 | API 31~32 | lens 库内部 return，只有 blur + 高光 |
| 厚玻璃 | API ≤ 30 / 软件渲染 | 纯 Canvas：近实心 ≥0.96 + 描边 + 高光 |
| 已关闭 | 用户关开关 / `forceStatic` | 只有染色 + 高光，零风险 |

## 关键守卫（早退，不 catch）

| 守卫 | 防什么 | 做法 |
|---|---|---|
| `gpuEffectsUsable` | RuntimeShader 在软件渲染不支持 | API ≥31 且非软件渲染才用，**不满足直接早退** |
| `lensBroken` | AGSL 设备现编，某些 GPU 驱动失败 | 第一次 catch 后**永久跳过 lens**，`@Volatile` 而非 state（绘制阶段写 state 会死循环） |
| blur try | 个别 OEM 在 draw 阶段抛 | 包 try，失败只少点毛感 |
| `forceStatic` | 连续崩溃 | CrashGuard 打开，全站静态玻璃兜底 |
| 尺寸保护 | Path 在极端尺寸抛 | `if (w > 0.5f && h > 0.5f)` 才构造 |

> ⚠️ **不能"试一下再 catch"**——RuntimeShader 报错发生在 Canvas 绘制里，抛出时半块玻璃已经画到屏幕上了，catch 都来不及体面收场。

## 静态档为什么是 0.96 近实心

没有模糊时，半透白会让背后的字变成"淡淡的但清清楚楚的影子"，比全透更难看。宁可**厚玻璃**，不要糊字。

## 调试

`tierLabel()` 看当前走到哪一档。出现视觉问题时先确认档位，再查 `07-Development-Rules/visual-bug-triage.md`。

## iOS 说明

AGSL RuntimeShader 是 Android 专有 API，iOS 无对应物。所以本项目没有 iOS 原生版，用 Web PWA + 添加到主屏幕。详见来源项目 README。
