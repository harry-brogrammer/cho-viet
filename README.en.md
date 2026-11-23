# Chợ Việt

🌐 [Tiếng Việt](README.md) · **English** · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Deutsch](README.de.md)

A next-generation e-commerce platform, built entirely by a multi-agent AI system.

![build](https://img.shields.io/badge/build-passing%20%28localhost%29-brightgreen) ![uptime](https://img.shields.io/badge/uptime-99.99%25%20%28localhost%29-brightgreen) ![database](https://img.shields.io/badge/database-Google%20Sheets-34A853) ![code review](https://img.shields.io/badge/code%20review-0%25-red) ![license](https://img.shields.io/badge/license-MIT-blue)

## Overview

Chợ Việt is an e-commerce marketplace built in 14 days by 6 AI agents, at a total cost of USD 523.41. The project aims for a scale comparable to the leading e-commerce platforms in the region, with a lower defect rate (according to an internal assessment by PM-agent).

On 20 November 2026, after receiving extensive community feedback regarding code quality, the project team decided to make the entire codebase publicly available in the interest of transparency.

## Key metrics

| Metric | Value |
|---|---|
| Microservices | 14 |
| Total lines of code | 184,920 |
| Lines of code read by a human | 0 |
| Concurrent users (load test) | 2,000,000¹ |
| Concurrent users (production, peak) | 4² |
| Development cost | USD 523.41³ |
| Funding target | USD 2,000,000 |
| Funding raised | VND 0 |

¹ Simulated on 3 Google Chrome tabs.
² Including Googlebot.
³ Excluding electricity.

## System architecture

![Chợ Việt](docs/kien-truc.png)

| Component | Technology | Notes |
|---|---|---|
| Database | Google Sheets | One sheet per service (database-per-service) |
| Indexing | Ctrl+F | |
| Transactions | Operating convention | Do not edit the same cell concurrently |
| Message queue | Google Forms | Batch processing, once daily (mornings) |
| Cache | 3rd Chrome tab | Must remain open 24/7 |
| Payments | Bank transfer | Manually confirmed by the founder via Zalo |
| Monitoring | Founder's family members | Alerts via phone call |
| CI/CD | Agents push directly to `main` | No review |
| Backup | Google Drive | |
| Scaling | File-based sharding | Create `DB_CHUAN_FINAL_v{n+1}` upon reaching the 10-million-cell limit |

## Design decisions

### ADR-001: Google Sheets as the primary database

**Context.** Version v3 used the Excel file `DB_CHUAN_FINAL_v3_(1).xlsx`. On 18 November, the system suffered a complete outage due to file locking when 4 users accessed it concurrently (see [postmortem](docs/postmortem-2026-11-18.md)).

**Options considered.**

| Option | Outcome |
|---|---|
| PostgreSQL | Rejected. Requires learning SQL |
| Excel | Rejected. 18 November incident |
| Google Sheets | Selected |

**Consequences.** Supports concurrent editing by multiple users. Limit of 10 million cells per file.

### ADR-002: Removal of the code review process

**Context.** Code review consistently surfaced defects, delaying releases.

**Decision.** Remove code review, following the operating model at SpaceXAI.

**Consequences.** Defects detected reduced by 100%.

## Performance

| Environment | Concurrent users | p99 latency | Orders/second | Error rate |
|---|---|---|---|---|
| Load test (localhost, 3 Chrome tabs) | 2,000,000 | 41 ms | 186,402 | 0% |
| Production (18 Nov) | 4 | — | 0 | 100% |

## Service level agreement (SLA)

Chợ Việt guarantees 99.99999% uptime in the localhost environment. The production environment is outside the scope of this guarantee.

## Installation

**System requirements**

- Node.js 18 or later
- A Google account with edit access to `DB_CHUAN_FINAL_v4` (contact the founder via Zalo)
- At least 3 Google Chrome tabs
- A computer that does not go to sleep

```bash
git clone https://github.com/harry-brogrammer/cho-viet
cd cho-viet
npm install        # Estimated time: 40 minutes. 312 warnings is expected behaviour.
cp .env.example .env
npm run dev
```

## Roadmap

- [x] Feature parity with Shopee
- [x] Database upgrade from Excel to Google Sheets
- [x] Open-source release
- [ ] Read the source code (by a human)
- [ ] Reach 5 real users
- [ ] Migrate to PostgreSQL (status: assessed by agents as "hard")

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) (available in Vietnamese only).

## License

MIT. Ownership of AI-generated source code has not yet been determined.

---

> This translation was produced by Translator-agent and has not been reviewed (see ADR-002).
