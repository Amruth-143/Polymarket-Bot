# Project state

Updated: 2026-09-30 UTC. Completed tasks: **Task 1 — scope; Task 2 — API contract; Task 3 — Python/configuration/logging/CLI foundation; Task 4 — shared models and persistent accounting; Task 5 — public discovery and reviewed paper pilot; Task 6 — validated order-book collection; Task 7 — depth-aware quote economics; Task 8 — traceable manual forecast ingestion and strategy research protocol; Task 9 — deterministic trade-intent strategy and no-trade audit records; Task 10 — atomic risk gate, durable loss halts, and broker submission boundary; Task 11 — deterministic depth-consuming paper broker; Task 12 — chronological event-driven replay and reproducible run manifests; Task 13 — traceable accounting and performance reporting; Task 14 — persistent public-data paper-pilot operations and preregistered evaluation; Task 15 — read-only live-account preflight; Task 16 — disabled-by-default durable live broker; Task 17 — startup/periodic reconciliation, order heartbeat, and settlement tracking; Task 18 — persistent local operator controls and exposure status**.

## Repository baseline and completed work

- Inspected the workspace, tracked files, Git status, and applicable ancestor instructions before changes. Baseline contained only `.git`; no source, tests, dependency manifest, `PROJECT_STATE.md`, or applicable `AGENTS.md` was found.
- Created [docs/requirements.md](docs/requirements.md): scope, unresolved operator choices, functional requirements, acceptance gates, non-goals, and future live prerequisites.
- Asked for operating country, product/account type, category, strategy, OS, coding experience, holding period, paper bankroll, potential live bankroll, and a separate maximum acceptable loss. Operator replied: “country Dubai / international prediction markets, crypto, whether prediction”. Recorded international prediction markets as selected and Dubai as the stated location. Asked for country confirmation (United Arab Emirates) and clarification of crypto versus weather as the single category; remaining choices are unresolved in the scope table.
- First strategy research protocol is specified in `docs/strategy.md`; there is no validated forecasting model or trading advantage. No real forecasts, fills, historical datasets, strategy results, or profitability claims have been created. Task 4 and Task 8 tests use explicitly synthetic fixtures only.
- Task 2: read existing scope/state before changes; created [docs/api-contract.md](docs/api-contract.md) with dated official sources, release-source findings, installed SDK signatures, adapter design, simulator plan, and an explicit gap register. Requirements and trading behavior remain unchanged.

## Decisions and public contracts recorded through Task 2

The following is the Task 2 baseline. Task 3 additions and current interfaces are recorded below.

- Paper-only prototype; exactly one binary-market category, selection pending. No live deployment or live authorization.
- Budget means allocation; acceptable loss is a separately configured constraint. No monetary defaults selected.
- International prediction markets selected; account/wallet type and eligibility pending. Polymarket US requires a distinct integration and is outside this scope. Perpetuals and the other exclusions in the scope are outside initial implementation.
- Preserve the proposed file responsibilities: `docs/{requirements,api-contract,strategy}.md`; `src/polybot/{config,models,storage,discovery,market_data,costs,signals,risk,orders,reconcile,backtest,reporting,cli}.py`; `src/polybot/brokers/{base,paper,live}.py`; matching `tests/` modules and sanitized fixtures. Requirements and API contract documents now exist; no source modules exist.
- Justified Task 2 file-map addition: reserve `src/polybot/exchange.py` for `ExchangeAdapter` and `PolymarketExchangeAdapter`, keeping all SDK/REST/authentication details behind one boundary. Section 8 of the API contract defines proposed operations, domain results, errors, and ownership; these are our design names, not invented SDK methods. No executable implementation exists yet.
- Verified package baseline: `polymarket-client==0.11.0`, import `polymarket`, requires Python >=3.11. CPython 3.12.14 temporary installation and imports passed; default MSYS 3.14.5 was not validated for SDK compatibility. No project dependency manifest/lock was added.
- No supported public hosted sandbox found in reviewed official sources; plan our own explicitly labeled simulator in `brokers/paper.py`. No sandbox endpoint is assumed. Hosted sandbox availability remains a gap.
- SDK order-heartbeat method is absent; future adapter uses documented authenticated REST `/v1/heartbeats`. WebSocket PING/PONG is separate. pUSD collateral has six decimals; analytics' USDC labels and server fee rounding must not be conflated with asset accounting.
- Required invariants: `Decimal` financial values, UTC timestamps, stable identifiers, explicit persistent state transitions, atomic accounting, separate paper/live balance stores, untrusted external text, and no credentials in chat/logs/commits.

## Research and verification

- Began with https://docs.polymarket.com/llms.txt; checked the official Python SDK and SDK overview pages linked in requirements on 2026-09-23. Both identify `polymarket-client`. Markdown-page retrieval failed; corresponding HTML pages loaded successfully.
- Task 1 installed no SDK. Task 2 rechecked the official index, PyPI metadata, GitHub release/manifest, and eligibility/authentication pages on September 24, and inspected official market, trading, fees, settlement, maintenance, limit, and stream documentation. API contract links the exact sources and distinguishes documentation, source, observations, design, and gaps.
- Task 2 downloaded the official 0.11.0 wheel into the OS temporary audit directory, verified its SHA-256 against PyPI, extracted/read source, and installed in a separate temporary CPython venv. No client was instantiated or authenticated. No credentials, approvals, orders, cancellations, redemption, or money movement occurred; no production account behavior was tested.
- Inspection commands: `Get-Location`, `rg --files --hidden` with dependency/Git exclusions, `Get-ChildItem -Force`, ancestor `AGENTS.md` lookups, `Test-Path PROJECT_STATE.md`, `git status --short`, and `git ls-files`. File search found no files; subsequent listing confirmed the empty baseline. A lookup batch returned exit 1 for absent instruction paths; the final Git inspection returned exit 0 with no output.
- Task 1 validation: PowerShell required-topic assertions and full `Get-Content` readback passed. `git diff --check` returned exit 0, but did not inspect the untracked files; a separate PowerShell trailing-whitespace check covered both documents. At that time, `git status --short` showed only the two requested documents as new. No financial/state implementation changed; no test suite existed.

### Task 2 commands and observed results

- Repository reads: `Get-Content PROJECT_STATE.md`, `Get-Content docs/requirements.md`, `rg --files --hidden -g '!.git/**'`, `git status --short` confirmed only Task 1 documents and no code/tests.
- Research: browser open/search plus `Invoke-WebRequest` / `Invoke-RestMethod` fetched official docs, PyPI JSON/wheel, GitHub latest release, and tagged `pyproject.toml`; `rg` / `Get-Content` inspected downloaded release source. Initial sandbox HTTPS failed; approved elevated read/download calls succeeded. Incorrect tentative GitHub `v0.11.0` URL did not load; the actual verified tag is `polymarket-client-v0.11.0` and all contract links use it.
- Runtime: `python --version`, `Get-Command python`, `python -c` and `py -0p` identified MSYS Python 3.14.5 and no launcher-managed Python. Initial `python -m venv` created a `bin` layout, so invoking `Scripts/python.exe` failed. Used bundled CPython to run `-m venv` in a separate temporary directory, then `-m pip install` on the verified local wheel: succeeded.
- Offline SDK checks: `inspect.signature` verified the public/secure call shapes, stream specs, and paginator methods; reflection confirmed no public heartbeat method and no `get_market_resolution`, with `get_resolutions` present instead. `python -m pip check`: **No broken requirements found**. `Get-FileHash -Algorithm SHA256`: wheel matched PyPI.
- Documentation-only task: no financial or state-transition code changed; financial behavioral tests would not exercise a new implementation. Contract coverage, release-link paths, whitespace, and signature-shape assertions are the relevant checks. No live tests or upstream metered integration tests were run.
- Final offline Python assertions passed: 26 required topics, 9 SDK parameter shapes, 14 distinct linked release-source paths matched extracted wheel files, and whitespace/fence checks on both changed documents. `git diff --check` returned exit 0 (untracked-file limitation covered by explicit content checks). Git status still shows the uncommitted documentation set; Task 2 changed only `docs/api-contract.md` and this state file.

## Task 3 implementation and public interfaces

- Created `pyproject.toml`, `uv.lock`, `.python-version`, `.gitignore`, `README.md`, `config.example.toml`, `src/polybot/{__init__,__main__,config,logging,cli}.py`, `tests/test_{config,logging,cli}.py`, and the implementation plan in `docs/superpowers/plans/2026-09-24-project-foundation.md`. Updated this state file; retained prior scope/API documents. No adapter, broker, strategy, fills, or financial ledger implemented.
- Project supports Python 3.12; `.python-version` selects verified CPython 3.12.14. `polymarket-client==0.11.0` is pinned; `uv.lock` records 35 packages including polybot, artifact hashes and the public PyPI registry. Default MSYS Python 3.14.5 is outside the supported project constraint. Build backend is hatchling 1.27.0.
- `config.load_config(path: Path) -> BotConfig`, `BotConfig.from_mapping(raw, *, base_directory)`, and `validate_startup(config, environ)` are public configuration interfaces; `ConfigError` provides safe diagnostics. Frozen nested PriceConfig/RiskConfig/FreshnessConfig/StorageConfig hold typed values. Financial inputs are quoted positive fixed-point strings, at most six decimals, parsed directly to Decimal. Units are pUSD and pUSD/share.
- Validation: `0 < min_price <= max_price < 1`; order exposure <= market exposure <= total exposure <= bankroll; independent positive max loss <= bankroll. Positive integer seconds/counts bounded by 2147483647; per-market order count <= total. Unknown/missing fields and nonfinite/float inputs rejected. Relative paper/live directories resolve against the config file and cannot alias or nest. No directories/balances are created.
- Mode defaults to paper. All live startup/config-check/status requests fail with prerequisites, including explicit authorization, eligibility, wallet/authentication, strategy, limits, reconciliation and live broker implementation. No credential presence or configuration boolean enables live. `POLYBOT_MODE` overrides are rejected.
- `cli.main(argv=None) -> int`: `config-check`, `status`, `run [--once]`, help/version. Default file is `config.local.toml`; the example needs `--config config.example.toml`. `status` describes configuration only. `cli.run(config, once=False)` gates mode again and emits STARTING/RUNNING/STOPPED; Ctrl+C/SIGTERM stop cleanly and restore handlers. Exit codes: 0 success, 2 config/usage/live rejection, 1 unexpected failure.
- Justified module additions: `logging.py` owns `configure_logging(environ)`, `RedactingFormatter`, and `redact`; `__init__.py`/`__main__.py` supply package/module entry points. Final output redacts registered environment secret values, credential key/value pairs, bearer/Basic tokens, private-key hex and URL passwords. UTC logs omit traceback source/locals. Arbitrary unlabeled secrets must never be logged.
- Only `purpose="smoke"` is currently supported. Example amounts, limits, and lifetime equity-drawdown semantics are explicitly synthetic inputs, not operator preferences. Limits are validated but not enforced against trades; there is no trading loop or simulator yet. Financial accounting/risk implementation requires its own failing behavioral tests.

