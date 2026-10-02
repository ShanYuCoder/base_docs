# base-docs

Platform **docs hub** (R2) — Overview, Surfaces, Modules, Functions, Architecture và product Code/common.

**Start now:** [`platform/guide/start-now.md`](./platform/guide/start-now.md) · Structure: [`SYSTEM-DOC-STRUCTURE.md`](./platform/guide/SYSTEM-DOC-STRUCTURE.md)

```bash
docskit init
pnpm install
pnpm docs:dev
```

`docskit init` generates the local harness, product/architecture skeleton, VitePress setup, and docs scripts used below.

| Path | Role |
|------|------|
| `platform/` | Handbook |
| `architecture/03-business-process/` | **Active** — product `FLOW-*` overview |
| `architecture/07-deployment/` | **Active** — `DEP-*` (stub-first) |
| `architecture/<other>/` | Stub OK when a real concern requires it |
| `product/overview/` | Overview + operational areas |
| `product/surfaces/<owner-surface>/modules/CMP-*/` | Modules |
| `…/modules/CMP-*/<slug>/code/{W-*,API-*}/` | Functions (no `functions/` folder) |
| `product/surfaces/common/code/{UI-CMN-*,API-CMN-*}/` | System Common IDs |
| `registries/docs-index.json` | Docs-side index (not FE codegen SSOT) |

Pilot: **CMP-01 Auth** · `W-AD-AUTH-001` · `API-AD-AUTH-001` · `FLOW-login`.

Local MCP harness and project maps are generated when you install the docs
toolkits — see [Toolkits (MCP)](./platform/guide/toolkits.md).
