# 字体 / Typography

> 来源：`ui/theme/Theme.kt`、`ui/theme/Design.kt`（Type 档位）。
> 字号数值见 `shape-system.md` 的「字号 9 档」；本文讲字体族、字重、缩放保护这些机制。

## 字体族：宋体衬线 + 0.02em 字距

Web 版 body 字体（`app.css:22`）：
```css
font-family: 'SimSun','宋体','STSong','Songti SC','Noto Serif SC', serif;
font-weight: 900;
letter-spacing: 0.02em;
```

### ⚠️ 关键坑：`FontFamily.Serif` 中文仍是黑体

源码注释实测：AOSP 的 `fonts.xml` 里 serif family 只挂了拉丁的 NotoSerif，中文 fallback 链全局共用 NotoSansCJK（黑体）。所以 `FontFamily.Serif` 只有英文字母数字变衬线，汉字纹丝不动。

**正解：内嵌字体文件。** 项目内嵌 `Noto Serif SC`（SIL OFL 1.1，可商用）：
- `AppSerif`：Normal/Medium 用 Regular，SemiBold/Bold 用 Bold。
- `AppLetterSpacing = 0.02.em`——宋体字面窄，加字距才有呼吸感。
- `LocalTextStyle` 兜底所有裸 `Text`（不在 Surface 里、弹窗内部、自定义容器）。

> ⚠️ 全站只有 4 处 Monospace：帧率胶囊 / 版本号 / 堆栈（+ 代码块）。其余一律继承 Serif，不要在别处写 fontFamily，否则又一处"和 Web 对不上"。

## 字重层级

| 层级 | 字重 | 说明 |
|---|---|---|
| 标题 | Bold (700) | headlineSmall 17sp、titleMedium 15sp |
| 半标题 | SemiBold (600) | titleSmall 14sp |
| 次要/标签 | Medium (500) | labelSmall 11.5sp |
| 正文 | Normal (400) | bodyLarge 14 / bodyMedium 13 / bodySmall 12 |

## 字号缩放保护（防"内容显示不全"）

**问题机制**：之前字号写死 sp，sp 跟随系统字体大小。用户把系统字体调大，卡片标题、统计数值、导航文字顶出固定高度容器——表现是"有些内容显示不全"，但截图里（fontScale=1）永远正常。

**解法**（`AppTextScale`）：
```kotlin
val scale = d.fontScale.coerceAtMost(1f).coerceAtLeast(0.85f)
```
- 系统 fontScale **封顶 1.0**：宁可字小，不能撑破布局。
- 下限 0.85：兼容极小字体偏好。
- 用 `LocalDensity` 统一缩放（一处生效、不漏），比改几百处字面量安全。

> 历史修正：曾试过整体缩到 0.9，正文 12sp 实际只有 10.8——偏小发虚。最终字重回归 1.0，设计稿标注的 17/15/14/13/12/11 就是屏幕上看到的。

## 常见错误

| 错法 | 后果 |
|---|---|
| 用 `FontFamily.Serif` 以为拿到宋体 | 中文还是黑体，和 Web 对不上 |
| 别处再写 fontFamily | 又一处字体不一致 |
| 不管 fontScale | 用户放大字体后内容被裁切 |
| 用 11.5/12.5 半号 | 字号表膨胀回 18 档 |
| 分享卡之外用 HERO 22sp | 破坏档位，导出图专用 |