### Task 3 commands and observed results

- Read repository, PROJECT_STATE, scope and API contract before changes; reopened official `llms.txt` and Python SDK page before installation. Installed SDK metadata/import and `inspect.signature(AsyncPublicClient.get_order_book)` confirmed 0.11.0 and the prior contract. No SDK client instantiated.
- Initial bundled-CPython `-m unittest discover -s tests -v` with `PYTHONPATH=src`: 3 missing-package import errors before implementation. First implementation: 17 tests passed. Added Basic-auth/generic SDK-key regression cases, observed two failing subcases, fixed redaction; added SIGTERM/handler-restoration coverage. Final suite: **19 tests passed**.
- `uv --version`: 0.12.0. `uv lock --python <bundled-CPython-path> --default-index https://pypi.org/simple --cache-dir .uv-cache` initially hit sandbox network denial; elevated retry resolved 35 packages and wrote the lock. `uv sync --locked --python <bundled-CPython-path> --cache-dir .uv-cache` created `.venv` successfully.
- Ran README commands: `uv run --locked polybot config-check/status/run --config config.example.toml` (run with `--once`), `uv run --locked python -m unittest discover -s tests -v`, `uv pip check`. Initial sandbox cache access failed; elevated retry passed: valid paper JSON, clean lifecycle, **19 passing tests**, **all 35 packages compatible**.
- Fresh non-editable installation: `UV_PROJECT_ENVIRONMENT=.venv-smoke`, `uv sync --locked --no-editable --python <bundled-CPython-path> --cache-dir .uv-cache` created a new venv and installed a built wheel. Its `polybot.exe config-check`, `polybot.exe run --once` (with example config) and `python -m unittest discover -s tests -v` passed; **19 tests passed again**. Tests strip wallet/config environment variables for subprocess smoke runs and reject socket creation for the in-process smoke.
- `git diff --check` passed; `git check-ignore` confirmed both venvs, local cache, `.env`, and local config are ignored. Explicit new-file whitespace/content checks supplement the diff check because files remain untracked. No commits, credentials, exchange calls, or orders were made. A fresh machine's Python download was not exercised: used existing CPython via the documented explicit-interpreter option.

## Task 4 implementation and public interfaces

- Read current code and state before changes; recorded the interface/accounting plan before implementation. Added `src/polybot/models.py`, `src/polybot/storage.py`, `tests/test_models.py`, `tests/test_storage.py`, `docs/model-storage-contract.md`, and `docs/superpowers/plans/2026-09-24-models-storage.md`; updated README and this state file. Existing CLI/configuration behavior and dependency pins are unchanged.
- Frozen, keyword-only, runtime-validated shared models: Market, Outcome, BookSnapshot (with BookLevel), Forecast, TradeIntent, RiskDecision, Order, Fill, Position, PortfolioSnapshot. Supporting RawDataReference, BalanceMovement and ReconciliationResult make provenance/accounting explicit. Fields and exact constructor names are recorded in the model/storage contract; `models.py` is the typed source of truth.
- All financial inputs are finite Decimal, persisted as text without scale loss; no float or SQLite financial SUM/REAL. UTC-aware timestamps only. Caller supplies stable IDs. Decimal inputs have a 120-digit coefficient/exponent [-120,120] bound; arithmetic runs at precision 1024 independently of caller context; out-of-bound results roll back rather than rounding silently. Position net cash flow is not realized P&L, cost basis or marked equity.
- `storage.Ledger.open(directory: Path, mode: RunMode = PAPER)` creates mode-named SQLite stores and validates the database mode marker. Use corresponding validated config directories. Context manager/close release resources. Schema v1 records mode, raw references, metadata versions, forecasts, intents, decisions and orders; v2 adds cash/positions/reservations/fills/movements/reconciliation. Transactional versioned migrations reject future versions.
- Public writes: record_raw, record_market, record_forecast, record_intent, record_decision (including rejected decisions), record_reconciliation, initialize_cash, submit_order, apply_fill, cancel_order. These are local database operations, never exchange calls. Public reads: typed get_record overloads, get_market, get_order, count, schema_version, snapshot. `snapshot(at)` labels current state, not a historical replay query.
- Local decisions have primary-key uniqueness; external fill IDs and movement references have UNIQUE constraints. External fill identity must distinguish account/order execution legs, not only an upstream multi-leg trade. Identical external-ID replay returns False even after restart; changed economics/order/confirmation time raises StorageError. Original evidence is retained when a replay has a different local receipt ID/raw reference.
- BUY reserves cash (including a caller-provided fee buffer); SELL reserves owned shares. Order creation/reservation, cancellation/release, and fill/cash/position/movement/order/reservation updates are atomic BEGIN IMMEDIATE transactions. No short selling or spending reserved funds. Fill transitions OPEN -> PARTIALLY_FILLED -> FILLED (or direct FILLED); cancellation is terminal locally. Unseen fills after cancellation require later reconciliation support. Fill accepts normalized confirmed pUSD fees, not estimated or pending exchange events.
- Reopened official docs starting with llms.txt and reviewed order-update, Python sqlite3 and SQLite transaction references. No new SDK integration/method assumptions or installs were needed; no client, credential, wallet action or order was created. The existing live-startup block remains intact. Opening an isolated offline LIVE database is not live deployment.

### Task 4 commands and observed results

- Baseline `.venv\Scripts\python.exe -m unittest discover -s tests -v`: **19 tests passed**. New storage behavioral suite was written before implementation; running with `-p test_storage.py -v` failed with missing `polybot.models`, as expected.
- Initial implementation run: accounting assertions passed, but three tests failed at Windows temporary-file cleanup because inspection sqlite connections were still open. Fixed the tests to close those connections explicitly. Added model validation, migration/reservation rollback, multi-connection reservation visibility, invalid fill/no-short and low-global-precision cases.
- Final `.venv\Scripts\python.exe -m unittest discover -s tests -v`: **37 tests passed**, including duplicate fill replay after reopening, one cash movement per fill, exact Decimal round-tripping, metadata version preservation, rejected decisions, upgrade/reopen/future-version migration handling, injected migration/fill/reservation failures with full rollback, separate modes, cancellation and BUY/SELL holdings.
- `.venv\Scripts\python.exe -m polybot run --config config.example.toml --once`: **exit 0**, STARTING/RUNNING/STOPPED in paper mode with trading disabled. `.venv\Scripts\python.exe -m compileall -q src tests`: **exit 0**. `git diff --check`: **exit 0**, with the existing untracked-file limitation. Direct new-file whitespace checking also passed. No dependency lock changes or commits.

## Task 5 implementation and public interfaces

- Operator explicitly selected **Crypto** during Task 5. Updated requirements; country/account, strategy, holding period and budget/loss choices remain unresolved. Read prior state, code and contracts before changes and wrote the discovery plan before implementation.
- Added `exchange.py`, `discovery.py`, `tests/test_discovery.py`, `discovery.example.toml`, `pilot-allowlist.json`, `docs/discovery.md`, `docs/pilot-review.md`, and `docs/superpowers/plans/2026-09-25-discovery.md`. Extended models, SQLite storage, CLI and CLI/storage tests; updated README, requirements, API contract and this state. Dependencies and lockfile unchanged.
- Reverified current official documentation from llms.txt, downloaded Gamma/CLOB schemas, and inspected installed SDK 0.11.0 signatures/source. SDK's Gamma parser names array index 0 `yes` without checking labels, and list parsing drops nonbinary records. Implemented the documented public REST paths behind the existing planned exchange boundary to preserve rejection evidence and validate labels. No private SDK APIs, credentials, authenticated clients or account endpoints are used.
- `exchange.ExchangeAdapter.page(category, page_size, cursor, observed_at, *, market_ids=()) -> DiscoveryPage`; PolymarketExchangeAdapter resolves category tag, pages `/markets/keyset` using opaque cursors, and reads `/clob-markets/{condition_id}`. JSON financial numbers decode directly to Decimal. Public HTTP only, bounded response size/timeouts, safe errors, no automatic retries or redirects.
- `discovery.DiscoveryPolicy`, `Review`, `DiscoveryReport`, `load_discovery_config`, `discover`, `render_shortlist` define selection/report contracts. Reject closed/inactive/resolving/non-order-accepting/unsupported markets, missing rules/source/metadata, unknown mappings, past end times and disagreeing Gamma/CLOB constraints. Explicit labels paired with token IDs must independently match CLOB labels. Volume is only an initial activity filter; book liquidity/price suitability is not tested.
- `models.MarketMetadata` wraps the original Market without breaking its constructor. Stores separate event, market, condition and labeled token identities, complete wording/rules/source, scheduled UTC end, ordinary-v1 type, volume, and Decimal FeeConfiguration. Report event_groups exposes related market IDs for later exposure control. No risk-limit enforcement is added.
- Schema 3 adds market_metadata and discovery_runs. `Ledger.record_discovery(report) -> DiscoveryReport` atomically persists raw references, canonical response evidence (including duplicates), immutable metadata versions and all inclusion/exclusion decisions. `get_discovery_json(id)` and `get_market_metadata(id, version)` read back records. Semantic metadata changes create versions; persistent identity changes are excluded. Migration preserves existing fill/account balances.
- `polybot discover --config ... --discovery-config ...` prints and persists a paper report without initializing cash. `--pilot-only` uses the same paginated API filtered to reviewed market IDs and explicitly labels that scope. Catalogue page caps/repeated cursors/page failures exclude all candidates and exit 1; a complete scan exits 0 even if empty. Live mode still exits 2 before discovery. Missing/malformed reviews reject startup; unreviewed/changed metadata cannot be included.
- A reviewed allowlist entry pins market **3849939**, event **905012**, its independent token mapping and full metadata hash, reviewed by Codex at 2026-09-25T11:20:09Z. Full review basis is in docs/pilot-review.md. This is paper metadata approval only, not a forecast, strategy recommendation, liquidity assessment or permission to submit orders.

