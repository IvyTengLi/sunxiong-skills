---
name: catclub-card
description: "Content caster for 狲熊AI训练营 (PandaCat AI Camp). Transforms text/URL/file into branded PNG visuals. Seven molds: -l long card, -i infograph, -m multi-card (1080x1440), -v sketchnote, -c comic (B&W), -w whiteboard, -b big-fonts (小红书 style). Output to {OUTPUT_DIR}. Trigger: 铸, cast, 做成图/卡片/信息图/海报, 视觉笔记, sketchnote, 漫画, comic, 白板, whiteboard, 大字, 附件图, big fonts, 小红书卡片."
user_invocable: true
version: "1.0.0"
---

# CatClub-Card: 铸

将内容铸成可见的形态。内容进去，PNG 出来。模具决定形状。

**本 skill 为狲熊AI训练营（简称「狲熊训练营」，英文名 PandaCat AI Camp）品牌专用。** 所有输出必须遵循品牌视觉体系（见 `references/brand-design.md`）。

## 参数

### 模具参数（必选其一）

| 参数 | 模具 | 默认画布 | 说明 |
|------|------|---------|------|
| `-l`（默认） | 长图 | 1080 x auto | 单张阅读卡，内容自动撑高 |
| `-i` | 信息图 | 1080 x auto | 内容驱动的自适应视觉布局 |
| `-m` | 多卡 | 1080 x 1440 | 自动切分为多张阅读卡片 |
| `-v` | 视觉笔记 | 1080 x auto | 手绘风格 sketchnote，动态选择风格路线 |
| `-c` | 漫画 | 1080 x auto | 日式黑白漫画风格，动态选择漫画家视觉语言 |
| `-w` | 白板 | 1080 x auto | 白板马克笔风格，结构化框图+箭头+彩色标记 |
| `-b` | 大字 | 1080 x 1440 | 碑刻大字 + 和紙 + 外阴影，小红书附件风格（单句/短段） |

### 分发环境参数（可选，自动适配）

| 参数 | 分发环境 | 画布宽度 | 正文字号 | 行高 | 边距 | 触发信号 |
|------|---------|---------|---------|------|------|---------|
| `-p wx-mobile` | 微信手机端 | 750px | 32px | 1.8 | 48px | 用户提及「微信」「手机」「朋友圈」 |
| `-p wx-desktop` | 微信电脑端 | 1080px | 36px | 1.7 | 72px | 用户提及「电脑」「PC」「Mac」 |
| `-p xhs` | 小红书 | 900px | 30px | 1.9 | 56px | 用户提及「小红书」「xhs」「笔记」 |
| `-p mp` | 微信公众号 | 900px | 30px | 1.85 | 60px | 用户提及「公众号」「推文」「文章配图」 |
| `-p poster` | 海报/直播间 | 1080px | 40px | 1.6 | 80px | 用户提及「海报」「直播」「预告」 |
| `-p default` | 通用（默认） | 1080px | 36px | 1.7 | 72px | 未指定或无法归类 |

**分发环境选择逻辑：**
1. 若用户显式指定 `-p` 参数 → 直接使用对应参数
2. 若未指定 `-p`，扫描用户 prompt 中的触发信号词：
   - 遍历 prompt 全文，检查是否包含上表「触发信号」列中的关键词
   - 命中多个信号时，按优先级排序：wx-mobile > xhs > mp > wx-desktop > poster
   - 示例：prompt 含「微信」+「手机」→ 选 wx-mobile；prompt 含「小红书」+「笔记」→ 选 xhs
3. 无信号命中 → 回退到 default（1080px / 36px / 1.7 / 72px）

🔴 **CHECKPOINT · 平台参数确认**

平台感知完成后，**必须在生成 HTML 前向用户报告所选平台**：「检测到分发环境为【XX】，使用画布【XXXpx】+ 正文【XXpx】+ 行高【X.X】，是否确认？」
- 用户确认 → 继续
- 用户否定 → 让用户指定平台，或进入手动选择模式

**手机可读性保障（所有环境）：**
- 正文字号 ≥ 30px（手机上缩放后 ≥ 11.5px，高于微信最小可读 11px）
- 行高 ≥ 1.6（防止密集排版）
- 边距 ≥ 48px（防止贴边）
- 最大元素与最小元素比例 ≥ 8:1（信息图模式仍保持 ≥ 10:1）

### 暗色模式参数（可选）

| 参数 | 说明 |
|------|------|
| `-d` | 暗色模式：深底浅色字，适合直播间海报、预告、夜间阅读 |

### Logo 参数（可选）

| 参数 | 行为 |
|------|------|
| `--logo` | 添加 Logo（自动决策） | 跳过审查报告，直接按决策矩阵添加 |
| `--logo=stamp` | 强制盖印 | 透明底+旋转，200-280px |
| `--logo=embed` | 强制镶嵌 | 落入深色区，80-200px |
| `--logo=sign` | 强制落款 | 完整版小尺度，48-64px |
| `--logo=ghost` | 强制浮水印 | 400-600px，透明度0.06-0.10 |

