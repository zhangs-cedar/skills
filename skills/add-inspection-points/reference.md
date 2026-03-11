# 新增点位 - 参考模板

将 `{XXX}` 替换为实际的 Pipeline 名称（如 MBB、MBF、MBZ）。

---

## Pipeline.py 模板

**路径**：`project/process_pipeline/Pipeline{XXX}.py`

```python
from smore_xrack.pipeline.pipeline_builder import PIPELINE

from .pipline_base import ProjectPipeline


@PIPELINE.register_module()
class Pipeline{XXX}(ProjectPipeline):
    pass
```

---

## SDK Settings JSON 模板

**路径**：`project/resources/sdk_settings/Pipeline{XXX}.json`

### 多图（5 张：同轴 + 条光1-4）— 使用 MultiViewMultiTaskSegModel

```json
{
  "type": "Pipeline{XXX}",
  "xrack_cfg": {
    "factory_config_path": "project/resources/sdk_settings/_Factory.json",
    "custom_module_config_paths": [
      "project/resources/ers/Pipeline{XXX}_custom_module.json"
    ],
    "label_map": [
      ["foo"],
      ["wuliao"]
    ],
    "xrack_module": {
      "type": "MultiViewMultiTaskSegModel",
      "preprocess_cfg": [
        {"type": "Resize", "output_size": [2048, 1500]},
        {"type": "Normalize", "mean": [0.5, 0.5, 0.5], "std": [0.5, 0.5, 0.5], "scale": [1.0, 1.0, 1.0]}
      ],
      "engine_cfg": {
        "type": "Onnx2TensorRTEngine",
        "model_path": "project/resources/models/Pipeline{XXX}_版本.onnx",
        "md5": "下载后填写"
      },
      "output_cfg": {
        "output_contours": true,
        "output_mask": false,
        "output_contours_area": true,
        "output_score_map": false,
        "output_rects": true
      },
      "postprocess_cfg": [
        [{"type": "FilterMask", "area_thresholds": [10000], "length_thresholds": [100]}],
        [{"type": "FilterMask", "area_thresholds": [100000], "length_thresholds": [320]}]
      ]
    }
  },
  "__comment__": "新增类别需要1. exp.yaml 增加num_class, class_weights, label_map, category_map; 2. Pipeline.json 增加label_map, postprocess_cfg"
}
```

### 单图 — 使用 MultiTaskSegModel

```json
{
  "type": "Pipeline{XXX}",
  "xrack_cfg": {
    "factory_config_path": "project/resources/sdk_settings/_Factory.json",
    "custom_module_config_paths": [
      "project/resources/ers/Pipeline{XXX}_custom_module.json"
    ],
    "label_map": [
      ["foo"],
      ["wuliao"]
    ],
    "xrack_module": {
      "type": "MultiTaskSegModel",
      "preprocess_cfg": [
        {"type": "Resize", "output_size": [2048, 1500]},
        {"type": "Normalize", "mean": [0.5, 0.5, 0.5], "std": [0.5, 0.5, 0.5], "scale": [1.0, 1.0, 1.0]}
      ],
      "engine_cfg": {
        "type": "Onnx2TensorRTEngine",
        "model_path": "project/resources/models/Pipeline{XXX}_版本.onnx",
        "md5": "下载后填写"
      },
      "output_cfg": {
        "output_contours": true,
        "output_mask": false,
        "output_contours_area": true,
        "output_score_map": false,
        "output_rects": true
      },
      "postprocess_cfg": [
        [{"type": "FilterMask", "area_thresholds": [10000], "length_thresholds": [100]}],
        [{"type": "FilterMask", "area_thresholds": [10000], "length_thresholds": [100]}]
      ]
    }
  },
  "__comment__": "新增类别需要1. exp.yaml 增加num_class, class_weights, label_map, category_map; 2. Pipeline.json 增加label_map, postprocess_cfg"
}
```

---

## ERS 配置文件模板

### custom_module.json

**路径**：`project/resources/ers/Pipeline{XXX}_custom_module.json`

