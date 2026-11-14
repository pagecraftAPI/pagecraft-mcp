# Company Enrichment by Domain

Send a company domain, get a company profile back in one call: name, description, logo, social profiles, business emails and phones, tech stack, email provider, DNS and SSL.

## Why this API
- **Live, not stale**: read from the company's own website at request time, so it covers small and brand-new companies that big data vendors miss.
- **Tech stack included**: 3,600+ technologies detected (CMS, ecommerce, analytics, marketing, payments, hosting) at no extra cost.
- **Contacts you can use**: role addresses such as hello@ and sales@ and phone numbers published on the site, with where each was found.
- **Email and DNS intelligence**: email provider (Google Workspace, Microsoft 365...), sending services, SPF and DMARC policy, tools verified in DNS.
- **Clear failures**: websites that are offline or block access return HTTP 422 with the reason.

## Quick start
```bash
curl --request GET \
  --url 'https://company-enrichment-by-domain.p.rapidapi.com/company?domain=apify.com' \
  --header 'x-rapidapi-key: YOUR_RAPIDAPI_KEY' \
  --header 'x-rapidapi-host: company-enrichment-by-domain.p.rapidapi.com'
```

```python
import requests
r = requests.get("https://company-enrichment-by-domain.p.rapidapi.com/company",
                 params={"domain": "apify.com"},
                 headers={"x-rapidapi-key": "YOUR_RAPIDAPI_KEY",
                          "x-rapidapi-host": "company-enrichment-by-domain.p.rapidapi.com"})
print(r.json()["companyName"], r.json()["emails"])
```

## Parameters
| Name | In | Required | Example | Notes |
| --- | --- | --- | --- | --- |
| domain | query | yes | apify.com | A domain or full URL. One per request. |

## Example response (real, 14 Nov 2026; shortened)
```json
{
  "input": "apify.com",
  "domain": "apify.com",
  "url": "https://apify.com/",
  "statusCode": 200,
  "companyName": "Apify",
  "legalName": "Apify Technologies s.r.o.",
  "title": "Apify: Marketplace of ready-to-run tools for AI",
  "description": "Thousands of tools to automate your business. Get real-time web data, track competitors, generate leads, and integrate your apps and AI agents.",
  "language": "en",
  "logo": {
    "url": "https://apify.com/img/apify-logo/apify-symbol-200x200.svg",
    "source": "structured-data"
  },
  "favicon": "https://apify.com/favicon.ico",
  "copyrightYear": 2026,
  "socialProfiles": {
    "linkedin": "http://linkedin.com/company/apify",
    "x": "https://x.com/apify",
    "github": "https://github.com/apify",
    "discord": "https://discord.com/invite/jyEM2PRvMU",
    "tiktok": "https://www.tiktok.com/@apifytech",
    "g2": "https://www.g2.com/products/apify/reviews"
  },
  "emails": [
    {
      "email": "hello@apify.com",
      "type": "role",
      "onDomain": true,
      "source": "link"
    }
  ],
  "phones": [],
  "keyPages": {
    "contact": "https://apify.com/contact",
    "careers": "https://apify.com/jobs",
    "pricing": "https://apify.com/pricing"
  },
  "technologyCount": 18,
  "technologiesByCategory": {
    "Analytics": [
      "Leadfeeder",
      "Mixpanel"
    ],
    "CDN": [
      "Amazon Cloudfront",
      "Amazon S3"
    ],
    "JavaScript frameworks": [
      "React",
      "styled-components"
    ],
    "Marketing automation": [
      "HubSpot"
    ],
    "Search engines": [
      "Algolia"
    ],
    "PaaS": [
      "Amazon Web Services"
    ]
  },
  "emailProvider": "Google Workspace",
  "dnsProvider": "Amazon Route 53",
  "emailSendingServices": [
    "Google Workspace",
    "Mailgun",
    "Amazon SES",
    "HubSpot"
  ],
  "verifiedServices": [
    "Apple",
    "Google Search Console",
    "HubSpot",
    "Mixpanel",
    "OpenAI"
  ],
  "emailSecurity": {
    "spf": true,
    "dmarcPolicy": "reject"
  },
  "ssl": {
    "issuer": "Amazon",
    "validTo": "2027-01-16T23:59:59.000Z",
    "daysUntilExpiry": 63,
    "authorized": true
  },
  "analyzedAt": "2026-11-14T14:06:10.404Z",
  "durationMs": 2135,
  "error": null
}
```

## Response fields
| Field | What it is |
| --- | --- |
| companyName, legalName | From structured data, meta tags and the page |
| description, title, language | The site's own description, page title and language |
| logo, favicon | Logo URL with where it was found |
| socialProfiles | LinkedIn, X, Facebook, Instagram, YouTube, GitHub, TikTok and more |
| emails | Emails found, with type (role or personal), whether on the company domain, and source |
| phones | Phone numbers found on the site |
| keyPages | Contact, about, careers, pricing, privacy and terms pages |
| technologyCount, technologiesByCategory | Detected technologies grouped by category |
| emailProvider, dnsProvider, emailSendingServices, verifiedServices | From DNS records |
| emailSecurity | SPF present and DMARC policy |
| ssl | Certificate issuer, expiry and days left |
| error | null on success, otherwise the reason |

## Status codes
- **200**: profile returned
- **400**: missing or invalid domain (send one domain per request)
- **422**: the website could not be read (offline, blocked or returned an error); the body says why

## Typical uses
Lead enrichment in a CRM, sales prospecting by technology, KYB and onboarding checks, marketplace seller vetting, and AI agents that need to know who a company is.

## Notes
Responses usually take 2 to 8 seconds (the site is read live; up to 45 seconds for slow sites). Only public information is read. You are responsible for using contact details lawfully (for example under GDPR).
