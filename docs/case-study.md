# Case Study — Lancelot Trading Machine

## Problem

A market automation system becomes difficult to trust when data collection, discovery, decision logic, execution and state management all live in one opaque flow. A transient API failure can then look like a trading signal, internal state can drift from external reality, and debugging becomes expensive.

## Architecture decision

The private system was split into explicit layers:

1. **Acquisition** — dedicated market-data workers collect and normalize source-specific data.
2. **Discovery** — potential candidates enter a separate qualification path instead of going directly to execution logic.
3. **Context** — additional intelligence is attached only after a candidate passes the earlier gates.
4. **Decision** — the system evaluates actions from normalized state rather than raw provider responses.
5. **Execution state** — action results are parsed and persisted explicitly.
6. **Reconciliation** — internal positions and external reality are compared to detect drift.
7. **Operations** — alerts, reports, retries and degradation checks make the system observable.

## Why dedicated workers

Binance and CoinGecko have different data shapes, timing characteristics and failure modes. Keeping them as dedicated workers avoids coupling provider-specific behaviour to the decision layer.

The CoinGecko path also demonstrates rate-limit control, pair validation and single-flight locking. The Binance path focuses on short-interval market state and normalized cache writes.

## State as a first-class component

The system does not assume that a successful request means the internal state is correct. BUY/SELL results, balances, open positions and reconciliation are handled as explicit state transitions.

This matters because production automation must recover from partial failures, duplicated events and delayed responses.

## Failure handling

The architecture includes:

- retries for transient operations
- degradation alerts
- reconciliation checks
- state rebuild/synchronization paths
- operational reporting
- separate monitoring of data acquisition and execution

## What remains private

The public repository intentionally excludes exact entry/exit criteria, sizing, proprietary scoring, wallet/account identifiers, private prompts, credentials, endpoints and the production workflow JSON.

## Takeaway

The key engineering idea is not "AI trading". It is **controlled automation around unreliable external systems**: normalize inputs, isolate responsibilities, keep state explicit, validate before action and reconcile after action.
