# AGENTS.md —— 在这个仓库里干活前先读这个

这是 **MCP 服务器**（TypeScript），消费端。模组那一半（读游戏、出快照、本地 HTTP）在
[Craft-Graph](https://github.com/Chemiofitor4096/Craft-Graph)，两边只通过协议通信。

## 规则

- **输出会进模型上下文**：改了渲染/格式就跑 `npm run measure`，看 token 涨没涨。
- **不要手工重算工具已经算过的东西**：配方树/产线由算法层出，工具说「未知」就是未知。
- **`opaque` 不等于「没有」**：读不懂的配方不能当成「不需要原料 / 不产出」。
- **基础原料汇总是按当前选路的估算**：每个物品只选一条配方，换配方/换标签数字就变，
  它不是「这个配方的直接原料」。
- **工具名/行为变了要同步 `skill/craftgraph/SKILL.md`**：skill 是模型的工作流，
  和工具脱节比没有 skill 更糟。
- **协议字段名不要单方面改**：`protocolVersion` 是两侧唯一的兼容判据，改字段 = 破坏性变更。

## 测试

```bash
npm run typecheck
npm run smoke     # 算法层，不需要游戏
npm run e2e       # 真实 stdio 握手 + 工具调用
npm run contract  # 吃 Java 侧真实输出，需要 fixtures/bridge-dump/
npm run live      # 对运行中的游戏体检（需要 Minecraft 已进世界）
```

`fixtures/bridge-dump/` 由 bridge 仓库的 `ContractDumpTest` 生成（那边 `./gradlew test`），
**不入库** —— 陈旧样本会让契约测试假通过。

## 提交

Conventional Commits，中文摘要 + bullet body。
