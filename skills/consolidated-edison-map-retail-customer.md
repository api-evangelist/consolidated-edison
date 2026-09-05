---
name: Map Con Edison subscriptions to real accounts, agreements and meters
description: Resolve a Green Button subscription down through customer account, service agreement, service location and meter so usage data can be attributed to a real premise.
api: openapi/consolidated-edison-green-button-connect-openapi.yml
operations:
  - getCustomerInformationBySubscription
  - getCustomerAccountInSubscription
  - getCustomerAccountByAccountIdInSubscription
  - getCustomerAgreementByAccountIdInSubscription
  - getCustomerAgreementByCustomerAgreementId
  - getAllServiceLocationByCustomerAgreementIdAndAccountIdInSubscription
  - getServiceLocationByServiceLocationId
  - getAllMetersForServiceLocationInSubscription
  - getMeterBySerialNumberId
generated: '2026-09-05'
method: generated
source: Con Edison GBC Third-Party Technical Onboarding Document v4.4
---

# Map a subscription to a real premise

Usage data arrives keyed on `subscriptionId` and `usagePointId`, neither of which means anything to a customer. The Retail Customer resources are how you attach it to an account number and an address. This requires functional blocks 51 and 53 at minimum in the granted scope; 56, 57, 58 and 60 add billing, agreement, service-location and meter detail.

## The walk

1. `getCustomerInformationBySubscription` — `GET /resource/Customer/{subscriptionId}`
2. `getCustomerAccountInSubscription` — `GET /resource/Customer/{subscriptionId}/CustomerAccount`. Returns the account number and address (street, city, state, town, postal code).
3. `getCustomerAgreementByAccountIdInSubscription` — `GET .../CustomerAccount/{accountId}/CustomerAgreement`. One agreement per active service; an account with electric and gas has two.
4. `getAllServiceLocationByCustomerAgreementIdAndAccountIdInSubscription` — `GET .../CustomerAgreement/{customerAgreementId}/ServiceLocation`
5. `getAllMetersForServiceLocationInSubscription` — `GET .../ServiceLocation/{serviceLocationId}/Meter`, then `getMeterBySerialNumberId` for one meter by serial.

## Cardinality that will bite you

Con Edison publishes these relationships explicitly, and they are not all one-to-one:

- Account to Usage Point: **1 to N**. A customer with a house and a store has one account and two usage points.
- Account to Reading Type: **1 to N**. An electric account carries both Total Consumption and Net Consumption (solar export/import) reading types.
- Usage Point to Meter: **1 to 1**. Each apartment has its own meter even under one building agreement.
- Usage Point to Meter Reading: **1 to N** over time.
- Meter Reading to Interval Block: **1 to 1**.
- Account to Customer Agreement: **1 to N** (1 to 1 for a single service).
- Customer Agreement to Usage Point: **1 to N**. A commercial agreement can cover several warehouse locations.

Do not model account-to-meter as one-to-one. Multifamily and commercial accounts break that assumption immediately.

## Your obligation

Con Edison states it as a third-party responsibility: you must map and maintain the Account ID against the usage data you store. Usage without an account mapping cannot be reconciled to a bill, and a re-authorization can hand you a new `subscriptionId` for the same premise.

## Batch alternative

`getCustomerInformationInBatch` (`GET /resource/Batch/RetailCustomer/{subscriptionId}`) pulls the same customer information asynchronously — see `skills/consolidated-edison-batch-retrieval.md`.
