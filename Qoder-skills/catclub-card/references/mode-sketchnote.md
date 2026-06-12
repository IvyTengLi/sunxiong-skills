# 模具：视觉笔记（-v）

## 核心信条

**把一个概念铸成一份编辑式的图文档案。**

不是「手绘风格的排版」。是**编辑式杂志专题**：问题→失败→转折→顿悟→命名，六站叙事弧线，每一站都是一张独立的版面，合起来是一份完整的探案档案。

## 五条公理（不可削减）

1. **有真问题在前**：必须从具体的、可触摸的卡住的瞬间开始，不是从定义开始
2. **必须有失败**：至少一次失败，线性推导杀掉张力
3. **顿悟在前、命名在后**：先"看到那个东西"，再被告知名字。标题不能剧透
4. **「现在」视角**：不是"100年后回望"，是"此刻能看到什么"
5. **文字克制，不点题**：禁止元自指（"你刚才发明了它"等）

## 步骤 1：读取模板

Read `assets/sketchnote_template.html`

模板提供：
- 1080xauto 自适应画布
- 品牌基底：Carrara 大理石噪点 + Miró 星座圆点 + 盖印 Logo
- 点阵背景（28px 间距）
- SVG 噪点滤镜 + 可复用箭头 marker
- 品牌签名栏
- 4 字族变量：--hand（Caveat）/ --sans（Noto Sans SC）/ --mono（JetBrains Mono）/ --serif（Noto Serif SC）
- 模板变量：`{{CANVAS_WIDTH}}` `{{BASE_TEXT_SIZE}}` `{{BASE_LINE_HEIGHT}}` `{{PAD_X}}` `{{PAD_Y}}` `{{LOGO_FILE}}` `{{CUSTOM_CSS}}` `{{CONTENT_HTML}}` `{{SOURCE_LINE}}`

## 步骤 2：内容预处理

- 识别标题行（`#`/`##`/`###` 开头，或独立短行）
- 识别引用块（`>` 开头）
- 识别加粗（`**text**`）
- 识别列表（`- ` 或 `1. ` 开头）
- 识别代码块
- 按空行分割为段落列表

## 步骤 3：6 站叙事弧线

将内容重构为 6 站叙事弧线。每站对应一种 layout 模具：

| 站 | 叙事功能 | Layout 模具 | 核心任务 |
|---|---------|------------|---------|
| 1 | **起点** | feature | 抛出具体的真问题 |
| 2 | **失败** | note | 第一次尝试 + 便签批注 |
| 3 | **失败** | archive | 第二次尝试 + 档案印章 |
| 4 | **转折** | cross | 视角翻转的爆点 |
| 5 | **顿悟** | hero | 核心洞察 + pull-quote |
| 6 | **命名** | closing | 概念名 + byline + 余韵 |

**不是每份内容都有 6 站**。如果内容只有 3-4 站，跳过不存在的站，但**顺序不可打乱**。

### 3.1 各站 layout 模具

#### feature — 开篇广角
- grid 6fr/6fr，左大图/大数字右文字
- 背景用 `--block` 浅色块
- 标题用 --serif，正文用 --sans
- **模板类**：`.feature-grid`（已预定义于 sketchnote_template.html）
  ```html
  <div class="feature-grid">
    <div class="feature-visual">[SVG/数字/图形]</div>
    <div class="feature-text">
      <h2>标题</h2>
      <p>正文</p>
    </div>
  </div>
  ```

#### note — 便签批注
- 双栏 grid：左 sidekick（小图/公式/箭头）+ 右便签纸
- 便签微旋 0.5deg，虚线穿孔，红笔删除线
- 手写体 --hand 用于批注
- **模板类**：`.note-grid`
  ```html
  <div class="note-grid">
    <div class="note-sidekick">[SVG/箭头/公式]</div>
    <div class="note-paper">
      <p><span class="strike">被删除的文字</span></p>
      <p class="hand-note">手写批注</p>
    </div>
  </div>
  ```

#### archive — 档案标签
- 黑色印章 ✕ + verdict 红色 italic
- 档案编号用 --mono
- 背景用 `--block` 比 feature 更淡
- **模板类**：`.archive-stamp`
  ```html
  <div class="archive-stamp">
    <span class="stamp-x">✕</span>
    <div>
      <p class="stamp-verdict">裁决文字</p>
      <p class="stamp-id">ARCH-001</p>
    </div>
  </div>
  ```