**参数解析逻辑：**
- `--logo` 未指定 → `{{LOGO_FILE}}` = 透明占位图 → 生成后展示审查报告
- `--logo` 指定但无值 → 按决策矩阵自动选择 `{{LOGO_FILE}}` + 动作
- `--logo=具体动作` → 按指定动作选择 `{{LOGO_FILE}}` + CSS

## 约束

本 skill 输出为视觉文件（PNG），不适用 L0 中的 Org-mode、Denote 和 ASCII-only 规范。

## 故障处理（Failure Modes）

执行中任一环节出错时，按以下 if-then 表处理，禁止静默跳过。

| 触发条件 | 一线修复 | 仍失败兜底 |
|---------|---------|-----------|
| WebFetch 获取 URL 失败（超时/404/非文本内容） | 重试 1 次；仍失败则向用户报告「无法获取该链接，请粘贴文本」 | 终止执行，不生成空卡片 |
| 粘贴文本为空或仅空白字符 | 向用户确认「内容为空，是否继续？」 | 用户确认空内容 → 生成仅含品牌签名的占位卡片；用户取消 → 终止 |
| Read 文件路径不存在或无权限 | 检查路径拼写；尝试相对路径 → 绝对路径转换 | 仍失败 → 向用户报告路径错误，请求重新提供 |
| 内容提取后 `{name}` 为空（无标题/无核心思想） | 用内容前 6 个汉字作为 fallback 文件名 | 仍无法提取 → 使用 `untitled` + 时间戳 |
| capture.js 运行时崩溃（playwright 未安装/浏览器缺失） | 执行 `cd {SKILL_DIR} && npm install playwright && npx playwright install chromium` | 仍失败 → 向用户报告依赖缺失，提供手动安装命令 |
| 生成的 HTML 文件大小为 0 或模板变量未替换（如 `{{BODY_HTML}}` 残留） | 检查模板变量名拼写；重新渲染 | 仍失败 → 向用户报告渲染错误，不输出损坏文件 |
| 截图输出 PNG 大小为 0 或损坏 | 检查 HTML 文件是否有效；用浏览器打开验证 | 仍失败 → 向用户报告截图失败，保留 HTML 供手动排查 |
| 暗色模式（`-d`）下 Logo 使用错误版本 | 检查 `body.dark` 类是否正确添加；深色画布必须使用 `logo-dark.png` 或 `logo-icon-dark.png` | 自动替换为正确版本并重新截图 |
| 平台参数 `-p` 值非法（非表格中的6个选项） | 向用户报告「不支持的平台参数【XX】，可用选项：wx-mobile / wx-desktop / xhs / mp / poster / default」 | 用户不修正 → 回退到 default 参数继续执行 |
| 平台感知信号冲突（如 prompt 同时含「微信」+「小红书」） | 按优先级排序（wx-mobile > xhs > mp > wx-desktop > poster），选择最高优先级 | 用户不同意自动选择 → 展示冲突信号，让用户手动指定 |
| 手机可读性检测失败（正文字号 < 30px 或行高 < 1.6） | 强制提升到最低标准（30px / 1.6），并记录警告 | 模板变量注入错误 → 检查 `{{BASE_TEXT_SIZE}}` 和 `{{BASE_LINE_HEIGHT}}` 是否正确替换 |

🔴 **CHECKPOINT · 色调选择确认**

步骤 2.5（色调感知）自动选择后，**必须在生成 HTML 前向用户报告所选色调**（背景色 + 强调色），询问：「检测到内容气质为【XX】，使用背景【#XXXXXX】+ 强调【#XXXXXX】，是否确认？」
- 用户确认 → 继续
- 用户否定 → 让用户指定色调，或进入手动选择模式

🔴 **CHECKPOINT · 切分确认（仅 `-m` 多卡模式）**

贪心切分完成后、截图前，**必须向用户报告切分结果**：「内容将分为 N 张卡片，每张约 X 字。是否确认切分？」
- 用户确认 → 继续截图
- 用户否定 → 让用户指定切分阈值（默认 380），重新切分

## 共享基础

### 路径约定

本 skill 所有内部路径使用占位符，实际运行时替换：

| 占位符 | 含义 | 典型值 |
|--------|------|--------|
| `{SKILL_DIR}` | skill 安装目录 | `~/.qoder/skills/CatClub-Card/` 或 runtime 对应路径 |
| `{OUTPUT_DIR}` | 输出目录 | `./`（当前工作目录） |

**所有路径引用统一格式**：`{SKILL_DIR}/assets/...`、`{SKILL_DIR}/references/...`

### 获取内容

- URL --> WebFetch 获取
- 粘贴文本 --> 直接使用
- 文件路径 --> Read 获取

### 文件命名

从内容提取标题或核心思想作为 `{name}`（中文直接用，去标点，≤ 20 字符）。

### 截图工具

```bash
node {SKILL_DIR}/assets/capture.js <html> <png> <width> <height> [fullpage]
```

依赖：`{SKILL_DIR}/node_modules/` 中的 playwright。如报错：

```bash
cd {SKILL_DIR} && npm install playwright && npx playwright install chromium
```

### Footer / 品牌签名

所有模具的 footer 必须统一使用品牌签名，**禁止出现个人签名或任何其他个人标识**。

