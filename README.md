# Polybot

Paper-first foundation for international Polymarket prediction-market research.
Includes configuration, offline lifecycle, SQLite accounting, read-only crypto
discovery, deterministic strategy/risk libraries, and a local paper broker.
**The CLI does not enable a trading loop or live execution.** `status` reports
configuration, not a daemon or wallet. Only `discover` contacts public APIs.

## Install on Windows (PowerShell)

Use standard CPython **3.12.14** and the pinned **polymarket-client 0.11.0**.
The MSYS Python found on this machine is not the tested project runtime.
Install [uv using its official Windows instructions](https://docs.astral.sh/uv/getting-started/installation/)
if `uv --version` is unavailable, then open PowerShell in this repository:

```powershell
uv python install 3.12.14
uv sync --locked
uv run --locked polybot config-check --config config.example.toml
uv run --locked polybot status --config config.example.toml
uv run --locked polybot run --config config.example.toml --once
```

No wallet, account, environment secrets, or virtual-environment activation is
needed. Initial installation needs internet; these commands thereafter perform
no exchange requests. The smoke check exits 0 and logs `mode=paper` with
`STARTING`, `RUNNING`, `STOPPED` and UTC timestamps. Without `--once`, the process
waits until Ctrl+C (or SIGTERM) and exits cleanly. No balance store is created.

If CPython 3.12.14 is already installed outside uv, use its explicit path:

```powershell
uv sync --locked --python 'C:\path\to\Python312\python.exe'
```

This path is an installation instruction, not a file shipped with this project.
`uv.lock` pins transitive versions and artifact hashes. Do not use `uv lock
--upgrade` as part of a smoke check. `pyproject.toml` restricts the project to
Python 3.12; `.python-version` selects the verified patch release. No authenticated
APIs are used. The public discovery adapter uses verified REST reads.

## Configuration and safety

`config.example.toml` contains **synthetic smoke-test values**, not operator
choices or a funded paper account. Crypto is the selected category. Country,
account type, strategy, intended
bankroll and loss preferences in [requirements](docs/requirements.md) remain
unresolved. An optional local copy is ignored by Git:

```powershell
Copy-Item config.example.toml config.local.toml
uv run --locked polybot config-check
uv run --locked polybot run --once
```

- Monetary/price values must be quoted positive fixed-point strings, at most
  six decimals; they become `Decimal`. Units are `pUSD` and `pUSD/share`.
- Prices satisfy `0 < min <= max < 1`. These are operator bounds, not a
  substitute for per-market tick and minimum-size checks.
- Exposure is committed collateral at risk, including pending reservations
  and costs: per order <= per market <= total <= bankroll.
- Maximum loss is independent of bankroll and no greater than bankroll.
  The CLI schema carries a lifetime drawdown limit, while the Task 10 library
  requires separate explicit daily-loss and funding-adjusted peak-drawdown
  thresholds. The example config remains a smoke fixture and is not silently
  converted into a trading policy.
- Freshness fields are positive integer seconds; order counts are positive
  integers, and per-market count cannot exceed total count. Integers are bounded
  by 2147483647. Values are smoke examples, not risk recommendations.
- Relative directories resolve from the TOML file. Paper/live directories must
  be distinct and non-nested, even after path resolution. They are reserved for
  future stores and are not created by this release.
- Unknown/missing fields are rejected. Mode is `paper` if omitted; `live` always
  fails startup/config-check/status in this release. There is no enable-live
  switch. `POLYBOT_MODE` environment overrides are rejected; credentials in the
  environment never activate live mode. Live needs the prerequisites listed in
  [the API contract](docs/api-contract.md) and a separately authorized implementation.

## Logging and errors

Console logs use UTC. Final rendered messages and exception summaries redact
credential-like key/value pairs, bearer/Basic tokens, private-key hex, URL passwords,
and values from secret-named environment variables. Traceback source/locals are
omitted. Parse errors do not echo raw config or command-line input. Redaction
cannot recognize every arbitrary unlabeled secret: never put secrets in TOML,
CLI arguments, log messages, or exceptions. No secret is needed for these commands.
Exit codes: 0 success/clean stop, 2 invalid config/arguments or blocked live mode,
1 unexpected failure. `--help` and `--version` need no configuration.

## Checks

Task 5 also provides read-only crypto discovery:

```powershell
uv run --locked polybot discover --config config.example.toml --discovery-config discovery.example.toml
uv run --locked polybot discover --config config.example.toml --discovery-config discovery.example.toml --pilot-only
```

This command reads public market metadata, prints inclusion/exclusion reasons and
saves an audit report in the paper SQLite store. It needs no credentials and does
not initialize cash. An incomplete scan exits 1 and includes no pilot markets.
See [discovery and review instructions](docs/discovery.md) for pagination, metadata
requirements, allowlist review and retained evidence.

Task 4 adds immutable shared models and an offline SQLite ledger with versioned
metadata, forecasts, decisions, orders, fills and atomic balance/reservation
updates. See [the model and storage contract](docs/model-storage-contract.md)
for typed interfaces, units, states and limitations. The CLI remains a paper
lifecycle smoke check; it does not submit orders or initialize account balances.
No additional dependencies are needed.

Task 6 adds the public order-book snapshot/stream adapter, validated in-memory
book state, bounded reconnect recovery, and chronological SQLite replay records.
See [the market-data contract](docs/market-data.md). It is a library interface;
the paper CLI still does not generate trade intents or start a long-running
collector.

Task 7 adds depth-aware BUY/SELL quote calculations in `polybot.costs`. Quotes
walk visible levels, require explicit fee and transaction-cost inputs, enforce
the active tick and minimum pUSD notional, and reject insufficient liquidity.
See [the cost contract](docs/costs.md). This remains offline paper analysis and
does not create or submit an order.

Task 8 adds a manual-only forecast pipeline in `polybot.signals` and the
[strategy research protocol](docs/strategy.md). Inputs require complete UTC
timing and evidence provenance and persist atomically in schema 5. Missing,
expired, future, or ambiguous signals return no eligible forecast. This does
not create trade intents or claim an automated forecasting advantage.

Task 9 adds deterministic paper strategy evaluation in `polybot.signals`.
It emits an optional `TradeIntent` without receiving an exchange client and
always emits a durable `StrategyEvaluation`, including no-trade outcomes.
The initial policy uses fixed target/max quantities, strict after-cost
uncertainty buffers, outstanding-order-aware sizing, and explicitly selected
hold-to-resolution or early-exit behavior. This validates pipeline behavior,
not the predictive value of a manual forecast.

Task 10 adds the persistent [risk gate](docs/risk.md) and mandatory broker
submission boundary. BUY orders reserve limit notional plus estimated fees and
explicit costs; SELL orders reserve shares. Partial, cancel-pending, and unknown
orders retain remaining reservations. Exposure includes filled positions plus
worst feasible outstanding BUY fills, with additive event-group limits and no
unsupported hedge offsets. Daily-loss baselines and halts survive restart.
Permitted prices and strategy freshness are checked again at submission time;
funding-adjusted peak-equity drawdown also persists across restarts. Every
`RiskPolicy` has an explicit run mode, so paper values cannot become live limits.

Task 11 adds the local [paper broker](docs/paper-broker.md). Approved GTC limit
orders arrive after explicit simulated latency, consume only marketable recorded
depth, pay the configured exchange-fee formula, and can partially fill. Consumed
snapshot depth persists in schema 9. Resting limits never fill from a displayed
touch alone, and delayed cancellation retains reservations until its effective
time. Every generated fill is labeled simulated; this module has no authenticated
exchange submission dependency.

Task 12 adds the offline [event-driven replay runner](docs/backtest.md). It
reveals books, forecasts, low-fidelity prices and settlements only at recorded
availability times, drives the existing strategy/risk/paper broker, assigns
related markets to chronological development/validation/holdout cohorts, and
requires parameters to be frozen before holdout. Canonical reports include the
exact configuration, boundaries, caller-supplied code version, input/result
hashes and data-quality limits. Repository tests use synthetic inputs only; no
empirical performance conclusion is included.

Task 13 adds read-only [accounting and performance reporting](docs/reporting.md).
It reconstructs FIFO cost basis and realized P/L from fills and settlements,
requires exact-quantity executable liquidation values for open equity, preserves
trace IDs, separates incentives from trading return, and reports financial,
execution, data-quality, Brier, calibration, market, and event results. Missing
values remain explicit; the synthetic tests do not establish profitability.

Task 14 adds the persistent public-data [paper pilot operator workflow](docs/pilot-operations.md)
and a preregistered [evaluation template](docs/pilot-evaluation-template.md).
`polybot pilot run`, `pilot stop`, and `pilot inspect` preserve exact run,
configuration, strategy, metadata, and code versions. Resting limits that lack
queue/trade evidence remain unfilled and are flagged. Pilot evaluation is still
pending; the repository contains no empirical profitability result.

Task 15 adds a [read-only live-account preflight](docs/live-preflight.md). It reads
existing credentials only from locally configured environment-variable names,
checks signer/account/wallet identity, authenticated closed-only status, pUSD
balance, onchain prediction-market approvals, and host geoblock state, and emits a
redacted JSON report. It cannot enable live mode, create credentials, deploy a
wallet, submit/cancel orders, set approvals, transfer/deposit funds, or redeem.
`preflight.example.toml` contains names and unresolved choices only, never secrets.

Task 16 adds a disabled-by-default [live broker library](docs/live-broker.md).
It persists the risk reservation, prepared identity, and submitting state before
one SDK `post_order` attempt; reconciles lost responses without resubmission;
keeps reservations and halts the affected market when acceptance is uncertain;
and applies duplicate REST/stream fills once. `live-execution.example.toml`
contains `enabled = false` and no secret. The CLI still rejects live startup, and
no account or real order was used to test this code.

Task 17 adds durable [reconciliation, heartbeat, and settlement](docs/reconciliation.md)
boundaries. Startup and periodic account comparisons consume all pages and block
new live risk until account state, rebuilt market data, and a healthy credential
heartbeat agree. Unknown external exposure halts the whole bot; known order
disagreements halt their market. Scheduled expiry is separate from resolution,
and redemption remains behind its own explicit authorization. The live CLI is
still disabled, so these interfaces were tested only with synthetic sources.

Task 18 adds persistent [local operator controls](docs/operator-controls.md).
`status` explains freshness, capital, exposure, P/L, orders, heartbeat, and
reconciliation. `pause-new-orders` and `emergency-halt` block submissions without
canceling orders or selling positions; `cancel-open-orders` is a separate action;
and `resume-after-checks` requires fresh strategy inputs plus live reconciliation
and heartbeat checks when applicable. Alerts remain local and external messaging
is disabled.

```powershell
uv run --locked polybot status --config config.example.toml --pilot-config pilot.example.toml
uv run --locked polybot pause-new-orders --config config.example.toml --pilot-config pilot.example.toml --reason "operator review"
uv run --locked polybot resume-after-checks --config config.example.toml --pilot-config pilot.example.toml --reason "checks reviewed"
uv run --locked polybot cancel-open-orders --config config.example.toml --pilot-config pilot.example.toml --reason "cancel paper orders"
uv run --locked polybot emergency-halt --config config.example.toml --pilot-config pilot.example.toml --reason "unexpected exposure"
```

```powershell
Copy-Item preflight.example.toml preflight.local.toml
# Populate the named variables from your protected local environment/secret store.
uv run --locked polybot live-preflight --preflight-config preflight.local.toml
```

```powershell
uv run --locked python -m unittest discover -s tests -v
uv pip check
```

Tests use synthetic canary strings, temporary directories and offline subprocesses.
See [PROJECT_STATE.md](PROJECT_STATE.md) for observed verification and remaining work.
