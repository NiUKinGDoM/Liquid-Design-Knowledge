# 按钮 / Buttons

> 来源：`ui/GlassWidgets.kt`（`GlassButton` / `liquidGlassChip`）。
> 用户两次强调："所有按钮都要液态玻璃质感，不要实色" → 后来又"所有按钮换为平面"。最终形态：**平面玻璃**，一张薄面 + 一条细边。

## 最终配方（`glassStatic(flat=true)`）

| 状态 | 白膜不透明度 |
|---|---|
| 禁用 | 0.06 |
| 默认 | 0.10 |
| 主操作 strong | 0.16 |
| 按下 | 0.20 |

- 描边：`#0F172A × 0.30`（浅）/ `White × 0.34`（深），1dp。不加深时整排按钮像几团白雾。
- Web 对照：`.glass-btn` = `rgba(255,255,255,0.10)`，hover 0.16。
- 圆角：`Radii.BUTTON = 12dp`；胶囊按钮端头用 CircleShape。
- 文字：BODY 14sp / 次要 LABEL 13sp；主操作字不能只靠强调色（紫玻璃 + 紫字几乎同色）。

## 语义三态（对齐 Web）

| 态 | 底 | 边 | 文字 |
|---|---|---|---|
| 普通 | 白 0.10 | 墨 0.30 | Ink / OnAccent |
| 危险 danger | `#FEE2E2 × 0.45`（深 `#460F14 × 0.20`） | `#DC2626 × 0.9` | DangerRed |
| 实心危险 | `#EF4444 × 0.85` | 红 0.95 | 白 |
| 正向 positive | `#16A34A × 0.16` | 绿 0.5 | 绿 |

> 紫色只给分享卡和难度色，正向操作禁用紫——正向一律绿。

## 交互

- 点击走 `GlassButton`（全站主操作唯一入口：保存/删除/确认都在这里）。
- 震动在这里统一补（`SafeTap.tap(view)`），偏好可关，异常吞掉。
- `SafeTap.run` 兜住 onClick 异常，不杀进程。
- 按压反馈：缩放 0.97 + 白膜 0.10→0.20 + 径向柔光（中心偏中上）。

## 严禁

- 给按钮加真 backdrop / lens shader——同屏 10 个会崩（`06-Compose-Implementation/performance.md`）。
- 选中态用实色 `AccentIndigo`——用染色玻璃。
- 默认涟漪（方块）——`indication = null`。
- 写裸 `Color(0xFF...)` 表示语义色——用 `Palette` / `DangerRed`。