品牌签名格式（三行，严格遵循 DESIGN.md §components.paw-divider）：
```html
<div class="brand-signature">
  <span class="paw">🐾</span>
  <span class="brand-name">狲熊AI训练营</span>
  <span class="slogan">无场景，不AI。</span>
</div>
```

Slogan 选择规则（按内容主题匹配，禁止随意替换）：

| 内容主题 | 使用 Slogan |
|---------|------------|
| 方法论、工具、实践指南 | "🐾 无场景，不AI。" |
| 学习路径、课程、训练 | "🐾 通关即学习。" |
| 创意、创作、开源、灵感 | "🐾 创造无需许可。" |
| 无法归类 | 默认 "🐾 无场景，不AI。" |

适用于 `-l`、`-i`、`-v`、`-c`、`-w` 模具（`-m` 多卡无 footer，不适用）。

内容来源（可选）——有明确来源时显示在签名右侧（如作者名、arxiv ID、网站名等），无来源时留空。使用 `{{SOURCE_LINE}}` 变量：有来源时填 `<span class="info-source">来源文字</span>`，否则空字符串。

### 交付

1. 报告文件路径
2. 报告卡片数量（`-m` 模式）
3. 报告所选色调（背景色 + 强调色）

## 品牌资产

### Logo

品牌已提供定稿 Logo，所有铸卡输出必须使用以下资产：

| 文件 | 内容 | 线条颜色 | 最佳对比背景 | 最差对比背景 |
|------|------|---------|-------------|-------------|
| `logo-light.png` | 完整版（图形+「狲熊AI训练营」文字） | 黑线 `#1A1A1A` | `#F0F0F0` 浅色 | `#0D0D12` 深色（文字消失） |
| `logo-dark.png` | 完整版（图形+「狲熊AI训练营」文字） | 金线 `#C9A227` | `#0D0D12` 深色 | `#F0F0F0` 浅色（图形消失） |
| `logo-icon.png` | 纯图形（无文字） | 金线 `#C9A227` | 深色背景 ≥ `#2A2A2A` | `#F0F0F0` 浅色（≈1.5:1，隐形） |
| `logo-icon-dark.png` | 纯图形（无文字） | 白线 `#F0F0F0` | 深色背景 ≥ `#1A1A1A` | `#F0F0F0` 浅色（对比度低） |

**核心原则：狲熊是显眼包，Logo 必须被一眼看见。** 所有 Logo 摆放决策以「第一眼能否看到 Logo」为唯一检验标准。

**关键洞察：`logo-icon.png`（金线）和 `logo-dark.png`（金线文字）在 `#F0F0F0` 浅色背景上对比度极差（≈1.5:1），裸放等于隐形。金线必须落在深色背景上。**

**Logo 动作速查：**

| 动作 | 优先级 | 适用场景 | 尺寸 |
|------|--------|---------|------|
| **盖印** | 1 | 信息图标题旁、海报角落 | 200-280px（画布≥1080px） |
| **镶嵌** | 2 | 图表黑底栏、漫画 gutter | 占深色区 60-80% |
| **落款** | 3 | 长图底部、多卡边缘 | 48-64px 高 |
| **浮水印** | 4 | 极简画面、大面积留白 | 400-600px，透明度 0.06-0.10 |

**决策模型**：三维判断法（画面主色调 → 位置背景色 → 视觉竞争密度）。详见 `references/logo-usage.md`。

**各模具默认策略**：`-l` 落款 / `-i` 盖印 / `-m` 首卡落款+其他盖印 / `-v` 盖印 / `-c` 镶嵌 / `-w` 盖印。详见 `references/logo-usage.md` §各模具默认 Logo 策略。

**禁止条款（8条）**：L1-L8，涵盖对比度、尺寸、旋转、边框等。详见 `references/logo-usage.md` §禁止条款。

### 角色头像

品牌提供两个角色头像资源，**必须以 base64 data URI 内嵌到 HTML 输出中，禁止使用外部文件引用**：

| 角色 | 触发词 | 头像文件 | CSS 变量 |
|------|--------|---------|----------|
| 兔狲 | 兔狲、狲、Pallas、pallas | `assets/avatar-pallas.png` | `--avatar-pallas` |
| 猫熊 | 猫熊、猫、Panda、panda | `assets/avatar-panda.png` | `--avatar-panda` |

**嵌入规则：**
- 所有 7 个模板的 `:root` 已预置 `--avatar-pallas` 和 `--avatar-panda` CSS 变量（base64 data URI）
- `.dual-signature` 组件的 `.sig-name::before` 自动显示对应头像（圆形，带阴影）
- `.dialog` 组件的 `.dialog-who::before` 自动显示对应头像（较小圆形）
- **铸卡时不需要手动添加头像 HTML**——只需使用正确的 class（`.pallas` / `.panda`），头像自动渲染
- **禁止**使用外部图片路径引用头像，必须使用模板内嵌的 base64 数据

**英文命名规范：**
- 训练营英文名统一使用 **PandaCat AI Camp**
- 所有英文标识、byline、class alt 文本、closing byline 等均使用此名称
- 禁止使用 SunXiong、Sun Xiong 等其他拼写

## 品牌品味准则

所有模具生成 HTML 前，必须先 Read `references/brand-design.md` 和 `references/taste.md`，作为视觉质量底线贯穿全流程。

