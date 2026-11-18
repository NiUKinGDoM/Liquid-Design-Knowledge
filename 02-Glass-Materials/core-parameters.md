# 核心参数 / Core Parameters

> 来源：`ui/liquid/LiquidGlass.kt`（`liquidGlassSurface` 函数签名与实现）。
> 每个数值都有出处。改参数前先读 `07-Development-Rules/liquid-rules.md`。

## `liquidGlassSurface` 参数表

| 参数 | 默认 | 含义 / 取值经验 |
|---|---|---|
| `refract` | 10f | 折射高度 dp。8~12 是 iOS 26 量级；>16 边缘像鱼眼。底栏细长条 22~26 + `depth=true` |
| `frost` | 0.62 | 白雾浓度 0~1，**唯一控制"背后能不能看穿"的旋钮**。正文压上面 → 0.8+；底栏 → 0.55~0.65；装饰件 → 0.3 |
| `veilAlpha` | -1f | 实心膜厚度 0~1。内容容器（卡片）必须 ≥0.5，否则正文压玻璃上读不清；不传则按 `0.04 + frost×0.22` 算（0.04~0.30） |
| `depth` | false | 体积弯折：中间也跟着弯。细长底栏必须开，否则只有上下两条边在动 |
| `tint` / `tintAlpha` | `#BFD4FF` / 0.10 | 冷调浸润，薄薄一层 |
| `showShadow` | true | **底栏/胶囊一律 false**。24dp 阴影在细长条上叠成方块 |
| `edgeWidth` | 0.7f | 边缘高光宽度 dp。底栏胶囊 0.42f（发丝线），弹窗卡片 0.7f。传 0 关掉 |
| `topHighlight` | true | 长条玻璃可关，免得像塑料膜 |
| `exported` | null | 把"自己下面的合成结果"导出，供弹窗内子玻璃采样 |
| `pressed` | false | 按下提亮，缩放固定 0.985（**缩小**） |

## 效果链硬编码数值

```
lens:
  refractionHeight = depth ? max(refractPx, size.height * 0.5f) : refractPx
  refractionAmount = height * (depth ? 1.1f : 1.5f)
  chromaticAberration = true
blur:
  radius = (1f + frost * 13f).coerceAtMost(10f) dp   ← 基数从 3 降到 1，上限 10 防 GPU 崩
highlight:
  width = 0.7dp, intensity = 0.48f, angle = 45f, falloff = 0.95f
内壁反光:
  1.5dp 描边，顶部 alpha 0.30 → 0.09 → 0.025 → 底部 0.11（浅色）
  再叠 1dp 细亮线：顶 0.52 → 中 0.09 → 底 0.20
```

**顺序：先 lens 再 blur。** lens 把 padding 设成折射高度（多取一圈采样），blur 只在这个外扩范围内生效，边缘不会糊出锯齿。

## 玻璃条 / 胶囊自绘配方

### `glassBar`（底栏，26dp 圆角）——从上到下五层

1. 本体：近乎清水，白 0.13 → 0.06 → 0.10（浅色）
2. 顶部漫反射带：占上部 45%，白 0.34 → 0.14 → 透明
3. 中间镜面高光：横向窄带 17%~30% 处，白 0.17，暗示弧面
4. 底部地面反光：76%~100%，白 0.13
5. 外沿 1dp 细线（顶亮底弱）+ 轮廓暗线 `Black × 0.055`

### `glassPill`（选中胶囊）

- 按下放大 **1.39 倍**
- squash & stretch：横向随速度拉伸、纵向反向压扁
- **禁用 `Modifier.clip`** → 用绘制阶段 `clipPath(outline) { ... }`，零离屏
- `clipPath` 只裁玻璃本身，`drawContent()` 放外面（在裁剪块里调会闪退）
- 尺寸保护：宽/高 ≤0.5px 跳过 Path 构造
- 顶部反射是**填充**亮带（白 0.60 → 0.18 → 透明），不是描边

## 平面按钮配方（`glassStatic(flat=true)`）

| 状态 | 白膜不透明度 |
|---|---|
| 禁用 | 0.06 |
| 默认 | 0.10 |
| 主操作 strong | 0.16 |
| 按下 | 0.20 |

描边（两次加深后）：普通按钮 `#0F172A × 0.30`（浅）/ `White × 0.34`（深），1dp。

语义三态：

| 态 | 底 | 边 |
|---|---|---|
| 危险 danger | `#FEE2E2 × 0.45`（深 `#460F14 × 0.20`） | `#DC2626 × 0.9` |
| 实心危险 | `#EF4444 × 0.85` | 红 0.95 + 白字 |
| 正向 positive | `#16A34A × 0.16` | 绿 0.5 |

## `glassPanel`（分组框，1:1 搬 Web `.glass-panel`）

```css
background: rgba(255,255,255,0.05);
border: 1px solid rgba(255,255,255,0.11);
border-radius: 20px;
box-shadow: 0 4px 24px rgba(0,0,0,0.02);
```
深色：`rgba(15,23,42,0.06)` + border `rgba(255,255,255,0.04)`。靠边框和极轻阴影成形，"若隐若现的框"。

## 顶栏参数

- 真模糊 **22dp**（8dp 还能看出字影）
- 白膜峰值 **0.30**（深色更低）：从 0.62 降下来，不能再低到 0.16（快速滚动文字轮廓透出）
- 玻璃铺到屏幕最顶（含状态栏），不留接缝
- 总高 68dp = 实体 50 + 羽化 22
