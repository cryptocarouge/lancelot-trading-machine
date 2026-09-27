# Lancelot Trading Machine

Public architecture showcase of a private, modular market-intelligence and automation system built with n8n.

The production workflow is intentionally not published. This repository documents the engineering approach without exposing credentials, wallets, private endpoints, execution parameters or proprietary trading logic.

## What it demonstrates

- Multi-source market-data ingestion
- Dedicated Binance and CoinGecko market workers
- Dynamic market-universe and discovery stages
- Candidate qualification before downstream processing
- Separation between data collection, decision logic and execution
- Persistent state and position reconciliation
- Operational alerts and Telegram control
- Daily, weekly and monthly reporting
- Cache, retry and rate-limit handling
- Monitoring and degradation detection

## Conceptual architecture

```text
Market Sources
     |
     +--> Binance Worker
     |
     +--> CoinGecko Worker
     |
     +--> Other Market / On-chain Sources
                 |
                 v
          Normalized Market State
                 |
                 v
             Discovery
                 |
                 v
           Qualification
                 |
                 v
       Intelligence / Context
                 |
                 v
          Decision Layer
                 |
                 v
       Risk & Position State
                 |
                 v
             Execution
                 |
                 v
      Reconciliation / Alerts
```

## Worker design

The production system uses dedicated workers rather than forcing every data source through one monolithic path.

**Binance Worker**
- Short-interval market data
- Normalization
- Cache/state writes

**CoinGecko Worker**
- Pair resolution
- Solana-pair validation
- OHLCV retrieval
- Rate-limit control
- Single-flight locking and cache writes

These workers are represented here as architecture components rather than separate public repositories.

## Engineering principles

1. **Separate acquisition from decisions.**
2. **Keep state explicit.**
3. **Reconcile internal state with external reality.**
4. **Use retries, locks and degradation alerts for operational resilience.**
5. **Keep sensitive execution rules private.**

## Security boundary

Not published:

- API keys, tokens or credentials
- Wallet identifiers or private keys
- Private webhook URLs
- Telegram chat IDs
- Google Sheet IDs
- Entry/exit thresholds
- Position-sizing logic
- Exact BUY/SELL rules
- Proprietary prompts or scoring logic
- Production workflow JSON

## Disclaimer

Personal engineering project and technical portfolio. Nothing in this repository is financial advice or a recommendation to trade.
