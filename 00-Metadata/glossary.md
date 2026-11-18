# 术语表 / Glossary

本库术语。区分"通用设计词"与"这套语言里的专有叫法"，避免 AI 把 AGSL 的 lens 理解成普通模糊。

| 术语 | 英文 / 源码标识 | 含义 | 别混淆成 |
|---|---|---|---|
| 液态玻璃 | Liquid Glass | 有厚度感、色散、折射暗示、弹性四个物理特征的玻璃材质 | 毛玻璃 / glassmorphism（只有模糊） |
| 毛玻璃 | classic glassmorphism | 高斯模糊 + 半透明白，没有折射 | 液态玻璃 |
| 折射 | lens / refraction | 背景像素被玻璃边缘真实弯折（凸透镜效果） | 模糊 |
| 色散 | chromaticAberration | 光穿过玻璃棱边被分解成青/品红/靛 | 彩虹渐变装饰 |
| 厚度感 | edge highlight | 边缘玻璃壁比中心亮：顶部高光 + 底部内反光 | 外阴影 |
| backdrop | `drawBackdrop` / CompositionLocal | 玻璃采样用的"背景纹理来源"，分三层 | 普通 background |
| 三层 backdrop | bg / content / dialog | 背景层 / 内容层 / 弹窗导出层，各自服务不同玻璃 | 单一背景 |
| veil | `veilAlpha` | 铺在玻璃上的实心膜厚度，控制背后文字透不透 | frost |
| frost | `frost` | 白雾浓度，唯一控制"背后能不能看穿"的旋钮 | veil |
| 形变 | squash & stretch | 横向拉伸纵向压扁的弹性形变 | 等比缩放 |
| 自绘玻璃 | `glassBar` / `glassPill` | 纯 Canvas 画的玻璃，零离屏层，用于会动的部件 | 真 backdrop 玻璃 |
| 离屏层 | graphicsLayer clip / RenderNode | 独立的离屏绘制缓冲，多个叠加会产生方块残影 | 普通图层 |
| 方块 | 视觉 bug | 玻璃移动/点击后残留的灰色矩形块 | 卡片 |
| AGSL | Android Graphics Shading Language | Android 的运行时着色器语言，RuntimeShader 用它 | GLSL / CSS filter |
| RuntimeShader | `android.graphics.RuntimeShader` | API 33+ 执行 AGSL 的类 | — |
| 真折射档 | tier: real refraction | API ≥33 且非软件渲染，lens + blur 全开 | — |
| 厚玻璃档 | tier: static glass | API ≤30，纯 Canvas 近实心兜底 | — |
| 档位 | Radii / Type / DialogSize | 集中定义的有限取值集合，不许现拍数字 | 设计变量 token |
| 顿号色 | Palette | 集中定义的语义色板 | 主题色 |
| 强调色 | AccentIndigo | 随主题变的靛蓝（浅 `#4F46E5` / 深 `#A5B4FC`） | 裸 Indigo |
| OnAccent | `OnAccent` | 强调色底上的文字色（浅白 / 深 `#1E1B4B`） | 固定白字 |
| 安全点击 | `SafeTap.run` | 统一兜住 onClick 异常的事件入口 | 普通 clickable |
| 光斑 | Aurora Spot | 背景三颗径向渐变圆，缓慢流动 | 渐变背景 |
| 羽化收口 | feathered edge | 顶栏下缘由真模糊渐变到透明，不是盖色块 | 渐变遮罩 |
| 兄弟节点 | sibling nodes | 浮层玻璃与内容层平级、画在之后，避免自采样 | 子节点 |
| 自采样 | self-sampling | 玻璃采样到包含自己的纹理，越糊越脏 | 正常采样 |
| 用户原话 | 源码注释标记 | 产品所有者的明确决定，优先级高于通用惯例 | 注释 |
| ⚠️ 推断 | inference marker | 未能从一手来源确认的结论，不可当规范 | 已核验结论 |
