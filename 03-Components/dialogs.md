# 弹窗 / Dialogs

> 来源：`ui/GlassOverlay.kt`、`ui/GlassWidgets.kt`（`DialogTitle` / `DialogMessage` / 收口按钮）。

## 尺寸档（`DialogSize`）

| 档 | 宽度占屏 | 用途 |
|---|---|---|
| SM | 0.80 | 确认类（一两句话） |
| MD | 0.88 | 一般弹窗（列表、详情） |
| LG | 0.92 | 表单与长正文 |
| MAX_HEIGHT | 0.80 | 统一限高，底部留可点边距 |

之前六种宽度四种限高，同一个 App 弹窗一张一个样——现在只有三档。

## 平面化

**用户原话："现在这个虚假立体感不好看。"**

弹窗玻璃去掉：顶部漫反射带、内壁棱、轮廓暗线、按下提亮。只留一层填充 + 一条极细边。大面积玻璃上堆光影会显得"鼓起来一块"。

## 排版（全站唯一写法）

| 元素 | 规格 |
|---|---|
| 标题 | 16sp（`DialogTitle`） |
| 正文 | 14sp / 行高 21sp，浅 `#1F2937` / 深 `#D1D5DB` |
| 按钮文字 | 15sp（比容器字号齐平或大一号，不要小一号） |
| 收口 | 统一底部"关闭"按钮，**X 去掉** |

## 二次 backdrop（防自采样）

弹窗内部按钮采样 `dialogBackdrop`——弹窗自己导出的合成结果，而不是 `contentBackdrop`。否则按钮会把弹窗背后 18 行列表折进来。

- 弹窗必须是内容层宿主的**兄弟节点**，画在之后。
- 如果弹窗挂在页面 composable 内部，`LocalInContentLayer` 会为 true——此时必须退回实色膜，因为采样的是包含自己的纹理，几帧后糊成一团。

详见 `02-Glass-Materials/glass-implementations.md` 第四节、`07-Development-Rules/visual-bug-triage.md` 铁律 6。

## 嵌套弹窗

源码有专门的嵌套弹窗扫描（`tools/overlayscan.py`）。嵌套弹窗要避免第三层 clip 离屏层——那是"方块"bug 第六个真凶。

## Toast 与弹窗

Toast 用 Popup 实现，保证弹窗打开时 Toast 盖在上面（`04-Interaction/feedback-and-motion.md` 第五节）。
