# 玻璃材质 / Glass Materials

这套语言最核心的分册。玻璃不是一种实现，是**四种按成本分层的实现 + 一套统一的材质参数**。

## 文档索引

| 文档 | 内容 |
|---|---|
| `glass-implementations.md` | 四特征、四条铁律、四种实现的选择、三层 backdrop、自绘配方、平面按钮、调试顺序（**主文档，最全**） |
| `core-parameters.md` | `liquidGlassSurface` 全部参数表与硬编码数值 |
| `degradation.md` | 四档能力降级与能力守卫 |
| `aurora-background.md` | 页面级背景流光三光斑 |

## 先记住这四句

1. **液态玻璃有四个物理特征**：厚度感、色散、折射暗示、弹性。缺一个就不是液态玻璃，只是毛玻璃。
2. **模糊 ≠ 玻璃**。存在感来自折射 + 边缘光，不是白雾。
3. **玻璃是地基，不许削弱换稳定**（用户拿四轮返工换来的铁律）。
4. **四种实现按场景选**：会动的自己画 / 静止的才折射 / 一堆小的走平面 / 分组块用淡框。

## 30 秒决策

| 你要做什么 | 用哪个 |
|---|---|
| 弹窗、大浮层 | `liquidGlassSurface`（真 backdrop + lens + blur） |
| 底栏玻璃条 | `glassBar`（自绘，零离屏） |
| 底栏选中胶囊 | `glassPill`（自绘 + clipPath + 形变） |
| 一堆按钮 | `liquidGlassChip` → `glassStatic(flat=true)`（平面） |
| 页面分组框 | `glassPanel`（白 0.05 底 + 白 0.11 边） |

## 三层 backdrop（必读）

任何玻璃都要先想清楚"采样谁"：

- `bgBackdrop`（纯背景）→ 卡片、按钮
- `contentBackdrop`（背景+内容）→ 底栏、弹窗
- `dialogBackdrop`（弹窗合成结果）→ 弹窗内按钮

采样错层 = 自折射 = 越糊越脏。**浮层是内容层的兄弟节点，不是子节点。**

## 出问题先看这里

方块残影 / 自折射 / 模糊盖屏 / 闪退 —— 先读 `07-Development-Rules/visual-bug-triage.md`，按十问清单排查。**没有一次是靠删玻璃修好的。**
