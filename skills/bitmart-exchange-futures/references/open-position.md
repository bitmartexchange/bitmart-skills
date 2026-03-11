# Open Position Workflow

## Scenario: Open a Long Position

**User prompt:** "Open a 10x long BTC position with 100 contracts on BitMart"

### Step 1: Pre-flight — Check futures balance

```bash
curl -s -H "X-BM-KEY: $BITMART_API_KEY" \
  'https://api-cloud-v2.bitmart.com/contract/private/assets-detail'
```

Verify the user has sufficient `available_balance` in USDT. If insufficient, suggest transferring from spot:

```
Insufficient futures balance. Available: 50.00 USDT, Required: ~670 USDT (estimated).
Would you like to transfer USDT from your spot wallet?
```

### Step 2: Pre-flight — Check contract details

```bash
curl -s 'https://api-cloud-v2.bitmart.com/contract/public/details?symbol=BTCUSDT'
```

Validate:
- `contract_size` — to calculate margin requirement
- `min_volume` — ensure order size meets minimum
- `max_volume` — ensure order size does not exceed maximum
- `max_leverage` — ensure requested leverage is supported
- `price_precision` — for limit orders

### Step 3: Pre-flight — Check leverage brackets

```bash
curl -s 'https://api-cloud-v2.bitmart.com/contract/public/leverage-bracket?symbol=BTCUSDT'
```

Verify the requested leverage (10x) is available for the position size. If position size exceeds the tier limit, warn the user:

```
Warning: At 10x leverage, maximum position is 500,000 contracts.
Your requested 100 contracts is within limits.
```

### Step 4: Set leverage (if needed)

Check current leverage and set if different:

```bash
TIMESTAMP=$(date +%s000)
BODY='{"symbol":"BTCUSDT","leverage":"10","open_type":"cross"}'
SIGN=$(echo -n "${TIMESTAMP}#${BITMART_API_MEMO}#${BODY}" | openssl dgst -sha256 -hmac "$BITMART_API_SECRET" | awk '{print $2}')
curl -s -X POST 'https://api-cloud-v2.bitmart.com/contract/private/submit-leverage' \
  -H "Content-Type: application/json" \
  -H "X-BM-KEY: $BITMART_API_KEY" \
  -H "X-BM-SIGN: $SIGN" \
  -H "X-BM-TIMESTAMP: $TIMESTAMP" \
  -d "$BODY"
```

### Step 5: Check/set position mode (if needed)

```bash
curl -s -H "X-BM-KEY: $BITMART_API_KEY" \
  'https://api-cloud-v2.bitmart.com/contract/private/get-position-mode'
```

If the user wants hedge mode (to hold long and short simultaneously), set it if not already:

```bash
TIMESTAMP=$(date +%s000)
BODY='{"position_mode":"hedge_mode"}'
SIGN=$(echo -n "${TIMESTAMP}#${BITMART_API_MEMO}#${BODY}" | openssl dgst -sha256 -hmac "$BITMART_API_SECRET" | awk '{print $2}')
curl -s -X POST 'https://api-cloud-v2.bitmart.com/contract/private/set-position-mode' \
  -H "Content-Type: application/json" \
  -H "X-BM-KEY: $BITMART_API_KEY" \
  -H "X-BM-SIGN: $SIGN" \
  -H "X-BM-TIMESTAMP: $TIMESTAMP" \
  -d "$BODY"
```

**Note:** Position mode cannot be changed while there are open positions.

### Step 6: Present order summary and ask for CONFIRM

Display to user:

```
Position Summary:
  Symbol: BTCUSDT
  Direction: Long (buy_open_long, side=1)
  Type: Market
  Size: 100 contracts
  Leverage: 10x
  Margin Mode: Cross
  Estimated Entry: ~67,123 USDT
  Estimated Margin: ~671 USDT
  Estimated Liquidation: ~60,500 USDT

  WARNING: Futures trading carries significant risk. Higher leverage
  amplifies both gains and losses. You could lose your entire margin.

Please type CONFIRM to proceed.
```

### Step 7: Submit order (after user confirms)

```bash
TIMESTAMP=$(date +%s000)
BODY='{"symbol":"BTCUSDT","side":1,"type":"market","size":100,"leverage":"10","open_type":"cross"}'
SIGN=$(echo -n "${TIMESTAMP}#${BITMART_API_MEMO}#${BODY}" | openssl dgst -sha256 -hmac "$BITMART_API_SECRET" | awk '{print $2}')
curl -s -X POST 'https://api-cloud-v2.bitmart.com/contract/private/submit-order' \
  -H "Content-Type: application/json" \
  -H "X-BM-KEY: $BITMART_API_KEY" \
  -H "X-BM-SIGN: $SIGN" \
  -H "X-BM-TIMESTAMP: $TIMESTAMP" \
  -d "$BODY"
```

Expected response:

```json
{
  "code": 1000,
  "data": {
    "order_id": 23456789012345678
  }
}
```

### Step 8: Verify position

```bash
curl -s -H "X-BM-KEY: $BITMART_API_KEY" \
  'https://api-cloud-v2.bitmart.com/contract/private/position-v2?symbol=BTCUSDT'
```

### Step 9: Report results

```
Position Opened Successfully:
  Symbol: BTCUSDT
  Direction: Long
  Size: 100 contracts
  Entry Price: 67,123.4 USDT
  Leverage: 10x
  Margin Mode: Cross
  Margin Used: 671.23 USDT
  Liquidation Price: 60,500.0 USDT
  Mark Price: 67,125.0 USDT
  Unrealized PnL: +0.16 USDT

Not financial advice. Futures trading carries significant risk of loss.
```

---

## Scenario: Open a Short Position

**User prompt:** "Short ETH with 20x leverage, 50 contracts, isolated margin"

The flow is the same as above, with these differences:

- **Side:** 4 (sell_open_short) instead of 1
- **Leverage:** 20x
- **Open type:** isolated
- **Liquidation direction:** above entry price (for shorts, liquidation is at higher price)

Order body:

```json
{"symbol":"ETHUSDT","side":4,"type":"market","size":50,"leverage":"20","open_type":"isolated"}
```

---

## Error Handling

| Error | Cause | Action |
|-------|-------|--------|
| `code != 1000` on submit | Various | Report error message; do not retry automatically |
| Insufficient balance | Not enough margin | Suggest transferring from spot or reducing position size |
| Invalid leverage | Exceeds bracket limit | Show leverage brackets and suggest a valid leverage |
| Position mode conflict | Has open positions | Cannot change mode; inform user to close positions first |
| Invalid size | Below min or above max | Show contract details with min/max volume |