#### cross — 转场爆点
- Serif 200px mega 字 + `--gold` 高亮
- 全宽，无侧边栏
- 留白最大（margin-top 为全稿最高）
- **模板类**：`.cross-mega`
  ```html
  <div class="cross-mega">
    <p class="mega-word">关<span class="gold-highlight">键</span>词</p>
    <p class="mega-sub">副标题说明</p>
  </div>
  ```

#### hero — 顿悟特写
- grid 7fr/5fr + `--miro-blue` 顶边 4px
- pull-quote 大引号
- 核心洞察用 --serif 大字号
- **模板类**：`.hero-grid`
  ```html
  <div class="hero-grid">
    <div class="hero-pullquote">核心洞察文字</div>
    <div class="hero-insight">解释说明</div>
  </div>
  ```

#### closing — 终格静默
- 中心对称 + 双线顶边
- mega-name 144px
- byline 小字在下方
- 余韵留白（margin-bottom 为全稿最大）
- **模板类**：`.closing-center`
  ```html
  <div class="closing-center">
    <p class="closing-name">概念名</p>
    <p class="closing-byline">BYLINE / 来源</p>
  </div>
  ```

### 3.2 漫画分镜式节奏

留白跟着叙事走：
- 起点：开阔（margin-top: 大）
- 失败：紧（margin-top: 中）
- 失败：紧（margin-top: 中）
- 转折：爆（margin-top: 最大）
- 顿悟：开阔（margin-top: 大）
- 命名：静（margin-top: 中，margin-bottom: 最大）

6 节 margin-top 不能相同。

### 3.3 4 字族对比

| 字族 | 字体 | 用途 |
|------|------|------|
| Sans | Noto Sans SC | 正文 body、说明文字 |
| Serif | Noto Serif SC | 大标题、命名、引言、pull-quote |
| Hand | Caveat + 楷体 | 手写批注、设问、caption、sidekick 标注 |
| Mono | JetBrains Mono | 编号、byline、stamp、档案编号 |

### 3.4 品牌色值映射

| 语义 | CSS 变量 | 色值 | 用途 |
|------|---------|------|------|
| 背景 | --bg | #F0F0F0 | Carrara 冷白 |
| 主文字 | --ink | #1A1A1A | 墨色 |
| 次要文字 | --ink-light | #4A4A4A | 辅助说明 |
| 强调色 | --accent | #C45C2E | Chirico 锈橙，跟随色调感知 |
| 深蓝 | --miro-blue | #1E3A5F | Miró 深蓝，用于顶边/标签 |
| 金箔 | --gold | #C9A227 | Kiefer 金箔，用于高亮/装饰 |
| 标记 | --marker | rgba(201,162,39,0.3) | 荧光笔底色 |
| 区块 | --block | #E8E8E8 | 卡片/区块背景 |
| 红 | --miro-red | #C41E3A | Miró 红圆点/印章 |
| 黄 | --miro-yellow | #E6A817 | Miró 黄空心圆 |

## 步骤 4：布局设计

视觉笔记的核心是**非线性阅读路径**。

- 用箭头连接相关概念（SVG marker）
- 用框圈出关键术语
- 用色块高亮金句
- 用图标代替部分文字
- 允许文字旋转 15-30°
- 允许元素重叠

## 步骤 5：格式化为 HTML

所有内容写入 `{{CONTENT_HTML}}`，所有样式写入 `{{CUSTOM_CSS}}`。

**基础元素：**
- 标题 → `<h1>` / `<h2>`，Serif 字体
- 段落 → `<p>`，允许旋转
- 列表 → `<ul>`，用自定义 bullet（手绘圆点、方框、星号）
- 金句 → `<div class="highlight-box">`，色块背景
- 连接 → `<svg>` 箭头，手绘风格
- 批注 → `<span class="hand">`，Caveat 手写体
- 编号 → `<span class="mono">`，JetBrains Mono

### 步骤 5.5：品牌视觉组件（强制）

视觉笔记模板已预置以下品牌组件，必须在 6 站叙事中自然嵌入：

**必须使用的组件（至少 3 个）：**
- `.paw-divider` — 站与站之间的呼吸分隔
- `.chirico-quote` — 关键引用/书摘
- `.elite-seal` — 档案/认证标记（适合 archive 站）
- `.stage-number` — 站编号（01-06）
- `.dropcap` — 开篇首字下沉
- `.dual-signature` — 结尾双角色 byline（头像通过 CSS 自动显示）

