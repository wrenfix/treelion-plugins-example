# TreeLion Harness 插件示例

这是 TreeLion Harness 的公开插件示例仓库，包含两个自包含插件。

| 插件 | 内容 | 配置 |
| --- | --- | --- |
| `code-review` | 代码审查 Skill 与审查清单 | 无需参数，可以直接安装 |
| `team-docs` | 文档编写 Skill 与 HTTP MCP | 文档空间、输出语言与个人访问令牌 |

## 登记与使用

1. 在 Harness 管理后台的“插件管理 → 来源”添加插件源，地址填写 `https://github.com/wrenfix/treelion-plugins-example.git`，其余留空即可（公开仓库、跟随默认分支 `main`、仓库根目录）。保存后立即同步，预期发现两个插件。
2. 在“插件管理 → 插件”打开两个插件的“开放”开关；需要限制范围时选择用户组。开放后授权用户即可在桌面客户端的插件中心看到示例。
3. 可以直接安装“代码审查”。“团队文档”的 `https://docs.example.com/mcp` 是占位地址；使用前 Fork 或复制本仓库，替换 `plugins/team-docs/plugin.json` 中的地址为真实文档 MCP 服务，再登记自己的仓库。
4. 安装“团队文档”时，在桌面客户端填写文档空间、输出语言和个人访问令牌，通过检查后完成安装。真实令牌只在客户端填写，不要提交到 Git。

克隆示例：

```sh
git clone https://github.com/wrenfix/treelion-plugins-example.git
cd treelion-plugins-example
```

## 插件结构与参数

仓库根目录单插件放 `plugin.json`，多插件放 `plugins/<目录>/plugin.json`。

`name` 是仓库内稳定名称，`displayName` 和 `description` 用于展示；可选 `version` 只作展示标签，未填写时显示提交号。技能自动发现 `skills/*/SKILL.md`，附件与插件完整打包。所有参数统一在顶层 `configFields` 声明，支持文本和密钥；MCP 只在 `mcpServers` 中引用参数。普通参数通过 `${config.KEY}` 供 Skill 正文和 MCP 使用，密钥通过 `${secret.KEY}` 供 MCP 使用，个人值只在客户端填写。

`code-review` 演示无参数 Skill，`team-docs` 演示文档空间、输出语言和令牌供 Skill 与 MCP 共用。客户端填写并检查通过后才安装，密钥可保留、更换或明确清除。

## 版本与更新

推送到 Git 后后台自动同步，插件内容变化自动形成新版本，插件始终指向最近一次收录的内容；回退提交会重新指向旧版本。无需 Tag、发布目录或手工发版。用户安装即订阅：兼容的改动在新建会话前自动生效，新增参数或改变 MCP 地址、认证方式时用户会看到“需要处理”，补填后生效。已有会话固定安装时的版本与参数。

本仓库不包含或部署文档 MCP 服务。同步与预览不执行任何程序。
