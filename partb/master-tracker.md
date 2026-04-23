# Part B Master Tracker

Use this file to avoid duplicate testing and to summarize progress across all allowed targets.

| Date | Platform | Program | Target / Asset | Tester(s) | Test Types | Status | Candidate Finding | Folder |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-04-22 | TBD | TBD | TBD | TBD | TBD | Not started | None | `partb/targets/` |
| 2026-04-22 | HackerOne | Basecamp | `basecamp.com` web app and in-scope assets from the program security page | YX / Codex | Scope review, public-surface review, signup/login flow analysis, authenticated owner/admin testing, anonymous direct-object checks | No finding | None | `partb/targets/2026-04-22-hackerone-basecamp/` |
| 2026-04-23 | HackerOne | Semrush | `www.semrush.com` public and authenticated surfaces including SEO, Home, Reports, and App Center | YX / Codex | Program review, pre-login rendered testing, authenticated surface review, reports/apps boundary checks | No finding | None | `partb/targets/2026-04-23-hackerone-semrush/` |
| 2026-04-23 | HackerOne | GitLab | `gitlab.com` public auth entry points and direct program page | YX / Codex | Program-page review, signup flow analysis, login fetch behavior review | In progress | None | `partb/targets/2026-04-23-hackerone-gitlab/` |
| 2026-04-23 | HackerOne | Privy (Bounty) | `www.privy.io` public site and direct program page | YX / Codex | Program-page review, public-site review, low-friction auth-path screening | No finding | None | `partb/targets/2026-04-23-hackerone-privy/` |
| 2026-04-23 | HackerOne | Quora | `www.quora.com` public homepage and direct program page | YX / Codex | Program-page review, homepage fetch behavior review, low-friction screening | No finding | None | `partb/targets/2026-04-23-hackerone-quora/` |
| 2026-04-23 | HackerOne | Shopify | `www.shopify.com`, `apps.shopify.com`, `shopify.dev`, Shopify account-login, and authenticated Partners surfaces | YX / Codex | Program intake, pre-auth DevTools review, authenticated partner/dashboard/stores/team/settings testing | No finding | None | `partb/targets/2026-04-23-hackerone-shopify/` |
| 2026-04-23 | HackerOne | Poe | Candidate HackerOne Poe URL and `poe.com` login surface | YX / Codex | Program URL validation, public login-surface review | Out of scope | None | `partb/targets/2026-04-23-hackerone-poe/` |
| 2026-04-23 | HackerOne | Airtable | `www.airtable.com` public site plus Airtable login/signup and developer/API surfaces | YX / Codex | Program intake, pre-auth UI review, pre-auth DevTools review | In progress | None | `partb/targets/2026-04-23-hackerone-airtable/` |

Status suggestions:
- `Not started`
- `In progress`
- `No finding`
- `Candidate finding`
- `Needs validation`
- `Out of scope`
- `Reported`
