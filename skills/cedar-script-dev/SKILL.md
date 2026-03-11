---
name: cedar-script-dev
description: 专门用于指导和生成当前项目 Python 脚本。提供项目专属的代码结构、main.py 模板、文件循环容错模板、代码编写规则、cedar 库常用 API 速查及 form.yaml 配置规范。当需要创建新 Python 脚本、实现循环处理、使用 cedar 库或搭建新的任务入口时使用此技能。
---

# Cedar Script Development 规范与模板

该技能整合了项目中编写 Python 脚本所需的所有核心模板、最佳实践和库用法。在生成或修改本项目脚本时请遵循以下规范。

## 脚本目录结构

一个标准的数据处理或分析脚本目录应包含：

```
脚本目录/
├── main.py       # 主入口（必需）
├── utils.py      # 工具函数（用于较复杂的脚本）
├── form.yaml     # 界面表单/参数配置定义
├── config.json   # 调试用的参数配置（不提交到 git）
└── README.md     # 简要使用说明
```

---

## 模板 1: `main.py` 标准入口

标准入口应当实现：基于 `__file__` 自动寻找并初始化配置；将 `input_dir` 和 `output_dir` 解耦；主逻辑封入 `run_task`，并使用 `@try_except` 装饰器对 `main` 进行全局异常捕获。

```python
import os, sys
import os.path as osp
from cedar.utils import init_cfg, print, try_except

sys.path.append(osp.dirname(osp.abspath(__file__)))


def run_task(cfg: dict):
    """核心业务逻辑"""
    input_dir = cfg['input_dir']
    output_dir = cfg['output_dir']
    os.makedirs(output_dir, exist_ok=True)
    
    # ... 具体业务代码 ...


@try_except
def main(config_file_path: str = None):
    run_task(init_cfg(config_file_path, __file__))


if __name__ == '__main__':
    main(sys.argv[1] if len(sys.argv) > 1 else None)
```

---

## 模板 2: 安全的文件循环处理

在进行大规模文件遍历时，必须要有健壮的容错机制，防止由于某张图片或某个文件异常导致整个处理流程挂掉。

```python
import traceback

# 获取需要处理的文件列表
files = get_files_list(input_dir, find_suffix=['.json', '.png'])
total = len(files)
processed, skipped = 0, 0

for i, file in enumerate(files, 1):
    try:
        name = file['name']
        
        # ... 核心判断与操作 ...
        if 条件判断_不符合要求:
            skipped += 1
            print(f'[{i}/{total}] {name} -> 跳过(原因: xxx)')
            continue
            
        processed += 1
        print(f'[{i}/{total}] {name} -> 成功')
        
    except Exception:
        skipped += 1
        print(f'[{i}/{total}] {file["name"]} -> 错误:\n{traceback.format_exc()}')

print(f'任务完成: 成功处理 {processed} 个, 失败/跳过 {skipped} 个')
```

---

## 项目代码规则

- **模块导入**：
  - 标准库一行导入多模块：`import os, sys, json`
  - 路径库必须使用：`import os.path as osp`
  - 内部工具库从 `cedar` 按需导入。
- **命名规范**：
  - 遵循 Python `snake_case`（小写带下划线）。
  - 目录变量必须以后缀结尾：`_dir`
  - 路径变量必须以后缀结尾：`_path`
  - 文件变量必须以后缀结尾：`_file`
  - 常用简写建议：`fn` (function/filename), `sp` (save path / string path), `cfg` (config), `im` (image)。
- **函数设计**：
  - 控制规模在 **20-50** 行内，逻辑切分清晰。
  - 读取字典配置项时，强制使用 `.get()` 防止键缺失报错：`cfg.get('key', default)`。
  - 采用 **"早返回 (Early Return)"** 模式减少嵌套深度。
  - 使用中文输出关键日志。
  - 函数签名下方保留一行 Docstring 注释。

---

## `cedar` 库常用速查

项目中提供了一套常用且健壮的工具方法。请优先使用这些 API，避免重复造轮子。

```python
init_cfg(path, __file__)        # 初始化并解析配置（关联 form.yaml / config.json）
load_config(path)               # 加载并解析 json / yaml 文件
write_config(data, path)        # 将 dict 写入 json / yaml 文件
get_files_list(dir, ['.png'])   # 安全获取目录下特定后缀的文件列表
create_name()                   # 基于时间戳生成唯一名称
imread(path)                    # 安全读取图像
imwrite(p, img)                 # 安全写入图像
@try_except                     # 全局异常捕获与日志记录装饰器
print(msg)                      # 增强版 print（支持自动写入日志系统）
```

---

## `form.yaml` 参数配置规范

`form.yaml` 用于定义参数并在可视化界面中渲染。支持的字段类型包含：
`text`, `multiline`, `int`, `float`, `bool`, `select`, `file`, `dir`, `date`, `doc`

一个典型的目录输入定义如下：
```yaml
- name: input_dir
  label: 输入目录
  type: dir
  default: ""
  required: true
  description: 待处理图片所在目录
```
