# 评审清单 / Review Checklist

提交视觉相关改动前逐条勾选。

## 设计语言一致性

- [ ] 圆角只取 6 档（20/16/12/10/6/4），没有野圆角
- [ ] 字号只取 9 档，没有半号（11.5/12.5 之类）
- [ ] 弹窗宽度只取 3 档（0.80/0.88/0.92），限高 0.80
- [ ] 文字只有黑白 + 透明度，没有彩色正文
- [ ] 语义色走 `Palette` / `DangerRed`，没有裸 `Color(0x...)`
- [ ] 强调色用 `AccentIndigo`，其底上文字用 `OnAccent`
- [ ] 字体继承 `AppSerif` + 0.02em，没有别处写 fontFamily（4 处 Monospace 例外除外）

## 玻璃实现

- [ ] 按场景选对实现（会动自绘 / 静止真折射 / 一堆小件平面）
- [ ] 浮层是内容层的**兄弟节点**，不是子节点
- [ ] 采样层正确（卡片采样 bgBackdrop，浮层采样 contentBackdrop，弹窗内按钮采样 dialogBackdrop）
- [ ] 会动的玻璃零离屏层（`clipPath` 而非 `Modifier.clip`）
- [ ] 底栏/胶囊 `showShadow = false`，`edgeWidth = 0.42`
- [ ] 白玻璃有轮廓暗线（1dp 黑 0.05~0.09 浅 / 白 0.16~0.22 深）

## 性能

- [ ] 同屏玻璃块数已算过（>3 块考虑平面化）
- [ ] `effects` / `shapeProvider` / `highlightProvider` 已 remember
- [ ] 动画值传 lambda，绘制阶段读，不在组合期读
- [ ] Modifier 链结构恒定，状态变化只进常驻 `graphicsLayer`
- [ ] 绘制前早退守卫齐全（API≥31 / 非软件渲染 / 尺寸>0.5px）

## 交互

- [ ] 点击走 `safeClickable` / `GlassButton`，`indication = null`
- [ ] 主操作带震动，偏好可关，异常吞掉
- [ ] 底栏"再点一次当前 tab"回最上层
- [ ] Toast 用 Popup，弹窗打开时盖在上面

## 兼容与降级

- [ ] API < 33 有降级路径，静态档 veil ≥0.96
- [ ] 深色模式验过（文字不发暗、强调色切换、白膜峰值）
- [ ] 系统字体放大时内容不被裁切（fontScale 封顶生效）
- [ ] 真机跑过（含至少一台低端机）

## 文档

- [ ] 新增数值写进 `05-Design-Tokens/` 对应 JSON
- [ ] 新增知识点写进 `08-Source-Map/README.md`
- [ ] 违反档位/铁律的例外写了注释理由