### Task 5 commands and observed results

- Baseline `.venv\Scripts\python.exe -m unittest discover -s tests -q`: **37 passed**. Initial new discovery test invocation failed with missing `polybot.discovery`, before implementation. Additional behavioral tests exposed a duplicate-volume filter bypass and acceptance of a conflicting CLOB negative-risk flag; both failed before their fixes.
- Read official docs with web open; `Invoke-WebRequest` downloaded official schemas after an initial sandbox TLS failure. `inspect.signature(AsyncPublicClient.list_markets)` and `Get-Content`/`rg --no-ignore` checked installed SDK parser and CLOB source. No SDK installation or upgrades.
- Real `python -m polybot discover --config config.example.toml --discovery-config discovery.example.toml` first failed public reads and persisted incomplete runs. Diagnostic tag reads later returned HTTP 200; the unchanged command then fetched **200 records across 10 pages**, capped, exit 1, no inclusions.
- A local ignored discovery config using page_size=100 and max_pages=50 fetched **5,000 records**, capped, exit 1. **31** passed metadata normalization; none were included before review. Exact exclusions, IDs and timestamps are in docs/pilot-review.md. This is not a complete catalogue or synthetic data.
- `python -m polybot discover --config config.example.toml --discovery-config discovery.example.toml --pilot-only`: after one intermittent read failure, unchanged retry completed **1 page, 1 included reviewed market, exit 0**, run eaa32153-c2c7-4608-9076-e3a7909ce078. Readable outputs are ignored `data/paper/task5-shortlist.txt` and `data/paper/task5-pilot-shortlist.txt`; audit evidence lives in the paper DB. No balance initialization, orders or fills occurred.
- Final `.venv\Scripts\python.exe -m unittest discover -s tests -v`: **51 tests passed**. Tests cover reversed outcomes, multiple pages, duplicates, changed rules, missing tokens, lifecycle/type/source constraints, per-event grouping, Decimal decoding, allowlist validation, scoped pilot queries, persistence/restart and schema-2 accounting preservation. `.venv\Scripts\python.exe -m polybot run --config config.example.toml --once`: **exit 0**, paper STARTING/RUNNING/STOPPED. `python -m compileall -q src tests` and `git diff --check`: **exit 0**. Direct changed-file whitespace checks supplement the untracked-file diff limitation. `git check-ignore` confirmed the local scan config, DB and generated report are ignored. No commits or lock changes.

## Task 6 implementation and public interfaces

- Re-read repository/state first and started official verification at llms.txt on 2026-09-25. Reviewed current real-time, raw market-channel and order-book documentation and inspected installed SDK 0.11.0 signatures/source. `AsyncPublicClient.get_order_book(*, asset_id=None, token_id=None)` and `MarketSpec(*, asset_ids=None, token_ids=None, custom_feature_enabled=False)` remain the verified SDK shapes. No package or lockfile changed.
- Added `market_data.py` with `BookCollector`, `BookStatus`, `CollectorPolicy`, `StreamMessage`, `MarketDataSource`, `BookUnavailable`, and `run_collector`. The strategy-facing gate is `require_trading_book(now)`: it raises unless the connection is healthy and the book is valid, fresh and open. Book freshness, connection health and last price change are independent state.
- Added `PolymarketMarketDataSource` behind `exchange.py`: public `/book` snapshot plus documented market WebSocket subscription with custom lifecycle events, application PING/PONG, bounded transport buffers, read timeouts and Decimal JSON parsing. It creates no credentials and has no order methods.
- The collector subscribes before snapshot, validates buffered handoff events by exchange timestamp, ignores provably pre-snapshot events, and requires a new snapshot after any detected gap. Exact duplicates are retained as duplicate audit records. No documented sequence or hash-chain rule exists; optional hashes are retained as evidence and never assigned guessed continuity semantics.
- Validation rejects prices outside `(0,1)`, off-tick prices, negative sizes, duplicate levels, crossed/locked books, identity mismatches, malformed/unknown events, timestamp regressions and invalid tick transitions. Queue overflow, disconnect, tick-size change and market resolution invalidate the book; resolution permanently closes it. A tick change updates the required tick before resnapshot, including when the documented optional old-tick field is absent.
- `run_collector` uses a bounded application queue, finite connection attempts, positive capped backoff and minimum snapshot-request spacing. An injected disconnect test proves the state becomes unusable, waits, reconnects, fetches a new snapshot and applies later data. Exhaustion leaves the book unusable.
- SQLite schema 4 adds append-only `market_data_events` and current `book_states`. Each event preserves a local sequence, raw canonical JSON, optional exchange UTC time, local receipt UTC time, disposition and reason. Full resulting books preserve Decimal text through the existing typed serializer. Invalidation and deletion of the latest usable pointer occur atomically; replay history remains. Paper/live stores remain separate.
- Added `tests/test_market_data.py`, `docs/market-data.md`, and the Task 6 implementation plan; updated storage migrations/tests, exchange adapter, API contract, README and this state. No CLI collector command, signal generation, broker, credentials, wallet operations or order submission was added.

### Task 6 commands and observed results

- Baseline before implementation: `.venv\Scripts\python.exe -m unittest discover -s tests -q` passed **51 tests**. The new behavioral test module was run before implementation and failed with `ModuleNotFoundError: polybot.market_data`, establishing the expected red state.
- Focused `.venv\Scripts\python.exe -m unittest tests.test_market_data tests.test_storage -v` passed **24 tests** after the initial implementation. The full suite exposed one migration-fixture error: its simulated schema-2 database retained schema-4 tables. Correcting the fixture to remove all post-v2 tables made the upgrade scenario valid. After optional stream-book, absent-exchange-time, optional old-tick and explicit schema-3-to-4 migration coverage, the focused set contains **28 passing tests**.
- Runtime SDK inspection first used the wrong public import path for `MarketSpec` and raised `ImportError`; the documented/exported `polymarket.streams.MarketSpec` path then returned the signatures recorded above. This was inspection only; no SDK client was instantiated.
- Final `.venv\Scripts\python.exe -m unittest discover -s tests -q`: **64 tests passed**. `.venv\Scripts\python.exe -m compileall -q src tests`: exit 0. `.venv\Scripts\python.exe -m polybot run --config config.example.toml --once`: exit 0 with paper STARTING/RUNNING/STOPPED and trading disabled. The first `uv pip check` could not read uv's global cache under the filesystem sandbox; rerunning with `UV_CACHE_DIR=.uv-cache` checked **35 packages** and reported all compatible. Changed-file trailing-whitespace checks and `git diff --check` returned exit 0. All market-data fixtures are synthetic. No production price, fill, strategy result or live-feed observation is claimed.

## Task 7 implementation and public interfaces

- Read current code/state/contracts before changes and started current verification at official llms.txt on 2026-09-25. Reviewed Fees, Market Details and Prices/Order Books, then inspected installed SDK 0.11.0 order rounding and fee-budget source. No package or lockfile changed.
- Added `costs.py`: `quote_shares(book, side, requested_shares, *, constraints, transaction_costs, planned_exit=None) -> ExecutionQuote`. Public immutable types are `QuoteConstraints`, `TransactionCost`, `PlannedExit`, `ExitCostEstimate`, and `ExecutionQuote`; errors are `QuoteError`, `MissingFeeInformation`, and `InsufficientLiquidity` with requested/available quantities.
- BUY walks ascending asks; SELL walks descending bids. Exact visible size, gross notional, VWAP, worst execution price and best spread are returned. Spread is informational and never added to the executable-price notional. Insufficient visible depth raises rather than extrapolating at the best quote.
- Requested shares are rounded upward to the verified two-decimal order-size grid. Every level must remain on the active supported tick and inside `[tick,1-tick]`; the two-sided book must be coherent. Active tick changes therefore cause stale-grid books to fail until Task 6 installs a matching snapshot.
- Current official Market Details text says `orderMinSize` is minimum pUSD notional. This corrects the earlier Task 2/5 interpretation as shares. To preserve serialized/database interfaces, `Market.minimum_order_shares` remains, with corrected read-only alias `minimum_order_pusd`; discovery output and new `QuoteConstraints.minimum_order_pusd` use the correct semantic name. Quotes enforce the minimum against walked gross notional.
- Fee estimate per visible level is `shares * rate * (price * (1-price)) ** exponent`; level results are summed and rounded to documented five-decimal precision. Enabled schedules require positive rate and taker-only semantics; disabled schedules require explicit zero rate. Missing/contradictory fee data never becomes zero. Maker/taker base-fee evidence is not added to the schedule formula. Maker/taker rebates and liquidity rewards are excluded from baseline economics.
- The official fee page does not define exact tie rounding or aggregation versus per-match rounding. SDK general rounding is half-even, but its budget estimator is not server-accounting proof. Paper quotes use half-even for non-ties and reject exact five-decimal ties. Live cost-dependent execution remains blocked on this gap.
- Transaction costs are explicit named pUSD-six-decimal values in a required tuple. Quotes return exchange fee, separate transaction total, total trading cost, trading cost per share, BUY total cash/all-in price, or SELL net proceeds/net price. Costs exceeding sale proceeds reject.
- Exit estimates are absent by default. `PlannedExit` must explicitly supply a same-market/version/outcome basis book, current constraints, complete fees and transaction costs; only then is the opposite side walked for the same normalized shares. This is a current-book scenario, not a forecast of exit price or liquidity.
- Added `tests/test_costs.py`, `docs/costs.md`, and the Task 7 plan. Updated models/discovery display, API/model/discovery contracts, README and this state. No intent, reservation, broker, credential, wallet operation or order submission was added.

### Task 7 commands and observed results

