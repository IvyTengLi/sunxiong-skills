# 品牌三件套文档模板

> 生成文件时严格遵循此结构。用用户收集到的真实信息填充，不要编造。

---

## BrandVoice.md 模板

```markdown
# [品牌名称] — 品牌声音指南

> [一句话品牌定位，不超过20字]

---

## 语气 DNA

**一句话总结**：[品牌的说话方式，用一句有画面感的话概括]

### 人格配方

| 原型 | 占比 | 体现 |
|------|------|------|
| [原型1] | [X]% | [在什么场景/内容中体现] |
| [原型2] | [X]% | [在什么场景/内容中体现] |
| [原型3] | [X]% | [在什么场景/内容中体现] |

### 关键词（说这些）

- [关键词1]：[一句话解释为什么选这个词，什么场景用]
- [关键词2]：[一句话解释]
- [关键词3]：[一句话解释]
- [关键词4]：[一句话解释]
- [关键词5]：[一句话解释]

### 禁飞区（不说这些）

| 禁用表达 | 为什么 | 替换为 |
|----------|--------|--------|
| [禁用词/短语1] | [原因] | [正确示例] |
| [禁用词/短语2] | [原因] | [正确示例] |
| [禁用词/短语3] | [原因] | [正确示例] |
| [禁用词/短语4] | [原因] | [正确示例] |
| [禁用词/短语5] | [原因] | [正确示例] |

> 至少列5条。这是品牌声音的边界。没有边界的品牌，每次都像即兴表演。

---

## 我们这样说话

### 社交媒体
- 语气：[描述]
- 示例：[一段真实的社交媒体文案示例，50字以内]

### 产品介绍
- 语气：[描述]
- 示例：[一段真实的产品介绍示例，100字以内]

### 客户沟通
- 语气：[描述]
- 示例：[一段真实的客户回复示例]

### 邮件/正式场合
- 语气：[描述]
- 示例：[一段真实的邮件开头示例]

---

## 我们绝不这样说话

> [一段"反面教材"——用品牌绝对不会用的方式写一段话。标注每一个问题。]

**反面示例**：
> "[用品牌禁飞区的词汇和语气写一段话]"

**问题分析**：
- "[问题1]" → 品牌不会这么说，因为[原因]
- "[问题2]" → 品牌不会这么说，因为[原因]

---

## 品牌人格速查

当 AI 帮你写东西时，先问自己：
1. 这段话读起来像 [品牌人格的一句话描述] 吗？
2. 里面有没有禁飞区的词？
3. 如果发给用户，他们会认出"这是我们"吗？

如果三个答案不全是"是"，重写。
```

---

## Product.md 模板

```markdown
# [品牌/产品名称] — 产品指南

> [一句话产品描述——说人话版，不是Slogan]

---

## 三个层级

### 一句话版（电梯演讲）
[一句话，20字以内，能让外行听懂你在做什么]

### 一段话版（官网首页）
[3-5句话。做什么、给谁用、解决什么问题、为什么选你。]

### 完整版（Pitch / 合作介绍）
[一段完整的产品介绍，200-300字。包含背景、产品、差异化、客户价值。]

---

## 给谁用

**核心用户**：[一句话描述]
**典型场景**：[用户在什么情况下会想到你]
**用户痛点**：
- [痛点1]
- [痛点2]
- [痛点3]

---

## 解决什么问题

| 问题 | 你的解决方案 |
|------|-------------|
| [用户问题1] | [你的方案] |
| [用户问题2] | [你的方案] |
| [用户问题3] | [你的方案] |

---

## 核心卖点

1. **[卖点1标题]**：[一句话解释]
2. **[卖点2标题]**：[一句话解释]
3. **[卖点3标题]**：[一句话解释]

---

## 为什么选你

[一段话解释差异化。不是"我们更好"，是"我们不同在哪里"。]

---

## 我们不做什么

> 边界和定位一样重要。

- 不做：[不做的1]
- 不做：[不做的2]
- 不做：[不做的3]
```

---

## Design.md 模板

> 这是一份**给 AI 读的网页设计系统规范**，同时也是一份品牌视觉识别手册。
> YAML frontmatter 中的 tokens 让 AI 能直接生成像素级精准的品牌网页。
> Markdown 部分承载设计哲学、视觉禁区和 AI 生图指引。

