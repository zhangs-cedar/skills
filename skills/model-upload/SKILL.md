---
name: model-upload
description: 指导 JH01 项目的模型上传与配置更新流程。当用户上传模型、批量更新多个 Pipeline、更新模型配置、部署新 Pipeline 模型到 GitLab 制品库，或询问如何上传/更新模型时使用。
---

# 模型上传

在 JH01 项目中上传 Pipeline 模型并更新 sdk_settings 的操作指引。

## Quick Start

**何时使用**：新增 Pipeline 后首次上传模型、模型迭代更新、将训练好的 SmartMore/ONNX 部署到 GitLab 制品库。

### 前置检查

`ci_setting.yaml` 的 `product_setting.model_list` 必须包含该 Pipeline。若新增了 Pipeline 类型，先按 [add-inspection-points](.cursor/skills/add-inspection-points/SKILL.md) 完成配置，并确保 Step 6 已把 Pipeline 加入 5 处 model_list（含 ci_setting.yaml）。

### 执行方式

**自动化（推荐）**：提供 pipeline 名称、模型目录、版本后缀后，使用含 sm-encrypt 的 Python（如 sm2.6）执行：

```bash
C:\Users\SMore\.conda\envs\sm2.6\python.exe dist/模型数据上传/模型上传.py -p PipelineMCB -m "C:\deploy\jh01\主板正面大面" -v v1
```

**交互式**：无参数运行，按提示输入流水线编号、模型目录、版本后缀：

```bash
python dist/模型数据上传/模型上传.py
```

## 流程说明

1. **sm2onnx 转换**：若目录下有 `.smartmore`，自动解密并转为 ONNX；否则使用已有 `.onnx`
2. **GitLab 上传**：将 ONNX 上传至 `{gitlab_url}/api/v4/projects/{project_id}/packages/generic/{Pipeline}.onnx/{ver}/`
3. **配置更新**：修改 `project/resources/sdk_settings/{Pipeline}.json` 的 `engine_cfg.model_path` 和 `engine_cfg.md5`
4. **label_map / postprocess_cfg 自动同步**：脚本会检查模型目录内是否存在训练导出 JSON（含 `module_cfg.label_map`），若存在则自动将 `label_map` 与 `postprocess_cfg` 同步到 `Pipeline.json`。同步规则详见 [add-inspection-points reference](.cursor/skills/add-inspection-points/reference.md) 中的「训练导出配置与 Pipeline.json 同步」。

## 批量更新多个 Pipeline

当输入为**父目录**（如 `C:\Users\SMore\Desktop\deploy\jh01`）且内含多个模型子目录时：

1. **查看目录结构**：`tree` 或 `ls` 列出子目录
2. **按映射执行**：根据目录名匹配 Pipeline（支架反面→STB、支架正面→STF、主板反面大面→MBB、主板正面大面→MBF），逐条执行上传
3. **统一版本**：同一批次使用相同版本后缀（如 v3）

详见 [reference.md](reference.md) 中的「文件夹与 Pipeline 映射」和「批量更新流程」。

## 与新 Pipeline 的联动

若为新增 Pipeline（如 PipelineMCB），需先完成 add-inspection-points 的 Step 1–6，再将 Pipeline 加入 `model_list`，方可被本脚本识别并上传。
