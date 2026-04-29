# Consolidated Edison (consolidated-edison)

Consolidated Edison, Inc. (Con Edison) is a Fortune 500 holding company that, through its subsidiaries, provides electric, natural gas, and steam service to customers in New York City and Westchester County. Con Edison does not publish a general-purpose developer portal; programmatic data access is delivered through the Green Button Connect My Data (GBC) program, which lets authorized third parties retrieve customer energy usage data via the NAESB Energy Services Provider Interface (ESPI) standard once the customer grants consent through Con Edison's authorization portal.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/consolidated-edison/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- Energy
- Fortune 500
- Green Button
- Natural Gas
- New York
- Steam
- Utility

## Timestamps

- **Created:** 2026-03-21
- **Modified:** 2026-04-28

## APIs

### Green Button Connect My Data
Green Button Connect My Data is the OAuth2-based ESPI service that lets Con Edison customers authorize a registered third party to receive their interval energy usage and account data on a recurring basis. Third parties register through Con Edison's onboarding process to obtain client credentials, then exchange them for access tokens that retrieve ESPI Atom/XML resources for usage points, meter readings, and electric power usage summaries.

**Human URL:** [https://www.coned.com/en/accounts-billing/share-energy-usage-data/become-a-third-party](https://www.coned.com/en/accounts-billing/share-energy-usage-data/become-a-third-party)

#### Tags

- Energy, Green Button, OAuth2, Smart Meter, Usage Data

#### Properties

- [Documentation](https://www.coned.com/en/accounts-billing/share-energy-usage-data/become-a-third-party)
- [Authentication](https://www.coned.com/en/business-partners/access-customer-data)

### Green Button Download My Data
Customer-driven file export that lets Con Edison residential and small commercial accounts download up to one year of smart-meter interval data as CSV or ESPI XML directly from the My Account portal. Useful for one-shot analytics, audits, and migrating data into third-party tools without requiring an OAuth integration.

**Human URL:** [https://www.coned.com/en/save-money/make-better-energychoices-with-green-button](https://www.coned.com/en/save-money/make-better-energychoices-with-green-button)

#### Tags

- CSV, Energy, Green Button, Self-Service, Usage Data, XML

#### Properties

- [Documentation](https://www.coned.com/en/save-money/make-better-energychoices-with-green-button)

## Common Properties

- [Website](https://www.coned.com)
- [Customer Portal](https://www.coned.com/en/accounts-billing)
- [Become a Third Party](https://www.coned.com/en/accounts-billing/share-energy-usage-data/become-a-third-party)
- [Share My Data Overview](https://www.coned.com/en/accounts-billing/share-energy-usage-data/share-my-data)
- [Investor Relations](https://investor.conedison.com)
- [Careers](https://www.conedjobs.com)
- [Privacy Policy](https://www.coned.com/en/about-us/privacy-statement)
- [Terms of Service](https://www.coned.com/en/about-us/terms-of-use)
- [Support](https://www.coned.com/en/contact-us)

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
