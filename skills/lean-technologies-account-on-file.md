---
name: Set up recurring Account-on-File payments
description: Create an authorised Account-on-File consent and initiate variable-value payments against it.
api: openapi/lean-technologies-account-on-file-openapi.yml
operations: [createAccountOnFileConsent, fetchConsent, initiateAccountOnFilePayment]
---

# Set up recurring Account-on-File (AoF) payments

Use this for merchant-/user-initiated variable-value payments (loan collections, returning checkout) under one long-lived authorisation.

## Auth
Backend token (`scope=api`).

## Steps
1. `createAccountOnFileConsent` — define the beneficiary and the consent limits (amount, frequency, validity). Store `consent_id`.
2. The customer authorises the consent via LinkSDK; confirm state with `fetchConsent` (must be `AUTHORISED` before initiating).
3. `initiateAccountOnFilePayment` against the authorised `consent_id` for each payment. **Send an `Idempotency-Key`.**
4. Track consumption with `fetchConsent` (returns consumption data for authorised consents).

## Notes
Consent lifecycle + limits: https://docs.leantech.me/docs/consent-lifecycle .
Revoke with the consent revoke operation when done. Terminal failures surface via webhook (`event_id`).