核心差异（与通用 ljg-card 的区别）：
- **字体**：中文标题用 Noto Serif SC（品牌衬线体），正文用 Noto Sans SC，标注/代码用 JetBrains Mono
- **色彩**：Carrara 大理石冷白 `#F0F0F0` 为画布；Chirico 锈橙 `#C45C2E` 为强调色；Miro 深蓝 `#1E3A5F` 为次要色；Kiefer 金箔 `#C9A227` 为成就标记
- **Logo**：必须使用定稿 Logo（`logo-light.png` / `logo-dark.png` / `logo-icon.png`），禁止生成或使用任何其他标识
- **签名**：🐾 + 品牌名 + slogan（三行），禁止任何个人署名
- **材质**：Carrara 大理石噪点纹理（冷灰，2.5% opacity），非暖色纸张噪点

## 禁止清单（Don'ts）

以下行为在铸卡过程中**绝对禁止**：

| # | 禁止项 | 后果 |
|---|--------|------|
| 1 | 使用个人签名或任何其他个人标识 | 品牌一致性崩塌 |
| 2 | 使用 Inter 字体替代品牌字体 | 视觉识别失效 |
| 3 | 使用纯黑 `#000000` 作为文字或背景色 | 破坏 Carrara 大理石质感 |
| 4 | 使用 AI 紫蓝渐变、霓虹光晕、外发光效果 | 落入 AI 生成廉价感 |
| 5 | 居中 Hero 标题（DESIGN_VARIANCE > 4 时） | 违反品牌非对称美学 |
| 6 | 三等分等宽卡片布局 | 典型的 AI 生成标志 |
| 7 | 使用「赋能」「无缝」「释放」「下一代」等 AI 文案腔 | 破坏 Tina Fey 式品牌语气 |
| 8 | Tyrian Purple 背景上使用深色文字 | 可读性灾难，必须用白色或金色 |
| 9 | 金箔效果使用平滑渐变替代不规则 clip-path | 失去 Kiefer 拼贴质感 |
| 10 | 生成或使用非定稿 Logo | 品牌识别混乱 |
| 11 | 暗色模式下使用浅色 Logo 版本 | 视觉对比失衡 |
| 12 | 弹点色（强调色）使用超过 2 处 | 稀释视觉冲击力 |

## 执行

根据参数选择模具，Read `references/brand-design.md` + `references/taste.md` + 对应的 mode 文件，按步骤执行。

### 通用执行流程（所有模具共享）

```
Step 1: 获取内容 → URL/WebFetch | 粘贴文本 | 文件路径/Read
Step 2: 提取 {name} → 内容标题或前6字，去标点，≤20字符
Step 3: 读取品牌规范 → Read references/brand-design.md + references/taste.md
Step 4: 选择模具 → 根据参数 -l/-i/-m/-v/-c/-w 进入对应分支
Step 5: 读取模具规范 → Read references/mode-{模具}.md
Step 6: 内容预处理 → 按模具要求识别标题/引用/列表/金句等
Step 6.5: 品牌组件语义映射（强制）→ 将识别出的语义元素映射为品牌视觉组件 class
Step 7: 平台感知 → 根据 -p 参数或触发信号选择画布宽度/字号/行高/边距
Step 8: 色调感知（-l/-m/-i）→ 扫描内容关键词匹配品牌色板，选择背景色+强调色双色对，报告用户确认

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

   > 品牌背景色偏移上限：相对 `#F0F0F0` 控制在 ±8 hex 以内，禁止暖黄/暖橙背景。
Step 9: 选择 Logo 版本 → 根据 `--logo` 参数 + 画布宽度 + 模具类型 + 是否 `-d`，确定 `{{LOGO_FILE}}`

   **默认（无 `--logo`）：**
   - `{{LOGO_FILE}}` = 透明占位图（1px GIF）：`data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7`
   - 视觉上不显示 Logo，仅保留 `.brand-signature`
   - 生成后进入 Step 13.5 展示 Logo 审查报告

   **有 `--logo`（自动决策）：**
   - 根据决策矩阵选择 Logo 文件和动作
   - 浅色背景 → `logo-light.png`（落款）或 `logo-icon.png`（盖印/镶嵌）
   - 深色背景 → `logo-dark.png`（落款）或 `logo-icon-dark.png`（盖印/镶嵌）

   **有 `--logo=具体动作`：**
   - `--logo=stamp` → `logo-icon.png` / `logo-icon-dark.png` + `.brand-logo-stamp`
   - `--logo=embed` → `logo-icon.png` / `logo-icon-dark.png` + `.logo-embedded`
   - `--logo=sign` → `logo-light.png` / `logo-dark.png` + `.brand-logo`
   - `--logo=ghost` → `logo-icon.png` / `logo-icon-dark.png` + `.logo-ghost`

