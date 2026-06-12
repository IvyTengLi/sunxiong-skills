# 狲熊的毛茸茸AI训练营（简称「狲熊训练营」）— 品牌视觉规范（铸卡专用）

本文件为 catclub-card skill 的铸卡视觉规范。所有 PNG 输出必须遵循以下体系。

---

## 1. 品牌色彩

### 核心色板

| 角色 | 色值 | 用途 |
|------|------|------|
| **Canvas** | `#F0F0F0` | Carrara 大理石冷白画布。所有卡片的默认背景。 |
| **Ink** | `#1A1A1A` | 墨色。主标题、正文。禁止纯黑 `#000000`。 |
| **Ink Muted** | `#4A4A4A` | 次要文字、描述、meta 信息。 |
| **Ink Subtle** | `#7A7A7A` | 辅助文字、时间戳、footer 来源。 |
| **PallasCat Rust** | `#C45C2E` | 主强调色。CTA、狲标记、暖色脉冲、金句左边框。来自 de Chirico《不安的缪斯》锈橙色。 |
| **Panda Blue** | `#1E3A5F` | 次要色。熊标记、技术链接、副标题标签。来自 Miró 晚期 Blue 系列。 |
| **Tyrian Purple** | `#6B2D5C` | 帝国紫。仅用于证书、精英徽章、限量解锁。**紫色上的文字必须是白色或金色，禁止深色文字。** |
| **Gold Foil** | `#C9A227` | Kiefer 式金箔。成就标记、进度条、装饰性下划线。不规则拼贴感。 |
| **Gold Foil Dark** | `#9A7B1A` | 金箔暗部。hover 状态、阴影。 |

### Miró 装饰色（仅用于点缀，不作为主色）

| 角色 | 色值 | 用途 |
|------|------|------|
| **Miró Red** | `#C41E3A` | 实心圆点、箭头。 |
| **Miró Yellow** | `#E6A817` | 咬痕圆点、三角。 |
| **Miró Black** | `#1A1A1A` | 星座线、隐藏 paw 图案。 |

### 表面层级

| 层级 | 色值 | 用途 |
|------|------|------|
| Surface 1 | `#E8E8E8` | 卡片抬升、hover 背景。 |
| Surface 2 | `#E0E0E0` | 精选卡片、CTA 横幅。 |
| Hairline | `#C0C0C0` | 分隔线、边框。 |
| Hairline Strong | `#A0A0A0` | 聚焦环、激活状态。 |

### 长阴影

- 色值：`#B8B8B8`
- 角度：45°
- 仅用于装饰性元素（hero 标题、精神形态），**禁止用于功能性 UI**（按钮、输入框、导航）。

---

## 2. 品牌字体

| 用途 | 字体 | 回退 |
|------|------|------|
| **标题/大字** | Noto Serif SC | Source Han Serif SC, STSong, serif |
| **正文/界面** | Noto Sans SC | Source Han Sans SC, PingFang SC, sans-serif |
| **代码/标注** | JetBrains Mono | ui-monospace, SF Mono, Menlo, monospace |

### 排版原则

- **标题**：负字距（`letter-spacing: -0.03em`），紧凑、纪念碑感。
- **正文**：正字距（`letter-spacing: 0.02em`），中文屏幕可读性。
- ** eyebrow/标签**：大写 + 宽字距（`letter-spacing: 0.08em`），JetBrains Mono。
- **禁止 Inter 字体**。

---

## 3. 品牌签名（Footer）

所有铸卡输出必须在 footer 以品牌签名结束。**禁止出现任何个人署名**（如"李继刚"、个人头像、个人链接）。

### 签名格式（三行，严格遵循 DESIGN.md §components.paw-divider）

```html
<div class="brand-footer">
  <p class="paw">🐾</p>
  <p class="brand-name">狲熊AI训练营</p>
  <p class="slogan">无场景，不AI。</p>
</div>
```

- 第一行：🐾 符号，20-24px，使用强调色（PallasCat Rust `#C45C2E` 或 Panda Blue `#1E3A5F`）
- 第二行：「狲熊AI训练营」，Noto Serif SC, 14px, `#4A4A4A`（Ink Muted）
- 第三行：Slogan，Noto Serif SC, 14px, `#7A7A7A`（Ink Subtle）
- 三行整体居中对齐，上方可选 1px hairline 分隔线

### Slogan 三选一（根据内容语境）

| 语境 | Slogan |
|------|--------|
| 通用/默认 | 🐾 无场景，不AI。 |
| 课程/学习路径/通关 | 🐾 通关即学习。 |
| 创造/Vibe Coding/技术 | 🐾 创造无需许可。 |

