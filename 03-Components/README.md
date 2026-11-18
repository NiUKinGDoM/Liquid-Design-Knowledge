# 组件 / Components

> 这套语言的组件不是独立设计，是"档位 + 玻璃实现 + 交互反馈"的组合落点。

## 文档索引

| 文档 | 内容 |
|---|---|
| `buttons.md` | 全站按钮：平面配方、语义三态、震动 |
| `dialogs.md` | 弹窗：尺寸档、平面化、收口按钮、二次 backdrop |
| `top-bar.md` | 顶栏：真模糊 + 羽化收口 + 帧率胶囊 |
| `bottom-bar.md` | 底栏：三层结构、Filled 图标、兄弟节点 |
| `toast.md` | 轻提示：Popup 实现、盖在弹窗之上 |
| `cards.md` | 卡片：glassPanel 淡框、描边、二级子卡 |

## 组件的共同约束

1. **尺寸从档位取**（`01-Foundations/shape-system.md`），不现拍数字。
2. **文字走黑白体系**（`01-Foundations/color-system.md`），层次靠透明度。
3. **选中态不能用实色** —— 用"染色玻璃"，且文字色要按 `OnAccent` 那套逻辑处理。
4. **点击走 `SafeTap.run`**，关默认涟漪。
5. **重要操作带震动**（`HapticFeedbackConstants.VIRTUAL_KEY`）。

## ⚠️ 组件和材质的边界

按钮是**平面**的，不是玻璃的——这是用户明确要求"所有按钮换平面"的结果。液态质感只在底栏胶囊、顶栏、弹窗三处浮层。别给按钮加真折射。
