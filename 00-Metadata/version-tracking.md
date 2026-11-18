# 版本追踪 / Version Tracking

本库记录**来源项目版本**、**依赖库版本**和**核验记录**。原则：只写能从一手来源确认的状态，不猜版本号，不把 artifact 存在等同于 GA。

## 来源项目版本快照

| 核验日期 | 对象 | 版本 | 来源 | 状态 |
|---|---|---|---|---|
| 2026-11-18 | 徒步小记原生版 | vc143 / v1.1.1.10（README 自述） | `README.md` + `version.json` | 生产版本 |
| 2026-11-18 | Liquid 设计语言基准建立 | 2026-11-02 | `ui/theme/Design.kt` 文件头注释 | 已建立 |
| 2026-11-18 | 液态玻璃 v1 → v2 | 2026-11-01 ~ 11-02 | `ui/Glass.kt` 注释 | v2（四特征齐全） |
| 2026-11-18 | 底栏液体形变定稿 | 2026-11-06 ~ 11-07 | `ui/AppChrome.kt` 注释 | 多次校准后定稿 |

## 技术依赖状态

| 依赖 | 用途 | API 要求 | 状态 | 备注 |
|---|---|---|---|---|
| `android.graphics.RuntimeShader` | AGSL 真折射 | API 33+ | 平台 API | 软件渲染下不可用，必须早退 |
| Kyant0/backdrop | `drawBackdrop` / lens / blur | 见上游 | 上游库 | API 行为以该库为准，升级时重查，**不在此固化版本号** |
| Jetpack Compose | UI 框架 | — | 平台 | 版本随项目 build.gradle，本库不猜 |
| HapticFeedbackConstants.VIRTUAL_KEY | 点击震动 | — | 平台 API | 偏好可关，异常吞掉 |

> ⚠️ 本库**不写** Kyant0/backdrop 或 Compose 的具体版本号——它们随项目 build 文件变化，写死会过期。需要时直接读来源项目的 `app/build.gradle.kts` 与 `gradle/libs.versions.toml`。

## 核验方法

本库的数值来自**源码实读**，不是网络抓取。核验路径：

1. 打开 `08-Source-Map/README.md`，找到知识点对应的源码文件。
2. 读该文件的定义与 ★ 注释。
3. 数值冲突时回到 Web 版 CSS（`app.css` / `app-modal.css`）对照。
4. 标注日期、来源文件、变化摘要到 `change-log.md`。

## 待确认清单

- [ ] Kyant0/backdrop 当前 Maven 稳定版本与 lens API 的实验性注解状态——读上游 release notes，本库暂不固化。
- [ ] `frost` / `veilAlpha` 在极端 fontSize 下的表现——源码注释未覆盖，属真机调试范畴。
- [ ] 不同 OEM GPU 驱动对 AGSL 的支持率——`lensBroken` 机制已兜底，但无统计数据。
- [ ] Web 版最新 CSS 数值是否与原生版注释里记录的一致——需定期对照，避免原生版注释滞后于 Web。

## 更新规则

- 来源项目发版时，检查 `Design.kt` / `Theme.kt` / `LiquidGlass.kt` / `AppChrome.kt` 是否变化。
- 上游库升级时，重查 `drawBackdrop` / lens / 高光相关 API。
- 设计规范变化查 Web CSS，不靠转述。
- **不从博客日期推断 API 状态**。能读到源码就读源码，读不到就标"待确认"。
