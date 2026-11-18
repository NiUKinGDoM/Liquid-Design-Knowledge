# 安全点击 / Safe Tap

> 来源：`ui/SafeTap.kt`。
> 解决"点一下就闪退"的根治方案——不在每个 handler 里猜哪句有雷，在事件入口统一兜。

## 问题机制

原生版 100 多个可点区域，onClick 直接跑在**主线程事件分发**里。里面任何一句抛异常（Room 查询、文件读写、JSON 解析、Uri 权限、null 解引用）都会冒到 Looper 上，系统立刻杀进程。这是"点一下就闪退"的全部机制。

## `SafeTap.run`

```kotlin
try { block() }
catch (t: Throwable) {
    Log.e(...)                          // 记日志
    CrashGuard.recordSoft("$tag 异常", t) // 写 filesDir/soft_error.txt
    Toast.makeText(appContext, "操作出错了：${msg.take(60)}", LENGTH_LONG).show()
}
```

- 异常不杀进程。
- 屏幕底部弹提示，用户能截图告诉你哪一步炸了。
- 堆栈进诊断文件，导出可见。

⚠️ 只兜"点击处理函数内部"的异常，**兜不住重组阶段**的异常。composable 里仍然不许做重活。

## `safeClickable` 的两个关键改动

1. **`indication = null`**（用户："把默认涟漪全关了吧"）。默认涟漪取自 onSurface（浅灰/深白），边界是节点矩形，扩散几百ms 才淡出——在玻璃/圆角控件上就是"冒出一块方块"。
2. **不传 `interactionSource = null`**，改成 `composed` 里 `remember` 一个稳定实例。传 null 时 Compose 走"临时新建"分支，在点击密集页面（设置页、列表页）有概率在事件分发阶段抛异常。

## 复用

- 全站可点区域统一用 `safeClickable` 而非原生 `clickable`。
- 主操作统一走 `GlassButton`（里面已经接了 SafeTap + 震动）。
