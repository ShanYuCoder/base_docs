# Base Docs (R2)

Docs hub — **Overview** (*why*) · **Architecture** (*what*) · **Product Code** (*detail*).

**Team mới:** [Start now](/platform/guide/start-now) · [Doc structure](/platform/guide/SYSTEM-DOC-STRUCTURE)

| Area | Path | Format |
|------|------|--------|
| Handbook | `platform/` | MD |
| Architecture (Active) | `architecture/03-business-process/` · `architecture/07-deployment/` | MD + Mermaid |
| Architecture (other) | `architecture/<nn>-…/` | Stub OK when needed |
| Modules | `product/surfaces/<owner-surface>/modules/CMP-*` | Owner surface SSOT |
| Functions | `…/modules/CMP-*/<slug>/code/{W-*,API-*}` | No folder named `functions/` |
| Common | `product/surfaces/common/code/{UI-CMN-*,API-CMN-*}` · scoped `…/common/` | Dynamic by scope |
| Structure | [`SYSTEM-DOC-STRUCTURE`](/platform/guide/SYSTEM-DOC-STRUCTURE) | Tree + standards |

## Pilot

- Overview: [`product/overview/`](/product/overview/) after `docskit init`
- Module: [CMP-01](/product/surfaces/admin-web/modules/CMP-01-auth/)
- Function: [login](/product/surfaces/admin-web/modules/CMP-01-auth/login/)
- business-process overview: [`FLOW-login`](/architecture/03-business-process/FLOW-login)
- ADR: [ADR-001](/architecture/09-decisions/ADR-001-arc42-toc)

```bash
docskit init
pnpm install
pnpm docs:dev
```

After the docs install profile generates the local harness, use
**`/business-process`** for product `FLOW-*`. Brownfield: Platform DNA **`/legacy`**
+ Docskit/Processkit skills (không dùng `/legacy-spec`).
