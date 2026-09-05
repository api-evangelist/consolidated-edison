---
name: Retrieve Con Edison Green Button data in batch
description: Submit an asynchronous bulk or per-subscription batch request, handle the 202, and collect the assembled files from the callback notification before they expire.
api: openapi/consolidated-edison-green-button-connect-openapi.yml
operations:
  - getAllUsageDataInBatch
  - getAllUsageDataForSubscriptionInBatch
  - getUsagePointBySubscriptionInBatch
  - getCustomerInformationInBatch
  - getAllRealTimeUsageDataInBatch
  - getAllRealTimeUsageDataForSubscriptionInBatch
  - getRealTimeUsagePointBySubscriptionInBatch
  - Token
generated: '2026-09-05'
method: generated
source: Con Edison GBC Third-Party Technical Onboarding Document v4.4
---

# Batch retrieval

Batch is how you pull data for many customers at once instead of walking each subscription. It is asynchronous, and the asynchrony is the whole story.

## Which token

Bulk operations (`getAllUsageDataInBatch`, `getAllRealTimeUsageDataInBatch`) span every customer currently authorized to you, so they need a Third-Party Client Access Token, not a per-customer token:

```
POST /oauth/Token
Authorization: Basic base64(client_id:client_secret)
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&scope=FB=34_35
```

Per-subscription batch operations use that customer's access token.

## Submit

- `getAllUsageDataInBatch` — `GET /resource/Batch/Bulk/{bulkId}`
- `getAllUsageDataForSubscriptionInBatch` — `GET /resource/Batch/Subscription/{subscriptionId}`
- `getUsagePointBySubscriptionInBatch` — `GET /resource/Batch/Subscription/{subscriptionId}/UsagePoint/{usagePointId}`
- `getCustomerInformationInBatch` — `GET /resource/Batch/RetailCustomer/{subscriptionId}`

Bulk operations take `publishedMin` / `publishedMax` — camelCase here, unlike the hyphenated `published-min` on the Subscription resources. That inconsistency is in Con Edison's specification, not a typo in this skill.

## Handle the 202

HTTP 202 means Con Edison accepted the request and is assembling the response. It is not a failure and it is not something to poll.

**Do not resubmit.** The platform rejects a duplicate batch request while one is pending, and resubmitting does not shorten the wait. Notifications normally arrive within an hour, but under load can take up to 24 hours.

## Collect from the callback

Con Edison POSTs to the `third_party_notify_uri` you registered on the technical registration form. The body is XML carrying a `BatchList` of resource URLs — some for Bulk resources, some individual.

Two things break naive clients here:

1. **The URLs are XML-escaped.** `&amp;` must be unescaped before you invoke the URL. Parse the XML properly rather than regexing the string out.
2. **The window is two days.** Fetch each resource URL with the Authorization header belonging to that batch request within 2 days of the notification. After that the assembled response is deleted and the URL behaves as a brand-new batch request. Inside the 2 days, re-requesting the same parameters returns the cached response, so a retry is cheap.

Responses over 200 MB are chunked into multiple files. Handle a BatchList with more than one entry.

## Changing your notification URI

Update the field on the technical registration form and email it to `ShareMyDataTech@coned.com`. Do not rename or repoint a callback URL without telling Con Edison first — it breaks the existing integration silently.

See `asyncapi/consolidated-edison-webhooks.yml` for the full callback contract.
