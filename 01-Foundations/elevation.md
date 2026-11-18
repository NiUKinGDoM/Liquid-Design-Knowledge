# 层级 / Elevation

> 来源：`ui/Glass.kt`、`ui/liquid/LiquidGlass.kt`、`ui/AppChrome.kt`。
> 这套语言的"层级"不靠 Material 的 elevation dp，靠**玻璃材质 + 描边 + 三层 backdrop 的关系**。

## 层级不是阴影，是材质关系

传统 MD 用 elevation dp 表达层级。Liquid 语言里：

| 层级信号 | 手段 |
|---|---|
| 浮起来的玻璃 | 折射 + 边缘光 + 薄白膜 |
| 贴底的分组块 | 极淡 `glassPanel` 框（白 0.05 底 + 白 0.11 边） |
| 白卡贴白底 | 1dp `#E2E8F0` 描边（唯一能看出边界的东西） |
| 白玻璃压白背景 | 1dp 黑 0.05~0.09 轮廓暗线（"扣"出形状） |

## 阴影的特殊规则

- `Shadow.Default` 半径 24dp——**比底栏半高还大**，细长条上下阴影叠成方块灰影。
- 所以底栏 / 胶囊一律 `showShadow = false`。
- 大浮层（弹窗）可用阴影，但数值要克制；顶栏靠模糊不靠阴影。

> 白描边只是高光，压在白背景上整块隐身。每层自绘玻璃都要配暗线。见 `07-Development-Rules/liquid-rules.md` 铁律 8。

## 三层 backdrop 决定层级关系

```
bgBackdrop（纯背景）      ← 页面卡片、按钮采样
contentBackdrop（背景+内容）← 底栏、弹窗采样
dialogBackdrop（弹窗合成结果）← 弹窗内按钮采样
```

**浮层玻璃必须是内容层宿主的兄弟节点，画在之后。** 子节点会被录进 backdrop 形成自折射。

详见 `02-Glass-Materials/glass-implementations.md` 第四节。

## 高度栈（从下到上）

1. Aurora 光斑背景
2. 页面内容 + 卡片
3. 顶栏（浮层，真模糊 + 羽化）
4. 底栏（浮层，自绘玻璃条 + 胶囊）
5. 弹窗（浮层，真折射 + 二次导出 backdrop）
6. Toast（反馈层，在弹窗之上）

⚠️ Toast 用 Popup 而不是系统 Toast，保证弹窗打开时 Toast 盖在上面——详见 `03-Components/toast.md`。
