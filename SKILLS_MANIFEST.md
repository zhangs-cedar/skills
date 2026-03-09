# Skills 声明文件

本文档列出项目中所有可用的 Skills，供 AI 和开发者参考：何时用、如何用、做什么。

## 使用说明

- **何时用**：当用户请求与某 skill 匹配的任务时，AI 应读取对应 `skills/<name>/SKILL.md` 并遵循其指示
- **如何用**：直接读取 `skills/<skill-name>/SKILL.md`，按其中指令执行
- **优先级**：若任务涉及多种技能，按主要交付物选择（如同时有表格和文档，交付物是 xlsx 则用 xlsx）

---

## 文档类 Skills

| Skill | 路径 | 何时用 | 做什么 |
|-------|------|--------|--------|
| **docx** | `skills/docx/` | 创建、读写、编辑 Word 文档 (.docx)；报告、备忘录、信函、模板 | 生成/修改 Word 文档，含目录、页眉页脚、表格等 |
| **xlsx** | `skills/xlsx/` | 电子表格为主输入或输出；.xlsx/.csv/.tsv 的读写、编辑、公式、图表、数据清洗 | 处理表格文件，金融模型颜色/格式规范 |
| **pptx** | `skills/pptx/` | 任何涉及 .pptx 的操作：创建/编辑演示文稿、提取内容、合并拆分 | PowerPoint 幻灯片的创建与编辑 |
| **pdf** | `skills/pdf/` | 任何涉及 PDF：提取文本/表格、合并/拆分、填表、加密、OCR | PDF 的读取、生成、合并、拆分、表单 |

---

## 设计与创意类

| Skill | 路径 | 何时用 | 做什么 |
|-------|------|--------|--------|
| **frontend-design** | `skills/frontend-design/` | 构建网页组件、页面、应用、仪表盘；美化 Web UI | 高质量、有辨识度的前端界面设计 |
| **web-artifacts-builder** | `skills/web-artifacts-builder/` | 需要 React/Tailwind/shadcn/ui 的复杂多组件 HTML 制品 | 现代前端技术栈的复杂 Web 制品 |
| **canvas-design** | `skills/canvas-design/` | 海报、艺术设计、静态视觉作品 (.png/.pdf) | 静态视觉设计与输出 |
| **algorithmic-art** | `skills/algorithmic-art/` | 代码生成艺术、粒子系统、流动场等算法艺术 | 使用 p5.js 的算法艺术创作 |
| **theme-factory** | `skills/theme-factory/` | 为幻灯片、文档、报告、落地页等应用主题 | 预设/自定义主题的应用 |
| **brand-guidelines** | `skills/brand-guidelines/` | 需要品牌色、字体、视觉规范时 | 应用 Anthropic 品牌规范 |

---

## 开发与测试类

| Skill | 路径 | 何时用 | 做什么 |
|-------|------|--------|--------|
| **mcp-builder** | `skills/mcp-builder/` | 构建 MCP 服务器，集成外部 API/服务 | 用 FastMCP 或 MCP SDK 创建 MCP 服务 |
| **webapp-testing** | `skills/webapp-testing/` | 测试本地 Web 应用、验证功能、调试 UI、截屏 | Playwright 测试与调试 |
| **claude-api** | `skills/claude-api/` | 代码使用 Anthropic SDK 或 Claude API | Claude API 应用开发 |

---

## 协作与流程类

| Skill | 路径 | 何时用 | 做什么 |
|-------|------|--------|--------|
| **internal-comms** | `skills/internal-comms/` | 撰写内部沟通：状态报告、领导层更新、FAQ、事故报告等 | 按公司格式撰写内部沟通 |
| **doc-coauthoring** | `skills/doc-coauthoring/` | 协作文档、提案、技术规格、决策文档 | 结构化文档协作文档流程 |
| **skill-creator** | `skills/skill-creator/` | 创建新 skill、修改/优化已有 skill、评估 skill 表现 | Skill 的创建、编辑与评估 |

---

## 其他

| Skill | 路径 | 何时用 | 做什么 |
|-------|------|--------|--------|
| **slack-gif-creator** | `skills/slack-gif-creator/` | 为 Slack 创建动画 GIF | 生成适配 Slack 的 GIF |
| **template-skill** | `template/` | 创建新 skill 时的模板参考 | 新 skill 的模板 |

---

