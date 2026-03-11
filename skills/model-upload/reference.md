# 模型上传 - 参考

## 文件夹与 Pipeline 映射

JH01 项目模型目录名与 Pipeline 的对应关系（目录名可带 `_正式`、`_未分组` 等后缀）：

| 目录名包含 | Pipeline | 说明 |
|-----------|----------|------|
| 支架反面 | PipelineSTB | 支架反面 |
| 支架正面 | PipelineSTF | 支架正面 |
| 支架螺纹柱 | PipelineSTZ | 支架螺纹柱 |
| 主板反面大面 | PipelineMBB | 主板反面大面 |
| 主板正面大面 | PipelineMBF | 主板正面大面 |
| 主板单视角 | PipelineMBZ | 主板单视角 |

示例：`支架正面_正式` → PipelineSTF，`主板反面大面_未分组` → PipelineMBB。

## 批量更新流程

当输入路径为**父目录**（如 `C:\Users\SMore\Desktop\deploy\jh01`）且内含多个模型子目录时：

1. **列出子目录**：`tree` 或 `ls` 查看结构
2. **智能映射**：根据目录名包含的关键词（支架反面、支架正面、主板反面大面、主板正面大面等）匹配 Pipeline
3. **依次上传**：对每个子目录执行 `模型上传.py -p <Pipeline> -m <子目录完整路径> -v <版本>`
4. **环境**：使用 `C:\Users\SMore\.conda\envs\sm2.6\python.exe`（含 sm-encrypt）

**批量命令示例**（版本 v3）：

```powershell
$base = "C:\Users\SMore\Desktop\deploy\jh01"
$py = "C:\Users\SMore\.conda\envs\sm2.6\python.exe"
$script = "d:\SMore_gitlab\jh01\dist\模型数据上传\模型上传.py"
cd d:\SMore_gitlab\jh01

# STB, STF, MBB, MBF
& $py $script -p PipelineSTB -m "$base\支架反面_正式" -v v3
& $py $script -p PipelineSTF -m "$base\支架正面_正式" -v v3
& $py $script -p PipelineMBB -m "$base\主板反面大面_未分组" -v v3
& $py $script -p PipelineMBF -m "$base\主板正面大面_未分组" -v v3
```

## 目录结构示例

```
jh01/
├── 主板反面大面_未分组/   → PipelineMBB
│   ├── 60000.smartmore
│   ├── *.json (训练配置，用于 label_map 同步)
│   └── Pipeline/3.0/smartmore.onnx
├── 主板正面大面_未分组/   → PipelineMBF
├── 支架反面_正式/         → PipelineSTB
└── 支架正面_正式/         → PipelineSTF
```
