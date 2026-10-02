# Changelog

This changelog records verifiable changes represented in the repository history.

## [Unreleased]

### Fork（个人二开）

- 本仓库是 [LeemanCheung/dsh-token-usage](https://github.com/LeemanCheung/dsh-token-usage)（MIT）的**个人 fork**，不是上游仓库，也未获得原作者背书；上游问题请提到上游，本二开改动请提到本仓库。
- npm 包名改为 `@hello_wk/dsh-token-usage`，并将 `cordis.patch.yml` 的模块 `name` 同步为该名（DSH 按模块标识符从 profile 的 `node_modules` 解析，必须与 `package.json` 的 `name` 完全一致）；以 `publishConfig.access = "public"` 公开发布，因此移除了 `private: true`。
- 补齐 npm 元数据：`license`、`author` / `contributors`（保留原作者署名）、指向本 fork 的 `repository` / `bugs` / `homepage`，并把 `CHANGELOG.md` 与 `LICENSE` 纳入发布 tarball。
- 内部标识**有意保持不变**：`tsdown.config.ts` 的 `PLUGIN_ID`、插件实例 `id`、RPC/schema 字符串（如 `dsh-token-usage/workbench-v2`）、`localStorage` 键与 DOM 事件名仍为 `dsh-token-usage`。它们不是 npm 包名，改动会破坏已持久化数据与 Host↔Client 协议；因此 `lib/` 产物无需重建，CSS Module 哈希也不变。

- 放宽 peer 范围以支持运行中的 DSH `0.2.0-rc.2`：原 `^0.1.2-rc.1` 被 semver 展开为 `>=0.1.2-rc.1 <0.2.0-0`，其上界 `-0` 恰好排除 `0.2.0-rc.2`（因为 `0.2.0-rc.2 > 0.2.0-0`），使 `@deepseek-ai/dsh-app-boot` 的 `evaluatePluginCompatibility()` 以 `incompatible-version` 拒绝加载全部 21 个 dsh peer。现改为 `^0.1.2-rc.1 || ^0.2.0-rc.2`，并把 `dsh.compatibility.dsh` 同步为 `>=0.1.2-rc.1 <0.3.0`（该字段仅作说明，DSH 不读取，真正卡加载的是 `peerDependencies`）。
- 0.2.0-rc.2 仅完成**静态表面核验**：8 个 `dsh.client.inject` 服务与 4 个运行时 value import 符号（`SessionId`、`isReplacementSurfaceEvent`、`BlockAssembler`、`createUserMessage`）在 0.2.0-rc.2 中均存在且被导出。**未**做端到端实测，因此 `dshReleases["0.2.0-rc.2"]` 如实标为 `surface-checked` 而非 `compatible`；详见 [docs/compatibility-0.2.0-rc.2.md](docs/compatibility-0.2.0-rc.2.md)。

### Upstream（沿用上游未发布改动）

- Keep client bundles and embedded source maps identical when the pinned Harness lives in a sibling checkout or an externally linked CI directory. Use Rolldown's whitespace-only AST printer with legal comments retained; normalize only dependency source-map labels, without rewriting generated JavaScript text.
- Reattribute identical final usage to its authoritative UTC day and replay older projection checkpoints without changing total usage or request counts.
- Keep unattributed fallback identities out of exact-route trend and budget controls; retain existing fallback budgets as removable, unavailable entries.
- Persist interrupted-analysis recovery before reporting readable state, preserving the first recovered end time across restarts.
- Limit clear/eviction coverage gaps to affected reporting windows, retaining a conservative startup boundary for legacy evictions with unknown dates.
- Persist expired request-reservation cleanup on startup, reads and configuration changes, including request-key copies in retained ledger records.

## [0.5.0] - 2026-09-12

### Added

- Completed scoped/actionable diagnostics, proportional Token node views and explicit offline receipt storage.
- Added common-window observable totals and durable request-ID reservations with bounded retention and restart protection.
- Added immutable price-book revisions, previewed rollback, import confirmation and stable pricing fingerprints.
- Added experiment ratio denominators and condition tables, post-run acceptance labels and duplicate-snapshot safeguards.
- Added cross-tariff interval scenarios, ordered reference-cost effects and quality-constrained optimization weekly cards.
- Added a separate numeric summary v2 event carrying budget states and confirmed-output samples while retaining the v1 contract.
- Extracted pure usage selectors and added an explicitly advisory upstream compatibility workflow.

### Boundaries

- No real provider calls or production bill verification are claimed. Invalid/unsupported tariffs remain unavailable.
- v1 persisted workbench state is validated and migrated; old experiments keep missing metrics unavailable.
- A bounded idempotency record is not an unlimited exactly-once guarantee; explicit new analyses get new IDs.

## [0.4.0] - 2026-09-12

### Added

- Added the bilingual local Usage workbench settings page without replacing the original Token usage dashboard or session projection.
- Added no-model session diagnostics, immutable paginated usage receipts, and JSON/Markdown exports with identifier hiding enabled by default.
- Added versioned exact-route price cards, separate USD/CNY estimates, validity intervals, provider-timezone tariffs, context tiers, and explicit unknown/partial/range pricing states. Built-in route templates contain no unverified numeric prices.
- Added a separate bounded, persistent auxiliary analysis ledger for usage and trajectory analysis, including cumulative multi-call accounting, missing usage, cancellation, failures, restart recovery, export and confirmed clearing.
- Added complete-day 7/30/90-day arithmetic change attribution, primary projects and tags, and rolling 30-day Token and USD/CNY money budgets. Budgets never block or reroute tasks.
- Added immutable optimization experiments with paired task labels and explicit human acceptance, cache/tariff scenarios, numeric-only weekly JSON/SVG exports, and default-off same-page summary integration.
- Added Chromium acceptance covering the actual React workbench and Host RPC with disk-backed settings, synthetic DSH events and a synthetic model transport. Coverage includes save/reload, exports, privacy boundaries, conflict protection, keyboard controls and a 390-pixel Chinese interface.

### Fixed

- Matched the pinned DSH event union and observable snapshot APIs, and corrected React StrictMode cancellation and exact optional property handling.
- Prevented cumulative usage updates from double-counting and separate model calls from overwriting one another in the auxiliary ledger.
- Bound form controls to explicit accessible labels and rechecked sharing consent through the Host before publishing numeric summaries.
- Reconciled README descriptions of local storage, price cards, money budgets and auxiliary analysis accounting.

### Validation and boundaries

- Retained the fixed DSH `a66e4702047846cdaa10c66c9d3df3951f5ea70d` toolchain, Host/Client/config type checks, the full regression suite, repeated-build SHA-256 verification, package allowlist, and committed-bundle equality gate.
- Added 47 workbench regression tests; the complete suite contains 184 tests across 26 files. The browser script covers 10 workflow cases.
- Browser acceptance uses synthetic provider events and does not claim verification of a production DSH installation or a real provider invoice. Prices remain estimates; currencies are not added together.
- Marked generated bundles as generated files. Only the emitted client JavaScript is exempt from end-of-line whitespace lint, avoiding blind rewriting of dependency literals; authored source, source maps and build reproducibility checks remain enforced.

## [0.3.1] - 2026-09-02

### Added

- Added a security policy, contribution guide, structured issue forms, and a pull request checklist (`bb233a2`).
- Added fact-based release notes and README navigation for the new maintenance resources.

### Fixed

- Stabilized committed CSS bundles across Windows and Linux by replacing platform-sensitive path hashes with a plugin-scoped namespace (`4943e02`).
- Added exact class-name regression coverage and normalized CSS build input while retaining the repeated-build hash gate.

## [0.3.0] - 2026-08-31

### Added

- Added confirmed-output throughput indicators for the current session and all sessions (`c59f948`).
- Sampled every five seconds with a rolling window of up to ten seconds, while explicitly distinguishing request-completion usage pulses from per-token decoding speed.

## [0.2.0] - 2026-08-28

### Added

- Added date-by-provider/model projections and exact rolling Token budgets (`28f5ffc`).
- Added coverage and conservation gates for model-level trends, budgets, and exports.

### Security

- Hardened private RPC input validation, model-catalog recovery, analysis termination, cancellation, and CSV formula neutralization.

## [0.1.0] - 2026-08-14

### Added

- Added the initial persistent Token usage dashboard for DeepSeek Harness (`0ea9f74`).
- Added Host-side usage projection, Web client surfaces, GitHub-installable bundles, installation metadata, and regression tests.
