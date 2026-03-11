# 个人通用 Skills 仓库

> 基于 Anthropic Skills 规范，用于 Cursor/Claude 的个人技能库，可在所有项目中复用。

## 仓库路径

`D:\SMore_gitlab\skills\`

## 快速开始

### 在项目 .cursorrules 中引用

可在各项目的 `.cursorrules` 中添加：

```markdown
## 共享 Skills
技能仓库：`D:\SMore_gitlab\skills\`
- 技能目录：`D:\SMore_gitlab\skills\skills\`
- 技能清单：`D:\SMore_gitlab\skills\SKILLS_MANIFEST.md`
任务涉及文档/设计/测试时，按 SKILLS_MANIFEST 选择对应 skill 并读取其 SKILL.md 执行。
```

## 创建新 Skill

新建 skill 时指定路径到本仓库即可统一管理：

- **目标路径**：`D:\SMore_gitlab\skills\skills\<skill-name>/`
- **示例**：创建 `code-review` 则放到 `skills/skills/code-review/`
- 创建完成后，在 `SKILLS_MANIFEST.md` 中补充该 skill 的说明

在本仓库内操作时，`.cursorrules` 已约定默认创建路径，可直接说「创建一个 xxx 技能」即可。

## 技能清单

详见 [SKILLS_MANIFEST.md](./SKILLS_MANIFEST.md)，包括：

- **文档类**：docx、xlsx、pptx、pdf
- **设计与创意**：frontend-design、canvas-design、theme-factory 等
- **开发与测试**：mcp-builder、webapp-testing、claude-api
- **协作与流程**：internal-comms、doc-coauthoring、skill-creator

## 项目结构

```
skills/
├── skills/            # 所有 skill 模块（符号链接的目标）
│   ├── docx/
│   ├── pdf/
│   └── ...
├── template/          # 新 skill 模板
├── SKILLS_MANIFEST.md # 技能清单与使用说明
├── .cursorrules       # 本仓库规则（含默认创建路径）
└── .ai-errors-log.md  # AI 错误模式记录
```

---

**Skill 基本结构**：每个 skill 为一个文件夹，含 `SKILL.md`（YAML frontmatter + Markdown 指令）。必填字段：`name`、`description`。可使用 `template/` 作为新 skill 的起点。
