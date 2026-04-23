# Target Test Log

## 1. Basic Information
- Date: 2026-04-23
- Platform: HackerOne
- Program: Shopify
- Program URL: `https://hackerone.com/shopify`
- Program overview section reviewed: Yes
- Key overview excerpt:
  - `Shopify - Bug Bounty Program | HackerOne`
  - `The Shopify Bug Bounty Program enlists the help of the hacker community at HackerOne to make Shopify more secure.`
- Scope proof:
  - The teacher-approved platform is HackerOne.
  - The direct HackerOne Shopify program page raw HTML was fetched and reviewed.
  - The recorded program page URL is `https://hackerone.com/shopify?type=team`.
- Scope page URL: `https://hackerone.com/shopify/policy_scopes?type=team`
- Official website URL: `https://www.shopify.com/`
- Target / asset tested: Direct HackerOne program page and Shopify official public website
- Tester(s): YX / Codex

## 2. Target Access
- Account required: Likely yes for higher-value authenticated testing, but the public site already exposes many application entry points.
- Account type(s) used: None.
- Registration/login notes:
  - The Shopify homepage exposes public `Log in` and `Start for free` entry points.
  - The `Start for free` call-to-action points to `admin.shopify.com`.
  - The public homepage also exposes clear links to `Customer Accounts`, `Shopify Admin`, `Shopify App Store`, and `shopify.dev`.
- Role setup: Not started.

## 3. Testing Summary
- Main functionality tested:
  - Direct HackerOne Shopify program page
  - Shopify official homepage
  - Shopify account login entry
  - Shopify admin entry
  - Shopify developer documentation
  - Shopify App Store homepage
  - Shopify App Store app-detail page
- Vulnerability classes attempted:
  - Initial public-surface review
  - Initial auth-entry feasibility review
  - Initial direct-object / parameter-flow review
  - Initial misconfiguration / unsafe exposure review
  - Initial Shopify-specific customized review
- Baseline checklist status:
  - 1. IDOR / horizontal access control: Not started.
  - 2. Authentication / authorization flow mistakes: Public auth-entry and redirect-flow review completed.
  - 3. Input handling issues: Initial parameter-flow review completed on App Store surfaces.
  - 4. Misconfiguration / unsafe exposure: Initial public-surface and entry-point review completed.
- Customized testing ideas:
  - Review whether `admin.shopify.com`, customer-account surfaces, or storefront/admin boundaries expose lower-friction public flows later.
  - Review whether app-store, developer, or admin-linked sub-surfaces offer better starting points than the main marketing site.
- Tools used: Direct HackerOne raw HTML fetch, Shopify homepage review, local tracker.
- AI assistance used: Yes.

## 4. Test Steps
1. Continued the Part B workflow using the independent-only approach.
2. Selected `Shopify` as the next HackerOne target candidate after excluding invalid or high-friction candidates.
3. Fetched and reviewed the raw HTML of the direct HackerOne program page at `https://hackerone.com/shopify?type=team`.
4. Extracted and recorded the program title and official description metadata from the direct HackerOne page.
5. Reviewed the public Shopify homepage at `https://www.shopify.com/`.
6. Confirmed that the homepage exposes multiple concrete public entry points, including `Log in`, `Start for free`, `Customer Accounts`, `Shopify Admin`, `Shopify App Store`, and `shopify.dev`.
7. Noted that the homepage appears significantly richer in publicly visible product surfaces than some earlier marketing-heavy targets.
8. Opened the Shopify account login flow and confirmed that public login resolves to `accounts.shopify.com/lookup` with request-specific `rid` and `verify` query parameters.
9. Opened `https://admin.shopify.com/` and confirmed that the public admin entry also redirects into the same Shopify account login flow rather than exposing admin content anonymously.
10. Opened `https://shopify.dev/docs` and confirmed that the developer documentation is publicly accessible and exposes links for `Apps`, `Storefronts`, `Agents`, `Help`, and `Log in`.
11. Verified the `shopify.dev` login entry by opening `http://dev.shopify.com/dashboard` directly and confirmed it redirects into the Shopify accounts login flow rather than exposing a developer dashboard anonymously.
12. Opened `https://apps.shopify.com/?locale=zh-CN` and confirmed that the App Store is publicly accessible, includes a search field, category pages, login/start links, and many app-detail pages.
13. Opened a specific App Store listing page and reviewed its public parameterized URL structure and visible content:
   - `https://apps.shopify.com/affliate-by-secomapp?...`
14. Confirmed that the app-detail page remains publicly viewable as expected, that login/install actions redirect into Shopify-controlled auth flows, and that no obvious open redirect, unauthenticated data leak, or abnormal parameter-driven behavior was observed in this round.
15. Stopped after approximately five meaningful Shopify checks because this round did not produce a confirmed vulnerability and the agreed workflow is to continue with another program after that point.

