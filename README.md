[Greenhouse Jobs Scraper](https://apify.com/vnx0/greenhouse-jobs-scraper?fpr=data)

# Greenhouse Job Board Scraper — Extract Jobs, Salaries & Hiring Data

Extract job openings, salaries, descriptions, and hiring data from any company's career board. This Apify actor scrapes job titles, locations, departments, compensation, metadata, and application URLs from the Greenhouse applicant tracking system (ATS) used by over 8,000 companies including Stripe, Coinbase, Airbnb, Uber, and Robinhood. The only Greenhouse job board scraper you need for recruitment analytics, competitive intelligence, and job aggregation.

## Overview

Greenhouse is one of the most popular ATS platforms, powering careers pages for thousands of companies worldwide. This actor provides a fast, reliable API alternative to manually browsing career pages, extracting all publicly available hiring data in structured JSON format. Perfect for recruitment analytics, competitive intelligence, job aggregation, and market research.

## ✨ Features

| Feature | Description |
| --- | --- |
| 🔍 **Multi-board scraping** | Extract jobs from any company's Greenhouse career board |
| 🏢 **8,000+ companies** | Works with Stripe, Coinbase, Airbnb, Uber, Robinhood, and thousands more |
| 🚀 **No authentication needed** | Scrape public job boards instantly — no API keys or login required |
| 📄 **Full job descriptions** | Get complete HTML and plain-text versions of every job listing |
| 📍 **Structured locations** | Parsed location data with city, state, and country where available |
| 🏷️ **Smart metadata extraction** | Automatically extracts employment type, experience level, remote status, and more |
| 🔗 **Direct apply links** | Get direct application URLs for every job |
| 🎯 **Department & location filters** | Only scrape jobs matching specific departments or locations |
| 🌐 **Custom domain support** | Handles companies using their own career domains (e.g., careers.company.com) |
| 📊 **Paginated results** | Automatically handles pagination for boards with hundreds of open roles |
| 📋 **Structured JSON output** | Clean, consistent dataset schema ready for analysis |
| ⚡ **High performance** | Extracts data efficiently with automatic retries and rate limiting |

## Use Cases

- **Scrape competitor job openings** — Track what roles your competitors are hiring for
- **Job aggregation platforms** — Build job boards by aggregating listings from multiple companies
- **Recruitment market research** — Analyze hiring trends across industries, locations, and departments
- **Salary data collection** — Extract compensation information when companies include it
- **Department growth tracking** — Monitor which teams are expanding at target companies
- **Remote job discovery** — Filter for remote positions across thousands of companies
- **Talent pipeline building** — Identify companies hiring for specific skill sets

## How It Works

1. **Enter board URLs or company names** — Provide career board URLs or just type company names
2. **Actor fetches all jobs** — Extracts all job listings with automatic pagination
3. **Results in structured JSON** — Get clean, structured data in the Apify dataset, ready for export to CSV, JSON, Excel, or your database

## Input

| Field | Type | Description | Required | Default |
| --- | --- | --- | --- | --- |
| `startUrls` | array | List of career board URLs to scrape | No | `[]` |
| `companyNames` | array | Company names to scrape jobs from | No | `[]` |
| `maxJobsPerBoard` | integer | Maximum jobs to extract per board (0 = unlimited) | No | `0` |
| `includeFullDescription` | boolean | Include full HTML job descriptions | No | `true` |
| `filterDepartments` | array | Only include jobs from these departments | No | `[]` |
| `filterLocations` | array | Only include jobs matching these locations | No | `[]` |
| `proxyConfiguration` | object | Apify proxy configuration | No | `{"useApifyProxy": true}` |

**Note:** Provide at least one of `startUrls` or `companyNames`.

### Supported URL Formats

- `https://boards.greenhouse.io/{company}`
- `https://boards.greenhouse.io/{company}/jobs`
- Custom career domains (e.g., `https://careers.company.com`)

## Output

Each job listing includes:

| Field | Type | Description |
| --- | --- | --- |
| `id` | integer | Internal job listing ID |
| `title` | string | Job title |
| `company` | string | Company name |
| `url` | string | Direct link to job listing |
| `applicationUrl` | string | Direct application link |
| `location` | object | Primary job location |
| `departments` | array | Departments (e.g., ["Engineering", "Product"]) |
| `offices` | array | Office locations |
| `description` | string | Full job description (HTML) |
| `descriptionText` | string | Plain text description |
| `requisitionId` | string | Company requisition ID |
| `internalJobId` | string | Company's internal job reference ID |
| `employmentType` | string | Full-time, Part-time, Contract, etc. |
| `experienceLevel` | string | Entry, Mid, Senior, Lead, etc. |
| `remote` | boolean | Whether position is remote |
| `metadata` | object | All custom company metadata fields |
| `updatedAt` | string | Last updated timestamp (ISO 8601) |
| `scrapedAt` | string | When job was scraped (ISO 8601) |

## Sample Output

```
{
  "id": 7532733,
  "title": "Account Executive, AI Sales",
  "company": "Stripe",
  "url": "https://stripe.com/jobs/search?gh_jid=7532733",
  "applicationUrl": "https://stripe.com/jobs/search?gh_jid=7532733",
  "location": {
    "name": "San Francisco, CA"
  },
  "departments": ["1175 Enterprise - Account Executives (NA)"],
  "offices": [
    {
      "name": "US",
      "location": null
    }
  ],
  "description": "<h2>Who we are</h2><h3>About Stripe</h3><p>Stripe is a financial infrastructure platform for businesses...</p>",
  "descriptionText": "Who we are About Stripe Stripe is a financial infrastructure platform for businesses. Millions of companies - from the world's largest enterprises to the most ambitious startups - use Stripe to accept payments...",
  "requisitionId": "See Opening ID",
  "internalJobId": "3336216",
  "employmentType": null,
  "experienceLevel": null,
  "remote": null,
  "metadata": {},
  "updatedAt": "2026-04-16T11:05:52-04:00",
  "scrapedAt": "2026-04-19T13:55:14.100852+00:00"
}
```

## Cost & Usage

This actor uses event-based pricing:

- **$0.002 per job** ($2 per 1,000 jobs)
- Typical board has 20-200 jobs
- Example: Scraping 10 companies with 50 jobs each = 500 jobs = $1.00

Apify free tier includes $5 of free usage monthly, enough for ~2,500 jobs.

## Tips

- **Use company names for quick scraping** — Just enter `stripe` instead of the full URL
- **Check the dataset tab** — Results appear in real-time as jobs are scraped
- **Filter before scraping** — Use department/location filters to reduce costs
- **Combine with other scrapers** — Use alongside LinkedIn or Indeed scrapers for comprehensive coverage
- **Monitor regularly** — Set up scheduled runs to track new job postings
- **Export to your tools** — Connect results to your ATS, CRM, or analytics platform via Apify integrations

## Example Use Cases

### Track Competitor Hiring

```
{
  "companyNames": ["stripe", "square", "adyen", "checkout"],
  "filterDepartments": ["Engineering", "Product"]
}
```

### Find Remote Engineering Jobs

```
{
  "companyNames": ["gitlab", "zapier", "automattic", "doist"],
  "filterLocations": ["Remote"]
}
```

### Build a Job Board

```
{
  "startUrls": [
    "https://boards.greenhouse.io/stripe",
    "https://boards.greenhouse.io/coinbase",
    "https://boards.greenhouse.io/airbnb"
  ],
  "maxJobsPerBoard": 100
}
```

## Limitations

- Only works with companies using Greenhouse ATS
- Some companies may have private boards (not publicly accessible)
- Custom metadata fields vary by company
- Salary information only included if company publishes it

## Support

For issues or questions, please contact us through the Apify platform.

## Privacy & Compliance

This actor only extracts publicly available data from Greenhouse job boards. No authentication or private data access is used. Always comply with applicable laws and terms of service when using scraped data.