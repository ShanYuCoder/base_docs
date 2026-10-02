# Design phase — Pipeline cycle

> Chi tiết **Phase 1** · Overview màu: [FULL-CYCLE-PIPELINE-DIAGRAM](./FULL-CYCLE-PIPELINE-DIAGRAM.md) · Hub: [Toolchain index](./index.md)

Diagram tách nhỏ: [FEATURE-ARTIFACT-GRILL](./FEATURE-ARTIFACT-GRILL.md) · [FEATURE-ARTIFACT-BUNDLE-IR](./FEATURE-ARTIFACT-BUNDLE-IR.md)

Gam màu Design = **emerald** (khớp full-cycle). Architecture gate = **blue** (Phase 0).

---

## Architecture gate (Phase 0 — trước Design khi cần)

Chỉ chạy khi **overview / surface / module / business-process boundary** thay đổi hoặc cần integration/deployment mới. Skip nếu các mapping này đã ổn định.

```mermaid
flowchart LR
  subgraph P0["0 Architecture"]
    direction TB
    NEED{"Boundary đổi?"}
    ARC["/architecture"]
    ROUTE["Chọn child skill cần thiết:<br/>/overview · /surfaces · /module<br/>/business-process · /db-erd · /cross-service<br/>/deployment · /decision"]
    READY["Architecture boundary ready"]
    NEED -->|yes| ARC --> ROUTE --> READY
    NEED -->|no| SKIP["→ Design"]
  end

  classDef p0 fill:#DBEAFE,stroke:#1D4ED8,color:#1E3A8A
  classDef p0acc fill:#93C5FD,stroke:#1E40AF,color:#1E3A8A
  class NEED,ARC,ROUTE,READY,SKIP p0
```

Skills: `/architecture` router · `/overview` · `/surfaces` · `/module` · `/business-process` · `/deployment`. Product `FLOW-*`: `architecture/03-business-process/`.

---

## Design cycle (Phase 1)

```mermaid
flowchart TD
  subgraph IN["Entry"]
    LEG["/legacy /spec\ntrace + bundle"]
    SPEC["/spec\nspecOrigin: requirement"]
  end

  subgraph CORE["Bundle → grill → dry"]
    BUNDLE["*.bundle.yaml"]
    SPLIT["pnpm spec:split"]
    BQA["/grill-bqa"]
    DEV["/grill-dev\nbundle.gen"]
    DRY["codegenkit gen:dry\nir/design.yaml"]
  end

  subgraph OUT["Prototype → scaffold handoff"]
    PR["/prototype"]
    GEN["codegenkit gen"]
    GP["/grill-prototype"]
    NEXT["Phase 2 Tests + API"]
  end

  GW["/grill-docs\noptional"]
  US["/update-spec"]

  LEG --> BUNDLE
  SPEC --> BUNDLE
  BUNDLE --> SPLIT --> BQA --> DEV --> DRY --> PR --> GEN --> GP --> NEXT
  DEV -.->|optional| GW --> DRY
  BQA -.->|gap| US
  DEV -.->|gap| US
  US --> BQA

  classDef entry fill:#A7F3D0,stroke:#047857,color:#064E3B
  classDef core fill:#D1FAE5,stroke:#059669,color:#064E3B
  classDef out fill:#ECFDF5,stroke:#10B981,color:#065F46
  classDef opt fill:#FEF3C7,stroke:#B45309,color:#78350F
  classDef gap fill:#FECDD3,stroke:#BE123C,color:#881337

  class LEG,SPEC entry
  class BUNDLE,SPLIT,BQA,DEV,DRY core
  class PR,GEN,GP,NEXT out
  class GW opt
  class US gap
```

Tint trong gam emerald: **Entry** đậm hơn · **Core** giữa · **Out** nhạt hơn · optional grill-with = amber (không phải bước default) · gap = rose.

---

## Ma trận lệnh

| Lệnh | Artifact |
|------|----------|
| `/architecture` … (Phase 0) | Overview, surfaces, modules, `architecture/03-business-process/FLOW-*`, deployment/ADR — không bundle Code |
| `/legacy /spec` | Platform DNA `/legacy` + Docskit `/spec` → legacy-dynamics + bundle.legacy |
| `/spec` | bundle design v1, `specOrigin: requirement` |
| `/grill-bqa` | Inventory UI vs common; AskQuestion / `qa/open` |
| `/grill-dev` | `bundle.gen` + 01 `action` → split `ir/design.yaml` |
| `/grill-docs` | Reconcile — **không** default |
| `/prototype` | Đọc **`ir/design.yaml`** (Codegenkit FE) |
| `/qa-resolve` | Đóng `qa/open/QA-…` |
| `pnpm docs:render` | `ir/spec.yaml` → `ir/generated/` + `qa/index.md` |
| `pnpm docs:publish` | `CATALOG.md` + README (GitHub) |

## Lệnh script (design phase)

Xem [FEATURE-ARTIFACT-COMMANDS](./FEATURE-ARTIFACT-COMMANDS.md).

## Tag & gap (phase này)

| Doc | Khi nào đọc |
|-----|----------------|
| [TECH-DEBT-FLOW](./TECH-DEBT-FLOW.md) | Grill defer câu hỏi → `#tech-debt:{id}` · step 0 mỗi grill |
| [UPDATE-SPEC-FLOW](./UPDATE-SPEC-FLOW.md) | Gap sau grill/prototype → `#update:*` |
| [FEATURE-ARTIFACT-GRILL](./FEATURE-ARTIFACT-GRILL.md) | Chuỗi bqa → dev → dry |

Grill step 0 (nhẹ): Module đã map đúng surface, business-process và deployment chưa? Missing → Phase 0 `/architecture` để route tới `/surfaces`, `/module`, `/business-process` hoặc `/deployment`; không bịa hierarchy trong bqa.

Sau `portal:gen:dry` pass → [Portal reference](https://github.com/raintr91/nuxt_4/blob/nuxt_v_3/docs/operational/PORTAL-CODEGEN.md) · [NEEDS-COMPONENT-FLOW](./NEEDS-COMPONENT-FLOW.md) (`#needs-component` trong `/prototype`).
