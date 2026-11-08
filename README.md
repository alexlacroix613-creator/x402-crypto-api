# x402 Crypto APIs

**Live pay-per-call crypto APIs with x402 micropayments on Base.**

No API keys. No subscriptions. No rate limits. **3 free calls/day** — then $0.01 USDC per call.

---

## 🚀 Quick Start (Free)

```bash
curl https://neat-brooms-shine.loca.lt/price?symbol=bitcoin
```

Returns real data — no payment needed for first 3 calls/day.

```json
{
  "symbol": "BITCOIN",
  "coin_id": "bitcoin",
  "price_usd": 75842,
  "change_24h_pct": 2.46,
  "free_calls_remaining": 2,
  "upgrade": "Pay $0.01/call for unlimited access"
}
```

**After 3 free calls:** Returns 402 with x402 payment requirements. Sign EIP-3009, retry with X-Payment header, continue.

---

## 💰 Pricing

| Tier | Price | Limits |
|------|-------|--------|
| **Free** | $0 | 3 calls/day per IP |
| **Paid** | $0.01/call | Unlimited |

**No signup. No API key. Pay per use.**

---

## 🛠️ Endpoints

### Price API — `https://neat-brooms-shine.loca.lt`

| Endpoint | Description |
|----------|-------------|
| `GET /price?symbol=bitcoin` | Real-time price + 24h change |
| `GET /docs` | API documentation |
| `GET /health` | Service status |

**Supported:** BTC, ETH, SOL, BNB, XRP, ADA, AVAX, DOGE, DOT, MATIC, LINK, UNI, ATOM, LTC, ETC + any CoinGecko ID

### Research API — `https://brown-deer-agree.loca.lt`

| Endpoint | Description | Price |
|----------|-------------|-------|
| `GET /research?project=solana` | Structured project dossier | $0.05/call |

---

## 🔧 Paid Call Example

```python
import base64, json, time
from eth_account import Account
from eth_account.messages import encode_typed_data

account = Account.from_key('0x...')

now = int(time.time())
payment = {
    'from': account.address,
    'to': '0x4721052333CA5A3fcf8cF1d839A8ADdffaf20a79',
    'value': 10000,  # $0.01
    'validAfter': now - 100,
    'validBefore': now + 600,
    'nonce': '0x' + '00' * 32,
    'asset': '0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913'
}

DOMAIN = {
    'name': 'USD Coin', 'version': '2', 'chainId': 8453,
    'verifyingContract': '0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913'
}
signed = account.sign_message(encode_typed_data(
    domain_data=DOMAIN,
    message_types={'TransferWithAuthorization': [
        {'name': 'from', 'type': 'address'}, {'name': 'to', 'type': 'address'},
        {'name': 'value', 'type': 'uint256'}, {'name': 'validAfter', 'type': 'uint256'},
        {'name': 'validBefore', 'type': 'uint256'}, {'name': 'nonce', 'type': 'bytes32'},
    ]},
    message_data={k: v for k, v in payment.items() if k != 'asset'}
))
payment['signature'] = '0x' + signed.signature.hex()

import urllib.request
req = urllib.request.Request(
    'https://neat-brooms-shine.loca.lt/price?symbol=bitcoin',
    headers={'X-Payment': base64.b64encode(json.dumps(payment).encode()).decode()}
)
print(json.loads(urllib.request.urlopen(req).read()))
```

---

## 🌐 Links

- **Price API:** https://neat-brooms-shine.loca.lt
- **Research API:** https://brown-deer-agree.loca.lt
- **Payment guard:** https://github.com/alexlacroix613-creator/x402-guard

---

**If this saved you time:** USDC on Base → `0x4721052333CA5A3fcf8cF1d839A8ADdffaf20a79`