### 签名样式

- 字体：Noto Serif SC, 14px, 400 weight
- 颜色：`#7A7A7A`（Ink Subtle）
- 位置：footer 中央或右侧
- paw 符号：20px，与 slogan 同色
- 上方可选：1px `var(--hairline)` 分隔线

---

## 4. 材质与纹理

### Carrara 大理石噪点

```css
background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 512 512' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='6' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
opacity: 0.025;
mix-blend-mode: multiply;
```

- 冷灰色调，2.5% 透明度
- 叠加在画布上，营造大理石颗粒感

### 金箔拼贴（Kiefer-esque）

```css
clip-path: polygon(
  0% 15%, 8% 0%, 25% 5%, 40% 0%, 55% 8%, 70% 2%, 85% 10%, 100% 5%,
  95% 25%, 100% 40%, 92% 55%, 100% 70%, 95% 85%, 100% 100%,
  85% 95%, 70% 100%, 55% 92%, 40% 100%, 25% 95%, 10% 100%, 0% 85%,
  5% 70%, 0% 55%, 8% 40%, 0% 25%
);
```

- 不规则多边形 clip-path，模拟金箔撕裂边缘
- 用于标题装饰线、CTA 按钮边框、成就标记

---

## 5. Miró 宇宙元素（装饰性）

铸卡中可适当点缀 Miró 视觉语言，增强品牌辨识度：

### 圆点（三种形态）

| 形态 | 描述 | CSS |
|------|------|-----|
| Solid | 实心圆 | `background: currentColor; border-radius: 50%;` |
| Hollow | 空心圆 | `background: transparent; border: 2px solid currentColor; border-radius: 50%;` |
| Bitten | 被咬掉一块 | `clip-path: polygon(0% 0%, 100% 0%, 100% 60%, 70% 60%, 70% 40%, 40% 40%, 40% 100%, 0% 100%);` |

- 尺寸：6px / 10px / 16px / 24px / 36px
- 颜色：Miró Red / Miró Blue / Miró Yellow
- 位置：角落、标题旁、footer 上方，作为星座碎片散落

### 三角形

- 不规则，边长不等
- 方向：up / down / right
- 颜色：Miró Red / Miró Yellow

### 箭头

- 20-30px 长，2px 粗
- 指向虚空，任意角度旋转
- 颜色：Miró Yellow / Miró Red

---

## 6. 暗色模式

暗色模式下，画布变为帝国夜色：

| 变量 | 暗色值 |
|------|--------|
| Canvas | `#0D0D12` |
| Ink | `#F2EDE4` |
| Ink Muted | `#C4BDB0` |
| Surface 1 | `#16161F` |
| Surface 2 | `#1E1E2A` |
| Hairline | `#2A2A2E` |
| Gold Foil | `#D4A82E` |

执行：在 `<body>` 标签上添加 `class="dark"`。

---

## 7. 品牌视觉组件（铸卡强制）

所有铸卡输出必须根据内容语义，强制使用以下品牌视觉组件。组件已预置于各模板 CSS 中，生成 HTML 时直接添加对应 class 即可。

### 7.1 爪印分隔线 `.paw-divider`

**设计**：两侧水平线 + 中央 🐾

```html
<div class="paw-divider"><span class="paw-icon">🐾</span></div>
```

**CSS 参数**：
- 线高：1px
- 线色：`var(--rule)`，opacity 0.4
- 爪印尺寸：`calc(var(--base-text) * 0.78)`
- 爪印色：`var(--accent)`

**使用场景**：章节分隔、卡片呼吸点。**禁止用空行代替**。

---

### 7.2 基里科引用块 `.chirico-quote`

**设计**：灰底 + 锈橙左边框 + 斜体衬线

```html
<blockquote class="chirico-quote">
  <p>引用文本</p>
  <div class="quote-source">—— 来源</div>
</blockquote>
```

**CSS 参数**：
- 背景：`linear-gradient(135deg, rgba(192,192,192,0.12) 0%, transparent 70%)`
- 左边框：4px solid `var(--accent)`
- 文字：italic `var(--font-serif)`，`calc(var(--base-text) * 1.05)`
- 来源：`var(--font-mono)`，`var(--subtitle-size)`

**使用场景**：名人名言、书摘、旁白引用。**禁止用普通 `<blockquote>` 或简单边框引用代替**。

---

### 7.3 帝国紫印章 `.elite-seal`

**设计**：Tyrian Purple `#6B2D5C` 圆角 pill，金色菱形前缀

```html
<span class="elite-seal">ELITE</span>
```

