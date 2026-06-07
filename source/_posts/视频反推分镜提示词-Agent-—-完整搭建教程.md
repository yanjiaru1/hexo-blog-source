---

---



title: 从视频到脚本：手把手搭建 AI 分镜提示词 Agent（Dify + 飞书）
date: 2026-06-07 20:59:08
tags:
    -dify
    -飞书表格



# 从视频到脚本：手把手搭建 AI 分镜提示词 Agent（Dify + 飞书）

> 本教程基于 Dify 工作流，实现上传视频 → 提取关键帧 → AI 生成分镜脚本 → 自动写入飞书电子表格的全流程自动化。

---

## 目录

1. [整体流程概览](#1-整体流程概览)
2. [前置准备](#2-前置准备)
3. [节点详细配置](#3-节点详细配置)
   - [节点 1：开始](#节点-1开始)
   - [节点 2：提取视频关键帧](#节点-2提取视频关键帧)
   - [节点 3：解析并内嵌图片](#节点-3解析并内嵌图片)
   - [节点 4：生成分镜脚本（LLM）](#节点-4生成分镜脚本llm)
   - [节点 5：获取飞书 Access Token](#节点-5获取飞书-access-token)
   - [节点 6：提取 Access Token](#节点-6提取-access-token)
   - [节点 7：新建飞书电子表格](#节点-7新建飞书电子表格)
   - [节点 8：获取 Sheet ID](#节点-8获取-sheet-id)
   - [节点 9：拼接写入 Body](#节点-9拼接写入-body)
   - [节点 10：写入表格内容](#节点-10写入表格内容)
   - [节点 11：结束](#节点-11结束)
4. [节点连线顺序](#4-节点连线顺序)
5. [常见报错与解决方法](#5-常见报错与解决方法)
6. [运行效果说明](#6-运行效果说明)

---

## 1. 整体流程概览

```
开始（上传视频）
  ↓
提取视频关键帧（FFmpeg 插件）
  ↓
解析并内嵌图片（Code 节点）
  ↓
生成分镜脚本（LLM / 通义千问）
  ↓
获取飞书 Access Token（HTTP 请求）
  ↓
提取 Access Token（Code 节点）
  ↓
新建飞书电子表格（HTTP 请求）
  ↓
获取 Sheet ID（Code 节点）
  ↓
拼接写入 Body（Code 节点）
  ↓
写入表格内容（HTTP 请求）
  ↓
结束
```

整个工作流共 **11 个节点**，其中 HTTP 请求节点 3 个、代码执行节点 4 个、工具节点 1 个、LLM 节点 1 个。

---

## 2. 前置准备

### 2.1 Dify 插件安装

在 Dify 的插件市场中安装以下两个插件：

| 插件名                     | 用途                            |
| -------------------------- | ------------------------------- |
| `langgenius/tongyi`        | 通义千问模型，用于生成分镜脚本  |
| `livien/ffmpeg_tools_dify` | FFmpeg 工具，用于提取视频关键帧 |

### 2.2 飞书开放平台配置

1. 进入[飞书开放平台](https://open.feishu.cn/)，创建一个**企业自建应用**。
2. 记录下应用的 `App ID` 和 `App Secret`。
3. 在应用的「权限管理」中，开通以下权限：
   - `sheets:spreadsheet`（电子表格读写权限）
   - `drive:drive`（云盘权限，用于新建文件）
4. 发布应用（或在测试环境下启用）。

---

## 3. 节点详细配置

### 节点 1：开始

**节点类型**：开始节点

**描述**：上传视频文件，可选填产品背景和风格偏好。

**输入变量配置**：

| 变量名            | 类型         | 必填 | 说明                                                      |
| ----------------- | ------------ | ---- | --------------------------------------------------------- |
| `video_file`      | 文件（视频） | 是   | 支持 .MP4 / .MOV / .M4V / .AVI / .MKV / .WEBM，最大 100MB |
| `product_context` | 段落文本     | 否   | 产品或内容背景描述，最多 2000 字                          |
| `style_hint`      | 段落文本     | 否   | 风格偏好描述，最多 2000 字                                |

---

### 节点 2：提取视频关键帧

**节点类型**：工具节点（FFmpeg 插件）

**工具**：`get_video_frame_list`

**参数配置**：

| 参数       | 类型 | 值                       |
| ---------- | ---- | ------------------------ |
| `video`    | 变量 | 引用 `开始.video_file`   |
| `gap_time` | 常量 | `2`（每隔 2 秒提取一帧） |
| `count`    | 常量 | `20`（最多提取 20 帧）   |

**输出**：

- `text`：JSON 格式的帧信息字符串
- `files`：图片文件列表（包含公网 URL）

---

### 节点 3：解析并内嵌图片

**节点类型**：代码执行（Python3）

**描述**：从 FFmpeg 工具的输出中提取分镜信息，构建时间轴和图片 Markdown。

**输入变量**：

| 变量名      | 来源                   |
| ----------- | ---------------------- |
| `json_data` | `提取视频关键帧.text`  |
| `files`     | `提取视频关键帧.files` |

**代码**：

```python
import json

def main(files: list, json_data: str) -> dict:
    shots = []
    frame_lines_text = []
    frame_lines_md = []
    
    json_list = []
    try:
        json_list = json.loads(json_data) if json_data else []
    except:
        pass
    
    time_map = {}
    if json_list and isinstance(json_list, list):
        for frame_info in json_list:
            if isinstance(frame_info, dict) and 'frames' in frame_info:
                for f in frame_info.get('frames', []):
                    frame_num = f.get('frame_number', 0)
                    seek_time = f.get('seek_time', 0)
                    time_map[frame_num] = seek_time
    
    for i, file_info in enumerate(files):
        if not isinstance(file_info, dict):
            continue
        
        filename = file_info.get('filename', f'frame_{i+1:03d}.jpg')
        try:
            frame_num = int(filename.split('_')[-1].replace('.jpg', ''))
        except:
            frame_num = i + 1
        
        seek_time = time_map.get(frame_num, i * 2.0)
        start_s = round(seek_time, 2)
        end_s = round(seek_time + 2.0, 2)
        image_url = file_info.get('url', '')
        
        shots.append({
            'shot_no': i + 1,
            'start': start_s,
            'end': end_s,
            'duration': 2.0,
            'frame_number': frame_num,
            'filename': filename,
            'keyframe_url': image_url,
        })
        
        frame_lines_text.append(
            f"分镜 {i+1:02d} | 时间: {start_s}s - {end_s}s | {filename}"
        )
        frame_lines_md.append(
            f"### 分镜 {i+1:02d}（{start_s}s - {end_s}s）\n"
            f"![{filename}]({image_url})"
        )
    
    return {
        'shots_json': json.dumps(shots, ensure_ascii=False, indent=2),
        'frame_urls': '\n'.join(frame_lines_text),
        'frames_md': '\n\n'.join(frame_lines_md),
        'shot_count': len(shots),
        'total_frames': len(files) if files else 0,
    }
```

**输出变量声明**：

| 变量名         | 类型   |
| -------------- | ------ |
| `shots_json`   | String |
| `frame_urls`   | String |
| `frames_md`    | String |
| `shot_count`   | Number |
| `total_frames` | Number |

---

### 节点 4：生成分镜脚本（LLM）

**节点类型**：LLM

**模型**：通义千问（`qwen3.6-plus-2026-04-02`），temperature 0.3，开启视觉模式（vision: high）

**System Prompt**：

```
你是"视频反推分镜提示词 Agent"，擅长把视频拆解为可复拍、可复用的广告/短视频分镜脚本。

重要规则：
1. 必须输出中文 Markdown。
2. 每个分镜必须包含以下字段（缺一不可）：
   分镜编号与标题（含精确时间）、分镜图片、画面主体、动作、环境、运镜、风格控制
3. "分镜图片"字段：直接使用 <分镜图片Markdown> 中对应编号的完整图片标签，
   格式为 ![分镜XX关键帧](https://...) ，原样复制，不要截断或修改 URL。
4. 时间范围严格使用 JSON 中的 start/end，不可自行更改。
5. 每个字段必须有具体描述，严禁只写"无"或"暂无"。
6. 结尾附"整体创作建议"段落。
```

**User Prompt**：

```
请根据以下信息生成完整分镜脚本。

<产品或内容背景>
{{#start.product_context#}}
</产品或内容背景>

<用户风格偏好>
{{#start.style_hint#}}
</用户风格偏好>

<分镜数量>
共 {{#parse_result.shot_count#}} 个分镜
</分镜数量>

<ffmpeg分镜JSON（含时间轴）>
{{#parse_result.shots_json#}}
</ffmpeg分镜JSON>

<分镜时间轴清单>
{{#parse_result.frame_urls#}}
</分镜时间轴清单>

<分镜图片Markdown（每个分镜对应一张，原样复制到输出中）>
{{#parse_result.frames_md#}}
</分镜图片Markdown>

---
请严格按照以下格式输出：

# 视频反推分镜脚本

---

## 分镜 01：场景标题 (0.0s - 2.0s)

**分镜图片：**
![分镜01关键帧](原样复制对应的完整URL)

**画面主体：** 描述画面中心元素。

**动作：** 描述人物/物体运动或变化。

**环境：** 描述背景、光线、氛围。

**运镜：** 如"固定镜头"、"缓慢推进"、"Pan right"等。

**风格控制：** 3-5个关键词，如"简洁、明亮、产品特写"。

---

（依次输出全部分镜，格式相同）

---

## 整体创作建议
```

**输出**：`text`（分镜脚本全文）

---

### 节点 5：获取飞书 Access Token

**节点类型**：HTTP 请求

**标题**：HTTP 请求 3-获取 access-token

**Method**：`POST`

**URL**：

```
https://open.feishu.cn/open-apis/auth/v3/app_access_token/internal
```

**Headers**：

```
Content-Type: application/json; charset=utf-8
```

**Body（JSON）**：

```json
{
    "app_id": "你的App ID",
    "app_secret": "你的App Secret"
}
```

> ⚠️ 将 `app_id` 和 `app_secret` 替换为飞书开放平台中你的应用凭证。建议将这两个值存入 Dify 的**环境变量**中，通过变量引用而不是硬编码。

**输出**：`body`（包含 `tenant_access_token` 的 JSON 字符串）

---

### 节点 6：提取 Access Token

**节点类型**：代码执行（Python3）

**标题**：代码执行-提取 body

**输入变量**：

| 变量名 | 来源                              |
| ------ | --------------------------------- |
| `body` | `HTTP请求3-获取access-token.body` |

**代码**：

```python
import json

def main(body: str) -> dict:
    data = json.loads(body)
    token = data["tenant_access_token"]
    return {
        "access_token": f"Bearer {token}"
    }
```

> 注意：这里直接在代码里拼接 `Bearer ` 前缀，后续 HTTP 节点 Header 中直接引用此变量，无需再手动拼接。

**输出变量声明**：

| 变量名         | 类型   |
| -------------- | ------ |
| `access_token` | String |

---

### 节点 7：新建飞书电子表格

**节点类型**：HTTP 请求

**标题**：HTTP 请求-创建表格

**Method**：`POST`

**URL**：

```
https://open.feishu.cn/open-apis/sheets/v3/spreadsheets
```

**Headers**：

| 键              | 值                                         |
| --------------- | ------------------------------------------ |
| `Authorization` | 引用变量：`代码执行-提取body.access_token` |
| `Content-Type`  | `application/json`                         |

**Body（JSON）**：

```json
{
  "title": "视频反推分镜脚本"
}
```

**输出**：`body`（包含新建表格的 `spreadsheet_token`）

---

### 节点 8：获取 Sheet ID

**节点类型**：代码执行（Python3）

**标题**：代码执行-取 sheetid

**描述**：解析创建表格的返回，并调用飞书 API 获取默认 Sheet 的 ID。

**输入变量**：

| 变量名         | 来源                             |
| -------------- | -------------------------------- |
| `body`         | `HTTP请求-创建表格.body`         |
| `access_token` | `代码执行-提取body.access_token` |

**代码**：

```python
import json
import requests

def main(body: str, access_token: str) -> dict:
    api_data = json.loads(body)
    spreadsheet_token = api_data["data"]["spreadsheet"]["spreadsheet_token"]
    
    # 调用 sheets/query 接口获取 sheet_id
    url = f"https://open.feishu.cn/open-apis/sheets/v3/spreadsheets/{spreadsheet_token}/sheets/query"
    resp = requests.get(url, headers={"Authorization": access_token})
    meta = json.loads(resp.text)
    
    if meta.get("code") != 0:
        raise Exception(f"获取sheet_id失败: {meta}")
    
    sheet_id = meta["data"]["sheets"][0]["sheet_id"]
    
    return {
        "spreadsheet_token": spreadsheet_token,
        "sheet_id": sheet_id,
        "debug": sheet_id
    }
```

**输出变量声明**：

| 变量名              | 类型   |
| ------------------- | ------ |
| `spreadsheet_token` | String |
| `sheet_id`          | String |
| `debug`             | String |

---

### 节点 9：拼接写入 Body

**节点类型**：代码执行（Python3）

**标题**：代码执行-拼接 body

**描述**：解析 LLM 输出的分镜脚本，提取结构化字段，生成飞书表格写入所需的 JSON Body。

**输入变量**：

| 变量名              | 来源                                   |
| ------------------- | -------------------------------------- |
| `llm_text`          | `生成分镜脚本.text`                    |
| `sheet_id`          | `代码执行-取sheetid.sheet_id`          |
| `spreadsheet_token` | `代码执行-取sheetid.spreadsheet_token` |

**代码**：

```python
import json, re

def main(llm_text: str, sheet_id: str, spreadsheet_token: str) -> dict:
    rows = [["分镜编号", "时间范围", "图片URL", "画面主体", "动作", "环境", "运镜", "风格控制"]]
    
    # 按分镜分割
    shots_raw = re.split(r"(?=## 分镜\s*\d+)", llm_text)
    
    for block in shots_raw:
        if not block.strip().startswith("## 分镜"):
            continue
        
        # 提取编号和时间范围
        m = re.search(r"## 分镜\s*(\d+)[：:](.*?)\((.+?)\)", block)
        num = f"分镜{m.group(1)}" if m else ""
        time_range = m.group(3) if m else ""
        
        # 提取图片 URL（只保留 https:// 链接，丢弃 base64）
        img_m = re.search(r"!\[.*?\]\((https://[^\)]+)\)", block)
        img_url = img_m.group(1) if img_m else ""
        
        # 提取各文字字段
        def get_field(field):
            fm = re.search(rf"\*\*{field}[：:]\*\*\s*(.+)", block)
            return fm.group(1).strip() if fm else ""
        
        rows.append([
            num, time_range, img_url,
            get_field("画面主体"),
            get_field("动作"),
            get_field("环境"),
            get_field("运镜"),
            get_field("风格控制"),
        ])
    
    end_row = len(rows)
    
    # 注意：飞书 v2 API 使用驼峰命名 valueRanges（不是 value_ranges）
    body = json.dumps({
        "valueRanges": [{
            "range": f"{sheet_id}!A1:H{end_row}",
            "values": rows
        }]
    }, ensure_ascii=False)
    
    return {
        "body": body,
        "debug": f"共{end_row-1}个分镜"
    }
```

**输出变量声明**：

| 变量名  | 类型   |
| ------- | ------ |
| `body`  | String |
| `debug` | String |

---

### 节点 10：写入表格内容

**节点类型**：HTTP 请求

**标题**：HTTP 请求 2-写入内容

**Method**：`POST`

**URL**：

在 URL 输入框中，依次输入以下内容（`spreadsheet_token` 部分通过变量选择器插入）：

```
https://open.feishu.cn/open-apis/sheets/v2/spreadsheets/{spreadsheet_token}/values_batch_update
```

其中 `{spreadsheet_token}` 引用变量：`代码执行-取sheetid.spreadsheet_token`

**Headers**：

| 键              | 值                                         |
| --------------- | ------------------------------------------ |
| `Authorization` | 引用变量：`代码执行-提取body.access_token` |
| `Content-Type`  | `application/json`                         |

**Body**：

- 类型选择：`raw`
- 内容：引用变量 `代码执行-拼接body.body`

**输出**：`body`（写入结果，成功时 `code` 为 0，`updatedCells` 为写入的单元格数量）

---

### 节点 11：结束

**节点类型**：结束节点

**输出变量**：

| 变量名         | 来源                        | 说明             |
| -------------- | --------------------------- | ---------------- |
| `final_script` | `生成分镜脚本.text`         | 完整分镜脚本文本 |
| `shot_count`   | `解析并内嵌图片.shot_count` | 分镜总数         |
| `debug`        | `代码执行5.debug`           | 写入结果调试信息 |

---

## 4. 节点连线顺序

按以下顺序连接节点（从左到右）：

```
开始
  → 提取视频关键帧
  → 解析并内嵌图片
  → 生成分镜脚本
  → HTTP请求3-获取access-token
  → 代码执行-提取body
  → HTTP请求-创建表格
  → 代码执行-取sheetid
  → 代码执行-拼接body
  → HTTP请求2-写入内容
  → 结束
```

---

## 5. 常见报错与解决方法

### `Missing access token for authorization`

**原因**：Authorization Header 中的 token 未正确传入。

**解决**：

- 检查「代码执行-提取body」节点的输出 `access_token` 是否有值。
- 确认写入 HTTP 节点的 Header 中，Authorization 值引用的是 `access_token` 变量，而不是 `spreadsheet_token`。
- 确保代码里已经拼好了 `Bearer ` 前缀，Header 中不要重复拼接。

---

### `Missing required parameter: valueRanges`

**原因**：Body 中字段名写错，使用了下划线 `value_ranges` 而不是驼峰 `valueRanges`。

**解决**：飞书 v2 API 要求驼峰命名，将代码中的 `value_ranges` 改为 `valueRanges`。

---

### `JSONDecodeError: Extra data`

**原因**：传入 Code 节点的 `body` 变量不是纯 JSON 字符串，或者包含额外内容。

**解决**：

- 确认 Code 节点输入变量引用的是 HTTP 节点的 `.body` 字段，而不是整个节点输出。
- Dify HTTP 节点的 `body` 输出是固定的字符串类型，直接用 `json.loads(body)` 解析即可。

---

### `KeyError: 'data'`

**原因**：API 调用失败，返回的 JSON 里没有 `data` 字段（通常是鉴权失败导致）。

**解决**：

- 在 Code 节点里加 `print(resp.text)` 打印原始返回，检查实际错误信息。
- 常见原因是 `access_token` 传了两次 `Bearer`（比如变量里已有 `Bearer xxx`，Header 里又写了 `Bearer {变量}`）。

---

### `404 page not found`

**原因**：飞书 API 路径错误。

**解决**：

- 检查 URL 中是否有 `open-apis`（含短横线），容易误写成 `openapis`。
- 检查 API 版本路径是否正确（v2 vs v3）。

---

### 输入显示 `null`

**原因**：节点运行时上游变量没有流过来，通常是单独调试某个节点，而不是从头运行整个工作流。

**解决**：点击左上角「运行」按钮，从头跑完整个工作流，不要单独点节点的运行按钮。

---

## 6. 运行效果说明

工作流运行成功后，飞书电子表格中将自动创建一个名为**「视频反推分镜脚本」**的表格，包含以下 8 列：

| 列   | 内容                         |
| ---- | ---------------------------- |
| A    | 分镜编号（如：分镜01）       |
| B    | 时间范围（如：0.0s - 2.0s）  |
| C    | 图片 URL（可点击查看关键帧） |
| D    | 画面主体                     |
| E    | 动作                         |
| F    | 环境                         |
| G    | 运镜                         |
| H    | 风格控制                     |

每次运行会新建一张表格，不会覆盖历史记录。表格链接格式为：

```
https://你的飞书域名.feishu.cn/sheets/{spreadsheet_token}
```

可在工作流结束节点的输出中或飞书云盘中找到。