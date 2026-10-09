# WorkBuddy 对话上下文 / workbuddy.md

本文件用于 **WorkBuddy 专项奖励** 联动活动提交，记录 MCTI 项目在 WorkBuddy 中的开发上下文、协作路径与产出，便于活动方核验「AI 协同开发」的真实性。

---

## 1. 项目概述

- **名称**：MCTI · 麦当劳人格类型指标（McDonald's Character Type Indicator）
- **形态**：单文件 Web 应用（`index.html`，零依赖、离线可用、移动端适配）
- **核心功能**：
  1. 人格测试结果页 —— 基于历史订单测出 16 型人格 + 四维剖面；
  2. 今日点餐推荐页 —— 按「人格 × 当前时段」从真实菜单生成组合。
- **视觉**：复古 / 开心乐园餐玩具风格（红黄奶油配色、粗黑描边贴纸卡、Memphis 点纹、拱门与玩具盒 SVG）。

## 2. 开发动机

作者希望把「日常麦当劳订单」变成一种可分享的趣味人格测评，同时验证 **WorkBuddy + mcd-mcp** 能否完成「真实数据读取 → 分析建模 → 可视化交付」的完整闭环，而非停留在演示脚本。

## 3. WorkBuddy 在开发中的角色

WorkBuddy 在本项目中承担**全流程协同**：

1. **能力探测**：通过 ToolSearch 检索 mcd-mcp 工具清单，确认 `order-list` / `query-meals` / `now-time-info` 可用，避免「空壳开发」。
2. **真实数据拉取**：经 DeferExecuteTool 调用上述 3 个 MCP Tool，获取 10 笔历史订单、深圳储能大厦店实时菜单、当前时间快照。
3. **建模与编码**：基于真实数据设计四维评分轴与 16 型人格模型，编写单文件应用逻辑。
4. **验证**：抽取脚本逻辑套 DOM 桩做 Node 冒烟测试，确认分析 / 推荐在真实数据上无报错。
5. **交付与文档**：补充 `README.md` 及本参赛包（`CONTEST_DECLARATION.md` / `MCP_INTEGRATION.md` / `mcp-config.example.json` / `workbuddy.md`）。

## 4. 关键对话节点（精选）

| 阶段 | 用户意图 | WorkBuddy 动作 | 产出 |
| --- | --- | --- | --- |
| 需求 | 用 mcd-mcp 做人格测评工具 | 探测 MCP 工具、拉真实数据 | 确认 `order-list` 等可用 |
| 数据 | 读取历史订单 + 菜单 + 时间 | 调用 3 个 MCP Tool | 10 笔订单 / 菜单快照 / 时段 |
| 建模 | 16 型人格 + 四维轴 | 设计模型并写 `index.html` | 单文件应用 |
| 验证 | 确认逻辑跑通 | Node 冒烟测试（DOM 桩） | 全绿，无报错 |
| 部署 | 可上传 GitHub 的文件 | 加 meta + `README.md` | 仓库就绪 |
| 参赛 | 配套声明与集成文档 | 产出本包 4 份文件 | 提交材料齐全 |

## 5. 实际使用的 MCP / 工具

- **MCP Server**：`mcd-mcp`（streamablehttp，端点 `https://mcp.mcd.cn`）
- **Tools（3 个，均为只读）**：
  - `order-list` —— 历史订单
  - `query-meals` —— 实时菜单（`storeCode:1420453, orderType:1, beType:1`）
  - `now-time-info` —— 当前时间快照
- **未使用**任何下单 / 支付 / 账户写操作接口。
- 完整调用流程、字段映射与业务价值见 `MCP_INTEGRATION.md`。

## 6. 联动活动说明

- 本作品为 **WorkBuddy 协同开发**产物，符合专项奖励「AI 辅助创作」的认定范畴。
- 随附 `CONTEST_DECLARATION.md` 声明原创性、合规性与敏感信息脱敏情况，防范直接盗用。
- 凭证与 PII 已脱敏：仓库不含任何 Bearer Token，订单数据仅保留 `date/hour/store/name/amt` 泛化字段。

## 7. 交付文件清单

```
mcti/
├── index.html                # 主应用（单文件，已验证）
├── README.md                 # 项目说明与部署
├── MCP_INTEGRATION.md        # MCP 集成与业务价值
├── mcp-config.example.json   # 脱敏 MCP 配置（仅环境变量占位符）
├── CONTEST_DECLARATION.md     # 参赛 / 原创 / 合规 / 敏感信息声明
├── workbuddy.md              # 本文件：WorkBuddy 对话上下文
└── LICENSE                   # MIT（建议补充）
```

## 8. 一句话总结

> 用 WorkBuddy 接上真实的麦当劳 MCP 数据，把一个「每天雷打不动一个麦满分」的订单习惯，变成了一张能发朋友圈的复古人格卡——数据是真的，笑点是真的，离线也能跑也是真的。