- Before implementation, `.venv\Scripts\python.exe -m unittest tests.test_costs -v` failed with `ModuleNotFoundError: polybot.costs`, the intended red state.
- Initial implementation produced seven passes and one error because the tick-change fixture used unsupported tick `0.05`. Replaced it with verified supported tick `0.1`, which correctly rejects the old `0.52/0.48` book grid. After the minimum-unit correction, the fee-tie fixture initially hit the notional minimum first; its explicit minimum was reduced so the intended tie guard was exercised.
- Focused `.venv\Scripts\python.exe -m unittest tests.test_costs -q`: **8 tests passed**. Hand calculations include BUY 5 shares across 3 @ 0.52 plus 2 @ 0.53: gross 2.62 pUSD, VWAP 0.524, fee 0.08729, explicit other cost 0.01 and total cash 2.71729; SELL 5 across 3 @ 0.48 plus 2 @ 0.47: gross 2.38, fee 0.08729, other cost 0.02 and net proceeds 2.27271.
- First full `.venv\Scripts\python.exe -m unittest discover -s tests -v` after implementation: **72 tests passed**. Final quiet full run also passed **72 tests**. `.venv\Scripts\python.exe -m compileall -q src tests`: exit 0. `.venv\Scripts\python.exe -m polybot run --config config.example.toml --once`: exit 0 with paper STARTING/RUNNING/STOPPED and trading disabled. Project-local-cache `uv pip check --python .venv\Scripts\python.exe` checked **35 packages**, all compatible. Changed-file trailing-whitespace checks and `git diff --check` returned exit 0. All quote fixtures are synthetic and prove arithmetic/state-independent validation only.

## Task 8 implementation and public interfaces

- Read the repository, prior contracts and current official documentation before changes, beginning with `https://docs.polymarket.com/llms.txt` on 2026-09-26. Reviewed current resolution, market-details and market/event documentation. Ordinary binary tokens settle to 1 or 0 according to the market rules; the documented rare Unknown/50-50 outcome pays both binary tokens 0.5. This supports complementing expected settlement payouts only for a verified two-token ordinary market with that exact settlement rule. It does not make arbitrary event-occurrence probabilities complementary.
- Added `docs/strategy.md`. There is no validated model, so the initial implementation is timestamped manual forecast ingestion solely for end-to-end pipeline testing. The falsifiable research hypothesis is that named primary-source crypto information may sometimes be incorporated slowly or incompletely into a reviewed prediction market; it must be tested against depth-aware executable prices and all known costs. This is a research proposition, not an observed edge or profitability claim.
- The pilot evidence source is the reviewed market's named Binance MEGA/USDT one-minute candle high. That source is documented but is not yet integrated as an automated feed or validated forecasting dependency. The forecast target is expected settlement payout; its research horizon runs from information availability through the applicable resolution window, while `expires_at` separately limits when the input may drive a decision. Holding period, entry/exit thresholds and automated probability generation remain unresolved.
- Added `signals.py` with public immutable inputs `ForecastEvidenceInput`, `ManualForecastSubmission`, and `SettlementSemantics`; enums `EvidenceKind` and `ProbabilityKind`; and operations `ingest_manual_forecast`, `eligible_forecast`, and `complement_probability`. `SignalError` is the fail-closed error contract.
- Manual probabilities must be finite `Decimal` values in `[0,1]`. Inputs require stable forecast, market, market-version and outcome IDs; UTC information/receipt/expiry times; source; a `manual-*` model version; and at least one evidence item. Ingestion rejects future information/retrieval times, evidence retrieved after the claimed information time, expired inputs, expiry at or before information time, invalid probability types/ranges, and incomplete news/LLM provenance.
- Evidence is canonical JSON with source, content, publication time where applicable, retrieval time, information/receipt/expiry times, forecast output and model version. LLM contributions additionally require stored model output; optional model confidence is retained as an unvalidated value and never used as calibration evidence. All external text is marked and treated as inert untrusted data: ingestion stores it and does not invoke tools, alter configuration or create orders.
- Evidence receives a SHA-256-addressed `RawDataReference`. SQLite schema 5 adds `forecast_evidence`; `Ledger.record_forecast_bundle(raw, evidence_json, forecast)` verifies the hash and atomically stores the raw reference, evidence and forecast, while `get_forecast_evidence(raw_id)` retrieves the exact payload. Stable-ID conflicts roll back the whole bundle; exact replay is idempotent. Paper/live databases remain separate.
- `eligible_forecast(..., as_of=...)` enforces information availability and local receipt time for chronological replay. Missing, expired, future-dated or ambiguous latest signals return `None`. The selector creates no `TradeIntent` or order, so missing signals cause no trade. News and LLM evidence types are supported only as provenance records; no news service or LLM is invoked.
- Added `tests/test_signals.py` and schema-4-to-5 migration coverage; updated storage/model contracts, requirements, API contract, README and this state. No CLI signal command, automated forecast, broker, credential, wallet operation or order submission was added. Dependencies and lockfile are unchanged.

### Task 8 commands and observed results

- Baseline before Task 8: the Task 7 full suite had **72 passing tests**. The new signal suite was written first and failed with `ModuleNotFoundError: polybot.signals`, establishing the intended red state.
- Focused `.venv\Scripts\python.exe -m unittest tests.test_signals tests.test_storage tests.test_market_data -q`: **36 tests passed** after implementation. An added rollback-inspection assertion initially left a SQLite connection open on Windows, causing temporary-directory cleanup to fail despite passing behavioral assertions; explicitly closing that inspection connection fixed the test resource leak.
- Final `.venv\Scripts\python.exe -m unittest discover -s tests -v`: **80 tests passed**. Coverage includes exact evidence persistence/restart, SHA-addressed atomic bundles, conflict rollback, probability boundaries, time validation, complete news/LLM provenance, restricted complement rules, schema migration, and no-signal behavior.
- `.venv\Scripts\python.exe -m compileall -q src tests`: exit 0. `.venv\Scripts\python.exe -m polybot run --config config.example.toml --once`: exit 0 with paper STARTING/RUNNING/STOPPED and trading disabled. Project-local-cache `uv pip check --python .venv\Scripts\python.exe` checked **35 packages**, all compatible. No production probability, market observation, LLM output, fill or profitability result was generated.

## Task 9 implementation and public interfaces

- Read the repository, state, model/storage contract and Task 8 strategy document before changes. Task 9 adds no exchange endpoint, SDK method, dependency or authenticated behavior; current official payoff behavior was already rechecked for Task 8 on 2026-09-26. No exchange client, credentials, wallet action, live request or order submission is reachable from the strategy API.
- Kept strategy decisions in `signals.py`, matching the proposed module contract. Added typed `StandardBinaryPayoff`, `StrategyPolicy`, `OutstandingOrder`, `StrategyResult`, and `evaluate_strategy(...)`. Added `HoldingMode`, `StrategyAction`, and durable `StrategyEvaluation` shared models. The strategy accepts data and returns an optional existing `TradeIntent`; it performs no I/O and has no adapter/ledger parameter.
- `StandardBinaryPayoff` accepts only a fully modeled ordinary two-outcome token paying 1 pUSD on win, 0 on loss and 0.5 in the documented exceptional 50/50 resolution. Any differing/unmodeled payoff creates an explained no-trade evaluation. Forecast probabilities remain expected settlement payout values, not invented event probabilities.
- BUY economics use Task 7 visible-depth quotes: expected settlement value is probability times shares; after-cost edge is value minus ask notional, estimated exchange fees and explicit transaction costs. A BUY requires the strict inequality `edge > quantity * uncertainty_buffer_pusd_per_share`; equality is no trade. Total cash required must fit the caller-supplied remaining pUSD risk budget.
- Sizing is fixed and bounded by configured target-position and maximum-per-intent shares. Projected position is held shares plus outstanding BUY shares minus outstanding SELL shares. The strategy requests only the remaining target gap, so unchanged inputs with a working order do not repeatedly buy. No Kelly sizing or probability-scaled sizing is used.
- Holding mode is explicit. `hold_to_resolution` emits no strategy SELL. `early_exit` quotes no more than unreserved inventory and the fixed order cap, then compares net sale proceeds with estimated remaining settlement value plus uncertainty, the supplied remaining risk budget, and maximum remaining resolution horizon. Valuation, risk-budget or horizon triggers are individually recorded; risk/horizon exits may accept worse forecast economics and label that reason.
- Every call returns `StrategyEvaluation`, including no-trade outcomes. It records forecast/book identity, action/mode, requested/target/projected shares, limit, settlement value, notional, fees and other costs, net sale proceeds, edge, uncertainty, risk budget, horizon, validity deadline, optional intent ID and explanation. Stable SHA-256 content IDs make identical input values and portfolio/order state reproduce the same evaluation and intent.
- SQLite schema 6 adds immutable `strategy_evaluations`. `Ledger.record_strategy_result(result)` validates forecast/intent linkage and atomically stores the evaluation with its optional intent. Exact replay is idempotent; conflicting records fail. A trigger-injected evaluation failure proves the newly inserted intent rolls back.
- Added `tests/test_strategy.py` and schema-5-to-6 migration coverage; updated strategy, requirements, model/storage contract, README, implementation plan and this state. The operator still must choose policy values and a holding mode before a paper trading loop is enabled. The lifecycle CLI remains smoke-only with trading disabled.

### Task 9 commands and observed results

- Baseline before Task 9: **80 tests passed**. The new strategy suite was written first and failed with `ImportError: cannot import name 'HoldingMode'`, establishing the intended red state.
- Initial implementation exposed a canonical-ID serializer omission for dictionaries; all eight strategy tests failed before that fix. After recursive dictionary serialization, the focused strategy suite passed. Schema 6 then made six existing migration expectations fail; updating historical fixtures to remove post-version tables and adding a direct v5-to-v6 preservation test resolved them.
- Focused `.venv\Scripts\python.exe -m unittest tests.test_strategy tests.test_storage -q`: **27 tests passed**. Final `.venv\Scripts\python.exe -m unittest discover -s tests -v`: **90 tests passed**. Coverage includes cost-erased gross edge, strict threshold equality, expired forecasts, unsupported payoff, deterministic identity, outstanding-order target behavior, three early-exit comparisons, inventory bounds, atomic persistence/restart, rollback and migration.
- `.venv\Scripts\python.exe -m compileall -q src tests`: exit 0. All strategy fixtures and forecasts are synthetic. No strategy performance, production probability, exchange fill or profit result is claimed.

## Task 10 implementation and public interfaces

