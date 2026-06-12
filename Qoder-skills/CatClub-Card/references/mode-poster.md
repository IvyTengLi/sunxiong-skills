# 模具：多卡（-m）

## 步骤 1：读取模板

Read `{SKILL_DIR}/assets/poster_template.html`

## 步骤 1.5：色调感知

与长图模具共享同一套色调系统。根据内容气质选择 `{{BG_COLOR}}` 和 `{{ACCENT_COLOR}}`。

**品牌色板（背景色 + 强调色双色对）：**

| 内容气质 | `{{BG_COLOR}}` | `{{ACCENT_COLOR}}` | 触发信号 |
|----------|---------------|-------------------|----------|
| 思辨/哲学 | `#F0EDE8` | `#C45C2E` | 认知、思维、本质、意义、哲学 |
| 技术/工程 | `#EBF0F5` | `#1E3A5F` | 架构、模型、算法、系统、代码 |
| 文学/叙事 | `#F2EDE5` | `#6B2D5C` | 故事、人物、写作、文字、诗 |
| 商业/产品 | `#F0F0E8` | `#C9A227` | 商业、金融、市场、投资、战略 |
| 默认 | `#F0F0F0` | `#C45C2E` | 无法归类时 |

> **品牌背景色偏移上限**：背景色相对品牌冷白 `#F0F0F0` 的偏移控制在 +/- 8 hex 以内，保持 Carrara 大理石“冷白”基调。

**暗色模式（`-d` 参数）：**

暗色模式下模板通过 `body.dark` 自动切换 CSS 变量，无需替换 `{{BG_COLOR}}` 和 `{{ACCENT_COLOR}}`。但仍需根据内容气质选择一组暗色强调色，覆盖模板默认的 `--accent-dark`：

| 内容气质 | `--accent-dark` 覆盖值 | 触发信号 |
|----------|----------------------|----------|
| 思辨/哲学 | `#D4713F` | 认知、思维、本质、意义、哲学 |
| 技术/工程 | `#2A4F7A` | 架构、模型、算法、系统、代码 |
| 文学/叙事 | `#8B3D78` | 故事、人物、写作、文字、诗 |
| 商业/产品 | `#D4A82E` | 商业、金融、市场、投资、战略 |
| 直播/预告/海报 | `#D4A82E` | 直播、预告、今晚、开播、海报 |
| 默认 | `#D4713F` | 无法归类时 |

**暗色模式执行：** 在生成的 HTML 的 `<body>` 标签上添加 `class="dark"`。

## 步骤 2：内容预处理

- 识别标题行（`#`/`##`/`###` 开头，或独立短行）
- 识别引用块（`>` 开头）
- 识别加粗（`**text**`）
- **识别金句**：独立成段的短句（通常 < 25 字），承载核心洞察，用 `.highlight` 渲染
- 按空行分割为段落列表

## 步骤 3：计算视觉重量

模板在 1080x1440 全分辨率渲染，正文 36px，行高 1.7。

- 普通段落：字符数 × 1.4
- 标题行（h1 首卡 84px）：字符数 × 6.0
- 金句（`.highlight` 40px + 左边框 + 上下留白）：字符数 × 3.0
- `.item` 条目组（label + 正文）：字符数 × 1.8
- 引用块：字符数 × 1.7
- 分割线（divider）：固定 60 权重
- 代码块：字符数 × 2.2
- Running title（续页头部）：固定 70 权重

## 步骤 4：贪心切分

- 阈值：每卡约 **380** 字符等价视觉重量
- 逐段累加，超过阈值时在当前段之前切分
- **切分规则**：
  - 绝不在句子中间切
  - 优先在段落/条目/章节边界切
  - 标题不落单（必须跟至少一个内容元素在同一卡）
  - 超长单段在句号处强制切
  - 一个章节（h2 + 3 items）通常刚好一卡

**特殊情况**：
- 只有一张卡：不显示页码
- 多张卡：显示 `1 / N` 格式页码

## 步骤 5：格式化为 HTML

模板已内置品牌视觉组件（Miró 圆点、Kiefer 金箔标题杠、金箔分割点），你只需正确使用以下 class：

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

**戏剧性金句（每卡最多 1 个，放大处理）：**
```html
<p class="highlight pull-quote">金句文本</p>
```
accent 色渲染 + 金箔横杠装饰。适合放在卡片开头或转折处。

**重点提示（Miró 深蓝底高亮，用于关键问句/提示）：**
```html
<p class="prompt">重点提示文本</p>
```
与 `.highlight` 的区分：
- `.highlight` = 作者下的金句结论（accent 色左线）
- `.prompt` = 抛给读者的问题/提示（Miró 蓝底）

典型场景：Q&A 的 Question、关键追问、阅读引导、召唤行动。一张卡内不超过 5 处。

**文内关键词高亮（荧光笔）：**
```html
<mark>关键词</mark>
```
Kiefer 金箔色调。每张卡不超过 3 处。

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
自动渲染为 Panda Blue 色 + 左侧竖线。

