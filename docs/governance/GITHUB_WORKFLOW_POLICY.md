# MeeChain GitHub Workflow Policy

Status: Proposed operating policy  
Repository: `mcb-tanawat/MeeChain-Ops-Dashboard`

## Authority

- Project Owner: Tanawat Prawanta — sole final approver and decision authority.
- ChatGPT: Technical Reviewer and Governance Assistant — may inspect, challenge, prepare, and recommend; has no approval or merge authority.
- Agents: implementation contributors — work on feature branches and submit pull requests.

No third-party approver is assumed in the current governance model.

## Branch model

- `main`: stable integration baseline.
- `foundation/*`: Foundation preparation work.
- `feature/*`: application or capability work.
- `fix/*`: corrective work.
- `docs/*`: documentation-only work.

Branch names are conventions, not evidence that branch protections are configured.

## Pull request flow

1. Agent creates or uses a feature branch from the current `main`.
2. Agent commits scoped changes to that branch only.
3. Pull request describes purpose, scope, affected components, risks, test evidence, and rollback considerations.
4. Automated checks run where configured.
5. Technical review identifies defects, missing evidence, and policy conflicts.
6. Project Owner reviews and decides whether to approve and merge.
7. Deployment, live integration, and runtime changes require separate authorization.

A pull request or successful CI run is not itself deployment approval.

## Main branch protection proposal

After current GitHub settings and plan constraints are verified:
- Restrict direct pushes to `main`.
- Require pull requests for agent-contributed changes.
- Require the Project Owner's review/approval before merge.
- Disable force pushes and deletion of `main`.
- Require only CI checks that have been implemented and validated.
- Keep bypass permissions limited to the Project Owner and explicitly documented emergency cases.

Do not configure a required status check using a guessed check name.

## Agent permission proposal

- Agent writes to its feature branch, not `main`.
- Agent cannot merge its own PR.
- Agent cannot change repository governance settings unless separately authorized.
- Agent cannot deploy or activate production integrations by virtue of repository write access.

## Secrets

- Never commit credentials, tokens, private keys, production environment files, or signed webhook secrets.
- Keep `.env.example` limited to variable names and safe placeholders.
- Store actual secrets in an approved server-side secret manager or hosting platform secret store.
- Define secret-scanning settings and alert response only after repository exposure, hosting, and application workflows are understood.
- If a credential is suspected to have been committed, treat it as exposed and rotate/revoke it; deleting the visible file is not sufficient.

## Evidence required for completion

For each merged change retain:
- Branch and PR reference.
- Commit SHA.
- Review decision by the Project Owner.
- Relevant CI/test outputs.
- Changed file list.
- Any migration or deployment record, if separately approved.