- Read the repository, current state, accounting, strategy, configuration and API contracts before changes. Task 10 adds no exchange call, SDK method, credential handling, wallet action or live submission. No dependency or lockfile changed.
- Added `risk.py` with immutable `RiskPolicy`, `LiquidationMark`, `ExposureGroup`, `GateResult`, `RiskGate`, and `RiskError`. The policy has no financial defaults and requires an explicit typed run mode, permitted price range, daily timezone (`UTC`, explicit `UTC±HH:MM`, or an available IANA zone), daily-loss and funding-adjusted peak-drawdown amounts, order/market/group/total exposure limits, open-order limits, mark freshness and strategy-evaluation freshness. Policy, intent and isolated ledger modes must match, so paper limits cannot silently become future live limits. No computer timezone is inferred.
- Added `brokers/base.py` and package exports. `RiskGatedBroker.submit` is the only broker submission workflow and cannot be overridden by subclasses. It records the strategy result, invokes the atomic gate, and calls `_submit_reserved` only with an approved reserved order. Rejection never reaches transport. Unknown submission responses mark the order UNKNOWN and retain reservation. There is no concrete matching or live broker.
- BUY reservation is `shares * limit_price + estimated_exchange_fee + explicit_transaction_costs` from the linked immutable strategy evaluation. SELL reservation is the full requested share count. Risk approval, decision persistence, order creation and reservation share one `BEGIN IMMEDIATE` transaction. A two-thread test with separate SQLite connections proves only one of two 2.50 pUSD candidates can reserve a 3.00 pUSD balance.
- Extended order states with CANCEL_PENDING, UNKNOWN and REJECTED. `request_cancel` and `mark_order_uncertain` retain remaining cash/shares. `confirm_cancelled` alone releases the reservation; legacy `cancel_order` now explicitly means confirmed cancellation. Confirmed fills continue to apply during CANCEL_PENDING/UNKNOWN. Partial fills decrement only confirmed cash/fee or share use and keep the remainder reserved.
- Worst-feasible exposure equals filled shares plus all remaining BUY shares for OPEN, PARTIALLY_FILLED, CANCEL_PENDING and UNKNOWN orders plus the candidate BUY. Pending SELL orders receive no exposure credit. Market, explicitly related event/group, and total gross limits add exposures at the standard binary one-pUSD maximum payout. No cross-market or complementary-outcome offset is recognized as a guaranteed hedge.
- Schema 7 adds `risk_days`. The first complete fresh observation in an explicit local day persists starting equity and external-flow baseline. Schema 8 adds singleton `risk_account_state` with starting funding-adjusted equity, peak adjusted equity and an account-level halt. Current equity is cash plus exact-quantity executable liquidation values. Confirmed fills/fees affect cash; unrealized value comes only from supplied marks. Missing, future, quantity-mismatched or stale marks block new BUY risk rather than becoming zero/last-known marks or changing the peak.
- `Ledger.adjust_cash` records explicit paper deposits (positive) and withdrawals (negative); withdrawals cannot consume reservations. Daily P&L is current equity minus starting equity minus net external flows after baseline, so deposits cannot hide loss and withdrawals cannot create it. Lifetime adjusted equity subtracts all deposits/withdrawals before comparison with the persisted peak. A daily-loss or drawdown threshold breach at equality or worse persists the corresponding halt. Restart does not clear either halt.
- The gate revalidates the intent's permitted limit price and the linked strategy evaluation's timestamp and expiry immediately before reservation. Per-order exposure uses maximum standard-binary payout (`shares * 1 pUSD`), while free-cash reservation uses limit notional plus fees and explicit costs. Active-order limits count OPEN, PARTIALLY_FILLED, CANCEL_PENDING and UNKNOWN orders.
- A halt blocks new BUY risk. It does not prove remote cancellation, automatically liquidate positions or guarantee a loss cap. Cancellation acknowledgements and position handling are separate: risk-reducing SELLs can still be assessed from owned unreserved shares, while fills, gaps and missing liquidity can take loss beyond the threshold. Full definitions are in `docs/risk.md`.
- Added `tests/test_risk.py`, schema-6-to-7 and schema-7-to-8 migration coverage, Task 10 plan and risk documentation; updated shared contracts, requirements, README and this state. All test books, fills, receipts, deposits and marks are synthetic.

### Task 10 commands and observed results

- Baseline before Task 10: **90 tests passed**. The new risk suite was written first and failed with `ModuleNotFoundError: polybot.brokers`, establishing the intended red state.
- The first implementation run failed all risk cases because this Windows Python installation has no IANA timezone database. The policy was made explicit and portable: it supports `UTC` and fixed `UTC±HH:MM` without an added package, and available IANA names when timezone data exists. Tests use the declared synthetic `UTC+04:00`; no location was inferred.
- Focused `.venv\Scripts\python.exe -m unittest tests.test_risk tests.test_storage tests.test_market_data -q`: **38 tests passed** before final broker subclass hardening. After making `submit` non-overridable, `.venv\Scripts\python.exe -m unittest tests.test_risk tests.test_storage -q`: **27 tests passed**.
- First full run after schema 7 exposed the expected stale schema-6 assertions and historical migration fixtures. Updating those fixtures to remove `risk_days` and adding direct schema-6-to-7 accounting preservation resolved them. The authoritative Task 10 follow-up then added permitted-price, strategy-freshness, active-order, per-order exposure and peak-drawdown behavioral tests before implementation; the first focused run failed because the new required policy fields did not yet exist.
- Final `.venv\Scripts\python.exe -m unittest discover -s tests -v`: **102 tests passed**. Focused `.venv\Scripts\python.exe -m unittest tests.test_risk -q`: **10 tests passed**. `.venv\Scripts\python.exe -m compileall -q src tests`: exit 0. `.venv\Scripts\python.exe -m polybot run --config config.example.toml --once`: exit 0 with paper STARTING/RUNNING/STOPPED and trading disabled. Workspace-cache `uv pip check --python .venv\Scripts\python.exe` checked **35 packages**, all compatible. `git diff --check` and explicit repository trailing-whitespace checks passed. No production prices, marks, fills, broker receipts, probabilities or performance results were generated.

## Task 11 implementation and public interfaces

- Read the repository, `PROJECT_STATE.md`, strategy, risk, market-data, cost, model and storage contracts before changes. Wrote `docs/superpowers/plans/2026-09-27-paper-broker.md`, then wrote the Task 11 behavior suite first. Its initial invocation failed with `ModuleNotFoundError: polybot.brokers.paper`, establishing the intended red state.
- Added exchange-independent `Broker` protocol while retaining non-overridable `RiskGatedBroker.submit`. Added `PaperBroker`, `PaperBrokerPolicy` and `PaperAdvanceResult`. The paper implementation accepts only an isolated PAPER ledger and has no SDK/exchange-adapter import or authenticated endpoint path.
- The first strategy order is now explicit as LIMIT GTC through defaulted `OrderType` and `TimeInForce` fields, preserving old stored payload compatibility. `FillSource` distinguishes exchange records from `SIMULATED`; every paper-generated fill requires the latter.
- Approved orders schedule at `created_at + latency`. `advance(at)` resolves the latest usable recorded book at the exact simulated arrival time, independent of when advance is called. It consumes only asks at/below a BUY limit or bids at/above a SELL limit, walks price priority, floors to 0.01 shares, permits partial fills and uses Task 7 fee calculation/rounding. Resulting fills use exact VWAP.
- A partial or nonmarketable remainder becomes resting and retains its cash/share reservation. Later displayed touches never fill it because queue position, trade prints and aggressor-side evidence are unavailable. Explicit delayed cancellation is required to release the remainder. Cancellation wins an equal-timestamp race; an earlier arrival can fill before cancellation becomes effective.
- Schema 9 adds `paper_orders`, `paper_depth_consumption`, and canonical `paper_fill_evidence`. Book/side/price consumption, evidence, fill, cash, position, reservation and schedule state change in one SQLite transaction. Persistent consumption prevents other orders or restarted workers from reusing unchanged snapshot liquidity. `Ledger.book_at` fails closed when the latest eligible event invalidated the book.
- Public paper interfaces: `PaperBroker.submit(...)` through the inherited risk gate; `cancel(order_id, requested_at=...)`; `advance(at) -> PaperAdvanceResult`; `Ledger.book_at`; `Ledger.apply_paper_execution`; and `Ledger.get_paper_fill_evidence`. Complete assumptions and limitations are in `docs/paper-broker.md`.
- Behavioral coverage includes insufficient liquidity/partial fill, two orders competing for one snapshot, persistence across restart, latency price movement, no touch-only resting fill, both cancellation-race orderings, reservation release, explicit simulated labels/fees/evidence, invalidated replay state, live-ledger rejection and deterministic independent replay.

### Task 11 commands and observed results

- Focused red test: `.venv\Scripts\python.exe -m unittest tests.test_paper_broker -v` failed at import because `polybot.brokers.paper` did not exist.
- First implemented focused run: **5 tests passed**. Added restart/live-isolation and invalidation behavior; final focused suite: **7 tests passed**.
- Schema 9 initially made eight historical schema assertions/fixtures fail as expected. Updating downgrade fixtures to remove all schema-9 tables and adding direct schema-8-to-9 accounting preservation resolved them. Focused paper/storage/market-data run: **38 tests passed** before the final two paper cases were added.
- Final `.venv\Scripts\python.exe -m unittest discover -s tests -v`: **110 tests passed**. Final focused `.venv\Scripts\python.exe -m unittest tests.test_paper_broker -v`: **7 tests passed**. `.venv\Scripts\python.exe -m compileall -q src tests`: exit 0. `.venv\Scripts\python.exe -m polybot run --config config.example.toml --once`: exit 0 with paper STARTING/RUNNING/STOPPED and trading disabled. Workspace-cache `uv pip check --python .venv\Scripts\python.exe` checked **35 packages**, all compatible. `git diff --check`, repository trailing-whitespace scan, and a static paper-module authenticated-client/import scan passed. No production book, probability, order, fill, fee, latency or performance observation was generated.

## Task 12 implementation and public interfaces

