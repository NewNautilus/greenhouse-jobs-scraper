[Greenhouse Jobs Scraper](https://apify.com/cryptosignals/greenhouse-jobs-scraper?fpr=data)

## Greenhouse ATS Jobs Scraper — Track Hiring at 10,000+ Companies

Pull every open job from any company's Greenhouse ATS board — title, location, department, posted date, full description, and apply URL — without scraping their flaky careers page or stitching together aggregator data. Hand it a list of company slugs (`airbnb`, `stripe`, `coinbase`, ...) and get back a clean, structured dataset.

---

## Why Scrape Greenhouse?

Greenhouse is the applicant-tracking system (ATS) behind the public `/careers` and `/jobs/*` pages of roughly **10,000+ mid-market and enterprise employers** — Airbnb, Stripe, DoorDash, Coinbase, Pinterest, Robinhood, and most well-funded YC and Series A-D startups. Their job boards live at `boards.greenhouse.io/<company>` or behind a custom subdomain that proxies to Greenhouse.

If you want to monitor **who is hiring for what, when, and where**, Greenhouse is one of the highest-signal sources on the internet. But getting that data into a database is harder than it looks:

- Each company customizes its careers page — different layouts, different filters, different department naming.
- Many embed the Greenhouse iframe inside their corporate site, which breaks naive scraping.
- Bulk job aggregators (Indeed, LinkedIn) re-host stale snapshots with missing fields and delayed updates.
- The official Greenhouse Job Board API requires per-company tokens and gives you no easy way to monitor hundreds of companies in one pass.

This actor handles all of it. Give it the company slugs you care about; get back fresh, structured job records every time it runs.

---

## What Data You Get

Each job record returns a structured object with:

- **Company** — the Greenhouse board slug (e.g. `stripe`, `airbnb`)
- **Title** — the job title (e.g. `Senior Backend Engineer`)
- **Location** — city/region/remote text exactly as the company posts it
- **Department** — internal department/team name (e.g. `Engineering`, `Sales — North America`)
- **URL** — direct link to the job application page
- **Description** — full plain-text job description (responsibilities, requirements, comp band when posted)
- **Posted date** — when the listing went live (when Greenhouse exposes it)
- **Job ID** — Greenhouse's internal identifier, useful for de-duping across runs

---

## Use Cases

**1. Recruiter intelligence and candidate sourcing**
Build a watchlist of target employers. Get a daily diff of new openings in your target roles — frontend, ML, sales, product — across hundreds of companies in one pull.

**2. Sales prospecting (find companies hiring for X)**
Looking for companies actively building data teams? Cybersecurity? AI infra? Filter the dataset by job title or department to surface in-market accounts with budget and intent. Pair it with revenue/funding data for tier-1 outbound lists.

**3. Competitive hiring analysis**
Track which roles a competitor is opening. New VP of Sales? Three open SRE roles? They're scaling. A whole new "Platform" department? They're rebuilding. Hiring is a leading indicator of strategy.

**4. Market and sector trend research**
Pull every company in a vertical and roll up by department, location, or seniority. Spot whether the AI agents space is hiring researchers or shipping engineers. Spot whether fintech is bringing back compliance hiring.

**5. Talent-market and compensation research**
For HR analytics teams: aggregate roles by city, function, and seniority over time to track local labor demand and benchmark openings.

**6. Investor due-diligence**
For VCs: monitor portfolio companies' hiring pace as a proxy for execution. Build comp tables across stage and sector from public job descriptions.

---

## How to Use

1. Open the actor on Apify: [apify.com/cryptosignals/greenhouse-jobs-scraper](https://apify.com/cryptosignals/greenhouse-jobs-scraper)
2. Click **Try for free** — no credit card required for small runs.
3. Paste the **Greenhouse company slugs** you want to monitor. The slug is the part after `boards.greenhouse.io/` in the company's careers URL. For example:

- `https://boards.greenhouse.io/airbnb` → slug `airbnb`
- `https://boards.greenhouse.io/stripe` → slug `stripe`
- `https://boards.greenhouse.io/coinbase` → slug `coinbase`
4. Set **Max jobs per company** (default 100, up to 5000).
5. Click **Start** and download results as **JSON**, **CSV**, **Excel**, or pull them programmatically via the Apify API.

> **Tip:** if a company's careers page lives at `careers.example.com/jobs/*` and you can't tell which Greenhouse slug it uses, view the page source — most embeds include the slug in an iframe `src` or a `boards.greenhouse.io/<slug>` link in the network tab.

---

## Input Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `companies` | array of strings | Yes | List of Greenhouse board slugs to scrape (e.g. `["airbnb", "stripe", "coinbase"]`). |
| `maxJobsPerCompany` | integer (1–5000) | Optional | Cap the number of jobs per company. Default 100. |

### Example input

```
{
  "companies": ["airbnb", "stripe", "coinbase", "pinterest", "robinhood"],
  "maxJobsPerCompany": 500
}
```

---

## Output Example

```
[
  {
    "company": "stripe",
    "id": "5012345",
    "title": "Senior Software Engineer, Payments",
    "location": "San Francisco, CA / Remote (US)",
    "department": "Engineering — Payments",
    "url": "https://boards.greenhouse.io/stripe/jobs/5012345",
    "postedDate": "2026-04-22",
    "description": "About Stripe\nStripe is a financial infrastructure platform... \n\nWhat you'll do\n- Design and ship payment systems handling billions of dollars...\n- Partner with product, design, and other engineering teams...\n\nWhat we're looking for\n- 5+ years of backend engineering experience\n- Strong track record building distributed systems..."
  },
  {
    "company": "airbnb",
    "id": "6678901",
    "title": "Staff Machine Learning Engineer, Trust",
    "location": "Seattle, WA",
    "department": "Trust",
    "url": "https://boards.greenhouse.io/airbnb/jobs/6678901",
    "postedDate": "2026-04-25",
    "description": "Airbnb was born in 2007...\n\nThe Difference You Will Make\n- Lead ML system design for fraud and risk models...\n- Mentor senior ICs and partner with product on trust strategy..."
  }
]
```

Datasets can be exported as JSON, CSV, XML, or Excel from the Apify console, or fetched programmatically via the dataset API.

---

## Calling the Actor Programmatically

### cURL

```
curl -X POST "https://api.apify.com/v2/acts/cryptosignals~greenhouse-jobs-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "companies": ["airbnb", "stripe", "coinbase"],
    "maxJobsPerCompany": 200
  }'
```

### Python (apify-client)

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_TOKEN")

run = client.actor("cryptosignals/greenhouse-jobs-scraper").call(run_input={
    "companies": ["airbnb", "stripe", "coinbase"],
    "maxJobsPerCompany": 200,
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item["company"], "—", item["title"], "—", item.get("location"))
```

### Node.js (apify-client)

```
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_APIFY_TOKEN' });

const run = await client.actor('cryptosignals/greenhouse-jobs-scraper').call({
    companies: ['airbnb', 'stripe', 'coinbase'],
    maxJobsPerCompany: 200,
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(`Pulled ${items.length} jobs`);
```

---

## Pricing

This actor uses **Pay-per-event** — you only pay for results scraped, not for compute time or failed runs.

- **Cost**: $0.005 per job result.
- **Free tier**: Apify's free plan includes $5/month of platform credits — enough to pull ~1,000 jobs to try it out.
- **Typical run**: 5 companies × 200 jobs = 1,000 jobs → about $5.
- **Daily monitoring of 50 companies**: about ~$1–$3 per day depending on how active their boards are.

[See full Apify pricing →](https://apify.com/pricing)

---

## FAQ

**Is scraping Greenhouse legal?**

Public Greenhouse job boards are designed to be indexed and shared — that's their entire purpose. Scraping publicly visible data is generally considered lawful in many jurisdictions. The U.S. Ninth Circuit's *hiQ v. LinkedIn* decision (2022) affirmed that scraping publicly accessible data does not violate the Computer Fraud and Abuse Act. This actor only collects data from publicly visible Greenhouse board pages — no logins, no paywalls, no private endpoints. Always consult your own legal counsel for your specific use case.

**How do I find a company's Greenhouse slug?**

Go to the company's careers page. If the URL contains `boards.greenhouse.io/<slug>`, that's it. If they have a custom careers subdomain (e.g. `careers.example.com`), open the page source or DevTools network tab and look for a `boards.greenhouse.io/<slug>` URL in an iframe or fetch call.

**What if a company isn't on Greenhouse?**

This actor only works for Greenhouse boards. For Lever, Workable, Ashby, or LinkedIn job listings, see our other actors — including the [LinkedIn Jobs Scraper](https://apify.com/cryptosignals/linkedin-jobs-scraper).

**Can I schedule this actor?**

Yes. Use Apify's built-in scheduler to run on a cron — daily, weekly, hourly. Push results to a webhook, Google Sheets, Airtable, Slack, or your own database. A common pattern: schedule daily, dedupe by `id`, and alert on new openings matching a keyword filter ("VP", "Head of Data", "Solutions Engineer").

**Will I see only US jobs?**

No. Greenhouse boards include all jobs the company chooses to publish — including EMEA, APAC, and remote roles. The `location` field returns whatever the company posted (e.g. `"London, UK"`, `"Remote — Worldwide"`, `"Berlin, Germany"`).

**What if the actor returns fewer jobs than I expect?**

Some companies hide certain departments or split internal/external boards. The actor returns what is publicly visible on `boards.greenhouse.io/<slug>`. If you suspect a missing field or a broken run, open an issue on the actor page and we'll investigate.

---

## Related Actors

Looking for more job-market or company data?

- **[LinkedIn Jobs Scraper](https://apify.com/cryptosignals/linkedin-jobs-scraper)** — Job listings from LinkedIn at scale, with company filters and seniority.
- **[Crunchbase Scraper](https://apify.com/cryptosignals/crunchbase-scraper)** — Funding rounds, investors, founders, and company profiles.
- **[G2 Reviews Scraper](https://apify.com/cryptosignals)** — Software reviews, pricing, and competitor mentions.
- **[Capterra Reviews Scraper](https://apify.com/cryptosignals)** — Software category reviews and ratings.

---

## About Web Data Labs

This actor is maintained by [Web Data Labs](https://web-data-labs.com) — we publish a catalog of 100+ production-ready scrapers on the Apify platform covering jobs, e-commerce, social media, software reviews, and company data. Pay-per-result pricing means you only ever pay for data you receive.

Questions or custom data needs? Reach out via the Apify contact form or visit [web-data-labs.com](https://web-data-labs.com).

---

## New to Apify? Start here

Sign up for Apify through [this link](https://apify.com/?fpr=yw6md3) to get $5 in free platform credits — enough to try this actor and many others on the Web Data Labs catalog at no cost.