```json
{
  "resolution": 0.01,
  "defect_drawer_cfg": {},
  "MappingProcessor": {
    "enabled": false,
    "config": {
      "template_path": "project/resources/ers/template{XXX}"
    }
  },
  "defect_info_analyser_cfg": {
    "enabled": true,
    "__doc__": {
      "enabled": "是否启用缺陷信息分析器",
      "dilate_kernel_size": "膨胀核大小",
      "normalize_value": "归一化范围",
      "offset": "计算区域的边界偏移量",
      "labels": "特殊缺陷的标签,用于计算缺陷的hue_percentage",
      "cal_mode": "特殊缺陷的计算模式可选值:  normalized_mean, mean, max, normalized_max"
    },
    "config": {
      "dilate_kernel_size": 7,
      "normalize_value": [-20, 0],
      "offset": 10,
      "cal_mode": "normalized_mean",
      "labels": ["foo"]
    }
  },
  "SpecificDefectRegionFilter": {
    "enabled": false,
    "__doc__": {
      "enabled": "是否启用特定缺陷区域过滤器",
      "region_labels": "Region区域名称",
      "region_name": "Region区域名称",
      "defect_labels": "在Region区域需要检测的缺陷类型"
    },
    "config": {
      "支架反面1": [
        {
          "region_labels": ["foo"],
          "region_name": "wuliao",
          "defect_labels": ["foo"]
        }
      ]
    }
  },
  "DefectFilter": {
    "enabled": false,
    "__doc__": {
      "enabled": "是否启用缺陷过滤器",
      "labels": "过滤缺陷标签，前端不渲染该缺陷"
    },
    "config": {
      "labels": ["foo"]
    }
  },
  "CommonDefectChecker": {
    "enabled": true,
    "__doc__": {
      "enabled": "是否启用普通缺陷检查器",
      "common_ers_config_path": "普通缺陷的配置文件路径"
    },
    "config": {
      "common_ers_config_path": "project/resources/ers/Pipeline{XXX}.json"
    }
  },
  "SpecialDefectChecker": {
    "enabled": true,
    "__doc__": {
      "enabled": "是否启用特殊缺陷检查器",
      "mask_scale": "掩码缩放比例",
      "special_ers_config_path": "特殊缺陷的配置文件路径"
    },
    "config": {
      "mask_scale": 0.25,
      "special_ers_config_path": "project/resources/ers/Pipeline{XXX}_Special.json"
    }
  },
  "summary_doc": {
    "description": "插件执行顺序",
    "order": [
      "MappingProcessor",
      "defect_info_analyser",
      "SpecificDefectRegionFilter",
      "CommonDefectChecker",
      "SpecialDefectChecker"
    ]
  }
}
```

### 普通缺陷配置 (Pipeline{XXX}.json)

**路径**：`project/resources/ers/Pipeline{XXX}.json`

```json
{
  "foo": {
    "area": null,
    "length": null,
    "number": null
  }
}
```

### 特殊缺陷配置 (_Special.json)

**路径**：`project/resources/ers/Pipeline{XXX}_Special.json`

```json
{
  "__doc__": {
    "description": "特殊缺陷检查器配置",
    "format": "每个缺陷类型支持多个配方 [{}, {}]，任一配方不满足即NG,所有参数均有默认值(可不配置)",
    "fields": {
      "hue_percentage_endpoints": "灰度区间 [min, max]",
      "area_threshold": "面积区间 [min, max]（平方毫米）",
      "length_threshold": "长度区间 [min, max]（毫米）",
      "surrounding_block_size": "周围区域大小（像素）",
      "surrounding_max_num": "周围数量>=此值则NG",
      "surrounding_area_sum": "周围面积和>=此值则NG（平方毫米）"
    },
    "logic": "筛选同时满足灰度区间和面积区间和长度区间的缺陷，计算周围密度和面积和，任一超限即NG，任一配方不满足即NG"
  },
  "temp": [
    {
      "hue_percentage_endpoints": [0.17, 0.25],
      "area_threshold": [0.004, 0.01],
      "surrounding_block_size": 300,
      "surrounding_max_num": 2
    }
  ]
}
```

---

## project_mapping 点位格式

**路径**：`project/resources/exposed_settings/project_mapping_GTK-JH01.json`

### 多图点位（5 张：同轴 + 条光1-4）

```json
"主板反面大面1": {
    "images": ["反面大面1_同轴", "反面大面1_条光1", "反面大面1_条光2", "反面大面1_条光3", "反面大面1_条光4"],
    "pipeline": "PipelineMBB",
    "roi": [0, 0, 4096, 3000],
    "image_number": 5
}
```

### 单图点位

```json
"主板反面短边内壁1_1": {
    "images": ["反面短边内壁1_1"],
    "pipeline": "PipelineMBZ",
    "roi": [0, 0, 4096, 3000],
    "image_number": 1
}
```

### 字段说明

| 字段 | 说明 |
|------|------|
| `images` | 图片名称列表，与采图命名对应 |
| `pipeline` | 使用的 Pipeline 类型 |
| `roi` | 检测区域 [x, y, width, height] |
| `image_number` | 图片数量，多图通常为 5，单图为 1 |

---

## 训练导出配置与 Pipeline.json 同步

训练目录内的 `{xxx}_正式.json` 为模型导出配置，与 `project/resources/sdk_settings/Pipeline{XXX}.json` 对应（如 支架反面_正式 ↔ PipelineSTB、主板反面大面_正式 ↔ PipelineMBB）。

**同步规则**：

1. **label_map**：从训练配置 `module_cfg.label_map` 直接复制到 Pipeline.json 的 `xrack_cfg.label_map`。
2. **postprocess_cfg**：训练导出通常为 `[]`，部署时需补齐。
   - `label_map[0]` 有 N 个缺陷类时，第一个 FilterMask 的 `area_thresholds`、`length_thresholds` 各需 N 个元素，与类别一一对应。
   - 常用默认值：`area_thresholds: [15,15,...]`，`length_thresholds: [2,2,...]`。
3. **wuliao 类别**：第二个 FilterMask 一般为 `area_thresholds: [100000]`，`length_thresholds: [320]`。
