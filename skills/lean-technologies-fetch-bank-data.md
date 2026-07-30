---
name: Fetch a customer's bank data
description: After a LinkSDK connection, list accounts and pull balances and transactions from a connected bank.
api: openapi/lean-technologies-data-openapi.yml
operations: [createCustomer, fetchAccountsV2, fetchBalancesV2, fetchTransactionsV2]
---

# Fetch a customer's bank data

Use this to read Open Finance data after the customer has connected a bank via LinkSDK.

## Auth
Backend token (`scope=api`) as `Authorization: Bearer <jwt>`.

## Prerequisites
The customer completed LinkSDK `.connect()` with data permissions (`accounts`, `balances`, `transactions`), which creates an **Entity** (`entity_id`) per connected bank.

## Steps
1. `createCustomer` (once per user) → `customer_id`.
2. `fetchAccountsV2` with the `entity_id` → list of `account_id`s (IBAN, sub-type, status).
3. `fetchBalancesV2` with `entity_id` + `account_id` → balance objects (CLOSING_AVAILABLE, INTERIM_BOOKED, ...).
4. `fetchTransactionsV2` with `entity_id` + `account_id` → paginated transaction history (category, type, running balance).

## Notes
Prefer the synchronous flow (`async=false`). For async requests, poll results via the results webhook.
Data failures (`EU_ACCOUNT_LOCKED`, `BANK_MAINTENANCE`, `DUPLICATED_REQUEST`) are in `errors/lean-technologies-decline-codes.yml` — respect the documented retry windows.