## 5. Evidence
- Important URLs:
  - `https://hackerone.com/shopify?type=team`
  - `https://hackerone.com/shopify/policy_scopes?type=team`
  - `https://www.shopify.com/`
  - `https://accounts.shopify.com/store-login`
  - `https://admin.shopify.com/`
  - `https://apps.shopify.com/`
  - `https://shopify.dev/`
  - `https://shopify.dev/docs`
  - `http://dev.shopify.com/dashboard`
  - `https://apps.shopify.com/affliate-by-secomapp?...`
- Important requests/responses:
  - HackerOne Shopify program page raw HTML includes:
    - `Shopify - Bug Bounty Program | HackerOne`
    - `The Shopify Bug Bounty Program enlists the help of the hacker community at HackerOne to make Shopify more secure.`
  - Shopify homepage includes:
    - `Log in`
    - `Start for free`
    - `Customer Accounts`
    - `Shopify App Store`
    - `shopify.dev`
    - references to `Shopify Admin`
  - Shopify account-login observations:
    - public login resolves to `accounts.shopify.com/lookup`
    - login flow includes request-specific `rid` and `verify` query parameters
    - login options include email continuation and third-party identity providers
  - Shopify developer docs observations:
    - `shopify.dev/docs` is publicly readable
    - visible `Log in` entry routes into Shopify-controlled login flow
    - no anonymous dashboard content was exposed
  - Shopify App Store observations:
    - homepage and app-detail pages are publicly readable
    - login/install actions route into Shopify-controlled auth flows
    - parameterized listing URLs did not show obvious unsafe behavior in this round
- Screenshots / recordings: None yet.
- Notes:
  - The direct HackerOne program page was readable through raw HTML and was reviewed first.
  - The official homepage exposed a broader public product surface than Privy or Quora.
  - This makes Shopify more promising as a future independent-only candidate than some previously reviewed programs, even though no vulnerability was confirmed in this first round.
  - The developer docs and App Store are both useful public surfaces, but this pass still did not reveal a concrete auth flaw, data leak, or direct-object exposure.

## 6. Outcome
- Result: No finding
- What worked:
  - Confirmed the direct HackerOne Shopify program page and official program description.
  - Confirmed that Shopify's public homepage exposes several concrete product and admin-adjacent entry points.
  - Identified Shopify as a richer public-surface candidate than more marketing-only or immediately challenge-gated targets.
  - Confirmed that both public login and public admin entry points redirect into Shopify-controlled account-auth flows rather than exposing protected content.
  - Confirmed that `shopify.dev/docs` is a rich public developer surface and that its visible login entry also routes into controlled auth.
  - Confirmed that App Store listing pages are publicly readable but that install/login actions remain behind Shopify auth flows.
- What failed:
  - Did not begin authenticated testing.
  - Did not find an obvious open redirect, unauthenticated object exposure, or parameter-handling issue in the App Store surfaces reviewed.
- Why no finding was confirmed yet:
  - This round focused on public login/admin/dev/app-store entry points only.
  - The more interesting Shopify bug classes likely require deeper follow-up inside authenticated merchant, developer, or partner contexts.

## 7. Candidate Finding Details
- Vulnerability title: None yet.
- Vulnerability class: None yet.
- Root cause: None yet.
- Reproduction summary: None yet.
- Security impact: None yet.
- Affected assets/data: None yet.
- Severity estimate: None yet.
- Platform severity mapping: None yet.
- CVSS fallback (if needed): None yet.

## 8. Novelty Check
- Similar public reports searched: Not started.
- Public references checked:
  - Direct HackerOne Shopify program page
  - Shopify official homepage
- Initial novelty judgment: Not applicable yet.
- Finding type: Unknown

## 9. Scope and Safety Notes
- Why this target is in scope:
  - Shopify is being assessed through the teacher-approved HackerOne platform.
  - The direct HackerOne Shopify program page was fetched and recorded before reviewing the target site.
- Safety constraints followed:
  - Only the public HackerOne program page and the public Shopify homepage were reviewed in this round.
  - No authenticated actions, brute force, or disruptive requests were used.
- Actions avoided:
  - No attempts against non-confirmed Shopify assets outside the clearly linked public surfaces.
  - No form submissions or account-creation attempts.

## 10. Next Actions
- Follow-up validation needed:
  - Check whether merchant, partner, or developer authenticated contexts can be entered later without excessive friction.
  - Compare Shopify against Semrush as a candidate for the strongest low-friction follow-up target.
- Questions for teammates:
  - Should Shopify be promoted into the higher-priority shortlist for deeper public-surface testing?
- Whether to keep testing this target: Later, but this public-surface 5-check round did not produce a finding.

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
