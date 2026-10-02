# Feature artifact — lệnh script

> **Docskit** sau `docskit init`: CLI `docskit split|merge|render|publish|openapi:gen|api:check`. `pnpm spec:*` / `docs:*` trên hub là alias. [Toolkits (MCP)](/platform/guide/toolkits).

> **Layout gen/registry (FE/BE repo):** [CODEGEN-LAYOUT](https://github.com/raintr91/codegenkit/blob/main/docs/CODEGEN-LAYOUT.md)

---

## Authoring & IR

| Lệnh | Input | Output |
|------|--------|--------|
| `pnpm spec:split -- <bundle.yaml>` · `docskit split` | Bundle | `ir/design.yaml`, `ir/spec.yaml`, `ir/generated/<slug>.md` |
| `pnpm spec:merge -- <bundle.yaml>` | `ir/*` | Bundle |
| `pnpm spec:split:check` · `docskit split --check` | Bundle + ir | Fail nếu lệch / common thiếu design |
| `pnpm spec:split:all` · `docskit split-all` | Mọi `*.bundle.yaml` dưới surfaces | Split từng file |
| `pnpm docs:render` · `docskit render` | `ir/spec.yaml` (skip màn chưa split) | `ir/generated/*.md` + **`qa/index.md`** |
| `pnpm docs:publish` · `docskit publish` · MCP `docskit_docs_publish` | MD đã có + OpenAPI | **`CATALOG.md`** + link **đầu** README |
| `pnpm docs:dev` | VitePress | Sidebar: surfaces (kèm `ir/generated`) + **QA** cuối |

GitHub: README → `CATALOG.md` (platform / product / QA) → click mở trang MD. Không lục YAML.

---

## Common (shared)

| Lệnh | Mục đích |
|------|----------|
| `pnpm spec:split:common` / `docs:render-common` | `product/surfaces/…/common/yaml` — split **phải** có `ir/design.yaml` |
| Codegenkit `/gen-common` rồi `/prototype` | FE molecules trước screen |

## API (cùng leaf với FE)

| Lệnh | Mục đích |
|------|----------|
| `/api-spec` | Inventory từ **`ir/design.yaml` actions** (`apiRefs` / `#reuse-api` + `reuseFrom`). Unique → `api/<seq>/` trio. Toàn reuse → **zero** `api/` |
| `docskit openapi:gen --spec …/01-backend-spec.yaml` | Ghi sibling `02-openapi.yaml` |
| `docskit api:check --spec …/01` | Gate 01 (không để 01 sát leaf) |
| `/qa-resolve QA-…` | Đóng một file `qa/open`, patch bundle/01, split |

## Phase aggregates

| Lệnh | Chạy | Dùng sau |
|------|------|----------|
| `pnpm phase:spec -- <bundle.yaml>` | split → check → `docs:render` | Sửa 1 màn |
| `pnpm phase:spec` | `split-all` → `docs:render` | Cả hub |
| `pnpm docs:publish` | catalog GitHub | Sau render (không gộp vào render) |

## Codegen — FE (`codegenkit gen`)

**Input:** `ir/design.yaml` (`--id` hoặc `--spec`). **Không** `ir/spec.yaml`.

| Lệnh | Mục đích |
|------|----------|
| `codegenkit gen:dry -- --id W-*` / `--spec …/ir/design.yaml` | Gate sau `/grill-dev` |
| `codegenkit gen` | Scaffold FE repo |
| `codegenkit contract-gen` | FE models từ **design** (lane FE) |

## Codegen — BE (`codegenkit api-gen`)

**Input:** `…/api/<seq>/01-backend-spec.yaml` only. `--id CMP-*` glob mọi 01 dưới module.

## Unit / E2E

| Lệnh | Input |
|------|--------|
| `codegenkit` unit-gen FE | `ir/design.yaml` |
| `codegenkit api-unit-gen` | `01-backend-spec.yaml` |
| Testkit `testcase:gen --id …` | Plans `base-tests` + docs `ir/design.yaml` / `FLOW-*` |

## Docs-mark

| Skill | Mục đích |
|-------|----------|
| `/docs-mark` | Tags trên **bundle** rồi split (không ghi `ir/spec` tay) |
| `/grill-dev` | Common candidates; `#reuse-api` trên **action/item** |

---

## Ví dụ

```bash
pnpm spec:split -- product/surfaces/admin/CMP-01/01/01/01/<slug>.bundle.yaml
pnpm docs:render
pnpm docs:publish
pnpm docs:dev

# FE
codegenkit gen:dry --docs-root ~/workspace/base-docs -- --spec …/ir/design.yaml

# BE
codegenkit api-gen:dry -- --spec …/api/01/01-backend-spec.yaml
```

Thứ tự team: [DESIGN-PHASE-DIAGRAM](./DESIGN-PHASE-DIAGRAM.md)
