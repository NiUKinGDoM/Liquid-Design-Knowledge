# 震动反馈 / Haptics

> 来源：`ui/SafeTap.kt`（`tap`）、`ui/GlassWidgets.kt`（`GlassButton`）。
> 用户："震动反馈有好多都没有，补全。"

## 实现

```kotlin
HapticFeedbackConstants.VIRTUAL_KEY
```

- 偏好 `KEY_HAPTIC` 可关（读内存缓存的 SharedPreferences，开销可忽略）。
- **任何异常吞掉**——震动失败绝不该影响点击本身。

## 统一入口

震动在 `GlassButton` 里取 View 并调用（因为 `Local.current` 只能在 composable 作用域读，不能在 onClick 里读）。全站主操作都走这个入口，保证不漏。

## 该震的地方

- 所有主操作按钮（保存、删除、确认）。
- 底栏 tab 切换（Web 版是全局 click 委托，tab-btn 也在列表里）。

## 不该震的地方

- 纯装饰点击。
- 频繁滑动（会有连续震动噪音）。
