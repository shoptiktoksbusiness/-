# 系统说明文档
<img width="1912" height="922" alt="image" src="https://github.com/user-attachments/assets/a4d6961f-25da-4b10-bee8-f1e76e1a249c" />


## 一、系统概述

本系统是一个基于 Flask 的综合管理平台，主要用于黄金珠宝行业的业务管理，包含用户管理、客户管理、金价行情、访问监控、日志管理等多个功能模块。

### 技术架构
- **后端框架**: Flask (Python)
- **数据库**: MySQL (通过 SQLAlchemy ORM)
- **前端**: HTML + CSS + JavaScript + Jinja2 模板
- **图表库**: ECharts 5.5.0
- **地图**: ECharts 中国地图 (GeoJSON)

### 运行环境
- Python 3.x
- MySQL 数据库
- 虚拟环境: `/www/wwwroot/Go-python/venv`

---

## 二、功能模块

### 1. 仪表盘 (Dashboard)
- 实时数据统计展示
- 访问数据概览
- 系统状态监控

### 2. 访问监控 (Access Monitor)
- 实时访问数据统计
- 中国地图可视化展示访问分布
- 访问趋势图表
- 访问来源分析（浏览器统计饼图）
- 访问记录列表
- 支持按时间范围筛选（1小时、6小时、24小时、7天、30天）
- 支持删除访问记录（按日期范围或天数）

### 3. 日志管理 (Logs)
- 操作日志记录（新增、修改、删除、重置密码等）
- 日志统计：今日操作、本周操作、涉及模块、活跃用户
- 支持按操作类型、模块、用户、日期筛选
- 分页展示
- **自动清理**: 180天前的日志自动删除
- 手动清理过期日志功能

### 4. 用户管理 (Users)
- 用户列表展示
- 创建新用户
- 编辑用户信息（姓名、部门、职位、入职日期等）
- 删除用户
- 重置密码
- 启用/禁用用户状态
- 菜单权限设置

### 5. 客户管理 (Customers)
- 客户列表展示
- 创建新客户（姓名、手机号必填）
- 编辑客户信息
- 删除客户
- 重置密码
- 首字母头像显示
- 地址字段记录

### 6. 金价行情 (Gold Price)
- 实时金价数据展示
- 回购价、销售价、最高价、最低价
- 自动刷新功能（可设置刷新间隔）
- 数据来源: 浏览器实时抓取

### 7. 系统设置 (Settings)
- 系统参数配置
- API密钥生成
- 邮件测试
- 短信测试
- 七牛云配置
- 数据库备份

### 8. 信息维护 (Information)
采用标签页(TAB)方式展示，包含：
- 供应商管理
- 柜台管理
- 分类管理
- 款式管理
- 成色管理
- 主石管理
- 辅石管理
- 级别管理
- 加工方式管理
- 邮递公司管理

### 9. 现货管理 (Spot)
- 进货管理
- 商品列表
- 商品库存
- 商品开单
- 商品订单

### 10. 加工管理 (Processing)
- 加工产品
- 加工订单

### 11. 回收管理 (Recycling)
- 回收订单

### 12. 预约管理 (Appointment)
- 预约列表
- 预约详情

### 13. 打印模板 (Print Template)
- 模板管理

---

## 三、数据库结构

### 主要数据表

| 表名 | 说明 |
|------|------|
| user | 用户表 |
| customer | 客户表 |
| access_log | 访问日志表 |
| operation_log | 操作日志表 |
| settings | 系统设置表 |

### AccessLog 表结构
```sql
- id: 主键
- ip_address: IP地址
- region: 地区
- city: 城市
- browser: 浏览器
- user_agent: 用户代理
- page: 访问页面
- status: 状态码
- created_at: 创建时间
```

