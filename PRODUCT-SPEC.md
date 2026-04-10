# PolicyPilot - Technical Product Specification

## Product Overview
AI-powered renewal tracking, cross-sell identification, automated quoting, and commission reconciliation for independent insurance agencies. Replaces manual spreadsheet tracking, reduces missed renewals by 67%, and surfaces $64K+ in cross-sell opportunities per agency annually.

## Architecture

### System Components
1. **Policy Ingestion Engine** - AMS sync + CSV import + manual entry
2. **Renewal Monitor** - 24/7 policy date tracking with 90/60/30-day alerting
3. **Cross-Sell Engine** - Coverage gap analysis across client portfolio
4. **Quoting Engine** - Multi-carrier rate comparison via API + programmatic access
5. **Communication Automation** - Client outreach (email + SMS) for renewals and cross-sells
6. **Commission Tracker** - Expected vs. received commission reconciliation per carrier
7. **Dashboard & Reporting** - Portfolio health, producer performance, revenue attribution
8. **Client Portal** - (Enterprise) White-label client-facing policy status view

### Tech Stack
- **Backend:** Python (FastAPI) + PostgreSQL + Redis
- **Frontend:** Next.js 14 + Tailwind CSS + shadcn/ui
- **AI/ML:** OpenAI GPT-4o for document parsing, custom models for cross-sell scoring
- **Infrastructure:** AWS (ECS Fargate + RDS + ElastiCache + S3)
- **Integrations:** Applied Epic API, HawkSoft API, EZLynx Connect, AMS360 API
- **Communication:** Twilio (SMS), SendGrid (email), Plivo (voice - future)
- **Auth:** Clerk (multi-tenant, per-agency isolation)
- **Monitoring:** Datadog + PagerDuty
- **CI/CD:** GitHub Actions, Terraform for infra

### AMS Integrations
Applied Epic: REST API, real-time webhook
HawkSoft: API v3, 15-min polling
EZLynx: EZLynx Connect, real-time
AMS360: REST API, 30-min polling
QQCatalyst: API, 30-min polling
NowCerts: API, 30-min polling
CSV Import: manual upload, on-demand

## Feature Specifications

### Renewal Monitor
- Ingest all policy expiration dates from AMS
- Generate alerts at configurable windows (default: 90/60/30 days)
- Auto-assign to producer based on policy ownership
- Client communication: email at 90 days, email + SMS at 60 days, SMS + call alert at 30 days
- Track client response: acknowledged, quote requested, renewed, lapsed
- Dashboard: renewal pipeline view (upcoming 30/60/90 days)
- Escalation: if no client response by 30 days, flag producer + agency owner

### Cross-Sell Engine
- Scan client portfolio for coverage gaps using rule-based + ML scoring
- Rules: auto without umbrella, commercial without cyber, homeowner without flood (zone-aware), business without E&O/D&O
- ML scoring: predict cross-sell likelihood based on client demographics, policy history, comparable client profiles
- Surface opportunities ranked by: estimated premium x close probability
- One-click outreach: pre-drafted email/SMS to client with coverage recommendation
- Attribution: track which cross-sell suggestions convert to bound policies

### Quoting Engine (Growth + Enterprise)
- Unified data entry: single form, maps to multiple carrier formats
- Carrier connections: API where available, browser automation where not
- Side-by-side comparison: premium, coverage, deductibles, carrier rating
- AI recommendation: highlights best value + best coverage options
- Proposal generation: PDF with agency branding, comparison table, recommendation
- Follow-up automation: if quote sent but not bound in 7 days, auto-follow-up

### Commission Reconciliation (Growth + Enterprise)
- Import commission statements (CSV, PDF parsing, carrier portal scraping)
- Match received commissions to expected (premium x rate per carrier schedule)
- Flag variances: missing commissions, incorrect amounts, late payments
- Producer split tracking: calculate per-producer earnings
- Monthly reconciliation report: by carrier, by producer, by line of business

## Security & Compliance
- SOC 2 Type II certified
- AES-256 encryption at rest, TLS 1.3 in transit
- Per-agency data isolation (separate database schemas)
- Role-based access control (owner, producer, CSR, read-only)
- Audit logging on all data access and modifications
- Insurance-specific: no binding authority (PolicyPilot generates quotes, humans bind)

## Infrastructure Costs (Estimated)
- AWS (ECS + RDS + Redis + S3): $800/month at 50 agencies
- Twilio (SMS + voice): $200/month at 50 agencies
- SendGrid (email): $100/month
- OpenAI (document parsing): $150/month
- Monitoring (Datadog): $200/month
- Total: ~$1,450/month at 50 agencies = $29/agency/month
- **Gross margin at $1,499/mo: 98.1%**

## Roadmap
- **V1 (Launch):** Renewal tracking, cross-sell identification, client communication, basic dashboard
- **V2 (Month 3):** Multi-carrier quoting, commission reconciliation, producer analytics
- **V3 (Month 6):** Client portal, carrier marketplace, voice outreach (AI calling for renewals)
- **V4 (Month 9):** Predictive churn modeling, market-rate benchmarking, M&A book valuation tool
