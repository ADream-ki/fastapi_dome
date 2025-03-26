# PyNote

**PyNote** 是一款轻量级的开源笔记应用程序，其灵感源自 [Memos](https://github.com/usememos/memos)。用户可以借助它迅速记录简短笔记、摘录内容或者突发灵感，还能选择性地为笔记添加待办事项和提醒功能。PyNote 以简洁高效为设计理念，运用现代 Python 技术构建了一个简洁的 RESTful API 后端。

## 技术栈

- **后端框架**：[FastAPI](https://fastapi.tiangolo.com/)（版本 0.115.0 或最新）
- **数据库 ORM**：[SQLAlchemy 2.0](https://docs.sqlalchemy.org/en/20/)，支持异步操作（`asyncio`）
- **数据验证**：[Pydantic v2](https://docs.pydantic.dev/latest/)
- **Python 版本**：3.13（支持最新语法，如 `str | None`）
- **数据库**：开发环境使用 SQLite，生产环境支持 PostgreSQL。

## 项目结构

```
pynote/
├── main.py          # FastAPI 应用程序入口文件
├── models.py        # SQLAlchemy 数据库模型定义文件
├── schemas.py       # Pydantic 请求/响应验证模式定义文件
├── README.md        # 项目文档（即本文档）
└── requirements.txt # 项目依赖文件
```

## 安装步骤

### 前提条件

- Python 3.13 及以上版本
- pip（Python 包管理器）

### 具体步骤

1. **克隆项目仓库**

```bash
git clone https://github.com/Adream-ki/fastapi.git
cd fastapi
```

2. **创建虚拟环境**

```bash
python -m venv venv
source venv/bin/activate  # 在 Windows 系统上使用：venv\Scripts\activate
```

3. **安装项目依赖**

```bash
pip install -r requirements.txt
```

4. **启动应用程序**

```bash
uvicorn main:app --reload
```

启动后，API 服务将在 `http://127.0.0.1:8000` 上运行。你可以访问 `http://127.0.0.1:8000/docs` 查看交互式 Swagger UI 文档。

## API 端点

### 笔记相关

- `POST /notes`：创建新笔记。
- `GET /notes`：列出所有笔记及其关联的待办事项和提醒。
- `GET /notes/{note_id}`：获取单个笔记及其关联的待办事项和提醒。
- `PATCH /notes/{note_id}`：更新笔记信息。
- `DELETE /notes/{note_id}`：删除笔记。

### 待办事项相关

- `POST /todos`：创建待办事项（可独立创建，也可通过 `note_id` 关联到特定笔记）。
- `GET /todos`：列出所有待办事项。
- `GET /todos?note_id={note_id}`：根据笔记 ID 筛选待办事项。
- `GET /todos/{todo_id}`：获取单个待办事项。
- `PATCH /todos/{todo_id}`：更新待办事项（如标记为已完成）。
- `DELETE /todos/{todo_id}`：删除待办事项。

### 提醒相关

- `POST /reminders`：创建提醒（可独立创建，也可通过 `note_id` 或 `todo_id` 关联到笔记或待办事项）。
- `GET /reminders`：列出所有提醒。
- `GET /reminders?note_id={note_id}`：根据笔记 ID 筛选提醒。
- `GET /reminders?todo_id={todo_id}`：根据待办事项 ID 筛选提醒。
- `GET /reminders/{reminder_id}`：获取单个提醒。
- `PATCH /reminders/{reminder_id}`：更新提醒（如标记为已确认）。
- `DELETE /reminders/{reminder_id}`：删除提醒。

## 数据库模型

### 用户模型

- `id`：整数类型（主键）
- `username`：字符串类型（唯一，必填）
- `password_hash`：字符串类型（必填）
- `email`：字符串类型（可选）
- `created_at`：日期时间类型
- 关联关系：`notes`、`todos`、`reminders`

### 笔记模型

- `id`：整数类型（主键）
- `user_id`：整数类型（外键）
- `title`：字符串类型（可选）
- `content`：字符串类型（可选）
- `created_at`：日期时间类型
- `updated_at`：日期时间类型
- 关联关系：`user`、`todos`、`reminders`

### 待办事项模型

- `id`：整数类型（主键）
- `user_id`：整数类型（外键）
- `note_id`：整数类型（可选，外键）
- `content`：字符串类型（必填）
- `is_completed`：布尔类型（默认值：False）
- `created_at`：日期时间类型
- `updated_at`：日期时间类型
- 关联关系：`user`、`note`

### 提醒模型

- `id`：整数类型（主键）
- `user_id`：整数类型（外键）
- `note_id`：整数类型（可选，外键）
- `todo_id`：整数类型（可选，外键）
- `reminder_time`：日期时间类型（必填）
- `message`：字符串类型（可选）
- `is_triggered`：布尔类型（默认值：False）
- `is_acknowledged`：布尔类型（默认值：False）
- `created_at`：日期时间类型
- 关联关系：`user`、`note`、`todo`

## 后续开发计划

- [ ] 基于 Celery 的提醒调度功能。
- [ ] 基于 RabbitMQ 和 WebSocket 的通知系统。
- [ ] React 前端开发。

## 许可证

本项目采用 [MIT 许可证](LICENSE)。
