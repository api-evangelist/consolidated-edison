---
name: Authorize a Con Edison customer and mint Green Button tokens
description: Take a Con Edison or Orange & Rockland retail customer through the Green Button Connect consent flow and exchange the resulting authorization code for a refresh token and a one-hour access token.
api: openapi/consolidated-edison-green-button-connect-openapi.yml
operations:
  - Token
  - Get all Third Party Authorizations
  - Get Third Party Authorization by ID
  - getThirdPartyApplicationById
generated: '2026-09-05'
method: generated
source: Con Edison GBC Third-Party Technical Onboarding Document v4.4
---

# Authorize a Con Edison customer

You cannot call this API until Con Edison has onboarded you. Registration, the Data Security Agreement and technical testing take 30 to 60 days and are driven by `dlsharemydatatech@coned.com`. Everything below assumes you already hold a `client_id`, `client_secret` and Registration Access Token.

## Before you start

- Production base: `https://api.coned.com/gbc/espi/1_1`. Test base: `https://apit.coned.com/gbc/espi/1_1`.
- Con Edison (CECONY) and Orange & Rockland (ORU) are separate data custodians with the same contract. Route the customer to the utility that serves the account being authorized, or the authorization will fail.

## 1. Retrieve your own registration

Call `getThirdPartyApplicationById` (`GET /resource/ApplicationInformation/{applicationInformationId}`) with the Registration Access Token to read back your `client_id` and `client_secret`. Do this once; do not hard-code credentials from email.

## 2. Send the customer to the utility

Redirect to the utility authorization server. Production: `https://www.coned.com/en/oauth/authorize` (ORU: `https://www.oru.com/en/oauth/authorize`). Test: `https://uat10.coned.com/en/oauth/authorize`.

Include `scope`, `client_id`, `redirect_uri`, `state` and `response_type`. The scope is a structured ESPI expression, not a scope list — see `scopes/consolidated-edison-scopes.yml`. A working consumption scope Con Edison publishes:

```
FB=1_3_4_5;IntervalDuration=Monthly_3600_900_300;BlockDuration=Monthly_Daily;HistoryLength=63072000;BR=000092
```

Functional blocks 1, 3 and 4 are mandatory for any consumption scope; 1 and 3 for billing; 51 and 53 for retail customer data. If any redirect parameter is missing or wrong the customer sees an error page rather than a consent screen. The customer may REMOVE preselected scopes on Con Edison's page but never add them, so ask for the minimum you need.

## 3. Exchange the code

`Token` (`POST /oauth/Token`), `Content-Type: application/x-www-form-urlencoded`, `Authorization: Basic base64(client_id:client_secret)`:

```
grant_type=authorization_code&code={code}&redirect_uri={your_redirect_uri}
```

The response carries `access_token`, `refresh_token`, `token_type: Bearer`, `expires_in` and the scope the customer actually granted — which may be narrower than what you asked for. Read the granted scope and adjust what you call.

## 4. Store the refresh token like it is irreplaceable, because it is

Con Edison issues the refresh token exactly once, at authorization time. If you lose it, the only recovery is to ask the customer to revoke and re-authorize. Persist it before you do anything else.

## 5. Refresh, and cache

Access tokens live 60 minutes. Store an expiry timestamp alongside the token and mint a new one before it lapses:

```
grant_type=refresh_token&refresh_token={refresh_token}&scope={granted_scope}
```

Reuse one access token for every call against that subscription for its full hour. The token endpoint is rate limited to 50 calls per minute across all flows except `authorization_code`, and it is the only rate limit Con Edison publishes — a per-request token mint will hit it.

## 6. Reconcile your authorizations

`Get all Third Party Authorizations` (`GET /resource/Authorization`) lists every live consent you hold; `Get Third Party Authorization by ID` (`GET /resource/Authorization/{authorizationId}`) reads one. There is no webhook for grants or revocations, so poll this to discover state changes.

## What silently ends an authorization

- The customer chooses "Stop Sharing My Data".
- A "share one time" authorization passes 24 hours; a "share until a date" authorization passes its end date.
- The authorization goes 365 days without being used — it is revoked automatically. Touch each authorization at least annually.
- The refresh token goes a year unused and expires.
- The account is removed from the customer's online profile.

All of these surface as a 401 or blocked access, not as a distinct error type. See `errors/consolidated-edison-problem-types.yml`.
