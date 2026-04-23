# Part B Master Tracker

Use this file to avoid duplicate testing and to summarize progress across all allowed targets.

| Date | Platform | Program | Program bounty URL | Program URL | Tester(s) | Tested areas / methods | Status | Candidate Finding | Folder |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-04-22 | TBD | TBD | TBD | TBD | TBD | TBD | Not started | None | `partb/targets/` |
| 2026-04-22 | HackerOne | Basecamp | `https://hackerone.com/basecamp?type=team` | `https://basecamp.com/` | YX / Codex | Scope review, authenticated object/ID testing, cross-bucket object-binding checks on project/message/document/upload/schedule URLs, invite-flow form inspection, preview/storage direct-link checks in isolated context | No finding | None | `partb/targets/2026-04-22-hackerone-basecamp/` |
| 2026-04-23 | HackerOne | Semrush | `https://hackerone.com/semrush?type=team` | `https://www.semrush.com/` | YX / Codex | Program review, pre-login rendered testing, authenticated surface review, reports/apps boundary checks | No finding | None | `partb/targets/2026-04-23-hackerone-semrush/` |
| 2026-04-23 | HackerOne | GitLab | `https://hackerone.com/gitlab?type=team` | `https://gitlab.com/` | YX / Codex | Program-page review, signup flow analysis, login fetch behavior review | In progress | None | `partb/targets/2026-04-23-hackerone-gitlab/` |
| 2026-04-23 | HackerOne | Privy (Bounty) | `https://hackerone.com/privy-bbp?type=team` | `https://www.privy.io/` | YX / Codex | Program-page review, public-site review, low-friction auth-path screening | No finding | None | `partb/targets/2026-04-23-hackerone-privy/` |
| 2026-04-23 | HackerOne | Quora | `https://hackerone.com/quora?type=team` | `https://www.quora.com/` | YX / Codex | Program-page review, homepage fetch behavior review, low-friction screening | No finding | None | `partb/targets/2026-04-23-hackerone-quora/` |
| 2026-04-23 | HackerOne | Shopify | `https://hackerone.com/shopify?type=team` | `https://www.shopify.com/` | YX / Codex | Program intake, pre-auth DevTools review, authenticated partner/dashboard/stores/team/settings testing | No finding | None | `partb/targets/2026-04-23-hackerone-shopify/` |
| 2026-04-23 | HackerOne | Poe | `https://hackerone.com/poe?type=team` | `https://poe.com/` | YX / Codex | Program URL validation, public login-surface review | Out of scope | None | `partb/targets/2026-04-23-hackerone-poe/` |
| 2026-04-23 | HackerOne | Airtable | `https://hackerone.com/airtable?type=team` | `https://www.airtable.com/` | YX / Codex | Pre-auth UI + DevTools, authenticated account/builder/API, OIDC/session review, token/OAuth form review, workspace/share/collaborator/invite review; no leak or bypass confirmed, current blocker is lack of second account and richer shared objects | No finding | None | `partb/targets/2026-04-23-hackerone-airtable/` |
| 2026-04-23 | HackerOne | Asana | `https://hackerone.com/asana?type=team` | `https://asana.com/` | YX / Codex | Program review, source/DOM/inline-state inspection, task/project/profile/member/share object ID extraction, task pane/share modal/manage-access modal review, low-frequency wrong-ID navigation tests on project/task/profile paths | No finding | None | `partb/targets/2026-04-23-hackerone-asana/` |
| 2026-04-24 | HackerOne | WordPress | `https://hackerone.com/wordpress?type=team` | `https://wordpress.org/` | YX / Codex | Program review, logged-in profile/account-security source + network inspection, `/wp-json/wp/v2/users/{id}?context=edit` read/write ID tests with real `X-WP-Nonce`, GitHub OAuth link/state/redirect parameter review | No finding | None | `partb/targets/2026-04-24-hackerone-wordpress/` |
| 2026-04-24 | HackerOne | Dropbox | `https://hackerone.com/dropbox?type=team` | `https://www.dropbox.com/` | YX / Codex | Program review, logged-in home/requests source + network inspection, `/2/files/browse` object extraction, file-request creation and public request token tests, one-character token mutation check, request-title HTML/payload validation against server-side character restrictions | No finding | None | `partb/targets/2026-04-24-hackerone-dropbox/` |

Status suggestions:
- `Not started`
- `In progress`
- `No finding`
- `Candidate finding`
- `Needs validation`
- `Out of scope`
- `Reported`
