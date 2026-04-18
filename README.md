# whispering233-marketplace

一个面向本地开发环境的插件市场仓库，用来同时给 Codex 和 Claude Code 提供可复用插件。

当前仓库已经按双平台 marketplace 结构组织：

- Codex marketplace 索引：`.agents/plugins/marketplace.json`
- Claude Code marketplace 索引：`.claude-plugin/marketplace.json`
- 插件目录：`plugins/<plugin-name>/`

## 仓库结构

```text
whispering233-marketplace/
  .agents/
    plugins/
      marketplace.json
  .claude-plugin/
    marketplace.json
  plugins/
    structured-design-doc/
      .codex-plugin/plugin.json
      .claude-plugin/plugin.json
      skills/
      templates/
      references/
      README.md
```

## 当前插件

### `structured-design-doc`

用途：把后端设计文档约束成稳定、可评审、可复用的结构化输出，而不是一次性自由发挥的说明文。

当前版本覆盖两类文档：

- 关系型数据库表设计文档
- HTTP / REST API 设计文档

插件内包含的能力：

- `design-doc-router`
  - 路由技能。先判断用户请求属于表设计、API 设计，还是两者都需要。
- `db-table-design`
  - 专门输出数据库表设计文档。
- `api-design-spec`
  - 专门输出 REST API 设计文档。
- `references/shared-design-rules.md`
  - 共享写作约束，保证不同设计文档的结构和表达风格一致。
- `templates/`
  - 独立模板文件，保证输出格式稳定，后续扩展新文档类型时也更容易维护。

适合的典型场景：

- 为新业务模块编写数据库表设计
- 为新接口或一组接口编写 REST API 设计文档
- 在同一轮需求里同时产出数据模型和 API 规范
- 希望团队成员输出风格统一，减少评审时的格式噪音

## 在 Codex 中安装和使用

本仓库已经符合 Codex 的 repo marketplace 约定。

重点说明：

- 现在不必先把插件复制到 `~/.codex/plugins` 这类固定目录。
- Codex 官方文档明确说明，示例里的插件目录只是常见写法，不是固定要求。
- Codex 实际依据的是 marketplace 文件中的 `source.path`，并且它是相对 marketplace 根目录解析的。

这意味着你可以直接把当前仓库当作 marketplace 仓库使用，只要目录里存在 `.agents/plugins/marketplace.json`，并且其中的 `source.path` 正确指向插件目录即可。

### 方式一：直接把本仓库作为 repo marketplace 使用

1. 克隆本仓库到本地。
2. 保持当前目录结构不变，尤其不要改掉：
   - `.agents/plugins/marketplace.json`
   - `plugins/structured-design-doc/.codex-plugin/plugin.json`
3. 直接在这个仓库根目录打开或重启 Codex。
4. 打开 Codex 的 Plugin Directory，选择 marketplace `whispering233-marketplace`。
5. 安装并启用 `structured-design-doc`。

这种方式就是“直接从指定目录使用 marketplace”，不需要先把插件复制到用户目录下的固定位置。

### 方式二：个人级 marketplace

如果你希望跨多个仓库复用这个插件，可以使用个人级 marketplace。这里也同样不要求必须复制到固定目录，关键仍然是 `~/.agents/plugins/marketplace.json` 里的 `source.path` 指向哪里。

1. 在 `~/.agents/plugins/marketplace.json` 中加入一个插件条目。
2. 让 `source.path` 指向你实际存放插件的目录。
3. 重启 Codex。
4. 在 Plugin Directory 中安装并启用 `structured-design-doc`。

个人级 marketplace 示例：

```json
{
  "name": "whispering233-marketplace",
  "interface": {
    "displayName": "whispering233-marketplace"
  },
  "plugins": [
    {
      "name": "structured-design-doc",
      "source": {
        "source": "local",
        "path": "./plugins/structured-design-doc"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

### Codex 中如何使用

安装后，直接在 Codex 会话中描述需求即可。推荐从路由技能对应的自然语言请求开始，例如：

```text
为一个订单系统写一份结构化数据库表设计文档。
```

```text
为用户资料管理写一份结构化 REST API 设计规范。
```

```text
把这个后端需求同时拆成数据库设计和 API 设计，两部分都按固定模板输出。
```

## 在 Claude Code 中安装和使用

本仓库同时提供了 Claude Code 所需的 marketplace 和插件元数据：

- 仓库级 marketplace：`.claude-plugin/marketplace.json`
- 插件级清单：`plugins/structured-design-doc/.claude-plugin/plugin.json`

### 安装步骤

1. 安装 Claude Code：

```bash
npm install -g @anthropic-ai/claude-code
```

2. 克隆本仓库到本地，保留当前目录结构不变。
3. 直接把这个目录作为 marketplace 加入 Claude Code：

```bash
/plugin marketplace add ./whispering233-marketplace
```

如果你当前终端已经在仓库根目录，也可以直接写：

```bash
/plugin marketplace add .
```

4. 从该 marketplace 安装 `structured-design-doc`：

```bash
/plugin install structured-design-doc@whispering233-marketplace
```

5. 启动一个新会话，开始直接描述你的数据库或 API 设计需求。

说明：

- 这个仓库已经具备 Claude Code marketplace 所需的双层元数据结构。
- Claude Code 官方文档已经明确给出“从本地目录添加 marketplace”的用法，即 `/plugin marketplace add ./my-marketplace`。
- 这意味着你不需要先把 marketplace 或插件复制到某个固定系统目录，再去安装。
- 如果你维护的是自己的 Claude Code marketplace 镜像，也可以直接复用本仓库里的 `.claude-plugin/marketplace.json` 和插件子目录结构。

### Claude Code 中如何使用

安装后可直接使用自然语言触发插件工作流，例如：

```text
请为优惠券系统写一份结构化数据库表设计文档，包含字段、类型、索引、约束和设计说明。
```

```text
请为订单查询接口写一份 REST API 设计文档，包含路径、参数、响应、错误码和边界条件。
```

```text
请把这个需求拆成数据库表设计和 REST API 设计两个部分，并严格按固定模板输出。
```

## 新增插件

如果你要往这个 marketplace 里继续添加插件，最少需要完成下面几件事：

1. 创建 `plugins/<your-plugin-name>/`。
2. 添加 Codex 清单：`plugins/<your-plugin-name>/.codex-plugin/plugin.json`。
3. 添加 Claude Code 清单：`plugins/<your-plugin-name>/.claude-plugin/plugin.json`。
4. 在 `.agents/plugins/marketplace.json` 中加入 Codex marketplace 条目。
5. 在 `.claude-plugin/marketplace.json` 中加入 Claude Code marketplace 条目。
6. 为插件补充自己的 `README.md`，说明用途、能力边界和使用示例。

## 参考文档

- Codex 插件文档：[Plugins - Codex](https://developers.openai.com/codex/plugins)
- Codex 插件构建与本地 marketplace 文档：[Build plugins - Codex](https://developers.openai.com/codex/plugins/build)
- Claude Code 安装文档：[Set up Claude Code](https://docs.anthropic.com/en/docs/claude-code/setup)
- Claude Code marketplace 文档：[Plugin Marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
