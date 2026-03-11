---
name: add-inspection-points
description: Guides adding new inspection points (新增点位) to the JH01 project. Use when the user wants to add new points, download points, or expand inspection configurations. Covers both new Pipeline types and adding points to existing Pipelines.
---

# 新增点位

在 JH01 项目中新增检测点位时的操作指引。根据 commit 6178bfcc 和 ea28d14 的实践总结。

## Quick Start：场景判断

先确定属于哪种场景：

| 场景 | 条件 | 修改范围 |
|------|------|----------|
| **A. 新增 Pipeline 类型** | 下载的点位使用新的 `pipeline` 名称（如 PipelineXXX，当前项目中不存在） | 完整流程：新建 6 类文件 + 修改 5 处 model_list + __init__.py |
| **B. 仅新增点位** | 点位使用已有 pipeline（如 PipelineSTF、PipelineMBB 等） | 仅更新 project_mapping |

---

## 场景 A：新增 Pipeline 类型

按以下 checklist 逐步完成：

```
Task Progress:
- [ ] Step 1: 新建 Pipeline{XXX}.py
- [ ] Step 2: 新建 project/resources/sdk_settings/Pipeline{XXX}.json
- [ ] Step 3: 新建 project/resources/ers/ 下 3 个配置文件
- [ ] Step 4: 更新 project/process_pipeline/__init__.py
- [ ] Step 5: 在 project_mapping 中追加点位
- [ ] Step 6: 在 5 处 model_list 中追加 Pipeline{XXX}
- [ ] Step 7: 下载模型后更新 sdk_settings 中的 model_path 和 md5
```

### Step 1: 新建 Pipeline Python 类

路径：`project/process_pipeline/Pipeline{XXX}.py`  
模板见 [reference.md](reference.md#pipeline-py-模板)。

### Step 2: 新建 SDK 配置

路径：`project/resources/sdk_settings/Pipeline{XXX}.json`  
- 多图（5 张：同轴 + 条光1–4）→ `xrack_module.type`: `MultiViewMultiTaskSegModel`  
- 单图 → `xrack_module.type`: `MultiTaskSegModel`  
模板见 [reference.md](reference.md#sdk-settings-json-模板)。

### Step 3: 新建 ERS 配置文件（3 个）

| 文件 | 说明 |
|------|------|
| `project/resources/ers/Pipeline{XXX}_custom_module.json` | 将 `MBB` 替换为 `{XXX}`，调整 `template_path`、`common_ers_config_path`、`special_ers_config_path` |
| `project/resources/ers/Pipeline{XXX}.json` | `{"foo": {"area": null, "length": null, "number": null}}` |
| `project/resources/ers/Pipeline{XXX}_Special.json` | 可直接复用 PipelineMBB_Special.json 内容 |

模板见 [reference.md](reference.md#ers-配置文件模板)。

### Step 4: 更新 __init__.py

在 `project/process_pipeline/__init__.py` 末尾添加：

```python
from .Pipeline{XXX} import *
```

### Step 5: 追加 project_mapping 点位

路径：`project/resources/exposed_settings/project_mapping_GTK-JH01.json`  
格式见 [reference.md](reference.md#project_mapping-点位格式)。

### Step 6: 更新 model_list（5 处）

在以下文件中的 `model_list` 或 `MODELS` 中追加 `Pipeline{XXX}`：

| 文件 | 位置 |
|------|------|
| `ci_setting.yaml` | `product_setting.model_list` |
| `offline_entrance.py` | `model_list = [...]` |
| `pb_dnn_model.py` | `model_list = [...]` |
| `project/distributer/manager.py` | `self.model_list = [...]` |
| `scripts/pre-push` | `MODELS=(...)` |

### Step 7: 下载模型后更新

从制品库下载对应模型后，在 `project/resources/sdk_settings/Pipeline{XXX}.json` 中更新：
- `engine_cfg.model_path`
- `engine_cfg.md5`

---

## 场景 B：仅新增点位

只需在 `project/resources/exposed_settings/project_mapping_GTK-JH01.json` 中追加点位条目：

```json
"点位名称": {
    "images": ["图1", "图2", ...],
    "pipeline": "PipelineXXX",
    "roi": [0, 0, 4096, 3000],
    "image_number": N
}
```

注意上一项末尾加逗号，最后一项不加逗号。

---

## 参考

- 详细模板和完整示例见 [reference.md](reference.md)
- 参考 commit：6178bfcc、ea28d14
