# UK Company Check by Pagecraft: connector documentation

Last updated: 24 November 2026

## Endpoint and authentication
- Hosted MCP endpoint (streamable HTTP): `https://mcp.apify.com?tools=pagecraft/companies-house-monitor,pagecraft/companies-house-scraper`
- Authentication: OAuth with the user's own Apify account (Apify's hosted MCP server; sign in and approve on first connection). A Bearer Apify API token also works.
- Billing: pay per event, charged by Apify to the user's Apify account at the prices on each tool's Store page. Nothing is charged inside Muse.

## Tools (both READ-ONLY)
Neither tool creates, changes or deletes data at Companies House or anywhere else, sends messages or makes payments. Classification: **Read**. No sensitive writes.

### 1. pagecraft/companies-house-monitor (Read)
Checks UK companies for changes since a date.
- Inputs: `companyNumbers` (array of company numbers or Companies House URLs, up to 5,000), `lookbackDays` (default 7) or `since` (YYYY-MM-DD), `onlyChanged`, `checkFilings`, `checkOfficers`, `checkOwners`, `rememberState` (stores status, name, address and SIC codes in a key-value store in the user's own Apify account to detect changes next run), `stateStoreName`, optional `apiKey`.
- Output: one row per company: `companyNumber`, `companyName`, `companyStatus`, `since`, `hasChanges`, `changeCount`, `significantChangeCount`, `alertCount`, `summary`, `latestChange`, `latestChangeDate`, `changes[]` (filings, officer and owner changes with dates and links), `alerts[]` (not active, overdue accounts or confirmation statement, insolvency history, address in dispute), `companiesHouseUrl`, `note`, `error`, `checkedAt`.
- Price: $4 per 1,000 companies checked (Free plan), $3.60 Bronze, $3.20 Silver, $2.80 Gold.
- Full README: https://apify.com/pagecraft/companies-house-monitor

### 2. pagecraft/companies-house-scraper (Read)
Looks up companies by number or name, or searches the register (name, status, type, SIC code, location, incorporation and dissolution dates). Optional full details add officers and persons with significant control.
- Output: company profile (name, number, status, type, dates, registered address, SIC codes, accounts and confirmation statement dates and overdue flags, charges, previous names), optional officers and owners.
- Prices: see https://apify.com/pagecraft/companies-house-scraper (per company with full details, per officer or owner, per search result, per search request).

## Side effects
None outside the user's Apify account. Each run stores its results in the user's Apify dataset; the monitor may store a small state record in the user's key-value store when `rememberState` is on.

## Status and errors
Each call returns when the run finishes. Per-company problems are reported in that row's `error` field (for example company not found) and are not charged. Bad input and rate-limit refusals are not charged.

## Rate limits
Companies House allows 600 requests per 5 minutes per API key. Each company takes up to four requests, about 25 to 30 companies a minute with the shared key. Users can add their own free Companies House key for large lists.

## Data, licence and personal data
Data comes from the official Companies House API and contains public sector information licensed under the Open Government Licence v3.0. Officer and owner names are public but are personal data; users are responsible for using them lawfully.

## Test account for reviewers
A dedicated Apify test account with free usage credit is provided to Meta reviewers separately. Test input: company number `00445790` (TESCO PLC).

## Support
https://apify.com/pagecraft/companies-house-monitor/issues or pagecraftapi@gmail.com. Privacy: PRIVACY.md. Terms: TERMS.md.
