# dsh-playwright-mcp

[DSH 插件导航：插件集合、Playwright MCP、使用计时](https://github.com/CQin04/awesome-dsh-plugin/blob/main/NAVIGATION.md)


[DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)（DSH）的**配置型插件**：把 Microsoft 官方 [`@playwright/mcp`](https://github.com/microsoft/playwright-mcp) 以 **Chrome 扩展模式（`--extension`）** 接入 DSH，让智能体直接操作你已登录的 Chrome 标签页——不另开浏览器、不丢登录态、不发账号密码。

A configuration-only DSH bundle that connects Microsoft's official `@playwright/mcp` to your running Chrome through the Playwright browser extension.

## 它是什么

- 浏览器自动化引擎是微软官方 `@playwright/mcp`，DSH 侧用内置 MCP 客户端 `@deepseek-ai/dsh-mcp-client` 启动它
- 整个插件**只有配置文件**（`package.json` + `cordis.patch.yml` + locale/icon），没有任何第三方代码
- 安装后智能体获得 `mcp__playwright__browser_*` 工具：导航、点击、输入、页面快照、截图、控制台/网络、标签页管理等

## 前置条件

- DSH（支持 `dsh.bundle.patch` 插件包的版本）
- Node.js（提供 `npx`）
- Chrome + Playwright 扩展（Chrome 应用商店搜索 "Playwright Extension"，扩展 ID `mmlmfjhmonkocbjadbfplnigmagldckm`）
- 扩展的访问 token

## 获取 token

1. 安装扩展后打开它的欢迎/状态页（扩展详情里有入口，或点扩展图标）
2. 页面上会显示一行 `PLAYWRIGHT_MCP_EXTENSION_TOKEN=...`
3. 复制该值

## 安装

1. 下载或 clone 本仓库（只有配置文件，无需构建、无需 npm install）
2. 打开 [`cordis.patch.yml`](cordis.patch.yml)，把 `REPLACE_WITH_YOUR_EXTENSION_TOKEN` 替换成你的 token
3. 在 DSH 里安装：**设置 → 插件 → 安装本地插件包**，选择本目录；或用插件管理器指向该目录
4. 安装后工具列表出现 `mcp__playwright__browser_*`；首次使用时自动连接你的 Chrome
5. 保持 Chrome 运行、扩展的 connect 页标签不要关（关掉连接会断，重新点扩展图标可重连）

## 配置说明

- 默认命令为 Windows 形式：`cmd.exe /c npx -y @playwright/mcp@latest --extension`（Windows 下裸 `npx` 无法直接 spawn）
- macOS / Linux：改成 `command: npx`，`args: ['-y', '@playwright/mcp@latest', '--extension']`
- 不想用扩展、想用独立/无头 Chromium：删掉 `--extension` 和 `env`，按 `@playwright/mcp` 文档配置，并先执行 `npx playwright install chromium`
- `toolCallTimeoutMs: 120000`：长页面/大操作留足超时
- `failOnStartupError: false`：MCP 起不来时降级为"没有这些工具"，而不是阻止 DSH 启动

## 安全

- **token 等同浏览器控制权**，请勿提交到公开仓库或发给他人
- 本仓库不包含任何真实 token / 账号数据，`cordis.patch.yml` 里是占位符
- 建议把填好 token 的配置副本放在私有位置（或安装后再改本地副本）

## 目录结构

```
dsh-playwright-mcp/
├── package.json
├── cordis.patch.yml   # 唯一需要改的文件（填 token）
├── icon.svg
├── locale/
│   ├── zh.json
│   └── en.json
├── README.md
└── LICENSE
```

## 许可

MIT
