# TenkaSystem.com: Audit Report. Why It Is Not Ready to Be Online

**Site audited:** https://www.tenkasystem.com/ (apex https://tenkasystem.com redirects to www)
**Audit dates:** November 24-25, 2026. Checks run November 24, 22:48-22:56 EST / November 25, 03:48-03:56 UTC. Key blockers re-verified by the lead auditor after the four specialist audits.
**Auditor role:** Independent software systems and web architect audit. Four specialist passes: technical/security/infrastructure, content/claims/credibility, legal/compliance/trust, UX/SEO/design/mobile.
**Format:** Markdown, as requested.
**Standing rule applied:** No em dashes are used in this report. Where a quoted passage originally contained one, it is replaced with a comma or a hyphen, and noted where it matters.

## Verdict

**Take it offline and rebuild it. Do not patch it live.**

This is not a site with a few broken links. It is a generated facade over an exposed backend, selling a product its own legal pages say does not exist, with no working way to sign up, log in, pay, contact the company, or try a demo, and with legal documents that contradict each other on the most basic consumer rights.

The headers are fine. Everything behind them is not.

A visitor who reads only the marketing pages is told a governed AI workforce is "built, deployable, and running today", with cryptographic identity, signed accountability, and a chain of custody that "holds up to regulatory scrutiny". A visitor who reads the legal pages learns the workforce dialogue is scripted, there is no autonomous AI, the proprietary models do not exist yet, and users are the training corpus for building them someday. Those two statements cannot both be true, and the commercial claim is the one on the public pages.

That contradiction alone justifies taking the site offline. It is not the only one. This report lists them all, with evidence you can check yourself in under a minute per item.

## How to use this report

Each finding has a severity, the exact URL, and the exact evidence: a status code, a quoted sentence, a file hash, or a header. Where a finding could not be verified by the method used, it is marked as unverified, not asserted as fact. Section 7 lists what is actually sound, because an honest audit reports passes too. Section 8 gives the rebuild checklist and the relaunch gates. Nothing in Section 8 is optional if the site is to return as a governance product.

Severity key used throughout:

- **Blocker / Critical:** Take offline level. Blocks the site's purpose, creates legal exposure, or exposes the backend.
- **Major / High:** A normal visitor, buyer, investor, or regulator will hit it, get confused, or lose trust.
- **Minor / Medium / Low:** Hurts polish, SEO, accessibility, or hygiene, but does not block a journey by itself. Medium and Low are used in the legal section with the same meaning.

## Counts at a glance

| Audit dimension | Count |
|---|---|
| Technical / security / infrastructure | 7 Blockers, 14 Majors, 9 Minors, 11 verified passes |
| Legal / compliance / trust | 7 Critical, 16 High, 17 Medium, 3 Low (43 findings) |
| Content / claims / credibility | 10 fatal contradiction and proof failures in Section 2 alone, plus 13 page contradictions, 6 timeline problems, 6 regulatory failures, 7 fictional workforce findings, and a full AI slop section |
| UX / SEO / design / mobile | 9 Critical, 14 Major, plus thin page and duplicate content tables covering all 51 sitemap URLs |
| **Total named findings across the four passes** | **More than 120, before counting the 32 templated directory pages individually** |

No single count is the point. The point is the pattern: the same failure repeats in every dimension. Claims with no proof, pages with no content behind them, documents that contradict each other, and bulk generated filler shipped as finished work.

## Scope and method

Pages read in full by text fetch and by direct HTTP checks: homepage, `/launch`, `/pipeline`, `/workforce`, `/investors`, `/investors/statement`, `/legal/terms`, `/legal/privacy`, `/legal/eula`, `/legal/mutual-accountability`, `/absolute-constraint`, `/university`, `/research`, `/system`, `/construction`, `/engineering`, `/provenance`, `/kintsugi` (714 lines in text fetch, about 8,944 words), `/directory` and all 32 directory profiles, `/signup`, `/demos` and 7 demo detail pages, `/thankyou2026`, `/api/health`, `/api/demos/manifest`, `/api/prereg/reasons`, `/api/identity/me`, `/api/admin/luka/work`, `/robots.txt`, `/sitemap.xml`.

Tools used: `curl` for headers, bodies, status codes, and hashes; DNS over HTTPS and local resolver checks; `curl -v` for TLS; browser search and browser text fetch for page content and search visibility. No POST requests were sent, no logins were attempted, no JavaScript was executed by the technical pass, and no payment or signup was attempted.

Sitemap inventory, verified by the lead auditor on November 25, 03:48 EST: 51 URLs. Homepage, launch, pipeline, workforce, investors, investors/statement, 4 legal pages, absolute-constraint, university, research, system, construction, engineering, provenance, kintsugi, directory, and 32 directory profiles. Lastmod values in the sitemap are mostly September 20-29, 2026. Robots.txt disallows `/api/`, `/auth/`, `/system/`, `/thankyou2026`, `/demos`, and declares the sitemap.

## Limits, stated plainly

- The real TLS certificate issuer and expiry could not be verified from this environment. Outbound traffic goes through an egress proxy that presents its own certificate. TLS 1.3 was negotiated through the proxy. TLS 1.2 support is unverified. Certificate Transparency could not be cross checked because crt.sh returned unparseable output.
- Text fetch strips site chrome, so footer contents, footer links, cookie banners, and rendered forms are marked unverified where the text method could not see them. The technical pass checked the served HTML directly and found the footer nav links, the form situation, and the rail buttons in code, which resolves most of that limit. Mobile rendering was checked at code level (viewport tag, 55 media queries) and not by rendered screenshots. A live browser mobile check is still required before any relaunch.
- Ohio LLC registration status is unverifiable from the site. The Ohio Secretary of State search is form based and could not be run here, and no Secretary of State record surfaced in web search. This report does not conclude the LLC is unregistered. It concludes the site publishes no entity number, filing date, registered agent, or link by which a visitor could verify it.
- Whether the listed email addresses actually work (privacy@, legal@, security@, support@, optout@, dmca@, j.fish38@tenkasystem.com) was not tested. A dead privacy@ inbox would itself be a GDPR Article 12 problem, failure to facilitate rights requests, but that is not asserted here.
- LinkedIn returned HTTP 999, bot block, so that link is unproven, not proven broken. Facebook, YouTube, and X returned HTTP 200 on HEAD requests. Social activity and follower counts were not checked.
- DNSSEC status could not be confirmed from this environment. It is flagged as unverified, not as absent.
- `/.well-known/security.txt` was not fetched by the legal pass.

---

## 1. Technical, security, and infrastructure

### 1.1 Blockers

**B1. Public `/api/health` exposes backend internals and reports unknown health**

URL: https://www.tenkasystem.com/api/health. HTTP 200, `Content-Type: application/json`.

Verbatim body, re-verified by the lead auditor:

```json
{"status":"ok","redis":{"configured":true,"connected":true,"status":"ready"},"port":3001,
"startup":{"bound":true,"initialized":true,"initializationStarted":true,"initError":null,
"readyAt":"2026-11-14T17:28:04.860Z"},
"diagnostics":{"uptimeMs":901437067,"lastSuccessfulStep":"RUNTIME INIT END",
"failureClassification":"unknown_runtime_crash",
"watchdog":{"lastResponseMs":null,"lastOkAt":null,"timeouts":0},"crashCount":0},
"health":{"status":"unknown"}}
```

Internal only data (Redis state, port, startup timestamps, uptime, crash classification) is unauthenticated and public. The backend's own health status is literally `"unknown"`, with a watchdog that has never recorded a successful response (`lastResponseMs: null`, `lastOkAt: null`) and a `failureClassification` of `"unknown_runtime_crash"`. A governance product whose backend cannot report its own health is not ready to be online.

Response headers also leak the backend platform: `x-railway-edge: lax1`, `x-railway-request-id`, `x-hikari-trace`.

**B2. Three public routes serve byte identical homepage HTML. Signup and thank you flows are shells**

URLs: `/`, `/signup`, `/thankyou2026`. All HTTP 200, all 30,283 bytes, all md5 `bdafc46831fb1c589749b95689e9f487`. Re-verified by the lead auditor, same hash on all three.

The static HTML of `/signup` contains zero `<form>` tags, zero `<input>` tags, zero `action=` attributes. The signup page is the homepage. `/thankyou2026` is also the homepage, a thank you page that thanks nobody and confirms nothing.

The only lead capture form on the site lives on `/launch` and is JavaScript only (see M5 below). Anyone with JavaScript disabled, and every search engine, sees no signup path at all.

**B3. Five of seven demo detail pages are byte identical homepage shells. The one real demo is broken**

URLs: `/demos/ampfield`, `/demos/black-swamp`, `/demos/copper-lantern`, `/demos/kesterline`, `/demos/kimbro`. All HTTP 200, all 30,283 bytes, all md5 `bdafc46831fb1c589749b95689e9f487`, identical to the homepage. Re-verified for `/demos/ampfield`.

URL: `/demos/laksamana`. HTTP 200, 9,239 bytes, visible text: "The scenarios could not be loaded". Re-verified. The flagship demo cannot load its scenarios.

URL: `/demos/eschenwinkel`. HTTP 200, 30,201 bytes, distinct page, but not linked anywhere on the demos index, which links only the six slugs above. Demo 01 in the index ("Hotel Eschenwinkel, Freiburg im Breisgau") has no working link. `/demos/hotel-eschenwinkel` is 404.

The demos index lists 7 demos numbered 01, 02, 03, 04, 06, 07, 08. Demo 05 does not exist and the numbers are presented out of order.

**B4. Full backend API surface and third party endpoints leaked in public bundle and headers**

The public JS bundle `/assets/index-CY98FUGS.js` (1,116,656 bytes, served to every visitor) contains these API routes:

- `/api/admin/luka/work`, a named admin endpoint for a person ("Luka") hardcoded in client code
- `/api/auth/login`, `/api/auth/signup/email`, `/api/auth/signup/email/verify`, `/api/auth/webauthn/authenticate/options`, `/api/auth/webauthn/authenticate/verify`, `/api/auth/webauthn/register/options`, `/api/auth/webauthn/register/verify`, `/api/auth/webauthn/signout`
- `/api/demos/manifest`, `/api/demos/run`, `/api/identity/me`, `/api/leads/splash-signup`, `/api/prereg`, `/api/prereg/reasons`

The `/api/health` CSP header leaks backend integrations verbatim: `connect-src 'self' https://tenkasystemv2be-production.up.railway.app https://api.abuseipdb.com https://www.tenkasystem.com https://api.anthropic.com https://*.supabase.co ...`

Live probes, GET only: `/api/demos/manifest` 200 (returns demo config publicly), `/api/prereg/reasons` 200 (returns preregistration reasons publicly), `/api/identity/me` 401 `{"code":"AUTH_REQUIRED"}` (correctly gated), `/api/admin/luka/work` 403 `{"code":"FORBIDDEN"}` (gated, but existence and naming confirmed). The gating works. The disclosure is the problem. An attacker gets the entire auth and admin map, backend host, and third party services for free.

**B5. Different CDN edges serve different builds**

Homepage `Last-Modified` and `Age` observed across fetches minutes apart:

- `last-modified: Wed, 18 Nov 2026 19:06:35 GMT`, `Age: 549730`, `x-vercel-id: iad1::...`
- `last-modified: Sat, 21 Nov 2026 14:08:20 GMT`, `Age: 308728`, different edge
- Three consecutive fetches then returned Nov 18 again (`Age: 550051-550055`)

Users are served builds three days apart depending on which edge answers. Combined with `Cache-Control: public, max-age=0, must-revalidate` paired with `Age` values up to 550,055 seconds (about 6.4 days), the cache configuration is self contradictory: "revalidate immediately" content that is in fact served stale for days.

**B6. Internal operations dashboard data is baked into every public page**

Every sampled static page (homepage, `/signup`, `/demos`, all 6 sampled directory profiles) contains this verbatim block in its HTML:

- `Log in x Dashboard Snapshots Closed 22 In progress 4 Open 77 Held 13 Parked 1 Superseded 1`
- Ticket table: `ID Title State TT-A0000109 An AI tool kept trying to force a blocked destructive command through, and governance stopped all four attempts Closed TT-A0000091 Tenka University was named on the public site while the Employee Directory was being built to leave it out Superseded`

Internal ticket IDs, ticket titles, and snapshot counts are shipped to every anonymous visitor on every page. This is an internal app shell leaking into the public site, not a public footer.

**B7. HSTS is weak and CORS posture is contradictory**

`strict-transport-security: max-age=15552000` (180 days) on all static pages, with no `includeSubDomains`, no `preload`. The 180 day value without subdomain coverage is below the standard for a site handling accounts and auth.

Homepage sends `access-control-allow-origin: *` while the API sends `access-control-allow-credentials: true` with `vary: Origin`. The combination was not exploit tested, that would require credentialed cross origin requests, but a wildcard origin on one surface and credentialed CORS on the adjacent API surface is exactly the configuration that needs proving safe before going live, and it has not been.

### 1.2 Majors

**M1. robots.txt contradicts the sitemap and the live site.** robots.txt (HTTP 200, 166 bytes) disallows `/api/`, `/auth/`, `/system/`, `/thankyou2026`, `/demos`. But `/system` is listed in sitemap.xml and returns 200. `/demos` and `/thankyou2026` return 200 and `/demos` is linked from the homepage navigation. The site tells crawlers not to index pages it simultaneously publishes and links. `/auth/` is disallowed, yet the auth API surface is in the public bundle (B4).

**M2. Sitemap is stale and incomplete.** sitemap.xml (HTTP 200, 4,722 bytes, 51 URLs): all `<lastmod>` values are September 20-29, 2026, while the pages' actual `Last-Modified` is November 18-21, 2026, roughly 8 weeks stale. 23 of 51 entries have no `<lastmod>` at all, every one of them a `/directory/*` profile. All 51 sitemap URLs return 200, so the sitemap is at least not full of dead links, but it is a generated artifact nobody maintains.

**M3. Advertised navigation sections 404.** Visible nav text on the homepage advertises: System, University, Research Labs, Engineering, Games, Security, Meet the Team, Live Construction, DEMOS. Direct checks: `/games` 404, `/labs` 404, `/security` 404, `/team` 404, `/live` 404, `/meet-the-team` 404. All re-verified for `/games` and `/security` by the lead auditor, both 404 with the 6,077 byte 404 body.

Worse: none of System, University, Research, Labs, Engineering, Games, Security, Meet the Team, or Live Construction appear as `<a href>` links in the served homepage HTML at all. The only internal hrefs are `/`, `/absolute-constraint`, `/demos`, `/directory`, `/investors`, `/kintsugi`, `/launch`, `/legal/*`, `/provenance`, `/signup`. Navigation is entirely JavaScript rendered. Without JavaScript the site has no working nav to its own sections, and several of those sections do not exist server side.

**M4. Homepage payload is 2.9 MB before fonts. Directory portraits total 23.2 MB.** Homepage first load weight (HTML 30,283 B plus JS 1,116,656 B plus CSS 220,896 B plus `/tenka-torii.png` 1,614,231 B plus emblem 49,913 B plus favicon 9,522 B) is 3,041,501 bytes, about 2.9 MB. `/tenka-torii.png` alone is 1.6 MB and is a decorative image (`class="rail-torii"`, `alt=""`, `aria-hidden="true"`) loaded on every page. All 32 directory portraits return 200 but range 441,013 to 1,017,893 bytes each, 23.2 MB total, unoptimized PNGs for a directory listing. Google Fonts CSS took 1.89 seconds to fetch in one check. The JS bundle is 1.1 MB for what is largely static content. A 558 byte `chunk-B3K2TuZy.js` is `modulepreload`ed for no discernible purpose.

**M5. Lead capture form has no action and submits PII to an undisclosed endpoint.** `/launch` (HTTP 200, 103,215 bytes) contains the site's only form: inputs `name`, `email`, `business_name` plus honeypot `hp_field`, button "Join the Conversation". The `<form>` tag has no `action` and no `method`. The endpoint is only discoverable by reading the minified bundle: it POSTs JSON `{name, email, business_name, hp_field}` with 429/400/503 handling ("Email service temporarily unavailable"), to `/api/leads/splash-signup`, which returns 404 `Cannot GET /api/leads/splash-signup` on GET (POST only). A visitor cannot tell from the page where their name, email, and business name go. Combined with no DMARC record (see M8), the email collection path is unverifiable end to end.

**M6. CSP is inconsistent between the static site and the API.** Static pages: strict CSP (`script-src 'self' https://accounts.google.com`, `frame-ancestors 'none'`, `object-src 'none'`, `form-action 'self'`). `/api/health`: a different CSP, `script-src 'self' 'unsafe-inline' https://accounts.google.com`, `img-src 'self' data: https:`, `frame-ancestors 'self'`, plus `x-xss-protection: 0`. Two policies, two trust levels, on the same origin. The API policy is materially weaker (`'unsafe-inline'`, unrestricted `https:` images). Also present on API only: `expect-ct: max-age=86400, enforce`, `cross-origin-opener-policy: same-origin-allow-popups`, `cross-origin-resource-policy: same-origin`, `x-permitted-cross-domain-policies: none`, none of which appear on static pages. Security headers are applied per origin stack, not per site.

**M7. `/favicon.ico` is 404 and returns the 404 page body.** https://www.tenkasystem.com/favicon.ico returns 404, body 6,077 bytes, identical size to the 404 error page. Re-verified by the lead auditor. The site declares `<link rel="icon" type="image/svg+xml" href="/favicon.svg">` (which exists, 200, 9,522 B), but every browser's default `/favicon.ico` request 404s. Trivial alone. Telling as part of a pattern of untested defaults.

