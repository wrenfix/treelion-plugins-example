# TreeLion Harness 插件示例

这是 TreeLion Harness 的公开插件示例仓库，包含两个使用第二版格式的自包含插件。

| 插件 | 内容 | 配置 |
| --- | --- | --- |
| `code-review` | 代码审查 Skill 与审查清单 | 无需参数，可以直接安装 |
| `team-docs` | 文档编写 Skill 与 HTTP MCP | 文档空间、输出语言与个人访问令牌 |

## 登记与使用

1. 在 Harness 管理后台的“插件仓库”新增来源，地址填写 `https://github.com/wrenfix/treelion-plugins-example.git`，认证选择公开仓库，目录留空。分支留空时跟随默认 `main`，也可指定 `main`。
2. 保存后等待同步完成，预期发现两个插件。新仓库默认隐藏；管理员设置仓库默认使用范围并开放后，授权用户才能在桌面客户端的插件中心看到示例。
3. 可以直接安装“代码审查”。“团队文档”的 `https://docs.example.com/mcp` 是占位地址；使用前 Fork 或复制本仓库，替换 `plugins/team-docs/plugin.json` 中的地址为真实文档 MCP 服务，再登记自己的仓库。
4. 安装“团队文档”时，在桌面客户端填写文档空间、输出语言和个人访问令牌，通过检查后完成安装。真实令牌只在客户端填写，不要提交到 Git。

克隆示例：

```sh
git clone https://github.com/wrenfix/treelion-plugins-example.git
cd treelion-plugins-example
```

## 插件结构与参数

仓库根目录单插件放 `plugin.json`，多插件放 `plugins/<目录>/plugin.json`。

清单使用 `schemaVersion: 2`，`name` 是仓库内稳定名称，`displayName` 和 `description` 用于展示；可选 `version` 只作标签。技能自动发现 `skills/*/SKILL.md`，附件与插件完整打包。所有参数统一在顶层 `configFields` 声明，首版支持文本和密钥；MCP 只在 `mcpServers` 中引用参数，不接受私有 `configFields`。普通参数通过 `${config.KEY}` 供 Skill 正文和 MCP 使用，密钥通过 `${secret.KEY}` 供 MCP 使用，个人值只在客户端填写。

`code-review` 演示无参数 Skill，`team-docs` 演示文档空间、输出语言和令牌供 Skill 与 MCP 共用。客户端填写和检查通过才安装，密钥可保留、更换或明确清除；保存仅对新建独立会话生效，旧会话及其分叉、子会话保留原配置。

提交 Git 后后台自动同步，内容变化自动留存不可变版本，无需发布目录或手工推荐版本。管理员在后台设置一次仓库默认使用范围；新仓库默认隐藏，插件可以继承或单独覆盖。

本仓库不包含或部署文档 MCP 服务。同步与预览不执行任何程序。
