# whispering233-marketplace

一个面向本地开发环境的插件 marketplace 仓库，用来同时给 Codex 和 Claude Code 提供可复用插件。

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
      README.md
      references/
      skills/
      templates/
    pop-music-studio/
      .codex-plugin/plugin.json
      .claude-plugin/plugin.json
      README.md
      references/
      skills/
      templates/
```

## 当前插件

当前仓库包含 2 个插件：`structured-design-doc`、`pop-music-studio`。

### `structured-design-doc`

用途：把后端设计文档约束成稳定、可评审、可复用的结构化输出，而不是一次性自由发挥的说明文。

当前版本支持 3 类文档：

- 设计方案 / 技术方案 / 系统设计文档
- 关系型数据库表设计文档
- HTTP / REST API 设计文档

插件内包含的能力：

- `design-doc-router`
  - 路由技能。判断请求应该进入设计方案、数据库设计、API 设计，还是按文档类型拆分输出。
- `design-solution`
  - 输出结构化设计方案文档，覆盖方案概览、子模块或子流程展开、约束、边界、接口和调试要求。
- `db-table-design`
  - 输出结构化数据库表设计文档。
- `api-design-spec`
  - 输出结构化 REST API 设计文档。
- `references/shared-design-rules.md`
  - 共享写作约束，保证不同设计文档的结构和表达风格一致。
- `templates/design-solution-template.md`
  - 设计方案模板。
- `templates/db-table-design-template.md`
  - 数据库表设计模板。
- `templates/api-design-spec-template.md`
  - API 设计模板。

适合的典型场景：

- 为新模块写技术方案或系统设计文档
- 为新业务模块编写数据库表设计
- 为新接口或一组接口编写 REST API 设计文档
- 把同一轮后端需求拆成方案设计、数据库设计、API 设计三部分分别输出
- 希望团队成员输出风格统一，减少评审时的格式噪音

### `pop-music-studio`

用途：把流行音乐创作过程拆成稳定的多阶段流水线，从歌词、歌曲结构、编曲、演唱设定，一直延伸到封面和 MV 方案，避免一次性把所有创意揉成一段不可复用的自由发挥。

当前版本支持 6 类创作阶段：

- 歌词创作
- 歌曲结构与旋律方向
- 编曲与制作方向
- 演唱与和声设定
- 封面设计
- MV 概念与镜头规划

插件内包含的能力：

- `music-pipeline-router`
  - 路由技能。判断请求应该进入歌词、歌曲、编曲、演唱、封面、MV，还是拆成完整流水线输出。
- `lyrics-writing`
  - 输出结构化歌词创作结果，包含主题、标题、hook、分段歌词和修改备注。
- `song-composition`
  - 输出结构化歌曲方案，包含曲式、节奏、调性、hook 与分段旋律方向。
- `arrangement-production`
  - 输出结构化编曲与制作说明，包含乐器、groove、分段能量和制作交接说明。
- `vocal-performance`
  - 输出结构化演唱说明，包含主唱音色、分段唱法、和声、ad-lib 与录音处理建议。
- `cover-art-design`
  - 输出结构化封面方向，包含视觉概念、构图、字体、配色和图像生成提示。
- `mv-generation`
  - 输出结构化 MV 方案，包含概念、场景、镜头、表演方向和剪辑节奏。
- `references/shared-pipeline-rules.md`
  - 共享创作约束，保证音乐和视觉阶段围绕同一份 brief 展开。
- `templates/`
  - 六个阶段对应的独立模板。

适合的典型场景：

- 从一句灵感出发，拆成完整的流行音乐创作流水线
- 先写歌词，再逐步扩展到歌曲结构和编曲说明
- 为已有歌曲补充演唱、封面和 MV 的统一创意包
- 需要让不同阶段的输出保持同一个主题、人设和情绪方向

## 当前状态

当前 marketplace 元数据中已经注册了 `structured-design-doc` 和 `pop-music-studio`：

- Codex marketplace：`.agents/plugins/marketplace.json`
- Claude Code marketplace：`.claude-plugin/marketplace.json`

插件自身也已经同时提供双平台元数据：

- Codex 清单：`plugins/structured-design-doc/.codex-plugin/plugin.json`
- Codex 清单：`plugins/pop-music-studio/.codex-plugin/plugin.json`
- Claude Code 清单：`plugins/structured-design-doc/.claude-plugin/plugin.json`
- Claude Code 清单：`plugins/pop-music-studio/.claude-plugin/plugin.json`

## 在 Codex 中安装和使用

本仓库已经符合 Codex 的本地 marketplace 约定。

重点说明：

- 不需要先把插件复制到 `~/.codex/plugins` 之类的固定目录。
- 你可以直接从当前目录使用 marketplace。
- Codex 最终依据的是 `.agents/plugins/marketplace.json` 中的 `source.path`，并按 marketplace 根目录解析该路径。

### 直接从当前目录使用

1. 克隆本仓库到本地。
2. 保持当前目录结构不变，尤其不要改掉：
   - `.agents/plugins/marketplace.json`
   - `plugins/structured-design-doc/.codex-plugin/plugin.json`
3. 在这个仓库根目录打开或重启 Codex。
4. 打开 Codex 的 Plugin Directory。
5. 从 marketplace `whispering233-marketplace` 安装并启用 `structured-design-doc`。

### Codex 中如何使用

安装后，直接在会话中描述你的需求即可。推荐从路由技能对应的自然语言请求开始，例如：

```text
请为订单履约系统写一份结构化技术方案文档。
```

```text
请为优惠券系统写一份结构化数据库表设计文档。
```

```text
请为订单查询接口写一份结构化 REST API 设计规范。
```

```text
请把这个后端需求拆成设计方案、数据库设计和 API 设计三部分，并分别按固定模板输出。
```

## 在 Claude Code 中安装和使用

本仓库同时提供了 Claude Code 所需的仓库级 marketplace 元数据和插件级元数据：

- 仓库级 marketplace：`.claude-plugin/marketplace.json`
- 插件级清单：`plugins/structured-design-doc/.claude-plugin/plugin.json`

重点说明：

- Claude Code 支持直接从本地目录添加 marketplace。
- 不需要先把 marketplace 或插件复制到某个固定系统目录。

### 安装步骤

1. 安装 Claude Code：

```bash
npm install -g @anthropic-ai/claude-code
```

2. 克隆本仓库到本地，保留当前目录结构不变。
3. 进入仓库根目录后执行：

```bash
/plugin marketplace add .
```

也可以显式传入目录：

```bash
/plugin marketplace add ./whispering233-marketplace
```

4. 安装插件：

```bash
/plugin install structured-design-doc@whispering233-marketplace
```

### Claude Code 中如何使用

安装后可直接使用自然语言触发插件工作流，例如：

```text
请为支付结算模块写一份结构化设计方案文档，包含目标、模块拆分、上下游边界、接口和调试要求。
```

```text
请为积分账户系统写一份结构化数据库表设计文档，包含字段、类型、索引、约束和设计说明。
```

```text
请为用户资料管理写一份 REST API 设计文档，包含路径、参数、响应、错误码和边界条件。
```

```text
请把这个需求拆成设计方案、数据库表设计和 REST API 设计三个部分，并严格按固定模板输出。
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
- Codex 插件构建文档：[Build plugins - Codex](https://developers.openai.com/codex/plugins/build)
- Claude Code 安装文档：[Set up Claude Code](https://docs.anthropic.com/en/docs/claude-code/setup)
- Claude Code marketplace 文档：[Plugin Marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
