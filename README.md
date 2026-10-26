# Steadfast Group (steadfast-group)

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Steadfast Group Limited (ASX:SDF) is the largest general insurance broker network and the largest group of insurance underwriting agencies in Australasia, headquartered in Sydney, Australia. It is a broker-intermediary rather than a risk carrier: the Steadfast Network comprises 414 independent brokerages placing approximately $12.7 billion in gross written premium, alongside 31 underwriting agencies writing roughly 100 products across business pack, liability, professional indemnity, cyber, construction, marine, aviation, farm, strata, motor and home and contents lines, plus complementary businesses covering premium funding (IQumulate), life insurance, workplace risk, legal and compliance.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/steadfast-group/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/steadfast-group/refs/heads/main/apis.yml)

## APIs

**Two — neither documented, neither announced.** Steadfast Group still publishes no developer portal, no API documentation and no specification of any kind. The first-round review recorded that correctly. A second enrichment round on 2026-07-25 went past the marketing site and found two genuinely machine-readable surfaces the estate never mentions.

### 1. Flood Risk Tracker API — public, anonymous, undocumented

The consumer [Flood Risk Tracker](https://floodrisktracker.steadfast.com.au/) is not just a map. Its own client JavaScript (`/Scripts/FloodRiskTracker/custom.js`) calls two JSON endpoints that answer anonymous requests from anywhere:

| Operation | Path | What it does |
| --- | --- | --- |
| `findAddress` | `GET /api/risk/find_address?searchText=` | Resolves free text to Australian addresses with their **G-NAF** national identifiers |
| `getFloodRisk` | `GET /api/risk/get_flood_risk?addressId=` | Returns **Swiss Re** river-flood and coastal storm-surge risk layers for that address |

No API key, no token, no cookie. ASP.NET Core behind Cloudflare, advertising `api-supported-versions: 1.0`, returning **RFC 9457 `application/problem+json`** on validation failure with a W3C Trace Context `traceId`. Australia-only; a New Zealand address returns an empty array.

There is no Steadfast specification for it, so one was **derived** from the client JavaScript and from live probes — every path, field, status code and example in [`openapi/`](openapi/steadfast-group-flood-risk-tracker-openapi.yml) was observed in a real response, and the verbatim exchanges are in [`examples/`](examples/steadfast-group-flood-risk-tracker-examples.yml). It is unsupported, uncommitted, and can change or close without notice. Treat it as observational intelligence, not as an interface Steadfast offers.

### 2. Steadfast Identity — an Okta OIDC provider with public discovery

`idp.steadfast.com.au` publishes a complete, anonymously readable **OpenID Connect discovery document**: authorization, token, userinfo, JWKS, introspection, revocation, device-authorization, dynamic-client-registration and logout endpoints, seven scopes, **PKCE S256** and **DPoP** proof-of-possession. The first round probed `broker.` and `api.` for `/.well-known/openid-configuration` and got 404/503; the IdP is on a third host, surfaced here through certificate transparency.

Discovery is open, access is not — anonymous client registration returns 403. The ROPC `password` grant and `implicit` response types are still enabled, both removed by OAuth 2.1. Captured verbatim in [`well-known/`](well-known/), profiled in [`authentication/`](authentication/steadfast-group-authentication.yml) and [`scopes/`](scopes/steadfast-group-scopes.yml).

### The rest is still shut

All 311 URLs in the public sitemap were fetched and searched on 2026-07-25. Zero pages reference a developer portal, a REST API, OpenAPI, Swagger, AL3, or Sunrise Exchange. Every candidate developer host and path was probed and its HTTP status recorded in [review.yml](review.yml). Certificate transparency additionally exposed `api-sf` and `api-sf-uat` — a production/UAT partner API pair — both closed to anonymous requests.

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
| API keys | None published; the public API requires no credential at all |
| OAuth2 / OIDC | **Live** — `idp.steadfast.com.au`, discovery public, registration 403 |
| `/.well-known/openid-configuration` | **200 on idp**; 404 on broker, 503 on api |
| Webhooks / event catalog / AsyncAPI | None found |
| GraphQL | `/graphql` → 404 on every host |
| gRPC / `.proto` | None found |
| Postman public workspace | 0 results for "steadfast insurance" |
| Official GitHub organization | None attributable |
| SDKs / client libraries | None — every "steadfast" package belongs to Steadfast Courier (Bangladesh) |
| Status page / changelog / roadmap / SLA | None |
| `llms.txt` / `security.txt` | 404 everywhere in the estate |
| Bug bounty / VDP / trust centre | None found |

## Artifacts in this repository

`openapi/` derived spec · `examples/` verbatim live exchanges · `overlays/` catalog annotations · `well-known/` harvested OIDC + OAuth metadata and every probe status · `authentication/` · `scopes/` · `conventions/` observed semantics and data quirks · `errors/` RFC 9457 catalogue · `data-model/` entity graph · `conformance/` standards posture including the ACORD gap · `lifecycle/` versioning and everything absent · `security/` TLS/HSTS/DNSSEC/CAA/SPF/DMARC posture · `mcp/` candidate tools and crosswalk · `skills/` agent skill · `agentic-access/` · `packages/` negative finding and homonym traps · `llms/`

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
