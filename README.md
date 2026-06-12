# SunXiong Skills

狲熊AI训练营（PandaCat AI Camp）品牌专属技能集合。这些技能为内容创作、品牌建设和公众号排版提供了一站式解决方案。

## 📦 技能列表

### 1. CatClub-Card - 铸卡器

**功能**：将文本/URL/文件转换为品牌风格的 PNG 视觉卡片

**7种模具**：
- `-l` 长图：单张阅读卡片，内容自动撑高
- `-i` 信息图：数据可视化驱动的信息图表
- `-m` 多卡：自动切分为多张阅读卡片
- `-v` 视觉笔记：手绘风格 sketchnote
- `-c` 漫画：日式黑白漫画风格
- `-w` 白板：结构化框图+箭头+彩色标记
- `-b` 大字：碑刻大字 + 和紙风格，小红书附件风

**特色**：
- 自动平台适配（微信手机/电脑端、小红书、公众号、海报）
- 暗色模式支持
- 智能 Logo 摆放决策
- 品牌色调自动感知
- 输出到当前工作目录

**触发词**：铸、cast、做成图/卡片/信息图/海报、视觉笔记、sketchnote、漫画、comic、白板、whiteboard、大字、big fonts、小红书卡片

---

### 2. CatsClub-Wechat - 狲熊排版器

**功能**：将 Markdown 或纯文本转换为微信公众号文章排版

**6种品牌风格**：
- **罗马废墟**：品牌故事、招募长文（锈橙 + 深蓝）
- **午夜广场**：技术深潜、Agent教程（深蓝 + Tyrian紫）
- **通关手册**：课程讲义、任务指南（金箔 + 锈橙）
- **短随想**：碎片化思考、灵感（墨灰 + 锈橙）
- **幽默小故事**：行业吐槽、荒诞叙事（锈橙 + 深蓝）
- **金箔拼贴**：成就展示、限量内容（金箔 + Tyrian紫）

**特色功能**：
- 🐾 品牌签名自动附加
- 幽默元素体系（内心OS、角色对话、包袱标记、任务卡片、血条等）
- 智能内容审视与AI插图提示词生成
- 实时浏览器预览（左侧编辑，右侧手机预览）
- 图片占位卡槽机制（完美适配微信公众号编辑器）
- 一键复制粘贴到公众号

**触发词**：狲熊排版、公众号排版、文章排版、排版工作室、排版美化、打开排版器

---

### 3. brand-voice-builder - 品牌声音构建器

**功能**：通过互动问答帮助用户找到品牌声音，生成完整的品牌三件套

**输出文件**：
- `BrandVoice.md`：品牌语气DNA、禁飞区词汇、示例对照
- `Product.md`：产品描述（一句话/一段话/一页纸三个层级）
- `Design.md`：完整网页设计系统规范（design tokens + 组件库）
- `AGENTS.md`：QoderWork 规则文件（始终生效 + 条件触发）

**工作流程**：
1. **资产收集**：Logo、品牌色、现有物料
2. **品牌发现**：六轮深度问答（品类→人群→对标→原型混合→基础信息→视觉系统）
3. **文件生成**：基于12种品牌原型混合配方生成三件套
4. **规则部署**：生成 AGENTS.md 并总结

**特色**：
- 12种品牌原型混合（智者、探险家、魔法师、英雄、叛逆者等）
- 色彩推导公式（hover/focus 状态自动生成）
- 行业定制组件（建筑/SaaS/消费品/教育/专业服务）
- 禁飞区自动生成（行业陈词滥调 + 原型冲突词 + 空泛形容词）

**触发词**：品牌声音、品牌规范、BrandVoice、品牌三件套、找到我的品牌风格、建立品牌知识库、品牌人格

---

## 🎯 使用场景

| 需求 | 推荐技能 |
|------|---------|
| 把文章变成视觉卡片发朋友圈/小红书 | CatClub-Card |
| 制作信息图、漫画、视觉笔记 | CatClub-Card |
| 公众号文章排版美化 | CatsClub-Wechat |
| 建立品牌视觉和语气规范 | brand-voice-builder |
| 为新项目定义品牌人格 | brand-voice-builder |
| 快速生成带品牌签名的PNG | CatClub-Card |

---

## 📁 目录结构

```
sunxiong-skills/
├── Qoder-skills/
│   ├── CatClub-Card/           # 铸卡器
│   │   ├── SKILL.md
│   │   ├── package.json        # Playwright 依赖
│   │   ├── assets/             # 模板HTML、Logo、头像、截图脚本
│   │   └── references/         # 品牌设计规范、各模具详细说明
│   │
│   ├── CatsClub-Wechat/        # 狲熊排版器
│   │   ├── SKILL.md
│   │   ├── README.md
│   │   ├── test-prompts.json
│   │   ├── assets/             # studio.html、Cover.gif
│   │   └── references/         # 6种风格样式定义
│   │
│   └── brand-voice-builder/    # 品牌声音构建器
│       ├── SKILL.md
│       ├── archetypes.md       # 12种品牌原型详解
│       ├── templates.md        # 三件套文件模板
│       ├── readme.txt
│       └── test-prompts.json
│
└── README.md                   # 本文档
```

---

## 🚀 安装方法

### 方式一：

直接复制到 Qoder/QoderWork skills 目录

### 方式二：

在Qoder/QoderWork 技能上添加

### 依赖安装（仅 CatClub-Card 需要）

```bash
cd Qoder-skills/CatClub-Card
npm install playwright
npx playwright install chromium
```

---

## 🎨 品牌资产

所有技能共享狲熊AI训练营品牌视觉体系：

- **主色板**：Carrara大理石冷白 `#F0F0F0`、Chirico锈橙 `#C45C2E`、Miro深蓝 `#1E3A5F`、Kiefer金箔 `#C9A227`
- **字体**：Noto Serif SC（标题）、Noto Sans SC（正文）、JetBrains Mono（代码/标注）
- **Logo**：完整版（图形+文字）、纯图形版、深色/浅色变体
- **角色头像**：兔狲（Pallas）、猫熊（Panda），Base64内嵌
- **品牌签名**：🐾 狲熊AI训练营 + Slogan（无场景，不AI。）

详见各技能的 `references/` 目录中的品牌规范文档。

---

## 🙏 致谢

- **Qoder**：AI 编程助手
- **QoderWork**：AI Agent桌面助手

---

🐾 *狲熊AI训练营 · 无场景，不AI。*
