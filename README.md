# CraftGraph MCP

**把运行中的 Minecraft 配方数据接到 AI 客户端上的 MCP 服务器。**

这是 [Craft-Graph](https://github.com/Chemiofitor4096/Craft-Graph) 的消费端：模组负责读游戏、出快照，
这里负责配方树、产线计算、缓存和 MCP 工具。两半通过本地 HTTP 协议通信（`protocolVersion` 是兼容判据）。

## 它是什么

- 输入：MCP（stdio），给 Claude Code / Cursor / DSH 这类客户端用
- 数据来源：本机运行中的 Minecraft（由 CraftGraph 模组暴露），游戏关着时退回磁盘快照
- 输出：配方树、产线规划、搜索结果，支持 markdown / tsv / json / html

## 安装

```bash
npm ci
npm run build
```

然后把客户端指向 `dist/index.js`（需要 Node ≥ 20）：

```jsonc
{
  "mcpServers": {
    "craftgraph": {
      "command": "node",
      "args": ["/绝对路径/Craft-Graph-MCP/dist/index.js"]
    }
  }
}
```

模组那一半（jar）在 [Craft-Graph releases](https://github.com/Chemiofitor4096/Craft-Graph/releases)。

## 工具

| 用途 | 工具 |
|---|---|
| 状态 / 覆盖度 | `get_bridge_status`、`refresh_recipes` |
| 查找 | `search_items`、`get_registry`、`list_recipe_types`、`expand_tag` |
| 配方 | `get_recipes_for_output`、`get_recipes_for_input`、`get_recipe_details`、`find_alternative_recipes` |
| 规划 | `build_recipe_tree`、`calculate_production_plan` |

不给 AI 客户端也行：`npm run plan -- --item create:brass_ingot --rate 100` 直接在终端出规划，
`--html` 生成自包含网页。

## Skill

`skill/craftgraph/SKILL.md` 是给 AI 客户端的工作流说明（先确认产量、怎么读 opaque/截断、不要手工重算）。
复制到客户端的 skills 目录即可。

## 开发

```bash
npm run typecheck   # 类型检查
npm run smoke       # 算法层，不需要游戏
npm run e2e         # MCP 协议层
npm run contract    # 跨语言契约，需要 fixtures/bridge-dump/
npm run live        # 对运行中的游戏体检
npm run measure     # 改了输出/渲染就跑，看 token 涨没涨
```

`fixtures/bridge-dump/` 由 bridge 仓库的 `ContractDumpTest` 生成（`./gradlew test`），
本仓库不提交它 —— 陈旧样本会让契约测试假通过。

## 许可

MIT
