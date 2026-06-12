# 模具：长图（-l / 默认）

## 步骤 1：读取模板

Read `{SKILL_DIR}/assets/long_template.html`

## 步骤 2：内容预处理

- 识别标题行（`#`/`##`/`###` 开头，或独立短行）
- 识别引用块（`>` 开头）
- 识别加粗（`**text**`）
- **识别金句**：独立成段的短句（通常 < 25 字），承载核心洞察，用 `.highlight` 渲染
- 按空行分割为段落列表
- **不做切分**：所有内容放在一张卡内

## 步骤 2.5：色调感知

根据内容气质选择一组背景底色 + 强调色，让每张卡片和内容产生共振。

**品牌色板（背景色 + 强调色双色对）：**

| 内容气质 | `{{BG_COLOR}}` | `{{ACCENT_COLOR}}` | 触发信号 |
|----------|---------------|-------------------|----------|
| 思辨/哲学 | `#F0EDE8` | `#C45C2E` | 认知、思维、本质、意义、哲学 |
| 技术/工程 | `#EBF0F5` | `#1E3A5F` | 架构、模型、算法、系统、代码 |
| 文学/叙事 | `#F2EDE5` | `#6B2D5C` | 故事、人物、写作、文字、诗 |
| 商业/产品 | `#F0F0E8` | `#C9A227` | 商业、金融、市场、投资、战略 |
| 幽默/轻松 | `#F0F0F0` | `#C45C2E` | 梗、段子、轻松、吐槽、自嘲 |
| 直播/预告/海报 | `#F0F0F0` | `#C9A227` | 直播、预告、今晚、开播、海报 |
| 通关/手册/教程 | `#F5F0E0` | `#C9A227` | 教程、手册、步骤、指南、通关 |
| 默认 | `#F0F0F0` | `#C45C2E` | 无法归类时 |

> **品牌背景色偏移上限**：背景色相对品牌冷白 `#F0F0F0` 的偏移控制在 +/- 8 hex 以内，保持 Carrara 大理石“冷白”基调。禁止使用暖黄/暖橙等明显偏暖的背景。

**暗色模式（`-d` 参数）：**

暗色模式下模板通过 `body.dark` 自动切换 CSS 变量，无需替换 `{{BG_COLOR}}` 和 `{{ACCENT_COLOR}}`。但仍需根据内容气质选择一组暗色强调色，覆盖模板默认的 `--accent-dark`：

| 内容气质 | `--accent-dark` 覆盖值 | 触发信号 |
|----------|----------------------|----------|
| 思辨/哲学 | `#D4713F` | 认知、思维、本质、意义、哲学 |
| 技术/工程 | `#2A4F7A` | 架构、模型、算法、系统、代码 |
| 文学/叙事 | `#8B3D78` | 故事、人物、写作、文字、诗 |
| 商业/产品 | `#D4A82E` | 商业、金融、市场、投资、战略 |
| 幽默/轻松 | `#D4713F` | 梗、段子、轻松、吐槽、自嘲 |
| 直播/预告/海报 | `#D4A82E` | 直播、预告、今晚、开播、海报 |
| 通关/手册/教程 | `#D4A82E` | 教程、手册、步骤、指南、通关 |
| 默认 | `#D4713F` | 无法归类时 |

**暗色模式执行：** 在生成的 HTML 的 `<body>` 标签上添加 `class="dark"`。

判断依据：扫描内容中的高频关键词和主题，匹配最贴近的一组。不需要精确——宁可用默认也不要错配。

## 步骤 3：格式化为 HTML

模板已内置品牌视觉组件（Miró 圆点、Kiefer 金箔、Chirico 阴影），你只需正确使用以下 class：

**基础元素：**
- 普通段落 → `<p>文本</p>`
- 章节标题（##/### 级别） → `<h2>标题</h2>`
- 引用 → `<blockquote><p>引用</p></blockquote>`（自动带 accent 色圆点标记）
- 加粗 → `<strong>文本</strong>`
- 列表 → `<ul><li>...</li></ul>`

**金句（独立成段的核心洞察短句，视觉突出）：**
```html
<p class="highlight">金句文本</p>
```
判断标准：独立成段、< 25 字、承载关键洞察。

**戏剧性金句（全文最重要的那一句，放大处理）：**
```html
<p class="highlight pull-quote">金句文本</p>
```
全文最多用 1 次。用 accent 色渲染 + 金箔横杠装饰。适合放在文章转折处或开头。

**重点提示（Miró 深蓝底高亮，用于关键问句/提示）：**
```html
<p class="prompt">重点提示文本</p>
```
判断标准：需要让读者停下来、用力看的“提示性”内容。典型场景：
- Q&A 中的 **Question**（Answer 用普通段落）
- 关键追问（“那为什么 X 不是 Y？”）
- 阅读引导提示（“读到这里，请先停下想一想”）
- 召唤行动的指令句