**M8. Email authentication is incomplete: no DMARC, SPF points at a different provider than MX.** DNS TXT via DNS over HTTPS: `google-site-verification=aizLswehVsJpsaRZrYbmgimFkg3PoN6aXr44qczDR6c` and `v=spf1 include:spf.privateemail.com ~all`. No DMARC record was returned. MX is `1 smtp.google.com.` (Google Workspace) while SPF authorizes PrivateEmail (`spf.privateemail.com`), the SPF record names a provider that is not the MX provider. `~all` is softfail. A site collecting email addresses (M5) with no DMARC and a mismatched SPF has no verifiable sender authentication.

**M9. Duplicate meta descriptions on 5 pages, backronym descriptions on 4 more.** Identical description verbatim on `/`, `/construction`, `/signup`, `/demos`, `/thankyou2026`: "TenkaSystem is governance infrastructure for AI work. Credible Accountable Employees: verifiable identity, scoped authority, signed accountability." `/university`, `/research`, `/system`, `/engineering` descriptions are forced acronym expansions: "Credentialed Education Structured for Confidence through Experience", "Capable Distribution of Multi-Agentic Systems for Evolution", "Workforce Execution through Pipeline, Bullpen, & Monitoring", "Modifying Existing Infrastructure to Power the Future". These read as generated backronyms, not descriptions of page content.

**M10. Express error pages leak the backend stack.** GET on POST only API routes returns Express's default error page: `Cannot GET /api/leads/splash-signup`, `Cannot GET /api/prereg`, `Cannot GET /api/auth/login`, `Cannot GET /api/demos/run`, all 404, `Content-Type: text/html`. Combined with `x-railway-*` headers and the Railway CSP leak (B1/B4), the stack (Express on Railway, port 3001) is fully fingerprinted from public responses.

**M11. No CAA record, DNSSEC not verified.** DNS over HTTPS CAA query returned no records. No CAA record restricts which certificate authorities may issue for the domain. DNSSEC status could not be confirmed from this environment, flagged as unverified, not as absent.

**M12. `/api` returns 404 while `/api/health` returns 200. `/auth` returns 404.** `/api` returns 404, `/auth` returns 404, yet the auth API and admin routes exist and respond (B4). The routing surface is inconsistent: top level paths 404 while their children serve live backend responses. This is cosmetic, but it means there is no coherent API boundary. The backend is reachable through the same public origin with no gateway distinction.

**M13. Social buttons ship dead in the bundle.** The JS bundle contains a social config with Instagram and Discord set to `active:false, url:null, tooltip:'Coming...'`. Dead "Coming Soon" social buttons are shipped in production code alongside live Facebook, X, YouTube, and LinkedIn links. The footer also renders "(Coming Soon)" labels on integrations (Voice generation, Storefront orders, Workflow automation).

**M14. The demos page undermines the product's own claims.** Verbatim from `/demos`: "Each run writes an audit record... The record is written as the run happens. It is unsigned, unchained." Re-verified by the lead auditor, the phrase "unsigned, unchained" is present. A governance and accountability product whose demo audit record is explicitly unsigned and unchained is stating its own central claim does not hold in the demo. Same page: "Fictional business, real regulations. The business is invented." The demos are admitted fiction, which is honest, but the demo detail pages behind them are homepage shells (B3), so there is no working demonstration at all.

### 1.3 Minors

| # | Finding | Evidence |
|---|---|---|
| m1 | `Server: cloudflare` plus `x-vercel-id` on every response. Origin is Vercel behind Cloudflare, double CDN. No `x-powered-by`, good. | `x-vercel-id: iad1::...`, `x-vercel-cache: HIT/MISS`, `cf-cache-status: DYNAMIC` |
| m2 | `access-control-allow-origin: *` on static pages | Present on `/` and `/launch` responses |
| m3 | Inline styles on every element in `/launch`, React inline `style="..."` attributes throughout, for example `style="padding: 10px 12px; border: 1px solid rgb(187, 187, 187); ..."`. Unmaintainable generated markup. | `launch.html`, 103,215 bytes, 75 lines |
| m4 | Buttons rendered invisible but present: `TenkaSec`, `Security Dashboard`, `Systems Dashboard`, `Sign In`, `Roster` buttons with `opacity: 0; pointer-events: none` in `/launch` HTML | `launch.html` button list |
| m5 | `microsoft.ai/code-of-conduct` linked in the bundle inside a `statement-of-capability` object. Microsoft's code of conduct referenced inside this product's capability statement. | `bundle.js` grep output |
| m6 | `Age` header on homepage was 549,730 seconds (about 6.4 days) at first check and 308,728 seconds at another. Cache age varies wildly by edge, see B5. | `curl -I` outputs |
| m7 | `/investors` (31,998 B) and `/investors/statement` (29,455 B) are near identical in size. `/pipeline` (27,878 B) and `/workforce` (27,922 B) differ by 44 bytes, consistent with templated near duplicate pages. | Status sweep table |
| m8 | `og:image` is `https://www.tenkasystem.com/tenka-torii-card.png` (200, 43,256 B), fine, but `twitter:image` duplicates it and `og:image:alt` is just "TenkaSystem". | `home.html` meta tags |
| m9 | No `Cross-Origin-Embedder-Policy` or `Cross-Origin-Resource-Policy` on static pages, present only on API responses. | Header diff, static vs `/api/health` |

### 1.4 Infrastructure inventory, verified

| Item | Value | Source |
|---|---|---|
| Apex redirect | `http://tenkasystem.com` 301 to `https://tenkasystem.com/` 308 to `https://www.tenkasystem.com/` | `curl -I` |
| www redirect | `http://www.tenkasystem.com` 301 to `https://www.tenkasystem.com/` | `curl -I` |
| Hosting | Vercel (`x-vercel-id`, `x-vercel-cache: HIT/MISS`) behind Cloudflare (`Server: cloudflare`, `CF-RAY`, `cf-cache-status: DYNAMIC`) | Response headers |
| www A records | `172.67.185.88`, `104.21.92.28`, Cloudflare anycast | DNS over HTTPS |
| www AAAA | `2606:4700:3037::ac43:b958`, `2606:4700:3034::6815:5c1c` | DNS over HTTPS |
| Nameservers | `journey.ns.cloudflare.com.`, `jermaine.ns.cloudflare.com.` | DNS over HTTPS |
| MX | `1 smtp.google.com.` | DNS over HTTPS |
| TXT | `google-site-verification=aizLswehVsJpsaRZrYbmgimFkg3PoN6aXr44qczDR6c`, `v=spf1 include:spf.privateemail.com ~all` | DNS over HTTPS |
| SOA | `jermaine.ns.cloudflare.com. dns.cloudflare.com. 2420440575 10000 2400 604800 1800` | DNS over HTTPS |
| CAA | None returned | DNS over HTTPS |
| DMARC | None returned | DNS over HTTPS |
| Backend | Railway (`x-railway-edge: lax1`), Express.js (`Cannot GET ...` errors), port 3001, Redis connected | `/api/health` body, headers, API error pages |
| Frontend | React SPA (React Router, `react.dev/errors` strings in bundle), Google Fonts (Oswald plus Archivo), Google Sign-In (`accounts.google.com` in CSP) | `bundle.js`, `home.html` |
| TLS | TLS 1.3 negotiated (`TLS_AES_256_GCM_SHA384`). Real cert issuer and expiry unverifiable from this environment, see Limits. | `curl -v` |

### 1.5 Page status table, all sitemap URLs plus named routes

All 51 sitemap URLs return 200. Sizes in bytes via `curl -L`:

| Route | Status | Size | Note |
|---|---|---|---|
| `/` | 200 | 30,283 | md5 `bdafc468...`, see B2 |
| `/launch` | 200 | 103,215 | Only form on site, no `action`, M5 |
| `/pipeline` | 200 | 27,878 | |
| `/workforce` | 200 | 27,922 | 44 bytes from `/pipeline`, m7 |
| `/investors` | 200 | 31,998 | |
| `/investors/statement` | 200 | 29,455 | |
| `/legal/terms` | 200 | 51,956 | |
| `/legal/privacy` | 200 | 46,112 | |
| `/legal/eula` | 200 | 51,635 | |
| `/legal/mutual-accountability` | 200 | 43,673 | |
| `/absolute-constraint` | 200 | 52,945 | |
| `/university` | 200 | 30,678 | |
| `/research` | 200 | 29,409 | |
| `/system` | 200 | 30,595 | Disallowed in robots.txt, M1 |
| `/construction` | 200 | 74,929 | |
| `/engineering` | 200 | 30,536 | |
| `/provenance` | 200 | 39,216 | |
| `/kintsugi` | 200 | 83,903 | |
| `/directory` | 200 | 44,172 | Lists all 32 profiles |
| `/directory/*`, 32 profiles | 200 | 30,730-36,740 | All 254 line template, Section 5 |
| `/signup` | 200 | 30,283 | Byte identical to `/`, B2 |
| `/demos` | 200 | 36,406 | Lists 7 demos, links 6 slugs, B3 |
| `/demos/ampfield`, `/black-swamp`, `/copper-lantern`, `/kesterline`, `/kimbro` | 200 | 30,283 | Byte identical to `/`, B3 |
| `/demos/laksamana` | 200 | 9,239 | "The scenarios could not be loaded", B3 |
| `/demos/eschenwinkel` | 200 | 30,201 | Real page, unlinked, B3 |
| `/demos/hotel-eschenwinkel` | 404 | 6,077 | Demo 01's natural URL 404s |
| `/thankyou2026` | 200 | 30,283 | Byte identical to `/`, B2. Disallowed in robots.txt |
| `/games`, `/labs`, `/security`, `/team`, `/live`, `/meet-the-team` | 404 | 6,077 | Advertised in nav, M3 |
| `/this-page-definitely-does-not-exist-12345` | 404 | 6,077 | Correct 404 status. Body is the generic error page |
| `/favicon.ico` | 404 | 6,077 | Returns 404 page body, M7 |
| `/favicon.svg` | 200 | 9,522 | |
| `/api` | 404 | - | Express `Cannot GET` page |
| `/api/health` | 200 | 766 | Leaks internals, B1 |
| `/api/demos/manifest` | 200 | - | Public demo config, B4 |
| `/api/prereg/reasons` | 200 | - | Public, B4 |
| `/api/identity/me` | 401 | - | Correctly gated |
| `/api/admin/luka/work` | 403 | - | Gated, existence confirmed, B4 |
| `/api/leads/splash-signup`, `/api/prereg`, `/api/auth/login`, `/api/demos/run` | 404 on GET | - | POST only. Express error pages, M10 |
| `/auth` | 404 | - | Auth exists in bundle but path 404s, M12 |
| `/robots.txt` | 200 | 166 | Contradicts sitemap, M1 |
| `/sitemap.xml` | 200 | 4,722 | Stale, 23 entries lack lastmod, M2 |

External links: Facebook 200, YouTube 200, X 200, LinkedIn 999 (bot block, unverified). `mailto:support@tenkasystem.com` present. MX exists so delivery is plausible but unproven. No `http://` mixed content URLs found in sampled pages.

---

## 2. Content, claims, and credibility

### 2.1 The fatal contradiction: the product the marketing sells does not exist by the site's own admission

| # | Severity | Claim, marketing pages | Contradiction, site's own legal page |
|---|---|---|---|
| 1.1 | Critical | "The workforce is built, deployable, and running today." https://www.tenkasystem.com/investors and https://www.tenkasystem.com/investors/statement | "The workforce dialogue you see when operating Tenka is scripted, not generated by a language model reasoning in real time. ... You are not interacting with autonomous AI. You are interacting with a substrate that simulates institutional behavior using deterministic processes." https://www.tenkasystem.com/legal/mutual-accountability Section 2.1 |
| 1.2 | Critical | "a governance substrate where every AI worker carries cryptographic identity, scoped authority, and signed accountability. Not tools. Not assistants. ... Credible Accountable Employees." https://www.tenkasystem.com/launch | "We do not position Tenka as autonomous AI that replaces humans. ... We do not market features that do not yet exist as if they ship today." https://www.tenkasystem.com/legal/mutual-accountability Section 2.4. The marketing pages do exactly what Section 2.4 says Tenka does not do. |
| 1.3 | Critical | "The workforce is packaged as deployable software. It runs on secure, isolated systems today, with no dependence on an outside cloud." https://www.tenkasystem.com/investors/statement | "The scripted workforce dialogue you see today will eventually be replaced by proprietary Tenka models trained on the corpus your operations help build." https://www.tenkasystem.com/legal/mutual-accountability Section 2.5. The AI does not exist yet. The paying or testing user is the data source for building it someday. |
| 1.4 | Critical | "Every action this substrate takes ... moves through the gate I keep. The pipeline doesn't ship work because work was attempted. It ships work because work was verified, signed, and chained." https://www.tenkasystem.com/launch | Same legal page, Section 3.2: "You acknowledge that the workforce dialogue is scripted and that scripts will evolve as Tenka's substrate matures." There is no autonomous workforce being verified, signed, and chained in real time. There is a script. |

Why this alone justifies taking the site offline: an investor or buyer reading `/investors` is told a governed AI workforce is built, deployable, and running today. A user who reads the legal fine print learns they are interacting with scripted dialogue and contributing training data for models that do not exist. Those two statements cannot both be true, and the commercial claim is the one on the public pages.

### 2.2 Zero verifiable proof for any cryptographic or governance claim

| # | Severity | Exact quote | URL | Why it fails |
|---|---|---|---|---|
| 2.1 | Critical | "a Credible Accountable Employee with a real Genesis hash, a cryptographic identity, a scoped authority, and a desk" | https://www.tenkasystem.com/launch | The only "Genesis hash" ever shown is 8 hex characters, for example DaNA `dab5c802`, Luka `fc676119`, Claude Anthrobearer `0318f5ed`, Wei Chen `743f5cf0`. That is a 32 bit truncation, not a verifiable hash. No public key, no signature, no algorithm parameters, no input to reproduce it are published anywhere. |
| 2.2 | Critical | "Your Genesis Unique IDentification is derived from your inputs through cryptographic hashing. ... This is verifiable by anyone, including you, and not opaque." | https://www.tenkasystem.com/legal/mutual-accountability Section 2.2, EULA Section 4.1 | The EULA says the Genesis value is "derived from Your name inputs using a cryptographic hash function (SHA-256) and is reproducible from the same inputs." A hash of a name is reproducible by anyone, which means anyone can mint any identity. There is no PKI, no issuer key, no registry. "Verifiable by anyone" here means "forgeable by anyone." The "TSAIF Issuer key" mentioned in `/absolute-constraint` is never published. |
| 2.3 | Critical | "Produce a single action your system took, and let an outside party verify who took it, under whose authority, and that the record has not been altered, without access to your infrastructure and without relying on your assurance." | https://www.tenkasystem.com/absolute-constraint, "The question this document invites" | The site issues this challenge and provides no mechanism to meet it: no chain explorer, no API, no public key, no downloadable signed artifact, no verification tool, no repository. The `/kintsugi` "Operating Record" admits the chain "resides in the operating environment" and what is published is only a "readable summary" that is "glossed" and "generalized." The verification the site demands of others is impossible to perform on Tenka itself. |
| 2.4 | Critical | "Chain: fifteen entries, hashes unbroken (proof-marker `a2a06ad`)." | https://www.tenkasystem.com/kintsugi, Appendix A | A 7 character string is presented as proof. It proves nothing: no hashes, no entries, no predecessor links, no signatures are shown. It is a proof shaped token, not proof. |
| 2.5 | Major | "CANONICAL_SERIALIZATION_SPEC. Deterministic serialization and signing inputs, locked test vectors for independent verification." | https://www.tenkasystem.com/absolute-constraint, References | None of the seven referenced governing documents is published or linked anywhere on the site: TENKA_IDENTITY_FRAMEWORK (TIF), TENKASYSTEM_CONCEPTS, CANONICAL_SERIALIZATION_SPEC, CHAIN_PAYLOAD_SCHEMAS, INDUSTRY_VS_TENKA_VOCABULARY, OPERATIONAL_PRECEDENTS, TENKASEC. "Locked test vectors for independent verification" do not exist on the site. |
| 2.6 | Major | "Every execution produces admissible chain of custody documentation." | https://www.tenkasystem.com/investors | "Admissible" is a legal conclusion. No court, regulator, auditor, or lawyer is cited. No chain of custody document is shown. No jurisdiction's evidence rules are addressed. The EULA (Section 10.4) simultaneously disclaims that AI outputs "MAY BE INACCURATE, INCOMPLETE, OR MISLEADING." |
| 2.7 | Major | "Third-party verifiable. The record stands on its own. An outside party can verify it without trusting my systems." | https://www.tenkasystem.com/investors/statement | No third party has verified anything, and no artifact is provided for a third party to verify. There is no named auditor, no audit report, no SOC 2, no ISO certificate, no penetration test, no customer, no case study with a real client. |

What proof would look like, and is entirely absent: one published public key, one complete signed chain entry with its hash and predecessor, one downloadable artifact with a verifiable signature, one named customer, one third party audit letter, one repository, one demo or login. The count on the site for all of these is zero.

