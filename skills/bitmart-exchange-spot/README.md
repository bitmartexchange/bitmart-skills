# bitmart-exchange-spot

BitMart spot trading skill for AI agents.

## Capabilities

- **Market Data**: Tickers, order book depth, recent trades, K-line/candlestick data, trading pair details, supported currencies
- **Trading**: Market / limit / limit_maker / IOC orders, batch orders
- **Order Management**: Cancel single / multiple / all orders, query orders by ID or client ID
- **Account**: Balance queries (all wallets and spot-only), fee rates (per pair and per account tier)
- **Margin**: Isolated margin trading, borrow, repay
- **System**: Server time, system service / maintenance status

## API Endpoints

| Category | Count | Auth |
|----------|-------|------|
| Public Market Data | 9 endpoints | NONE |
| Account / Margin Account | 8 endpoints | KEYED |
| Spot Trading | 6 endpoints | SIGNED |
| Order Query | 6 endpoints | SIGNED |
| Margin Loan | 3 endpoints | SIGNED |
| System | 2 endpoints | NONE |

**Total: 34 endpoints** (aligned with Go SDK v1.4.0)

## Authentication

Requires BitMart API credentials:
- `BITMART_API_KEY` — API key
- `BITMART_API_SECRET` — Secret key
- `BITMART_API_MEMO` — Memo passphrase

Set via environment variables or `~/.bitmart/config.toml`. See `references/authentication.md` for the full setup guide.

## Usage

Ask your AI agent in natural language:

```text
Buy 100 USDT worth of BTC on BitMart
```

```text
Check my BitMart spot balance
```

```text
Cancel all my open orders on BTC_USDT
```

```text
What are my recent trades on BitMart?
```

## Related Skills

- [bitmart-exchange-futures](../bitmart-exchange-futures/) — Futures / contract trading
