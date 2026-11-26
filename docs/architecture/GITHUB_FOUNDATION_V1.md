# MeeChain GitHub Foundation v1

Status: Preparation draft  
Repository: `mcb-tanawat/MeeChain-Ops-Dashboard`  
Baseline branch: `main`  
Proposed working branch: `foundation/github-foundation-v1`  
Owner / final approver: Tanawat Prawanta

## 1. Purpose

Establish GitHub as the canonical source repository for MeeChain application source, shared contracts, architecture decisions, governance documentation, tests, and version history.

GitHub is not the operational database and is not the evidence-object store. Runtime records belong in a central operational data service; original evidence files belong in controlled file/object storage. The central Integration Layer mediates shared access between applications and external systems.

## 2. Confirmed design principles

- Keep the user experience simple while keeping integration and verification complexity behind stable interfaces.
- Preserve application autonomy; applications consume shared contracts and central APIs rather than becoming independent integration hubs.
- Use least privilege and read-only integration by default.
- Preserve original evidence, provenance, timestamps, and content hashes.
- Distinguish verification status (`VALID`, `INVALID`, `UNVERIFIED`), freshness (`FRESH`, `STALE`, `UNKNOWN`), and processing status (`PASS`, `FAIL`, `BLOCKED`).
- Never treat a proposed or reviewed change as executed until a system result proves it.
- Owner approval remains the final authority. Agents submit changes through branches and pull requests; they do not merge to `main` or deploy without separate authorization.

## 3. Proposed repository layout

```text
MeeChain-Ops-Dashboard/
├── artifacts/
│   ├── api-server/
│   └── mockup-sandbox/
├── apps/                       # future app source, only after migration decision
├── services/
│   └── integration-api/        # future service; behavior deferred to app evidence
├── connectors/
│   ├── debian-besu/
│   ├── appdeploy/
│   ├── nito/
│   └── external/
├── lib/
│   ├── api-client-react/
│   ├── api-spec/
│   ├── api-zod/
│   ├── db/
│   ├── contracts/
│   ├── evidence/
│   └── connector-sdk/
├── docs/
│   ├── architecture/
│   ├── governance/
│   ├── security/
│   ├── api-contracts/
│   └── operations/
├── infra/
│   └── deployment/
├── scripts/
├── .github/
│   ├── workflows/
│   ├── CODEOWNERS
│   └── pull_request_template.md
├── README.md
├── SECURITY.md
└── CONTRIBUTING.md
```

This is a target layout, not a claim that these directories or capabilities already exist. Add directories only when there is a concrete file or deliverable to place in them. Do not create placeholder services that imply working integrations.

## 4. Backend and database decision boundary

The current backend and database are foundations, not proof of a complete application workflow. Do not infer business behavior or create production endpoints/tables from architecture diagrams alone.

### Can be prepared now
- Repository layout and ownership conventions.
- API contract versioning and review conventions.
- Connector interface principles.
- Evidence integrity requirements.
- Security and testing standards.
- Read-only CI checks that do not require production secrets.
- Architecture Decision Records (ADRs) that record confirmed decisions and unresolved questions.

### Must wait for real application evidence
- Concrete API endpoints and request/response behavior.
- Database entities, relationships, indexes, retention, and migration strategy.
- User roles, scopes, and permission matrix.
- Evidence upload, storage, correction, deletion, and audit lifecycle.
- Connector authentication, signing, retries, and provider-specific behavior.
- Production secrets, deployment environment, and live integration activation.

For every future API or data model, record its source evidence, owning application, observed workflow, validation rules, lifecycle, and compatibility requirements before implementation.

## 5. Application and integration boundaries

- `MeeChain Evidence Console` and `MeeChain QBFT Network Monitor` remain separate applications.
- Do not assume their AppDeploy source is already represented in this repository.
- Preserve the AppDeploy v1 snapshots and provenance before any source migration.
- Cross-application data sharing must use a central Integration API/Gateway; do not assume AppDeploy databases are shared.
- Debian/Besu integration remains read-only and private by default. No public RPC, signing, validator administration, or node start/stop controls are included in this foundation.

## 6. CI foundation

Initial CI should validate source integrity without requiring live credentials:
1. Install dependencies using the committed lockfile.
2. Run formatting/lint checks where configured.
3. Run TypeScript type checks.
4. Run unit and contract tests that exist.
5. Build affected workspaces.
6. Report results as PR checks.

Do not make deployment a CI side effect. Deployment remains a separate, explicitly approved action. Do not mark a workflow as a required branch check until the workflow exists, has been tested, and its check name is confirmed.

## 7. Release and traceability

- Use short-lived feature branches.
- Use pull requests for review and evidence.
- Use immutable version tags for releases after approval.
- Preserve commit SHA, PR link, test results, release tag, and deployment reference.
- Keep original application snapshots and hashes when migrating code.
- Never place credentials, tokens, private keys, or production `.env` files in Git.

## 8. Deferred decisions

The following remain intentionally unresolved until the real application workflows and provider constraints are evidenced:
- Production database schema and retention policy.
- Central API hosting and authentication mechanism.
- Connector credential storage and rotation.
- Evidence object storage provider and lifecycle.
- Secret-scanning configuration and alert response procedure.
- Branch-protection required checks and review configuration.
- Deployment promotion and rollback policy.

## 9. Change boundary

This document authorizes preparation and review of the Foundation design only. It does not authorize merging to `main`, deploying applications, publishing a public API, configuring production secrets, activating live connectors, or changing Debian/VM/Besu runtime state.