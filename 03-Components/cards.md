# 卡片 / Cards

> 来源：`ui/Glass.kt`（`glassPanel`）、`ui/theme/Design.kt`（Radii）。
> 卡片是页面内容层的一部分，采样 `bgBackdrop`（纯背景），**不采样内容层**。

## 主卡（glassPanel）

1:1 搬 Web `.glass-panel`——"若隐若现的框"：

| 项 | 浅色 | 深色 |
|---|---|---|
| 底 | 白 × 0.05 | `#0F172A` × 0.06 |
| 边 | 白 × 0.11，1dp | 白 × 0.04，1dp |
| 圆角 | 20dp (`Radii.CARD`) | 同 |
| 阴影 | `0 4px 24px rgba(0,0,0,0.02)`（极轻） | 同 |

> 之前原生版走 `liquidGlassSurface`（veil 0.34 + 折射），看起来是"一块半透明白板"，不是"若隐若现的框"。用户："那个透明框若隐若现的特别漂亮"——改成 Web 的淡白底 + 淡白边。

## 二级子卡（Radii.SUB = 16dp）

统计卡、山册卡、列表容器——比主卡小一档圆角。

## 白卡贴白底

`Palette.borderLight = #E2E8F0`（还有 subtle `#EDF0F7`、soft `#EFF1F6`）——白卡贴白底时唯一能看出边界的东西。1dp。

## veilAlpha 约束

内容容器（卡片）的 `veilAlpha` 必须 ≥0.5，否则正文直接压在玻璃上会透出背后的字，读不清。这是卡片和浮层（浮层要透出背景内容）的关键区别。

## 严禁

- 卡片采样 `contentBackdrop`（会采样到别的卡片 = 递归）。
- 圆角现拍数字——只用 Radii 档位。
- 用彩色字分层——文字只有黑白 + 透明度。