视觉上以 **Miró 深蓝淡底 + 深蓝左边线** 呈现，与 `.highlight` 形成区分：
- `.highlight` = 作者下的金句结论（accent 色左线）
- `.prompt` = 抛给读者的问题/提示（Miró 蓝底）

一张卡内 `.prompt` 不超过 5 处，避免高亮泛滥。

**文内关键词高亮（荧光笔）：**
```html
<mark>关键词</mark>
```
用于文内单个关键词/短语的强调，模拟荧光笔标注。Kiefer 金箔色调。每张卡不超过 3 处。

**首字下沉（第一个正文段落）：**
```html
<p class="dropcap">段落正文...</p>
```
仅首个正文段落使用。首字母自动变为金箔色衬线体，营造经典编辑排版的开篇仪式感。

**条目组（有标题+正文的并列条目）：**
```html
<div class="item">
  <p class="label">条目标题</p>
  <p>条目正文</p>
</div>
```
label 自动带 accent 色方块标记。

**副标题标签：**
```html
<p class="subtitle">标签文字</p>
```
自动渲染为 Panda Blue 色 + 左侧竖线，JetBrains Mono 大写宽字距。

**分割线（章节之间）：**
```html
<div class="divider"></div>
```
自动渲染为细线 + 中央金箔圆点。

**Chirico 长影分隔块（章节大转折，制造空间停顿）：**
```html
<div class="shadow-divider">
  <p>章节标题</p>
  <p class="shadow-sub">副标题或补充说明</p>
</div>
```
用于文章大章节之间的视觉停顿，带长影渐变背景。替代空行或简单分割线。

**米罗星点散落（内容节奏标记）：**
```html
<div class="miro-dots"></div>
```
在需要呼吸感的位置插入，Miró 式实心红圆 + 空心黄圆组合。自动居中。

**缺口圆环标 ◐（列表标记或装饰前缀）：**
```html
<span class="ring-mark"></span>列表项文字
```
用于列表项前缀或作为装饰标记，旋转 -15° 的缺口圆环。

**眉标 Eyebrow（章节小标题前缀）：**
```html
<p class="eyebrow">章节代号 / 阶段标记</p>
<h2>章节标题</h2>
```
Miró 深蓝横线 + 大写宽字距标签，为章节提供视觉层级。放在 h2 之前。

**任务卡片 Quest Card（行动指令/待办）：**
```html
<div class="quest-card">
  <p class="quest-label">MISSION / TASK / QUEST</p>
  <p>任务描述内容</p>
</div>
```
金箔色左边线 + 虚线边框，用于行动指令、待办事项、练习任务。

**角色对话 Dialog（兔狲 vs 猫熊）：**
```html
<div class="dialog">
  <div class="dialog-bubble pallas">
    <p class="dialog-who">🐱 兔狲</p>
    <p>对话内容</p>
  </div>
  <div class="dialog-bubble panda">
    <p class="dialog-who">🐼 猫熊</p>
    <p>对话内容</p>
  </div>
</div>
```
兔狲（锈橙色条，左对齐）vs 猫熊（深蓝色条，右对齐）。用于教学对话、Q&A、角色互动场景。

## 步骤 3.5：品牌视觉自检（强制）

生成 HTML 后、写入文件前，逐项确认：

**必须使用的品牌视觉组件（至少 3 种）：**
- [ ] `.paw-divider` — 章节分隔（替代 `.divider`，不要用空行）
- [ ] `.chirico-quote` — 引用/名言（替代普通 `<blockquote>`）
- [ ] `.elite-seal` — 精英/限量/通关标记
- [ ] `.stage-number` — 步骤/阶段编号
- [ ] `.dropcap` — 第一个正文段落首字下沉
- [ ] `.dual-signature` — 兔狲/猫熊双角色观点签名

**基础组件（继续使用）：**
- [ ] 是否有至少 1 个 `.highlight` 金句？（纯文字墙是禁忌）
- [ ] 最重要的金句是否用了 `.highlight.pull-quote`？（全文最多 1 次）
- [ ] 是否有 `.subtitle` 标签为章节提供视觉层级？
- [ ] 整体排版是否有节奏变化？（段落→金句→列表→引用→分割线→段落，不是连续 N 个段落）

**HTML 示例：**
```html
<div class="paw-divider"><span class="paw-icon">🐾</span></div>
<blockquote class="chirico-quote">
  <p>引用文本</p>
  <div class="quote-source">—— 来源</div>
</blockquote>
<span class="elite-seal">ELITE</span>
<span class="stage-number">01</span>
<p class="dropcap">首段正文……</p>
<div class="dual-signature">
  <div class="sig-block pallas"><div class="sig-name">兔狲</div><p class="sig-text">……</p></div>
  <div class="sig-block panda"><div class="sig-name">猫熊</div><p class="sig-text">……</p></div>
</div>
```

