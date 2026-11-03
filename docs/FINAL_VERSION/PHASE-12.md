# PHASE 12 — Automation Workflow Builder

**Status:** PENDING OWNER APPROVAL | **Version:** 1.0 | **Target:** Antigravity

## Objective and feature scope
Trigger/condition/action graph, draft/publish versions, assignment rules, schedules, retries, dead-letter monitoring and loop protection.

## Master checklist traceability
AUT-01..AUT-13

## Required deliverables
Versioned executions, step logs, retry dedupe and safe rollback.

## Role and dashboard behavior
- Platform Super Admin: manage tenant/subscription metadata and aggregated platform KPIs; customer lead PII restricted by default.
- Company Admin: company-wide configuration and reports, subject to entitlements.
- Manager: team-scoped leads, calls and reports where granted.
- Agent/Counselor: own assigned records and permitted actions only; reassignment revokes prior ownership visibility.
- For modules not used by a role, deny access rather than showing unauthorized data.

## Mandatory implementation rules
- This is a planning document, not permission to start coding. Work only on this phase after owner approval.
- All company-specific fields, limits, status catalogs, filters, widgets and permissions must be metadata/database-driven; never hardcode company data or dashboard metrics.
- Enforce tenant isolation, RBAC and entitlements in backend queries and jobs, not only UI. Preserve audit history.
- Use actual persisted records for all metrics. Show loading, empty, error and permission-denied states.
- No credentials in GitHub. Use sandbox/mock providers until integration is separately authorized.
- Before implementation, inspect existing code and preserve working features; document migration and rollback.
## Antigravity execution sequence
1. Read this phase, master checklist and earlier approved phase completion reports.
2. Produce a screen-by-screen design, permissions matrix, API contracts, data model and test plan; flag open decisions before coding.
3. Implement only the approved scope, with unit, integration and role/tenant security tests.
4. Provide screenshots/demo, test evidence, changed-file summary, known issues and rollback steps.
5. **STOP** for user testing. If a bug is found, fix and retest within this phase. Do not start the next phase without explicit approval.
## Acceptance gate
- All listed requirements mapped to screen, API, storage and test IDs.
- Automated tests pass; manual happy-path, error-path and access-denied tests recorded.
- Owner marks this phase APPROVED. Until then status is PENDING APPROVAL.

## Owner sign-off
Decision: [ ] APPROVED [ ] CHANGES REQUESTED | Date: ____ | Notes: ____
