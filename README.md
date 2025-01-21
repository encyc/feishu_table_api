# Feishu Table API Python Client

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)

一个简洁高效的飞书开放API Python客户端，专为快速集成飞书多维表格、用户系统等功能开发。

## 功能特性

✅ 开箱即用的单文件实现

✅ 完善的类型提示与文档说明

✅ 自动化的Token缓存与刷新机制

✅ 智能处理Pandas DataFrame数据格式

✅ 全面的错误处理与重试机制

✅ 支持批量操作与分块上传

✅ 符合企业级应用的安全规范

## 快速开始

### 安装方式

#### 直接使用（单文件）

1. 下载 [`feishu_table_api.py`](https://github.com/encyc/feishu_table_api/feishu_table_api.py) 到项目目录
2. 安装依赖：

```
pip install requests pandas
```

### 基础用法

```python
from feishu_api import FeishuAPI
import pandas as pd
from datetime import datetime

# 初始化客户端（推荐使用环境变量）
api = FeishuAPI(
    app_id="your_app_id",
    app_secret="your_app_secret"
)

# 示例1：获取用户信息
try:
    user_id = api.get_user_id(email="user@company.com")
    print(f"用户ID: {user_id}")
except FeishuAPIError as e:
    print(f"查询失败: {e}")

# 示例2：写入多维表格
data = pd.DataFrame({
    "项目名称": ["AI助手开发", "数据分析平台"],
    "负责人": [user_id],
    "截止时间": [datetime.now().replace(hour=18, minute=0)]
})

response = api.insert_multi_data_to_table(
    app_token="test_app_token",
    table_id="test_table_id",
    data=data,
    chunk_size=500
)
```

## 核心方法

### 数据操作

| 方法名                         | 功能说明                 |
| ------------------------------ | ------------------------ |
| `insert_data_to_table`       | 插入单条记录             |
| `insert_multi_data_to_table` | 批量插入数据（自动分块） |
| `get_table_data`             | 查询表格数据             |
| `delete_all_records`         | 清空表格数据             |
| `delete_records`             | 批量删除指定记录         |

### 用户管理

| 方法名            | 功能说明                |
| ----------------- | ----------------------- |
| `get_user_id`   | 通过邮箱/手机获取用户ID |
| `get_user_info` | 获取用户详细信息        |

### 系统管理

| 方法名                   | 功能说明     |
| ------------------------ | ------------ |
| `get_app_access_token` | 获取应用凭证 |
| `refresh_tenant_token` | 刷新租户凭证 |

## 配置指南

### 1. 获取API凭证

1. 登录[飞书开放平台](https://open.feishu.cn/)
2. 创建企业自建应用
3. 在「凭证与基础信息」获取：
   - App ID
   - App Secret

### 2. 权限配置

根据需求开启以下权限：

- 通讯录权限（获取用户信息）
- 云文档权限（多维表格操作）
- 消息权限（通知推送）

## 最佳实践

### 安全建议

```python
# 推荐通过环境变量配置凭证
import os

api = FeishuAPI(
    app_id=os.getenv("FEISHU_APP_ID"),
    app_secret=os.getenv("FEISHU_APP_SECRET")
)
```

### 异常处理

```python
try:
    # API操作代码...
except AuthenticationError as e:
    # 处理认证错误
    send_alert(f"凭证失效: {e}")
except APIRequestError as e:
    # 处理请求错误
    logger.error(f"API请求失败: {e}")
except FeishuAPIError as e:
    # 通用错误处理
    print(f"操作异常: {e}")
```

## 注意事项

⚠️ **数据格式要求**

- 日期字段自动转换为时间戳
- NaN值自动转为空字符串
- 支持最大5000条/次的批量操作

📝 **使用限制**

- 默认请求超时：10秒
- Token缓存时间：2小时
- 频率限制：50次/秒（根据应用等级变化）

## 参与贡献

欢迎通过以下方式参与项目：

1. 提交Issue报告问题
2. Fork仓库并提交Pull Request
3. 完善项目文档（[README.md](https://github.com/encyc/feishu_table_api/blob/main/README.md)）

代码规范：

- 使用Google风格文档字符串
- 遵循PEP8编码规范
- 重要变更需添加单元测试

## 开源协议

本项目采用 [MIT License](LICENSE)

## 技术支持

如有技术问题请联系：

- 知乎：@高原高冷
- 提交issue
