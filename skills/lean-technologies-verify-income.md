---
name: Run income and credit assessment
description: Turn a connected bank account into income verification and pre-computed credit indicators for underwriting.
api: openapi/lean-technologies-insights-openapi.yml
operations: [verifyIncome, getCreditAssessments, createBankStatementsReport]
---

# Run income and credit assessment

Use this to underwrite a borrower from their connected-account data (Lean Insights).

## Auth
Backend token (`scope=api`).

## Prerequisites
An `entity_id` from a LinkSDK connection with data permissions (transactions/balances).

## Steps
1. `verifyIncome` with the `entity_id` → income insights (salary vs non-salary, monthly patterns, stability stats).
2. `getCreditAssessments` → pre-computed indicators (DBR, affordability ratio, net cashflow, max installment capacity); pass your policy config to calibrate.
3. Optionally `createBankStatementsReport` for a downloadable statements report.

## Notes
These read-only insight calls are safe to retry. Use them for automated credit decisioning, DBR calculation, and reducing manual underwriting.
