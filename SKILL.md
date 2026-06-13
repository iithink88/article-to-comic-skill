---
name: article-to-comic
version: 1.0.0
description: One-click conversion of articles, stories, or narratives into illustrated comics with consistent characters. Use when the user wants to convert text into comics, create manga from articles, illustrate stories, generate comic panels, or make visual narratives from written content.
description_zh: 一键将文章、故事或叙事文本转换为漫画，保证角色形象一致性。当用户想把文章转成漫画、为故事配插图、生成漫画面板、或制作视觉叙事内容时使用。
---

# 文章转漫画 (Article-to-Comic)

将文章/故事一键转化为具有角色一致性的漫画面板，输出为精美 HTML 漫画页面。

## 核心优势

- **角色一致性**: 通过"角色档案 + 全文注入"机制，确保同一角色在所有面板中外观统一
- **智能分镜**: AI 自动提取最有画面感的关键场景
- **一站式输出**: 最终生成自包含的 HTML 文件，浏览器直接打开即可查看

## 完整工作流程

### Step 1: 文章分析与场景提取

1. 通读全文，理解故事主题、情感基调和叙事节奏
2. 识别所有出场角色（人物、拟人化动物、重要实体）
3. 智能提取 6-12 个关键场景（按文章长度调整）：
   - 短篇（<1000字）→ 4-6 个面板
   - 中篇（1000-3000字）→ 6-10 个面板
   - 长篇（>3000字）→ 10-16 个面板，可拆分为多个章节
4. 选取场景的优先级：情感高潮 > 对话互动 > 动作场景 > 环境描写

每个场景需记录：

| 字段 | 说明 |
|------|------|
| 场景编号 | 顺序编号 |
| 场景描述 | 画面内容的视觉描述（用于生成图片） |
| 出场角色 | 该面板中出现的角色列表 |
| 对话/旁白 | 需要叠加在画面上的文字（区分对话和旁白） |
| 情绪氛围 | 该场景的情感关键词（温馨、紧张、悲伤等） |
| 镜头建议 | 远景/中景/近景/特写 |

### Step 2: 角色档案建立（一致性核心）

这是保证角色一致性的**最关键步骤**。为每个角色建立详细的外观档案：

```
角色档案格式：
- 名称: [角色名]
- 年龄外观: [大致年龄段]
- 性别: [男/女/其他]
- 发型发色: [具体描述，如"黑色齐肩短发，微卷"]
- 面部特征: [具体描述，如"圆脸，弯弯的笑眼，小巧的鼻子"]
- 体型: [具体描述，如"纤细中等身材"]
- 标志性服装: [具体描述，如"米白色高领毛衣，深蓝牛仔裤"]
- 标志性配饰: [如有，如"红色围巾，银色手链"]
- 气质关键词: [如"温柔文静", "活泼开朗", "沉稳冷酷"]
```

**建档原则：**
- 描述越具体，一致性越好。"黑色短发"比"深色头发"好
- 必须包含**发型发色、服装、配饰**三个核心识别要素
- 服装在故事中保持一致（除非剧情需要换装）
- 从文章描写中提取特征；未提及的部分，合理创作并确保风格协调

### Step 3: 向用户确认（可选但推荐）

在开始生成图片前，将以下信息展示给用户确认：
- 角色档案列表（让用户核实或修改角色外观设定）
- 场景分镜列表（让用户核实或增删场景）
- 画风确认（默认使用温馨插画风，可根据用户偏好调整）

如果用户表示"直接生成"或"不用确认"，可跳过此步。

### Step 4: 图片生成

对每个场景面板调用 ImageGen 工具生成图片。

#### 提示词构造公式

每个面板的 prompt 严格按以下结构拼接：

```
[风格前缀] + [场景环境描述] + [角色A完整外观] + [角色B完整外观（如有）] + [动作/表情/互动] + [镜头/构图] + [光线/氛围]
```

#### 风格前缀（每个面板必须包含，一字不差）

**温馨插画风（默认）：**
```
Soft watercolor illustration style, warm pastel color palette, gentle lighting, cozy atmosphere, children's book illustration quality, rounded linework, dreamy and heartwarming mood
```

**其他可选风格前缀：**

日式动漫风：
```
Japanese anime manga style, clean linework, vibrant colors, expressive large eyes, dynamic composition, manga panel aesthetic
```

美式漫画风：
```
American comic book style, bold ink outlines, dynamic action poses, vivid saturated colors, halftone dot shading, graphic novel quality
```

