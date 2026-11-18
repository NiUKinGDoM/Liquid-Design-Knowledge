# 设计令牌 / Design Tokens

机器可读的设计值。AI 和人都可以直接读 JSON 取数，不用从散文里抠数字。

| 文件 | 内容 |
|---|---|
| `colors.json` | 文字黑白体系、强调色、语义色板、主题底色、Aurora 光斑色 |
| `shapes.json` | 圆角 6 档、弹窗 3 档、Chrome 尺寸、描边 |
| `typography.json` | 字号 9 档、字重、字体族、缩放保护 |
| `glass.json` | 玻璃全部参数、效果链数值、四档降级 |

## 使用约定

1. **令牌是事实的镜像，不是源头。** 源头是源码（`Design.kt` / `Theme.kt` / `LiquidGlass.kt`）。源码改了要同步这里。
2. **颜色令牌按浅/深列出解析后的实际值**，方便非 Compose 环境（Web / Flutter / 设计工具）直接取用。Compose 里仍走 `@Composable` getter。
3. **玻璃参数标了 `note` 的不要盲改**——那些是多次返工校准的结果。
4. 每次来源项目发版，对照 `00-Metadata/version-tracking.md` 检查令牌是否需要更新。

## 校验

JSON 应能被标准解析器读取。项目中 `tools/designcheck.py` 做源码侧的硬编码扫描，与本目录令牌形成双向约束。
