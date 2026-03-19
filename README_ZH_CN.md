# BitMart Skills

[English](README.md) | [中文](README_ZH_CN.md)

BitMart Skills 是一个开放的 Skills 市场，赋予 AI Agent 原生访问 BitMart 加密货币生态系统的能力。从现货交易、合约持仓到深度市场分析 —— 全部通过自然语言完成。

由 BitMart 构建，为加密社区而生。

这些 Skills 兼容任何 AI Agent 框架。无论你使用 Claude Code、Codex、OpenClaw、LangChain、CrewAI 还是自有框架，你的 Agent 都能轻松接入 BitMart 的加密货币智能服务。

---

## Skills 总览

| Skill | 描述 | 版本 | 状态 |
|-------|------|------|------|
| [bitmart-exchange-spot](#-bitmart-exchange-spot) | 现货交易：买卖、订单管理、账户查询、保证金交易 | `2026.3.13` | ✅ 可用 |
| [bitmart-exchange-futures](#-bitmart-exchange-futures) | USDT 永续合约：开平仓、止盈止损、计划委托、杠杆管理 | `2026.3.13` | ✅ 可用 |
| [bitmart-wallet-ai](#-bitmart-wallet-ai) | Web3 钱包：代币搜索、行情数据、聪明钱追踪、地址余额查询、地址近期交易、兑换报价（无需 API Key） | `2026.3.17` | ✅ 可用 |

---

## 💱 bitmart-exchange-spot

> **Path**: `skills/bitmart-exchange-spot/`

BitMart 现货交易，涵盖买卖（市价单和限价单）、批量下单、订单管理（撤单/查询）、账户余额查询、保证金交易和费率查询。所有下单操作均需用户明确确认。

**示例提示词**：
- `帮我在 BitMart 买入 100 USDT 的 BTC`
- `挂一个限价卖单，0.5 ETH 价格 4000 USDT`
- `查看我的 BitMart 现货余额`
- `撤销我在 BTC_USDT 的所有挂单`
- `查询我在 BitMart 最近的成交记录`

---

## 📊 bitmart-exchange-futures

> **Path**: `skills/bitmart-exchange-futures/`

BitMart USDT 永续合约交易。支持开仓/平仓、杠杆管理、计划委托（条件单）、止盈止损、追踪委托和持仓模式切换。

**示例提示词**：
- `在 BitMart 开多 BTC 10 张合约，20 倍杠杆`
- `平掉我所有 ETH 空头仓位`
- `给我的 BTC 仓位设置止盈 70000、止损 58000`
- `设置一个计划委托，ETH 跌到 3000 时买入`

---

## 💰 bitmart-wallet-ai

> **Path**: `skills/bitmart-wallet-ai/`

BitMart Web3 钱包能力，覆盖 7 条链（Solana、BSC、Ethereum、Arbitrum、Polygon、Optimism、Base）。支持代币搜索、链详情、K 线图表、热门代币和美股映射代币排行、聪明钱盈亏排行及地址分析、地址余额查询、**地址近期交易**、代币兑换报价和批量价格查询。**无需 API Key** —— 所有接口均支持直接 HTTP POST 请求。

**示例提示词**：
- `查询 Solana 上 TRUMP 的价格`
- `显示近 7 天盈利最高的聪明钱地址`
- `查询地址 0x4396... 在 BSC 上的代币余额`
- `查询地址 2h4hhjuWxEo4uyzGAxzWvpdSotAozchjpfyefvVWvi8R 在 Solana 上的近期交易`
- `在 Solana 上用 100 USDT 能换多少 SOL？`
- `最近 24 小时的热门代币有哪些？`

---

## 快速开始

### 前置条件

- 支持 Skill 加载的 AI Agent 环境（如 Claude Code、Codex、OpenClaw）
- BitMart API 凭证（交易类 Skill 需要）：[创建 API Key](https://www.bitmart.com/zh-CN/api-config)

### API 密钥配置

1. 登录 [BitMart](https://www.bitmart.com)
2. 前往 **API 管理** → **创建 API Key**
3. 设置权限：**只读** + **现货交易**（和/或 **合约交易**）
4. 保存你的 **API Key**、**Secret Key** 和 **Memo**
5. 安全存储凭证（切勿在聊天中分享）

### 快速开始

用自然语言向你的 AI Agent 提问任何市场或交易问题：

```text
查询 BTC 在 BitMart 的当前价格
```

---

## Skills 安装指南

### 通用 Skills 安装（推荐）

1. 确认已安装 `npx`：
   ```bash
   npx -v
   ```

2. 交互式选择安装 Skills：
   ```bash
   npx skills add https://github.com/bitmartexchange/bitmart-skills
   ```

3. 安装指定 Skill：
   ```bash
   npx skills add https://github.com/bitmartexchange/bitmart-skills --skill bitmart-exchange-spot
   ```

### 在 Claude Code 中安装

#### 方式一：自然语言安装（推荐）

```text
帮我安装 skills，GitHub 地址是：https://github.com/bitmartexchange/bitmart-skills
```

#### 方式二：手动安装

1. 从 GitHub 下载 Skills 包
2. 将 Skill 文件夹复制到 `~/.claude/skills/`
3. 验证：在 Claude Code 中运行 `/skills`

### 在 Codex CLI 中安装

#### 方式一：自然语言安装（推荐）

```text
帮我安装 skills，GitHub 地址是：https://github.com/bitmartexchange/bitmart-skills
```

#### 方式二：手动安装

1. 从 GitHub 下载 Skills 包
2. 将 Skill 文件夹复制到 `~/.codex/skills/`
3. 重启 Codex 并通过 `/skills` 验证

### 在 OpenClaw 中安装

#### 方式一：通过对话安装（推荐）

```text
帮我安装这个 Skill：https://github.com/bitmartexchange/bitmart-skills
```

#### 方式二：手动安装

1. 从 GitHub 下载 Skills 包
2. 将 Skill 文件夹复制到 `~/.openclaw/skills/`
3. 重启 OpenClaw Gateway

---

## 仓库结构

```
bitmart-skills/
├── README.md                              # 英文 README
├── README_ZH_CN.md                        # 中文 README
├── .gitignore
└── skills/
    ├── bitmart-exchange-spot/             # 现货交易 Skill
    │   ├── SKILL.md
    │   ├── README.md
    │   └── references/
    │       ├── api-reference.md
    │       ├── authentication.md
    │       └── scenarios.md
    ├── bitmart-exchange-futures/          # 合约交易 Skill
    │   ├── SKILL.md
    │   ├── README.md
    │   └── references/
    │       ├── api-reference.md
    │       ├── open-position.md
    │       ├── close-position.md
    │       ├── plan-order.md
    │       └── tp-sl.md
    └── bitmart-wallet-ai/                 # Web3 钱包 Skill（无需 API Key）
        ├── SKILL.md
        └── README.md
```

---

## 关于本仓库

每个 Skill 位于 `skills/` 目录下的独立文件夹中，包含：

- **`SKILL.md`** — Skill 定义文件，包含 YAML 前置元数据（名称、版本、描述、触发词）和结构化指令（包括路由规则和工作流）。这是 AI Agent 读取并执行 Skill 的核心文件。
- **`references/`** — 详细的子模块文档，包含分步工作流、API 调用规范和报告模板。
- **`README.md`** — 面向开发者的 Skill 说明文档。

---

## 贡献指南

欢迎贡献！添加新 Skill 的步骤：

1. **Fork 本仓库**并创建新分支：
   ```bash
   git checkout -b feature/<skill-name>
   ```

2. 在 `skills/` 下**创建新文件夹**，至少包含一个 `SKILL.md` 文件。

3. **遵循规定格式**：
   ```markdown
   ---
   name: <skill-name>
   description: "清晰描述该 Skill 的功能和触发条件。"
   homepage: "https://www.bitmart.com"
   metadata: {"author":"bitmart","version":"<version>","sdk_version":"1.4.0","updated":"<YYYY-MM-DD>"}
   ---

   # <Skill 标题>

   [在此添加指令、路由规则、工作流和报告模板]
   ```

4. 在 `references/` 子文件夹中**添加参考文档**，用于复杂的子模块。

5. 向 `master` 分支**提交 Pull Request** 进行审核。

---

## 免责声明

BitMart Skills 仅供参考之用。所有内容和输出均按"现状"和"可用"的基础提供，不附带任何明示或暗示的陈述、保证或担保。BitMart Skills 生成的任何内容均不应被视为投资、财务、法律、税务、交易或其他专业建议，本文件中的任何内容均不构成购买、出售或持有任何数字资产或金融产品的要约、招揽或推荐。这些技能仅限于基于可用数据的只读分析，不执行交易或账户操作。所有加密货币投资（包括收益）均具有高度投机性质，涉及重大损失风险。过去的、假设的或模拟的表现并不一定预示未来的结果。数字货币的价值可能上升或下降，买卖、持有或交易数字货币存在重大风险。您应根据个人投资目标、财务状况和风险承受能力，仔细考虑交易或持有数字货币是否适合您。