Step 10: 格式化为 HTML → 按模具元素规范生成 HTML，注入模板变量

   **品牌组件语义映射执行规则（Step 6.5 落地）：**

   遍历预处理后的段落列表，按优先级匹配下表，将内容语义转换为对应 HTML class：

   | 内容语义 | 识别信号 | 映射组件 | 使用场景 |
   |---------|---------|---------|---------|
   | 章节分隔 | 空行 + 新标题 | `.paw-divider` | 长图/多卡/视觉笔记章节之间 |
   | 引用/名言 | `>` 开头 或 "XX说" | `.chirico-quote` | 引用名人名言、书摘 |
   | 精英/限量标记 | "精英""限量""通关""认证" | `.elite-seal` | 标记特殊内容等级 |
   | 步骤/阶段 | "第一步""阶段一""01." | `.stage-number` | 教程、流程、步骤 |
   | 首段正文 | 第一个 `<p>` | `.dropcap` | 长图/信息图/视觉笔记开篇 |
   | 双角色观点 | "兔狲""猫熊" 交替出现 | `.dual-signature` | 角色对话后的总结签名（头像自动显示） |
   | 金句/核心洞察 | <25字独立短句 | `.highlight` | 关键结论句 |
   | 戏剧性金句 | 全文最重要的一句 | `.highlight.pull-quote` | 全文仅1处 |
   | 提问/提示 | "为什么""请注意""想想看" | `.prompt` | 抛给读者的问题 |
   | 行动指令 | "去做""任务""练习" | `.quest-card` | 待办、任务、行动项 |
   | 角色对话 | 问答/对话体 | `.dialog` | 兔狲vs猫熊对话（头像自动显示） |
   | 并列条目 | 有标题+正文的列表 | `.item` | 特征列表、要点 |
   | 大章节转折 | 文章结构大切换 | `.shadow-divider` | 章节大停顿 |
   | 呼吸感标记 | 需要视觉节奏处 | `.miro-dots` | 段落间呼吸 |

   **映射执行逻辑：**
   1. 遍历预处理后的段落列表
   2. 对每个段落应用上表规则，按优先级匹配（从上到下）
   3. 匹配成功后，在 HTML 生成时使用对应组件 class
   4. 未匹配到的段落使用默认 `<p>` 或 `<blockquote>`

   **强制检查清单（生成后必须校验，未通过则重写 HTML）：**
   - [ ] 是否至少使用了 3 种不同的品牌视觉组件？（纯文字墙是禁忌）
   - [ ] 是否有 `.paw-divider` 作为章节分隔？（不要用空行代替）
   - [ ] 引用内容是否用了 `.chirico-quote`？（不要用普通 blockquote）
   - [ ] 步骤/流程是否用了 `.stage-number`？（不要用纯数字）
   - [ ] 首段是否用了 `.dropcap`？（长图/信息图/视觉笔记）

   HTML 示例：
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
     <div class="sig-block pallas">
       <div class="sig-name">兔狲</div>
       <p class="sig-text">……</p>
     </div>
     <div class="sig-block panda">
       <div class="sig-name">猫熊</div>
       <p class="sig-text">……</p>
     </div>
   </div>
```


   **变量分组（按功能）：**

   | 组 | 变量 | 说明 |
   |---|------|------|
   | 画布 | `{{CANVAS_WIDTH}}` `{{CANVAS_HEIGHT}}` | 画布尺寸 |
   | 平台 | `{{BASE_TEXT_SIZE}}` `{{BASE_LINE_HEIGHT}}` `{{PAD_X}}` `{{PAD_Y}}` | 字号/行高/边距 |
   | 色调 | `{{BG_COLOR}}` `{{ACCENT_COLOR}}` | 背景色/强调色 |
   | 品牌 | `{{LOGO_FILE}}` `{{BRAND_SIGNATURE}}` | Logo路径/签名HTML |
   | 内容 | `{{TITLE_BLOCK}}` `{{BODY_HTML}}` `{{CONTENT_HTML}}` `{{CUSTOM_CSS}}` | 标题/正文/自定义样式 |
   | 元信息 | `{{SOURCE_LINE}}` `{{PAGE_INFO}}` | 来源/页码 |

   > 各模具实际使用的变量子集见模具分支说明。
Step 11: 写入 HTML 文件 → /tmp/catclub_cast_{模具}_{name}.html
Step 12: 截图 → node {SKILL_DIR}/assets/capture.js <html> <png> <width> <height> [fullpage]
Step 13: 交付 → 报告文件路径 + 卡片数量（-m）+ 色调 + 平台参数 + Logo 状态

   **无 `--logo` 参数时的交付格式：**
   - 报告 PNG 文件路径
   - 报告「本次生成未添加 Logo，仅保留品牌签名」
   - 进入 Step 13.5 展示 Logo 审查报告并询问用户

Step 13.5: Logo 审查报告（仅无 `--logo` 参数时）

   生成 PNG 后，向用户展示审查报告：

   ```
   🐾 铸卡完成。Logo 审查报告如下。

   ━━━━━━━━━━━━━━━━━━━━━━
   【模具类型】{{模具名}}
   【画布尺寸】{{CANVAS_WIDTH}}px
   【背景色调】{{BG_COLOR}}（{{浅色/深色}}）
   ━━━━━━━━━━━━━━━━━━━━━━

   【背景分析】
   这张卡的底色是 {{BG_COLOR}}，属于 {{浅色/深色}} 阵营。
   {{浅色：浅色背景上，金线 Logo 对比度 ≈1.5:1，裸放等于隐形。}}
   {{深色：深色背景是金线 Logo 的主场，对比度 >8:1，一眼可见。}}

   【Logo 推荐】
   推荐动作：{{动作}}
   推荐文件：{{LOGO_PATH}}
   推荐尺寸：{{尺寸}}
   推荐位置：{{位置}}
   理由：{{理由}}

   ━━━━━━━━━━━━━━━━━━━━━━

   要加上 Logo 吗？回复「加」或「放 Logo」，我重新生成带 Logo 的版本。
   觉得位置不对？告诉我「放左上角」「放大一点」「不要旋转」——具体指令都行。
   不想加？直接保存这张就行。🐾 签名已经在了。
   ```

   **用户确认后的迭代生成流程：**
   1. 读取已生成的 HTML 文件：`/tmp/catclub_cast_{模具}_{name}.html`
   2. 解析当前状态：画布宽度、背景色、是否 dark 模式、模具类型
   3. 根据决策矩阵或用户指令确定：LOGO_FILE、动作类型、CSS 参数
   4. 替换 HTML 中的 `{{LOGO_FILE}}` 为实际文件路径
   5. 根据动作类型，在 `<style>` 中注入/覆盖 `.brand-logo` 的 CSS
   6. 写入新文件：`/tmp/catclub_cast_{模具}_{name}_logo.html`
   7. 重新截图：`node capture.js <新html> <新png> <width> <height> [fullpage]`
   8. 交付新 PNG：`{OUTPUT_DIR}/{name}_branded.png`（避免覆盖原文件）
```

