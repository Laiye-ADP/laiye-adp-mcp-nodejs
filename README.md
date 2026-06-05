# Laiye ADP MCP server for Node.js

这个包把 Laiye ADP 的文档抽取和文档解析能力封装成 MCP tools，方便 Agent 直接调用。

当前只暴露 `/v1/app/doc` 相关能力：

- 预置文档抽取应用
- 预置文档解析应用
- 异步任务结果查询
- 自定义抽取应用列表与执行

## 环境要求

- Node.js 20+
- npm 10+

## 安装

```bash
npm install
npm run build
```

## 环境变量

```bash
ADP_API_KEY=your-adp-api-key
```

`ADP_ACCEPT_LANGUAGE` 默认是 `zh`。
MCP server 内部固定使用租户 `laiye`。
接口域名按语言自动选择：`zh` 使用 `https://adp.laiye.com`，`en` 使用 `https://adp-global.laiye.com`。

## 启动

```bash
npm start
```

## Agent MCP 配置

```json
{
  "mcpServers": {
    "laiye-adp": {
      "command": "node",
      "args": ["D:/playground/docs/laiye-adp-mcp-nodejs/dist/index.js"],
      "env": {
        "ADP_API_KEY": "your-adp-api-key"
      }
    }
  }
}
```

## 工具列表

- `extract_china_invoice`：中国票据
- `extract_vehicle_cert`：车辆合格证
- `extract_account_permit`：开户许可证
- `extract_driver_license`：驾驶证
- `extract_business_license`：营业执照
- `extract_passport_cn`：护照-中国
- `extract_vehicle_license`：行驶证
- `extract_org_code_cert`：组织机构代码证
- `extract_id_card`：身份证
- `extract_household_book`：户口本
- `extract_bank_card`：银行卡
- `extract_hk_macao_permit`：港澳通行证
- `extract_purchase_order`：订单
- `parse_document`：通用文档解析
- `extract_global_invoice`：海外发票/收据
- `extract_global_invoice_fast`：海外发票/收据（高速模式）
- `extract_sea_invoice_fast`：海外发票/收据（高速模式-东南亚版）
- `query_task`：查询任务状态
- `get_result`：获取任务结果
- `list_custom_extract_apps`：列出自定义抽取应用
- `execute_custom_extract_app`：执行自定义抽取应用

预置工具不会暴露 `app_id`，每个工具内部固定对应的应用 ID。
自定义抽取应用可以先用 `list_custom_extract_apps` 查询可用应用，再用 `execute_custom_extract_app` 传入 `app_id` 执行。
传 `wait=true` 时同步处理并返回结果；传 `wait=false` 时创建异步任务并返回 `task_id`。