**禁止**：用空行代替 `.paw-divider`；用普通 `<blockquote>` 代替 `.chirico-quote`；用纯数字代替 `.stage-number`；长图没有≥3种品牌组件。

**头像自动显示**：模板已内嵌兔狲/猫熊头像 base64，使用 `.pallas` / `.panda` class 时头像自动渲染，禁止外部引用。英文名统一使用 PandaCat AI Camp。

## 步骤 4：平台感知

根据用户指定的分发环境（`-p` 参数或内容中的触发信号），选择对应的平台参数：

| 参数 | 分发环境 | 画布宽度 | 正文字号 | 行高 | 边距 | 触发信号 |
|------|---------|---------|---------|------|------|---------|
| `-p wx-mobile` | 微信手机端 | 750px | 32px | 1.8 | 48px | 用户提及「微信」「手机」「朋友圈」 |
| `-p wx-desktop` | 微信电脑端 | 1080px | 36px | 1.7 | 72px | 用户提及「微信」「电脑」「PC」 |
| `-p xhs` | 小红书 | 900px | 30px | 1.9 | 56px | 用户提及「小红书」「xhs」「笔记」 |
| `-p mp` | 微信公众号 | 900px | 30px | 1.85 | 60px | 用户提及「公众号」「推文」「文章配图」 |
| `-p poster` | 海报/打印 | 1080px | 40px | 1.6 | 80px | 用户提及「海报」「打印」「展架」 |
| 默认 | 通用 | 1080px | 36px | 1.7 | 72px | 无明确信号 |

**选择逻辑**：
- 若用户显式指定 `-p` 参数，直接使用对应参数
- 若未指定，扫描内容中的触发信号词，匹配最贴近的平台
- 多个信号冲突时，优先选择移动端（wx-mobile > xhs > mp > wx-desktop > poster）
- 无信号时回退到默认（1080px / 36px / 1.7 / 72px）

**手机可读性铁律**：
- 最小正文字号 ≥ 30px（在 750px 画布上缩放后约 11.5px，高于微信 11px 下限）
- 行高 ≥ 1.6（手机端需更大呼吸空间）
- 边距随画布宽度递减（750px 用 48px，避免内容被挤压）

## 步骤 5：选择 Logo 版本

根据画布宽度 + 暗色模式参数，选择正确的 Logo 文件：

| 条件 | 使用文件 |
|------|---------|
| 画布宽度 < 900px | `logo-icon.png`（空间受限，纯图形） |
| 画布宽度 ≥ 1080px + 非暗色模式 | `logo-light.png`（长图品牌身份完整露出） |
| 画布宽度 ≥ 1080px + 暗色模式（`-d`） | `logo-dark.png` |

## 步骤 6：渲染模板

替换模板变量：

| 变量 | 规则 |
|------|------|
| `{{BG_COLOR}}` | 步骤 2.5 确定的背景底色 |
| `{{ACCENT_COLOR}}` | 步骤 2.5 确定的强调色 |
| `{{CANVAS_WIDTH}}` | 步骤 4 确定的画布宽度（如 `750px`、`900px`、`1080px`） |
| `{{CANVAS_HEIGHT}}` | 长图不固定高度，留空由浏览器自动撑开 |
| `{{BASE_TEXT_SIZE}}` | 步骤 4 确定的正文字号（如 `32px`、`36px`、`40px`） |
| `{{BASE_LINE_HEIGHT}}` | 步骤 4 确定的行高（如 `1.8`、`1.7`、`1.6`） |
| `{{PAD_X}}` | 步骤 4 确定的水平边距（如 `48px`、`72px`） |
| `{{PAD_Y}}` | 步骤 4 确定的垂直边距（等于 PAD_X 或略小） |
| `{{LOGO_FILE}}` | 步骤 5 确定的 Logo 文件名 |
| `{{TITLE_BLOCK}}` | 有标题时：`<div class="title-area"><h1>标题</h1></div>`；无标题时：空字符串 |
| `{{BODY_HTML}}` | 步骤 3 生成的全部 HTML |
| `{{SOURCE_LINE}}` | 内容来源（可选）：`<span class="info-source">来源文字</span>`，无来源时空字符串 |
| `{{BRAND_SIGNATURE}}` | 品牌签名：`<div class="brand-signature"><span class="paw">🐾</span><span class="slogan">无场景，不AI。</span></div>`（或根据语境从三句 slogan 中选择） |

写入：`/tmp/catclub_cast_long_{name}.html`

## 步骤 7：截图

```bash
node {SKILL_DIR}/assets/capture.js /tmp/catclub_cast_long_{name}.html {OUTPUT_DIR}/{name}.png {CANVAS_WIDTH} 800 fullpage
```

`{CANVAS_WIDTH}` 使用步骤 4 确定的画布宽度（750/900/1080）。

**多平台并行**：如果用户要求生成多个平台的版本，使用同一套 HTML 内容，仅替换平台变量，并行截图。
