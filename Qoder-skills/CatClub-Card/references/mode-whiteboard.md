# 模具：白板（-w）

## 步骤 1：读取模板

Read `{SKILL_DIR}/assets/whiteboard_template.html`

## 步骤 2：内容预处理

- 识别标题行（`#`/`##`/`###` 开头，或独立短行）
- 识别列表（`- ` 或 `1. ` 开头）
- 识别层级关系（缩进、嵌套）
- 识别流程/步骤（首先、然后、最后等）
- 识别对比关系（A vs B、优点/缺点）

## 步骤 3：白板结构选择

根据内容类型，选择一种白板结构：

| 结构 | 特征 | 适用内容 |
|------|------|---------|
| **思维导图** | 中心放射、分支连接 | 概念发散、头脑风暴 |
| **流程图** | 箭头串联、步骤递进 | 操作步骤、SOP |
| **矩阵** | 四象限、表格 | 对比分析、决策 |
| **时间轴** | 横向/纵向时间线 | 历史、项目进度 |
| **架构图** | 分层、模块、接口 | 系统架构、组织 |

选择依据：扫描内容结构，匹配最贴近的白板类型。

## 步骤 4：布局设计

**品牌色应用**：
- 主线条：Ink `#1A1A1A`
- 强调框：PallasCat Rust `#C45C2E`（狲熊品牌主色）
- 次要框：Panda Blue `#1E3A5F`（狲熊品牌次要色）
- 高亮标记：Gold Foil `#C9A227`
- 背景：Board `#F7F3EC`
- 便签色：
  - 红色便签：`#E85D5D`
  - 蓝色便签：`#5D9FE8`
  - 绿色便签：`#5DE88A`
  - 黄色便签：`#E8D45D`

**手绘感元素**：
- 边框：2-3px 实线，略带不规则
- 箭头：手绘风格，带箭头标记
- 便签：旋转 2-5°，阴影
- 文字：Kalam 手写体 + 偶尔的大写标记

## 步骤 5：格式化为 HTML

所有内容写入 `{{CONTENT_HTML}}`，所有样式写入 `{{CUSTOM_CSS}}`。

**基础元素：**
- 容器 → `<div class="board">`
- 框 → `<div class="box">`（带边框、背景色）
- 箭头 → `<svg>` 带 `marker-end`
- 便签 → `<div class="sticky">`（旋转、阴影）
- 列表 → `<ul>` 或编号步骤
- 分组 → `<div class="cluster">`（虚线框）

### 步骤 5.5：品牌视觉组件（强制）

白板模板已预置以下品牌组件，必须在内容中使用：

**必须使用的组件（至少 2 个）：**
- `.paw-divider` — 步骤/分组之间的分隔
- `.chirico-quote` — 关键原则/名言引用
- `.elite-seal` — 重点标记/认证标签
- `.stage-number` — 步骤编号（流程类白板必须）
- `.dual-signature` — 兔狲/猫熊双角色结论签名

**HTML 示例：**
```html
<div class="paw-divider"><span class="paw-icon">🐾</span></div>
<blockquote class="chirico-quote"><p>关键原则</p></blockquote>
<span class="elite-seal">KEY</span>
<span class="stage-number">01</span>
<div class="dual-signature">
  <div class="sig-block pallas"><div class="sig-name">兔狲</div><p class="sig-text">……</p></div>
  <div class="sig-block panda"><div class="sig-name">猫熊</div><p class="sig-text">……</p></div>
</div>
```

**禁止**：纯框图无品牌元素；用普通数字列表代替 `.stage-number`；整张白板没有≥2种品牌组件。

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

**白板特殊规则**：
- 模板已注入 `--canvas-width`、`--base-text`、`--base-line`、`--pad-x`、`--pad-y` 等 CSS 变量
- 在 `{{CUSTOM_CSS}}` 中，使用 `var(--base-text)` 作为基准计算所有字号
- 框内文字最小不低于 `calc(var(--base-text) * 0.83)`（约 25-33px）
- 便签文字最小不低于 `calc(var(--base-text) * 0.75)`（约 22-30px）

## 步骤 7：渲染模板

替换变量：

| 变量 | 内容 |
|------|------|
| `{{CANVAS_WIDTH}}` | 步骤 6 确定的画布宽度（如 `750px`、`900px`、`1080px`） |
| `{{BASE_TEXT_SIZE}}` | 步骤 6 确定的正文字号（如 `32px`、`36px`、`40px`） |
| `{{BASE_LINE_HEIGHT}}` | 步骤 6 确定的行高（如 `1.8`、`1.7`、`1.6`） |
| `{{PAD_X}}` | 步骤 6 确定的水平边距（如 `48px`、`72px`） |
| `{{PAD_Y}}` | 步骤 6 确定的垂直边距（等于 PAD_X 或略小） |
| `{{CUSTOM_CSS}}` | 全部 CSS |
| `{{CONTENT_HTML}}` | 全部 HTML |
| `{{SOURCE_LINE}}` | 内容来源（可选） |
| `{{BRAND_SIGNATURE}}` | 品牌签名：`<div class="brand-signature"><span class="paw">🐾</span><span class="slogan">无场景，不AI。</span></div>` |

写入：`/tmp/catclub_cast_whiteboard_{name}.html`

## 步骤 9：截图

```bash
node {SKILL_DIR}/assets/capture.js /tmp/catclub_cast_whiteboard_{name}.html {OUTPUT_DIR}/{name}.png {CANVAS_WIDTH} 800 fullpage
```

`{CANVAS_WIDTH}` 使用步骤 6 确定的画布宽度（750/900/1080）。