### -l（默认）：长图

**输入**：文本内容（任意长度，单张卡片自动撑高）
**输出**：`{OUTPUT_DIR}/{name}.png`，宽度由平台参数决定（750/900/1080px），高度自适应

执行步骤：
1. Read `references/mode-long.md`
2. 内容预处理：识别标题/引用/加粗/金句/列表/段落
3. 平台感知：根据 `-p` 参数或触发信号选择画布宽度/字号/行高/边距
4. 色调感知：扫描关键词匹配色板 → 🔴 CHECKPOINT 确认
5. 格式化：普通段落→`<p>`，金句→`<p class="highlight">`，首段→`<p class="dropcap">`
6. 渲染模板：替换 `{{BG_COLOR}}` `{{ACCENT_COLOR}}` `{{CANVAS_WIDTH}}` `{{BASE_TEXT_SIZE}}` `{{BASE_LINE_HEIGHT}}` `{{PAD_X}}` `{{PAD_Y}}` `{{LOGO_FILE}}` `{{TITLE_BLOCK}}` `{{BODY_HTML}}` `{{SOURCE_LINE}}` `{{BRAND_SIGNATURE}}`
7. 写入 `/tmp/catclub_cast_long_{name}.html`
8. 截图：`node {SKILL_DIR}/assets/capture.js /tmp/catclub_cast_long_{name}.html {OUTPUT_DIR}/{name}.png {CANVAS_WIDTH} 800 fullpage`

> 注：Logo 版本选择已在通用流程 Step 9 中统一处理，模具分支不再重复。

模板：`{SKILL_DIR}/assets/long_template.html`

### -i：信息图

**输入**：文本内容或 URL（需 WebFetch 获取）
**输出**：`{OUTPUT_DIR}/{name}.png`，宽度由平台参数决定（750/900/1080px），高度自适应

**核心定义**：信息图不是「有边框的文字排版」。信息图是**数据可视化 + 图形承载信息**。纯文字列表伪装成信息图是绝对禁止的。

执行步骤：
1. Read `references/mode-infograph.md`
2. 提取元信息：标题（≤15字）/ 副标题（≤30字）/ 来源 / REF 编码
3. 三维判断：密度（稀/中/密）→ 结构（单点/对比/层级/流程/辐射/并列）→ 情绪（沉思/锐利/技术/帝国）
4. **数据→图形转换（新增强制步骤）**：将内容中的量化信息转换为可视化元素
5. 平台感知：根据 `-p` 参数或触发信号选择画布宽度/字号/行高/边距
6. 色调选择：匹配品牌色板 → 覆盖 CSS 变量 → 🔴 CHECKPOINT 确认
7. 设计画面：锚点位置/画面分割/文字大小（最大:最小 ≥10:1）/ 色彩 90/8/2
8. 选择 Logo 版本：按通用流程 Step 9 执行。无 `--logo` 时默认透明占位；有 `--logo` 时信息图优先「盖印」动作
9. 写 CSS + HTML：全部写入 `{{CUSTOM_CSS}}` 和 `{{CONTENT_HTML}}`，CSS 中使用 `var(--base-text)` 计算字号
10. 自检：弹点色≤2处？最大最小比例≥10:1？正文≥`var(--base-text)`？**信息图必须有≥1个数据可视化元素？**
11. 写入 `/tmp/catclub_cast_infograph_{name}.html`
12. 截图：`node {SKILL_DIR}/assets/capture.js /tmp/catclub_cast_infograph_{name}.html {OUTPUT_DIR}/{name}.png {CANVAS_WIDTH} 800 fullpage`

