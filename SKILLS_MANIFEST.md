# Skills 声明文件

<skills_system priority="1">

## 使用说明

当用户要求执行任务时，检查以下可用技能是否能更有效地完成任务。技能提供专业化的能力和领域知识。

**如何使用技能：**
- 读取：`skills/<skill-name>/SKILL.md`（按路径加载完整指令）
  - 多技能：依次读取 `skills/skill-one/SKILL.md`、`skills/skill-two/SKILL.md`
- 技能目录用于解析捆绑资源：`references/`、`scripts/`、`assets/`

**注意事项：**
- 仅使用下方列出的技能
- 不要重复加载已 in-context 的技能
- 若任务涉及多种技能，按主要交付物选择（如表格+文档且交付物为 xlsx → 用 xlsx）

---

## 可用技能

### 文档类

| name | description |
|------|-------------|
| docx | 创建、读写、编辑 Word 文档 (.docx)；报告、备忘录、信函、模板。生成/修改 Word 文档，含目录、页眉页脚、表格等 |
| xlsx | 电子表格为主输入或输出；.xlsx/.csv/.tsv 的读写、编辑、公式、图表、数据清洗。处理表格文件，金融模型颜色/格式规范 |
| pptx | 任何涉及 .pptx 的操作：创建/编辑演示文稿、提取内容、合并拆分。PowerPoint 幻灯片的创建与编辑 |
| pdf | 任何涉及 PDF：提取文本/表格、合并/拆分、填表、加密、OCR。PDF 的读取、生成、合并、拆分、表单 |

### 设计与创意类

| name | description |
|------|-------------|
| frontend-design | 构建网页组件、页面、应用、仪表盘；美化 Web UI。高质量、有辨识度的前端界面设计 |
| web-artifacts-builder | 需要 React/Tailwind/shadcn/ui 的复杂多组件 HTML 制品。现代前端技术栈的复杂 Web 制品 |
| canvas-design | 海报、艺术设计、静态视觉作品 (.png/.pdf)。静态视觉设计与输出 |
| algorithmic-art | 代码生成艺术、粒子系统、流动场等算法艺术。使用 p5.js 的算法艺术创作 |
| theme-factory | 为幻灯片、文档、报告、落地页等应用主题。预设/自定义主题的应用 |
| brand-guidelines | 需要品牌色、字体、视觉规范时。应用 Anthropic 品牌规范 |

### 开发与测试类

| name | description |
|------|-------------|
| mcp-builder | 创建高质量 MCP 服务器，集成外部 API/服务。支持 Python（FastMCP）或 Node/TypeScript（MCP SDK） |
| webapp-testing | 测试本地 Web 应用、验证功能、调试 UI、截屏。Playwright 测试与调试 |
| claude-api | 代码使用 Anthropic SDK 或 Claude API。Claude API 应用开发 |

### 协作与流程类

| name | description |
|------|-------------|
| internal-comms | 撰写内部沟通：状态报告、领导层更新、FAQ、事故报告等。按公司格式撰写内部沟通 |
| doc-coauthoring | 协作文档、提案、技术规格、决策文档。结构化文档协作文档流程 |
| skill-creator | 创建新 skill、修改/优化已有 skill、评估 skill 表现。Skill 的创建、编辑与评估 |

### 其他

| name | description |
|------|-------------|
| slack-gif-creator | 为 Slack 创建动画 GIF。生成适配 Slack 的 GIF |
| template-skill | 创建新 skill 时的模板参考。路径：`template/` |

</skills_system>

---
