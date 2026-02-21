# Minara OpenClaw Skills

An [OpenClaw](https://docs.openclaw.ai) skill that gives your AI agent crypto trading intelligence via [Minara](https://minara.ai). Uses the **Minara CLI** for wallet login, trading, swaps, perps, AI chat, and market discovery. Optionally uses the **Minara Agent API** for programmatic intent parsing, strategy suggestions, and prediction markets. Supports both **EVM** and **Solana** chains.

## Features

| Capability              | Interface         | Chains       | Description                                                            |
| ----------------------- | ----------------- | ------------ | ---------------------------------------------------------------------- |
| **Login & Account**     | CLI               | All          | Email, Google, or Apple login — persistent session                     |
| **Balance & Portfolio** | CLI               | All          | Quick balance, spot holdings with PnL, perps equity & positions        |
| **Spot Swap**           | CLI / API         | EVM + Solana | Chain-abstracted swaps (auto-detect chain from token); NL intent parsing via API |
| **Transfer & Withdraw** | CLI               | EVM + Solana | Send tokens to external wallets                                        |
| **Deposit**             | CLI               | EVM + Solana | Spot deposit addresses; deposit to perps (direct or Spot → Perps transfer) |
| **Perpetual Futures**   | CLI / API         | EVM          | Orders, positions, leverage, TP/SL; AI strategy via API                |
| **Limit Orders**        | CLI               | EVM + Solana | Price-triggered limit orders with expiry                               |
| **AI Chat**             | CLI / API         | All          | Interactive REPL & single-shot with fast/quality/thinking modes        |
| **Market Discovery**    | CLI               | All          | Trending tokens, Fear & Greed Index, BTC metrics, token search         |
| **Prediction Market**   | API               | All          | Polymarket event analysis with probability estimates                   |

## How it works

The skill uses an **intent routing** model — each user request pattern maps to either a CLI command or an API call:

- **CLI** (`minara` binary) — handles all wallet operations, trading execution, AI chat, and market discovery. Requires `minara login` once.
- **Agent API** (optional, requires `MINARA_API_KEY`) — adds natural-language swap intent parsing, perp strategy suggestions, and prediction market analysis. When API key is not set, the skill falls back to CLI for equivalent functionality.

```
User intent
    ↓
┌─────────────────────────────────────┐
│  Intent Routing (SKILL.md)          │
│  pattern match → action             │
└────┬────────────────────┬───────────┘
     │                    │
  CLI command         API endpoint
  (always available)  (if MINARA_API_KEY set)
     │                    │
     ▼                    ▼
┌─────────────┐   ┌──────────────────┐
│ Minara CLI  │   │ Minara Agent API │
│ minara swap │   │ intent-to-swap   │
│ minara chat │   │ perp-suggestion  │
│ minara perps│   │ prediction-market│
│ ...         │   │ chat             │
└─────────────┘   └──────────────────┘
```

## Quick Start

```bash
# 1. Install the CLI
npm install -g minara

# 2. Login
minara login

# 3. Start using
minara account                                    # View wallet addresses
minara balance                                    # Quick total balance
minara deposit spot                               # Show spot deposit addresses
minara chat "BTC price?"                          # Ask the AI
minara swap -s buy -t '$BONK' -a 100             # Swap tokens (chain auto-detected)
minara discover trending                          # Trending tokens
```

### With OpenClaw skill system

```bash
clawhub install minara
```

Or clone manually:

```bash
git clone https://github.com/Minara-AI/openclaw-skill.git
cp -r openclaw-skill/skills/minara /path/to/your/openclaw/workspace/skills/
```

Add to `~/.openclaw/openclaw.json`:

```json
{
  "skills": {
    "entries": {
      "minara": {
        "enabled": true,
        "apiKey": "YOUR_MINARA_API_KEY"
      }
    }
  }
}
```

`apiKey` is optional — omit it to use CLI-only mode. Then run `minara login` once.

## Supported Chains

Ethereum, Base, Arbitrum, Optimism, Polygon, Avalanche, BSC, Solana, Berachain, Blast, Manta, Mode, Sonic.

## Examples

See [`examples.md`](skills/minara/examples.md) for full commands and code:

| # | Scenario | Interface |
|---|---|---|
| 1 | Login & account | CLI |
| 2 | Swap tokens | CLI |
| 3 | Swap tokens (intent parsing) | API |
| 4 | Transfer & withdraw | CLI |
| 5 | Balance, portfolio & deposit | CLI |
| 6 | Perpetual futures | CLI |
| 7 | Perp strategy | API |
| 8 | AI chat | CLI / API |
| 9 | Market discovery | CLI |
| 10 | Prediction market | API |
| 11 | Limit orders | CLI |
| 12 | Premium & subscription | CLI |

## File Structure

```
skills/minara/
├── SKILL.md          # Agent-facing intent routing, CLI reference, API reference
└── examples.md       # Command and API code examples for each scenario
```

## Links

- [Minara](https://minara.ai) — Crypto trading intelligence platform
- [Minara CLI (npm)](https://www.npmjs.com/package/minara) — CLI client
- [Minara API Docs](https://minara.ai/docs/ecosystem/agent-api/getting-started-by-api-key)
- [OpenClaw Skills](https://docs.openclaw.ai/tools/skills) — Skill authoring guide
- [ClawHub](https://clawhub.ai) — Skill registry

## License

MIT