```markdown
---
version: "1.0"
name: [brand-id]
description: "[一句话品牌视觉定位——色彩灵魂、设计哲学、核心张力]"

colors:
  # 品牌色（含交互状态）
  primary: "[Hex]"
  primary-hover: "[比primary亮5-10%]"
  primary-focus: "[比primary暗5-10%]"
  on-primary: "[primary上的文字色，通常白或深]"
  secondary: "[Hex]"
  secondary-hover: "[比secondary亮5-10%]"
  secondary-focus: "[比secondary暗5-10%]"
  on-secondary: "[secondary上的文字色]"
  # 点缀色（1-2个，含状态）
  accent-1: "[Hex]"
  accent-1-hover: "[Hex]"
  accent-1-focus: "[Hex]"
  accent-2: "[Hex]"        # 可选
  accent-2-hover: "[Hex]"  # 可选
  accent-2-focus: "[Hex]"  # 可选
  # 文字色阶（暖底或冷底，取决于品牌）
  ink: "[Hex — 主文字，不是纯黑]"
  ink-muted: "[Hex — 次要文字]"
  ink-subtle: "[Hex — 辅助文字]"
  ink-tertiary: "[Hex — 最淡文字]"
  # 表面色阶（4级，从浅到深）
  canvas: "[Hex — 默认背景，不是纯白]"
  surface-1: "[Hex — 卡片/面板]"
  surface-2: "[Hex — hover/强调]"
  surface-3: "[Hex — 下拉/子导航]"
  surface-4: "[Hex — 弹窗/最深层]"
  # 分割线
  hairline: "[Hex — 默认]"
  hairline-strong: "[Hex — 强调]"
  hairline-tertiary: "[Hex — 嵌套]"
  # 暗色模式
  inverse-canvas: "[Hex — 深色背景，不是纯黑]"
  inverse-surface-1: "[Hex]"
  inverse-surface-2: "[Hex]"
  inverse-ink: "[Hex — 深色上的主文字]"
  inverse-ink-muted: "[Hex — 深色上次要文字]"
  # 语义色
  semantic-success: "[Hex]"
  semantic-warning: "[Hex]"
  semantic-error: "[Hex]"
  semantic-overlay: "[rgba — 图片上的半透明遮罩]"

typography:
  # 展示级（衬线体/品牌字体）
  display-xl:
    fontFamily: "[品牌展示字体], serif"
    fontSize: 72px
    fontWeight: [600-700]
    lineHeight: 1.08
    letterSpacing: [-1.5 to -2.0px]
  display-lg:
    fontFamily: "[同上]"
    fontSize: 52px
    fontWeight: [600-700]
    lineHeight: 1.12
    letterSpacing: [-1.0px]
  display-md:
    fontFamily: "[同上]"
    fontSize: 36px
    fontWeight: [600-700]
    lineHeight: 1.18
    letterSpacing: [-0.5px]
  headline:
    fontFamily: "[品牌中文展示字体], serif"
    fontSize: 28px
    fontWeight: 600
    lineHeight: 1.30
    letterSpacing: [-0.3px]
  card-title:
    fontFamily: "[同上]"
    fontSize: 22px
    fontWeight: 600
    lineHeight: 1.35
    letterSpacing: [-0.1px]
  # 正文级（无衬线体/UI字体）
  subhead:
    fontFamily: "[品牌无衬线体], sans-serif"
    fontSize: 20px
    fontWeight: 400
    lineHeight: 1.50
    letterSpacing: 0
  body-lg:
    fontFamily: "[同上]"
    fontSize: 18px
    fontWeight: 400
    lineHeight: 1.72
    letterSpacing: 0.01em
  body:
    fontFamily: "[同上]"
    fontSize: 16px
    fontWeight: 400
    lineHeight: 1.75
    letterSpacing: 0.01em
  body-sm:
    fontFamily: "[同上]"
    fontSize: 14px
    fontWeight: 400
    lineHeight: 1.65
    letterSpacing: 0.01em
  caption:
    fontFamily: "[同上]"
    fontSize: 12px
    fontWeight: 400
    lineHeight: 1.50
    letterSpacing: 0.02em
  # 功能级
  button:
    fontFamily: "[无衬线体], sans-serif"
    fontSize: 14px
    fontWeight: 500
    lineHeight: 1.20
    letterSpacing: 0.06em
    textTransform: "uppercase"
  eyebrow:
    fontFamily: "[等宽字体], monospace"
    fontSize: 11px
    fontWeight: 500
    lineHeight: 1.30
    letterSpacing: 0.10em
    textTransform: "uppercase"
  mono:
    fontFamily: "[同上]"
    fontSize: 13px
    fontWeight: 400
    lineHeight: 1.60
    letterSpacing: 0
  quote:
    fontFamily: "[品牌展示字体], serif"
    fontSize: 24px
    fontWeight: 500
    lineHeight: 1.45
    letterSpacing: 0.02em
    fontStyle: "italic"
  label:
    fontFamily: "[无衬线体], sans-serif"
    fontSize: 13px
    fontWeight: 500
    lineHeight: 1.30
    letterSpacing: 0.04em

rounded:
  none: 0px
  xs: 2px
  sm: 4px
  md: 6px
  lg: 10px
  xl: 16px
  xxl: 24px
  pill: 9999px
  full: 9999px

spacing:
  xxs: 4px
  xs: 8px
  sm: 12px
  md: 16px
  lg: 24px
  xl: 32px
  xxl: 48px
  section: 96px
  hero: 160px

components:
  # ═══════════════════════════════════════
  # 通用品牌组件（所有品牌都需要）
  # ═══════════════════════════════════════
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.button}"
    rounded: "{rounded.sm}"
    padding: "14px 32px"
  button-primary-hover:
    backgroundColor: "{colors.primary-hover}"
    textColor: "{colors.on-primary}"
    rounded: "{rounded.sm}"
  button-primary-pressed:
    backgroundColor: "{colors.primary-focus}"
    textColor: "{colors.on-primary}"
    rounded: "{rounded.sm}"
  button-secondary:
    backgroundColor: "transparent"
    border: "1.5px solid {colors.primary}"
    textColor: "{colors.primary}"
    typography: "{typography.button}"
    rounded: "{rounded.sm}"
    padding: "12px 30px"
  button-secondary-hover:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    rounded: "{rounded.sm}"
  button-tertiary:
    backgroundColor: "transparent"
    textColor: "{colors.ink-muted}"
    typography: "{typography.button}"
    rounded: "{rounded.sm}"
    padding: "12px 24px"
  button-dark:
    backgroundColor: "{colors.secondary}"
    textColor: "{colors.on-secondary}"
    typography: "{typography.button}"
    rounded: "{rounded.sm}"
    padding: "14px 32px"
  button-dark-hover:
    backgroundColor: "{colors.secondary-hover}"
    textColor: "{colors.on-secondary}"
    rounded: "{rounded.sm}"
  top-nav:
    backgroundColor: "{colors.canvas}"
    textColor: "{colors.ink}"
    typography: "{typography.body-sm}"
    height: "72px"
    backdropBlur: "16px"
    backgroundOpacity: "92%"
  footer:
    backgroundColor: "{colors.inverse-canvas}"
    textColor: "{colors.inverse-ink-muted}"
    typography: "{typography.caption}"
    padding: "80px 48px"
  cta-banner:
    backgroundColor: "{colors.secondary}"
    textColor: "{colors.on-secondary}"
    typography: "{typography.headline}"
    rounded: "{rounded.xl}"
    padding: "64px"
  testimonial-card:
    backgroundColor: "{colors.surface-1}"
    textColor: "{colors.ink-muted}"
    typography: "{typography.body-lg}"
    rounded: "{rounded.lg}"
    padding: "32px"
  text-input:
    backgroundColor: "{colors.surface-1}"
    textColor: "{colors.ink}"
    typography: "{typography.body}"
    rounded: "{rounded.sm}"
    padding: "12px 16px"
    border: "1px solid {colors.hairline}"
  text-input-focused:
    backgroundColor: "{colors.surface-1}"
    textColor: "{colors.ink}"
    rounded: "{rounded.sm}"
    border: "2px solid {colors.primary}"
  tag:
    backgroundColor: "{colors.surface-2}"
    textColor: "{colors.ink-subtle}"
    typography: "{typography.caption}"
    rounded: "{rounded.pill}"
    padding: "4px 12px"

  # ═══════════════════════════════════════
  # 行业/业务展示组件（根据品牌行业定制）
  # 以下组件名和属性根据品牌的行业和需求变化。
  # 生成时至少定义 4-6 个行业专属组件。
  # ═══════════════════════════════════════
  # 示例（空间设计/建筑类）：
  # project-card:        # 项目展示卡——大图+信息
  # gallery-item:        # 图片画廊——保持原始比例
  # philosophy-block:    # 设计理念引用块——左侧品牌色线
  # stat-block:          # 数据展示——深色底+品牌色数字
  #
  # 示例（SaaS/科技类）：
  # feature-card:        # 功能介绍卡——图标+标题+描述
  # pricing-card:        # 定价卡——价格+功能列表+CTA
  # integration-logo:    # 集成品牌logo展示
  #
  # 示例（消费品/餐饮类）：
  # product-card:        # 产品卡——产品图+名称+价格
  # menu-section:        # 菜单分区——分类标题+条目
  # ingredient-block:    # 原料/工艺展示
  #
  # 示例（教育/培训类）：
  # course-card:         # 课程卡——封面+标题+元信息
  # instructor-card:     # 讲师卡——头像+简介+专长
  # curriculum-block:    # 课程大纲展示
  #
  # [根据品牌实际行业，定义对应的展示组件，每个组件包含：
  #  backgroundColor, textColor, typography, rounded, padding,
  #  border, hover状态（如适用）]
---

## Overview

[2-3段品牌视觉哲学描述。解释：
- 品牌色彩灵魂——主色为什么是这个颜色，它代表什么
- 核心设计张力——如"手工感 vs 建筑秩序"、"极简 vs 丰富"
- 字体搭配逻辑——衬线体代表品牌的什么面，无衬线体代表什么面
- 这个文档要求什么、禁止什么]

## Colors

> "[一句与品牌视觉相关的引言]"

### Brand & Accent

[每个颜色一段，包含：
- Token名 + Hex值
- 颜色的品牌含义（不是"蓝色"，是"叙事蓝——庭院黄昏的颜色"）
- 使用场景
- hover/focus 状态的含义（如"像黄铜捕捉到午后光线"）]

### Surface

[描述表面色阶的哲学——从浅到深像什么。
canvas 不是纯白（解释为什么），inverse-canvas 不是纯黑（解释为什么）。
每级 surface 的使用场景。
hairline 边框的用法。]

### Text

[描述文字色阶。ink 不是纯黑——解释它的温度。
ink-muted / ink-subtle / ink-tertiary 各用于什么场景。]

### Semantic

[success / warning / error / overlay 各是什么颜色，为什么选这个颜色。]

## Typography

### Font Family

[3组字体及其角色：
1. 展示/衬线体 —— 品牌的灵魂声音
2. 正文/无衬线体 —— 功能性界面
3. 等宽体 —— 技术精确性
解释为什么选这些字体，它们和品牌原型的关系]

### Hierarchy

| Token | Size | Weight | Line Height | Letter Spacing | Use |
|---|---|---|---|---|---|
| `{typography.display-xl}` | 72px | ... | ... | ... | [用途] |
| ... | ... | ... | ... | ... | ... |

[完整列出所有15个字体token]

### Principles

[3-5条字体使用原则：
- 展示级用负tracking（密度感）
- 正文用正tracking（可读性）
- 按钮/标签用宽tracking（建筑感）
- 衬线体和中文字体的切换逻辑]

## Layout

### Spacing System

- 基础单元：4px
- Tokens: [列出全部spacing tokens]
- 卡片内边距、按钮边距、表单边距的具体规则

### Grid & Container

- 最大内容宽度：1200px
- [根据品牌行业定义的网格规则]
- Hero区：全宽 + hero间距

### Whitespace Philosophy

[品牌的留白哲学——1-2段。和品牌的空间/产品哲学呼应。
引用一句品牌相关的比喻。]

## Elevation & Depth

| Level | Treatment | Use |
|---|---|---|
| 0 (flat) | 无投影无边框 | [用途] |
| 1 (surface lift) | surface-1 + hairline | [用途] |
| 2 (hover lift) | surface-2 + hairline-strong | [用途] |
| 3 (overlay) | semantic-overlay | [用途] |
| 4 (modal) | surface-4 + backdrop blur | [用途] |

[说明品牌用什么方式创造深度感——表面色阶？摄影层次？材质叠加？
明确说明不使用投影（或什么情况下使用）。]

## Shapes

### Border Radius Scale

| Token | Value | Use |
|---|---|---|
| [列出全部rounded tokens] |

### Photography & Imagery

[品牌的摄影艺术指导：
- 图片比例规则（如项目图4:3、人物4:5、细节1:1）
- 摄影风格关键词
- 必须包含/必须避免的元素
- 图片圆角规则]

### Decorative Elements

[品牌的装饰元素规则：
- 分割线样式
- 品牌标记元素
- 明确禁止的装饰（渐变、玻璃拟态等）]

## Components

### 通用组件

#### Buttons

[button-primary / button-secondary / button-tertiary / button-dark 的详细定义。
每个按钮的：背景色、文字色、字体token、内边距、圆角。
hover 和 pressed 状态。
使用场景说明——什么时候用 primary，什么时候用 dark。]

#### Navigation

[top-nav 定义：背景透明度、毛玻璃模糊、高度、Logo位置、链接样式、当前页标记。
footer 定义：深色背景、多列布局、底部版权。]

#### CTA Banner

[cta-banner 定义：深色背景 + 品牌色按钮 = "故事的结尾+邀请"。
标题、副文本、按钮的组合规则。]

#### Inputs & Forms

[text-input 和 text-input-focused 定义。
表单标签、错误状态。]

#### Tags & Badges

[tag 定义：分类标签、筛选器。
active 状态。]

### 行业展示组件

[根据品牌行业，定义 4-6 个专属展示组件。每个组件包含：

**组件名** — 一句话说明
- 视觉定义：背景色、文字色、字体token、圆角、内边距、边框
- 默认状态 和 hover 状态
- 内容结构：顶部放什么、中间放什么、底部放什么
- 使用场景

例如空间设计类：
- **project-card** — 项目叙事卡
- **gallery-item** — 画廊单项
- **philosophy-block** — 设计理念引用
- **stat-block** — 数据成就展示
- **team-card** — 团队成员

例如SaaS类：
- **feature-card** — 功能介绍卡
- **pricing-card** — 定价方案卡
- **integration-card** — 集成/合作伙伴
- **changelog-entry** — 更新日志条目]

## Visual Forbidden Zones

> [品牌的视觉边界——"看起来不像你"的雷区]

[列出 3-5 个绝对不用的视觉风格，每个包含：
- 风格名称
- 典型特征描述
- 为什么不用——和品牌哲学的冲突点]

[列出绝对不用的具体元素]

## Slogans

[定义 2-3 个品牌口号变体：
- 主口号：品牌核心定位（Hero区使用）
- 副口号：价值主张（B2B/Pitch场景）
- 第三口号：品牌哲学（理念展示区）
每个口号标注中英文和使用场景。]

## Do's and Don'ts

### Do

[8-10条具体可执行的Do规则]

### Don't

[8-10条具体可执行的Don't规则]

## Responsive Behavior

### Breakpoints

| Name | Width | Key Changes |
|---|---|---|
| Desktop-XL | 1440px | [描述] |
| Desktop | 1280px | [描述] |
| Tablet | 1024px | [描述] |
| Mobile-Lg | 768px | [描述] |
| Mobile | 480px | [描述] |

### Touch Targets

[所有可交互元素的最小触摸目标尺寸]

### Collapsing Strategy

[导航、网格、图片在不同断点的折叠策略]

## AI Image Generation Prompt Template

**必须包含**：[品牌视觉关键词]
**必须避免**：[品牌禁区关键词]
**参考Prompt**：
> [一段可直接用的AI生图prompt]

## Iteration Guide

[5-8条给AI的迭代指引——怎么用这个design system]

## Known Gaps

[列出已知的不完整项：暗色模式、中文字体加载策略、特定组件缺失等]
```