**CSS 参数**：
- 背景：`#6B2D5C`
- 圆角：9999px
- 文字：`#F0F0F0`，uppercase，`var(--font-mono)`，`calc(var(--base-text) * 0.56)`
- 前缀：`◆`，`#C9A227`

**使用场景**：精英、限量、通关、认证标记。**紫色背景上文字为白色或金色**。

---

### 7.4 通关数字标 `.stage-number`

**设计**：锈橙方块编号 01/02/03

```html
<span class="stage-number">01</span>
```

**CSS 参数**：
- 背景：`var(--accent)`
- 圆角：2px
- 文字：`#FFFFFF`，`var(--font-mono)`，`calc(var(--base-text) * 0.72)`
- 尺寸：`calc(var(--base-text) * 1.44)` × `calc(var(--base-text) * 1.44)`

**使用场景**：教程步骤、流程阶段、回数编号。**禁止用纯数字文本代替**。

---

### 7.5 金箔首字下沉 `.dropcap`

**设计**：首字母金箔色衬线体放大下沉

```html
<p class="dropcap">首段正文……</p>
```

**CSS 参数**：
- 首字母字号：`calc(var(--base-text) * 3.56)`
- 行高：0.82
- 颜色：`var(--gold)`
- 浮动：left

**使用场景**：长图、信息图、视觉笔记、多卡首段开篇。**big 模板不适用**。

---

### 7.6 双角色签名 `.dual-signature`

**设计**：兔狲 + 猫熊分栏签名，头像通过 CSS `::before` 自动显示（模板已内嵌 base64 头像数据）

```html
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

**CSS 参数**：
- 布局：flex，gap `calc(var(--base-text) * 0.89)`
- 背景：`linear-gradient(135deg, rgba(232,232,232,0.5) 0%, transparent 60%)`
- 兔狲名色：`var(--accent)`
- 猫熊名色：`var(--miro-blue)`
- 正文：`var(--body-size)`，`var(--font-sans)`，`var(--text-mid)`

**使用场景**：角色对话总结、双角色观点对比、结尾 byline。头像自动渲染在 `.sig-name` 前方，无需手动添加 HTML。

---

### 7.7 Carrara 大理石噪点

**设计**：`body::before` 全屏 SVG fractalNoise 噪点

```css
body::before {
  content: '';
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 9998;
  opacity: 0.025;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 512 512' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='6' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  mix-blend-mode: multiply;
}
```

**使用场景**：long / poster / infograph / comic / sketchnote 模板默认叠加。whiteboard 保持无噪点（白板材质），big 保持和紙专用噪点。

---

### 7.8 组件使用数量要求

| 模具 | 最少品牌组件数 | 推荐组合 |
|------|---------------|---------|
| `-l` 长图 | 3 种 | `.paw-divider` + `.chirico-quote` + `.stage-number` + `.dropcap` |
| `-i` 信息图 | 3 种 | `.paw-divider` + `.elite-seal` + `.stage-number` |
| `-m` 多卡 | 3 种/卡 | `.paw-divider` + `.chirico-quote` + `.stage-number` |
| `-v` 视觉笔记 | 3 种 | `.paw-divider` + `.stage-number` + `.dual-signature` |
| `-c` 漫画 | 2 种 | `.paw-divider` + `.chirico-quote` |
| `-w` 白板 | 2 种 | `.paw-divider` + `.stage-number` |
| `-b` 大字 | 2 种 | `.paw-divider` + `.elite-seal` |

---

## 8. 反 AI 生成清单（品牌特供版）

生成任何视觉内容前，逐项排查：

- [ ] **禁止李继刚签名**：footer 必须是 🐾 + slogan，禁止任何个人署名
- [ ] **禁止 Inter 字体**：用 Noto Serif SC / Noto Sans SC / JetBrains Mono
- [ ] **禁止纯黑**：用 `#1A1A1A` 代替 `#000000`
- [ ] **禁止 AI 紫蓝**：霓虹渐变、紫色光晕一律禁止
- [ ] **禁止居中 Hero**（DESIGN_VARIANCE > 4 时）：用左对齐、分屏、非对称留白
- [ ] **禁止三等分卡片**：用 2 列锯齿、非对称网格
- [ ] **禁止 AI 文案腔**：「赋能」「无缝」「释放」「下一代」禁止
- [ ] **紫色上禁止深色文字**：Tyrian Purple 背景必须用白色或金色文字
- [ ] **金箔必须不规则**：clip-path 多边形，禁止平滑渐变
- [ ] **间距数学精确**：padding 和 margin 不留尴尬间隙
