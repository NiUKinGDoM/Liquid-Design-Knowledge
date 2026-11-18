# 性能 / Performance

> 来源：`ui/Aurora.kt`、`ui/AppChrome.kt`、`ui/liquid/LiquidGlass.kt` 的性能注释。
> 玻璃项目的崩溃几乎都来自"层数 × 重绘频率"，不是逻辑 bug。

## 先算层数，再决定实现

| 场景 | 层数 | 实现 |
|---|---|---|
| 同屏 10 个玻璃按钮 | 10 shader + 10 离屏 | ❌ 崩 → 平面自绘 |
| 一张弹窗 | 1 真 backdrop | ✅ |
| 底栏 + 胶囊 | 自绘，0 离屏（胶囊） | ✅ |

**口诀：会动的自己画，静止的才折射，一堆小的走平面。** 详见 `02-Glass-Materials/glass-implementations.md`。

## 五个掉帧/崩溃点

| 问题 | 机制 | 解法 |
|---|---|---|
| `effects` lambda 每次重组新建 | `DrawBackdropElement.equals` 比 lambda 引用，不等 → 绘制缓存全废，滚动掉帧 | `effects` / `highlightProvider` / `shapeProvider` 用 `remember` 锁住 |
| 按压缩放改 Modifier 链长度 | 重建渲染层，旧离屏层残留成方块 | `graphicsLayer` 常驻链，只改 scale 参数 |
| 动画值在组合期读 | 每帧重组 + 重建渐变（60fps = 60 次重组 + 180 次分配） | 传 lambda，在绘制阶段读 |
| 背景每帧重绘 | N 层玻璃跟着每帧重新采样 | Aurora 只动 `graphicsLayer` 平移/缩放，位图栅格化一次 |
| 绘制阶段抛异常 | RuntimeShader 软件渲染不支持 / Path 极端尺寸 / OEM 驱动 | 早退 + try + 尺寸保护（见 `degradation.md`） |

## 兜底机制

- `lensBroken`：AGSL 设备现编失败后永久跳过 lens（`@Volatile`，不是 state）。
- `forceStatic`：连续崩溃后 CrashGuard 打开，全站静态玻璃。
- `SafeTap.run`：点击异常不杀进程。

## 检查项

- [ ] 同屏玻璃块数已算过，>3 块就考虑平面化
- [ ] 所有 provider lambda 都 remember 了
- [ ] 动画值不在组合期读
- [ ] Modifier 链结构恒定，状态变化只进 graphicsLayer
- [ ] 绘制前早退守卫齐全（API / 软件渲染 / 尺寸）
- [ ] 真机（含低端机）跑过，不是只看模拟器截图
