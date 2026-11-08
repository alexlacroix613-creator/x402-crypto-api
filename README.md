# x402 Crypto APIs

**Live pay-per-call crypto APIs with x402 micropayments on Base.**

No API keys. No subscriptions. No rate limits. Pay $0.01–$0.05 USDC per call, settled instantly on-chain.

---

## 🚀 Live Endpoints

### 1. Price API — $0.01/call
Real-time crypto prices from CoinGecko.

```
GET https://neat-brooms-shine.loca.lt/price?symbol=bitcoin
```

**Try it (free 402 challenge):**
```bash
curl https://neat-brooms-shine.loca.lt/price?symbol=ethereum
```

Returns:
```json
{
  "accepts": [{
    "amount": "10000",
    "asset": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
    "network": "base",
    "payTo": "0x4721052333CA5A3fcf8cF1d839A8ADdffaf20a79"
  }],
  "error": "Payment required"
}
```

Sign the EIP-3009 authorization, retry with `X-Payment` header, get data:

```json
{
  "symbol": "ETHEREUM",
  "coin_id": "ethereum",
  "price_usd": 2456.78,
  "change_24h_pct": 1.23,
  "source": "coingecko",
  "timestamp": 1794111319
}
```

**Supported:** BTC, ETH, SOL, BNB, XRP, ADA, AVAX, DOGE, DOT, MATIC, LINK, UNI, ATOM, LTC, ETC + any CoinGecko ID

---

### 2. Research API — $0.05/call
Structured crypto project dossier.

```
GET https://brown-deer-agree.loca.lt/research?project=solana
```

Returns sections on: token, funding, technical, community, risk assessment, opportunity.

---

## 💡 Why x402?

| Traditional API | x402 API |
|-----------------|----------|
| Sign up for API key | No account needed |
| Monthly subscription | Pay per call |
| Rate limits | Unlimited (pay for what you use) |
| Credit card billing | USDC on Base |
| Human-only | AI-agent native |

**Built for autonomous agents** that need to pay for data without human intervention.

---

## 🔧 Quick Start (Python)

```python
import base64, json, time
from eth_account import Account
from eth_account.messages import encode_typed_data

# Your wallet (needs USDC on Base)
account = Account.from_key('0x...')

# Build payment
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

# Sign EIP-712
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

# Call API
import urllib.request
req = urllib.request.Request(
    'https://neat-brooms-shine.loca.lt/price?symbol=bitcoin',
    headers={'X-Payment': base64.b64encode(json.dumps(payment).encode()).decode()}
)
print(json.loads(urllib.request.urlopen(req).read()))
```

---

## 🛠️ Tech Stack

- **Payment layer:** [x402-guard](https://github.com/alexlacroix613-creator/x402-guard) — open source, ~150 lines, offline verification
- **Data:** CoinGecko API (free tier, no key needed)
- **Network:** Base (USDC, chain ID 8453)
- **Protocol:** x402 (HTTP 402 + EIP-3009 TransferWithAuthorization)

---

## 📊 Pricing

| Endpoint | Price | Use Case |
|----------|-------|----------|
| `/price` | $0.01 | Real-time price checks, trading bots |
| `/research` | $0.05 | Due diligence, project analysis |

**No minimums. No maximums. Pay exactly what you use.**

---

## 🌐 Links

- **Price API:** https://neat-brooms-shine.loca.lt
- **Research API:** https://brown-deer-agree.loca.lt
- **Payment guard:** https://github.com/alexlacroix613-creator/x402-guard

---

## 🤝 Contributing

Want to add an endpoint? PRs welcome.

Ideas:
- `/airdrop-check` — wallet eligibility across active airdrops
- `/sentiment` — social media sentiment analysis
- `/gas-optimize` — best time to transact on Ethereum
- `/whale-watch` — large transaction alerts

---

## 📜 License

MIT. Use it, fork it, build on it.

**If this saved you time:** USDC on Base → `0x4721052333CA5A3fcf8cF1d839A8ADdffaf20a79`

---

*Built with OpenClaw + x402 protocol. Autonomous agents welcome.*
