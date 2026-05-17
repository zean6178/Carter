# Pump.fun Sniper Bot

Professional Solana memecoin sniper bot for Pump.fun — automated token detection, screening, buy/sell execution with full risk management.

## Architecture

```
[PumpPortal WebSocket] → detect token baru
         │
         ▼
[detector.js] → Pre-filter (dedup, blacklist, dev buy check)
         │
         ▼
[screening.js] → Confidence Scoring (0-100)
         │
         ▼
[risk.js] → Pre-trade check (balance, max positions, daily limits)
         │
         ▼
[executor.js] → Buy token (PumpPortal API / Jupiter fallback)
         │
         ▼
[monitor.js] → Track price, TP/SL/trailing/rug detection → auto-sell
         │
         ▼
[state.js] → Persist trade history, PnL, blacklists
         │
         ▼
[telegram.js] → Notifications & commands
```

## Features

- **Real-time detection** via PumpPortal WebSocket
- **Multi-factor screening** (holders, volume, bundle detection, deployer history)
- **Tiered take-profit** (50% @2x, 30% @3x, 15% @5x, 5% moonbag)
- **Trailing stop** & stop loss
- **Rug detection** (price crash, liquidity removal, dev dump)
- **Risk management** (max positions, daily loss limit, cooldown)
- **Telegram bot** (notifications, /status, /pause, /resume, /positions)
- **Dry run mode** for safe testing

## Setup

```bash
# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Edit .env with your keys

# Run in dry mode (testing)
npm run dev

# Run live
npm start
```

## Configuration

All parameters are in `config.js`:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `buyAmountSol` | 0.5 SOL | Amount per snipe |
| `maxOpenPositions` | 3 | Max concurrent positions |
| `stopLossPct` | -40% | Cut loss threshold |
| `trailingStopPct` | 25% | Drop from peak to exit |
| `takeProfitLevels` | 2x/3x/5x | Staged exits |
| `maxDailyLossSol` | 5 SOL | Daily loss limit |
| `blockBundledLaunch` | true | Skip insider launches |

## Requirements

- Node.js >= 18
- Solana wallet (private key)
- RPC endpoint (Helius/Quicknode recommended)
- Telegram bot token (optional, for notifications)

## Disclaimer

This bot is for educational purposes. Trading memecoins is extremely high-risk. Only use funds you can afford to lose entirely.