- Read the current state and strategy, risk, market-data, paper-broker, model and storage contracts before changes. Added the implementation plan at `docs/superpowers/plans/2026-09-27-event-replay.md`; wrote Task 12 behavioral tests first. The first focused invocation failed with `ModuleNotFoundError: polybot.backtest`, establishing the intended red state.
- Added `backtest.py` with `ReplayRunner`, UTC `ReplayBoundaries`, homogeneous `DataProvenance`, `DataFidelity`, related-event `EvaluationPeriod` assignment, replay market/event types, `FrozenParameters`, `ReplayConfig`, timestamped `ReplayDecision`, and canonical `ReplayReport`. The runner is offline and paper-only.
- External books, forecasts, low-fidelity price observations and settlements become visible only at recorded `available_at`. Book/forecast receipt times must equal that availability and information times cannot be later. Market/rules metadata must be available by replay start. Internal clock events cover broker latency, intent expiry and cancellation delay without fabricated market ticks. `Ledger.book_at` independently prevents an arrival from seeing later depth.
- The runner directly reuses `eligible_forecast`, `evaluate_strategy`, `RiskPolicy`/`RiskGate`, `PaperBroker`, depth-aware quote/fee logic and `Ledger`; it copies no decision or fill economics. Outstanding reservations feed production target sizing, liquidation marks use executable bid depth, and early-exit mode uses the production SELL path when selected.
- `ReplayBoundaries` use half-open development/validation/holdout intervals. All markets in a related event group are assigned together using the group's latest scheduled end, preventing group leakage. Closed-later markets are never filtered and are enumerated in the report. Missing closed/delisted history remains a dataset-builder limitation.
- `FrozenParameters.create` hashes exact strategy/risk/paper settings and freeze time. The runner recomputes the hash and rejects parameters frozen after the holdout boundary. Exact configuration, all boundaries, market assignments/counts, caller-supplied immutable code version, canonical dataset/result hashes, provenance and data-quality limitations accompany every report.
- Synthetic and empirical events cannot mix. Candle/last-price-only data is retained as context but creates no executable book or passive fill; the report marks execution limited. Mixed book/low-fidelity runs say price-only inputs were excluded from execution. All repository Task 12 fixtures are synthetic and separate from any future empirical evaluation.
- Added `Settlement` and schema 10 `settlements`. Paper settlement requires exact owned shares, a simulated source, no active related orders/reservations, and `available_at >= resolved_at`; it atomically records and credits `shares * payout` only at availability. Schema-9-to-10 migration preserves prior cash/fill state.
- `ReplayRunner.run(report_path=None)` returns a report and optionally writes canonical JSON. The integration fixture exercises signal -> strategy -> risk -> reserved order -> latency price/depth change -> partial simulated fill/fee -> expiry/cancellation release -> delayed settlement -> ending portfolio.

### Task 12 commands and observed results

- Red test: `.venv\Scripts\python.exe -m unittest tests.test_backtest -v` failed because `polybot.backtest` did not exist.
- First implementation focused run had one expected fixture-design failure: the strategy correctly rejected a five-share intent when the decision-time book exposed only two shares. Splitting the decision book (100 shares) from the smaller arrival book (2 shares) produced the intended latency-driven partial fill without weakening production strategy checks.
- Final focused `.venv\Scripts\python.exe -m unittest tests.test_backtest -v`: **5 tests passed**. Coverage proves later forecast injection cannot change decisions through the earlier cutoff, related groups cannot cross cohorts, closed markets remain, late parameter freeze is rejected, low-fidelity execution is limited, independent runs hash identically, and delayed settlement proceeds are unavailable early.
- Focused `.venv\Scripts\python.exe -m unittest tests.test_storage tests.test_market_data tests.test_backtest -q`: **39 tests passed** after schema 10 migration updates. Final `.venv\Scripts\python.exe -m unittest discover -s tests -v`: **116 tests passed**. `.venv\Scripts\python.exe -m compileall -q src tests`: exit 0. `.venv\Scripts\python.exe -m polybot run --config config.example.toml --once`: exit 0 with paper STARTING/RUNNING/STOPPED and trading disabled. Workspace-cache `uv pip check --python .venv\Scripts\python.exe` checked **35 packages**, all compatible. `git diff --check`, repository trailing-whitespace scan and authenticated-client/import scan for `backtest.py` passed. No production dataset, forecast, order, fill, settlement, probability or performance result was generated.

## Task 13 implementation and public interfaces

- Read the repository and current state first, recorded the plan in `docs/superpowers/plans/2026-09-29-reporting.md`, then added `reporting.py`, `tests/test_reporting.py`, and `docs/reporting.md`. Existing SDK, lockfile, schema 10, execution, risk, and replay behavior remain unchanged.
- `build_report` is a deterministic read-only projection over ledger records. It reconstructs FIFO lots, capitalizes buy fees, subtracts sell fees, realizes partial exits and terminal settlements exactly once, and retains fill/settlement source IDs for every reported realized gain. Open lots retain their buy fill IDs.
- `ExecutableValuation` requires an exact-share, fee-inclusive executable liquidation value, recorded book ID and UTC time. Missing, uncertain, future, or quantity-mismatched values never become midpoint estimates: known equity remains available, while total equity, unrealized total and net performance become unknown and data gaps explain why.
- Reports include cash, reserved/available cash, open basis, realized and executable unrealized P/L, fees, separately confirmed rebates/rewards, known/total equity, funding-adjusted performance, gross exposure, turnover, fill rate, limit-based execution shortfall, risk rejections, persisted book invalidations, capital locked, drawdown checkpoints, and market/event breakdowns.
- Resolved-only forecast reporting includes Brier score, ten-bin calibration, constant-0.5 and recorded contemporaneous-market baselines, plus market/event score breakdowns. Event group is the independent unit. The approximate 95% interval uses event-group mean scores only when at least two groups exist; small/correlated samples carry an explicit caution.
- `Ledger.records` exposes typed immutable audit rows in insertion order. `recorded_data_gaps` exposes invalidation evidence. `record_incentive` records only positive paper rebates/rewards, separately from trading P/L; duplicate identical movement IDs are idempotent and conflicts fail. External deposit/withdrawal retries now have the same idempotent behavior.
- The accounting convention, metric definitions, public inputs, traceability and statistical limits are documented in `docs/reporting.md`. No current market data is fetched by reporting and no live endpoint was added.

### Task 13 commands and observed results

- Baseline from Task 12: **116 tests passed**. The new behavioral suite was written before implementation; `.venv\Scripts\python.exe -m unittest tests.test_reporting -v` failed with `ModuleNotFoundError: polybot.reporting`, establishing the expected red state.
- The first implemented focused run had two assertion-only failures: the hand calculation had incorrectly stated limit-price shortfall as -1.60 instead of -1.20 pUSD, and the caution rendered “1” instead of “one”. Correcting the fixture arithmetic and wording produced **5 passing reporting tests**. No production result changed.
- Final `.venv\Scripts\python.exe -m unittest discover -s tests -q`: **121 tests passed**. The reporting cases cover a hand-calculated fee-bearing ledger, partial FIFO exit, deposit and idempotent retry, separate rebate and retry, pending reservation, executable mark, missing/uncertain mark, settlement without double counting, rejected intent, market/event attribution, and resolved-only forecast scoring/baselines/calibration.
- `.venv\Scripts\python.exe -m compileall -q src tests`: exit 0. `.venv\Scripts\python.exe -m polybot run --config config.example.toml --once`: exit 0 with paper STARTING/RUNNING/STOPPED. Workspace-cache `uv pip check --python .venv\Scripts\python.exe` checked **35 packages**, all compatible. `git diff --check` and the repository Python/Markdown trailing-whitespace scan passed. No dependency or schema migration changed; no real forecast, quote, fill, P/L, statistical result, credential, exchange call, order, or money movement was created.

## Task 14 implementation and public interfaces

- Read the current CLI, configuration, discovery, market-data, strategy, risk, paper-broker, replay, reporting, and state contracts before changes. Added `docs/superpowers/plans/2026-09-29-paper-pilot.md`, then wrote the pilot behavior suite first. Its first run failed with `ModuleNotFoundError: polybot.pilot`, establishing the intended red state.
- Added `pilot.py` and strict `pilot.example.toml`. `load_pilot_config` rejects unknown/missing fields, parses financial values directly to Decimal, creates the existing typed StrategyPolicy/RiskPolicy/PaperBrokerPolicy/CollectorPolicy, hashes the exact file, and creates a second canonical FrozenParameters digest. The example is `paper-pilot-v1` / `manual-hold-v1`, frozen 2026-09-29T00:00:00Z, hold-to-resolution, and explicitly a tested research configuration rather than operator-selected live limits or an observed edge.
- `polybot pilot run` validates the latest persisted Task 5 reviewed metadata hash, event, outcomes, type and schedule before initializing paper cash. It records an immutable manifest with run/code/pilot/strategy/config/parameter/metadata versions, runs both public outcome collectors, reuses eligible_forecast/evaluate_strategy/RiskPolicy/PaperBroker, advances latency/cancellation, and makes no intent when no eligible manual forecast exists. It imports no authenticated order interface.
- The persistent run remains foreground. `polybot pilot stop` writes a durable stop request from a second process; the run appends a terminal transition and clears its active pointer. `pilot run --once` only captures one usable snapshot cycle and deliberately performs no strategy evaluation or submission. It is a connectivity check, not a pilot pass.
- Run artifacts live under the configured paper directory at `pilot/runs/<run-id>/{manifest.json,events.ndjson}` with `active.json` and `stop.request` control records. Manifests are immutable and transitions append-only. SQLite schema remains 10 and continues to own books, forecasts, decisions, orders, fills and accounting.
- `polybot pilot inspect` reports run/version identity, current/expired signal counts, risk rejection reasons, simulated/non-simulated fills, fill rate, recorded interruptions, queue-uncertain resting order IDs, cash/reservations/equity gaps, drawdown availability, event performance and explicit readiness gates. It always labels the evaluation pending; it cannot turn a short run into a passed pilot. New ledger reads expose latest reviewed metadata, initialization state, run-bounded invalidations, and resting paper orders without changing accounting transitions.
- Resting displayed-touch fills remain unfilled under Task 11 semantics. Inspection explicitly flags those orders as requiring unavailable queue/trade evidence for any hypothetical fill, and the preregistered criteria exclude them from performance.
- Added `docs/pilot-operations.md` with Windows commands and the pre/during/post operator checklist. Added `docs/pilot-evaluation-template.md` with criteria fixed before outcomes: chronology, 100% signal freshness, broker-gate coverage, retained data gaps, arrival-depth fill support, exact accounting, halt behavior, full holding horizons, at least 30 independent resolved events, 20 approved marketable paper orders and 10 supported simulated fills, favorable holdout Brier comparisons and uncertainty, positive after-cost event performance with justified uncertainty, and drawdown within the frozen limit.
- The current reviewed market's scheduled end is 2026-10-01T04:00:00Z. The operational observation floor is through 2026-10-02T04:00:00Z or actual later settlement availability. Statistical readiness requires at least 30 independent resolved event groups; the current single reviewed event can never satisfy it. Duration alone does not replace event independence.

### Task 14 commands and observed results