赛博朋克风：
```
Cyberpunk illustration style, neon glow lighting, dark atmospheric tones, futuristic cityscape, holographic elements, high contrast digital art
```

#### 角色描述注入规则

**每个出场角色的完整外观描述必须在每个 prompt 中完整出现**，不可省略或简化。

示例 — 角色"小雪"在所有面板中的描述保持一致：
```
a young woman in her early 20s with shoulder-length wavy black hair with straight bangs, gentle almond-shaped brown eyes, slim build, wearing a cream-colored turtleneck sweater and light blue jeans, with a soft red scarf around her neck
```

即使该面板只描述她的侧脸或背影，也要包含上述完整描述以维持一致性。

#### 图片尺寸选择

| 场景类型 | 推荐尺寸 | 适用说明 |
|----------|----------|----------|
| 标准叙事面板 | 1792x1024 (16:9) | 默认选择，适合大多数场景 |
| 竖版特写 | 1024x1536 (2:3) | 角色情绪特写、单人场景 |
| 正方形 | 1024x1024 (1:1) | 四格漫画、日常片段 |
| 宽幅全景 | 2560x1080 (21:9) | 开篇环境、结尾远景 |

建议以 1792x1024 作为默认尺寸，特殊场景（如角色特写、开篇全景）可调整。

#### 生成节奏

- 使用 Task 工具并行生成多个面板（推荐每批 3-4 张）以提高效率
- 每张图片生成后，记录文件路径，供后续 HTML 组装使用

### Step 5: HTML 漫画页面组装

所有面板图片生成完毕后，组装为一个自包含的 HTML 文件。

#### 图片嵌入方式

将所有生成的图片转为 Base64 编码，直接嵌入 HTML 的 `<img>` 标签中，确保单个 HTML 文件即可完整展示。

#### HTML 结构

详细的 HTML 模板参见 [comic-template.html](comic-template.html)。核心结构如下：

```
HTML 文档
├── <style> 漫画样式（面板网格、气泡、字体、动画）
├── <div class="comic-container">
│   ├── <header> 标题 + 副标题
│   ├── <div class="chapter"> （可选：多章节时使用）
│   │   └── <div class="panel"> × N
│   │       ├── <img> 面板图片
│   │       ├── <div class="narration"> 旁白文字
│   │       └── <div class="speech-bubble"> 对话气泡
│   └── <footer> 结语
└── （结束）
```

#### 面板布局规则

- 默认 2 列网格布局（桌面端），移动端自动切换为单列
- 重要场景（高潮、转折）使用 `full-width` 横跨整行
- 面板之间保留 12px 间距，模拟漫画格子效果
- 面板圆角 8px + 微弱阴影，现代感与漫画感兼顾

#### 文字叠加规则

- **旁白**: 半透明底部横条，白色文字，用于场景描述和内心独白
- **对话气泡**: 白色圆角气泡 + 小三角指示器，定位在说话角色附近
- **情绪文字**: 大号斜体，用于强调情感高潮

### Step 6: 输出与交付

1. 将 HTML 文件保存到用户的工作目录或输出目录
2. 使用 `present_files` 工具展示给用户
3. 告知用户可直接用浏览器打开 HTML 文件查看漫画

## 提示词质量提升技巧

### 让角色更一致

1. **固定描述顺序**: 每次描述同一角色时，按相同顺序列出特征（发型→发色→眼睛→体型→服装→配饰）
2. **避免模糊词汇**: 不用"similar"、"same outfit"等，每次都写完整描述
3. **环境不影响角色**: 光照、天气等环境描述不要改变角色本身的颜色描述

### 让画面更好看

1. prompt 末尾添加质量词：`highly detailed, professional illustration, beautiful composition`
2. 用具体的光线描述代替泛泛的描述：`golden hour sunlight streaming through window` 优于 `nice lighting`
3. 加入构图关键词：`rule of thirds, foreground-background depth, cinematic framing`

### 避免的常见错误

- 不要在 prompt 中放入对话文字（文字由 HTML 叠加，不要烧进图片里）
- 不要用否定描述（如"no hat"），ImageGen 对此处理不稳定
- 不要在一个面板中塞入超过 3 个角色，画面会过于拥挤
- 长文章不要试图覆盖每一个段落，只选最有视觉冲击力的场景

## 完整示例

详细的输入输出示例参见 [examples.md](examples.md)。