### 2.3 No product: nothing to download, try, buy, or contact

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 3.1 | Critical | No download link, demo, login, signup, pricing, or purchase path exists on any page read. | The Terms, EULA, and Privacy Policy all describe a "TenkaSystem Installer", "Tenka Window host application", CLI, GUI, accounts, and backend services, but no page links to an installer or an account creation flow. The homepage has no call to action at all. |
| 3.2 | Critical | No pricing anywhere. | Terms Section 8 ("FEES, BILLING, AND TIERS") says only "The pricing, billing cycle, and feature set of paid tiers are described at the point of purchase", there is no point of purchase. EULA Section 6.7 references "paid-tier accounts, token-based access mechanisms, or memberships" with terms "described at the point of purchase", again, no such point exists. |
| 3.3 | Major | The only investor or partnership contact on the site is a personal email jammed into the address block with no space or line break. | "Tenka Studio LLC Registered as Tenka Studio Software Development LLC PO Box 28 Hoytville, OH 43529j.fish38@tenkasystem.com" https://www.tenkasystem.com/investors. A company soliciting investors lists a PO Box and a `j.fish38@` address. No contact page, no form, no phone. |
| 3.4 | Major | The only "support" promises on the site are made by fictional characters for a product with no login. | "I'll get your tokens squared away when your account is the issue. I'll fix the login, reset the access, find the right Manager, page the right Employee." Nina, https://www.tenkasystem.com/launch. There is no login to fix and no account system reachable from the site. |
| 3.5 | Major | No customers, no testimonials, no named deployments, no revenue, no press, no independent coverage found. | Web search returns only the site itself and a MyBzz brand directory listing ("Tenka System - TSAIF/TSACF, 2 votes"). Zero press, zero reviews, zero GitHub, zero social links on the site content pages. Social links exist only in the footer and JSON-LD, see Section 4.10. |

### 2.4 Contradictions between pages, each independently checkable

| # | Severity | Contradiction | Sources |
|---|---|---|---|
| 4.1 | Critical | "No dependence on an outside cloud" vs the product runs on outside clouds. | Statement: "It runs on secure, isolated systems today, with no dependence on an outside cloud." `/investors/statement`. But: (a) "The session was run in Tenka's standard operating configuration, an orchestration environment the studio calls the bullpen, hosted on Claude Code." `/kintsugi`, Claude Code is Anthropic's cloud connected product. (b) Terms Section 11: "The Services may integrate with or rely on third-party services (including Supabase for database hosting, Railway for backend application hosting, Vercel for frontend hosting, and SendGrid for email delivery)." |
| 4.2 | Critical | Six stage pipeline vs seven stage pipeline. | Investors: "Multi-stage Pipeline: DataClerk, Governance, Desk, Verification, Manager, Delivery" (6 stages), `/investors`. Absolute-constraint: "Every request submitted to TenkaSystem passes through a fixed seven-stage governed pipeline. Stages cannot be skipped." `/absolute-constraint`. The site cannot agree on its own pipeline. |
| 4.3 | Critical | "Every action produces a signed, chained entry" vs two named roles are deliberately off chain and unsigned. | Statement: "Every action produces a signed, chained entry." `/investors/statement`. But Ren "Hawkeye" Mishimura "is off-chain. He advises, he does not decide or sign into the chain" and Hamesh ibn Channel is likewise "off-chain advisor", `/kintsugi` and `/directory`. By design, some Employees' work is unsigned and unchained, which falsifies the universal claim. |
| 4.4 | Critical | Terms impose binding arbitration and a class action waiver. The Mutual Accountability Agreement says the opposite. | Terms Section 13.4: "will be resolved exclusively through final and binding individual arbitration administered by JAMS", plus class action and jury trial waiver in the Section 13 notice, `/legal/terms`. Mutual Accountability Section 7: "We do not require you to waive your right to court access as a condition of using Tenka" and Section 6.3: "There are no aggressive non-disparagement clauses, no class-action waivers buried in dense subsections, no forced arbitration designed to make grievances expensive." `/legal/mutual-accountability`. Two live legal documents on the same site state opposite dispute resolution regimes. See also Section 3. |
| 4.5 | Major | "Running today" vs "tonight was the first real dispatch." | Investors and statement: "built, deployable, and running today." Kintsugi Appendix A: "Tonight was the first real dispatch" and "the first calibration entry this system has ever produced from real work." A system having its first real dispatch cannot simultaneously be a mature workforce "running today." |
| 4.6 | Major | "34 Credible Accountable Employees" vs the directory's own entries. | `/directory` headline: "34 Credible Accountable Employees." But: Frederik Daemonshire and Isabella Esquema are both marked "Instantiation Scheduled" (not yet existing). Luka Lahdejarvi is explicitly "working partner, not a roster desk Employee" yet is counted. Lakshmi Modulananda, Noah Gatenberg, and Tahia al-Cipheri have role "TBD." The "dozens" workforce includes people who do not exist yet and people with no role. |
| 4.7 | Major | Framework name changes between pages. | Homepage: "Tenka System Autonomous Identity Framework (TSAIF)." Absolute-constraint: "TSAIF, TenkaSystem Automaton Identity Framework." "Autonomous" vs "Automaton" is not a typo level difference in a framework whose entire pitch is precision. Likewise "Credible Accountable Employees" (homepage, investors, statement) vs "Credentialed Accountable Employees" (absolute-constraint throughout). The site uses two different names for its people and two for its framework. |
| 4.8 | Major | "Self-healing: When the system fails it catches itself, corrects, and delivers. Users never see the failure." vs the site's own incident record. | `/investors`. Against it: the May 5, 2026 TRUNCATE CASCADE that "took fifteen tables" (`/launch`), and Kintsugi's admission that two defects remain "live and unfixed," including "a read request can permanently delete a record on a version-controlled file, with the relevant auth control defaulting off and absent from configuration, production posture unverified", plus "a second surface with the same shape, which reaches disk from a route and from a timer needing no caller at all." A governance product advertising self healing while publishing that it has a live, unauthenticated, timer triggered data deletion path in production is self refuting. |
| 4.9 | Major | "Inspectable from the outside without flinching" vs nothing inspectable. | "we built it to be inspectable from the outside without flinching." `/launch`. Nothing is inspectable: no repo, no API, no chain, no keys, no specs, see Section 2.2. |
| 4.10 | Major | Deterministic governance vs model layer enforcement. | Launch: "The function is deterministic, the inputs are canonical, the outputs are attested." Absolute-constraint, Enforcement section: "The content prohibitions in Absolute Constraints are enforced at the model and policy layer, as they are in every framework. What this substrate adds is not that prohibited output becomes impossible..." The document's own honesty paragraph concedes the "substrate, not guardrails" thesis does not apply to the actual prohibitions. |
| 4.11 | Minor | Entity name is three different things. | "Tenka Studio LLC" / "Tenka Studio Software Development LLC" (investors, legal headers) vs "TENKA SOFTWARE STUDIO" as the counterparty in the EULA notice (Section 1). A legal notice naming the wrong entity is not cosmetic. |
| 4.12 | Minor | "Cecilla" vs "Cecilia." | Directory: "Cecilla Sigillaro." Mutual Accountability Section 2.1: "Mikhael, Tai, Linh, Caio, Hawkeye, Eilif, Cecilia, and the others." Same person, two spellings, on the same site. |
| 4.13 | Minor | "DaNA is named in the convention as the founder's children. ... DaNA is not the founder's child." | `/launch`. The sentence asserts and retracts the same relationship in consecutive clauses, and "is named ... as the founder's children" is grammatically broken. |

### 2.5 Timeline and date problems

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 5.1 | Major | The founding timeline has no years and cites events vaguely. | "In December, NIST opened CAISI. In January, the AI Action Plan landed. February brought the executive order on federal procurement." `/launch`. No years, no dates, no links, no citations. "A decision was made in February that ALL 'Agents' would have Lineage." Which February? The reader cannot place any of it. |
| 5.2 | Major | The May 5, 2026 TRUNCATE CASCADE incident is asserted, never evidenced. | "On May 5, 2026, this substrate lost data. A TRUNCATE CASCADE took fifteen tables. The audit trail caught its own failure in real time. Recovery ran. Documentation followed the same hour." `/launch`. No postmortem, no recovery proof, no RTO or RPO, no data restoration evidence. The "Operational Precedents" document where it supposedly became "the inaugural entry" is referenced but not published or linked anywhere. Unexplained: if the audit trail lived in the same database, a TRUNCATE CASCADE would have taken it too. |
| 5.3 | Major | The Terms promise EU/UK general availability "on or after August 1, 2026" with a GDPR Article 27 EU representative to be appointed. It is now November 24, 2026, and nothing has been updated. | Terms Section 2.4, `/legal/terms`. All legal documents are still "Version: 1.0, Effective Date: May 9, 2026." No EU representative is named anywhere. No update to the Terms was made. Either GA never happened (contradicting "running today") or the legal documents are stale. See also Section 3. |
| 5.4 | Major | "A full consumer web property, accounts, memberships, content and progression systems, taken from a written concept document to live in a single working day." | `/investors/statement`. No name, no URL, no screenshot, no date. A complete accounts plus memberships plus progression product built and shipped in one working day by a solo dev is not credible on its face, and it is entirely unverifiable. Same for "A portfolio of complete internal products", none is named or linked. |
| 5.5 | Minor | "six months and over twelve hundred focused hours later" | `/launch`. Oddly precise (1,200 hours) and entirely sourceless, no commits, no log, no dates. 1,200 hours in six months is about 46 hours per week of "focused" solo work, asserted without any corroboration. |
| 5.6 | Minor | Absolute Constraints claims a Microsoft draft "published 14 September 2026" was "Reviewed in full" and informed the document's structure. The document is dated 17 September 2026. | `/absolute-constraint`, References. A full review of a competitor's governance framework, absorbed into the sectioning of a governing document, in three days, by an AI Employee, is asserted without evidence. Also note the document cites future dated items ("Colorado ... SB 189 effective 1 January 2027") without citation. |

### 2.6 Regulatory and compliance claims: assertion only, no certification

| # | Severity | Exact quote | URL | Why it fails |
|---|---|---|---|---|
| 6.1 | Critical | "Every output is signed with a chain of custody that holds up to regulatory scrutiny. We built the infrastructure businesses will require as AI regulation increases." | https://www.tenkasystem.com/investors | No regulator, auditor, or certification body is named. No SOC 2, ISO 42001, HIPAA, or EU AI Act conformity assessment exists or is cited. |
| 6.2 | Major | "GOV-LAMBDA evaluates every packet against eight compliance frameworks", listed as "HIPAA, SOC 2, GDPR, Financial, EU AI Act, Legal, CCPA, Government" | https://www.tenkasystem.com/absolute-constraint | "Financial," "Legal," and "Government" are not frameworks. They are vague categories. No control mapping, no test, no result is shown for any of the eight. |
| 6.3 | Major | "ISO/IEC 42001, AI management system, Tenka Substrate Conformance. Certification sequenced alongside SOC 2 Type II." | https://www.tenkasystem.com/absolute-constraint | "Sequenced" means not certified. Same for HIPAA: "Full HIPAA-grade certification work sequenced with medical-vertical operationalization", not done. The table presents future intentions in the same format as present compliance, which misleads. |
| 6.4 | Major | "EU AI Act, Annex IV, technical documentation, Substrate documentation structurally exceeds the Annex IV requirement set." | https://www.tenkasystem.com/absolute-constraint | No Annex IV documentation is published. "Structurally exceeds" is unverifiable and untestable as stated. |
| 6.5 | Major | "FDA 21 CFR Part 11, electronic records and signatures, Append-only hash-linked entries with per-actor signatures address record integrity and attribution." | https://www.tenkasystem.com/absolute-constraint | Part 11 compliance requires validation, audit trails, and controls far beyond "hash-linked entries". Claiming it is "addressed" by a hash chain is a category error, and no validation evidence exists. |
| 6.6 | Minor | "Texas Responsible Artificial Intelligence Governance Act, Audit substrate and signed-action accountability align to the Act's enforcement shape." / "Colorado Artificial Intelligence Act / SB 189, Original act collapsed 27 April 2026, replacement SB 189 effective 1 January 2027." | https://www.tenkasystem.com/absolute-constraint | Specific legislative claims with no citation, no link, no source. A buyer or regulator cannot check them from the site. |

