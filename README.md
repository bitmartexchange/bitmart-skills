# BitMart Skills

[English](README.md) | [中文](README_ZH_CN.md)

BitMart Skills is an open skills marketplace that empowers AI agents with native access to BitMart's cryptocurrency ecosystem. From spot trading and futures positions to deep market analysis — all through natural language.

Built by BitMart. Built for the crypto community.

These skills are designed to work with any AI agent framework. Whether you're using Claude Code, Codex, OpenClaw, LangChain, CrewAI, or your own stack, your agents can plug into BitMart's crypto intelligence with minimal configuration.

---

## Skills Overview

| Skill | Description | Version | Status |
|-------|-------------|---------|--------|
| [bitmart-exchange-spot](#-bitmart-exchange-spot) | Spot trading: buy/sell, order management, account queries, margin trading | `2026.3.23` | ✅ Active |
| [bitmart-exchange-futures](#-bitmart-exchange-futures) | USDT perpetual futures: open/close position, TP/SL, plan orders, leverage | `2026.3.23` | ✅ Active |
| [bitmart-wallet-ai](#-bitmart-wallet-ai) | Web3 Wallet: token search, market data, smart money tracking, address balances, recent transactions, swap quotes (no API key required) | `2026.3.23` | ✅ Active |

---

## 💱 bitmart-exchange-spot

> **Path**: `skills/bitmart-exchange-spot/`

Spot trading on BitMart covering buy/sell (market & limit), batch orders, order management (cancel/query), account balance queries, margin trading, and fee rate lookup. All order placements require explicit user confirmation.

**Example Prompts**:
- `Buy 100 USDT worth of BTC on BitMart`
- `Place a limit sell order for 0.5 ETH at 4000 USDT`
- `Check my BitMart spot balance`
- `Cancel all my open orders on BTC_USDT`
- `What are my recent trades on BitMart?`

---

## 📊 bitmart-exchange-futures

> **Path**: `skills/bitmart-exchange-futures/`

USDT perpetual futures trading on BitMart. Supports opening/closing positions, leverage management, plan (conditional) orders, take-profit/stop-loss, trailing orders, and position mode switching.

**Example Prompts**:
- `Long BTC 10 contracts with 20x leverage on BitMart`
- `Close all my ETH short positions`
- `Set take-profit at 70000 and stop-loss at 58000 for my BTC position`
- `Place a plan order to buy ETH when it drops to 3000`

---

## 💰 bitmart-wallet-ai

> **Path**: `skills/bitmart-wallet-ai/`

BitMart Web3 Wallet capabilities across 5 chains (Solana, BSC, Ethereum, Arbitrum, Base). Includes token search, chain details, K-line charts, hot token & xStock rankings, smart money P&L rankings and address analysis, address balance queries, **address recent transactions**, token swap quotes, and batch price queries. **No API key required** — all endpoints accept direct HTTP POST requests.

**Example Prompts**:
- `What's the price of TRUMP on Solana?`
- `Show me the top smart money wallets by 7-day profit`
- `Check token balances for address 0x4396... on BSC`
- `Check recent transactions for address 2h4hhjuWxEo4uyzGAxzWvpdSotAozchjpfyefvVWvi8R on Solana`
- `How much SOL do I get for swapping 100 USDT on Solana?`
- `What are the hot tokens in the last 24 hours?`

---

## Getting Started

### Prerequisites

- An AI agent environment that supports skill loading (e.g., Claude Code, Codex, OpenClaw)
- BitMart API credentials (for trading skills): [Create API Key](https://www.bitmart.com/en-US/api-config)

### API Key Setup

1. Log in to [BitMart](https://www.bitmart.com)
2. Navigate to **API Management** → **Create API Key**
3. Set permissions: **Read-Only** + **Spot Trade** (and/or **Futures Trade**)
4. Save your **API Key**, **Secret Key**, and **Memo**
5. Store credentials securely (never share in chat)

### Quick Start

Ask your AI agent any market or trading question in natural language:

```text
What's the current price of BTC on BitMart?
```

---

## Skills Installation Guide

### General Skills Installation (Recommended)

1. Verify `npx` is installed:
   ```bash
   npx -v
   ```

2. Install skills with interactive selection:
   ```bash
   npx skills add https://github.com/bitmartexchange/bitmart-skills
   ```

3. Install a specific skill:
   ```bash
   npx skills add https://github.com/bitmartexchange/bitmart-skills --skill bitmart-exchange-spot
   ```

### Install Skills in Claude Code

#### Option 1: Natural Language Installation (Recommended)

```text
Help me install skills, GitHub URL is: https://github.com/bitmartexchange/bitmart-skills
```

#### Option 2: Manual Installation

1. Download the skills package from GitHub
2. Copy skill folders to `~/.claude/skills/`
3. Verify: run `/skills` in Claude Code

### Install Skills in Codex CLI

#### Option 1: Natural Language Installation (Recommended)

```text
Help me install skills, GitHub URL is: https://github.com/bitmartexchange/bitmart-skills
```

#### Option 2: Manual Installation

1. Download the skills package from GitHub
2. Copy skill folders to `~/.codex/skills/`
3. Restart Codex and verify with `/skills`

### Install Skills in OpenClaw

#### Option 1: Install via Chat (Recommended)

```text
Help me install this skill: https://github.com/bitmartexchange/bitmart-skills
```

#### Option 2: Manual Installation

1. Download the skills package from GitHub
2. Copy skill folders to `~/.openclaw/skills/`
3. Restart OpenClaw Gateway

---

## Repository Structure

```
bitmart-skills/
├── README.md                              # English README
├── README_ZH_CN.md                        # Chinese README
└── skills/
    ├── bitmart-exchange-spot/             # Spot trading skill
    │   ├── SKILL.md
    │   ├── README.md
    │   └── references/
    │       ├── api-reference.md
    │       ├── authentication.md
    │       └── scenarios.md
    ├── bitmart-exchange-futures/          # Futures trading skill
    │   ├── SKILL.md
    │   ├── README.md
    │   └── references/
    │       ├── api-reference.md
    │       ├── open-position.md
    │       ├── close-position.md
    │       ├── plan-order.md
    │       └── tp-sl.md
    └── bitmart-wallet-ai/                 # Web3 Wallet skill (no API key required)
        ├── SKILL.md
        └── README.md
```

---

## About This Repository

Each skill lives in its own folder under `skills/` and contains:

- **`SKILL.md`** — Skill definition with YAML frontmatter (name, version, description, trigger phrases) and structured instructions including routing rules and workflows. This is the file that AI agents read to understand and execute the skill.
- **`references/`** — Detailed sub-module documentation with step-by-step workflows, API calling conventions, and report templates.
- **`README.md`** — Human-readable skill description for developers.

---

## Contribution

We welcome contributions! To add a new skill:

1. **Fork the repository** and create a new branch:
   ```bash
   git checkout -b feature/<skill-name>
   ```

2. **Create a new folder** under `skills/` containing at least a `SKILL.md` file.

3. **Follow the required format**:
   ```markdown
   ---
   name: <skill-name>
   description: "A clear description of what the skill does and when to trigger it."
   homepage: "https://www.bitmart.com"
   metadata: {"author":"bitmart","version":"<version>","sdk_version":"1.4.0","updated":"<YYYY-MM-DD>"}
   ---

   # <Skill Title>

   [Add instructions, routing rules, workflows, and report templates here]
   ```

4. **Add reference documents** in a `references/` subfolder for complex sub-modules.

5. **Open a Pull Request** to `master` for review.

---

## Disclaimer

BitMart Skills is provided for informational and reference purposes only. All content and outputs are made available on an “as is” and “as available” basis, without any express or implied representations, guarantees, or warranties. Nothing generated by BitMart Skills should be construed as investment, financial, legal, tax, trading, or other professional advice, and nothing herein constitutes an offer, solicitation, or recommendation to purchase, sell, or hold any digital asset or financial product. These skills are limited to read-only analysis based on available data and do not execute trades or perform account operations. All crypto investments, including earnings, are highly speculative in nature and involve substantial risk of loss. Past, hypothetical, or simulated performance is not necessarily indicative of future results. The value of digital currencies can go up or down and there can be a substantial risk in buying, selling, holding, or trading digital currencies. You should carefully consider whether trading or holding digital currencies is suitable for you based on your personal investment objectives, financial circumstances, and risk tolerance.
