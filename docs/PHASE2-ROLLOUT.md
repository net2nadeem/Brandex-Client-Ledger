# Phase 2 rollout

## What this adds

- Uppercase text presentation and input normalization.
- A case-centric client ledger with TM/folder/application search, stage and outstanding filters, date filtering, and per-case outstanding totals.
- Per-entry charge, receive, edit, soft-void, and print actions. Charges and receivings have distinct labels and print styling.
- A six-month charge/receiving chart, a today KPI, and a recent-activity limit of 10 rows.
- Staff sign-in, database policies that require the `authenticated` role, FIFO allocation of case-linked general receipts, and database-enforced stage sequencing.

## Safe deployment order

1. Take a database backup and review the migration against the active schema.
2. Apply `supabase/migrations/004_staff_auth_stage_gates.sql` in a test database first.
3. Create and test staff accounts in Supabase Auth. The migration grants data access to every authenticated staff account; restrict account creation to trusted administrators.
4. Verify sign-in, RLS denial for anonymous requests, case-linked lump-sum allocation, stage-gate rejection, soft void, and reports using disposable test data.
5. Deploy the frontend only after migration and staff-account provisioning. The migration removes anonymous table access, so deploying the sign-in-required UI first can lock out staff.

The migration does not delete, reset, or seed ledger data. Do not run destructive cleanup against production to test these changes. CMS integration and legacy-client import remain separate follow-up work; no service-role key belongs in browser code.