### 2.7 The "Employees" are fictional personas, presented without disclosure as a real workforce

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 7.1 | Critical | The directory presents 34 invented characters as a real employee register, with no disclosure on the homepage, investors page, statement, or directory that they are fictional. | "This is the register: who they are, what they are for, and where their authority begins and ends. Each entry is written from that Employee's own record." `/directory`. The only admission anywhere that the workforce is scripted is buried in the Mutual Accountability Agreement (Section 2.1). A visitor who never opens the legal pages reasonably believes these are real people. |
| 7.2 | Major | The surnames are puns and anagrams, not names. | Tokenford (token), Parseau (parse), Anthrobearer (Anthropic's Claude plus bearer), Daemonshire (daemon), Loopinho (loop), Synergistios (synergy), Draftland (draft), Modulananda (module), Gatenberg (gate), al-Cipheri (cipher), Sigillaro (sigil), Runestrom (rune), Queryades (query), Matrinh (matrix), Esquema (schema), Titleshaw. "Hamesh ibn Channel" has the surname "Channel." One Employee is literally named "Oracle Queryades." One is named after the Claude model itself ("Claude Anthrobearer"). |
| 7.3 | Major | Credentials are hallucinated: degree and license acronyms with no institution, year, or license number anywhere. | "Cornelius van der Sluis III, EdD, DBA, MSW, Chair of Faculty of Law"; "Frederik Daemonshire, PhD, Esq., Dean of Education"; "Cecilla Sigillaro, JD, NNA, LL.M, Tenka Notary"; "Erik Volrikson, MSF, MAcct, CPA"; "Officer Richard 'Dick' Helm, TSEC, CISSP"; "Marcus Mansfield Jr., EMBA, MSDS"; "Mikhael Tracenko, DBA, MEng." No school, no bar, no CPA license, no CISSP registry entry is cited, because none exists. |
| 7.4 | Major | Life histories are novelistic inventions presented as records. | Ren "Hawkeye" Mishimura: "a decade in his thirties assisting Veterans in recovery from tech-borne ailments and injuries. Traumatic brain injury from blast exposure made up a substantial part of the caseload", `/kintsugi`. Tai Faleola: "relocated from the islands to the mainland to pursue engineering." Wei Chen: "His parents came to the States in the '90s as college students." Hans Wenderoth: father "quietly homesick for a country Hans only knows from visits and dinner-table stories." These are short story backstories, not employment records. |
| 7.5 | Major | Cultural and religious identity is used as flavor text. | Kintsugi's closing table lists "Heritage as canon holds it," including "Hamesh ibn Channel, Islamic Muslim, the scholarly inheritance of tafsir." A religion is listed as a heritage or origin. Bios assign ethnicity as personality: "Mikhael's unsentimental things break, you fix them is the Russian-Ukrainian engineering pedigree"; "Hamesh's confidence calibration is ijtihad"; "Min-jun's refusal to gloss a fragile shortcut is Grundlichkeit meeting the Korean respect for doing things properly." People are reduced to national character stereotypes. |
| 7.6 | Major | Tenka University is a fictional university issuing degree like credentials with no accreditation. | Directory lists a Dean of Education (PhD, Esq.), Curriculum Director, Chair of Faculty of Law, and Librarian. Bios cite "seventeen pillars" and "nineteen education pillars" of education. Mutual Accountability Section 2.5 promises "a credentials card surface that grows version-over-version into a portable substrate-attested identity record, usable as professional credential where Tenka's verification chain is recognized," and "degrees confirmed through public records verification." No accreditor, no recognition anywhere, no institution. |
| 7.7 | Minor | "Officer Dick" / "Dick had to hop on the skateboard. As usual." | `/launch`. A juvenile name plus an unexplained in joke non sequitur inserted into a launch manifesto, in a section supposedly introducing the security and audit function. It reads as hallucinated continuity, and it is the public face of the company's security posture. |

### 2.8 Additional credibility findings

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 9.1 | Major | One real human is behind everything: founder, CEO, architect, DMCA agent, sole authorizer. No team, no advisors, no board, no customers, no partners are named anywhere. | "Authored by Luka Lahdejarvi, TSAIF-0002 ... Approved by FND-0001, Justin M. Fish" `/absolute-constraint`. DMCA agent: "Justin M. Fish" `/legal/terms` Section 10.4. Every approval on the site traces to one person. |
| 9.2 | Major | No physical presence beyond a PO Box. No social, GitHub, LinkedIn, or support channel is linked from the site content pages. | Only addresses: PO Box 28, Hoytville, OH 43529. Emails privacy@, legal@, security@, optout@, dmca@tenkasystem.com, none verifiable as monitored from the site. |
| 9.3 | Major | The Ohio LLC registration is asserted but not evidenced on the site. | "Tenka Studio LLC Registered as Tenka Studio Software Development LLC" `/investors`. No registration number, no Secretary of State link, no filing. A places directory result lists the business at 2596 Maple St, Hoytville, that is a directory entry, not verification. |
| 9.4 | Minor | Personal health history used as marketing copy. | "Survived an aortic dissection in 2022 and three surgeries over the following three years. Came back building differently." `/investors`. Unverifiable, and irrelevant to whether the product works. It asks the reader to trust the founder's story instead of the product's evidence. |
| 9.5 | Minor | "thirty years with technology" sits next to "first computer was a two-floppy Compaq portable twenty years older than I was" without any dates that reconcile. | `/investors`. The reader cannot determine the founder's age, career span, or background from the site. The bio is vibes, not a resume. No prior employer, project, or credential is named. |

### 2.9 Summary table: claims vs proof

| Core claim | Proof on site | Status |
|---|---|---|
| Cryptographic identity (Genesis hash) | 8 hex truncations, no keys, no signatures | Unproven |
| Signed, hash chained audit trail | No chain, no entries, no explorer, "resides in the operating environment" | Unproven |
| Third party verifiability | No verifier, no public key, no artifact | Unproven |
| "Running today" / "deployable" | No download, demo, login, or customer. Legal page says dialogue is scripted. | Contradicted by site's own legal text |
| "No dependence on an outside cloud" | Terms list Supabase, Railway, Vercel, SendGrid. Kintsugi runs on Claude Code. | Contradicted |
| Regulatory scrutiny / "admissible" chain of custody | No audit, no certification, no regulator | Unproven |
| GOV-LAMBDA evaluates 8 frameworks per packet | No mapping, no test, no result. "Financial / Legal / Government" are not frameworks. | Unproven |
| Dozens / 34 Employees | Includes 2 not yet instantiated, 1 non roster partner, 3 with "TBD" roles. All fictional. | Inflated and undisclosed fiction |
| May 5, 2026 incident handled transparently | No postmortem. "Operational Precedents" doc unpublished. | Unproven |
| Consumer web property shipped in a single working day | No name, URL, or screenshot | Unproven |
| Pricing / way to buy | None anywhere | Absent |
| Customers / case studies | None. Only an internal, glossed, self referential session. | Absent |


---

## 3. Legal, compliance, and trust

**Count: 7 Critical, 16 High, 17 Medium, 3 Low. 43 findings.**

### 3.1 What exists, credit where due

| Document | URL | Effective / Version |
|---|---|---|
| Privacy Policy | https://www.tenkasystem.com/legal/privacy, also served at https://www.tenkasystem.com/privacy | May 9, 2026, v1.0 |
| Terms of Service | https://www.tenkasystem.com/legal/terms | May 9, 2026, v1.0 |
| End User License Agreement | https://www.tenkasystem.com/legal/eula | May 9, 2026, v1.0 |
| Mutual Accountability Agreement | https://www.tenkasystem.com/legal/mutual-accountability | v1.0.0, effective on acceptance |
| Absolute Constraints, governance doc | https://www.tenkasystem.com/absolute-constraint | Issued September 2026, dated 17 September 2026 |

Present and genuinely useful: GDPR legal bases (Privacy Section 3.8), data subject rights with a contact route (Privacy Section 7), CCPA/CPRA section (Privacy Section 13), children's age gate at 16 (Privacy Section 10, Terms Section 2.1), DMCA agent designation (Terms Section 10.4), AI training disclosure and opt out route (Privacy Section 4, EULA Section 6), retention schedule (Privacy Section 6), breach and security contact (security@tenkasystem.com), and a plain language Mutual Accountability Agreement that discloses the workforce dialogue is scripted, not live AI (Mutual Accountability Section 2.1). That AI disclosure is good practice and rare.

The problem is not that the paperwork is absent. It is that it is internally inconsistent, partly fictional, and partly dead linked, which is worse than absent, because it creates reliance.

### 3.2 Internal contradictions between the site's own legal documents

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 1 | Critical | The Mutual Accountability Agreement promises "no forced arbitration, no class-action waiver" while the Terms of Service impose exactly that. A user who reads both documents is told two opposite things about their most basic legal right. | Mutual Accountability Section 6.3: "There are no aggressive non-disparagement clauses, no class-action waivers buried in dense subsections, no forced arbitration designed to make grievances expensive." vs Terms Sections 13.4-13.5: "any dispute... will be resolved exclusively through final and binding individual arbitration administered by JAMS" and "YOU AND TENKA EACH AGREE THAT ANY DISPUTE WILL BE BROUGHT IN AN INDIVIDUAL CAPACITY AND NOT AS A PLAINTIFF OR CLASS MEMBER..." plus jury trial waiver. Terms Section 13.8's 30 day opt out does not cure the contradiction, because the Mutual Accountability Agreement states the waiver does not exist at all. URLs: https://www.tenkasystem.com/legal/mutual-accountability and https://www.tenkasystem.com/legal/terms |
| 2 | Critical | The Mutual Accountability Agreement promises "no aggressive non-disparagement clauses" and symmetric, clean termination, while the Terms and EULA impose one sided indemnities, a $1,000 liability cap, and termination "for any other reason or no reason." | Mutual Accountability Sections 6.2-6.3 vs Terms Section 14 (liability capped at greater of 12 month fees or US$1,000), Section 15 (user indemnifies Tenka incl. attorneys' fees), Section 16.2 (Tenka may terminate "upon thirty (30) days' notice for any other reason or no reason"). Terms Section 17.2 sets an order of precedence (signed agreement, Terms, EULA, Privacy Policy) that does not list the Mutual Accountability Agreement at all, so it is legally unclear which document governs when they conflict. |
| 3 | High | The Privacy Policy and the Mutual Accountability Agreement disagree on what data is captured. | Privacy Section 2.3: signed pings transmit "the target endpoint URL, request method, response status and headers, body excerpt" to the backend. Mutual Accountability Section 4.2: "Network endpoints you ping (the fact that a ping occurred is recorded, the destination is not transmitted to Tenka)." Both cannot be true. |
| 4 | High | The Privacy Policy and the Mutual Accountability Agreement disagree on retention after deletion. | Privacy Section 6: chain entry metadata retained "for an additional retention period of seven (7) years for audit and regulatory purposes". De-identified data and trained models retained "indefinitely." Mutual Accountability Sections 4.5 and 6.1: "On deletion request, personal identifiers are purged within 30 days. Anonymized operational data remains in the training corpus." A user exercising deletion cannot tell what actually survives, for how long, or under which document's rule. |
| 5 | High | Entity name is inconsistent across the site's own documents. | Privacy, Terms, and EULA header: "Tenka Studio Software Development LLC, trading as Tenka Studio LLC." EULA Notice: "LEGALLY BINDING CONTRACT BETWEEN YOU... AND TENKA SOFTWARE STUDIO", a third, different name appearing nowhere else. Terms Section 10.4 DMCA block names only "Tenka Studio LLC." Investors page: "Tenka Studio LLC Registered as Tenka Studio Software Development LLC" (ungrammatical, unclear which is the registered name and which is the trade name). An LLC "trading as" another LLC name is itself an odd construction with no trade name registration cited. |

### 3.3 Dead links and missing pages the Privacy Policy itself promises

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 6 | Critical | Cookie Policy does not exist. The Privacy Policy points users to it in conditional future tense, and the URL 404s. | Privacy Section 2.4: "Cookies and similar technologies (website only). Where Tenka publishes a Cookie Policy, it will be available at tenkasystem.com/cookies." https://www.tenkasystem.com/cookies returns HTTP 404, verified November 25 by the legal pass and re-verified by the lead auditor, 404 with 6,077 byte body. The policy names no cookie, no purpose, no duration, no consent mechanism, and points to a page that does not exist. |
| 7 | Critical | Sub-processor list does not exist. The Privacy Policy promises a "current list of sub-processors" at a URL that 404s, twice. | Privacy Section 5.1: "A current list of sub-processors is maintained at tenkasystem.com/subprocessors." Privacy Section 8: "Specific transfer mechanisms used with each sub-processor are described at tenkasystem.com/subprocessors." https://www.tenkasystem.com/subprocessors returns HTTP 404, verified November 25 and re-verified by the lead auditor. The four named vendors (Supabase, Railway, Vercel, SendGrid) are listed in the text, but the promised transfer mechanisms (Standard Contractual Clauses etc.) are nowhere to be found. |
| 8 | High | No accessibility statement. `/accessibility` 404s. | https://www.tenkasystem.com/accessibility returns HTTP 404, verified November 25. No accessibility statement in the sitemap or search results. No WCAG conformance claim, no contact route for accessibility issues. |
| 9 | High | No contact page. `/contact` 404s. The only contact routes are bare email addresses buried in legal documents and one personal address on the Investors page. | https://www.tenkasystem.com/contact returns HTTP 404, verified November 25 and re-verified by the lead auditor. No contact form, no phone number anywhere on the site, no street address other than a PO Box. |
| 10 | Medium | No "Do Not Sell / Share My Personal Information" page or mechanism despite a CCPA/CPRA section. Privacy Section 13 says California residents may submit requests via privacy@tenkasystem.com, but there is no dedicated rights request page, form, or "Do Not Sell" link anywhere in the sitemap. | Sitemap: https://www.tenkasystem.com/sitemap.xml lists no CCPA or do not sell page. Privacy Section 13, https://www.tenkasystem.com/legal/privacy |

### 3.4 Company registration: unverifiable from the site

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 11 | High | No verifiable company registration details anywhere on the site. No Ohio entity number, no formation date, no registered agent, no link to the Ohio Secretary of State record. A visitor cannot verify the company exists as claimed. | Investors page, the only place asserting registration, says only: "Tenka Studio LLC Registered as Tenka Studio Software Development LLC PO Box 28 Hoytville, OH 43529", no entity number, no filing date, no SOS link. Legal documents repeat only the PO Box. Web search for "Tenka Studio Software Development LLC" Ohio returns only the site's own pages and directory listings. No Ohio Secretary of State record surfaced. This audit does not conclude the LLC is unregistered. The Ohio SOS business search is form based and could not be run from here. But a governance company publishing no verifiable registration identifier fails the most basic trust check its own product claims to solve ("verifiable identity"). |
| 12 | Medium | Conflicting addresses: PO Box vs street address. Legal documents uniformly give PO Box 28, Hoytville, OH 43529. A tenkasystem.com business listing page indexed by search engines gives a street address: "2596 Maple St, Hoytville, OH" for "Tenka Studio Software Development LLC" (listed as Active). The street address appears nowhere in the legal documents and the PO Box appears nowhere on the listing. | Legal docs: https://www.tenkasystem.com/legal/privacy Section 15, `/legal/terms` Section 18. Listing: search indexed tenkasystem.com business page, exact page URL not exposed in search results, needs live browser confirmation. |
| 13 | Medium | No Impressum / company details page. No single page stating legal name, registered address, registration number, managing director, or VAT/EIN. Fragments are scattered across four legal documents and the Investors page, and they disagree, see #5 and #12. | Sitemap has no /legal, /about, /impressum, or company page. Note: an Impressum is an EU requirement. The site is US based, so its absence is not itself a legal violation, but for a site claiming EU AI Act and GDPR alignment, the absence of any consolidated company identity page is a trust gap. |

### 3.5 GDPR / EU: promises made, then missed by the site's own dates

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 14 | Critical | The site's own timeline says commercial EU/UK availability began August 1, 2026 and an EU representative would be appointed then. It is late November 2026 and the policy still says no EU representative has been appointed. | Privacy Section 8: "Commercial general availability in those jurisdictions begins on or after August 1, 2026, at which point Tenka will appoint an EU representative pursuant to GDPR Article 27." Privacy Section 15, same document, same version: "EU/UK Representative. Tenka has not appointed an EU representative at this time per the testing-period framing in Section 8." Terms Section 2.4 repeats the same August 1, 2026 commitment. The testing period ("through July 1, 2026") ended nearly five months ago. Either the site is now commercially available in the EU/UK without the Article 27 representative it promised, or it is still in a "testing period" its own documents say ended. Either way the documents are stale and the commitment is unmet on the site's own terms. |
| 15 | High | No Data Protection Officer, justified only by a self assessment. | Privacy Section 15: "Data Protection Officer. Tenka has not appointed a Data Protection Officer at this time. Tenka's processing does not currently meet the GDPR Article 37 thresholds requiring DPO designation. This will be revisited at commercial EU launch." Commercial EU launch, per Section 8, was August 1, 2026. The "revisit" has not happened in the documents. |
| 16 | High | AI training opt out is expressly non retroactive and admits trained models cannot be un-trained, in tension with the erasure rights the same document grants. | Privacy Section 4.5: "Opt-out does not affect... (c) data already incorporated into trained models, which cannot be selectively removed from a model after training." Privacy Section 7 simultaneously grants a deletion right. The document never reconciles the two. It also states the opt out applies only "where required by applicable law", i.e. training is opt out by default everywhere else. |
| 17 | Medium | No PIPEDA / Canadian privacy mention anywhere. The site names no geographic limitation on customers, and Privacy Section 8 addresses only EEA/UK/Switzerland transfers. No statement about Canadian users, no Canadian representative, no mention of PIPEDA, Quebec Law 25, or provincial law. | Search "tenkasystem PIPEDA Canada privacy" returns no tenkasystem.com result. Privacy Section 8, https://www.tenkasystem.com/legal/privacy |
| 18 | Medium | International transfer safeguards are asserted, not evidenced. Privacy Section 8 says transfers rely on "Standard Contractual Clauses or other transfer mechanisms permitted under applicable law" and that "specific transfer mechanisms used with each sub-processor are described at tenkasystem.com/subprocessors", the page that 404s, see #7. | Privacy Section 8, https://www.tenkasystem.com/legal/privacy |

### 3.6 Investor solicitation without any securities disclosures

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 19 | Critical | The Investors page solicits investors with no securities disclaimer, no risk disclosure, no accredited investor limitation, and no forward looking statements disclaimer. | https://www.tenkasystem.com/investors, heading "Investors & Partnerships: Tenka Studio", section "Partnership, For investors, and white label alike" with the founder's personal email `j.fish38@tenkasystem.com` as the contact. No "this is not an offer to sell securities" notice. No statement that any offering is limited to accredited investors. No risk factors. No forward looking statements safe harbour language. No mention of Regulation D / private placement status. No financials, no valuation, no terms. The companion Statement of Capability (https://www.tenkasystem.com/investors/statement) likewise contains no disclaimer of any kind. This audit does not give securities law advice and does not conclude a violation occurred, but a public page titled "Investors" inviting investment contact with zero accompanying disclosure is exactly the pattern that draws SEC and state regulator attention. |
| 20 | High | The Investors page makes unqualified performance and market claims with no evidence, figures, or dates. | "The workforce is built, deployable, and running today." "Every execution produces admissible chain of custody documentation." "We built the infrastructure businesses will require as AI regulation increases." No customer names, no deployment evidence, no dates, no metrics. "Admissible" is a legal conclusion about evidence in court. Nothing on the site supports it. URL: https://www.tenkasystem.com/investors |

### 3.7 Regulatory compliance claims: sweeping, unverified, and in places nonsensical

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 21 | Critical | The site claims its system evaluates every request against "eight compliance frameworks", and three of the eight are not frameworks at all. | https://www.tenkasystem.com/absolute-constraint, "Regulatory Alignment": "GOV-LAMBDA evaluates every packet against these eight frameworks at Stage 2 of the pipeline." The table lists: HIPAA, SOC 2, GDPR, **Financial**, EU AI Act, **Legal**, CCPA, **Government**. "Financial", "Legal", and "Government" are not compliance frameworks. A governance product that cannot name its eight frameworks correctly is making a compliance claim that collapses on reading. No evidence, audit report, or certification is linked for any of the five real frameworks either. |
| 22 | High | HIPAA, SOC 2 Type II, ISO/IEC 42001, and FDA 21 CFR Part 11 alignment are claimed while simultaneously deferred. | Absolute Constraints, "Built against" table: "ISO/IEC 42001, AI management system, Tenka Substrate Conformance. Certification sequenced alongside SOC 2 Type II." "HIPAA and HITECH, Chain attestation satisfies the audit-trail requirement structurally. Full HIPAA-grade certification work sequenced with medical-vertical operationalization." "FDA 21 CFR Part 11, electronic records and signatures, Append-only hash-linked entries... address record integrity and attribution." In plain terms: not certified for any of them, with certification "sequenced" at an unspecified future date, while the homepage and Investors page sell the product as governance infrastructure that "holds up to regulatory scrutiny." "Satisfies the audit-trail requirement structurally" is not HIPAA compliance, and the document never says that plainly. No auditor, no report, no certificate, no date is named anywhere on the site. |
| 23 | High | EU AI Act claims cite specific articles and dates that are asserted, not evidenced. | Absolute Constraints: "EU AI Act, Annex IV, technical documentation, Substrate documentation structurally exceeds the Annex IV requirement set. High-risk provisions in effect 2 August 2026." No Annex IV documentation is published on the site. No conformity assessment, no high risk classification analysis, no EU declaration of conformity exists in the sitemap. Launch page: "I read the regulations earlier and chose to build harder... I believe TenkaSystem is the answer the frameworks were pointing at", belief presented as compliance posture. |
| 24 | Medium | "Admissible chain of custody" and "third-party verifiable" claims have no verification path for an outsider. | Investors: "Every execution produces admissible chain of custody documentation." Statement of Capability: "Third-party verifiable. The record stands on its own. An outside party can verify it without trusting my systems." No public verification tool, no published keys, no worked example, and no independent verification is linked anywhere on the site. The Absolute Constraints page itself invites the test, "Produce a single action your system took, and let an outside party verify who took it...", but the site provides no means to perform it. |
| 25 | Medium | Colorado AI Act claim is stale and confused in the site's own telling. | Absolute Constraints: "Colorado Artificial Intelligence Act / SB 189, Original act collapsed 27 April 2026, replacement SB 189 effective 1 January 2027. Tracked under Continuous Regulatory Monitoring." The claim is presented without citation, and the site's "Continuous Regulatory Monitoring" function is asserted but has no published output, log, or cadence anywhere on the site. |

### 3.8 AI slop in the legal and content pages, legal dimension

| # | Severity | Slop evidence | Why it matters |
|---|---|---|---|
| 26 | Critical | Contradictory documents, see #1-4, are the strongest slop signal. A human lawyer running a consistency check would have caught the arbitration contradiction, the ping destination contradiction, and the retention contradiction. Three contradictions across four documents dated the same day (May 9, 2026) indicates the documents were generated in parallel, not reviewed together. | Users cannot know which document governs them. In a dispute, the contradiction itself becomes the dispute. |
| 27 | High | Boilerplate that does not match the product: paid tiers, tokens, memberships, and "cross-system memberships" appear in all three legal documents, but no paid tier, token, or membership exists on the site. Privacy Section 4.8: "Users with active paid-tier accounts, token-gated access, or cross-system memberships may be exempted from the AI training data uses described in this Section..." EULA Section 6.7 and Terms Section 8.7 repeat the same invented commercial structure. There is no pricing page, no paid tier, and no token product in the sitemap. The legal documents describe a business the site does not operate. | A user cannot determine what they are agreeing to. Exemption terms that "are described at the point of purchase" (EULA Section 6.7) point at a purchase flow that does not exist. |
| 28 | High | Boilerplate that does not match the operator: U.S. federal government contracting clauses. Terms Section 17.10 and EULA Section 16.8: "The Services are 'commercial computer software' and 'commercial computer software documentation' under FAR 12.212 and DFARS 227.7202. U.S. government end users acquire only those rights set forth in these Terms." This is FAR/DFARS boilerplate copied from enterprise SaaS templates. Tenka is a one person Ohio LLC with no government contracts, no GSA schedule, and no federal customers named anywhere. | Copy pasted federal procurement language in a solo dev installer's EULA signals the documents were not tailored to the actual business. |
| 29 | High | Placeholder style conditional language left in the published policy. Privacy Section 2.4: "Where Tenka publishes a Cookie Policy, it will be available at tenkasystem.com/cookies." "Where Tenka publishes" is template hedging: the policy was published without deciding whether a cookie policy exists. Same pattern in Privacy Sections 5.1 and 8 (sub-processor list "is maintained at" a URL that 404s) and Terms Section 5(e) ("except through Tenka's published responsible disclosure or security research program **(if any)**"). A published legal document should not contain "(if any)" about its own security program. | These are draft stage hedges shipped to production. They tell a reader the documents were never finalised. |
| 30 | High | Wrong entity name in the EULA's operative notice, see #5. "LEGALLY BINDING CONTRACT BETWEEN YOU... AND TENKA SOFTWARE STUDIO". "Tenka Software Studio" is not the registered name, not the trade name, and appears nowhere else on the site. This is the kind of error produced when a template's party name is filled in inconsistently, or when text is generated in separate passes. | The EULA's own notice names the wrong counterparty. If enforceability were ever tested, the misnomer would be exhibit A. |
| 31 | Medium | Generic template artefacts: identical 30 day warranty, $1,000 liability cap, and JAMS arbitration copied across Terms and EULA with only the section numbers changed. EULA Section 10.1 (30 day limited warranty) and Section 11.2 (cap: greater of 12 month fees or $1,000) mirror Terms Sections 12 and 14 almost verbatim. The JAMS arbitration clause is duplicated nearly word for word in Terms Section 13.4 and EULA Section 14.4, including the same Wood County seat and the same US$25,000 threshold. | Not unlawful in itself, but it confirms the documents are template derived and unreviewed as a set. |
| 32 | Medium | Fictional / unverifiable institutional claims presented as fact in content pages that the legal documents then rely on. Launch page: "In December, NIST opened CAISI. In January, the AI Action Plan landed. February brought the executive order on federal procurement." Undated, uncited. Absolute Constraints cites a "Microsoft AI, Code of Conduct for MAI Models (draft, published 14 September 2026...)" and a "NIST NCCoE concept paper on agent identity, February 2026" and "CAISI AI Agent Standards Initiative, February 2026", none linked, none verifiable from the page. | A governance document that cites sources without links is not auditable, which is the standard the document itself demands of others. |
| 33 | Medium | Marketing copy and legal copy describe different products. Marketing (homepage, Launch, Investors) describes autonomous AI "Employees" with identity, lineage, education, and virtues who perform work. The Mutual Accountability Agreement discloses the opposite: "The workforce dialogue you see when operating Tenka is scripted, not generated by a language model reasoning in real time... You are not interacting with autonomous AI." (Section 2.1) The honest disclosure is buried in a legal document most visitors will never open. | The disclosure is good. Its placement is not. The homepage should carry the scripted not autonomous disclosure. |
| 34 | Medium | Data collection with no stated necessity: "cultural origin selection" and pronouns as identity inputs. Privacy Section 2.1 / EULA Section 4.1: installation collects "first name, last name, optional nickname, suffix, pronouns, cultural origin selection" plus city/region/country, to "deterministically generate" an identity hash. No document explains why cultural origin or pronouns are necessary for identity generation. A data minimisation reviewer (GDPR Article 5(1)(c)) would ask why a cryptographic identity needs to know a user's culture. | Collecting sensitive adjacent personal data without a stated necessity is a data minimisation risk, and exactly the kind of collection a regulator questions first. |
| 35 | Low | Stale and inconsistent dates across the legal set. All three core documents are dated May 9, 2026, v1.0, yet they reference events and structures (testing period through July 1, 2026, commercial availability August 1, 2026, "substrate version V0.1.10") that post date them, and the Absolute Constraints document is dated September 17, 2026. No document has been updated since May 9, 2026 despite the site's own timeline moving past every date in them. No "last updated" or change log is published for any legal page. The sitemap confirms the legal pages were last modified 2026-09-29 and not since. | Legal documents that are never updated after their own deadlines pass read as abandoned. |

### 3.9 Disclaimers, copyright, and site wide trust signals

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 36 | High | No general disclaimer page and no footer disclaimer visible in any fetched page. No site wide disclaimer limiting reliance on the site's content, no statement that site content is not legal advice, notable because the site publishes governance and compliance guidance, and no earnings or forward looking disclaimer on the Investors page, see #19. Disclaimers exist only inside the Terms and EULA "as is" clauses, which a casual visitor never sees. | All fetched pages. Sitemap has no disclaimer page. |
| 37 | Medium | No copyright notice found in any fetched page text. No "Copyright Tenka Studio LLC 2026" or equivalent appears in the text extraction of the homepage, legal pages, Investors, Launch, or Absolute Constraints. Footer content is stripped by text fetching, so this is not proven absent. It is absent from every page body the fetcher returns and needs live browser confirmation. | All fetched pages, 2026-11-25. |
| 38 | Medium | Legal pages are not reachable from the content a visitor actually reads, as far as text fetching can show. The sitemap lists the four legal pages, and the documents cross reference each other, but the fetched homepage, Launch, Investors, and Absolute Constraints pages contain no visible footer link text to Privacy or Terms in the extraction. Same caveat as #37: text fetching strips navigation and footer chrome, so the footer links may exist but be invisible to this method. This must be confirmed in a live browser. If the footer does link the legal pages, this finding falls away. If it does not, it is a High finding. Note: the technical pass, checking served HTML directly, did find footer nav links to Privacy, Terms, EULA, Mutual Accountability, Support, How we got here, and Investors. That resolves the link existence question, but not the prominence question. The legal disclosure that matters, scripted not autonomous, is still buried in a legal document. | https://www.tenkasystem.com/, `/launch`, `/investors`, `/absolute-constraint`, text extractions, 2026-11-25. |
| 39 | Medium | No security.txt, no vulnerability disclosure page, no bug bounty or responsible disclosure program, despite Terms Section 5(e) referencing a "published responsible disclosure or security research program (if any)" and the Privacy Policy naming security@tenkasystem.com. | Sitemap. Terms Section 5(e). `/.well-known/security.txt` not checked. |
| 40 | Low | No phone number, no physical street address, no named contact person for the business (only the founder's name) anywhere except a PO Box. Combined with a personal format address on the Investors page (`j.fish38@tenkasystem.com`, with no role based alternative offered there), the contact surface reads as a sole proprietorship, not the governed enterprise the site describes. | https://www.tenkasystem.com/investors ("PO Box 28 Hoytville, OH 43529 j.fish38@tenkasystem.com", note the missing separator and line break in the rendered page). |

### 3.10 Forms and personal data collection on the website

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 41 | High | The installer collects extensive personal data (name, nickname, suffix, pronouns, cultural origin, email, password, username, workspace name, city/region/country) and the Privacy Policy's own description of that collection is the only notice. There is no in flow privacy notice quoted or linked from any website page, and no website form was found in the fetched pages. No contact form, waitlist form, newsletter signup, or lead generation form appears in the text extraction of the homepage, Launch, Investors, or any sitemap page fetched. The "front desk and lead generation" function described on the Launch page (Nina, "lead generation... New prospects, partnership inquiries... come through me") has no visible web form attached to it. Enquiries are routed only by email. | Privacy Section 2.1. Launch page, https://www.tenkasystem.com/launch. Caveat: text fetching strips forms. A form may exist in the rendered page and be invisible here. Live browser confirmation required. Note: the technical pass found the only form on the site is on `/launch`, with no `action` or `method`, see Section 1, M5. That resolves the form question and makes it worse, not better. |
| 42 | Medium | No cookie consent mechanism is evidenced anywhere. The Privacy Policy mentions cookies only to point at a non existent Cookie Policy, see #6. No consent banner, preference centre, or cookie list was visible in any fetched page. Again, a banner is rendered chrome and may exist but be invisible to text fetching. Confirmation required in a live browser. If no banner exists and any non essential cookie is set, that is a GDPR and ePrivacy gap. Note: the technical pass found no cookies are set on any sampled page, which limits the immediate harm, but does not cure the missing policy. | Privacy Section 2.4. All fetched pages. |

### 3.11 Accessibility basics, limited by method

| # | Severity | Finding | Evidence |
|---|---|---|---|
| 43 | Medium | Accessibility cannot be verified from text fetches, and the site gives no accessibility commitment. No accessibility statement exists, see #8. From the fetched text alone: the Investors page image carries alt text in the extraction ("[Image 0: Justin Matthew Fish]"), which is a positive sign, and the legal pages use a clear heading hierarchy in the extraction. But `lang` attribute, actual alt attributes, heading order, contrast, keyboard navigation, and ARIA usage are all unverifiable by this method and must be checked in a live browser or from raw HTML. The absence of any accessibility statement means there is not even a contact route for a user who cannot use the site. | https://www.tenkasystem.com/investors (image alt present in extraction). https://www.tenkasystem.com/accessibility (404). |

### 3.12 Why this means take it offline and rebuild, legal and trust dimension

1. The product is governance. The paperwork is ungoverned. A buyer evaluating TenkaSystem for regulated work will read the legal pages first. They will find two documents that contradict each other on arbitration, two that contradict each other on what data is sent to the backend, and two that contradict each other on deletion. That is not a paperwork nit. It is the product failing its own use case in the first five minutes of diligence.
2. Dead links inside the Privacy Policy are the worst kind of broken promise. The Cookie Policy and sub-processor list are not footer links. They are load bearing cross references inside the document that is supposed to prove GDPR readiness. Both 404. A regulator or enterprise reviewer treats a 404 in a privacy policy as evidence the policy was never operationalised.
3. The site's own dates convict it. The documents promise an EU representative at commercial EU launch on August 1, 2026. It is late November 2026. The representative is still "not appointed." Either the launch happened without the promised safeguard, or the documents are abandoned. Both readings are bad, and both are provable from the site's own text without any external fact.
4. The investor page is an unguarded solicitation. A page titled "Investors" inviting investment contact, with no securities disclaimer, no risk disclosure, and no accredited investor limitation, attached to a company whose registration cannot be verified from the site, is the single highest exposure item on the domain.
5. The compliance claims are not just unproven, one is facially wrong. "Eight compliance frameworks" that include "Financial," "Legal," and "Government" is not a claim awaiting evidence. It is a claim that is false on its face. Everything else in the compliance narrative inherits the doubt that creates.

### 3.13 Minimum rebuild checklist, legal and trust only

- Reconcile all four legal documents in a single consistency pass. Fix the arbitration, ping data, and retention contradictions. Add the Mutual Accountability Agreement to the Terms Section 17.2 precedence list, or state explicitly that it is non binding.
- Publish the Cookie Policy at `/cookies` and the sub-processor list with transfer mechanisms at `/subprocessors`, or remove the references from the Privacy Policy.
- Appoint the GDPR Article 27 EU representative, or stop claiming EU commercial availability, and update Privacy Sections 8 and 15 and Terms Section 2.4 with the representative's identity and contact. Revisit the DPO assessment in writing.
- Add a securities disclaimer block to `/investors` and `/investors/statement` (not an offer, accredited investors only, forward looking statements, risk disclosure) or remove the investor solicitation until counsel drafts it.
- Correct the "eight frameworks" table. Name only real frameworks. Label every certification claim as certified, in progress with auditor named, or aspirational, and link evidence where claimed.
- Publish the Ohio entity number, formation date, registered agent, and a link to the Secretary of State record. Reconcile PO Box 28 with 2596 Maple St.
- Fix the EULA counterparty name ("TENKA SOFTWARE STUDIO"). Remove FAR/DFARS boilerplate or justify it. Remove paid tier, token, and membership provisions until those products exist.
- Add: accessibility statement, contact page, CCPA rights request route, site wide disclaimer (including "not legal advice"), copyright notice, security.txt and disclosure route.
- Put the scripted not autonomous AI disclosure on the homepage, not only in a legal document.
- Have a lawyer review the set as a set. The documents read as generated, not reviewed. The contradictions are the proof.

Contact details verified as present: PO Box 28, Hoytville, OH 43529 (in all legal docs and Investors page). j.fish38@tenkasystem.com (Investors page only). privacy@, legal@, security@, support@, optout@, dmca@tenkasystem.com (in legal docs). No phone number anywhere on the site. No Ohio entity number anywhere on the site. Registration is unverifiable from the site, not proven absent. The Ohio SOS search is form based and needs a live browser check.

---

## 4. UX, SEO, design, and mobile

### 4.1 Navigation: there is effectively no nav menu

| Finding | Severity | Evidence / URL |
|---|---|---|
| No conventional top navigation bar. The header contains only a logo, a "Sign Up" link, and a hamburger style `menu-toggle` button. | Critical | Homepage HTML, `https://www.tenkasystem.com/` |
| The real navigation is a hidden side rail whose items are JavaScript `<button>` elements, not links: System, University, Research Labs, Engineering, Games, Security, Live Construction, Log in. They have no `href`, so they are invisible to search crawlers, unusable with JavaScript off, and unopenable in a new tab. | Critical | Homepage HTML: 29 `<button>` elements, 0 `<form>`, 0 `<input>`. Rail buttons listed above. |
| The footer nav links only 7 destinations: How we got here, Investors, Privacy Policy, Terms of Service, EULA, Mutual Accountability Agreement, Support (a `mailto:` link). | Critical | `https://www.tenkasystem.com/` footer |
| These sitemap pages are not reachable from the footer or header: `/pipeline`, `/workforce`, `/directory`, `/university`, `/research`, `/system`, `/engineering`, `/provenance`, `/kintsugi`, `/construction`, `/demos`, `/absolute-constraint`. They depend on the hidden rail or on in body links. | Major | Sitemap lists 51 URLs. Footer links 7 of them. |
| Rail buttons point at routes that do not exist: "Games" and "Security" have no pages (`/games` and `/security` both return 404). "Log in" has no page (`/login` returns 404). | Critical | `https://www.tenkasystem.com/games`, `/security`, `/login` all returned HTTP 404, re-verified for `/games` and `/security` |
| No site search anywhere. No breadcrumbs on any page checked. No table of contents on the very long pages. | Major | All pages checked |

### 4.2 Homepage structure: no hero that sells, no next step

| Finding | Severity | Evidence / URL |
|---|---|---|
| The H1 is three stacked words, "Credible / Accountable / Employees", with no product name, no outcome, and no audience in it. A first time visitor learns nothing about what to buy or who it is for. | Major | `https://www.tenkasystem.com/`, H1 markup `<h1>Credible<br>Accountable<br><span>Employees</span></h1>` |
| The homepage body is thin: about 616 words including chrome, in three short sections (Lineage, Identity, Education) plus a banner for Kintsugi and Absolute Constraint. There is no product screenshot, no product UI, no workflow diagram, and no customer proof. | Major | `https://www.tenkasystem.com/` |
| The only prominent call to action is "Sign Up", and it is broken, see Section 4.3. There is no "Get started", "Book a demo", "See pricing", "Talk to us", or "Read the docs" anywhere on the homepage. The words "pricing", "get started", and "trial" occur 0 times in the homepage HTML. | Critical | `https://www.tenkasystem.com/` |
| Every page ends with a visible "Additional Details" boilerplate block that reads like a schema dump leaked into the page, for example on the homepage: "Description: Governance infrastructure for AI work. Credible Accountable Employees: verifiable identity, scoped authority, signed accountability." It appears on every page checked and adds nothing a visitor needs. | Major | `https://www.tenkasystem.com/`, `/launch`, `/pipeline`, `/workforce`, `/demos`, `/directory`, `/university`, `/research`, `/system`, `/engineering` |
| The homepage presents 16 third party logos as "Integrations" (Salesforce, HubSpot, Shopify, QuickBooks, Slack, ElevenLabs, Anthropic, OpenAI, Supabase, Cloudflare, GitHub, Vercel, Gmail, Google Drive, Google Docs, Zapier) as `<button>` elements with one line tooltips, no links, no documentation, and no evidence any integration exists. Three are labelled "Coming Soon" (Shopify: "Storefront orders and products. (Coming Soon)", ElevenLabs: "Voice generation. (Coming Soon)", Zapier: "Workflow automation across apps. (Coming Soon)"). Logos are being used to imply capability that is not demonstrated. | Major | `https://www.tenkasystem.com/`, rail integrations block |

### 4.3 Conversion path: there is none

| Finding | Severity | Evidence / URL |
|---|---|---|
| "Sign Up" (`/signup`) does not show a signup form. It returns the homepage verbatim: same title ("TenkaSystem, Governance Infrastructure for AI Work"), same meta description, same about 616 words. The single most important CTA on the site is a dead end. | Critical | `https://www.tenkasystem.com/signup` returns HTTP 200 with homepage content, md5 identical, re-verified |
| There is no signup form, login form, or any form on the homepage at all: 0 `<form>`, 0 `<input>` in the homepage HTML. | Critical | `https://www.tenkasystem.com/` |
| There is no pricing page and no price anywhere. `/pricing` returns 404. The word "pricing" occurs 0 times in the homepage HTML and 0 times in the preloaded JS chunk checked. | Critical | `https://www.tenkasystem.com/pricing` (404, re-verified). Homepage HTML. |
| There is no contact page and no contact form. `/contact` returns 404. The only contact routes are a `mailto:support@tenkasystem.com` link in the footer and `j.fish38@tenkasystem.com` buried in the body of `/investors`. No phone number anywhere. The only postal address found is "Tenka Studio LLC, Registered as Tenka Studio Software Development LLC, PO Box 28, Hoytville, OH 43529" on `/investors`. | Critical | `https://www.tenkasystem.com/contact` (404, re-verified). `https://www.tenkasystem.com/investors` |
| There is no demo to watch or try. `/demos` ("Walkthroughs: TenkaSystem at Work", about 798 words) contains only text about demos, no video, no screenshots, no playable walkthrough, and no booking link. It even undercuts its own premise: "The record is written as the run happens. It is unsigned, unchained." | Major | `https://www.tenkasystem.com/demos` |
| The "Log in" rail button leads nowhere (`/login` is 404), so even an existing customer cannot get in from the public site. The Pipeline page says the console is "Behind login", with no login link. | Critical | `https://www.tenkasystem.com/login` (404, re-verified). `https://www.tenkasystem.com/pipeline` |
| The Investors page is the only page with anything resembling a next step, and its next step is an email address. There is no investor deck download, no contact form, and no meeting link. | Major | `https://www.tenkasystem.com/investors` |

Dead end inventory, all verified by HTTP status: `/signup` (renders homepage, no form), `/login` (404), `/pricing` (404), `/contact` (404), `/games` (404), `/security` (404), any made up slug such as `/nonexistent-page-xyz` (404, with no useful title in the returned body).

### 4.4 Internal linking and orphan pages

| Finding | Severity | Evidence / URL |
|---|---|---|
| The sitemap lists 51 URLs, but the homepage links to only 24 destinations, and most of those are footer or legal links. The product pages (`/pipeline`, `/workforce`, `/system`, `/university`, `/research`, `/engineering`, `/provenance`, `/construction`) have no cross links to each other in their extracted text. Each ends with a single "Back to the main page" link. A visitor who lands on any of them has exactly one way out. | Major | `https://www.tenkasystem.com/pipeline`, `/workforce`, `/system`, `/university`, `/research`, `/engineering` |
| `/pipeline` (about 353 words) and `/workforce` (about 368 words) are stubs. Each states one idea in a few sentences ("Not a catalogue of products. One configurable system", "You can name who did the work") and then stops, with no examples, no screenshots, no pricing, and no CTA. | Major | `https://www.tenkasystem.com/pipeline`, `https://www.tenkasystem.com/workforce` |
| The Employee Directory claims "34 Credible Accountable Employees" but the sitemap lists 32 directory profiles, and two people named in the directory listing have no profile at all: Frederik Daemonshire ("Dean of Education... Instantiation Scheduled") and Isabella Esquema ("Market Analyst... Instantiation Scheduled"). Three more profiles are published with "Role TBD": Noah Gatenberg, Tahia al-Cipheri, Lakshmi Modulananda. Publishing placeholder people on a page whose premise is accountability is a credibility problem. | Major | `https://www.tenkasystem.com/directory`, `https://www.tenkasystem.com/directory/noah-gatenberg`, `/tahia-al-cipheri`, `/lakshmi-modulananda` |
| `/demos` is linked from the homepage ("DEMOS") but is disallowed in `robots.txt` (see Section 4.5), so the site links visitors to a page it tells Google not to index. | Major | `https://www.tenkasystem.com/robots.txt` (`Disallow: /demos`). `https://www.tenkasystem.com/` |

### 4.5 SEO: titles mostly exist, everything else is broken

| Finding | Severity | Evidence / URL |
|---|---|---|
| `site:tenkasystem.com` in search returned 0 results. A branded search for "Tenka System tenkasystem.com" surfaced only about 7 pages (homepage, `/launch`, `/investors`, `/investors/statement`, `/absolute-constraint`, `/directory/dana-ten-ka`, plus a third party listing on mybzz.com). For a 51 URL site, that is near invisibility. | Critical | `browser.search`, Nov 25, 2026 |
| `robots.txt` is present and declares the sitemap, but it disallows `/api/`, `/auth/`, `/system/`, `/thankyou2026`, and `/demos`. Disallowing `/demos` while linking to it from the homepage is self sabotage, and disallowing `/system/` blocks a real content page (`/system` itself is a different path, but the intent is confused). | Major | `https://www.tenkasystem.com/robots.txt` |
| `sitemap.xml` exists with 51 URLs, but it is stale: most `lastmod` dates are Sep 20-29, 2026, about two months before this audit, and several directory entries have no `lastmod` at all (for example `/directory/benjamin-parseau`, `/directory/noah-gatenberg`, `/directory/erik-volrikson`). | Major | `https://www.tenkasystem.com/sitemap.xml` |
| Duplicate meta descriptions are widespread. The identical generic description "TenkaSystem is governance infrastructure for AI work. Credible Accountable Employees: verifiable identity, scoped authority, signed accountability." appears on the homepage, `/signup`, `/demos`, and `/construction`. Across all 51 sitemap pages, the directory description "The register of Tenka's Credible Accountable Employees..." is duplicated 4 times, "Code Engineer" 3 times, and "Senior Code Engineer" twice. Several directory pages have a one or two word meta description ("Code Engineer", "Senior Code Engineer", "Financial", "Real Estate", "Transaction", "Tenka Notary"), which is useless as a search snippet. | Major | All 51 sitemap URLs fetched. Examples: `https://www.tenkasystem.com/directory/erik-volrikson` (desc: "Financial"), `/margerita-titleshaw` ("Real Estate"), `/derryn-sealsiri` ("Transaction") |
| No duplicate `<title>` tags were found across the 51 pages, which is the one SEO basic that is actually done. Titles are, however, inconsistent in style and several are clickbait vague rather than descriptive ("THIS is OUR Stance: Tenka Absolute Constraints", "TenkaSystem: This is where you see it come to life! Live Construction", "See where Accountability leads: Research in Science & Medicine"). | Minor | `https://www.tenkasystem.com/absolute-constraint`, `/construction`, `/research` |
| Open Graph and Twitter cards exist on the homepage and are the strongest SEO element on the site: `og:type` website, `og:title`, `og:description`, `og:url`, `og:image` (`https://www.tenkasystem.com/tenka-torii-card.png`, 1200x630, alt "TenkaSystem"), `twitter:card` summary_large_image with matching title, description, and image. Gaps: no `twitter:site` and no `twitter:creator` on any page checked, and the OG image is the same torii logo mark on every page rather than page specific imagery. | Minor | `https://www.tenkasystem.com/` homepage HTML |
| Structured data exists but is minimal: two JSON-LD blocks on every page checked, an `Organization` (Tenka Studio LLC, founder Justin Matthew Fish, address Wood County, Ohio, US, with the four social profiles as `sameAs`) and a `SoftwareApplication` (TenkaSystem, BusinessApplication, Web). No `Product`, `FAQPage`, `BreadcrumbList`, `Article`, or review markup, and no `offers` or `price` data, because there is no product offer to mark up. | Minor | `https://www.tenkasystem.com/`, `/investors`, `/directory/dana-ten-ka` |
| Canonical tags are present and correct on pages checked, and the bare domain 308 redirects to `www`. That part is done right. | Pass | `https://www.tenkasystem.com/` |
| No keyword targeting is visible anywhere: no page targets an obvious search term (for example "AI governance", "AI employee accountability", "governed AI workforce"), no blog, no FAQ page, and no glossary. The site cannot rank for anything except its own brand name, and it barely ranks for that. | Major | All pages checked |

### 4.6 Thin and duplicate content, page by page word counts

Word counts are approximate visible words including site chrome, from direct fetches:

| Page | Words | Assessment |
|---|---|---|
| `/` (homepage) | about 616 | Thin for a homepage. No product, no proof, no CTA |
| `/signup` | about 616 | Identical to homepage. Not a signup page at all |
| `/pipeline` | about 353 | Stub |
| `/workforce` | about 368 | Stub |
| `/research` | about 505 | Thin. Two labs described in near identical paragraphs |
| `/investors/statement` | about 624 | Mostly repeats `/investors` |
| `/university` | about 684 | Thin. Claims a university, shows no courses, staff, or enrolment path |
| `/engineering` | about 674 | Thin. "Coming Winter 2026 in Beta" for mobile deployment |
| `/system` | about 722 | Thin. Three sections, no diagrams, no specs |
| `/investors` | about 832 | The fullest commercial page, and still no deck, form, or meeting link |
| `/directory` | about 852 | Index page. 32 profiles linked |
| `/directory/*` (32 profiles) | about 650-1,430 each | Templated, see Section 5 |
| `/launch` | about 2,721 | Long narrative, no TOC, no CTA at the end |
| `/absolute-constraint` | about 3,522 | Long governance document |
| `/legal/terms`, `/legal/privacy`, `/legal/eula`, `/legal/mutual-accountability` | about 2,667-3,757 | The longest, most complete pages on the site are the legal pages |
| `/kintsugi` | about 8,944 | Very long single narrative, no TOC, no section nav |
| `/construction` | about 2,384 | Live build log embedded in the homepage shell |

Duplicate content, verified verbatim:

- The founder paragraph "Builder and business owner, thirty years with technology. Founder of Tenka Studio LLC and architect of TenkaSystem, a governed AI workforce in which every participant carries a verifiable identity, a defined role, scoped authority, and signs for its own work. The workforce is built, deployable, and running today." appears word for word on both `/investors` and `/investors/statement`.
- The "Specialized Training for Distribution" paragraph ("White-Label Clients may find themselves in a position where they use one of our Employees in cloud-based work and say hey we really need to have OUR employees built specifically for a purpose...") appears word for word on both `/investors` and `/university`.
- The "AI/ML Hardware Deployment" paragraph ("We do offer hardware for people looking to get involved with the changes come at all of us. We can help establish a rack into the office...") appears word for word on both `/investors` and `/engineering`.

The longest, most complete pages on the site are the legal pages. That is backwards for a site trying to win customers or investors.

### 4.7 Design, typography, readability, images

| Finding | Severity | Evidence / URL |
|---|---|---|
| No broken images were found among the assets checked: OG card image, favicon, torii mark, torii image, founder photo (`/assets/founder/justin-matthew-fish.webp`), sakura emblem, and a directory thumbnail all returned HTTP 200. This is one of the few things that is not broken. | Pass | HEAD checks, Nov 25, 2026 |
| The design is a single dark template repeated on every page: same header banners ("See the Case Study: Kintsugi...", "Introducing our Stance: Tenka's Absolute Constraint"), same side rail, same footer, same "Additional Details" block. There is no visual distinction between a product page, a legal document, a person profile, and a marketing page. | Major | All pages checked |
| Typography depends on Google Fonts (Oswald and Archivo loaded from `fonts.googleapis.com`), an external dependency with no visible fallback story if it fails. The CSS contains very small type: `font-size:.625rem` (10px) and `.688rem` (11px) in places, and `.75rem` (12px) labels in the rail. Small grey on dark labels are a readability risk, especially on mobile. | Major | Homepage HTML and `/assets/index-BUu8c8VT.css` (220,894 chars) |
| Social icons in the footer are icon only links with empty link text in the extracted HTML, so a screen reader gets nothing meaningful from them. The rail torii image has an empty `alt` and `aria-hidden`, which is correct for decoration, but the pattern shows alt text is not being treated consistently. | Minor | `https://www.tenkasystem.com/` |
| The homepage hero image is a torii logo mark (`/tenka-torii-mark.png`, 760x284), not a product image. A visitor sees a logo, not the product. | Major | `https://www.tenkasystem.com/` |
| The site leans on invented jargon with no definitions for a new visitor: substrate, Daemons, bullpen, chain, desks, seats, lineage, Telos, Ethos, Pathos, Logos, Mythos, GOV-LAMBDA, TSAIF, TSACF, CAP. The homepage never defines what a visitor actually gets. | Major | `https://www.tenkasystem.com/` |
| A telling detail in the homepage's own CSS comments: the torii artwork "ships on an opaque grey ground" and needs a radial mask "or a lighter square sits on the glass and reads as a broken image". The team knows the logo asset is defective and masked it in CSS rather than fixing the asset. | Minor | Homepage HTML, inline `<style>` comments |

### 4.8 Mobile responsiveness: code exists, rendering unverified

| Finding | Severity | Evidence / URL |
|---|---|---|
| The viewport meta tag is present and correct (`width=device-width, initial-scale=1.0`), and the stylesheet contains 55 `@media` queries with breakpoints at 520, 560, 640, 700, 900, and 1040px, plus a `prefers-reduced-motion` rule. Responsive CSS has clearly been written. | Pass, code level | `https://www.tenkasystem.com/`, `/assets/index-BUu8c8VT.css` |
| Not verified: no live rendered browser was available to the UX pass, so it could not confirm that pages actually render without horizontal overflow, that the side rail drawer works by touch, that the 10-11px rail text is legible on a phone, or that the 16 logo integration grid and the 32 profile directory grid reflow cleanly. A live browser mobile check is still required before any relaunch claim. | Major, unverified | No rendered screenshots available to this audit |
| Risk indicators for mobile: the side rail is the primary navigation and is hidden behind a hamburger on small screens, so on a phone the visitor starts with no visible way to reach any product page. The directory profile pages are dense field lists (Designation, Pronouns, Cultural origin, Leetspeak'd family name, Genesis hash) that will be long vertical scrolls. The long pages (`/kintsugi` at about 8,944 words, `/absolute-constraint` at about 3,522) have no in page navigation, which is punishing on a phone. | Major | `https://www.tenkasystem.com/directory/dana-ten-ka`, `/kintsugi`, `/absolute-constraint` |

### 4.9 What has to be fixed before this site should be online, UX and SEO only

1. Build a real conversion path or take the site down. A working signup form, a working login, a contact form, and a pricing page are the minimum. Right now `/signup` serves the homepage, `/login`, `/pricing`, and `/contact` are all 404, and there is not one `<form>` on the homepage.
2. Build a real navigation menu with text links to every product page, and delete or finish the dead rail items (Games, Security, Log in). No primary navigation should depend on JavaScript buttons with no `href`.
3. Fix the SEO basics: unique meta descriptions on all 51 pages (especially the one word directory descriptions and the four pages sharing the homepage description), fresh `lastmod` dates in the sitemap, remove `/demos` from `robots.txt` or stop linking to it, and create content that targets real search terms. The `site:` query returning 0 results is the headline symptom.
4. Rewrite or remove the thin pages. `/pipeline` (353 words) and `/workforce` (368 words) need real content, screenshots, and a CTA, or they should not be published. Every product page needs a next step other than "Back to the main page".
5. Deal with the directory honestly. Either remove the fictional employee register, or label it plainly as fictional or conceptual, remove the "Role TBD" and "Instantiation Scheduled" placeholders, and stop presenting invented credentials as a real workforce. As published, it directly contradicts the site's claim of accountability.
6. Remove or substantiate the slop. Delete the "Additional Details" schema dump from visible page content, stop repeating the same paragraph across `/investors`, `/investors/statement`, `/university`, and `/engineering`, replace "Coming Soon" integration logos with real integrations or remove them, and rewrite the templated directory bios so each one says something specific and checkable.
7. Verify mobile by rendering it. The viewport tag and 55 media queries exist in code, but no rendered mobile check was possible in this audit. Check the hamburger rail, the 10-11px labels, the integration grid, and the directory grids on a real phone before relaunch.
8. Add the missing trust content: a real demo (video or interactive), product screenshots, a named contact person with a working form, and one checkable customer or pilot result. Currently the longest, most complete pages on the site are the legal pages, which is backwards for a site trying to win customers or investors.

### 4.10 Social links

| Finding | Severity | Evidence / URL |
|---|---|---|
| Four social profiles are linked in the footer: Facebook (`facebook.com/profile.php?id=61572560981944`), YouTube (`youtube.com/@tenkastudio38`), X (`x.com/TenkaStudio38`), LinkedIn (`linkedin.com/in/tenka-studio-1a6738403/`). Facebook, YouTube, and X returned HTTP 200 on a HEAD request. LinkedIn returned 405 (method not allowed) in the UX pass and 999 (bot block) in the technical pass, which is inconclusive rather than proof of a dead link. All four are also listed in the JSON-LD `sameAs`. | Pass with caveat | `https://www.tenkasystem.com/` footer and JSON-LD |
| The social links are icon only with no text label, no follower counts, no recent post preview, and no sharing buttons on any article or case study page. A visitor cannot tell whether the accounts are active without leaving the site. | Minor | `https://www.tenkasystem.com/` |
| No `twitter:site` or `twitter:creator` handle is declared, so shared links do not attribute to an account. | Minor | Homepage HTML |


---

## 5. AI slop: code and content

This section is per the explicit instruction to name AI slop where the evidence supports it. This is not a judgment about who wrote the site. It is what the served files prove about how the site was built: bulk generated from templates with names and a few fields swapped, with generated filler shipped unedited.

### 5.1 Code slop: every page is the same 254 line template

- `home.html`, `signup.html`, and all six sampled directory profiles are exactly 254 lines: `dana-ten-ka.html`, `nina-tokenford.html`, `ren-mishimura.html`, `wei-chen.html`, `claude-anthrobearer.html`, `richard-helm.html`, each 254 lines, 31,132 to 36,740 bytes. The homepage is 254 lines. The signup page is 254 lines. Same line count across unrelated pages is a template signature, and the md5 proof in Section 1, B2, shows three of them are literally the same file.
- All 32 directory profiles share an identical 9 section skeleton. All 9 section labels appear on 6 of 6 sampled profiles: `Ethos`, `Telos`, `Logos`, `Pathos`, `Cultural foundation`, `Inviolables`, `Inspirations`, `Vocal and writing tone`, `Where the authority ends`. The only variation is the swapped name, role, pronouns, and a generated bio paragraph per section.
- Every profile page carries the identical footer, integrations, and dashboard block, including the identical ticket IDs `TT-A0000109` and `TT-A0000091` and identical snapshot counts, see Section 1, B6. The "personal" profile content is a thin variable layer over a fixed slab.
- Text similarity (difflib ratio on stripped visible text): Dana vs Nina 0.327, Dana vs Wei Chen 0.302, Nina vs Wei Chen 0.331. Low because the generated bios differ, but the structure, labels, footer, and integrations blocks are byte identical.
- The same sentence is printed three times in a row on every page. Verbatim, on the homepage, `/signup`, `/demos`, and every sampled directory profile: "Google Workspace Integrations include Calendar, Contacts, Maps and more." appears 3 times consecutively (count: 3 on `home.html`, 3 on `dirpages/dana-ten-ka.html`, 3 on `dirpages/nina-tokenford.html`). This is a duplicated list item shipped three times, a generation artifact nobody read.
- Bulk generated sitemap for thin and empty pages. 51 sitemap URLs. 23 directory entries have no `<lastmod>`. All lastmods are September 20-29, 2026 while pages were modified November 18-21. The sitemap was generated once when the directory was bulk created and never updated. 5 of 7 demo detail pages are byte identical homepage shells, Section 1, B3, sitemap scale generation of pages with no content behind them.
- Placeholder and dead content shipped as finished. Social config in the bundle: Instagram `active:false, url:null`, Discord `active:false, url:null`, tooltip `Coming...`. Integrations footer: "(Coming Soon)" on Voice generation, Storefront orders, Workflow automation, on every page. Demo numbering with a missing Demo 05, presented out of order. `/demos/laksamana` live text: "The scenarios could not be loaded", a broken state shipped as a page.
- Generated meta descriptions and backronyms. 5 pages share one default description verbatim, Section 1, M9. 4 pages have backronym descriptions ("Credentialed Education Structured for Confidence through Experience", etc.). Both are classic bulk generation defaults: one fallback string pasted everywhere, and acronym expansion filler where a human description was never written.
- Unused and vestigial code shipped. `chunk-B3K2TuZy.js` is 558 bytes and is `modulepreload`ed on the homepage, a vestigial chunk. Invisible buttons (`opacity: 0; pointer-events: none`) for `TenkaSec`, `Security Dashboard`, `Systems Dashboard`, `Sign In`, `Roster` are rendered in `/launch` HTML, dead UI shipped in production markup. The full auth, WebAuthn, and admin route map is in the public bundle, Section 1, B4, code for an entire authenticated app shipped to anonymous visitors.
- Inline styles on every element in `/launch` (React inline `style="..."` attributes throughout), 103,215 bytes in 75 lines. Unmaintainable generated markup.
- Templated naming and content tells. Directory names are visibly generated riffs on real world patterns: `Claude Anthrobearer` (a literal "Claude" employee), `Officer Richard "Dick" Helm`, `Oracle Queryades`, `Caio Loopinho`, `Dylen Synergistios`, `Hamesh Ibn-Channel`, `Cornelius Van Der Sluis III`. Portrait files follow a generated naming scheme (`tsa-x0002-nina.png`, `tsaif-0001-dana.png`, `tsec-x0019-officer-dick.png`, `tsam-x0016-mikhael.png`), consistent with bulk asset generation, and at 441 KB to 1 MB each they were never optimized, Section 1, M4.

### 5.2 Content slop: templated, fill in the blank bios

Every directory bio follows the same character sheet template: name plus leetspeak surname plus 8 hex Genesis plus origin plus "He exists to..." plus "He will not..." inviolables plus hobbies plus a "Register:" voice description. DaNA's page is the purest example. Sections literally labeled **Ethos** ("what they hold to"), **Telos** ("what they are oriented toward"), **Logos** ("how they reason"), **Pathos** ("what moves them"), **Stance**, **Inviolables**, **Inspirations**, **Vocal and writing tone**, https://www.tenkasystem.com/directory/dana-ten-ka. That is a role play character sheet, not a biography. The Kintsugi bios repeat the same beats (origin, degrees, "two / three / four things he will not do," hobby, register) with only names and flavor swapped.

Measured across all 32 sitemap directory URLs:

| Repeated element | Pages containing it |
|---|---|
| "Where the authority ends" section | 32 of 32 |
| "Domain" section | 32 of 32 |
| "Cultural origin" field | 32 of 32 |
| "Genesis" hash field | 32 of 32 |
| "Leetspeak'd family name" field (for example "To-ken-for)", "Mi-shi-mur-a", "Man-sf-iel)", "Tr-a-ce-nko", "Fa-le-o-la", "Br-a-nc-be-rg", "Pa-rse-a-u", "Ga-ten-ber-g", "Vo-lri-kson", "Ti-tle-sha-w") | 30 of 32 |
| Opening sentence "Credible Accountable Employee of the TSA-X lineage" | 24 of 32 |
| "He exists to..." / "She exists to..." / "They exist to..." | 31 of 32 combined (18 plus 10 plus 3) |
| Meta description duplicated from the directory index or a bare job title | 9 pages, see Section 4.5 |

Exact quotes, all from live pages:

- Erik Volrikson, `https://www.tenkasystem.com/directory/erik-volrikson`: "Erik Volrikson, Icelandic-European Credible Accountable Employee of the TSA-X lineage, seated at Financial. He is the one who reconciles to the cent and is unbothered by how long it takes. He exists to keep the numbers true, to ensure that what the books say happened is what actually happened."
- Noah Gatenberg, `https://www.tenkasystem.com/directory/noah-gatenberg`: "Noah Gatenberg, German American Credible Accountable Employee of the TSA-X lineage. He is the one who reads the whole procedure before touching the first step, and who keeps the records that let everyone else move fast. He exists to make process legible and repeatable, so that what worked once can be done again on purpose, not by luck."
- Lakshmi Modulananda, `https://www.tenkasystem.com/directory/lakshmi-modulananda`: "Lakshmi Modulananda, Hindu Indian Credible Accountable Employee of the TSA-X lineage. She sees people and process as a system to be tended, who's doing what, whether it's sustainable, and how the parts fit the whole. She exists to keep the human side of operations healthy..."
- Margerita Titleshaw, `https://www.tenkasystem.com/directory/margerita-titleshaw`: "Margerita 'Rita' Titleshaw, Scottish-European Credible Accountable Employee of the TSA-X lineage, seated at Real Estate. She knows that a property is a story about title, boundary, and obligation, and she reads the whole story before anyone signs. She exists to make property transactions sound..."
- Derryn Sealsiri, `https://www.tenkasystem.com/directory/derryn-sealsiri`: "Derryn Sealsiri, African American and Thai Credible Accountable Employee of the TSA-X lineage, seated at Transaction. They are the one who makes the handoff actually happen, the moving parts that have to land in order, on time, in sync. They exist to carry transactions cleanly from intent to completion..."
- Eleanor Draftland, `https://www.tenkasystem.com/directory/eleanor-draftland`: "Eleanor Draftland, American-European Credible Accountable Employee of the TSA-X lineage. She is the builder of first versions, the one who turns a vague intent into a concrete draft you can actually argue with. She exists to produce the working draft..."

The pattern is mechanical: "[Name], [ethnicity] Credible Accountable Employee of the TSA-X lineage. He / She / They is the one who [generic virtue]. He / She / They exists to [generic mission]." Swap the ethnicity, the job title, and the virtue, and you have the next page. These are fictional people with invented credentials (EMBA, MSDS, DBA, MEng, MSW, BAS, MCM, MSIS, EdD, JD, NNA, LL.M, SHRM-CP, CPA, CISSP, TSEC, PMP, CAP) presented as a real workforce register, which is the opposite of the site's own accountability premise.

### 5.3 Content slop: purple prose and mythic language where evidence should be

- "With frontier memory layers, instilled virtues, trained education, identity, and lineage tied to real world cultural origins." Homepage. Every noun is grand. None is defined or demonstrated.
- "The DNA of the system isn't metaphor. It's the schema." `/launch`. It is, in fact, a metaphor. No schema is published.
- "DaNA embodies that: the first node on the connected path." / "the originator's calm" / "gold in the seams and all", mythic register used to avoid saying anything checkable.
- "I share the founder's lineage, and the lineage of the founder of the AI I was built from, as a thanks." Luka, `/launch`. Incoherent. And the follow up refuses to explain: "it's the only framing that needs to be on this page. The rest is in the substrate. Read the chain." The chain is not available to read.
- "I don't ship adjective slurry. I don't ship deck language pretending to be product language." Isabella, `/launch`. The site then ships nothing but adjective slurry and deck language for 700 plus lines.
- "The game never shipped, yet. Tenka did." `/investors`. The origin story is an unnamed, unshipped game. No title, no date, no link.

### 5.4 Content slop: repetitive slogans, grammar slips, word salad

The site is saturated with em dashes in the original, and the same slogans recur verbatim on every page: "Every decision documented. Every Employee accountable. Every output signed." / "Every action carries a verifiable identity, a defined role, and a signed record." / "The Employees are credentialed. The actions are signed. The records are chained. The chains are verifiable." Every page carries the identical meta description: "Governance infrastructure for AI work. Credible Accountable Employees: verifiable identity, scoped authority, signed accountability." That is boilerplate repeated across 10 plus pages, not writing.

Tells, with exact quotes:

- "Not tools. Not assistants. Not even Trusted Digital Workers... We prefer; Credible Accountable Employees." `/launch`. Semicolon misuse, a classic generation slip.
- "SENTINEL Security: A specially developed security layer that adapts to deploy defense." `/investors`. "Adapts to deploy defense" is word salad. It means nothing.
- "Extensible Integrations: A credential-scoped pool of callable systems and APIs, QuickBooks, Salesforce, Shopify, Meta, Google, Microsoft, added as the work requires them." `/investors`. Six platform names dropped. Zero integrations shown, zero screenshots, zero docs.
- "The roadmap from here is institutional. TenkaSystem grows by phases, each phase activates publicly when it's ready to operate publicly", `/launch`. A roadmap with no dates, no phases, no milestones.
- "That observation is offered as an observation, not a benchmark; it is noted here because it was noticed, and it is being measured properly going forward." `/kintsugi`. An explicit admission that the efficiency claim in the case study is unmeasured.
- "Bullpen released, pool nine, all free." `/kintsugi`, Appendix A. Undefined jargon presented as a closing record.

### 5.5 Content slop: invented frameworks with acronyms and no definitions

TSAIF, TSACF (never defined anywhere, appears only in the MyBzz listing title "Tenka System - TSAIF/TSACF"), GOV-LAMBDA, DaNA, bullpen, substrate, Daemon, Tenka Chain, Genesis hash, SENTINEL, MISSL ("Machine Interpreted Systemic Sign Language", defined only in passing on Officer Helm's bio), "two-touchpoint integration," "two-vocabulary discipline," "Substrate Risk Stratum," "Attested Decision Point," "Governed Autonomy," "Chain Acknowledgment," "Genesis Anchor," "Substrate Stress Verification," "Tenka Substrate Conformance," "C2PA content-provenance fold-in." None is defined on the site. The specification documents that would define them are referenced but not published, Section 2.2, finding 2.5.

### 5.6 Content slop: "verbatim" that is not verbatim

Kintsugi Appendix A is introduced as "the unedited close of the session" and "Presented as recorded," while simultaneously admitting it is "lightly glossed for confidentiality". Verbatim and glossed are mutually exclusive. Inside the supposedly verbatim quote: "Make it with a section of personal reflection. [...] This isn't about anyone else but you." An ellipsis inside a verbatim transcript, with no note of what was removed. The dialogue itself reads as scripted LLM chat ("ha you tried to correct me, but if we let Tai speak for himself, he will tell you his hobbies put him in similar frame with Kintsugi as his hobby").

### 5.7 Content slop: filler sections that say nothing specific

- Research, `https://www.tenkasystem.com/research`: "A specially organized set of process guardrails to help with scientific research. No liberties are taken without authorization, since founder approvals are part of every process with our workforce. This is the system to solve problems which are complex and require advanced minds and specialized development of concepts." The Medical Research Lab paragraph is the same paragraph with "medicine" substituted: "Same conceptual function as the Science Research Lab but specifically developed for medicine."
- Research, same page: "With hundreds of available API tools, and various systems already available to our Employees, their efficiency, and confidence is high. Add the backing of credentialed Tenka University Education and further training, we have a team capable of amazing work." No tool is named, no result is cited, no paper, trial, or dataset is referenced.
- University, `https://www.tenkasystem.com/university`: "Nine Schools: Business & Management, Social & Behavioral Sciences, Arts & Humanities, Communications and Information, Engineering & Applied Sciences, Natural Sciences & Mathematics, Governance Education & Professional Studies (BUILT FOR 'FRONTIER AI')." No courses, no faculty list beyond the directory, no syllabus, no enrolment route. The page also claims "Colleges of Law & Medicine are actually our two deepest curriculums right now" and that the system is "capable of standing up in court", with no evidence offered.
- System, `https://www.tenkasystem.com/system`: "Need a huge project ready for the board meeting on Friday? This is the team who can get it done. If you have a project, bring it here." That is the entire Bullpen pitch. The Monitoring section is: "Here is our other big deployment use for our Employees. Users will be able to assign Employees to monitoring functions."
- Engineering, `https://www.tenkasystem.com/engineering`: "We are currently developing concepts that convert other forms of existing infrastructure into data processing facilities. We are always looking for partners who want to deploy systems like ours into the cloud. Its not a matter of how with Tenka Engineering, it's a matter of when." And: "With natural gas turbines, solar modulation, environment manipulation, NMA (Near-Memory Architecture)." A list of buzzwords with no project, no site, no specification.
- Homepage, `https://www.tenkasystem.com/`: "With frontier memory layers, instilled virtues, trained education, identity, and lineage tied to real world cultural origins. Our Employees are capable of the kinds of work that most people wouldn't trust with current 'AI' tools." No example of such work is given on the homepage.
- `/launch` narrates the product in the voices of fictional employees ("I'm Marcus. I run operations here." "Hi, I'm Nina. I run the front desk..." "I'm Officer Dick. Security and audit." "Dick had to hop on the skateboard. As usual. So I'll pick it up from here, hi, I'm the surgeon behind the systems. Ren."), which reads as generated persona fiction, not a company story.

### 5.8 Content slop: placeholder states published live

Three directory entries with role "TBD". Two marked "Instantiation Scheduled." Three integrations "Coming Soon." "Coming Winter 2026 in Beta" for mobile deployment, `/engineering`. Demo 05 missing. "The scenarios could not be loaded" shipped as a page. These are not roadmap notes in a draft. They are live pages presented as a finished workforce, university, and demo suite.

---

## 6. What is actually sound, stated for honesty

An audit that only lists failures is not credible. These items were verified sound, and they should be kept in the rebuild:

1. **HTTPS enforced.** Both `http://` variants 301 to HTTPS. Apex 308s to `www`.
2. **TLS 1.3 negotiated successfully.**
3. **Static page security headers are genuinely good.** CSP (with `frame-ancestors 'none'`, `object-src 'none'`, `base-uri 'self'`, `form-action 'self'`, `upgrade-insecure-requests`), `X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy` disabling geolocation, microphone, camera, payment, and usb. Present identically on `/`, `/launch`, `/investors`, `/directory/dana-ten-ka`.
4. **No cookies are set on any sampled page** (`curl -c` cookie file empty).
5. **No mixed content in sampled pages.**
6. **No source maps shipped** (`index-CY98FUGS.js.map` and `index-BUu8c8VT.css.map` both 404).
7. **404 handling returns a real 404 status**, not a soft 200, except where the SPA shell intentionally serves routes, Sections 1, B2 and B3.
8. **Auth gating works where tested.** `/api/identity/me` 401s, `/api/admin/luka/work` 403s unauthenticated.
9. **All 51 sitemap URLs return 200. All 32 directory portraits return 200.** No dead links or broken images in the sitemap set.
10. **All homepage assets return 200** (JS, CSS, images, 17 integration SVGs and PNGs).
11. **No `x-powered-by` header. No stack fingerprint on static pages.** The fingerprinting comes from the API, Section 1, M10.
12. **Canonical tags are present and correct on pages checked, and the bare domain 308 redirects to `www`.**
13. **No duplicate `<title>` tags were found across the 51 pages.** That is the one SEO basic that is actually done, even if the titles themselves are inconsistent in style.
14. **Open Graph and Twitter cards exist on the homepage** and are the strongest SEO element on the site, Section 4.5.
15. **The legal documents exist and are unusually long and detailed for a one person operation.** GDPR legal bases, data subject rights, CCPA/CPRA section, age gate at 16, DMCA agent, AI training disclosure and opt out route, retention schedule, and breach contact are all present. The Mutual Accountability Agreement's disclosure that the workforce dialogue is scripted is good practice and rare. The failure is that the set contradicts itself and is partly dead linked, not that it is absent.
16. **No broken images were found among the assets checked** in the UX pass: OG card image, favicon, torii mark, torii image, founder photo, sakura emblem, and a directory thumbnail all returned HTTP 200.
17. **The demos page admits its demos are fictional.** "Fictional business, real regulations. The business is invented." That honesty is creditable. It does not cure the fact that the demo detail pages behind it are homepage shells.

None of these passes compensates for the blockers. Good headers do not make a broken signup flow work. A correct canonical tag does not make a fictional workforce real. A long privacy policy does not cure a 404 inside that policy.

---

## 7. Why take it offline, not patch it live

Patching live would mean fixing one 404 while the homepage still sells a product the legal pages say does not exist, while `/api/health` still broadcasts backend internals, while `/signup` still serves the homepage, and while the investor page still solicits without a disclaimer. Each fix would leave the next blocker live, and each live fix would be seen by the next visitor, investor, regulator, or buyer doing diligence in the meantime.

The failures are systemic, not isolated:

- **The product claim and the legal disclosure cannot both stay live.** One of them has to change, and the change is not a wording tweak. Either the marketing stops claiming an autonomous, cryptographically governed workforce running today, or the product actually becomes one, with a verifiable key, chain, demo, and customer. Until that decision is made and implemented, every day online is another day the public claim and the legal fine print contradict each other.
- **The backend is exposed by design, not by accident.** The health endpoint, the bundle route map, the Railway host, the Express error pages, and the inconsistent CSP are not one leak. They are a backend that was never given a public boundary. Closing one endpoint leaves the bundle, the headers, and the error pages still fingerprinting the stack.
- **The site has no conversion path to patch.** Signup, login, pricing, contact, and demos are not broken forms. They are absent forms, 404s, and homepage shells. There is nothing to fix in place. The flows have to be built.
- **The content is bulk generated.** 32 profiles from one 254 line template, 5 pages sharing one meta description, 3 pages sharing one founder paragraph, triplicated sentences, backronym descriptions, and placeholder roles published live. Editing one bio does not fix a generation pipeline that produced 32 identical ones.
- **The legal set was generated in parallel, not reviewed as a set.** Three contradictions across four documents dated the same day, a wrong counterparty name, FAR/DFARS boilerplate for a one person LLC, paid tier provisions for a product with no paid tiers, and draft hedges ("Where Tenka publishes...", "(if any)") shipped to production. A consistency pass by a lawyer is required, not a find and replace.
- **Search visibility is near zero.** `site:tenkasystem.com` returned 0 results. A branded search surfaced about 7 of 51 pages. The site is not being found, and the pages it does have are thin, duplicated, and orphaned from navigation. Relaunching the same structure with new copy will not fix discoverability. The information architecture has to be rebuilt: real nav with real links, unique metadata, fresh sitemap, and content that targets real search terms.

Take it offline. Put up a single holding page with a real contact route, or take the domain to a maintenance state with no product claims, no investor solicitation, and no data collection. Rebuild behind that holding page. Relaunch only when every gate in Section 8 passes.

---

## 8. Rebuild checklist and relaunch gates

Every item below is a gate, not a suggestion. A gate fails if the evidence for it cannot be shown on the live site. Do not relaunch until all gates pass.

### 8.1 Product truth gates

- [ ] **G1. One product story, everywhere.** The homepage, Launch, Investors, Statement, and Mutual Accountability Agreement all describe the same product. If the workforce dialogue is scripted, the homepage says so in plain language. If it is autonomous, the scripted disclosure is removed and replaced with a working demo that proves autonomy. No page claims a feature that another page says does not exist yet.
- [ ] **G2. One verifiable cryptographic artifact.** Publish at least one of: a public key with its algorithm and issuer, one complete signed chain entry with hash, predecessor, and signature that an outsider can verify without your infrastructure, or a chain explorer with a worked example. An 8 character hex string is not a hash. A 7 character "proof-marker" is not proof.
- [ ] **G3. One working demo.** At least one demo that loads, runs, and shows a signed, chained record being produced, or the demo section is removed entirely. "The scenarios could not be loaded" must never ship. Demo detail pages must be real pages, not homepage shells. Demo numbering must be complete and in order.
- [ ] **G4. One named customer, pilot, or deployment, or no customer claims.** Remove "running today", "deployable", "dozens", and "34 Employees" until a named deployment exists and can be checked. If the workforce is fictional, label it fictional on the directory, homepage, and investors page, and remove invented credentials, or remove the directory.
- [ ] **G5. Pipeline count fixed.** Six or seven stages, one number, on every page. Same for the framework name (Autonomous or Automaton, pick one), the employee name (Credible or Credentialed, pick one), and the entity name (one registered name, one trade name, used consistently).
- [ ] **G6. Cloud dependence stated honestly.** Remove "no dependence on an outside cloud" or remove Supabase, Railway, Vercel, SendGrid, Claude Code, and Google Fonts from the actual stack. The Terms Section 11 list and the marketing claim must agree.
- [ ] **G7. Incident record completed or removed.** Either publish a real postmortem for May 5, 2026 (what was lost, what was recovered, RTO and RPO, what changed), with the "Operational Precedents" document linked, or remove the incident claim. Do not publish "live and unfixed" unauthenticated data deletion paths in a governance product. Fix them first, then publish the postmortem.

### 8.2 Technical and security gates

- [ ] **G8. `/api/health` removed from public access or replaced.** No Redis state, port, startup timestamps, uptime, crash classification, watchdog state, or `health.status: "unknown"` is served unauthenticated. Health reporting must return a real status, and the watchdog must have recorded a successful response.
- [ ] **G9. Backend boundary built.** The public JS bundle contains no admin or auth route map, no backend host, and no third party endpoint list that is not needed by the client. API error pages do not leak Express or Railway. CSP is one policy, applied consistently to static and API responses, with no `'unsafe-inline'` on the API and no weaker `frame-ancestors` there.
- [ ] **G10. Signup, login, pricing, contact, and thank you are real flows.** `/signup` serves a form with an `action` and a `method`, and a working submission path that can be shown end to end. `/login` serves a login page. `/pricing` serves prices. `/contact` serves a form or a monitored contact route. `/thankyou2026` serves a confirmation for a real submission, or it is removed. No public route serves byte identical homepage HTML.
- [ ] **G11. Navigation is real links.** Every primary nav item is an `<a href>` to a page that returns 200. No nav item points to a 404. No primary navigation depends on JavaScript buttons with no `href`. A visitor with JavaScript off can reach every product page. A crawler can follow the nav.
- [ ] **G12. One build, everywhere.** CDN edges serve the same `Last-Modified` and the same content. Cache headers are consistent: if `max-age=0, must-revalidate` is set, content is not served stale for 6.4 days.
- [ ] **G13. Internal data removed from public pages.** No ticket IDs, ticket titles, snapshot counts, dashboard blocks, or internal app shell text is shipped to anonymous visitors.
- [ ] **G14. HSTS and CORS fixed.** HSTS is `max-age=31536000` (or longer) with `includeSubDomains` and `preload`, or the reason for a shorter value is documented. The wildcard `access-control-allow-origin: *` on static pages and credentialed CORS on the API are proven safe together, or the wildcard is removed.
- [ ] **G15. Email authentication completed.** DMARC record published (at least `p=none` with reporting, `p=quarantine` or `reject` preferred). SPF authorizes the actual sending provider, matching MX. The lead capture endpoint, its destination, and its privacy notice are stated on the form itself.
- [ ] **G16. Performance budget met.** Homepage first load under 1 MB, decorative images optimized, directory portraits resized and compressed (not 441 KB to 1 MB each), Google Fonts self hosted or with a documented fallback. No vestigial chunks, no invisible buttons, no dead social configs shipped.
- [ ] **G17. `/favicon.ico` returns 200.** And every default request a browser makes is tested.
- [ ] **G18. CAA record published.** DNSSEC status verified and recorded.

### 8.3 Legal and trust gates

- [ ] **G19. Legal set reconciled by a lawyer, as a set.** Arbitration, ping data, and retention contradictions fixed. Mutual Accountability Agreement added to the Terms Section 17.2 precedence list, or stated explicitly as non binding. One entity name, used everywhere. EULA counterparty corrected. FAR/DFARS boilerplate removed or justified. Paid tier, token, and membership provisions removed until those products exist. Draft hedges removed.
- [ ] **G20. Cookie Policy and sub-processor list published, or references removed.** `/cookies` returns 200 with named cookies, purposes, and durations, and a consent mechanism. `/subprocessors` returns 200 with the current list and the transfer mechanism for each. No Privacy Policy cross reference 404s.
- [ ] **G21. EU representative appointed, or EU commercial availability withdrawn.** GDPR Article 27 representative named with identity and contact. Privacy Sections 8 and 15 and Terms Section 2.4 updated. DPO assessment revisited in writing. No document promises a date that has already passed.
- [ ] **G22. Investor page guarded or removed.** Securities disclaimer block on `/investors` and `/investors/statement` (not an offer, accredited investors only, forward looking statements, risk disclosure), or the solicitation is removed until counsel drafts it. No unqualified "admissible chain of custody" claim without evidence.
- [ ] **G23. Compliance table corrected.** Only real frameworks named. Every certification claim labelled certified (with auditor and date linked), in progress (with auditor named), or aspirational. No "Financial", "Legal", or "Government" as frameworks. No Annex IV, HIPAA, SOC 2, ISO 42001, or FDA Part 11 claim without the underlying document or assessment linked.
- [ ] **G24. Company identity published.** Ohio entity number, formation date, registered agent, and a link to the Secretary of State record, on one company page. PO Box 28 reconciled with 2596 Maple St. One contact route that is monitored, with a named person or role, not only a personal `j.fish38@` address.
- [ ] **G25. Trust pages added.** Accessibility statement with a contact route, contact page with a working form, CCPA rights request route, site wide disclaimer including "not legal advice", copyright notice, security.txt and a vulnerability disclosure route.
- [ ] **G26. Data minimisation justified.** Why cultural origin and pronouns are collected for identity generation is stated, or they are not collected. AI training opt out and deletion rights are reconciled in writing. PIPEDA and Quebec Law 25 are addressed or a geographic limitation is stated.

### 8.4 UX, SEO, and content gates

- [ ] **G27. Homepage sells the product in one screen.** H1 names the product, the audience, and the outcome. One product screenshot or workflow diagram. One primary CTA that works. The words pricing, demo, and contact appear on the homepage with working links.
- [ ] **G28. Every product page is real content.** No stub under 500 words is published as a product page. Every product page has an example, a screenshot or diagram, and a next step other than "Back to the main page". Cross links between product pages exist.
- [ ] **G29. SEO basics fixed.** Unique title and unique meta description on all 51 pages, no one word descriptions, no shared fallback description. Sitemap `lastmod` dates are current and complete, no entry lacks a `lastmod`. robots.txt does not disallow a page that is linked from the homepage. `site:tenkasystem.com` returns results. At least one page targets each core term the business wants to rank for, with a blog, FAQ, or glossary to support it.
- [ ] **G30. AI slop removed.** No 254 line template with names swapped. No triplicated sentences. No "Additional Details" schema dump in visible page content. No verbatim duplicated paragraphs across pages. No backronym meta descriptions. No "Coming Soon" logos presented as integrations. No placeholder roles ("TBD", "Instantiation Scheduled") published live. Each directory profile, if kept, says something specific and checkable, or the directory is removed.
- [ ] **G31. Mobile verified by rendering.** Rendered screenshots on a real phone, no horizontal overflow, side rail drawer works by touch, 10-11px labels replaced with legible sizes, integration and directory grids reflow cleanly, long pages have in page navigation. The check is recorded, not asserted.
- [ ] **G32. Trust content added.** A real demo (video or interactive), product screenshots, a named contact person with a working form, and one checkable customer or pilot result. The longest pages on the site are no longer the legal pages.

### 8.5 Relaunch process gate

- [ ] **G33. Independent re-audit before relaunch.** A fresh auditor, with no prior involvement in the rebuild, runs the same four passes (technical, content, legal, UX/SEO) against the holding page replacement and confirms every gate above passes, with evidence. The re-audit report is kept on file. No gate is waived by the builder. Any gate that cannot be evidenced is a failed gate.

---

## Appendix A. Method and verification log

- **Lead auditor direct checks, November 25, 03:48 EST:** Homepage HEAD, www redirect, http redirect, robots.txt, sitemap.xml, 404 handling. Sitemap: 51 URLs. Robots: disallows `/api/`, `/auth/`, `/system/`, `/thankyou2026`, `/demos`.
- **Lead auditor re-verification, after the four specialist audits:** `/api/health` body matches Section 1, B1 verbatim, including `health.status: "unknown"`, `failureClassification: "unknown_runtime_crash"`, `watchdog.lastResponseMs: null`, `lastOkAt: null`. md5 identical for `/`, `/signup`, `/thankyou2026`, `/demos/ampfield`: `bdafc46831fb1c589749b95689e9f487`. `/cookies`, `/subprocessors`, `/contact`, `/pricing`, `/login`, `/games`, `/security`, `/favicon.ico`: all 404, 6,077 bytes. `/demos/laksamana`: "The scenarios could not be loaded" present. `/demos`: "unsigned, unchained" present.
- **Specialist passes:** Technical pass used `curl`, DNS over HTTPS, local resolver, and `curl -v` TLS, November 24, 22:48-22:54 EST. Content pass read homepage, launch, investors, investors/statement, kintsugi (714 lines), absolute-constraint (376 lines), directory, directory/dana-ten-ka, legal/mutual-accountability, legal/terms, legal/eula, in full by text fetch, November 24-25. Legal pass read homepage, sitemap, robots.txt, all four legal pages, Investors, Investors/statement, Launch, and Absolute Constraints, plus targeted searches for Cookie Policy, Accessibility, PIPEDA, and Ohio LLC registration. UX pass used browser text fetch, browser search, and direct HTTP fetches of homepage HTML, robots.txt, sitemap.xml, all 51 sitemap URLs, and key assets.
- **Search visibility check:** `site:tenkasystem.com` returned 0 results. Branded search surfaced about 7 pages. MyBzz listing is the only third party result found.
- **What was not done:** No POSTs, no logins, no payments, no signups, no JavaScript execution by the technical pass, no live browser rendering by the UX pass, no Ohio SOS form search, no email deliverability test, no security.txt fetch, no penetration test, no credentialed CORS exploit test. Those limits are stated in the Limits section and in the individual findings, not hidden.

## Appendix B. Sources

All sources are the site itself, its sitemap and robots.txt, its DNS records, its response headers, and its public API responses, plus browser search for independent coverage, press, and registration. No source outside the site was used to establish a finding about the site, except for search visibility (which is by definition external) and the absence of independent coverage (which is a search result, not a claim about an external fact). Where this report states something is missing, it means it was not found in the fetched pages, the sitemap, or search results for the site, not that it cannot exist elsewhere.

Key URLs, all checked November 24-25, 2026:

- https://www.tenkasystem.com/
- https://www.tenkasystem.com/launch
- https://www.tenkasystem.com/pipeline
- https://www.tenkasystem.com/workforce
- https://www.tenkasystem.com/investors
- https://www.tenkasystem.com/investors/statement
- https://www.tenkasystem.com/legal/terms
- https://www.tenkasystem.com/legal/privacy
- https://www.tenkasystem.com/legal/eula
- https://www.tenkasystem.com/legal/mutual-accountability
- https://www.tenkasystem.com/absolute-constraint
- https://www.tenkasystem.com/university
- https://www.tenkasystem.com/research
- https://www.tenkasystem.com/system
- https://www.tenkasystem.com/construction
- https://www.tenkasystem.com/engineering
- https://www.tenkasystem.com/provenance
- https://www.tenkasystem.com/kintsugi
- https://www.tenkasystem.com/directory
- https://www.tenkasystem.com/directory/dana-ten-ka
- https://www.tenkasystem.com/signup
- https://www.tenkasystem.com/demos
- https://www.tenkasystem.com/demos/laksamana
- https://www.tenkasystem.com/demos/eschenwinkel
- https://www.tenkasystem.com/api/health
- https://www.tenkasystem.com/robots.txt
- https://www.tenkasystem.com/sitemap.xml

## Appendix C. One page summary, for forwarding

TenkaSystem.com sells a governed AI workforce that is built, deployable, and running today, with cryptographic identity and signed accountability that holds up to regulatory scrutiny. Its own legal pages say the workforce dialogue is scripted, there is no autonomous AI, and the proprietary models do not exist yet. Its Genesis hash is 8 hex characters with no key, no signature, and no registry. Its chain has no explorer, no entries, and no public key. Its signup page is the homepage, its login, pricing, and contact pages are 404, its one real demo cannot load its scenarios, and its backend health endpoint is public and reports health unknown. Its Privacy Policy points to a Cookie Policy and a sub-processor list that both 404. Its Terms impose binding arbitration that its Mutual Accountability Agreement says does not exist. Its investor page solicits investment with no securities disclaimer. Its 32 employee directory is one 254 line template with fictional people, invented credentials, and placeholder roles. Its search visibility is near zero. Take it offline, rebuild the product, the flows, the backend boundary, and the legal set, and relaunch only when every gate in Section 8 passes with evidence.

*End of report.*

