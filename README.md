# 个人通用 Skills 仓库

> 基于 Anthropic Skills 规范，用于 Cursor/Claude 的个人技能库，可在所有项目中复用。


## 在项目中使用

### 子模块（推荐）

以 Git 子模块引入，每个项目显式声明依赖，随项目版本管理，适合协作。

**在需要 skills 的项目根目录执行：**

```bash
git submodule add <本仓库URL> .cursor/skills
```

示例（替换为实际 GitLab 地址）：

```bash
git submodule add https://gitlab.com/xxx/skills.git .cursor/skills
```

**克隆含子模块的项目后需初始化：**

```bash
git clone <项目URL>
cd <项目目录>
git submodule update --init --recursive
```

**更新 skills 到最新：**

```bash
cd .cursor/skills && git pull origin main
```

**目录结构：**

```
your-project/
  .cursor/
    skills/           # 子模块，指向本仓库
      docx/
      canvas-design/
      ...
```



## 项目结构

```
skills/
├── docx/              # skill 模块（含 SKILL.md）
├── pdf/
├── canvas-design/
├── ...
├── template/           # 新 skill 模板
├── SKILLS_MANIFEST.md  # 技能清单与使用说明
├── .cursorrules        # 本仓库规则（含默认创建路径）
├── link-global-skills.ps1  # 全局符号链接脚本（方式三）
└── README.md
```

**Skill 基本结构**：每个 skill 为一个文件夹，含 `SKILL.md`（YAML frontmatter + Markdown 指令）。必填字段：`name`、`description`。可使用 `template/` 作为新 skill 的起点。