**无数据可可视化时的 fallback**：
若内容中无百分比/比例/时间/对比/数量/步骤/层级等可图形化信息，则：
1. 降级为 `-l` 长图模具执行（保留信息图的大字号标题和视觉张力）
2. 或向用户报告「当前内容缺乏可可视化的数据，建议改用长图模式或补充量化信息」
3. 禁止用 bullet list 伪装信息图

模板：`{SKILL_DIR}/assets/infograph_template.html`

**数据→图形转换规则（强制）**：

| 文字信息类型 | 必须转换为何种图形 | 禁止做法 |
|-------------|-------------------|---------|
| 百分比/比例 | 条形图、进度条、饼图 | 写成「XX%」纯文字 |
| 时间/周期 | 时间轴、流程轴、管道 | 写成「需要X天」纯文字 |
| 对比（A vs B） | 左右双栏仪表盘、对比条 | 写成「A是...B是...」列表 |
| 数量/规模 | 数字大字 + 单位标注 | 写成「有大量...」模糊文字 |
| 步骤/流程 | 箭头连接的节点图 | 写成「第一步...第二步...」列表 |
| 层级关系 | 金字塔、阶梯、嵌套框 | 写成「底层...上层...」文字 |

**信息图反死亡清单（新增）**：

| 如果你发现自己在做这个... | 停下来 |
|------------------------|-------|
| 用 bullet list 展示对比数据 | 换成条形图或仪表盘 |
| 用段落文字描述时间流程 | 换成时间轴或流程图 |
| 用「很多」「大量」「快速」等模糊词 | 换成具体数字 + 可视化 |
| 整个画面没有≥1个图表/图形/时间轴 | 这不是信息图，重做 |
| 最大元素是文字标题而非图形/数字 | 放大数字或图形作为锚点 |

### -m：多卡

**输入**：文本内容（较长，需切分）
**输出**：`{OUTPUT_DIR}/{name}_1.png` ... `{name}_N.png`，每张宽度由平台参数决定（750/900/1080px）×1440

执行步骤：
1. Read `references/mode-poster.md`
2. 内容预处理：识别标题/引用/加粗/金句/列表
3. 平台感知：根据 `-p` 参数或触发信号选择画布宽度/字号/行高/边距
4. 色调感知：匹配色板 → 🔴 CHECKPOINT 确认
5. 计算视觉重量：普通段落×1.4 / 标题×6.0 / 金句×3.0 / 条目×1.8 / 引用×1.7 / 分割线×60
6. 贪心切分：阈值 380，逐段累加，超阈值时切分 → 🔴 CHECKPOINT 确认切分结果
7. 格式化：同 `-l` 模具元素规范
8. 选择 Logo 版本：按通用流程 Step 9 执行。无 `--logo` 时默认透明占位；有 `--logo` 时首卡用「落款」，后续卡用「盖印」
9. 渲染模板（每张卡）：替换 `{{BG_COLOR}}` `{{ACCENT_COLOR}}` `{{CANVAS_WIDTH}}` `{{CANVAS_HEIGHT}}` `{{BASE_TEXT_SIZE}}` `{{BASE_LINE_HEIGHT}}` `{{PAD_X}}` `{{PAD_Y}}` `{{LOGO_FILE}}` `{{HEADER_BLOCK}}` `{{TITLE_BLOCK}}` `{{BODY_HTML}}` `{{PAGE_INFO}}` `{{BRAND_SIGNATURE}}`
10. 写入 `/tmp/catclub_cast_poster_{name}_{N}.html`
11. 截图（并行）：`node {SKILL_DIR}/assets/capture.js /tmp/catclub_cast_poster_{name}_{N}.html {OUTPUT_DIR}/{name}_{N}.png {CANVAS_WIDTH} 1440`
12. 交付：报告 N 张卡片 + 每张前 30 字摘要 + 平台参数

模板：`{SKILL_DIR}/assets/poster_template.html`

### -v：视觉笔记

**输入**：文本内容
**输出**：`{OUTPUT_DIR}/{name}.png`，宽度由平台参数决定（750/900/1080px），高度自适应

**核心定义**：把一个概念铸成一份编辑式的图文档案。6 站叙事弧线（起点→失败→失败→转折→顿悟→命名），每站一种 layout 模具。

执行步骤：
1. Read `references/mode-sketchnote.md`
2. 内容预处理：识别标题/引用/加粗/列表/代码块
3. 平台感知：根据 `-p` 参数或触发信号选择画布宽度/字号/行高/边距
4. 重构为 6 站叙事弧线：扫描内容，映射到 feature/note/archive/cross/hero/closing
5. 布局设计：非线性阅读路径，箭头连接/框圈关键词/色块高亮/文字旋转 15-30°
6. 格式化：标题→`<h1>`/`<h2>`（Serif），段落→`<p>`（允许旋转），金句→`<div class="highlight-box">`，批注→`<span class="hand">`（Caveat），编号→`<span class="mono">`
7. 选择 Logo 版本：按通用流程 Step 9 执行。无 `--logo` 时默认透明占位；有 `--logo` 时视觉笔记优先「盖印」动作
8. 渲染模板：替换 `{{CANVAS_WIDTH}}` `{{BASE_TEXT_SIZE}}` `{{BASE_LINE_HEIGHT}}` `{{PAD_X}}` `{{PAD_Y}}` `{{LOGO_FILE}}` `{{CUSTOM_CSS}}` `{{CONTENT_HTML}}` `{{SOURCE_LINE}}` `{{BRAND_SIGNATURE}}`
9. 写入 `/tmp/catclub_cast_sketchnote_{name}.html`
10. 截图：`node {SKILL_DIR}/assets/capture.js ... {CANVAS_WIDTH} 800 fullpage`

