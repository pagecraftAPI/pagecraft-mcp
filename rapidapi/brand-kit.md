# Brand Kit by Domain

Send a domain, get the company's brand kit: logo, icons, primary colour, ranked brand colours, fonts. Read live from the website and its own stylesheets, so it works for any site, not only big brands in a database.

## What you get
- **Logo** with where it was found (structured data, header, icons), plus **favicon**, **Apple touch icon** and **OG image**
- **Primary colour**: the main brand colour (a white or black theme-color does not win over real brand colours)
- **Brand colours**: up to 8, ranked, each with its source (`brand-variable` = a CSS variable named like primary/brand/accent, `theme-color`, `stylesheet`)
- **Neutral colours**: up to 5 greys, whites and near-blacks used for layout
- **Fonts**: the first real font of each font stack, plus Google Fonts the page loads
- **Company name and description**

Typical uses: white-label onboarding (theme your app in a customer's colours), sales and CRM enrichment, design tools, pitch decks and mock-ups.

## Quick start
```bash
curl --request GET \
  --url 'https://brand-kit-by-domain.p.rapidapi.com/brand?domain=stripe.com' \
  --header 'x-rapidapi-key: YOUR_RAPIDAPI_KEY' \
  --header 'x-rapidapi-host: brand-kit-by-domain.p.rapidapi.com'
```

```python
import requests
r = requests.get("https://brand-kit-by-domain.p.rapidapi.com/brand", params={"domain": "stripe.com"},
                 headers={"x-rapidapi-key": "YOUR_RAPIDAPI_KEY", "x-rapidapi-host": "brand-kit-by-domain.p.rapidapi.com"})
kit = r.json()
print(kit["primaryColour"], kit["logo"]["url"], kit["fonts"])
```

```javascript
const res = await fetch("https://brand-kit-by-domain.p.rapidapi.com/brand?domain=stripe.com", {
  headers: { "x-rapidapi-key": "YOUR_RAPIDAPI_KEY", "x-rapidapi-host": "brand-kit-by-domain.p.rapidapi.com" }
});
const kit = await res.json();
console.log(kit.primaryColour, kit.brandColours.map(c => c.hex));
```

## Parameters
| Name | In | Required | Example | Notes |
| --- | --- | --- | --- | --- |
| domain | query | yes | stripe.com | A domain or full URL. One per request. |

## Example response (real, 14 Nov 2026)
```json
{
  "input": "stripe.com",
  "domain": "stripe.com",
  "url": "https://stripe.com/",
  "statusCode": 200,
  "blocked": false,
  "companyName": "Stripe",
  "logo": {
    "url": "https://images.stripeassets.com/fzn2n1nzq965/1hgcBNd12BfT9VLgbId7By/01d91920114b124fb4cf6d448f9f06eb/favicon.svg",
    "source": "structured-data"
  },
  "favicon": "https://images.stripeassets.com/fzn2n1nzq965/1hgcBNd12BfT9VLgbId7By/01d91920114b124fb4cf6d448f9f06eb/favicon.svg",
  "appleTouchIcon": "https://images.stripeassets.com/fzn2n1nzq965/4vVgZi0ZMoEzOhkcv7EVwK/8cce6fdcf2733b2ec8e99548908847ed/favicon.png",
  "ogImage": "https://b.stripecdn.com/mkt-ssr-statics/assets/_next/static/media/stripe.37119086.jpg",
  "primaryColour": "#533afd",
  "brandColours": [
    {
      "hex": "#533afd",
      "count": 83,
      "source": "brand-variable"
    },
    {
      "hex": "#1c1e54",
      "count": 52,
      "source": "brand-variable"
    },
    {
      "hex": "#ff9014",
      "count": 52,
      "source": "brand-variable"
    },
    {
      "hex": "#ffaf2d",
      "count": 52,
      "source": "brand-variable"
    },
    {
      "hex": "#ffd552",
      "count": 52,
      "source": "brand-variable"
    }
  ],
  "neutralColours": [
    {
      "hex": "#f5f5ff",
      "count": 26,
      "source": "brand-variable"
    },
    {
      "hex": "#e8e9ff",
      "count": 26,
      "source": "brand-variable"
    },
    {
      "hex": "#f6f7ff",
      "count": 26,
      "source": "brand-variable"
    }
  ],
  "fonts": [
    "sohne-var",
    "SourceCodePro",
    "Segoe UI"
  ],
  "googleFonts": [],
  "description": "Stripe is a financial services platform that helps all types of businesses accept payments, build flexible billing models, and manage money movement.",
  "analyzedAt": "2026-11-14T15:30:14.875Z",
  "durationMs": 417,
  "error": null
}
```

Other real results from the same day: bbc.co.uk primary `#0051ad`, fonts ReithSans and ReithSerif; apify.com primary `#246dff`, fonts Inter and GT Walsheim.

## Errors
| Status | Meaning |
| --- | --- |
| 400 | Missing or invalid domain, or more than one domain in a request |
| 422 | The website could not be read (blocked automated access, HTTP error, timeout); the body says why |
| 503 | Busy, retry in a few seconds |

## Notes
- Colours come from the site's own CSS and meta tags. Sites that draw their brand only in images may return few brand colours; the logo and icons still come back.
- Most requests finish in 1 to 5 seconds; the limit is 45 seconds.
- Only public pages are read. No logins, no personal data.
