# Toast

> 来源：`ui/Toast.kt`。
> 用户原话："toast 你学一下 web 端的，现在背景透明的。"

## 实现要点

- **用 Popup 而不是系统 Toast**——保证弹窗打开时 Toast 盖在弹窗之上。
- 用户 2026-11-16 反馈："打开弹窗之后，toast 被弹窗盖住"——所以这里特意不改成普通 Toast。
- 关闭弹窗时不要顺手关 Toast。
- 背景不透明（对齐 Web `_createToast`），不是透明底。

## 用途

- 危险操作后的提示，不打断当前流程。
- `SafeTap.run` 兜住点击异常后，底部弹"操作出错了：xxx"，用户能截图告诉你哪一步炸了。

详见 `04-Interaction/feedback-and-motion.md` 第五节。
