---
name: Collect a pay-by-bank payment
description: Create a customer, define a payment intent and destination, then confirm settlement via Lean Open Finance.
api: openapi/lean-technologies-payments-core-openapi.yml
operations: [createCustomer, createPaymentDestination, createPaymentIntent, getPaymentIntentById, getPaymentById]
---

# Collect a pay-by-bank payment

Use this to take a single bank-to-bank payment with Lean (UAE Open Finance).

## Auth
Mint a backend token: `POST https://auth.leantech.me/oauth2/token`
(`grant_type=client_credentials`, `scope=api`). Send `Authorization: Bearer <jwt>`.
For the LinkSDK step, mint a `customer.<customer_id>` token (>= 10 min validity).

## Steps
1. `createCustomer` with your `app_user_id` (map 1:1 to your user). Store `customer_id`.
2. `createPaymentDestination` — the account to be paid (IBAN + legal name). Store `payment_destination_id`.
3. `createPaymentIntent` — the amount + currency contract. **Send an `Idempotency-Key` header** (unique per operation). Store `payment_intent_id`.
4. Hand `payment_intent_id` to the LinkSDK `.pay()` / `.checkout()` on the client so the payer authorises at their bank.
5. Poll `getPaymentIntentById` / `getPaymentById`, or better, consume the payment-status webhook (`lean-signature`, HMAC-SHA512; dedupe on `event_id`).

## Error handling
Failure codes arrive on the payment status and via webhook — see `errors/lean-technologies-decline-codes.yml`
(`INSUFFICIENT_BALANCE`, `USER_CANCELLED`, `BANK_ISSUE`, ...). Retry bank-side failures with backoff; never retry a write without the same `Idempotency-Key`.