### OperationLog 表结构
```sql
- id: 主键
- user_id: 用户ID
- username: 用户名
- action: 操作类型（新增、修改、删除等）
- module: 模块名称
- description: 操作描述
- ip_address: IP地址
- user_agent: 用户代理
- request_method: 请求方法
- request_path: 请求路径
- status_code: 状态码
- detail: 详细信息
- created_at: 创建时间
```

---

## 四、API接口

### 用户管理 API
| 接口 | 方法 | 说明 |
|------|------|------|
| /api/users | GET | 获取用户列表 |
| /api/users | POST | 创建用户 |
| /api/users/<id> | PUT | 更新用户 |
| /api/users/<id> | DELETE | 删除用户 |
| /api/users/<id>/reset-password | POST | 重置密码 |
| /api/users/<id>/toggle-status | POST | 切换状态 |
| /api/users/<id>/menus | PUT | 更新菜单权限 |

### 客户管理 API
| 接口 | 方法 | 说明 |
|------|------|------|
| /api/customers | GET | 获取客户列表 |
| /api/customers | POST | 创建客户 |
| /api/customers/<id> | PUT | 更新客户 |
| /api/customers/<id> | DELETE | 删除客户 |
| /api/customers/<id>/reset-password | POST | 重置密码 |

### 访问监控 API
| 接口 | 方法 | 说明 |
|------|------|------|
| /api/access_stats | GET | 获取访问统计 |
| /api/access_logs/delete | POST | 删除访问日志 |

### 操作日志 API
| 接口 | 方法 | 说明 |
|------|------|------|
| /api/operation_logs | GET | 获取操作日志（支持分页、筛选） |
| /api/operation_logs/stats | GET | 获取日志统计 |
| /api/operation_logs/cleanup | POST | 手动清理过期日志 |

### 金价 API
| 接口 | 方法 | 说明 |
|------|------|------|
| /api/gold-prices | GET | 获取金价数据 |
| /api/gold-refresh-interval | POST | 设置刷新间隔 |

---

## 五、操作日志记录

系统自动记录以下操作：

| 操作类型 | 模块 | 说明 |
|----------|------|------|
| 新增 | 用户管理 | 创建新用户 |
| 修改 | 用户管理 | 更新用户信息 |
| 删除 | 用户管理 | 删除用户 |
| 重置密码 | 用户管理 | 重置用户密码 |
| 新增 | 客户管理 | 创建新客户 |
| 修改 | 客户管理 | 更新客户信息 |
| 删除 | 客户管理 | 删除客户 |
| 修改 | 系统设置 | 更新系统设置 |
| 删除 | 访问监控 | 删除访问日志 |
| 清理 | 日志管理 | 手动清理过期日志 |

---

## 六、日志清理策略

- **自动清理**: 系统启动时自动删除180天前的日志
- **清理范围**: 访问日志 (access_log) + 操作日志 (operation_log)
- **手动清理**: 日志管理页面提供"清理过期"按钮

---

## 七、默认配置

### 默认管理员
- 用户名: admin
- 密码: admin123

### 数据库连接
```
mysql+pymysql://admin:password@localhost/admin_db
```

### 服务器端口
- 默认端口: 5000
- 访问地址: http://localhost:5000

---

## 八、启动方式

```bash
cd /www/wwwroot/Go-python
./venv/bin/python3 app.py
```

---

## 九、文件结构

```
/www/wwwroot/Go-python/
├── app.py                 # 主应用文件
├── templates/
│   └── index.html         # 主页面模板
├── static/
│   └── json/
│       └── china.json     # 中国地图GeoJSON
└── venv/                  # Python虚拟环境
```

---

## 十、注意事项

1. 日志保留期限为180天，超期自动删除
2. 地图数据使用本地GeoJSON文件，路径: `/static/json/china.json`
3. 金价数据通过浏览器实时抓取获取
4. 操作日志自动记录，无需手动触发
5. 访问日志在用户登录后自动记录每次请求

---

*文档生成时间: 2026-06-18*
