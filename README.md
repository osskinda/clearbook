# Clearbook source release

The complete MIT-licensed source, tests, dependency lockfile, documentation, and demonstration assets are in [clearbook-source.zip](./clearbook-source.zip). GitHub connector write access to newly created repositories was unavailable, so this release uses a full source archive uploaded through GitHub.

Download and extract the archive, then run:

```sh
npm ci
npm run dev -- --hostname 127.0.0.1
```

The working product is at https://clearbook-monad.nutmegmoose8.chatgpt.site. Video pages: `/demo` and `/pitch`.

---

# Clearbook

**Explain every Perpl account balance change on Monad.** Exact account accounting, original-record inspection, and explicit discrepancy detection. A Monad Metropolis solo entry by **kinda**, built for Onchain Finance / Perpl analytics.

Product: https://clearbook-monad.nutmegmoose8.chatgpt.site

## Judge walkthrough

1. Open the product; the default account is explicitly **synthetic example data**.
2. Choose **Missing record**. Removing one funding event produces an 8 AUSD discrepancy.
3. Open the finding and inspect expected balance, reported balance and original record.
4. Choose **Sources → Check live Monad evidence**. This independently reads real chain ID, deployed exchange bytecode and a recent exchange receipt when one exists in the checked block.
5. Import original JSON/CSV history or connect a **read-only** Perpl Ed25519 API key. Export the report.

No wallet, funds or key is required for the demonstration or public source checks.

## Capabilities

- Exact BigInt financial arithmetic in raw collateral units.
- Fee-inclusive account changes with protocol/builder costs decomposed once.
- Per-instance/account boundaries, block/transaction/log order, identical overlap removal and conflicting-duplicate detection.
- Sequence gaps, unknown event types, missing coordinates, incomplete pagination and snapshot differences remain visible.
- Browser-local JSON / raw-unit CSV import, original record inspection and JSON report export.
- Browser Ed25519 signatures, cursor pagination and a restricted read-only Perpl proxy.
- Monad mainnet/testnet source checks, receipt verification and explorer links.
- Responsive desktop/mobile workspace.

## Accounting limits

This is **account-balance reconciliation**, not total portfolio PnL, a trading recommendation or tax reporting. Perpl `a` already includes `f`; `bfa` is part of `f`. Display decomposition: `(a + f) − (f − bfa) − bfa = a`. Cash flows, funding, execution, collateral movements, protocol transfers and unknown events remain separate. Locked balances and unrealized positions are not added.

Opening balance is inferred from the earliest supplied record (`b − a`). A matching window cannot prove complete earlier history. A later snapshot can contain newer activity; the difference is flagged rather than silently corrected. Receipts prove transaction inclusion and exchange log presence, not every API amount.

Demo financial records are synthetic and contain no fabricated transaction hashes. Public source checks use real RPC reads. Signature and pagination tests use mocked responses. Live public Perpl context and Monad RPC have been exercised; authenticated pagination against a populated trader account has not been verified end to end. No real customer adoption or revenue is claimed.

## Privacy and security boundaries

Imported files and Ed25519 secrets stay in browser memory. Secret bytes are cleared after loading. Signed headers and account responses pass through the server to Perpl because the upstream does not support the required browser CORS preflight. No application database stores them; infrastructure may retain ordinary access logs. Refresh clears account history; exported reports contain financial records.

Only `/v1/pub/context`, `/v1/trading/account-history` and `/v1/trading/wallet` GET requests are allowed. Cookies, arbitrary destinations and redirects are not forwarded. The RPC proxy allows only chain ID, head block, configured exchange bytecode, bounded exchange logs and receipts. It cannot submit transactions. Never enter a wallet private key or seed phrase.

## Run and validate

Node 22.13+ (24 recommended):

```sh
npm install
npm run dev
npm run build
node --experimental-strip-types --test tests/ledger.test.ts tests/integration.test.ts
npx tsc --noEmit
npx eslint app/page.tsx app/api lib/ledger.ts lib/perpl.ts lib/demo.ts
```

Portable preview: http://127.0.0.1:5173. Build output is Cloudflare Workers-compatible Vinext. `.openai/hosting.json` identifies the deployment. On Windows with a broken npm shim, call the installed `npm-cli.js` with Node directly.

## Import contract

JSON: Clearbook Dataset, report envelope containing `data`, or raw Perpl event array. Raw arrays use selected network/decimal/symbol settings and are marked incomplete. Example:

```json
{"version":1,"name":"My history","network":"mainnet","source":"import","complete":false,"decimals":6,"symbol":"AUSD","events":[{"in":1,"id":42,"et":1,"a":"1000000","b":"1000000","f":"0","at":{"b":100,"t":1791532800000,"tx":0,"l":0}}]}
```

`complete` is declared, not independently proven. Optional snapshot: `{sn: block, as: [{in,id,b}]}`. Amount fields are atomic-unit integer strings. Optional `at.txid` must be a real 32-byte hash. Synthetic exports retain their label when reimported.

CSV required columns: `instance_id,account_id,event_type,amount,balance,fee`. Optional: `timestamp,block,tx_index,log_index,tx_hash,builder_fee,locked_balance,market_id`. UTC timestamps are Unix milliseconds. For 6 decimals, 1 AUSD = `1000000`. Arbitrary exchange CSV formats are not guessed; multiline cells are unsupported. Limits: 3 MB / 10,000 events.

## Architecture and integration

`lib/ledger.ts`: pure accounting/import engine. `lib/perpl.ts`: local signatures and pagination. `app/api/perpl/route.ts`: fixed-host read proxy. `app/api/rpc/route.ts`: restricted Monad reads. `app/page.tsx`: account workspace.

Perpl mainnet: chain **143**, exchange `0x34B6552d57a35a1D042CcAe1951BD1C370112a6F`.
Perpl testnet: chain **10143**, exchange `0x1964c32f0be608e7d29302aff5e61268e72080cc`.
These are existing Perpl contracts integrated by Clearbook, not contracts deployed by this project. Actual source-check evidence is preserved in `docs/live-evidence.json`.

## Demand and competitors

Public trader reports motivated the hypothesis:
- https://www.reddit.com/r/algotrading/comments/1snj3cg/running_live_on_hyperliquid_for_2_months_stuck_on/
- https://www.reddit.com/r/hyperliquid1/comments/1riw84d/how_do_you_handle_taxes_on_your_hyperliquid_trades/

These are user claims about Hyperliquid, not independently audited errors. Applying this need to Perpl remains a hypothesis. Perpl already has native charts; DEX PULSE advertises Perpl aggregation. Clearbook focuses on explaining original-record discrepancies. Next validation: compare anonymized real Perpl exports with existing tools and require a reproducible unexplained case before expanding. No outreach is claimed.

## Build window and attribution

Original product code authored **October 9–10, 2026 Sydney time** for Metropolis. No prior Clearbook product reused. Starting scaffold: OpenAI Sites Vinext starter; vendor licenses retained. Kinda is the solo entrant. Codex generated code, tests, documentation and demo materials under the entrant's direction. Video voice is synthesized Microsoft Windows text-to-speech, not a recording of the entrant. No invented team member, user, transaction or traction.

Protocol definitions/signing: https://github.com/PerplFoundation/api-docs (`types.md`, `authentication.md`, `rest-endpoints.md`). Dependencies include React, Next, Vinext, Cloudflare tools, Noble Ed25519 and Lucide; their respective licenses apply. Original Clearbook code is MIT.