- Initial focused test: `.venv\Scripts\python.exe -m unittest tests.test_pilot -v` failed at import before implementation. The first implementation run exposed a test fixture missing the required negative-risk snapshot field and keyword-only quote constraints; after correcting those contract uses, **4 pilot tests passed**.
- Focused pilot/CLI run: **11 tests passed**. Final focused pilot suite: **5 tests passed**, including clean KeyboardInterrupt transition/active-pointer removal. Pilot/reporting run after run-bounded data-gap changes: **9 tests passed**. Coverage includes exact file/parameter hashes, versioned manifest, reviewed-metadata mismatch before cash creation, no-signal/no-trade, public snapshot persistence, durable stop, terminal-state behavior, pending inspection, and insufficient independent-event coverage.
- Actual read-only `.venv\Scripts\python.exe -m polybot pilot inspect --config config.example.toml --pilot-config pilot.example.toml`: exit 0 and reported no run, zero forecasts/decisions/fills, no interruptions, unknown drawdown from zero observations, minimum observation end 2026-10-02T04:00:00Z, zero of 30 independent events, and `evaluation_status: pending`. No pilot was started and no observation was fabricated.
- Rechecked official documentation on 2026-09-29 beginning at `https://docs.polymarket.com/llms.txt`; the current Real-Time Data API section retains the unauthenticated market WebSocket, `assets_ids`, `PING`/`PONG`, snake-case raw events, and `custom_feature_enabled` behavior used by the existing adapter. Offline installed-SDK inspection reported `polymarket-client 0.11.0`, `AsyncPublicClient.get_order_book(*, asset_id=None, token_id=None)`, and `MarketSpec(*, asset_ids=None, token_ids=None, custom_feature_enabled=False)`. No install or authenticated client was used.
- Final `.venv\Scripts\python.exe -m unittest discover -s tests -q`: **126 tests passed**. `.venv\Scripts\python.exe -m compileall -q src tests`: exit 0. Paper lifecycle smoke, pilot command help, `git diff --check`, and repository Python/Markdown trailing-whitespace checks passed. Workspace-cache `uv pip check --python .venv\Scripts\python.exe` checked **35 packages**, all compatible. No dependency or schema migration changed. No real forecast, strategy decision, fill, P/L, calibration result, authenticated call, credential, wallet action, live order, or money movement was created.

## Next task and limitations

Task 14 is complete; await the user's next numbered task. Pilot evaluation is explicitly pending. The operator has not supplied a validated forecasting model, empirical cohort, chosen paper/live bankroll and separate loss limit, or verified country/account eligibility. The frozen example values are paper research inputs, not production preferences. No persistent public-data pilot has been started, no actual forecast has been submitted, and zero independent events have been evaluated. The current single reviewed event cannot meet the 30-event statistical floor. The foreground runner has no OS service manager; an OS crash can leave an active pointer requiring preserved evidence and manual recovery. Settlement/redemption is not automated. Historical drawdown needs source-linked equity checkpoints, and open equity needs executable liquidation values. Resting order fills remain unavailable without queue/trade evidence. Statistical thresholds guide a future held-out evaluation but cannot guarantee profitability or authorize live trading. No live broker, authenticated endpoint, credential, wallet approval, order submission or money movement is enabled.

## Task 15 implementation and public interfaces

- Reopened the official documentation at `https://docs.polymarket.com/llms.txt` on 2026-09-29 and rechecked Python SDK, wallet/authentication, geoblock, pUSD and contract documentation. Inspected installed `polymarket-client==0.11.0` signatures and source for secure client construction, authenticated closed-only and balance reads, onchain approval reads, approval setup, transfers and redemption. Python remains 3.12.14 and the lockfile/dependencies did not change.
- Confirmed from installed source that `AsyncSecureClient.create(...)` validates or derives CLOB credentials and calls `_ensure_wallet_ready()`, which can deploy and await creation of the default Deposit Wallet. It is therefore excluded from preflight. Mutating SDK methods for orders, approvals, transfers, position operations and redemption remain unreachable.
- Added `preflight.py` with strict `PreflightConfig`, redacted typed report/check records, `PreflightProbe`, and `OfficialReadOnlyProbe`. The production probe allows only public geoblock GET, authenticated CLOB GET `/auth/ban-status/closed-only`, authenticated CLOB GET `/balance-allowance`, Polygon `eth_chainId`, and Polygon `eth_call`. It rejects every other CLOB path and RPC method and does not call `/balance-allowance/update`.
- Identity is derived locally from the protected signer environment value. The configured account wallet is classified against the pinned production EOA, Proxy, Safe and Deposit Wallet derivations and compared with the expected type. An unrelated signer/session-key relationship remains unresolved rather than being assumed. The report fingerprints addresses and never emits complete addresses or credentials. Current wallet terminology supersedes the older `funder`; no separate funder is inferred.
- Collateral is reported from integer pUSD base units using exact six-decimal `Decimal` conversion. Prediction-market approvals are read onchain for pUSD and Conditional Tokens at the standard and negative-risk exchanges. Missing approvals produce setup guidance only. The adapter cannot set them, deposit, transfer, deploy, redeem, submit/cancel orders, or create/derive credentials.
- Host geoblock, authenticated closed-only status, and explicit operator eligibility confirmation are separate checks. A successful IP check is always labeled insufficient to prove legal/account eligibility. `operator_country="UNRESOLVED"` remains in the example because “Dubai” did not confirm a country and timezone is not used. The actual unauthenticated host check returned `blocked: false` with country/region fields present; this is only a transport observation and no eligibility conclusion.
- Added secret-free `preflight.example.toml`, `polybot live-preflight --preflight-config ...`, `docs/live-preflight.md`, README instructions and the Task 15 API-contract supplement. Existing `run`, `status`, and `config-check` still reject live mode. Report exit codes: 0 only when all implemented checks pass; 1 when a redacted report is incomplete/not ready; 2 for config or missing protected environment values.

### Task 15 tests and observed results

- Wrote `tests/test_preflight.py` before implementation. Its first focused run failed with `ModuleNotFoundError: polybot.preflight`, establishing the expected red state.
- Final focused behavior coverage includes complete ready flow, IP/account/legal separation, blocked and closed-only states, zero collateral, missing approvals, missing/malformed secrets, error redaction, strict secret-free configuration, CLI safety, absence of execution methods, and rejection of `eth_sendRawTransaction` before network I/O.
- Final `.venv\Scripts\python.exe -m unittest discover -s tests -q`: **133 tests passed**. `.venv\Scripts\python.exe -m compileall -q src tests`: exit 0. Paper lifecycle smoke remained exit 0 with `STARTING/RUNNING/STOPPED` and `trading_enabled=false`. `live-preflight --help` exited 0. Workspace-cache `uv pip check --python .venv\Scripts\python.exe` checked **35 packages**, all compatible. `git diff --check` and changed-file trailing-whitespace checks passed.
- No protected environment values were supplied, no authenticated endpoint was called, and no secure SDK client, API credential, wallet, approval, deposit, transfer, redemption, order or cancellation was created. The only production observation was the public geoblock GET described above.

## Next task and limitations after Task 15

Task 15 is complete; stop here and await the next numbered task. The preflight has not been run against an account because no local secrets were supplied. Country remains unresolved, operator eligibility is unconfirmed, account/wallet type is unselected, live bankroll and separate loss limit are missing, and session-key authorization cannot be proven by local address derivation. CLOB balance/allowance cache freshness is not changed by preflight; exact approvals are read onchain. A positive IP result is not a legal conclusion. No live broker, order submission, cancellation, wallet mutation, money movement or live startup is enabled. Paper-pilot and strategy-evidence limitations from Task 14 remain unchanged.

## Task 16 implementation and public interfaces

- Rechecked current official order, cancellation, user-stream, maintenance, and
  SDK material beginning at `https://docs.polymarket.com/llms.txt`. The locked
  runtime remains Python 3.12.14 and `polymarket-client==0.11.0`. Verified sync
  `SecureClient` signing, POST, order/trade lookup, paginator, and cancellation
  signatures. No authenticated client was created.
- Added `brokers/live.py` with a `LiveExchange` boundary, disabled-by-default
  `LiveBrokerPolicy`, strict secret-free loader, risk-gated `LiveBroker`, and
  `PolymarketLiveExchange`. The adapter accepts an already authenticated client,
  uses `create_limit_order` plus `post_order`, and never invokes client creation,
  allowance recovery, approvals, wallet deployment, transfers, or redemption.
- Added schema 11 durable live submissions, append-only state events, market
  halts, and fill observations. Public storage operations persist prepared and
  submitting states before POST, acknowledge or reject an attempt, preserve
  unknown/cancel-pending reservations, record expiry/cancellation, recover after
  restart, and apply an external fill once across REST and stream observations.
- Live order states are prepared, submitting, acknowledged, partially filled,
  cancel pending, cancelled, filled, rejected, expired, and unknown. Raw
  exchange status is retained. There is one POST attempt. A lost response gets
  one lookup; unresolved or ambiguous acceptance keeps capital reserved and
  halts the market. No undocumented idempotency behavior is assumed.
- The live adapter supports only LIMIT GTC in this first version and revalidates
  token mapping, stored tick, share precision, minimum pUSD notional, strategy
  expiry, and the live risk gate. Only CONFIRMED exchange trade events affect
  accounting. Strategy evaluation can emit exchange-independent live intents
  for a live portfolio; this does not authorize transport, and every submission
  still passes the live risk gate and runtime switch.
- Added `live-execution.example.toml`, `docs/live-broker.md`, Task 16 API-contract
  verification, and fake-exchange behavioral tests. Existing CLI `run`, `status`,
  and `config-check` remain incapable of starting live mode.

### Task 16 tests and observed results

- Wrote `tests/test_live_broker.py` before implementation. Its first run failed
  with `ModuleNotFoundError: polybot.brokers.live`, establishing the expected red
  state. Focused live/risk/paper/storage verification passed 50 tests before
  final constraint/full-fill coverage; the live-broker module has 12 cases.
- Fake-exchange cases cover disabled activation, durable state before POST,
  accepted order with lost response, unresolved timeout and halt, duplicate
  stream/REST fill, partial fill before cancellation, cancel rejection, rate
  limit, maintenance rejection, expiry, and restart during submission. Automated
  tests use no SDK client, network, credential, wallet, or real order. The final
  cases also verify full-fill reservation release and rechecking the newest
  stored market constraints before transport.