**HTML 示例：**
```html
<div class="paw-divider"><span class="paw-icon">🐾</span></div>
<blockquote class="chirico-quote"><p>关键引用</p></blockquote>
<span class="elite-seal">ARCHIVE</span>
<span class="stage-number">03</span>
<p class="dropcap">开篇段落……</p>
<div class="dual-signature">
  <div class="sig-block pallas"><div class="sig-name">兔狲</div><p class="sig-text">……</p></div>
  <div class="sig-block panda"><div class="sig-name">猫熊</div><p class="sig-text">……</p></div>
</div>
```

**禁止**：6 站全用手写体无品牌组件；用普通数字代替 `.stage-number`；整张视觉笔记没有≥3种品牌组件。

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

**视觉笔记特殊规则**：
- 模板已注入 `--canvas-width`、`--base-text`、`--base-line`、`--pad-x`、`--pad-y` 等 CSS 变量
- 在 `{{CUSTOM_CSS}}` 中，使用 `var(--base-text)` 作为基准计算所有字号
- 手绘标注最小不低于 `calc(var(--base-text) * 0.75)`

## 步骤 7：选择 Logo 版本

视觉笔记模具默认使用纯图形 Logo（手绘风格，完整 logo 过于正式）：

| 条件 | 使用文件 |
|------|---------|
| 非暗色模式 | `logo-icon.png` |
| 暗色模式（`-d`） | `logo-icon-dark.png` |

## 步骤 8：渲染模板

替换变量：

| 变量 | 内容 |
|------|------|
| `{{CANVAS_WIDTH}}` | 步骤 6 确定的画布宽度（如 `750px`、`900px`、`1080px`） |
| `{{BASE_TEXT_SIZE}}` | 步骤 6 确定的正文字号（如 `32px`、`36px`、`40px`） |
| `{{BASE_LINE_HEIGHT}}` | 步骤 6 确定的行高（如 `1.8`、`1.7`、`1.6`） |
| `{{PAD_X}}` | 步骤 6 确定的水平边距（如 `48px`、`72px`） |
| `{{PAD_Y}}` | 步骤 6 确定的垂直边距（等于 PAD_X 或略小） |
| `{{LOGO_FILE}}` | 步骤 7 确定的 Logo 文件名 |
| `{{CUSTOM_CSS}}` | 全部 CSS（含 6 站 layout 模具样式） |
| `{{CONTENT_HTML}}` | 全部 HTML（含 6 站内容区块） |
| `{{SOURCE_LINE}}` | 内容来源（可选） |
| `{{BRAND_SIGNATURE}}` | 品牌签名：`<div class="brand-signature"><span class="paw">🐾</span><span class="slogan">无场景，不AI。</span></div>` |

写入：`/tmp/catclub_cast_sketchnote_{name}.html`

## 步骤 9：自检

- [ ] 是否有真问题在前？（不是从定义开始）
- [ ] 是否有至少一次失败？
- [ ] 顿悟是否在前、命名是否在后？
- [ ] 6 站叙事弧线是否完整？（允许跳过不存在的站，但顺序不可打乱）
- [ ] 6 节 margin-top 是否各不相同？
- [ ] 是否用了 4 字族对比？（Sans/Serif/Hand/Mono）
- [ ] 是否避免了中文翻译腔？
- [ ] SVG viewBox 是否对齐实测高度？
- [ ] sidekick 是否为空？（至少放 SVG/公式/箭头之一）
- [ ] 品牌签名是否为 🐾 + slogan？

## 步骤 10：截图

```bash
node {SKILL_DIR}/assets/capture.js /tmp/catclub_cast_sketchnote_{name}.html {OUTPUT_DIR}/{name}.png {CANVAS_WIDTH} 800 fullpage
```

`{CANVAS_WIDTH}` 使用步骤 6 确定的画布宽度（750/900/1080）。

## Known Pitfalls

- **SVG viewBox 必须对齐实测高度**：如果内容撑高到 2400px，viewBox 必须设为 `0 0 1080 2400`，否则截图会裁切
- **sidekick 不能空**：note 模具的左栏至少放 SVG/公式/箭头之一，不能留白
- **cross mega 字不能默认套"等等——"**：必须是内容本身的高潮词
- **中文翻译腔是最容易触的雷**：逐句默念检查
- **禁止元自指**："你刚才发明了它""这就是 X 的本质"等一律删除
