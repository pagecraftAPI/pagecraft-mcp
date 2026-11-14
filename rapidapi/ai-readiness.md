# AI Readiness and llms.txt Audit

How visible is a website to ChatGPT, Claude, Perplexity and Gemini? Send a domain, get a 0 to 100 AI readiness score with the checks behind it and the fixes that matter most.

## What it checks
- **llms.txt and llms-full.txt**: present or not, size and a preview
- **AI crawler access**: for 14 AI bots (GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, Claude-SearchBot, Claude-User, PerplexityBot, Perplexity-User, Google-Extended, Applebot-Extended, CCBot, Meta-ExternalAgent, Bytespider, Amazonbot), allowed or blocked and the exact robots.txt rule that decides it
- **Structured data**: JSON-LD present, Organization schema, all schema.org types found
- **Readable without JavaScript**: whether AI crawlers that do not run JavaScript can read the content
- **Machine interfaces**: OpenAPI, MCP server and AI plugin manifests
- **Basics**: sitemap.xml, meta description, HTTPS

Each check has a weight, so the score shows what to fix first. Grades run from A (85+) to E.

## Quick start
```bash
curl --request GET \
  --url 'https://ai-readiness-and-llms-txt-audit.p.rapidapi.com/ai-readiness?domain=stripe.com' \
  --header 'x-rapidapi-key: YOUR_RAPIDAPI_KEY' \
  --header 'x-rapidapi-host: ai-readiness-and-llms-txt-audit.p.rapidapi.com'
```

```javascript
const res = await fetch("https://ai-readiness-and-llms-txt-audit.p.rapidapi.com/ai-readiness?domain=stripe.com", {
  headers: { "x-rapidapi-key": "YOUR_RAPIDAPI_KEY", "x-rapidapi-host": "ai-readiness-and-llms-txt-audit.p.rapidapi.com" }
});
const audit = await res.json();
console.log(audit.score, audit.grade, audit.recommendations);
```

## Parameters
| Name | In | Required | Example | Notes |
| --- | --- | --- | --- | --- |
| domain | query | yes | stripe.com | A domain or full URL. One per request. |

## Example response (real, 14 Nov 2026; 5 of the 14 crawlers shown)
```json
{
  "input": "stripe.com",
  "domain": "stripe.com",
  "url": "https://stripe.com/",
  "statusCode": 200,
  "score": 85,
  "grade": "A",
  "checks": [
    {
      "id": "llms-txt",
      "label": "llms.txt present",
      "pass": true,
      "weight": 15
    },
    {
      "id": "llms-full",
      "label": "llms-full.txt present",
      "pass": false,
      "weight": 5
    },
    {
      "id": "ai-search-access",
      "label": "AI search and assistant bots can read the site",
      "pass": true,
      "weight": 20
    },
    {
      "id": "structured-data",
      "label": "Structured data (JSON-LD)",
      "pass": true,
      "weight": 15
    },
    {
      "id": "organization-schema",
      "label": "Organization schema",
      "pass": true,
      "weight": 5
    },
    {
      "id": "meta-description",
      "label": "Meta description",
      "pass": true,
      "weight": 5
    },
    {
      "id": "server-rendered-text",
      "label": "Content readable without JavaScript",
      "pass": true,
      "weight": 15
    },
    {
      "id": "sitemap",
      "label": "sitemap.xml present",
      "pass": true,
      "weight": 5
    },
    {
      "id": "machine-interface",
      "label": "API, MCP or plugin manifest discoverable",
      "pass": false,
      "weight": 10
    },
    {
      "id": "https",
      "label": "Served over HTTPS",
      "pass": true,
      "weight": 5
    }
  ],
  "recommendations": [
    "If you have an API, publish /openapi.json or an MCP server manifest so agents can act on your service.",
    "Optionally publish /llms-full.txt with your docs or key content in one file."
  ],
  "llmsTxt": {
    "url": "https://stripe.com/llms.txt",
    "bytes": 69770,
    "preview": "# Stripe\n\n> Stripe is a technology company that provides financial infrastructure for businesses."
  },
  "llmsFullTxt": null,
  "aiCrawlers": [
    {
      "agent": "GPTBot",
      "owner": "OpenAI (training)",
      "allowed": true,
      "rule": "default (*) allows"
    },
    {
      "agent": "OAI-SearchBot",
      "owner": "OpenAI (ChatGPT search)",
      "allowed": true,
      "rule": "default (*) allows"
    },
    {
      "agent": "ClaudeBot",
      "owner": "Anthropic (training)",
      "allowed": true,
      "rule": "default (*) allows"
    },
    {
      "agent": "PerplexityBot",
      "owner": "Perplexity (search)",
      "allowed": true,
      "rule": "default (*) allows"
    },
    {
      "agent": "Google-Extended",
      "owner": "Google (Gemini training)",
      "allowed": true,
      "rule": "default (*) allows"
    }
  ],
  "structuredDataTypes": [
    "WebSite",
    "Organization",
    "Person",
    "ImageObject",
    "ContactPoint",
    "Place",
    "PostalAddress"
  ],
  "machineInterfaces": {
    "openapi": false,
    "mcpManifest": false,
    "aiPluginManifest": false
  },
  "analyzedAt": "2026-11-14T14:07:10.459Z",
  "durationMs": 407,
  "error": null
}
```

## Status codes
- **200**: audit returned
- **400**: missing or invalid domain (send one domain per request)
- **422**: the website could not be read; the body says why

## Typical uses
GEO and SEO agencies auditing client sites, AI visibility dashboards, website builders and CMS plugins, and lead generation for agencies (find sites that block AI search).

## Notes
Responses usually take 1 to 5 seconds. Only public files are read (home page, robots.txt, llms.txt, sitemap.xml and well-known manifests).
