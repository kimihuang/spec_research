# lark-cli 操作指南

> 飞书官方 CLI 工具，由 larksuite 团队维护。覆盖 12 大业务域、200+ 命令，同时支持人类用户和 AI Agent 在终端中操作飞书。
>
> - **npm 包**: `@larksuite/cli`
> - **源码仓库**: https://github.com/larksuite/cli
> - **开源协议**: MIT

---

## 目录

- [lark-cli 操作指南](#lark-cli-操作指南)
  - [目录](#目录)
  - [安装](#安装)
    - [环境要求](#环境要求)
    - [安装方式](#安装方式)
    - [验证安装](#验证安装)
  - [快速开始](#快速开始)
  - [配置管理](#配置管理)
    - [config init 参数](#config-init-参数)
  - [认证与授权](#认证与授权)
    - [身份模型](#身份模型)
    - [认证命令](#认证命令)
    - [auth login 参数](#auth-login-参数)
  - [三层命令体系](#三层命令体系)
    - [1. 快捷命令（Shortcuts）](#1-快捷命令shortcuts)
    - [2. API 命令](#2-api-命令)
    - [3. 通用 API 调用](#3-通用-api-调用)
  - [全局参数](#全局参数)
    - [输出格式](#输出格式)
    - [jq 过滤](#jq-过滤)
    - [分页](#分页)
    - [Dry Run](#dry-run)
  - [各业务域命令详解](#各业务域命令详解)
    - [日历 calendar](#日历-calendar)
    - [即时通讯 im](#即时通讯-im)
    - [云文档 docs](#云文档-docs)
      - [命令参数速查](#命令参数速查)
      - [更新模式说明](#更新模式说明)
      - [操作示例](#操作示例)
    - [云空间 drive](#云空间-drive)
    - [电子表格 sheets](#电子表格-sheets)
      - [命令参数速查](#命令参数速查-1)
      - [操作示例](#操作示例-1)
    - [多维表格 base](#多维表格-base)
      - [核心参数说明](#核心参数说明)
      - [字段类型对照表](#字段类型对照表)
      - [操作示例](#操作示例-2)
    - [任务管理](#任务管理)
      - [命令参数速查](#命令参数速查-2)
      - [操作示例](#操作示例-3)
    - [邮箱 mail](#邮箱-mail)
    - [通讯录 contact](#通讯录-contact)
    - [知识库 wiki](#知识库-wiki)
      - [命令结构](#命令结构)
      - [节点类型（node\_type）](#节点类型node_type)
      - [操作示例](#操作示例-4)
    - [视频会议 vc](#视频会议-vc)
    - [妙记 minutes](#妙记-minutes)
      - [命令结构](#命令结构-1)
      - [命令参数速查](#命令参数速查-3)
      - [妙记信息包含内容](#妙记信息包含内容)
      - [操作示例](#操作示例-5)
    - [审批 approval](#审批-approval)
    - [事件订阅 event](#事件订阅-event)
  - [Schema 自省](#schema-自省)
  - [健康检查](#健康检查)
  - [AI Agent Skills](#ai-agent-skills)
    - [安装 Skills](#安装-skills)
    - [Skills 列表](#skills-列表)
  - [安全须知](#安全须知)

---

## 安装

### 环境要求

- Node.js（npm/npx）

### 安装方式

```bash
# 从 npm 安装（推荐）
npm install -g @larksuite/cli

# 安装 AI Agent Skills（推荐）
npx skills add larksuite/cli -y -g
```

也可以仅安装特定领域的 Skill：

```bash
npx skills add larksuite/cli -s lark-calendar -y
npx skills add larksuite/cli -s lark-im -y
```

### 验证安装

```bash
lark-cli --version
lark-cli --help
```

---

## 快速开始

三步即可开始使用：

```bash
# 1. 配置应用凭证（交互式引导，仅需一次）
lark-cli config init

# 2. 登录授权（推荐自动选择常用权限）
lark-cli auth login --recommend

# 3. 开始使用
lark-cli calendar +agenda
```

---

## 配置管理

配置文件路径：`~/.lark-cli/config.json`

| 命令 | 说明 |
|------|------|
| `config init` | 初始化配置（交互式引导设置 appId、appSecret、brand） |
| `config show` | 显示当前配置 |
| `config default-as` | 查看/设置默认身份类型（user/bot/auto） |
| `config remove` | 删除所有应用配置和 token |

```bash
# 初始化新配置
lark-cli config init --new

# 指定品牌和语言
lark-cli config init --brand feishu --lang zh

# 查看当前配置
lark-cli config show

# 设置默认身份为 bot
lark-cli config default-as bot

# 查看当前默认身份
lark-cli config default-as
```

### config init 参数

| 参数 | 说明 |
|------|------|
| `--new` | 强制创建新配置 |
| `--app-id` | 应用 ID |
| `--app-secret-stdin` | 从 stdin 读取应用密钥 |
| `--brand` | 品牌：feishu（飞书）/ lark |
| `--lang` | 语言：zh / en |

---

## 认证与授权

### 身份模型

| 身份 | Token 类型 | 说明 |
|------|-----------|------|
| `user` | user_access_token | 用户身份，需 OAuth 登录，可访问用户个人资源 |
| `bot` | tenant_access_token | 应用身份，自动获取，仅能操作应用级别资源 |
| `auto`（默认） | 自动选择 | 根据命令上下文自动选择 |

### 认证命令

| 命令 | 说明 |
|------|------|
| `auth login` | OAuth 设备码登录 |
| `auth logout` | 登出并清除凭证 |
| `auth status` | 查看当前登录状态和已授权 scope |
| `auth check` | 校验指定 scope（exit 0=有权限, 1=缺失） |
| `auth scopes` | 列出应用所有可用 scope |
| `auth list` | 列出所有已认证用户 |

```bash
# 交互式登录（TUI 引导选择业务域和权限级别）
lark-cli auth login

# 推荐权限模式（自动选择常用 scope）
lark-cli auth login --recommend

# 按业务域筛选
lark-cli auth login --domain calendar,task

# 精确指定 scope
lark-cli auth login --scope "calendar:calendar:readonly"

# Agent 模式：立即返回验证 URL，不阻塞
lark-cli auth login --domain calendar --no-wait

# 稍后恢复轮询
lark-cli auth login --device-code <DEVICE_CODE>

# 查看登录状态
lark-cli auth status

# 校验是否有特定 scope
lark-cli auth check --scope "calendar:calendar:readonly"

# 身份切换示例
lark-cli calendar +agenda --as user
lark-cli im +messages-send --as bot --chat-id "oc_xxx" --text "Hello"
```

### auth login 参数

| 参数 | 说明 |
|------|------|
| `--domain` | 按业务域筛选 scope（可用：approval, base, calendar, contact, docs, drive, event, im, mail, minutes, sheets, task, vc, wiki, all） |
| `--scope` | 精确指定 scope |
| `--recommend` | 自动选择常用审批 scope |
| `--no-wait` | Agent 模式，立即返回验证 URL |
| `--device-code` | 使用已有的设备码恢复轮询 |
| `--json` | JSON 格式输出 |

---

## 三层命令体系

lark-cli 提供三种粒度的命令调用方式，按需选择：

### 1. 快捷命令（Shortcuts）

以 `+` 为前缀，对人类和 AI 友好，内置智能默认值、表格输出和 dry-run 预览。

```bash
lark-cli calendar +agenda
lark-cli im +messages-send --chat-id "oc_xxx" --text "Hello"
lark-cli docs +create --title "周报" --markdown "# 本周进展\n- 完成了 X 功能"
```

查看某个业务域的所有快捷命令：

```bash
lark-cli <service> --help
```

### 2. API 命令

从飞书 OAPI 元数据自动生成，100+ 精选命令与平台端点一一对应。

格式：`lark-cli <service> <resource> <method>`

```bash
lark-cli calendar calendars list
lark-cli calendar events instance_view --params '{"calendar_id":"primary","start_time":"1700000000","end_time":"1700086400"}'
```

### 3. 通用 API 调用

直接调用任意飞书开放平台端点，覆盖 2500+ API。

格式：`lark-cli api <METHOD> <path>`

```bash
lark-cli api GET /open-apis/calendar/v4/calendars
lark-cli api POST /open-apis/im/v1/messages \
  --params '{"receive_id_type":"chat_id"}' \
  --data '{"receive_id":"oc_xxx","msg_type":"text","content":"{\"text\":\"Hello\"}"}'
```

---

## 全局参数

所有命令均可使用的全局参数：

| 参数 | 说明 |
|------|------|
| `--as <type>` | 身份类型：user / bot / auto（默认 auto） |
| `--format <fmt>` | 输出格式：json（默认）/ ndjson / table / csv / pretty |
| `--params <json>` | URL / 查询参数 JSON |
| `--data <json>` | 请求体 JSON（POST/PATCH/PUT/DELETE） |
| `--page-all` | 自动翻页获取所有数据 |
| `--page-size <N>` | 每页大小（0 = API 默认值） |
| `--page-limit <N>` | `--page-all` 时最大页数（默认 10，0 = 无限制） |
| `--page-delay <MS>` | 翻页间隔毫秒数（默认 200） |
| `-o, --output <path>` | 二进制响应输出到文件 |
| `--jq <expr>` / `-q <expr>` | 用 jq 表达式过滤 JSON 输出 |
| `--dry-run` | 仅打印请求不执行 |

### 输出格式

```bash
lark-cli calendar +agenda --format json      # 完整 JSON（默认）
lark-cli calendar +agenda --format pretty    # 人性化格式
lark-cli calendar +agenda --format table     # 表格
lark-cli calendar +agenda --format ndjson    # 换行分隔 JSON（适合管道）
lark-cli calendar +agenda --format csv       # CSV 格式
```

### jq 过滤

```bash
# 提取特定字段
lark-cli calendar +agenda --jq '.items[].summary'

# 简写
lark-cli calendar +agenda -q '.items[].summary'
```

### 分页

```bash
# 自动翻页获取所有数据
lark-cli calendar calendars list --page-all

# 限制最多 5 页
lark-cli calendar calendars list --page-all --page-limit 5

# 自定义每页大小和翻页间隔
lark-cli calendar calendars list --page-all --page-size 50 --page-delay 500
```

### Dry Run

对可能产生副作用的命令先预览请求：

```bash
lark-cli im +messages-send --chat-id oc_xxx --text "hello" --dry-run
```

---

## 各业务域命令详解

### 日历 calendar

管理日历、日程和参会人。

| 快捷命令 | 说明 |
|---------|------|
| `+agenda` | 查看日程（默认今天） |
| `+create` | 创建日程并可选邀请参会人 |
| `+freebusy` | 查询用户忙闲状态和 RSVP 状态 |
| `+rsvp` | 回复日程邀请（接受/拒绝/暂定） |
| `+suggestion` | 智能推荐空闲时段 |

API 资源：`calendars`、`events`、`event.attendees`、`freebusys`

```bash
# 查看今日日程
lark-cli calendar +agenda

# 查看指定日期范围日程
lark-cli calendar +agenda --start 2025-01-01 --end 2025-01-07

# 查看指定日历日程
lark-cli calendar +agenda --calendar-id "cal_xxx"

# 创建日程
lark-cli calendar +create --title "团队会议" --start 2025-01-15T14:00:00 --end 2025-01-15T15:00:00

# 查询忙闲
lark-cli calendar +freebusy

# 推荐空闲时段
lark-cli calendar +suggestion

# 回复邀请
lark-cli calendar +rsvp --event-id "evt_xxx" --action accept
```

---

### 即时通讯 im

消息和群聊管理。

| 快捷命令 | 说明 |
|---------|------|
| `+messages-send` | 发送消息（支持文本/Markdown/富文本/图片/文件/视频/音频） |
| `+messages-reply` | 回复消息（支持话题回复） |
| `+messages-search` | 搜索消息（仅用户身份） |
| `+messages-mget` | 批量获取消息 |
| `+messages-resources-download` | 下载消息中的图片/文件 |
| `+chat-create` | 创建群聊 |
| `+chat-update` | 更新群聊名称或描述 |
| `+chat-search` | 搜索群聊 |
| `+chat-messages-list` | 获取聊天消息列表 |
| `+threads-messages-list` | 获取话题内消息列表 |

API 资源：`chats`、`chat.members`、`messages`、`images`、`pins`、`reactions`

```bash
# 发送文本消息
lark-cli im +messages-send --chat-id "oc_xxx" --text "Hello"

# 发送 Markdown 消息
lark-cli im +messages-send --chat-id "oc_xxx" --markdown "# 标题\n正文内容"

# 发送图片
lark-cli im +messages-send --chat-id "oc_xxx" --image /path/to/image.png

# 发送文件
lark-cli im +messages-send --chat-id "oc_xxx" --file /path/to/doc.pdf

# 私聊发送消息
lark-cli im +messages-send --user-id "ou_xxx" --text "私聊消息"

# 回复消息
lark-cli im +messages-reply --message-id "om_xxx" --text "回复内容"

# 在话题中回复
lark-cli im +messages-reply --message-id "om_xxx" --text "话题回复" --in-thread

# 搜索消息
lark-cli im +messages-search --query "关键词"

# 创建群聊
lark-cli im +chat-create --name "新群" --member-ids "ou_x1,ou_x2"

# 搜索群聊
lark-cli im +chat-search --query "群名称"

# 获取群消息列表
lark-cli im +chat-messages-list --chat-id "oc_xxx"

# 下载消息中的文件
lark-cli im +messages-resources-download --message-id "om_xxx" --file-key "file_xxx"

# 以用户身份发送
lark-cli im +messages-send --as user --chat-id "oc_xxx" --text "Hello"
```

---

### 云文档 docs

创建、读取、更新、搜索飞书文档。

| 快捷命令 | 说明 |
|---------|------|
| `+create` | 创建飞书文档 |
| `+fetch` | 获取文档内容 |
| `+update` | 更新文档（追加/覆盖/替换/插入/删除） |
| `+search` | 搜索云空间中的文档、知识库和电子表格 |
| `+media-download` | 下载文档中的图片或画板缩略图 |
| `+media-insert` | 在文档末尾插入本地图片或文件 |
| `+whiteboard-update` | 用 DSL 更新文档中的画板 |

#### 命令参数速查

| 命令 | 关键参数 | 说明 |
|------|---------|------|
| `+create` | `--title`, `--markdown`, `--folder-token`, `--wiki-space`, `--wiki-node` | 创建文档，可选放入文件夹或知识库 |
| `+fetch` | `--doc`（URL 或 token） | 获取文档内容 |
| `+update` | `--doc`, `--markdown`, `--mode`, `--new-title` | 更新文档，6 种模式 |
| `+search` | `--query`, `--filter` | 搜索文档 |
| `+media-download` | `--token`, `--type`（media/whiteboard）, `--output` | 下载媒体 |
| `+media-insert` | `--doc`, `--file`, `--type`（image/file） | 插入图片或文件 |
| `+whiteboard-update` | `--whiteboard-token`, `--overwrite` | 用 DSL 更新画板 |

#### 更新模式说明

`+update --mode` 支持 6 种模式：

| 模式 | 说明 |
|------|------|
| `append` | 在文档末尾追加内容 |
| `overwrite` | 覆盖整个文档 |
| `replace_all` | 全局替换匹配内容 |
| `replace_range` | 替换指定范围 |
| `insert_before` | 在定位点之前插入 |
| `insert_after` | 在定位点之后插入 |
| `delete_range` | 删除指定范围 |

#### 操作示例

```bash
# ========== 创建文档 ==========

# 创建空文档
lark-cli docs +create --title "周报"

# 创建带 Markdown 内容的文档
lark-cli docs +create --title "周报" --markdown "# 本周进展\n- 完成了 X 功能\n- 进行中 Y 功能"

# 创建文档并放入指定文件夹
lark-cli docs +create --title "方案文档" --folder-token "fld_xxx" --markdown "# 方案内容"

# 创建文档并放入知识库（个人知识库用 my_library）
lark-cli docs +create --title "技术文档" \
  --wiki-space "spc_xxx" --wiki-node "node_xxx" \
  --markdown "# 技术方案\n## 背景\n..."

# 创建文档并创建空白画板
lark-cli docs +create --title "架构图" --markdown "<whiteboard type=\"blank\"></whiteboard>"

# ========== 获取文档内容 ==========

# 通过 token 获取
lark-cli docs +fetch --doc "doccnxxx"

# 通过 URL 获取
lark-cli docs +fetch --doc "https://xxx.feishu.cn/docx/doccnxxx"

# ========== 更新文档 ==========

# 在末尾追加内容
lark-cli docs +update --doc "doccnxxx" --mode append \
  --markdown "## 新增章节\n补充内容"

# 覆盖整个文档
lark-cli docs +update --doc "doccnxxx" --mode overwrite \
  --markdown "# 全新内容\n## 第一章"

# 在指定标题后插入内容
lark-cli docs +update --doc "doccnxxx" --mode insert_after \
  --selection-by-title "## 背景介绍" \
  --markdown "### 补充说明\n新增内容"

# 替换指定范围的文本
lark-cli docs +update --doc "doccnxxx" --mode replace_range \
  --selection-with-ellipsis "旧内容开始...旧内容结束" \
  --markdown "新内容"

# 更新文档标题
lark-cli docs +update --doc "doccnxxx" --new-title "新版本周报"

# ========== 搜索文档 ==========

# 按关键词搜索
lark-cli docs +search --query "项目方案"

# 按条件筛选搜索
lark-cli docs +search --query "周报" --filter '{"type":"docx"}'

# ========== 媒体操作 ==========

# 下载文档中的图片
lark-cli docs +media-download --token "img_xxx" --output ./image.png

# 下载画板缩略图
lark-cli docs +media-download --token "wb_xxx" --type whiteboard --output ./thumbnail.png

# 在文档末尾插入图片
lark-cli docs +media-insert --doc "doccnxxx" --file ./screenshot.png --type image

# 在文档末尾插入文件（附 caption）
lark-cli docs +media-insert --doc "doccnxxx" --file ./report.pdf --type file --caption "Q4 报告"

# ========== 画板操作 ==========

# 覆盖更新画板内容
lark-cli docs +whiteboard-update --whiteboard-token "wb_xxx" --overwrite < dsl_input.json

# 追加更新画板内容
lark-cli docs +whiteboard-update --whiteboard-token "wb_xxx" < dsl_input.json
```

---

### 云空间 drive

文件、评论、权限管理。

| 快捷命令 | 说明 |
|---------|------|
| `+upload` | 上传本地文件到云空间 |
| `+download` | 从云空间下载文件 |
| `+export` | 导出文档/docx/sheet/bitable 为本地文件 |
| `+export-download` | 通过 file_token 下载已导出的文件 |
| `+import` | 导入本地文件为云文档 |
| `+move` | 移动文件或文件夹 |
| `+add-comment` | 添加文档评论 |
| `+task_result` | 查询异步任务结果 |

API 资源：`files`、`metas`、`file.comments`、`permission.members`、`user`、`file.statistics`、`file.view_records`

```bash
# 上传文件
lark-cli drive +upload --file /path/to/file.pdf

# 下载文件
lark-cli drive +download --token "file_xxx" --output ./local.pdf

# 导出文档
lark-cli drive +export --token "doc_xxx" --type pdf

# 导入文件
lark-cli drive +import --file /path/to/local.docx --type docx

# 移动文件
lark-cli drive +move --token "file_xxx" --folder-token "fld_xxx"

# 添加评论
lark-cli drive +add-comment --token "doc_xxx" --comment "评价内容"
```

---
### 电子表格 sheets
#### 命令参数速查

| 命令 | 关键参数 | 说明 |
|------|---------|------|
| `+create` | `--title`, `--headers`, `--data`, `--folder-token` | 创建电子表格 |
| `+read` | `--url`/`--spreadsheet-token`, `--range`, `--sheet-id`, `--value-render-option` | 读取单元格 |
| `+write` | `--url`/`--spreadsheet-token`, `--range`, `--values` | 写入（覆盖） |
| `+append` | `--url`/`--spreadsheet-token`, `--range`, `--values` | 追加行 |
| `+find` | `--url`/`--spreadsheet-token`, `--find`, `--range`, `--search-by-regex`, `--match-entire-cell`, `--ignore-case` | 查找 |
| `+info` | `--url`/`--spreadsheet-token` | 查看表格信息 |
| `+export` | `--url`/`--spreadsheet-token`, `--file-extension`（xlsx/csv）, `--sheet-id`（CSV 必填）, `--output-path` | 导出 |

> **指定表格**：所有命令都支持 `--url`（表格链接）或 `--spreadsheet-token`（token）两种方式定位表格。
>
> **Range 格式**：`SheetId!A1:D10`（带 Sheet）或 `A1:D10`（配合 `--sheet-id`）或单个单元格 `C2`。
>
> **value-render-option**：`ToString`（字符串）、`FormattedValue`（格式化）、`Formula`（公式）、`UnformattedValue`（原始值）。

#### 操作示例

```bash
# ========== 创建电子表格 ==========

# 创建空表格
lark-cli sheets +create --title "销售数据"

# 创建带表头的表格
lark-cli sheets +create --title "销售数据" --headers '["月份","销售额","利润"]'

# 创建带表头和初始数据的表格
lark-cli sheets +create --title "销售数据" \
  --headers '["月份","销售额","利润"]' \
  --data '[["1月",10000,2000],["2月",15000,3000],["3月",12000,2500]]'

# 创建表格并放入文件夹
lark-cli sheets +create --title "Q4数据" --folder-token "fld_xxx" \
  --headers '["日期","收入","支出"]'

# ========== 读取数据 ==========

# 通过 token 读取指定范围
lark-cli sheets +read --spreadsheet-token "spreadsheet_xxx" --range "A1:C10"

# 通过 URL 读取
lark-cli sheets +read --url "https://xxx.feishu.cn/sheets/xxx" --range "A1:C10"

# 指定 Sheet ID 读取
lark-cli sheets +read --spreadsheet-token "spreadsheet_xxx" \
  --sheet-id "sheet_xxx" --range "A1:D20"

# 读取为原始数值（不格式化）
lark-cli sheets +read --spreadsheet-token "spreadsheet_xxx" \
  --range "B2:B10" --value-render-option UnformattedValue

# 读取公式内容
lark-cli sheets +read --spreadsheet-token "spreadsheet_xxx" \
  --range "C1" --value-render-option Formula

# ========== 写入数据 ==========

# 写入单个单元格
lark-cli sheets +write --spreadsheet-token "spreadsheet_xxx" \
  --range "A1" --values '[["标题"]]'

# 写入一个区域（覆盖模式）
lark-cli sheets +write --spreadsheet-token "spreadsheet_xxx" \
  --range "A1:C3" \
  --values '[["月份","销售额","利润"],["1月",10000,2000],["2月",15000,3000]]'

# 写入到指定 Sheet
lark-cli sheets +write --spreadsheet-token "spreadsheet_xxx" \
  --sheet-id "sheet_xxx" --range "A1" \
  --values '[["新增数据"]]'

# ========== 追加数据 ==========

# 追加一行
lark-cli sheets +append --spreadsheet-token "spreadsheet_xxx" \
  --range "A1" --values '[["4月",13000,2800]]'

# 追加多行
lark-cli sheets +append --spreadsheet-token "spreadsheet_xxx" \
  --range "A1" \
  --values '[["5月",14000,3000],["6月",16000,3500],["7月",15000,3200]]'

# ========== 查找内容 ==========

# 在整个表格中查找
lark-cli sheets +find --spreadsheet-token "spreadsheet_xxx" --find "关键词"

# 在指定范围中查找
lark-cli sheets +find --spreadsheet-token "spreadsheet_xxx" \
  --range "A1:D100" --find "张三"

# 忽略大小写查找
lark-cli sheets +find --spreadsheet-token "spreadsheet_xxx" \
  --find "hello" --ignore-case

# 精确匹配整个单元格
lark-cli sheets +find --spreadsheet-token "spreadsheet_xxx" \
  --find "北京" --match-entire-cell

# 正则表达式查找
lark-cli sheets +find --spreadsheet-token "spreadsheet_xxx" \
  --find "^2025-0[1-6]" --search-by-regex

# 在公式中查找
lark-cli sheets +find --spreadsheet-token "spreadsheet_xxx" \
  --find "SUM" --include-formulas

# ========== 查看表格信息 ==========

# 通过 token 查看
lark-cli sheets +info --spreadsheet-token "spreadsheet_xxx"

# 通过 URL 查看
lark-cli sheets +info --url "https://xxx.feishu.cn/sheets/xxx"

# ========== 导出表格 ==========

# 导出为 xlsx
lark-cli sheets +export --spreadsheet-token "spreadsheet_xxx" \
  --file-extension xlsx --output-path ./export.xlsx

# 导出为 csv（需要指定 sheet-id）
lark-cli sheets +export --spreadsheet-token "spreadsheet_xxx" \
  --file-extension csv --sheet-id "sheet_xxx" --output-path ./export.csv

# 通过 URL 导出
lark-cli sheets +export --url "https://xxx.feishu.cn/sheets/xxx" \
  --file-extension xlsx --output-path ./data.xlsx
```

---
### 多维表格 base
#### 核心参数说明

| 参数 | 说明 |
|------|------|
| `--base-token` | 多维表格 token（basc_xxx） |
| `--table-id` | 数据表 ID 或名称（tbl_xxx） |
| `--view-id` | 视图 ID 或名称 |
| `--record-id` | 记录 ID（rec_xxx） |
| `--json` | 通用 JSON 输入参数 |

#### 字段类型对照表

| type 值 | 字段类型 |
|---------|---------|
| 1 | 多行文本 |
| 2 | 数字 |
| 3 | 单选项 |
| 4 | 多选项 |
| 5 | 日期 |
| 7 | 复选框 |
| 11 | 人员 |
| 13 | 电话号码 |
| 14 | URL |
| 15 | 邮箱 |
| 17 | 单选分组 |
| 1001 | 创建时间 |
| 1002 | 最后修改时间 |
| 1003 | 创建人 |
| 1004 | 修改人 |
| 1005 | 自动编号 |
| 1007 | 公式 |
| 1008 | 关联记录 |
| 1009 | 查找引用 |
| 1014 | 成员 |
| 1015 | 进度 |
| 1017 | 附件 |
| 1018 | 位置 |

#### 操作示例

```bash
# ========== Base 管理 ==========

# 创建 Base
lark-cli base +base-create --name "项目跟踪"

# 获取 Base 信息
lark-cli base +base-get --base-token "basc_xxx"

# 复制 Base
lark-cli base +base-copy --base-token "basc_xxx" --name "项目跟踪-副本"

# ========== 数据表管理 ==========

# 创建空数据表
lark-cli base +table-create --base-token "basc_xxx" --name "任务表"

# 创建数据表并定义字段
lark-cli base +table-create --base-token "basc_xxx" --name "任务表" \
  --fields '[
    {"field_name":"任务名称","type":1},
    {"field_name":"状态","type":3,"property":{"options":[{"name":"待处理","color":0},{"name":"进行中","color":1},{"name":"已完成","color":2}]}},
    {"field_name":"负责人","type":11},
    {"field_name":"优先级","type":3,"property":{"options":[{"name":"高","color":0},{"name":"中","color":1},{"name":"低","color":2}]}},
    {"field_name":"截止日期","type":5}
  ]'

# 创建数据表并同时创建视图
lark-cli base +table-create --base-token "basc_xxx" --name "工单表" \
  --fields '[{"field_name":"标题","type":1}]' \
  --view '{"view_name":"看板视图","view_type":3}'

# 列出所有数据表
lark-cli base +table-list --base-token "basc_xxx"

# 获取指定数据表信息
lark-cli base +table-get --base-token "basc_xxx" --table-id "tbl_xxx"

# 重命名数据表
lark-cli base +table-update --base-token "basc_xxx" --table-id "tbl_xxx" --name "新名称"

# 通过名称操作数据表（使用名称代替 ID）
lark-cli base +table-get --base-token "basc_xxx" --table-id "任务表"
lark-cli base +record-list --base-token "basc_xxx" --table-id "任务表"

# ========== 字段管理 ==========

# 列出数据表的所有字段
lark-cli base +field-list --base-token "basc_xxx" --table-id "tbl_xxx"

# 创建单选字段
lark-cli base +field-create --base-token "basc_xxx" --table-id "tbl_xxx" \
  --json '{"field_name":"优先级","type":3,"property":{"options":[{"name":"P0","color":0},{"name":"P1","color":1},{"name":"P2","color":2}]}}'

# 创建人员字段
lark-cli base +field-create --base-token "basc_xxx" --table-id "tbl_xxx" \
  --json '{"field_name":"负责人","type":11}'

# 创建日期字段
lark-cli base +field-create --base-token "basc_xxx" --table-id "tbl_xxx" \
  --json '{"field_name":"截止日期","type":5}'

# 创建公式字段
lark-cli base +field-create --base-token "basc_xxx" --table-id "tbl_xxx" \
  --json '{"field_name":"完成率","type":1007,"property":{"expression":"IF({完成数}/{总数}>0.8,\"达标\",\"未达标\")"}}'

# 更新字段（如修改选项）
lark-cli base +field-update --base-token "basc_xxx" --table-id "tbl_xxx" \
  --json '{"field_name":"优先级","type":3,"property":{"options":[{"name":"紧急","color":0},{"name":"普通","color":1},{"name":"低","color":2}]}}'

# 删除字段
lark-cli base +field-delete --base-token "basc_xxx" --table-id "tbl_xxx" --field-id "fld_xxx"

# 搜索单选字段的选项
lark-cli base +field-search-options --base-token "basc_xxx" --table-id "tbl_xxx" --query "高"

# ========== 记录管理 ==========

# 创建新记录
lark-cli base +record-upsert --base-token "basc_xxx" --table-id "tbl_xxx" \
  --json '{"fields":{"任务名称":"完成 API 对接","状态":"进行中","优先级":"高"}}'

# 创建记录（带人员字段）
lark-cli base +record-upsert --base-token "basc_xxx" --table-id "tbl_xxx" \
  --json '{"fields":{"任务名称":"编写测试用例","负责人":["ou_xxx"]}}'

# 更新已有记录
lark-cli base +record-upsert --base-token "basc_xxx" --table-id "tbl_xxx" \
  --record-id "rec_xxx" \
  --json '{"fields":{"状态":"已完成"}}'

# 获取单条记录
lark-cli base +record-get --base-token "basc_xxx" --table-id "tbl_xxx" --record-id "rec_xxx"

# 列出记录（默认 100 条）
lark-cli base +record-list --base-token "basc_xxx" --table-id "tbl_xxx"

# 列出记录（分页）
lark-cli base +record-list --base-token "basc_xxx" --table-id "tbl_xxx" \
  --limit 50 --offset 100

# 列出记录（通过视图筛选）
lark-cli base +record-list --base-token "basc_xxx" --table-id "tbl_xxx" --view-id "vew_xxx"

# 删除记录
lark-cli base +record-delete --base-token "basc_xxx" --table-id "tbl_xxx" --record-id "rec_xxx"

# 上传附件到记录
lark-cli base +record-upload-attachment --base-token "basc_xxx" \
  --table-id "tbl_xxx" --record-id "rec_xxx" --file /path/to/file.pdf

# 查看记录变更历史
lark-cli base +record-history-list --base-token "basc_xxx" \
  --table-id "tbl_xxx" --record-id "rec_xxx"

# ========== 视图管理 ==========

# 列出视图
lark-cli base +view-list --base-token "basc_xxx" --table-id "tbl_xxx"

# 创建视图（type: 1=表格, 2=看板, 3=甘特图, 4=画册, 5=表单, 6=日历, 7=时间线）
lark-cli base +view-create --base-token "basc_xxx" --table-id "tbl_xxx" \
  --json '[{"view_name":"看板视图","view_type":2}]'

# 设置视图筛选（筛选状态为进行中的记录）
lark-cli base +view-set-filter --base-token "basc_xxx" --table-id "tbl_xxx" \
  --view-id "vew_xxx" \
  --json '{"conditions":[{"field_name":"状态","operator":"is","value":["进行中"]}]}'

# 设置视图排序
lark-cli base +view-set-sort --base-token "basc_xxx" --table-id "tbl_xxx" \
  --view-id "vew_xxx" \
  --json '{"sorts":[{"field_name":"截止日期","desc":true}]}'

# 设置视图分组
lark-cli base +view-set-group --base-token "basc_xxx" --table-id "tbl_xxx" \
  --view-id "vew_xxx" \
  --json '{"groups":[{"field_name":"状态"}]}'

# 重命名视图
lark-cli base +view-rename --base-token "basc_xxx" --table-id "tbl_xxx" \
  --view-id "vew_xxx" --name "新视图名"

# 删除视图
lark-cli base +view-delete --base-token "basc_xxx" --table-id "tbl_xxx" --view-id "vew_xxx"

# ========== 仪表盘管理 ==========

# 创建仪表盘
lark-cli base +dashboard-create --base-token "basc_xxx" --name "数据概览"

# 列出仪表盘
lark-cli base +dashboard-list --base-token "basc_xxx"

# 在仪表盘中创建区块
lark-cli base +dashboard-block-create --base-token "basc_xxx" \
  --dashboard-id "dash_xxx" --json '{"block_name":"统计图表",...}'

# ========== 表单管理 ==========

# 创建表单
lark-cli base +form-create --base-token "basc_xxx" --table-id "tbl_xxx" --name "需求收集"

# 列出表单
lark-cli base +form-list --base-token "basc_xxx" --table-id "tbl_xxx"

# 为表单创建问题
lark-cli base +form-questions-create --base-token "basc_xxx" \
  --form-id "form_xxx" \
  --json '[{"name":"需求描述","type":1,"required":true}]'

# ========== 自动化流程 ==========

# 创建自动化流程
lark-cli base +workflow-create --base-token "basc_xxx" \
  --json '{"title":"状态变更通知","trigger":{...},"steps":{...}}'

# 列出自动化流程
lark-cli base +workflow-list --base-token "basc_xxx"

# 启用流程
lark-cli base +workflow-enable --base-token "basc_xxx" --workflow-id "wf_xxx"

# 禁用流程
lark-cli base +workflow-disable --base-token "basc_xxx" --workflow-id "wf_xxx"

# ========== 数据聚合查询 ==========

# 使用 DSL 查询（筛选 + 排序 + 分页）
lark-cli base +data-query --base-token "basc_xxx" --table-id "tbl_xxx" \
  --dsl '{
    "filter": {
      "conditions": [
        {"field_name": "状态", "operator": "is", "value": ["进行中"]},
        {"field_name": "优先级", "operator": "is", "value": ["高"]}
      ],
      "conjunction": "and"
    },
    "sort": [{"field_name": "截止日期", "desc": true}],
    "limit": 50
  }'

# ========== 角色权限 ==========

# 列出角色
lark-cli base +role-list --base-token "basc_xxx"

# 创建自定义角色
lark-cli base +role-create --base-token "basc_xxx" \
  --json '{"role_name":"只读成员","permission_rules":{...}}'

# 启用高级权限
lark-cli base +advperm-enable --base-token "basc_xxx"
```

---
### 任务管理
#### 命令参数速查

| 命令 | 关键参数 | 说明 |
|------|---------|------|
| `+create` | `--summary`, `--description`, `--due`, `--assignee`, `--tasklist-id`, `--data` | 创建任务 |
| `+update` | `--task-id`, `--summary`, `--description`, `--due`, `--data` | 更新任务 |
| `+complete` | `--task-id` | 标记完成 |
| `+reopen` | `--task-id` | 重新打开 |
| `+comment` | `--task-id`, `--content` | 添加评论 |
| `+assign` | `--task-id`, `--add`, `--remove` | 分配/移除成员 |
| `+reminder` | `--task-id`, `--set`（如 15m/1h/1d）, `--remove` | 管理提醒 |
| `+followers` | `--task-id`, `--add`, `--remove` | 管理关注者 |
| `+get-my-tasks` | `--complete`, `--due-start`, `--due-end`, `--query`, `--page-all` | 查询我的任务 |
| `+tasklist-create` | `--name`, `--member`, `--data` | 创建清单 |
| `+tasklist-members` | `--tasklist-id`, `--add`, `--remove`, `--set` | 管理清单成员 |
| `+tasklist-task-add` | `--tasklist-id`, `--task-id` | 添加任务到清单 |

> **截止日期格式**：支持多种格式 -- ISO 8601（`2025-01-20T18:00:00`）、日期（`date:2025-01-20`）、相对时间（`+2d`/`+1h`/`+30m`）、毫秒时间戳。

#### 操作示例

```bash
# ========== 创建任务 ==========

# 创建简单任务
lark-cli task +create --summary "完成周报" --as user

# 创建任务并指定截止日期
lark-cli task +create --summary "完成 API 对接" --due date:2025-02-01 --as user

# 创建任务（相对截止日期：2天后）
lark-cli task +create --summary "跟进客户反馈" --due +2d --as user

# 创建任务（30分钟后截止）
lark-cli task +create --summary "紧急修复" --due +30m --as user

# 创建任务并指定负责人
lark-cli task +create --summary "编写测试用例" --assignee "ou_xxx" --as user

# 创建任务（带描述）
lark-cli task +create --summary "系统架构设计" \
  --description "完成 L0 架构设计文档的初稿" --due +5d --as user

# 创建任务并加入任务清单
lark-cli task +create --summary "前端页面开发" \
  --tasklist-id "tasklist_xxx" --as user

# 创建任务（使用任务清单 applink URL）
lark-cli task +create --summary "接口联调" \
  --tasklist-id "https://applink.feishu.cn/client/todo/list/xxx" --as user

# 使用 JSON 创建任务（完整参数控制）
lark-cli task +create --data '{
  "summary": "完成部署",
  "description": "将服务部署到生产环境",
  "due": 1738000000000,
  "assignee": "ou_xxx"
}' --as user

# ========== 查看任务 ==========

# 查看我的未完成任务
lark-cli task +get-my-tasks --as user --format pretty

# 查看已完成的任务
lark-cli task +get-my-tasks --complete --as user --format pretty

# 查看指定截止日期范围内的任务
lark-cli task +get-my-tasks --due-start date:2025-01-01 --due-end date:2025-01-31 --as user

# 按关键词搜索任务
lark-cli task +get-my-tasks --query "周报" --as user

# 自动翻页获取所有任务
lark-cli task +get-my-tasks --page-all --as user

# ========== 更新任务 ==========

# 修改任务标题
lark-cli task +update --task-id "task_xxx" --summary "新标题" --as user

# 修改截止日期
lark-cli task +update --task-id "task_xxx" --due +3d --as user

# 修改任务描述
lark-cli task +update --task-id "task_xxx" --description "更新后的描述" --as user

# 同时更新多个属性
lark-cli task +update --task-id "task_xxx" \
  --summary "修改后标题" --due date:2025-03-01 --description "新描述" --as user

# 使用 JSON 批量更新多个任务
lark-cli task +update --task-id "task_x1,task_x2" --data '{"due":1738000000000}' --as user

# ========== 完成与重新打开 ==========

# 标记任务完成
lark-cli task +complete --task-id "task_xxx" --as user

# 重新打开已完成的任务
lark-cli task +reopen --task-id "task_xxx" --as user

# ========== 评论 ==========

# 添加评论
lark-cli task +comment --task-id "task_xxx" --content "进展顺利，预计明天完成" --as user

# ========== 成员管理 ==========

# 添加负责人
lark-cli task +assign --task-id "task_xxx" --add "ou_xxx" --as user

# 移除负责人
lark-cli task +assign --task-id "task_xxx" --remove "ou_xxx" --as user

# ========== 提醒管理 ==========

# 设置提醒（截止前 15 分钟提醒）
lark-cli task +reminder --task-id "task_xxx" --set 15m --as user

# 设置提醒（截止前 1 小时）
lark-cli task +reminder --task-id "task_xxx" --set 1h --as user

# 设置提醒（截止前 1 天）
lark-cli task +reminder --task-id "task_xxx" --set 1d --as user

# 清除所有提醒
lark-cli task +reminder --task-id "task_xxx" --remove --as user

# ========== 关注者管理 ==========

# 添加关注者
lark-cli task +followers --task-id "task_xxx" --add "ou_x1,ou_x2" --as user

# 移除关注者
lark-cli task +followers --task-id "task_xxx" --remove "ou_xxx" --as user

# ========== 任务清单管理 ==========

# 创建任务清单
lark-cli task +tasklist-create --name "Sprint 1" --as user

# 创建任务清单并添加成员
lark-cli task +tasklist-create --name "项目任务" --member "ou_x1,ou_x2" --as user

# 创建任务清单并同时创建任务
lark-cli task +tasklist-create --name "本周计划" \
  --data '[
    {"summary":"任务A","due":"+2d"},
    {"summary":"任务B","assignee":"ou_xxx"}
  ]' --as user

# 管理清单成员
lark-cli task +tasklist-members --tasklist-id "tasklist_xxx" --add "ou_x1,ou_x2" --as user
lark-cli task +tasklist-members --tasklist-id "tasklist_xxx" --remove "ou_xxx" --as user
lark-cli task +tasklist-members --tasklist-id "tasklist_xxx" --set "ou_x1,ou_x2" --as user

# 向清单添加任务（支持多个）
lark-cli task +tasklist-task-add --tasklist-id "tasklist_xxx" \
  --task-id "task_x1,task_x2" --as user
```

---

### 邮箱 mail

浏览、搜索、阅读、发送、回复、转发邮件，管理草稿。

| 快捷命令 | 说明 |
|---------|------|
| `+send` | 撰写新邮件（默认存草稿，`--confirm-send` 可直接发送） |
| `+reply` | 回复邮件 |
| `+reply-all` | 回复全部 |
| `+forward` | 转发邮件 |
| `+draft-create` | 创建草稿（非回复/转发场景） |
| `+draft-edit` | 编辑已有草稿 |
| `+message` | 读取单封邮件完整内容 |
| `+messages` | 批量读取邮件完整内容 |
| `+thread` | 查看完整邮件会话 |
| `+triage` | 列出邮件摘要（日期/发件人/主题） |
| `+watch` | 通过 WebSocket 监听新邮件事件 |

API 资源：`user_mailboxes`、`user_mailbox.messages`、`user_mailbox.threads`、`user_mailbox.drafts`、`user_mailbox.folders`、`user_mailbox.labels`、`user_mailbox.mail_contacts`、`user_mailbox.message.attachments`

```bash
# 列出邮件摘要
lark-cli mail +triage

# 搜索邮件
lark-cli mail +triage --query "关键词"

# 读取单封邮件
lark-cli mail +message --message-id "msg_xxx"

# 查看邮件会话
lark-cli mail +thread --thread-id "thread_xxx"

# 撰写并发送邮件
lark-cli mail +send --to "user@example.com" --subject "会议通知" --body "会议时间..." --confirm-send

# 回复邮件
lark-cli mail +reply --message-id "msg_xxx" --body "收到，谢谢"

# 转发邮件
lark-cli mail +forward --message-id "msg_xxx" --to "other@example.com"

# 创建草稿
lark-cli mail +draft-create --to "user@example.com" --subject "草稿" --body "内容"

# 监听新邮件
lark-cli mail +watch
```

---

### 通讯录 contact

按姓名/邮箱/手机号搜索用户，获取用户信息。

| 快捷命令 | 说明 |
|---------|------|
| `+search-user` | 搜索用户（按相关性排序） |
| `+get-user` | 获取用户信息（省略 user_id 查自己） |

```bash
# 搜索用户
lark-cli contact +search-user --query "张三"

# 获取自己的信息
lark-cli contact +get-user

# 获取指定用户信息
lark-cli contact +get-user --user-id "ou_xxx"
```

---
### 知识库 wiki
#### 命令结构

```
lark-cli wiki spaces <method> [--params <json>]
lark-cli wiki nodes  <method> [--params <json>] [--data <json>]
```

| 命令 | 说明 |
|------|------|
| `spaces list` | 获取知识空间列表 |
| `spaces get` | 获取知识空间信息 |
| `spaces get_node` | 获取知识空间节点信息 |
| `nodes list` | 获取知识空间子节点列表 |
| `nodes create` | 创建知识空间节点 |
| `nodes copy` | 创建知识空间节点副本 |

#### 节点类型（node_type）

| node_type | 说明 |
|-----------|------|
| `docx` | 新版文档 |
| `doc` | 旧版文档 |
| `sheet` | 电子表格 |
| `bitable` | 多维表格 |
| `mindnote` | 思维导图 |
| `file` | 文件 |
| `wiki` | 知识库（子空间） |
| `folder` | 文件夹 |

#### 操作示例

```bash
# ========== 知识空间 ==========

# 列出我有权限的知识空间
lark-cli wiki spaces list --as user --format pretty

# 自动翻页列出所有知识空间
lark-cli wiki spaces list --as user --page-all --format pretty

# 获取指定知识空间信息
lark-cli wiki spaces get --as user \
  --params '{"space_id":"spc_xxx"}' --format pretty

# ========== 节点查询 ==========

# 列出知识空间的子节点（根目录）
lark-cli wiki nodes list --as user \
  --params '{"space_id":"spc_xxx"}' --format pretty

# 分页查询子节点
lark-cli wiki nodes list --as user \
  --params '{"space_id":"spc_xxx","page_size":50,"page_token":""}' --format pretty

# 自动翻页获取所有子节点
lark-cli wiki nodes list --as user \
  --params '{"space_id":"spc_xxx"}' --page-all --format pretty

# 获取指定节点的详细信息
lark-cli wiki spaces get_node --as user \
  --params '{"space_id":"spc_xxx","node_token":"node_xxx"}' --format pretty

# 提取节点标题
lark-cli wiki spaces get_node --as user \
  --params '{"space_id":"spc_xxx","node_token":"node_xxx"}' \
  -q '.node.title'

# ========== 创建节点 ==========

# 在知识库根目录创建文档
lark-cli wiki nodes create --as user \
  --data '{"space_id":"spc_xxx","node_type":"docx","title":"技术方案"}'

# 在指定节点下创建子文档
lark-cli wiki nodes create --as user \
  --data '{"space_id":"spc_xxx","parent_node_token":"node_xxx","node_type":"docx","title":"子文档"}'

# 创建文件夹节点
lark-cli wiki nodes create --as user \
  --data '{"space_id":"spc_xxx","parent_node_token":"node_xxx","node_type":"folder","title":"归档"}'

# 创建电子表格节点
lark-cli wiki nodes create --as user \
  --data '{"space_id":"spc_xxx","parent_node_token":"node_xxx","node_type":"sheet","title":"数据统计"}'

# 创建多维表格节点
lark-cli wiki nodes create --as user \
  --data '{"space_id":"spc_xxx","parent_node_token":"node_xxx","node_type":"bitable","title":"需求跟踪"}'

# ========== 复制节点 ==========

# 复制知识库节点到同一空间
lark-cli wiki nodes copy --as user \
  --params '{"space_id":"spc_xxx","src_node_token":"node_xxx"}' \
  --data '{"dest_node_token":"dest_node_xxx","dest_space_id":"spc_yyy"}'

# ========== 组合使用：在知识库中创建并编辑文档 ==========

# 1. 在知识库中创建文档节点
lark-cli wiki nodes create --as user \
  --data '{"space_id":"spc_xxx","parent_node_token":"node_xxx","node_type":"docx","title":"周报"}' \
  -q '.node.token'

# 2. 用返回的 token 编辑文档内容
lark-cli docs +update --as user --doc "返回的token" --mode overwrite \
  --markdown "# 本周周报\n## 完成事项\n- 完成了 X 功能"

# ========== 组合使用：搜索知识库文档 ==========

# 搜索知识库中的文档
lark-cli docs +search --as user --query "架构设计" --filter '{"type":"docx"}'
```

---

### 视频会议 vc

搜索会议记录、查询会议纪要。

| 快捷命令 | 说明 |
|---------|------|
| `+search` | 搜索会议记录（至少需要一个筛选条件） |
| `+notes` | 查询会议纪要（总结、待办、章节、逐字稿） |

API 资源：`meeting`

```bash
# 搜索会议记录
lark-cli vc +search --start-time 2025-01-01 --end-time 2025-01-31

# 查询会议纪要
lark-cli vc +notes --meeting-ids "meeting_xxx"

# 通过妙记 token 查询
lark-cli vc +notes --minute-tokens "minute_xxx"
```

---
### 妙记 minutes
#### 命令结构

```
lark-cli minutes +download      # 快捷命令：下载音视频
lark-cli minutes minutes get     # API 命令：获取妙记元数据
```

#### 命令参数速查

| 命令 | 关键参数 | 说明 |
|------|---------|------|
| `+download` | `--minute-tokens`（逗号分隔，最多 50 个）, `--output`（单文件路径或批量目录）, `--overwrite`, `--url-only` | 下载音视频 |
| `minutes get` | `--params`（含 `minute_id`） | 获取妙记信息 |

> **妙记 URL 格式**：`https://xxx.feishu.cn/minutes/<minute-token>`，从中可提取 `minute-token`。

#### 妙记信息包含内容

通过 `minutes minutes get` 获取的元数据包括：
- 基础信息：标题、开始时间、时长、创建人、参与者
- AI 产物：`summary`（总结）、`todo`（待办）、`chapters`（章节）
- 媒体：音视频文件 URL

#### 操作示例

```bash
# ========== 获取妙记信息 ==========

# 获取妙记元数据和 AI 产物
lark-cli minutes minutes get \
  --params '{"minute_id":"minute_xxx"}' --as user --format pretty

# 仅提取标题
lark-cli minutes minutes get \
  --params '{"minute_id":"minute_xxx"}' --as user -q '.minute.title'

# 提取 AI 总结
lark-cli minutes minutes get \
  --params '{"minute_id":"minute_xxx"}' --as user -q '.minute.ai_summary.summary'

# 提取 AI 待办事项
lark-cli minutes minutes get \
  --params '{"minute_id":"minute_xxx"}' --as user -q '.minute.ai_todo'

# 提取 AI 章节结构
lark-cli minutes minutes get \
  --params '{"minute_id":"minute_xxx"}' --as user -q '.minute.ai_chapters'

# ========== 下载音视频 ==========

# 下载单个妙记的音视频（默认保存到当前目录）
lark-cli minutes +download --minute-tokens "minute_xxx" --as user

# 下载并指定保存路径
lark-cli minutes +download --minute-tokens "minute_xxx" \
  --output ./recordings/meeting.mp4 --as user

# 下载多个妙记（批量，保存到目录）
lark-cli minutes +download \
  --minute-tokens "minute_x1,minute_x2,minute_x3" \
  --output ./meetings/ --as user

# 强制覆盖已存在的文件
lark-cli minutes +download --minute-tokens "minute_xxx" \
  --output ./meeting.mp4 --overwrite --as user

# 仅获取下载 URL（不实际下载）
lark-cli minutes +download --minute-tokens "minute_xxx" --url-only --as user

# ========== 从 URL 提取妙记信息 ==========

# 假设妙记 URL 为 https://xxx.feishu.cn/minutes/MINUTE_TOKEN
# 提取 MINUTE_TOKEN 后即可使用
MINUTE_TOKEN="从URL中提取的token"

# 获取信息
lark-cli minutes minutes get \
  --params "{\"minute_id\":\"$MINUTE_TOKEN\"}" --as user --format pretty

# 下载录音
lark-cli minutes +download --minute-tokens "$MINUTE_TOKEN" \
  --output ./$MINUTE_TOKEN.mp4 --as user
```

---

### 审批 approval

审批实例和审批任务管理。

API 资源：`instances`（get/cancel/cc）、`tasks`（query/approve/reject/transfer）

```bash
# 查询审批任务
lark-cli approval tasks query

# 同意审批
lark-cli approval tasks approve --params '{"approval_code":"xxx","instance_id":"xxx","task_id":"xxx"}'

# 拒绝审批
lark-cli approval tasks reject --params '{"approval_code":"xxx","instance_id":"xxx","task_id":"xxx"}'

# 查看审批实例
lark-cli approval instances get --params '{"approval_code":"xxx","instance_id":"xxx"}'

# 撤回审批
lark-cli approval instances cancel --params '{"approval_code":"xxx","instance_id":"xxx"}'
```

---

### 事件订阅 event

通过 WebSocket 实时监听飞书事件。

| 快捷命令 | 说明 |
|---------|------|
| `+subscribe` | 订阅飞书事件（WebSocket，NDJSON 输出） |

```bash
# 订阅所有事件
lark-cli event +subscribe

# 订阅特定类型事件
lark-cli event +subscribe --event-type "im.message.receive_v1"

# 正则路由过滤
lark-cli event +subscribe --filter 'im\.message\.receive_v1'
```

---

## Schema 自省

使用 `schema` 命令查看任意 API 方法的参数结构、请求体、响应格式、支持身份和所需 scope：

```bash
# 查看所有可用的 schema
lark-cli schema

# 查看指定方法的 schema
lark-cli schema calendar.events.instance_view
lark-cli schema im.messages.create
lark-cli schema sheets.spreadsheets.get
```

在调用 Raw API 或不熟悉的 API 命令前，建议先用 schema 了解参数要求。

---

## 健康检查

```bash
# 完整健康检查（含网络连通性）
lark-cli doctor

# 仅检查本地状态（跳过网络检查）
lark-cli doctor --offline
```

---

## AI Agent Skills

lark-cli 配套 20 个 AI Agent Skills，指导 AI 代理正确使用 lark-cli。Skills 不是可执行代码，而是 SKILL.md 指令文档。

### 安装 Skills

```bash
# 安装全部 Skills
npx skills add larksuite/cli -y -g

# 仅安装特定领域
npx skills add larksuite/cli -s lark-calendar -y
```

### Skills 列表

| Skill | 说明 |
|-------|------|
| `lark-shared` | 基础：配置初始化、认证登录、身份切换、权限管理、安全规则（所有 skill 自动加载） |
| `lark-calendar` | 日历日程、议程、忙闲查询、时间建议 |
| `lark-im` | 消息发送/回复、群聊管理、消息搜索、媒体上传下载、表情回复 |
| `lark-doc` | 文档创建/读取/更新/搜索（基于 Markdown） |
| `lark-drive` | 文件上传下载、权限与评论管理 |
| `lark-sheets` | 电子表格创建/读写/追加/查找/导出 |
| `lark-base` | 多维表格、字段、记录、视图、仪表盘、数据聚合分析 |
| `lark-task` | 任务、任务清单、子任务、提醒、成员分配 |
| `lark-mail` | 邮件浏览/搜索/阅读/发送/回复/转发、草稿管理、监听新邮件 |
| `lark-contact` | 用户搜索、获取用户信息 |
| `lark-wiki` | 知识空间、节点、文档 |
| `lark-event` | 实时事件订阅（WebSocket）、正则路由、Agent 友好格式 |
| `lark-vc` | 搜索会议记录、会议纪要产物（总结/待办/逐字稿） |
| `lark-whiteboard` | 画板/图表 DSL 渲染（需 `@larksuite/whiteboard-cli`） |
| `lark-minutes` | 妙记元数据与 AI 产物 |
| `lark-openapi-explorer` | 从官方文档探索底层 API |
| `lark-skill-maker` | 自定义 Skill 创建框架 |
| `lark-approval` | 审批任务管理 |
| `lark-workflow-meeting-summary` | 工作流：会议纪要汇总与结构化报告 |
| `lark-workflow-standup-report` | 工作流：日程待办摘要 |

---

## 安全须知

> 使用前必读

1. **工具风险**：本工具可被 AI Agent 调用自动操作飞书开放平台，存在模型幻觉、执行不可控、提示词注入等固有风险
2. **授权范围**：授权飞书权限后，AI Agent 将以您的用户身份在授权范围内执行操作
3. **安全防护**：工具默认启用多层安全保护（输入防注入、终端输出净化、OS 密钥链存储凭证），不建议修改默认安全配置
4. **使用建议**：建议将对接本工具的飞书机器人作为私人对话助手使用，不要拉入群聊或允许其他用户交互
5. **Dry Run**：执行可能有副作用的操作前，始终先用 `--dry-run` 预览
6. **凭证安全**：应用密钥使用 OS 原生密钥链加密存储，配置文件位于 `~/.lark-cli/`