**分割线（章节之间）：**
```html
<div class="divider"></div>
```
自动渲染为细线 + 中央金箔圆点。

**排版节奏自检**：每张卡片必须混合使用段落、金句、列表、引用、分割线，禁止连续 3+ 个普通段落。

### 步骤 5.5：品牌视觉组件（强制）

多卡模板已预置以下品牌组件，每张卡片必须使用：

**必须使用的组件（至少 3 个）：**
- `.paw-divider` — 卡片内章节分隔（替代 `.divider`）
- `.chirico-quote` — 引用/名言
- `.elite-seal` — 精英/限量/通关标记
- `.stage-number` — 步骤/阶段编号
- `.dropcap` — 首卡首段首字下沉
- `.dual-signature` — 兔狲/猫熊双角色观点签名（适合末卡）

**HTML 示例：**
```html
<div class="paw-divider"><span class="paw-icon">🐾</span></div>
<blockquote class="chirico-quote"><p>引用文本</p></blockquote>
<span class="elite-seal">ELITE</span>
<span class="stage-number">01</span>
<p class="dropcap">首段正文……</p>
<div class="dual-signature">
  <div class="sig-block pallas"><div class="sig-name">兔狲</div><p class="sig-text">……</p></div>
  <div class="sig-block panda"><div class="sig-name">猫熊</div><p class="sig-text">……</p></div>
</div>
```

**禁止**：用空行代替 `.paw-divider`；用普通 `<blockquote>` 代替 `.chirico-quote`；用纯数字代替 `.stage-number`；单张卡片没有≥3种品牌组件。

**头像自动显示**：模板已内嵌兔狲/猫熊头像 base64，使用 `.pallas` / `.panda` class 时头像自动渲染，禁止外部引用。英文名统一使用 PandaCat AI Camp。

## 步骤 6：平台感知

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

## 步骤 7：选择 Logo 版本

根据画布宽度 + 暗色模式参数，选择正确的 Logo 文件：

| 条件 | 使用文件 |
|------|---------|
| 画布宽度 < 900px | `logo-icon.png`（空间受限，纯图形） |
| 画布宽度 ≥ 1080px + 非暗色模式 | `logo-light.png`（多卡海报品牌身份完整露出） |
| 画布宽度 ≥ 1080px + 暗色模式（`-d`） | `logo-dark.png` |

## 步骤 8：渲染模板

对每张卡片，替换模板变量：

| 变量 | 规则 |
|------|------|
| `{{BG_COLOR}}` | 步骤 1.5 确定的背景底色 |
| `{{ACCENT_COLOR}}` | 步骤 1.5 确定的强调色 |
| `{{CANVAS_WIDTH}}` | 步骤 6 确定的画布宽度（如 `750px`、`900px`、`1080px`） |
| `{{CANVAS_HEIGHT}}` | 固定 `1440px`（多卡标准高度） |
| `{{BASE_TEXT_SIZE}}` | 步骤 6 确定的正文字号（如 `32px`、`36px`、`40px`） |
| `{{BASE_LINE_HEIGHT}}` | 步骤 6 确定的行高（如 `1.8`、`1.7`、`1.6`） |
| `{{PAD_X}}` | 步骤 6 确定的水平边距（如 `48px`、`72px`） |
| `{{PAD_Y}}` | 步骤 6 确定的垂直边距（等于 PAD_X 或略小） |
| `{{LOGO_FILE}}` | 步骤 7 确定的 Logo 文件名 |
| `{{HEADER_BLOCK}}` | 续页卡：`<div class="header"><span class="running-title">文章标题</span></div>`；首卡或单卡：空字符串 |
| `{{TITLE_BLOCK}}` | 首卡有标题时：`<div class="title-area"><h1>标题</h1></div>`；续页卡或无标题时：空字符串 |
| `{{BODY_HTML}}` | 步骤 5 生成的 HTML |
| `{{SOURCE_LINE}}` | 内容来源（可选）：`<span class="info-source">来源文字</span>`，无来源时空字符串 |
| `{{PAGE_INFO}}` | 多卡时 `1 / N`，单卡时空字符串 |
| `{{BRAND_SIGNATURE}}` | 仅在最后一张卡追加：`<div class="brand-signature"><span class="paw">🐾</span><span class="slogan">无场景，不AI。</span></div>`。非末页不加。 |

**结尾标记**：仅在最后一张卡的 `{{BODY_HTML}}` 末尾追加 `<p style="text-align:right;font-size:16px;color:#ACACB0;margin-top:40px;">∎</p>`。非末页不加。

写入：`/tmp/catclub_cast_poster_{name}_{N}.html`

## 步骤 9：截图

```bash
node {SKILL_DIR}/assets/capture.js /tmp/catclub_cast_poster_{name}_{N}.html {OUTPUT_DIR}/{name}_{N}.png {CANVAS_WIDTH} 1440
```

`{CANVAS_WIDTH}` 使用步骤 6 确定的画布宽度（750/900/1080）。

多张卡片可并行截图。多平台版本可并行生成。

交付时报告卡片数量 + 每张摘要（前 30 字）+ 所用平台参数。
