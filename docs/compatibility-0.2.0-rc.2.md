# DSH 0.2.0-rc.2 compatibility

Status: **surface-checked, not end-to-end verified.**

This record exists because the desktop DeepSeek Harness build bundles DSH `0.2.0-rc.2`, and the
published plugin had to be allowed to load there. It deliberately does **not** claim `compatible`
for this release — see [Compatibility boundary](#compatibility-boundary).

## Why a change was needed

`dsh.compatibility` is documentation only: DSH never reads it (a full scan of the 0.2.0-rc.2 bundle
finds no reference to `dshReleases`). What actually gates loading is
`evaluatePluginCompatibility()` in `@deepseek-ai/dsh-app-boot`, which checks every
`@deepseek-ai/dsh` / `@deepseek-ai/dsh-*` entry in `peerDependencies` with
`semver.satisfies(runtimeVersion, range, { includePrerelease: true })` and rejects the plugin with
`incompatible-version` when a peer does not match.

The previous peer range was `^0.1.2-rc.1`, which semver expands to `>=0.1.2-rc.1 <0.2.0-0`. The
`-0` upper bound is what excludes `0.2.0-rc.2`, because `0.2.0-rc.2 > 0.2.0-0`. All 21 dsh peers
therefore failed on 0.2.0-rc.2.

The peers are now `^0.1.2-rc.1 || ^0.2.0-rc.2`, which accepts `0.1.2-rc.1` and `0.2.0-rc.2` while
still rejecting `0.1.0-rc.6` and `0.2.0-rc.1`.

## Verified (static surface only)

Checked against the `0.2.0-rc.2` bundle inside the desktop app's `app.asar`:

- All eight client services named in `dsh.client.inject` exist at `0.2.0-rc.2`:
  `dsh-api-session-controller`, `dsh-client-connection`, `dsh-client-ui-conversation`,
  `dsh-client-locale`, `dsh-client-ui-primitives`, `dsh-client-ui-renderer`,
  `dsh-client-ui-settings`, `dsh-client-ui-sidebar`.
- Every value (non-type) symbol the built bundles import at runtime is still exported:

  | Symbol | Package | 0.2.0-rc.2 |
  | --- | --- | --- |
  | `SessionId` | `@deepseek-ai/dsh-session` | exported |
  | `isReplacementSurfaceEvent` | `@deepseek-ai/dsh-session` | exported |
  | `BlockAssembler` | `@deepseek-ai/dsh-llm` | exported |
  | `createUserMessage` | `@deepseek-ai/dsh-llm` | exported |

  Everything else the source imports from `@deepseek-ai/dsh-*` is `import type` and disappears at
  build time, so it cannot fail at runtime.
- `lib/index.js` imports only `@deepseek-ai/schemastery`, `@deepseek-ai/dsh-session`,
  `@deepseek-ai/dsh-llm` and `zod`; `lib/client.js` additionally requires
  `@deepseek-ai/dsh-client-ui-primitives`.

A matching exported symbol means the import resolves. It does not prove the behaviour, the option
shapes, or the persisted-data formats are unchanged.

## Compatibility boundary

Not yet done on 0.2.0-rc.2, and the reason this record is not called `compatible`:

- No real Web profile install/activation run on 0.2.0-rc.2.
- No check that existing session history and its Token projections stay readable after that upgrade.
- No confirmation that the dashboard, the workbench, or either Token-rate indicator renders.
- No type check, unit-test run, or rebuild against the 0.2.0-rc.2 source contract. This checkout has
  no `node_modules`; the project takes TypeScript, Vitest and tsdown from a DSH workspace.

To promote this record to `compatible`, install into a Web profile running 0.2.0-rc.2, confirm the
checks above, then set `dshReleases["0.2.0-rc.2"]` to `compatible` in `package.json` and replace this
section with the observed results.

If DSH rejects the plugin on a release outside the declared range, the documented escape hatch is an
exact-version exemption via `dsh plugin allow-version` or the plugin manager — not an edit to this
file.
