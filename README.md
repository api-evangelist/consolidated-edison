# Consolidated Edison (consolidated-edison)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
