# 01 色彩与字体

> 来源：徒步小记原生版 `ui/theme/Theme.kt`、`ui/theme/Design.kt`。
> 核心决策：文字只有黑白，层次靠透明度；彩色只给装饰，不给正文。

---

## 一、文字颜色：只有黑白

用户原话："别加那么多颜色，把现有的颜色全删掉……只需存在白色字和黑色字。"

| 角色 | 浅色主题 | 深色主题 | 用途 |
|---|---|---|---|
| `Ink` 主文字 | 纯黑 `Color.Black` | 纯白 `Color.White` | 正文、标题、数值 |
| `InkSecondary` 次要 | 黑 × 0.62 透明 | 白 × 0.70 透明 | 副标题、说明、次要图标 |
| `InkMuted` 更弱 | 黑 × 0.40 透明 | 白 × 0.48 透明 | 图例、脚注、徽章 |

**规则：**
- 三个都是随主题变的 getter，只能用在 composable 作用域（含 `Modifier.composed` 工厂块）。
- 层次**只靠透明度 + 字重**，不靠色相。
- 这条只约束"文字/图标颜色"，背景、玻璃膜、装饰色块不受限。

---

## 二、强调色：靛蓝，随主题切

| 名称 | 浅色 | 深色 | 说明 |
|---|---|---|---|
| `Indigo` 主色 | `#4F46E5` | — | 原始主色，用于图标、色带、进度、选中态 |
| `AccentIndigo` | `#4F46E5` | `#A5B4FC` | **新代码一律用这个**。裸 Indigo 在深底 `#0F172A` 上对比度只有 ~2.4:1 |
| `OnAccent` | 白 | `#1E1B4B` | 强调色底上的文字色。深色下白字对比度 ≈1.6:1，必须换深靛 |

⚠️ 凡是 `.background(AccentIndigo)` 的地方，文字必须是 `OnAccent`。

---

## 三、危险色：全站一个红

`DangerRed = Color(0xFFDC2626)`

- 普通 val，不是 composable getter——深浅色用同一个红，两边对比度都够。
- 原来 8 个文件硬编码 27 次（删除键、错误提示、过期徽章、签名校验失败……），改一次要全工程搜一遍还容易漏。现在只有这一个源。

语义色板 `Palette`（`Design.kt`）：

| 语义 | 色值 | 变体 |
|---|---|---|
| 危险 / 删除 | `#DC2626` | soft `#EF4444`、深色 `#F87171` |
| 成功 / 完成 | `#16A34A` | deep `#15803D`、深色 `#6EE7A8`、light `#10B981` |
| 警告 / 过期 | `#F59E0B` | deep `#D97706` |
| 信息 / 明天 | `#0EA5E9` | — |
| 中性灰 | `#64748B` | soft `#94A3B8`、border `#CBD5E1` |
| 卡片描边（浅） | `#E2E8F0` | subtle `#EDF0F7`、soft `#EFF1F6` |
| 强调紫 | `#7C3AED` | deep `#6D28D9` |
| 图表辅助 | 蓝 `#3B82F6`、青 `#14B8A6`、紫 `#A855F7`、橙 `#F97316` | — |

> 紫色有明确边界：**分享卡、难度色可用；正向操作禁用**，正向一律绿。

---

## 四、主题底色

| 角色 | 浅色 | 深色 |
|---|---|---|
| `background` | `#F7F9FC` | `#0F172A` |
| `surface` | `#FFFFFF` | `#162032` |
| `surfaceVariant` | `#F1F5F9` | `#1E293B` |
| `onSurfaceVariant` | 黑 × 0.78 | 白 × 0.84 |
| `outline` | `#CBD5E1` | `#334155` |

**分工铁律：正文永远黑/白，装饰可以有颜色。** 这句是全站配色争议的裁判标准。

---

## 五、字体：宋体衬线 + 0.02em 字距

Web 版 body 是：
```css
font-family: 'SimSun','宋体','STSong','Songti SC','Noto Serif SC', serif;
font-weight: 900;
letter-spacing: 0.02em;
```

Android 上 `FontFamily.Serif` 中文**仍然是黑体**（AOSP 的 serif family 只挂拉丁 NotoSerif，中文 fallback 到 NotoSansCJK）。要真宋体必须内嵌字体文件：`Noto Serif SC`（SIL OFL 1.1，可商用），Regular + Bold 两个字重。

- `AppSerif`：Normal/Medium 用 Regular，SemiBold/Bold 用 Bold。
- `AppLetterSpacing = 0.02em`，宋体字面窄，加字距才有呼吸感。
- `LocalTextStyle` 兜底所有裸 `Text`（不在 Surface 里、弹窗内部、自定义容器）。
- 全站只有 4 处 Monospace：帧率胶囊、版本号、堆栈（+ 代码块）——其余一律继承 Serif，不要在别处再写 fontFamily。

### 字号缩放保护

系统 `fontScale` **封顶 1.0**（下限 0.85），防止用户放大系统字体后把固定高度容器撑破——表现就是"有些内容显示不全"，而截图里永远正常。

设计稿标注的 17 / 15 / 14 / 13 / 12 / 11 就是屏幕上看到的，不做额外缩放。

---

## 六、字重层级

| 层级 | 字重 |
|---|---|
| 标题 | Bold (700) |
| 正文 | Normal (400) |
| 次要 / 标签 | Medium (500) |
| 半标题 | SemiBold (600) |

Typography 映射：headlineSmall 17sp Bold / titleMedium 15sp Bold / titleSmall 14sp SemiBold / bodyLarge 14sp / bodyMedium 13sp / bodySmall 12sp / labelSmall 11.5sp Medium。

---

## 常见错误

| 错法 | 后果 | 正解 |
|---|---|---|
| 深色模式继续用裸 `Indigo` | 对比度 2.4:1，图标文字发暗 | 用 `AccentIndigo` |
| 强调色底上写白字（深色模式） | 对比度 1.6:1，几乎看不见 | 用 `OnAccent` |
| 为了层次给次要文字换成灰色系 | 变成第二套配色体系，主题一切就乱 | 用 `InkSecondary` 透明度 |
| 直接 `FontFamily.Serif` 以为拿到宋体 | 中文还是黑体，和 Web 对不上 | 内嵌 Noto Serif SC |
| 新写一个"稍微不同的红" | 又一处漏改点 | 用 `DangerRed` / `Palette` |
