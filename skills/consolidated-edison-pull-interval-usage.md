---
name: Pull interval energy usage for an authorized Con Edison customer
description: Walk a Green Button Connect subscription down to interval readings and convert raw ESPI values into real kWh or cubic feet using the ReadingType multiplier.
api: openapi/consolidated-edison-green-button-connect-openapi.yml
operations:
  - getAllUsagePointsBySubscription
  - getUsagePointsForSubscription
  - getAllMeterReadingsForUsagePointInSubscription
  - getMeterReadingForUsagePointInSubscription
  - getAllIntervalBlocksForUsagePointMeterReadingInSubscription
  - getIntervalBlocksForUsagePointMeterReadingInSubscription
  - getAllReadingTypes
  - getReadingTypesById
  - getLocalTimeParameters
  - getAllElectricPowerUsageSummaries
generated: '2026-09-05'
method: generated
source: Con Edison GBC Third-Party Technical Onboarding Document v4.4
---

# Pull interval usage

Every resource hangs off the `subscriptionId` the customer's authorization created. Send the bearer access token on every call; anonymous requests return 401.

## The walk

1. `getAllUsagePointsBySubscription` — `GET /resource/Subscription/{subscriptionId}/UsagePoint`. One usage point per metered service location and commodity. An account with a house and a store has two.
2. `getAllMeterReadingsForUsagePointInSubscription` — `GET /resource/Subscription/{subscriptionId}/UsagePoint/{usagePointId}/MeterReading`.
3. `getAllIntervalBlocksForUsagePointMeterReadingInSubscription` — `GET .../MeterReading/{meterReadingId}/IntervalBlock`. This is the time series: `IntervalReading` entries with `value`, `timePeriod.duration`, `timePeriod.start` and `ReadingQuality`.
4. `getAllReadingTypes` / `getReadingTypesById` — you MUST fetch this. The reading value is meaningless without it.

For billing-period rollups use `getAllElectricPowerUsageSummaries` (`GET .../UsagePoint/{usagePointId}/UsageSummary`), which carries `overallConsumptionLastPeriod` and `BillLastPeriod`. ESCO charges, where the customer has one, are inside `BillLastPeriod`.

## Scaling — get this wrong and every number you show is wrong by 1000x

```
actual = IntervalReading.value * 10 ^ ReadingType.powerOfTenMultiplier
```

Con Edison's current multipliers are `3` for electric and `0` for gas. `uom` 72 is watt-hours; `uom` 119 is cubic feet. Gas is billed in CCF. Never assume the multiplier — read it from the ReadingType attached to the reading you are scaling.

## Time filtering

Use `published-min` and `published-max` (note the hyphens — the Batch family spells the same filters `publishedMin`/`publishedMax`). `startIndex` is the only paging control; there is no page size and no next-link.

Timestamps must be UTC, computed from the local window with the correct offset: UTC-5 in EST, UTC-4 in EDT. Con Edison's own examples:

```
published-Min=2025-09-15T04:00:00Z&published-Max=2025-09-16T04:00:00Z   # DST, 288 five-minute intervals
published-Min=2025-02-15T05:00:00Z&published-Max=2025-02-16T05:00:00Z   # EST, 288 intervals
published-Min=2024-11-03T04:00:00Z&published-Max=2024-11-04T05:00:00Z   # DST ends, 300 intervals
published-Min=2025-03-09T05:00:00Z&published-Max=2025-03-10T04:00:00Z   # DST starts, 276 intervals
```

Do not assume 288 intervals per day. `getLocalTimeParameters` returns the DST rules if you would rather compute them than hard-code them.

## What you can and cannot ask for

- Two years of history from today. Nothing older.
- Interval granularity depends on the meter: 5-minute for electric commercial AMI, 15-minute for electric residential AMI and legacy interval meters, 1-hour for gas AMI, monthly for non-interval meters.
- Solar accounts return NET consumption; non-solar return consumption only.
- Historical data settles: 80-90% available within 24 hours, 99% within 3 days, 99.8% within 7 days. Do not treat a same-day gap as missing data.
- `ReadingQuality.quality` of 17 means good and validated. Real-time readings are provisional and are explicitly not billing quality.

## Real-time

The `/resource/RealTime/...` family mirrors the historic one (`getAllRealTimeIntervalBlocksForUsagePointMeterReadingInSubscription`, `getAllRealTimeReadingTypes`). It covers electricity only, the last 24 hours only, and lags 45 minutes behind the request. It sits outside the Green Button V3.3 standard — it is a Con Edison extension, so do not expect it from other utilities.
