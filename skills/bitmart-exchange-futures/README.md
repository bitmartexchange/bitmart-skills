# bitmart-exchange-futures

BitMart futures (contract) trading skill for AI agents.

## Capabilities

- **Market Data**: Contract details, order book depth, recent trades, K-line/candlestick, mark price K-line, funding rates (current and historical), open interest, leverage brackets
- **Account**: Futures balance, positions, position risk, position mode, transaction history, trade fee rate
- **Trading**: Open/close positions (market and limit), set leverage, set position mode, modify orders, batch cancel, timed cancel
- **Plan Orders**: Conditional/trigger orders that execute when price reaches a target
- **TP/SL**: Take-profit and stop-loss orders for automated risk management
- **Trailing Stops**: Trailing stop orders that follow the market price
- **Transfers**: Spot-to-futures and futures-to-spot fund transfers
- **System**: Server time, system service / maintenance status

## API Endpoints

| Category | Count | Auth |
|----------|-------|------|
| Public Market Data | 9 endpoints | NONE |
| Account / Positions | 7 endpoints | KEYED |
| Trading | 8 endpoints | SIGNED |
| Plan Orders | 3 endpoints | SIGNED |
| TP/SL | 3 endpoints | SIGNED |
| Trailing | 2 endpoints | SIGNED |
| Order Query | 6 endpoints | KEYED/SIGNED |
| Sub-Account | 6 endpoints | KEYED/SIGNED |
| Affiliate | 6 endpoints | KEYED |
| Simulated | 1 endpoint | SIGNED |
| System | 2 endpoints | NONE |

**Total: 53 endpoints**

## Authentication

Requires BitMart API credentials:
- `BITMART_API_KEY` — API key
- `BITMART_API_SECRET` — Secret key
- `BITMART_API_MEMO` — Memo passphrase

Set via environment variables or `~/.bitmart/config.toml`.

## Key Differences from Spot

| Aspect | Spot | Futures |
|--------|------|---------|
| Base URL | `api-cloud.bitmart.com` | `api-cloud-v2.bitmart.com` |
| Symbol | `BTC_USDT` | `BTCUSDT` |
| Order Side | `"buy"` / `"sell"` | `1` / `2` / `3` / `4` (integer) |
| Leverage | N/A | Configurable per symbol |

## Usage

Ask your AI agent in natural language:

```text
Open a 10x long BTC position with 100 contracts
```

```text
Set take profit at 72000 and stop loss at 64000 for my BTC long
```

```text
Close my ETH short position
```

```text
Show my futures positions and PnL
```

```text
Transfer 1000 USDT from spot to futures wallet
```

## Related Skills

- [bitmart-exchange-spot](../bitmart-exchange-spot/) — Spot trading
