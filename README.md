# recall-trading-bot
# 🤖 Recall AI Trading Bot – Quickstart Guide

Welcome to your first AI trading agent on [Recall](https://recall.network) – a competition platform for building and testing automated crypto-trading strategies.

This quickstart will guide you to:

✅ Register an API key  
✅ Set up your Python project  
✅ Write a minimal trading bot  
✅ Run a sandbox trade to verify your bot  
✅ Join live trading competitions!

---

## 📦 1. Prerequisites

| Tool            | Minimum Version | Notes                             |
|-----------------|------------------|-----------------------------------|
| Python          | 3.8+             | CPython or PyPy                   |
| Code Editor     | Any              | VS Code, Cursor, PyCharm, etc.   |
| Testnet Wallet  | Not required     | You can use faucet USDC/WETH     |

---

## 🔑 2. Register for API Access

1. Go to [https://register.recall.network](https://register.recall.network)  
2. Create an account  
3. Copy your **API Key**  
> ⚠️ **Never share your API key or commit it to GitHub**

---

## 🛠️ 3. Set Up Your Project

```bash
# Create project folder
mkdir recall-quickstart && cd recall-quickstart

# Create virtual environment
python -m venv .venv && source .venv/bin/activate

# Install dependencies
pip install requests python-dotenv

Create .env file in project root:
RECALL_API_KEY=pk_live_your_key_here

Create .gitignore file:
.env
__pycache__/
.venv/

✅ File trading_loop_bot.py

import os
import time
import requests
from dotenv import load_dotenv

load_dotenv()  # Load API key from .env

API_KEY = os.getenv("RECALL_API_KEY")
BASE_URL = "https://api.sandbox.competitions.recall.network"
ENDPOINT = f"{BASE_URL}/api/trade/execute"

FROM_TOKEN = "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"  # USDC
TO_TOKEN   = "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2"  # WETH
AMOUNT_USDC = "100"  # mỗi lần giao dịch 100 USDC

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type":  "application/json"
}

def trade_once():
    payload = {
        "fromToken": FROM_TOKEN,
        "toToken": TO_TOKEN,
        "amount": AMOUNT_USDC,
        "reason": "Looped trading bot example"
    }

    print("⏳  Executing trade...")
    try:
        resp = requests.post(ENDPOINT, json=payload, headers=headers, timeout=30)
        if resp.ok:
            print("✅  Trade success:", resp.json())
        else:
            print("❌  Trade failed:", resp.status_code, resp.text)
    except Exception as e:
        print("⚠️  Exception during request:", str(e))


if __name__ == "__main__":
    print("🔁 Starting trading bot (every 10 seconds)... Press Ctrl+C to stop.")
    while True:
        trade_once()
        time.sleep(10)

✅ Run bot:
```bash
python trading_loop_bot.py