---

## Rules 文件结构参考

### AGENTS.md 结构

```markdown
# 品牌规则

## 始终生效规则（alwaysApply）

### 品牌语气
- 所有文字输出必须遵守 BrandVoice.md 中定义的语气DNA
- [列出具体规则，每条简洁明了]

### 禁飞区
- 以下词汇和表达绝对禁止：[列出]
- 详见 BrandVoice.md 禁飞区章节

### 视觉底线
- 所有视觉输出必须使用 Design.md 定义的色彩系统
- [列出具体规则]

### 网页/UI生成规范
- 生成网页时，严格使用 Design.md YAML frontmatter 中的 design tokens（`{colors.*}`, `{typography.*}`, `{spacing.*}`, `{rounded.*}`）
- 组件引用 Design.md 中的 `components:` 定义，通过 token 名称调用
- 标题/品牌声音用[展示字体]；正文/UI用[无衬线体]；数据/标注用[等宽体]
- 卡片深度通过表面色阶区分（canvas → surface-1 → surface-2），[说明投影规则]
- 间距基准：区块间 `{spacing.section}` 96px，Hero区 `{spacing.hero}` 160px

### 品牌名称
- 品牌名称的正确写法：[正确写法]
- 错误写法：[错误1]、[错误2]、[错误3]

## 条件触发规则

### 文案生成（当用户要求写文案、内容、帖子、邮件时）
- 参考 BrandVoice.md 语气DNA和"我们这样说话"章节
- [具体规则]

### 视觉设计 / 网页生成（当用户要求生成图片、海报、视觉物料、网页、网站时）
- 参考 Design.md YAML frontmatter 中的完整 design tokens
- 生成网页时：通过 `{components.*}` token 名称引用组件（通用组件 + 行业展示组件），使用 tokens 而非硬编码色值
- 字体规则：[展示字体用于标题]；[无衬线体用于正文]；[等宽体用于标注]
- AI 生图时使用 Design.md 中的参考 Prompt 模板
- [品牌特有的视觉要求]

### 产品介绍（当用户需要介绍产品、写pitch、回答"我们做什么"时）
- 参考 Product.md 三个层级
- [具体规则]
```