模板：`{SKILL_DIR}/assets/sketchnote_template.html`

### -c：漫画

**输入**：故事/场景描述
**输出**：`{OUTPUT_DIR}/{name}.png`，宽度由平台参数决定（750/900/1080px），高度自适应

执行步骤：
1. Read `references/mode-comic.md`
2. 内容拆分为场景：画面描述 + 对话/旁白
3. 平台感知：根据 `-p` 参数或触发信号选择画布宽度/字号/行高/边距
4. 漫画家风格选择：手冢治虫（科普）/ 井上雄彦（热血）/ 浦泽直树（悬疑）/ 宫崎骏式（治愈）
5. 分镜设计：3-6 格/页，宽横格=全景，竖长格=紧张，小方格=快速切换，破格=高潮
6. 格式化：分镜格→`<div class="panel">`，对话框→`<div class="bubble">`，拟声词→`<span class="sfx">`
7. 选择 Logo 版本：按通用流程 Step 9 执行。无 `--logo` 时默认透明占位；有 `--logo` 时漫画优先「镶嵌」动作
8. 渲染模板：替换 `{{CANVAS_WIDTH}}` `{{BASE_TEXT_SIZE}}` `{{BASE_LINE_HEIGHT}}` `{{PAD_X}}` `{{PAD_Y}}` `{{LOGO_FILE}}` `{{CUSTOM_CSS}}` `{{CONTENT_HTML}}` `{{SOURCE_LINE}}` `{{BRAND_SIGNATURE}}`
9. 写入 `/tmp/catclub_cast_comic_{name}.html`
10. 截图：`node {SKILL_DIR}/assets/capture.js ... {CANVAS_WIDTH} 800 fullpage`

模板：`{SKILL_DIR}/assets/comic_template.html`

### -w：白板

**输入**：结构化内容（步骤/流程/对比/概念）
**输出**：`{OUTPUT_DIR}/{name}.png`，宽度由平台参数决定（750/900/1080px），高度自适应

执行步骤：
1. Read `references/mode-whiteboard.md`
2. 内容预处理：识别标题/列表/层级/流程/对比
3. 平台感知：根据 `-p` 参数或触发信号选择画布宽度/字号/行高/边距
4. 白板结构选择：思维导图/流程图/矩阵/时间轴/架构图
5. 布局设计：手绘感边框/箭头/便签/分组
6. 格式化：容器→`<div class="board">`，框→`<div class="box">`，箭头→`<svg>`，便签→`<div class="sticky">`
7. 选择 Logo 版本：按通用流程 Step 9 执行。无 `--logo` 时默认透明占位；有 `--logo` 时白板优先「盖印」动作
8. 渲染模板：替换 `{{CANVAS_WIDTH}}` `{{BASE_TEXT_SIZE}}` `{{BASE_LINE_HEIGHT}}` `{{PAD_X}}` `{{PAD_Y}}` `{{LOGO_FILE}}` `{{CUSTOM_CSS}}` `{{CONTENT_HTML}}` `{{SOURCE_LINE}}` `{{BRAND_SIGNATURE}}`
9. 写入 `/tmp/catclub_cast_whiteboard_{name}.html`
10. 截图：`node {SKILL_DIR}/assets/capture.js ... {CANVAS_WIDTH} 800 fullpage`

模板：`{SKILL_DIR}/assets/whiteboard_template.html`

### -b：大字

**输入**：单句/短段（≤60 字）
**输出**：`{OUTPUT_DIR}/{name}.png`，1080x1440 固定画布

**核心定义**：一句话砸在纸上——碑刻于和紙，悬空如附件。审美三柱：重、旧、悬。

执行步骤：
1. Read `references/mode-big.md`
2. 内容形态判断：确认 ≤60 字，否则建议改用 `-l` 或 `-m`
3. 计算字号：按字符数匹配字号表（10字→220px，60字→98px）
4. 手动断行：按语义单元 `<br>` 断行，每行 2-10 字
5. 选择 Logo 版本：按通用流程 Step 9 执行。无 `--logo` 时默认透明占位；有 `--logo` 时大字优先「镶嵌」动作
6. 渲染模板：替换 `{{FONT_SIZE}}` `{{MAIN_TEXT}}` `{{LOGO_FILE}}` `{{SOURCE_LINE}}` `{{CUSTOM_CSS}}`
7. 自检：重/旧/悬 三感齐备？Chirico 锈橙高亮 ≤1 处？
8. 写入 `/tmp/catclub_cast_big_{name}.html`
9. 截图：`node {SKILL_DIR}/assets/capture.js /tmp/catclub_cast_big_{name}.html {OUTPUT_DIR}/{name}.png 1080 1440`

模板：`{SKILL_DIR}/assets/big_template.html`
