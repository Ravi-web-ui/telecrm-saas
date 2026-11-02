# Super Admin Panel — Development & Testing Specification (Draft v0.1)

Status: **Planning only — not approved for implementation**  
Product: Original multi-tenant SaaS CRM inspired by TeleCRM.  
Reference: User's master checklist and read-only feature audit (2026-11-01). This specification describes **our proposed product**, not a claim that all features were verified in TeleCRM.

## 1. Scope and user roles
- Platform Super Admin: manage tenant companies, plan catalog, subscriptions, feature entitlements, platform-level audit and support.
- Platform Support (restricted): view tenant metadata and support cases only as authorized; no automatic access to customer leads or call recordings.
- Company Admin and Employees are separate tenant-scoped roles; their detailed screens will be specified in later documents.

## 2. Screens and acceptance criteria

### SA-01 Authentication and access
- Secure sign-in, logout, password reset, MFA for privileged accounts, session timeout.
- Only platform-authorized users can access `/super-admin/*`.
- Tests: unauthenticated access rejected; company admin cannot enter; expired sessions rejected; MFA challenge enforced.

### SA-02 Overview dashboard
- Tenant counts by state, active/trial/expired subscriptions, upcoming renewals, revenue metrics with explicit currency and date range, failed payment alerts.
- Every metric links to its filtered underlying records.
- Tests: dashboard totals match filtered records; tenant data not disclosed in aggregate drilldowns to unauthorized roles.

### SA-03 Company directory and onboarding
- Search, filter, create tenant, view tenant metadata, activate/suspend/reactivate, assign primary company admin, record onboarding state.
- Unique tenant ID and tenant-scoped domain/slug; no duplicate tenant creation on retried request.
- Tests: duplicate detection; authorized suspension; suspended tenant behavior; company A never reads company B's records.

### SA-04 Plan catalog and entitlements
- Versioned plans with **monthly, quarterly and yearly** billing cycles; per-seat limits, feature flags, quotas, trial length, price/currency and effective date.
- Changing a plan does not silently rewrite previously purchased subscriptions.
- Tests: plan-cycle validation; entitlement enforcement in backend APIs (not only UI); plan version compatibility.

### SA-05 Subscriptions and billing
- Subscription lifecycle: trial, active, past_due, grace_period, suspended, canceled, expired; renewal, upgrade/downgrade, seat changes, proration policy, invoices, taxes and refunds to be confirmed.
- Payment gateway webhooks must be authenticated, idempotent, replay-safe and auditable.
- Tests: monthly/quarterly/yearly renewal boundaries, failed payments, duplicate webhook delivery, downgrade over seat quota, cancellation effective date.

### SA-06 Tenant access and feature controls
- Per-company seat usage, quotas, enabled integrations and plan overrides with justification, actor and expiration.
- Explicit separation of tenant admin privileges from platform admin privileges.
- Tests: feature disabled server-side; overrides logged and reversible; least-privilege authorization.

### SA-07 Platform audit, support and monitoring
- Immutable audit events for tenant creation, plan edits, subscription changes, privileged access, suspensions and support access.
- Health indicators for API, database, background jobs, webhook processing and integrations; avoid exposing secrets or customer payloads.
- Tests: actor/time/before-after event captured; restricted log access; sensitive values redacted.

## 3. Cross-cutting technical requirements
- Tenant isolation enforced at service and database query boundaries; never trust tenant ID supplied by a client.
- Role-based authorization with explicit permission checks; auditable administrative actions.
- Encrypt traffic and sensitive stored data; manage secrets outside source control; define retention and backups.
- Prefer a modular monolith initially; finalize stack after evaluating hosting, team skills, telephony, WhatsApp and reporting constraints.
- API design: versioned endpoints, pagination, filtering, validation, idempotency for state-changing payment/onboarding operations.
- Test pyramid: unit, API/integration, UI end-to-end, security/tenant-isolation, and smoke tests. CI must block merges on failing critical checks.
- No live credentials, real customer records or provider tokens in this repository, particularly while it is public.

## 4. Definition of Done — every module
1. Approved requirement and UI states, including loading/empty/error states.
2. Database migration and rollback strategy reviewed.
3. Backend permission and tenant-isolation tests passing.
4. Automated unit, integration and end-to-end tests passing.
5. Manual acceptance on a staging environment using synthetic data.
6. Audit events and operational monitoring verified.
7. Code reviewed; no high-severity security issues; owner approves before merge/deployment.

## 5. Open decisions — do not silently assume
- SaaS brand, deployment region and currency/tax treatment.
- Technology stack, hosting, identity provider and payment gateway.
- Pricing, seat counting, trial/grace periods, refunds, prorations and invoice rules.
- Whether platform staff may impersonate tenant users (default: **no**).
- Tenant data retention/deletion and compliance requirements.
- Plan limits and which modules are P0/P1/P2.
- Lead reassignment policy is known for the Employee module: reassigned leads leave the previous counselor's active list, appear in the new counselor's list, and remain visible to authorized admins in the Activity Timeline.

## 6. Next implementation sequence
1. Approve this specification and resolve open decisions.
2. Create architecture decision records, schema and permission matrix.
3. Build platform auth and tenant boundary with tests.
4. Implement SA-03 company onboarding and SA-04 plan catalog.
5. Implement SA-05 billing only after gateway and lifecycle rules are agreed.
6. Add dashboards, audit/monitoring, end-to-end regression tests.
7. Review each increment before moving to Company Admin and Employee modules.

**Antigravity instruction:** Read this file and return a proposed architecture, task breakdown, dependencies and test matrix. Do not generate production code, connect live services or deploy until explicit approval.