- Final `.venv\Scripts\python.exe -m unittest discover -s tests -q`: **146
  tests passed**. `.venv\Scripts\python.exe -m compileall -q src tests`: exit 0.
  The paper lifecycle smoke exited 0 with `STARTING`, `RUNNING`, and `STOPPED`,
  `mode=paper`, and `trading_enabled=false`. Runtime signature inspection printed
  the verified official SDK methods. `uv pip check` initially could not open the
  user cache under sandbox permissions; rerunning with the ignored workspace
  `.uv-cache` checked 35 packages and found all compatible. `git diff --check`
  and the source/docs/config trailing-whitespace scan passed. Schema is now 11;
  migration tests cover upgrade from schema 10 without accounting loss.

## Next task and limitations after Task 16

Task 16 is complete; stop here and await the next numbered task. Live startup is
still blocked. The broker is a tested library and is not connected to a live CLI
or long-running authenticated user stream. Account preflight has not been run,
operator country/legal/account eligibility is unresolved, no live bankroll or
separate live loss limits are selected, and no explicit real-order authorization
was given. The documented order heartbeat lifecycle is not implemented. An
immediate full fill followed by a lost POST response may not appear in open-order
lookup and cannot be uniquely bound from documented trade fields; the order
therefore remains unknown and halted for manual authoritative reconciliation.
No mechanism to clear a live market halt is exposed. Paper-pilot and
strategy-evidence limits remain unchanged, and no profitability evidence exists.

## Task 17 implementation and public interfaces

- Re-read the repository and this state file before changes. Reverification on
  2026-09-29 began at `https://docs.polymarket.com/llms.txt`; reviewed the current
  manage-orders, real-time order updates, matching-engine maintenance,
  resolution, and position-management documentation. Inspected the locked
  `polymarket-client==0.11.0` synchronous signatures and resolution models under
  Python 3.12.14. No authenticated client was created and no endpoint was called.
- Added `reconcile.py` with `AccountSource`, `PolymarketAccountSource`, typed
  authoritative orders/positions/account reads, `ReconciliationPolicy`, reports,
  and `ReconciliationService.run`, `run_periodic_if_due`, and `recover`. Startup
  and every actual periodic pass close the durable gate first. Reads consume all
  pages and overlap recent fills; confirmed fills apply once. Reconnect and
  maintenance recovery rebuild market state before account reconciliation.
- Unknown manual/external orders or fills, unidentified local submissions,
  balance/position differences, propagation lag, read/rebuild failure, and
  unhealthy heartbeat halt the whole bot. Known missing orders, terminal-state
  disagreements, and unconfirmed cancellation halt the affected market. A
  cancellation request never releases reservations by itself. Market halts clear
  only when the authoritative order evidence resolves the affected identity.
- Added `heartbeat.py` with a five-second credential-scoped heartbeat, ten-second
  stale detection, persisted rotating IDs, expected-ID retry protocol, and health
  that is independent from process liveness. `LiveBroker` now requires completed
  reconciliation, rebuilt market data, and an active healthy heartbeat before
  the risk gate can reserve or the adapter can prepare an order.
- Added `settlement.py` with an official public resolution reader requiring a
  reviewed ordered outcome mapping, pending/disputed/resolved state, redeemable
  action reporting, separately authorized redemption dispatch, and confirmation-
  only proceeds. Scheduled expiry never implies resolution. Live redemption
  credits exact proceeds once only after transaction identity, empty authoritative
  positions, and authoritative collateral agree. The paper fixture settles and
  reconciles end to end without touching the live store.
- Schema 12 adds durable reconciliation runtime/issues, authoritative position
  observations, heartbeat state, resolution observations, and redemption
  requests. Existing accounting survives a direct schema-11-to-12 migration.
  `HeartbeatState` is the new shared model. Complete behavior and halt scope are
  in `docs/reconciliation.md`; `docs/api-contract.md`, `docs/live-broker.md`, and
  `README.md` were updated.

### Task 17 tests and observed results

- Wrote `tests/test_reconcile.py`, `tests/test_heartbeat.py`, and
  `tests/test_settlement.py` before implementation; their first run failed with
  missing Task 17 modules. Behavioral coverage includes multi-page restart,
  overlapping duplicate fills, unknown external exposure, global/market halt
  scope, due-only periodic runs, reconnect rebuild order, cancellation evidence,
  heartbeat rotation/failure/staleness, explicit payout ordering, dispute state,
  disabled redemption, confirmation-only proceeds, and exactly-once paper/live
  settlement. All sources and transactions in tests are synthetic.
- Focused Task 17/live/storage verification: **51 tests passed**. Final
  `uv run --locked python -m unittest discover -s tests -v`: **162 tests passed**
  in 27.936 seconds. `uv run --locked python -m compileall -q src tests`: exit 0.
  `uv run --locked polybot run --config config.example.toml --once`: exit 0 with
  paper `STARTING`, `RUNNING`, and `STOPPED`, and trading disabled. Workspace-cache
  `uv pip check --python .venv\Scripts\python.exe` checked **35 packages** and
  reported all compatible. `git diff --check` and repository Python/Markdown/TOML
  trailing-whitespace checks passed.

## Next task and limitations after Task 17

Task 17 is complete; stop here and await the next numbered task. Live startup
remains disabled and no live authorization was provided. A future live process
must wire the implemented services into its scheduler and supply reviewed token,
fee, and payout mappings; it must not bypass the durable gate. Authenticated
account reads are not atomic, so configured propagation grace still halts risk.
SDK 0.11.0 exposes no public heartbeat method, and its rejected-request exception
does not retain the documented HTTP 400 expected-ID body; the pinned transport
therefore fails closed on that production correction path. No authenticated
reconciliation, heartbeat, resolution, or redemption call was tested. Account
eligibility, live bankroll, separate live loss limits, and explicit real-order or
redemption authorization remain unresolved. Paper-pilot evaluation and strategy
profitability remain pending; no result in this task is profitability evidence.

## Task 18 implementation and public interfaces

- Added `operator.py` with `OperatorContext`, `OperatorStatus`,
  `CancellationBatchResult`, `LocalPaperCancellationBroker`, and
  `OperatorService.status`, `pause_new_orders`, `resume_after_checks`,
  `cancel_open_orders`, and `emergency_halt`. Controls are local and use the
  selected isolated paper/live database. No dashboard or external notification
  integration was added.
- Schema 13 adds singleton `operator_control` and append-only `operator_actions`
  and `operator_alerts`. Every mutation records a stable action ID, UTC time,
  action, and explicit reason. Paused and emergency states survive restart and
  stay separate across paper/live stores. Schema-12-to-13 migration preserves
  accounting and initializes the control to `running`.
- `pause-new-orders` blocks all new BUY and SELL orders without changing working
  orders, reservations, cash, or positions. `emergency-halt` does the same and
  creates a critical local alert. Neither command cancels orders or sells an
  existing position; emergency market liquidation is not implemented.
- `cancel-open-orders` is a separate broker operation. Paper CLI cancellation
  uses the configured simulation delay or confirms a never-scheduled local order.
  Live library cancellation uses the existing broker contract and retains
  reservations on uncertainty. The standalone CLI refuses live cancellation
  because live startup/authenticated controller wiring remains disabled.
- `resume-after-checks` requires a fresh valid book for every configured outcome,
  current unexpired forecast per market, consistent local accounting, no unknown
  orders, and no persistent risk halt. Live mode additionally requires fresh
  successful reconciliation and an active healthy heartbeat. Failure retains the
  halt and records exact check failures plus a warning alert.
- Submission control is enforced three times: atomically inside risk reservation,
  after reservation in the non-overridable broker template, and immediately
  before the live exchange POST. A halt injected during signing/preparation
  rejects the provably unsent order, releases its reservation, records the block,
  and makes zero exchange POST calls.
- Local alerts cover emergency halt, failed resume, failed/uncertain cancellation,
  pre-POST blocks, market-data invalidation, heartbeat failure, reconciliation
  issues, and daily-loss/drawdown halts. Alerts stay in SQLite and redacted local
  logs; `external_notifications_enabled` is always false.
- `polybot status` now reports run mode, frozen strategy version, worst and
  per-source feed/forecast ages, total/free/reserved pUSD cash, positions and
  reserved shares, additive maximum-payout exposure, realized and executable-
  liquidation P/L, explicit uncertain valuation, open/unknown orders, heartbeat,
  reconciliation, operator control, resume-check failures, and local alerts.
  Status performs no exchange or wallet call.
- Added top-level `status`, `pause-new-orders`, `resume-after-checks`,
  `cancel-open-orders`, and `emergency-halt` CLI commands with explicit `--reason`
  on mutations and optional `--mode paper|live`. Complete semantics and commands
  are documented in `docs/operator-controls.md`; README and live-broker docs were
  updated.

### Task 18 tests and observed results

- Wrote `tests/test_operator.py` and the live pre-POST test before implementation;
  the first focused run failed with `ModuleNotFoundError: polybot.operator`.
  Tests cover restart persistence, mode isolation through distinct ledgers,
  pause/emergency non-cancellation and non-liquidation, atomic risk rejection,
  cancellation uncertainty and reservations, fresh/stale resume checks, live
  reconciliation/heartbeat requirements, full status fields, CLI persistence,
  and a halt arriving during live preparation before POST.
- Focused operator/live/storage/CLI verification passed **51 tests**. A broader
  operator/live/heartbeat/reconciliation/risk/market-data set passed **49 tests**.
  Final `uv run --locked python -m unittest discover -s tests -q`: **169 tests
  passed** in 42.536 seconds. `uv run --locked python -m compileall -q src tests`:
  exit 0. `uv run --locked polybot run --config config.example.toml --once`:
  exit 0 with paper `STARTING`, `RUNNING`, and `STOPPED`, trading disabled.
- `uv pip check --python .venv\Scripts\python.exe` checked **35 packages** and
  reported all compatible. The real local `polybot status` command exited 0 and
  displayed paper mode, `manual-hold-v1`, zero cash/exposure in the uninitialized
  example store, missing feed/forecast checks, no heartbeat, local reconciliation,
  and external notifications disabled. It made no network or account call.
  `git diff --check` and the explicit Python/Markdown/TOML trailing-whitespace
  scan passed.

## Next task and limitations after Task 18

Task 18 is complete; stop here and await the next numbered task. Live startup and
live CLI cancellation remain disabled. Live cancellation requires the future
running authenticated controller and authoritative reconciliation; a request is
never confirmation. Alerts are local only because no notification destination or
authorization was supplied. Status cannot calculate unrealized P/L without an
executable exact-quantity liquidation quote and reports that value as unknown.
Existing Task 17 SDK heartbeat limitations, unresolved operator eligibility/live
financial choices, paper-pilot evidence limits, and absence of profitability
evidence remain unchanged.
