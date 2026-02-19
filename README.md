<div align="center">

<img src="https://pyp.stanl.ink/logo.png" alt="PyP Logo" width="120" />

# PyP

**Build Trading Strategies That Actually Work.**

*Create · Train · Deploy · Monetize*

[![Platform](https://img.shields.io/badge/Platform-pyp.stanl.ink-FF6B35?style=for-the-badge)](https://pyp.stanl.ink)
[![Docs](https://img.shields.io/badge/Docs-Read%20Now-1A1A2E?style=for-the-badge)](https://pyp.stanl.ink/docs)
[![Marketplace](https://img.shields.io/badge/Marketplace-Browse-2ECC71?style=for-the-badge)](https://pyp.stanl.ink/marketplace)
[![Discord](https://img.shields.io/badge/Discord-Join%20Community-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/pyp)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

</div>

---

## What is PyP?

**PyP** is an AI-powered trading strategy platform that lets traders and developers **write, train, and deploy profitable strategies** — without machine learning expertise or complex coding.

You write your strategy logic in **pypscript** — a purpose-built language with 300+ built-in indicators. PyP's training pipeline runs it against real historical market data and distills the profitable patterns into a compact binary `.pyp` file. That file becomes your deployable strategy — testable in our PPE simulation environment and deliverable as live signals to Telegram, Discord, WhatsApp, or the web.

> *No ML expertise. No black boxes. No auto-trading legal headaches. Just signals that work.*

---

## The PyP Pipeline

```
  ┌─────────────────────────────────────────────────────────────────┐
  │                                                                 │
  │   .ppc file          Training Pipeline        .pyp file         │
  │   (pypscript)   ──►  (AI + Market Data)  ──►  (Binary Vector)  │
  │                                                                 │
  └─────────────────────────────────────────────────────────────────┘
           │                                           │
           ▼                                           ▼
   Write your logic                         Run in PPE Engine
   using indicators                         See real performance
   like MA, MACD, RSI                       metrics per pair
                                                       │
                                                       ▼
                                            Sell on Marketplace
                                            or deploy live signals
```

### 1. Write in pypscript

```pypscript
#!pyp/1.0
engine: pyp-1.0

import indicators.sma
import functions.cross

strategy "simple_ma_crossover" {
    fast_ma = sma.evaluate(close, 10)
    slow_ma = sma.evaluate(close, 20)

    patterns {
        cross(fast_ma, slow_ma)  -> UP(confidence: 0.75)
        cross(slow_ma, fast_ma)  -> DOWN(confidence: 0.75)
        default                  -> HOLD(confidence: 0.95)
    }
}

meta {
    author: "you"
    version: "1.0"
    tags: ["trend-following", "moving-average"]
}

config {
    timeframes: ["1h", "4h"]
    pairs: ["EURUSD", "GBPUSD"]
    optimization: "accuracy"
}
```

### 2. Train — Get a `.pyp` File

PyP compiles your `.ppc`, runs it against historical OHCLV data per timeframe, and uses a proprietary ML pipeline to extract the profitable pattern signature from your strategy. The output is a binary `.pyp` file — your trained strategy, compact and deployable.

```
simple-ma-crossover-1h-EURUSD.pyp
simple-ma-crossover-4h-EURUSD.pyp
simple-ma-crossover-1h-GBPUSD.pyp
```

One strategy. Multiple releases. Each optimized for its pair and timeframe.

### 3. Prove It — Run PPE

The **Proof-of-Performance Environment** simulates your `.pyp` against historical market data you haven't trained on. Watch the Actor make paper trades in real time, see your equity curve move, and get verified performance metrics:

| Pair | Win Rate | Max Drawdown | Profit Factor |
|------|----------|--------------|---------------|
| EUR/USD | 63% | 11% | 1.8 |
| GBP/USD | 58% | 14% | 1.5 |
| USD/JPY | 49% | 19% | 1.1 |

PPE results are **server-verified** and tamper-proof — users on the marketplace see exactly what your strategy is capable of.

### 4. Deploy Live Signals

Once live, PyP's **Brain** continuously analyzes real market data using the same indicators defined in your `.ppc`, runs them through the trained model embedded in your `.pyp`, and outputs a confidence-weighted signal in real time.

The **Signal Dispatcher** delivers to:

- 📱 **Telegram** — instant push notification
- 🎮 **Discord** — bot message to your channel
- 💬 **WhatsApp** — via Business API
- 🌐 **Web Dashboard** — real-time in-app signal feed

> All signals carry: pair, direction (UP/DOWN), confidence score, timeframe, and timestamp.

---

## Supported Markets

| Asset Class | Status |
|-------------|--------|
| **Forex (FX)** | ✅ v1.0 |
| **Crypto** | ✅ v1.0 |
| Commodities | 🔜 v3+ |
| Indices | 🔜 v3+ |

**Pairs at launch:** 15 FX majors + minors · 15 crypto pairs

---

## pypscript — 300+ Indicators Built In

PyP ships with a comprehensive indicator dictionary covering:

| Category | Examples |
|----------|---------|
| Trend | SMA, EMA, WMA, DEMA, TEMA, HMA |
| Momentum | RSI, MACD, Stochastic, CCI, ROC |
| Volatility | Bollinger Bands, ATR, Keltner Channels |
| Volume | OBV, VWAP, MFI, CMF |
| Pattern | Crossovers, Divergence, Candlestick patterns |
| Custom | Composable functions and multi-indicator logic |

All indicators work identically across FX and Crypto. Volume-based indicators on FX use tick volume — documented clearly in the [docs](https://pyp.stanl.ink/docs).

---

## The Marketplace

PyP strategies can be:

- 🔒 **Private** — signals for your eyes only
- 🌍 **Open Source** — community can view and fork the `.ppc` logic
- 💰 **For Sale** — list your `.pyp` with verified PPE metrics, earn per subscriber
- 🆓 **Free** — share with the community

Every marketplace listing shows PPE-verified performance across multiple pairs. No fake backtests. No cherry-picked results.

---

## Architecture

PyP is built on a modern, high-performance stack:

```
Frontend          React + Lightweight Charts (TradingView)
Signal Delivery   Cloudflare Workers (Signal Dispatcher)
PPE Engine        Rust → WASM running in Cloudflare Durable Objects
Compiler          Rust (pypscript → IR → training pipeline)
Storage           Cloudflare R2 (OHCLV data + .pyp files)
Database          Cloudflare D1 (users, subscriptions, preferences)
```

- **PPE sessions** are isolated Durable Objects — one per simulation run
- **`.pyp` files** never leave the server for paid/private strategies
- **OHCLV data** is streamed from R2 in chunks — memory efficient at scale
- **Signals** are stateless — dispatched per event, no session overhead

---

## Roadmap

```
v0.1.0  ──►  v1.0.0   Bug fixes · Stability · Core FX + Crypto
v2.0          90% bug resolution · PPE reliability · Marketplace trust
v3.0+         Commodities · Indices · Advanced features · Community requests
```

---

## ⚠️ Disclaimer

Strategies, signals, and performance metrics on PyP are **for informational purposes only** and do not constitute financial advice. Past performance in PPE simulations does not guarantee future results. Always do your own research before making trading decisions.

---

## Community

<div align="center">

[![Discord](https://img.shields.io/badge/Join%20our%20Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/pyp)
[![Platform](https://img.shields.io/badge/Get%20Started%20Free-FF6B35?style=for-the-badge)](https://pyp.stanl.ink)
[![Docs](https://img.shields.io/badge/Read%20the%20Docs-1A1A2E?style=for-the-badge)](https://pyp.stanl.ink/docs)
[![Marketplace](https://img.shields.io/badge/Browse%20Marketplace-2ECC71?style=for-the-badge)](https://pyp.stanl.ink/marketplace)

*Built by traders. For traders.*

</div>
