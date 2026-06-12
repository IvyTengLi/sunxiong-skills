# 模具：漫画（-c）

## 步骤 1：读取模板

Read `{SKILL_DIR}/assets/comic_template.html`

## 步骤 2：内容预处理

- 将内容拆分为**场景**（scene）
- 每个场景包含：画面描述 + 对话/旁白
- 识别情绪转折点（冲突、高潮、结局）

## 步骤 3：漫画家视觉语言选择

根据内容气质，选择一种漫画家风格：

| 风格 | 特征 | 适用内容 |
|------|------|---------|
| **手冢治虫** | 大圆眼睛、夸张表情、流畅动作线 | 科普、教育、轻松 |
| **井上雄彦** | 粗粝线条、写实比例、泼墨背景 | 体育、热血、成长 |
| **浦泽直树** | 精密网格、悬疑氛围、大量留白 | 悬疑、推理、心理 |
| **宫崎骏式** | 柔和线条、自然背景、温暖色调 | 治愈、自然、童话 |

选择依据：扫描内容关键词和情绪弧线，匹配最贴近的漫画家。

## 步骤 4：分镜设计

- 每页 3-6 格
- 格子的形状和大小反映节奏：
  - 宽横格 = 全景、建立场景
  - 竖长格 = 紧张、压迫
  - 小方格 = 快速切换、细节
  - 破格 = 高潮、冲击

**品牌色应用**（黑白漫画中的灰度）：
- 网点密度：20%（淡）、40%（中）、60%（深）
- 强调色（仅限封面或彩色插页）：PallasCat Rust `#C45C2E`（狲熊品牌主色）

## 步骤 5：格式化为 HTML

所有内容写入 `{{CONTENT_HTML}}`，所有样式写入 `{{CUSTOM_CSS}}`。

**基础元素：**
- 分镜格 → `<div class="panel">`
- 对话框 → `<div class="bubble">`（ tail 方向根据说话者位置）
- 旁白框 → `<div class="narration">`
- 拟声词 → `<span class="sfx">`（大字号、倾斜、爆炸感）
- 速度线 → `<svg>` 放射线

### 步骤 5.5：品牌视觉组件（强制）

漫画模板已预置以下品牌组件，必须在内容中使用：

**必须使用的组件（至少 2 个）：**
- `.paw-divider` — 章节/场景分隔
- `.chirico-quote` — 旁白引用
- `.elite-seal` — 特殊章节标记（如"限定篇""特别篇"）
- `.stage-number` — 回数编号（第01回、第02回）
- `.dual-signature` — 兔狲/猫熊作者签名（放在结尾）

**HTML 示例：**
```html
<div class="paw-divider"><span class="paw-icon">🐾</span></div>
<blockquote class="chirico-quote"><p>旁白文本</p></blockquote>
<span class="elite-seal">LIMITED</span>
<span class="stage-number">01</span>
<div class="dual-signature">
  <div class="sig-block pallas"><div class="sig-name">兔狲</div><p class="sig-text">……</p></div>
  <div class="sig-block panda"><div class="sig-name">猫熊</div><p class="sig-text">……</p></div>
</div>
```

**禁止**：纯黑白分镜无品牌元素；个人署名替代品牌签名；整张漫画没有≥2种品牌组件。

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

**漫画特殊规则**：
- 模板已注入 `--canvas-width`、`--base-text`、`--base-line`、`--pad-x`、`--pad-y` 等 CSS 变量
- 在 `{{CUSTOM_CSS}}` 中，使用 `var(--base-text)` 作为基准计算所有字号
- 对话框文字最小不低于 `calc(var(--base-text) * 0.83)`（约 25-33px）
- 拟声词可放大至 `calc(var(--base-text) * 3.0)` 以上

## 步骤 7：选择 Logo 版本

漫画模具默认使用纯图形 Logo（漫画风格，完整 logo 破坏画面节奏）：

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
| `{{CUSTOM_CSS}}` | 全部 CSS |
| `{{CONTENT_HTML}}` | 全部 HTML |
| `{{SOURCE_LINE}}` | 内容来源（可选） |
| `{{BRAND_SIGNATURE}}` | 品牌签名：`<div class="brand-signature"><span class="paw">🐾</span><span class="slogan">无场景，不AI。</span></div>` |

写入：`/tmp/catclub_cast_comic_{name}.html`

## 步骤 9：截图

```bash
node {SKILL_DIR}/assets/capture.js /tmp/catclub_cast_comic_{name}.html {OUTPUT_DIR}/{name}.png {CANVAS_WIDTH} 800 fullpage
```

`{CANVAS_WIDTH}` 使用步骤 6 确定的画布宽度（750/900/1080）。
