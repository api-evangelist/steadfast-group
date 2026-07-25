# Steadfast Group (steadfast-group)

Steadfast Group Limited (ASX:SDF) is the largest general insurance broker network and the largest group of insurance underwriting agencies in Australasia, headquartered in Sydney, Australia. It is a broker-intermediary rather than a risk carrier: the Steadfast Network comprises 414 independent brokerages placing approximately $12.7 billion in gross written premium, alongside 31 underwriting agencies writing roughly 100 products across business pack, liability, professional indemnity, cyber, construction, marine, aviation, farm, strata, motor and home and contents lines, plus complementary businesses covering premium funding (IQumulate), life insurance, workplace risk, legal and compliance.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/steadfast-group/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/steadfast-group/refs/heads/main/apis.yml)

## APIs

**None.** Steadfast Group publishes no public, self-serve API and no downloadable API specification.

This is the honest and expected outcome for a broker-intermediary in a market with no open-insurance mandate, and it is recorded here deliberately rather than filled in with an invented API family.

All 311 URLs in the public sitemap were fetched and searched on 2026-07-25. Zero pages reference a developer portal, a REST API, OpenAPI, Swagger, AL3, or Sunrise Exchange. Every candidate developer host and path was probed and its HTTP status recorded in [review.yml](review.yml).

### What actually exists

- **[broker.steadfast.com.au](https://broker.steadfast.com.au/)** — HTTP 200, redirects to `/login`, page title "Steadfast Broker Login". A credentialed portal for accredited Steadfast Network brokers. A **login wall, not a developer portal.**
- **api.steadfast.com.au** — a live IIS host on Azure that returns **HTTP 403 Forbidden** at the root and HTTP 404 on `/swagger`, `/swagger/v1/swagger.json`, `/api`, `/v1`, `/docs`, `/health` and `/index.html`. Undocumented private/partner infrastructure with no public reference material.
- `developer.`, `developers.` and `docs.steadfast.com.au` do not resolve in DNS.

### Trading technology

The **Steadfast Client Trading Platform (SCTP)**, launched in 2009, lets network brokers send one question set to a panel of insurers for instant comparative quotes. It transacted over **$1.5 billion in GWP in CY25** across 9 insurer lines and 23 connected partners. SCTP and the **INSIGHT** cloud policy management platform are being consolidated into **Steadfast Apps**, an end-to-end client, policy and claims broking platform. The public announcement describes "an innovative configuration engine which removes the requirement for lengthy and costly development" — configuration, explicitly not a published API. Insurer and partner connectivity is arranged commercially, not through self-serve onboarding.

Quote, bind, issue and FNOL all happen inside these systems, and all four are **partner-only** — behind the broker login, with no public interface for any of them.

## ACORD posture

**ACORD board leadership, no published ACORD implementation.**

The sole ACORD reference across the entire public site is a biography on the [board and management page](https://www.steadfast.com.au/about-us/board-and-management/): founder, Managing Director and CEO **Robert B. Kelly AM** is "the Chair of the ACORD Board in New York." No ACORD XML, AL3, or NGDS technical detail is published anywhere. Governance-level involvement in the standards body, zero public implementation surface — a gap worth naming.

Ebix Australia (ebix.com.au), operator of the **Sunrise Exchange**, was probed to test an attribution and still self-describes as "part of Ebix Limited" with no Steadfast branding. Sunrise Exchange is therefore **not** attributed to Steadfast Group in this record.

## Market context

Australia has the legal machinery for open insurance and no live obligation. APRA handles prudential supervision, and the Consumer Data Right — which already opened banking and energy — was designated to extend to general insurance, then deferred and de-prioritized. The CDR seam that made Australian banking legible simply stops before insurance. There is no forcing function that would make a broker network of this scale publish a public API, and Steadfast has not published one voluntarily. Its integration value sits in the closed SCTP insurer panel: a commercial network effect rather than an open interface.

## Auth, webhooks, and other surfaces

| Surface | Status |
| --- | --- |
| Self-serve signup | None |
| API keys / OAuth2 client registration | None published |
| `/.well-known/openid-configuration` | 404 on broker, 503 on api |
| Webhooks / event catalog / AsyncAPI | None found |
| GraphQL | `/graphql` → 404 |
| gRPC / `.proto` | None found |
| Postman public workspace | 0 results for "steadfast insurance" |
| Official GitHub organization | None attributable |
| `llms.txt` / `security.txt` | 404 |

## Tags

- Insurance
- Australia
- Broker
- Insurance Broker Network
- General Insurance
- Property and Casualty
- Underwriting Agency
- Agency Management
- ACORD
- Partner Gated
- New Zealand

## Links

- [Website](https://www.steadfast.com.au/)
- [About Steadfast](https://www.steadfast.com.au/about-us/)
- [Board and Management](https://www.steadfast.com.au/about-us/board-and-management/)
- [Investor Centre](https://investor.steadfast.com.au/investor-centre/)
- [Steadfast Underwriting Agencies](https://steadfastagencies.com.au/)
- [Steadfast Life](https://www.steadfastlife.com.au/)
- [Steadfast New Zealand](https://www.steadfastnz.nz/)
- [Steadfast Singapore](https://www.steadfast.com.sg/)
- [Well Covered (blog)](https://www.steadfast.com.au/well-covered/)
- [LinkedIn](https://www.linkedin.com/company/steadfast-group-limited/)

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25

## Review

See [review.yml](review.yml) for the full reviewer finding, every probed URL with its HTTP status, the ACORD evidence quoted verbatim, and the transport inventory.
